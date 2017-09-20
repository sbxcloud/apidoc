---
title: sbxcloud API

language_tabs:
  - shell: cURL
  - javascript

<!-- toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a> -->

<!-- includes:
  - errors -->

search: true
---

# Introducción

> **Librerias de la API** <br>
> Las librerias oficial de la API se encuentran en la cuenta de [github](https://github.com/sbxcloud).

Bienvenido a la API de sbxcloud! Puedes usar nuestra API para acceder a los recursos alojados en nuestra infraestructura con el fin de construir aplicativos moviles y web con una infraestructura solida.

Encontraras ejemplos de casos de uso en Curl y Javascript. Puedes ver el codigo de ejemplo en el area oscura a la derecha, y puedes cambiar el lenguaje de programación de los ejemplos haciendo uso de los tabs en la parte superior derecha.

La API de sbxcloud esta basada en REST. Nuestra API tiene URLs previsibles y orientadas a recursos, y usa códigos de respuesta HTTP para indicar errores de la API. Las respuestas de la API se encuentran en JSON, incluyendo los errores.

<aside class="notice">
<strong>API Endpoint</strong> <code><strong>https://sbxcloud.com/api</strong></code>
</aside>

#Configuración Básica

```shell
Petición de ejemplo

curl -X POST \
  https://sbxcloud.com/api/data/v1/row/find \
  -H 'authorization: Bearer USER_TOKEN' \
  -H 'content-type: application/json' \
  -d '{
        "domain": DOMAIN_VALUE ,
        "row_model": "MODEL_NAME"
      }'
```

```javascript
//Petición de ejemplo

var data = JSON.stringify({
  "domain": DOMAIN_VALUE,
  "row_model": "MODEL_NAME"
});

var xhr = new XMLHttpRequest();

xhr.addEventListener("readystatechange", function () {
  if (this.readyState === 4) {
    console.log(this.responseText);
  }
});

xhr.open("POST", "https://sbxcloud.com/api/data/v1/row/find");
xhr.setRequestHeader("authorization", "Bearer USER_TOKEN");
xhr.setRequestHeader("content-type", "application/json");

xhr.send(data);
```

Para hacer uso de la platadorma [sbxcloud](https://sbxcloud.com/#!/) debemos tener un dominio previamente creado. Por cada dominio que tengamos creado en la plataforma se generara una llave que debe ser usada en la petición de autenticación a un dominio.

El **`USER_TOKEN`** se obtiene en la respuesta de la petición de autenticación a un dominio.

Para cada petición que se realiza se debe agregar, en el encabezado de la petición, la llave `Authorization`. Es un valor [alfanumérico](https://es.wikipedia.org/wiki/Alfanum%C3%A9rico).

<code> **"Authorization" : "Bearer USER_TOKEN"**</code>

El <code>**App-Key**</code> se utiliza para realizar la autenticación de un dominio, se obtiene de la siguiente manera. Al ingresar al dashboard seleccionamos el dominio con el cual trabajeremos. Luego, en la navegacion lateral seleccionamos la opcion de Apps Registradas.

![Apps Registradas](../images/image01.png)

Seleccionamos la opción Keys y se desplegara un popup en donde podremos ver el <code>App-Key</code>.

![App Key](../images/image02.png)

Se debe enviar el dominio de la aplicación en cada petición <code>GET</code> o <code>POST</code>. Este lo obtenemos de la url, al estar ubicado en la vista de un dominio.

![Domain number](../images/image03.png)

<code> **"domain" : "DOMAIN_VALUE"**</code>

<aside class="notice">
Los verbos <code>HTTP</code> usados en la plataforma son <code>GET</code> y <code>POST</code>.
El verbo <code>GET</code> se utiliza para realizar la autenticación y el verbo <code>POST</code>  
se usa para realizar peticiones a la API, como consultas a un modelo en especifico.
</aside>


# Autenticación

La autenticación a la API es realizada via Bearer auth. Para obtener un token de autorización se debe realizar login a la plataforma con las credenciales obtenidas al momento de hacer el registro en [sbxcloud](https://sbxcloud.com/#!/). La forma para obtener el token de autenticación, por dominio, es a traves del verbo <code>GET</code>.

##Login

```shell
Petición de autenticación

curl -X GET \
  'https://sbxcloud.com/api/user/v1/login?email=EMAIL_SBXCLOUD&password=PASSWORD_SBXCLOUD&domain=DOMAIN_VALUE,' \
  -H 'app-key: APP_KEY' \
  -H 'content-type: application/json'

```

```javascript
//Petición de autenticación
var xhr = new XMLHttpRequest();

xhr.addEventListener("readystatechange", function () {
  if (this.readyState === 4) {
    console.log(this.responseText);
  }
});

xhr.open("GET", "https://sbxcloud.com/api/user/v1/login?email=EMAIL_SBXCLOUD&password=PASSWORD_SBXCLOUD&domain=DOMAIN_VALUE");
xhr.setRequestHeader("app-key", "APP_KEY");
xhr.setRequestHeader("content-type", "application/json");

xhr.send();
```

> Esta petición retorna un JSON con la siguiente estructura, de ser exitoso la autenticación:

```json
  {
  "success": true,
  "token": "TOKEN",
  "user": {
    "id": USER_ID,
    "name": "NOMBRE",
    "code": null,
    "email": "jhon_doe@sbxcloud.com",
    "login": "jdoe",
    "role": "ROLE",
    "domain": "root",
    "display_name": "Carpeta Personal",
    "domain_id": 0,
    "membership_role": "USER",
    "member_of": [
      {
        "domain_id": DOMAIN_ID,
        "domain": "DOMAIN_NAME",
        "display_name": "DISPLAY_NAME",
        "role": "ROLE",
        "home_key": "HOME_KEY"
      }
    ],
    "following": [],
    "home_folder_key": "HOME_FOLDER_KEY"
    }
  }
```
> Si las credenciales enviadas no son correctas, se retorna el siguiente JSON.

```json
  {
    "success": false,
    "error": "Invalid Credentials"
  }
```

Esta ruta te permite realizar login a un dominio especifico.

###Petición HTTP

`GET https://sbxcloud.com/api/user/v1/login`

### Parametros de consulta

Parametro | Default | Descripción
--------- | ------- | -----------
email | na | Correo o alias con el cual se encuentra registrado.
password | na | Contraseña con el cual se encuentra registrado.
domain | na | Número del dominio de la app creada.

##Cambio de clave

```shell
Petición de campo de clave

curl -X GET \
  'https://sbxcloud.com/api/user/v1/password?domain=ID_DOMINIO_SBXCLOUD&password=NUEVA_CLAVE,' \
  -H 'app-key: APP_KEY' \
  -H 'authorization: Bearer USER_TOKEN' \
  -H 'content-type: application/json'

```

```javascript
//Petición de autenticación
var xhr = new XMLHttpRequest();

xhr.addEventListener("readystatechange", function () {
  if (this.readyState === 4) {
    console.log(this.responseText);
  }
});

xhr.open("GET", "https://sbxcloud.com/api/user/v1/password?domain=ID_DOMINIO_SBXCLOUD&password=NUEVA_CLAVE");
xhr.setRequestHeader("app-key", "APP_KEY");
xhr.setRequestHeader("app-key", "Bearer USER_TOKEN");
xhr.setRequestHeader("content-type", "application/json");

xhr.send();
```

> Esta petición retorna un JSON con la siguiente estructura, de ser exitoso la autenticación:

```json
  {"success":true}
```
> Si las credenciales(TOKEN por ejemplo) enviadas no son correctas, se retorna el siguiente JSON.

```json
  {"success":false,"error":"Invalid Token"}
```

Esta ruta te permite realizar el cambio de clave del usuario actualmente autenticado.

###Petición HTTP

`GET https://sbxcloud.com/api/user/v1/login`

### Parametros de consulta

Parametro | Default | Descripción
--------- | ------- | -----------
password | na | Nueva contraseña deseada para el usuario.
domain | na | Número del dominio de la app creada.


#Crear

```shell

Petición de creación

curl -X POST \
  https://sbxcloud.com/api/data/v1/row \
  -H 'authorization: Bearer USER_TOKEN' \
  -H 'content-type: application/json' \
  -d '{
  "domain": DOMAIN_VALUE,
  "row_model":"MODAL_NAME",
  "rows":[
    {
      "MODEL_FIELD_A":"VALUE_A",
      "MODEL_FIELD_B":"VALUE_B"
    },{
      "MODEL_FIELD_A":"VALUE_A",
      "MODEL_FIELD_B":"VALUE_B"
    }
      ]
}'
```

```javascript

//Petición de creación
var data = JSON.stringify({
  "domain": DOMAIN_VALUE,
  "row_model": "MODEL_NAME",
  "rows": [
    {
      "MODEL_FIELD_A":"VALUE_A",
      "MODEL_FIELD_B":"VALUE_B"
    },
    {
      "MODEL_FIELD_A":"VALUE_A",
      "MODEL_FIELD_B":"VALUE_B"
    }
  ]
});

var xhr = new XMLHttpRequest();

xhr.addEventListener("readystatechange", function () {
  if (this.readyState === 4) {
    console.log(this.responseText);
  }
});

xhr.open("POST", "https://sbxcloud.com/api/data/v1/row");
xhr.setRequestHeader("authorization", "Bearer USER_TOKEN");
xhr.setRequestHeader("content-type", "application/json");

xhr.send(data);
```

> La petición anterior retorna un JSON con la siguiente estructura:

```json
{
  "success": true,
  "keys": [
    "KEY_REGISTRY",
    ...,
    "KEY_REGISTRY_N"
  ]
}
```

La creación de registros en un modelo de la plataforma [sbxcloud](https://sbxcloud.com/#!/) se realiza de forma sencilla. Debemos crear un objeto JSON con todos los atributos que este modelo tiene y cada uno con su valor. Se pueden crear n cantidad de objetos al tiempo, luego de esto creamos un Array con todos los objetos a crear.

<aside class="notice">
La cantidad maxima de registros que se pueden crear por petición es de 1000.
</aside>


###Petición HTTP

`POST https://sbxcloud.com/api/data/v1/row`

### Parametros de creación

Parametros | Default | Descripción
--------- | ------- | -----------
domain | na | Número del dominio de la app.
row_model | na | Nombre del modelo creado en el dominio.
rows | na | Array de objetos, cada objeto debe contener los campos del modelo con su respectivo valor.



# Buscar

```shell

Petición de busqueda

curl -X POST \
  https://sbxcloud.com/api/data/v1/row/find \
  -H 'authorization: Bearer USER_TOKEN' \
  -H 'content-type: application/json' \
  -d '{
        "domain": DOMAIN_VALUE,
        "row_model": "MODEL_NAME",
        "where": {
          "keys": ["KEY",...,"KEY_N"]
        }
      }'
```

```javascript

//Petición de busqueda

var data = JSON.stringify({
  "domain": DOMAIN_NUMBER,
  "row_model": "MODEL_NAME",
  "where": {
    "keys": [
      "KEY",
      ...,
      "KEY_N"
    ]
  }
});

var xhr = new XMLHttpRequest();

xhr.addEventListener("readystatechange", function () {
  if (this.readyState === 4) {
    console.log(this.responseText);
  }
});

xhr.open("POST", "https://sbxcloud.com/api/data/v1/row/find");
xhr.setRequestHeader("authorization", "Bearer USER_TOKEN");
xhr.setRequestHeader("content-type", "application/json");

xhr.send(data);
```

Esta ruta retorna todos los registros de un modelo basado en condiciones si estas se encuentran presente. Los unicos parametros necesarios son `domain` y `row_model` los demas son opcionales.

Se debe agregar el atributo `where`, su valor es un objeto, con el atributo `keys` el cual contiene un array de llaves de los registros a solicitar. Si este atributo es omitido la plataforma retorna todos los registros con el valor de `size` por defecto que es 250.


### Petición HTTP

`POST https://sbxcloud.com/api/data/v1/row/find`

### Parametros de consulta

Parametros | Default | Descripción
--------- | ------- | -----------
domain | na | Número del dominio de la app.
row_model | na | Nombre del modelo creado en el dominio.
page | 1 | Pagina a solicitar.
size | 250 | Cantidad de registros a solicitar por pagina.
fetch | na | Array que contiene los nombres de las referencias que se encuentran en la tabla.
where | na | Estructura JSON con atributos para consultas avanzadas.

## Fetch

El atributo `fetch` tiene la función de traer las referencias que se encuentran en un modelo. `fetch` recibe un array de String con el nombre del campo de el modelo a solicitar la información, mas no el nombre de el modelo de la referencia, en el resultado de la petición habra un atributo con el nombre de `fetched_results`.

`fetched_results` es un objeto en el que cada atributo es el nombre del modelo referenciado,luego entonces, cada atributo de el modelo referenciado es la llave de el registro y su valor es el registro completo de ese modelo.

![Fetched_results](../images/image04.png)

Tengamos en cuenta el siguiente caso.

```shell
Petición con fetch

curl -X POST \
  https://sbxcloud.com/api/data/v1/row/find \
  -H 'authorization: Bearer USER_TOKEN' \
  -H 'content-type: application/json' \
  -d '{
      "domain": DOMAIN_VALUE,
      "row_model":"client",
      "fetch":["type", "type.zone"]
      }'

```

```javascript

//Petición con fetch

var data = JSON.stringify({
  "domain": DOMAIN_VALUE,
  "row_model": "client",
  "fetch": [
    "type",
    "type.zone"
  ]
});

var xhr = new XMLHttpRequest();

xhr.addEventListener("readystatechange", function () {
  if (this.readyState === 4) {
    console.log(this.responseText);
  }
});

xhr.open("POST", "https://sbxcloud.com/api/data/v1/row/find");
xhr.setRequestHeader("authorization", "Bearer USER_TOKEN");
xhr.setRequestHeader("content-type", "application/json");

xhr.send(data);
```

> La petición anterior retorna un JSON con la siguiente estructura:

```json

{
  "success": true,
  "model": [
    {
      "id": NUMBER,
      "type": "TIPO",
      "name": "NAME",
      "reference_type": null,
      "reference_type_name": null,
      "is_label": BOOLEAN,
      "is_array": BOOLEAN,
      "is_required": BOOLEAN,
      "is_sequence": BOOLEAN,
      "reference_model":{...}
    },
  ],
  "row_count": NUMBER,
  "total_pages": NUMBER,
  "results": [
    {
      .
      .
      .
      ,
      "_KEY": "KEY_REGISTER",
      "_META": {
        "created_time": "DATE",
        "updated_time": "DATE"
      }
    },
  ],
  "fetched_results": {
    "client_type": {
      "KEY_REGISTER": {
        .
        .
        .
        ,
        "_META": {
          "created_time": "DATE",
          "updated_time": "DATE"
        }
      }
    },
    "zone_type": {
      "KEY_REGISTER": {
        .
        .
        .
        ,
        "_META": {
          "created_time": "DATE",
          "updated_time": "DATE"
        }
      }
    }
  }
}

```

Tenemos los siguiente modelos `client`, `client_type` y `client_zone` que tienen la siguiente estructura.

* `client`
  * name - string.
  * age  - string.
  * type - reference (client_type).

* `client_type`
  * code - int.
  * name - string.
  * zone - reference (zone_type).

* `zone_type`
  * name  - string.
  * city  - string.
  * hours - int.

En la zona derecha podremos ver una petición haciendo uso de `fetch`, en este ejemplo realizamos un `fetch` de primer y segundo nivel, podemos ver que en el array de `fetch` referenciamos el modelo por medio de el nombre de el campo en el modelo `client`, muy importante tener esto claro. En el resultado, si retorna el nombre de el modelo, en este caso `client_type` y `client_zone`.

<aside class="warning">
Es posible hacer <code>fetch</code> de referencias a un modelo con un nivel de profundidad máximo de 2. Es decir, no es posible hacer <code>fetch</code> con una sintaxis como la siguiente
<p style="text-align: center; margin: 0;"><code><strong>fetch: ["referencia.referencia2.referencia3" ]</strong></code></p>
esto generaría un error.
</aside>

## Busquedas avanzadas

```shell

Petición basada en condiciones

curl -X POST \
  https://sbxcloud.com/api/data/v1/row/find \
  -H 'authorization: Bearer USER_TOKEN' \
  -H 'content-type: application/json' \
  -d '{
      "domain": DOMAIN_VALUE,
      "row_model":"MODEL_NAME",
      "page": NUMBER,
      "size": NUMBER,
      "where": [
          {
            "ANDOR": "AND",
            "GROUP": [
                {
                  "OP": "=",
                  "ANDOR": "AND",
                  "VAL": "true",
                  "FIELD": "FIELD_MODEL"
                }
              ]
          }
        ]
      }'

```

```javascript

//Petición basada en condiciones

var data = JSON.stringify({
  "domain": DOMAIN_VALUE,
  "row_model": "MODEL_NAME",
  "page": NUMBER,
  "size": NUMBER,
  "where": [
    {
      "ANDOR": "AND",
      "GROUP": [
        {
          "OP": "=",
          "ANDOR": "AND",
          "VAL": "true",
          "FIELD": "FIELD_MODEL"
        }
      ]
    }
  ]
});

var xhr = new XMLHttpRequest();

xhr.addEventListener("readystatechange", function () {
  if (this.readyState === 4) {
    console.log(this.responseText);
  }
});

xhr.open("POST", "https://sbxcloud.com/api/data/v1/row/find");
xhr.setRequestHeader("authorization", "Bearer USER_TOKEN");
xhr.setRequestHeader("content-type", "application/json");

xhr.send(data);
```

> La petición anterior retorna un JSON con la siguiente estructura:

```json

{
  "success": true,
  "model": [
    {
      "id": NUMBER,
      "type": "TIPO",
      "name": "NAME",
      "reference_type": null,
      "reference_type_name": null,
      "is_label": BOOLEAN,
      "is_array": BOOLEAN,
      "is_required": BOOLEAN,
      "is_sequence": BOOLEAN,
      "reference_model":{...}
    },
  ],
  "row_count": NUMBER,
  "total_pages": NUMBER,
  "results": [
    {
      .
      .
      .
      ,
      "_KEY": "KEY_REGISTER",
      "_META": {
        "created_time": "DATE",
        "updated_time": "DATE"
      }
    }
  ]
}

```

Para realizar busquedas avanzadas debemos agregar un array de objetos con ciertas condiciones en el atributo `where`, los objetos deben tener el atributo `ANDOR` y en el primer objeto del Array el valor debe ser `AND`. Este Array debe tener la estructura que se encuentra en la zona derecha.

<aside class="notice">
Recordar que el primer objeto del array el valor de <code>ANDOR</code> debe ser <code>AND</code>.
</aside>

### Petición HTTP

`POST https://sbxcloud.com/api/data/v1/row/find`

### Atributos de consulta

Atributo | Descripción
--------- | -----------
ANDOR | Conector lógico, valores `AND` y `OR`.
GROUP | Array que contiene un objeto con los siguientes atributos `OP`, `ANDOR`, `VAL`, `FIELD`.

### Atributos de GROUP

Atributo | Descripción
--------- | -----------
OP | Los valores permitidos son `in`, `not in`, `is`, `is not`, `!=`, `=`, `<`, `<=`, `>=`, `>`, `LIKE`.
ANDOR | Conector lógico, valores `AND` y `OR`.
VAL | Valor por el cual se va realizar la consulta, e.g. true, 1, "Colombia".
FIELD | Campo del modelo.

# Actualizar

```shell

Petición de actualización

curl -X POST \
  https://sbxcloud.com/api/data/v1/row/update \
  -H 'authorization: Bearer USER_TOKEN' \
  -H 'content-type: application/json' \
  -d '{
  "domain": DOMAIN_VALUE,
  "row_model":"MODEL_NAME",
  "rows":[
            {
              "_KEY": "KEY_REGISTRY",
              "FIELD_MODEL": "VALUE"
            },
            .
            .
            .
            ,
            {
              "_KEY": "KEY_REGISTRY",
              "FIELD_MODEL": "VALUE"
            }
        ]
}'
```
```javascript

// Petición de actualización

var data = JSON.stringify({
  "domain": DOMAIN_NUMBER,
  "row_model": "MODEL_NAME",
  "rows": [
    {
      "_KEY": "KEY_REGISTRY",
      "FIELD_MODEL": "VALUE"
    },
    {
      "_KEY": "KEY_REGISTRY",
      "FIELD_MODEL": "VALUE"
    }
  ]
});

var xhr = new XMLHttpRequest();

xhr.addEventListener("readystatechange", function () {
  if (this.readyState === 4) {
    console.log(this.responseText);
  }
});

xhr.open("POST", "https://sbxcloud.com/api/data/v1/row/update");
xhr.setRequestHeader("authorization", "Bearer USER_TOKEN");
xhr.setRequestHeader("content-type", "application/json");

xhr.send(data);
```

> Se retorna el siguiente JSON:

```json
  {
    "success": true
  }
```

Esta ruta nos permite actualizar registros de un modelo. El atributo `rows` recibe un array de objetos, cada uno debe tener la llave `_KEY`, el cual es la llave que identifica a un registro unico, y los campos a actualizar con su valor.

###Petición HTTP

`POST https://sbxcloud.com/api/data/v1/row/update`

### Parametros de consulta

Parametros | Default | Descripción
--------- | ------- | -----------
domain | na | Número del dominio de la app
row_model | na | Nombre del modelo creado en el dominio.
rows | na | Array de objetos, cada objeto debe contener el atributo `_KEY` el cual es la key del registro a actualizar. Ademas los campos a actualizar con un valor valido.

<aside class="notice">
Tener en cuenta que para realizar una actualización de uno o varios registros de un modelo no es necesario enviar todos los atributos del modelo solo deben enviarse los campos del modelo a actualizar, esto con el fin de tener un mejor rendimiento.
</aside>

# Eliminar

```shell

Petición de eliminación

curl -X POST \
  https://sbxcloud.com/api/data/v1/row/delete \
  -H 'authorization: Bearer USER_TOKEN' \
  -H 'content-type: application/json' \
  -d '{
        "domain": DOMAIN_VALUE,
        "row_model": "MODEL_NAME",
        "where": {
          "keys": ["KEY",...,"KEY_N"]
        }
      }'
```

```javascript

//Petición de eliminación

var data = JSON.stringify({
  "domain": DOMAIN_NUMBER,
  "row_model": "MODEL_NAME",
  "where": {
    "keys": [
      "KEY_REGISTRY"
    ]
  }
});

var xhr = new XMLHttpRequest();

xhr.addEventListener("readystatechange", function () {
  if (this.readyState === 4) {
    console.log(this.responseText);
  }
});

xhr.open("POST", "https://sbxcloud.com/api/data/v1/row/delete");
xhr.setRequestHeader("authorization", "Bearer USER_TOKEN");
xhr.setRequestHeader("content-type", "application/json");

xhr.send(data);
```

> Se retorna el siguiente JSON:

```json
{
  "success": true
}
```

Esta ruta nos permite eliminar registros de un modelo. Debe enviarse en el objeto `where` el atributo `keys` que contiene un array de llaves, que son las llaves de los registros a eliminar.

###Petición HTTP

`POST https://sbxcloud.com/api/data/v1/row/delete`

### Parametros de consulta

Parametros | Default | Descripción
--------- | ------- | -----------
domain | na | Número del dominio de la app
row_model | na | Nombre del modelo creado en el dominio.
where | na | Objeto que tiene el atributo `keys` el cual recibe un array de llaves, que son los registros a eliminar.
