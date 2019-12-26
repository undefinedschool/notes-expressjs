> El siguiente contenido fue elaborado por [@_nhsz](https://twitter.com/_nhsz) como guía para las clases de [undefined school](https://twitter.com/undefinedSchool)

> Son bienvenidos los _issues_ y _PRs_ para mejorar el contenido, corregir errores, etc

# [WIP] Notas sobre ExpressjsJS

## Contenido

- [Documentación](#documentación)
- [Instalación](#instalación)
- [¿Qué es ExpressJS?](#qué-es-expressjs)
- [Ventajas sobre usar utilizar Node (`http`)](#ventajas-sobre-usar-utilizar-node-http)
- [Usos](#usos)
- [Routing](#routing)
  - [Router](#router)
  - [Rutas](#rutas)
  - [URL Parameters](#url-parameters)
- [Middleware](#middleware)
  - [Tipos de middleware](#tipos-de-middleware)
- [Práctica](#práctica)
  - [Setup](#setup)
  - [Ejercicios](#ejercicios)

---

## Documentación

- [Tutorial oficial](https://expressjs.com/en/starter/installing.html)
- [Guía oficial y conceptos](https://expressjs.com/en/guide/routing.html)
- [Express API](https://expressjs.com/en/5x/api.html)

## Instalación   

_Express_ es paquete de Node, por lo que se instala con `npm` como cualquier otra dependencia de nuestro proyecto

```js
npm install express
```

## ¿Qué es ExpressJS?

Es un _framework_ para desarrollar aplicaciones web (_server-side_) con _Node_, _minimalista_ y _no opinionado_. Esto último significa que no asume que vayamos a construir nuestras aplicaciones de alguna forma en particular, cómo manejar los _requests_ o generar los _responses_, etc, en ese sentido, mantenemos el control.

## Ventajas sobre usar utilizar Node (`http`)

- Express está **construido sobre el módulo `http` de Node**, por lo que básicamente funciona como una _API_ que **simplifica y aumenta la funcionalidad que este módulo nos provee**.
- Nos permite construir aplicaciones web con Node de una forma mucho más **simple**
- Es **muy liviano y rápido**
- **Minimalista:** hace muy pocas cosas, se enfoca más bien en reducir el _boilerplate_ y las partes más repetitivas y tediosas del código en Node (setear _headers_, MIME types, _cerrar conexiones_, servir archivos estáticos, simplifica mucho el _routing_, etc)
- **No opinionado:** tenemos control total sobre cómo diseñar la aplicación, qué librerías y _middlewares_ utilizar, cómo manejar los _requests_, _responses_, etc
- Reduce la cantidad de código necesaria y nos abstrae de mucha de la complejidad que tenemos al utilizar el módulo `HTTP`
- Tiene una _API_ muy simple y _expresiva_
- **Middleware:** funciones que nos van a permitir interceptar y manipular _requests_ antes de hacer cualquier otra cosa
- Express **es el framework más popular** para desarrollar aplicaciones web con Node

## Usos

- _servers_
- _APIs_
- _Microservicios_

Generalmente, vamos a recibir _requests_ con nuestra aplicación Express (_back-end_), generar la respuesta en formato JSON y enviársela al cliente (_front-end_) para que la consuma y haga con esta lo que quiera, genere las _vistas_ correspondientes, etc. También podemos generar vistas desde el servidor, utilizando HTML plano como hicimos anteriormente en Node o algún _template engine_ como [Handlebars](https://handlebarsjs.com/) o [Pug](https://pugjs.org/). 

## Routing

Dependiendo de en qué URL se realizar el _request_, debemos responder de una forma distinta, es decir, los _endpoints_ de nuestra aplicación deben reaccionar de diferentes maneras a los _requests_ del cliente.

Hacer que diferentes _requests HTTP_ apunten a diferentes partes del código de nuestra aplicación es lo que se conoce como **routing**. 

- Ver [Basic routing](https://expressjs.com/en/starter/basic-routing.html)

### Router

Para una aplicación muy simple, alcanza con agregar las rutas en condicionales dentro del _callback_ que recibe `createServer` en Node y modularizar esta función, pero a medida que la complejidad de la aplicación crece, se vuelve más engorroso. Es por esto que la lógica de _routing_ de las aplicaciones web suele separarse en otro módulo.

A esta parte del código encargada del _routing_, se la conoce como **router**.

Express incluye el objeto [`router`](http://expressjs.com/en/4x/api.html#router).

**Nota:** el uso del _router_ que nos provee Express es **opcional**, podemos seguir manejando el routing con Node o usar otro _router engine_ (buscar en NPM). El router de Express es bastante minimal y **nos permite separar la configuración y la lógica de la aplicación del manejo del routing**. 

### Rutas

Definimos _rutas_ usando los métodos que nos provee el objeto que genera Express, que se corresponden con **métodos HTTP**: por ejemplo podemos usar `app.get()` para manejar requests de tipo `GET` y `app.post()` para requests de tipo `POST`. 

**Nota:** los métodos de `app` pueden encadenarse

```js
const express = require('express');
const app = express();
const PORT = 8080;

app
  .get('/', (req, res) => res.send('Hola Mundo!'))
  .listen(PORT, () => console.log(`Express app listening on port ${PORT}!`));
```

También existe el método `.all`, que va a aceptar cualquier tipo de _request_

```js
// '*' es una ruta comodín, cualquier request a cualquier ruta que hagamos va a caer acá
app.all('*', (req, res) => res.send('Hello'));
```

Ver [todos los métodos HTTP disponibles](https://expressjs.com/en/4x/api.html#app.METHOD)

### URL Parameters

Podemos proveer diferentes respuestas a través de una misma ruta si utilizamos parámetros. Vamos a obtener estos parámetros a través de la URL. Esto se conoce como **_dynamic routing_** (ruteo dinámico). Por el contrario, las rutas que no tienen parámetros son _rutas estáticas_.

Estos parámetros se almacenan en el objeto `params`, el cual existe dentro del objeto `request`.

Para especificar qué parte de una URL será un parámetro, agregamos el caracter `:` y luego le damos un nombre. Esto se va a mapear a una propiedad con ese nombre dentro del objeto `params`. 

**Es importante notar que los parámetros URL son siempre `strings`**.

### ⚠️ El orden en el que declaramos las rutas importa

[DESPUES DESARROLLO]

## Middleware

Las funciones middleware... **son funciones** que tienen acceso al _objeto request_ (`req`), al _objeto response_ (`res`) y a la _siguiente función middleware_ (generalmente identificada con una variable llamada _next_) en el ciclo request-response de nuestra aplicación.

Vamos a usar `app.use()` para indicar que vamos a utilizar un _middleware_ determinado como función _callback_

Ver [Using middleware](https://expressjs.com/en/guide/using-middleware.html)

### Tipos de _middleware_

Una aplicación Express puede utiilizar los siguientes tipos de _middleware_:

- Application-level middleware
- Router-level middleware
- Error-handling middleware
- Built-in middleware
- Third-party middleware

## Práctica

### Setup

1. Crear un archivo `.env`. En este archivo vamos a setear las _variables de entorno_ de nuestro proyecto. 
2. Instalar y utilizar `nodemon` para correr nuestras aplicaciones, con el script `dev` del `package.json`
3. Instalar y utilizar [`dotenv`](https://www.npmjs.com/package/dotenv) para leer _variables de entorno_ de un archivo `.env`

```js
const dotenv = require('dotenv');
dotenv.config();
```

Luego accederemos a las variables definidas en el archivo `.env` a través de `process.env`. Por ejemplo

```js
// .env
PORT=8080
```

```js
// server.js
const { PORT } = process.env;
```

:warning: **Nota**: el archivo `.env` no debe comitearse, agregarlo al `.gitignore`

### Ejercicios

1. Crear un servidor en Express, que escuche en el puerto `8080` (leerlo del archivo `.env`) y responda con un `'Hola Mundo!'` cuando reciba un request a la ruta `/`. En el caso de que no haya un puerto seteado en `.env`, la aplicación debe escuchar en el puerto `8001`. Cuando el servidor esté levantado y corriendo, la aplicación debe loguear por consola `Express app listening on port ${PORT}!`, donde `PORT` es el puerto seteado.
2. Modificar el ejercicio anterior, para que al recibir un request a la ruta `/salute/{name}`, la aplicación responda con un `'Hola {name}!'`, donde `name` es un parámetro que recibe por URL. 
3. Rehacer el [ejercicio 3](https://github.com/undefinedschool/notes-nodejs#ejercicios-1) utilizando Express. Usar [`pug` como _template engine_](http://expressjs.com/en/guide/using-template-engines.html) para retornar las diferentes _vistas HTML_ de nuestra aplicación.
4. Rehacer el [ejercicio 6](https://github.com/undefinedschool/notes-nodejs#ejercicios-1), [sirviendo los archivos estáticos (assets) con `Express`](https://expressjs.com/en/starter/static-files.html) desde la carpeta `/public`.
