# Creación de una API web con Node.js y Express

Usaremos Express para Node.js para crear API RESTful. Crearemos y configuraremos un middleware para agregar cosas como funciones de registro, autenticación y autorización junto a otras tecnologías de desarrollo web.

### Requisitos previos

* Tener Git y Node.js instalado en el equipo
* Conocimientos básicos de edición de texto y archivos de código en cualquier editor de texto
* Conocimientos básicos de HTTP
* Experiencia en el uso de la línea de comandos, incluidas las operaciones de Git

### Objetivos de aprendizaje

* Describir los conceptos básicos del marco web Express.
* Configurar middleware para controlar cómo se trata una solicitud.

<hr/>

## Introducción

Usará la web como plataforma para ejecutar la aplicación. Esta plataforma permite que cualquiera llegue a la aplicación mediante un explorador, cliente o software compatible con HTTP.

En este módulo, es el desarrollador de un distribuidor en línea. El distribuidor va a crear un conjunto de API HTTP para su aplicación. La aplicación se basa en Node.js. El trabajo consiste en crear una API que muestre los productos que venden. La API que va a compilar permite que las aplicaciones trabajen con los datos de los productos.

Puede crear una página web mediante páginas HTML, JavaScript y CSS. Node.js tiene un módulo principal llamado HTTP que sirve para crear aplicaciones web. Admite solicitudes para leer, escribir y trabajar con distintos tipos de contenido.

Aunque el módulo HTTP de Node.js tiene capacidad suficiente, no es tan rápido como usar un marco. Para crear una API eficaz que pueda controlar operaciones complejas como las de autenticación y autorización, también usará un marco de trabajo.

Hay muchos marcos web para Node.js, como Hapi, Fastify, Koa y Express. Muchos desarrolladores usan Express. Existe desde hace mucho tiempo. Las API están bien pensadas, hay parches para los problemas de seguridad, etc.

En este módulo, aprenderá a controlar solicitudes HTTP con Node.js. También obtendrá información sobre el marco Express, que permite crear sitios web y API HTTP.

<hr/>

## Creación de una aplicación web del marco Express

Las empresas suelen almacenar una gran cantidad de datos en sistemas de archivos y bases de datos. Para acceder a esos datos, los proporcionan por medio de aplicaciones web y API mediante HTTP.

A continuación, se muestran conceptos importantes que se deben tener en cuenta al crear aplicaciones web y API:

- **Enrutamiento**: la aplicación se divide en distintas secciones según las partes de la dirección URL.
- Compatibilidad con distintos tipos de contenido: los datos que se van a proporcionar pueden estar en diferentes formatos de archivo, como texto sin formato, JSON, HTML y CSV, entre otros.
- **Autenticación o autorización**: algunos datos pueden ser confidenciales. Es posible que un usuario tenga que iniciar sesión o disponer de un rol o nivel de permiso concreto para acceder a los datos.
- **Lectura y escritura de datos**: normalmente los usuarios necesitan ver y agregar datos al sistema. Para agregar datos, los usuarios pueden introducirlos en un formulario o cargar archivos.
- **Tiempo de comercialización**: para crear aplicaciones web y API de forma eficaz, elija las herramientas y bibliotecas que proporcionen soluciones a problemas comunes. Estas opciones ayudan a los desarrolladores a dedicar tanto tiempo como sea posible a los requisitos empresariales del trabajo.

### Módulo HTTP en Node.js

Node.js incluye un módulo HTTP integrado. Se trata de un módulo relativamente pequeño y que controla la mayoría de los tipos de solicitudes. Admite tipos comunes de datos como encabezados, la dirección URL y las cargas.

Las clases siguientes ayudan a administrar una solicitud de principio a fin:

- `http.Server`: representa una instancia de un servidor HTTP. Este objeto debe recibir la instrucción de escuchar diferentes eventos en una dirección y un puerto específicos.
- `http.IncomingMessage`: este objeto es una secuencia legible creada por http.Server o http.ClientRequest. Se usa para acceder al estado, los encabezados y los datos.
- `http.ServerResponse`: este objeto es una secuencia creada de forma interna por el servidor HTTP. Esta clase define el aspecto de la respuesta, por ejemplo, el tipo de encabezados y el contenido de la respuesta.

### Aplicación web de ejemplo

```JavaScript
const http = require('http');
const PORT = 3000;

const server = http.createServer((req, res) => {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('hello world');
});

server.listen(PORT, () => {
  console.log(`listening on port ${PORT}`)
})
```

En este ejemplo se configura la aplicación con los pasos siguientes:

1. **Creación del servidor**: el método `createServer()` crea una instancia de la clase `http.Server`.
2. **Implementación de la devolución de llamada**: el método `createServer()` espera una función conocida como *devolución de llamada*. Cuando se invoca la devolución de llamada, se proporcionan al método instancias de las clases `http.IncomingMessage` y `http.ServerResponse`. En este ejemplo, se proporcionan las instancias req y res:
    - **Solicitud cliente**: el objeto `req` investiga qué encabezados y datos se han enviado en la solicitud del cliente.
    - **Respuesta del servidor**: para crear una respuesta, el servidor indica al objeto `res` los datos y encabezados de respuesta con los que debe responder.
3. **Inicio de la escucha de solicitudes**: se invoca el método `listen()` con un puerto especificado. Después de la llamada al método `listen()`, el servidor está listo para aceptar solicitudes de cliente.

### Secuencias
 
Las *secuencias* no son un concepto de Node.js, sino del sistema operativo. Las secuencias definen la forma en que se transportan los datos. Los datos se envían fragmento a fragmento desde un cliente a un servidor y desde un servidor a un cliente. Las secuencias permiten que el servidor pueda controlar muchas solicitudes a la vez.

Una secuencia es una estructura de datos fundamental de Node.js que puede leer y escribir datos, así como enviar y recibir mensajes o *eventos*. En el módulo HTTP, el streaming se implementa mediante clases que son secuencias.

En el ejemplo, los parámetros `req` y `res` son secuencias. Use el método on() para escuchar los datos entrantes de una solicitud de cliente como la siguiente:

```JavaScript
req.on('data', (chunk) => {
  console.log('You received a chunk of data', chunk)
})
```

Use el método `end()` para los datos devueltos al cliente en la secuencia de respuesta del objeto `res`:

```JavaScript
res.end('some data')
```

### Marco Express

Hasta ahora, ha visto las funciones del módulo HTTP de Node.js. Es una opción perfectamente válida en aplicaciones web más pequeñas. Si una aplicación aumenta de tamaño, un marco como Express puede ayudarle a crear la arquitectura de una manera escalable.

Después de haber compilado varias aplicaciones web, comprobará que resuelve una y otra vez los mismos problemas. Problemas como la administración de rutas, la autenticación y la autorización, o la administración de errores son habituales. Llegado a este punto, empieza a buscar una biblioteca o un marco que se encargue de estos problemas, ya sea parcial o totalmente.

¿Por qué usar Express como marco para la próxima aplicación?

- **Características de calidad**: Express tiene un conjunto de características que le permiten ser rápido y productivo.
- **Sin complejidad**: Express prescinde de conceptos complicados como, las secuencias, y, por tanto, hace que toda la experiencia de desarrollo sea mucho más fácil.
- **Solución de problemas web comunes**: Express ayuda a solucionar problemas comunes, como la administración de rutas, el almacenamiento en caché y el redireccionamiento.
- **Confianza de millones de desarrolladores**: según GitHub, en la actualidad 6,8 millones de desarrolladores usan Express para sus aplicaciones web.

#### Administración de rutas en Express

Cuando un cliente realiza una solicitud de una aplicación web, usa una dirección URL, es decir, una dirección que apunta a un servidor concreto. Una dirección URL puede tener el aspecto siguiente:

```text
http://localhost:3000/products
```

El término `localhost` de la URL hace referencia a su equipo. En una dirección URL más orientada a producción, posiblemente el término `localhost` se sustituya por un nombre de dominio como `microsoft.com`. La parte final de la dirección URL es la *ruta*. Decide a qué lugar concreto del servidor dirigirse. En este caso, la ruta es `/products`.

En el marco Express se usan la dirección URL, la ruta y verbos HTTP para la administración de rutas. Los verbos HTTP como `post`, `put` y `get` describen la acción deseada por el cliente. Cada verbo HTTP tiene un significado específico para lo que debería ocurrir con los datos. Express ayuda a registrar rutas y a emparejarlas con los verbos HTTP adecuados para organizar la aplicación web.

Express dispone de métodos dedicados para administrar distintos verbos HTTP y un sistema inteligente para asociar diferentes rutas con otras partes del código. En el ejemplo siguiente, Express le ayuda a controlar las solicitudes dirigidas a una ruta con la dirección `/products` asociada con el verbo HTTP `get`:

```JavaScript
app.get('/products', (req, res) => {
  // handle the request
})
```

Express ve `get` hacia `/products` de forma distinta a `post` hacia `/products`, como se muestra en el ejemplo de código siguiente:

```JavaScript
app.get('/products', (req, res) => {
  // handle the request
})

app.post('/products', (req, res) => {
  // handle the request
})
```

El verbo HTTP `get` significa que un usuario quiere leer datos. El verbo HTTP `post` significa que un usuario quiere escribir datos. Tiene sentido dividir la aplicación para que distintos emparejamientos de verbo y ruta ejecuten otras partes del código. Este concepto se describirá con más detalle posteriormente.

#### Servicio de tipos de contenido diferentes

Express admite muchos formatos de contenido diferentes que se pueden devolver a un cliente que realiza la llamada. El objeto `res` incluye un conjunto de funciones auxiliares para devolver distintos tipos de datos. Para devolver texto sin formato, usaríamos el método `send()` así:

```JavaScript
res.send('plain text')
```

Para otros tipos de datos, como JSON, existen métodos dedicados que garantizan el tipo de contenido y las conversiones de datos correctos. Para devolver JSON en Express, use el método `json()` de esta forma:

```JavaScript
res.json({ id: 1, name: "Catcher in the Rye" })
```

El método de código de Express anterior equivale a este código de módulo HTTP:

```JavaScript
res.writeHead(200, { 'Content-Type': 'application/json' });
res.end(JSON.stringify({ id: 1, name: "Catcher in the Rye" }))
```

Se establece el encabezado `Content-Type` en HTTP y la respuesta también se convierte de un objeto JavaScript a una versión de *cadena JSON* antes de devolverla al cliente que realiza la llamada.

Si se comparan los dos ejemplos de código, puede ver que, al usar métodos auxiliares para tipos de archivo comunes, como JSON y HTML, con Express se evita tener que escribir algunas líneas.

#### Creación de una aplicación Express

Para empezar a desarrollar una aplicación de Node.js con el marco Express, debemos instalarlo como una dependencia. También se recomienda inicializar primero un proyecto de Node.js para que las dependencias descargadas terminen en el archivo **package.json**. Esta recomendación es general para cualquier aplicación desarrollada para el runtime de Node.js. Las ventajas de hacerlo serán palpables cuando el código se inserte en un repositorio como GitHub, ya que cualquiera que obtenga el código desde GitHub podrá usarlo fácilmente si primero instala sus dependencias.

Siga estos pasos para crear una aplicación web con el marco Express:

1. **Cree una instancia de la aplicación**: cree una instancia de una aplicación web. En este momento, la instancia no se puede ejecutar, pero tiene algo que se puede ampliar.
2. **Defina rutas y controladores de ruta**: defina a qué rutas debe escuchar la aplicación. Una ruta forma parte de la dirección URL. Por ejemplo, en la dirección URL `http://localhost:8000/products`, el elemento de ruta es `/products`. Express usa otras rutas para ejecutar diferentes fragmentos de código. Otros ejemplos de rutas son barra diagonal `/`, que también se conoce como ruta predeterminada, y `/orders`. Las rutas se explorarán con más detalle en una sección posterior de este módulo.
3. **Configure el middleware**: el middleware es un fragmento de código que se puede ejecutar antes o después de una solicitud. También puede usar middleware para controlar la autenticación y la autorización, o bien para agregar una funcionalidad a la aplicación.
4. **Inicie la aplicación**: defina un puerto y, después, indique a la aplicación que escuche en ese puerto. La aplicación ya está lista para recibir solicitudes.

<hr/>

## Ejercicio: Crear una aplicación web Express

Se nos ha asignado la tarea de crear una API sencilla usando el marco Express. El minorista en línea quiere evaluar Express para ver si es fácil de usar. Como parte de esa evaluación, les gustaría que cree una aplicación web que proporcione diferentes rutas.

### Creación de una aplicación web básica con Express

Cree una aplicación básica que controle las solicitudes.

1. Abra un terminal y escriba los comandos siguientes:

    ```Bash
    npm init -y
    npm install express
    ```

    El comando `init` crea un archivo package.json predeterminado para el proyecto de Node.js. El comando install `instala` el marco Express.

2. En un editor de código, abra el archivo package.json. En la sección dependencies, busque la entrada express:

    ```Bash
    "dependencies": {
    "express": "^4.18.1"
    ```

3. En un editor de código, cree un archivo denominado *app.js* y agregue el código siguiente:

    ```JavaScript
    const express = require('express');
    const app = express();
    const port = 3000;

    app.get('/', (req, res) => res.send('Hello World!'));

    app.listen(port, () => console.log(`Example app listening on port ${port}!`));
    ```

    El código crea una instancia de una aplicación de Express mediante la invocación del método `express()`.

    Observe cómo el código configura una ruta a barra diagonal `/` con la sintaxis:

    > `app.get('/', (req, res) => res.send('Hello World!'))`;

    Después de configurar la ruta, el código inicia la aplicación web mediante la invocación del método `listen()`:

    > `app.listen(port, () => console.log(Example app listening on port ${puerto}!))`;

4. Guarde los cambios en el archivo *app.js* y ciérrelo.

5. En el terminal, ejecute el comando siguiente para iniciar la aplicación web Express:

    ```Bash
    node app.js
    ```

    Debería aparecer la siguiente salida:

    ```text
    Example app listening on port 3000!
    ```

6. Vaya a `http://localhost:3000` en un explorador. Debería ver la siguiente salida:
    
    ```text
    Hello World!
    ```

7. En el terminal, presione `Ctrl + C` para detener el programa web de Express.

### Creación de una aplicación web que devuelve datos JSON

Use el mismo archivo *app.js* para agregar una ruta nueva.

1. En un editor de código, abra el archivo app.js. Agregue el código siguiente después de la sintaxis `app.get` existente:

    ```JavaScript
    app.get("/products", (req,res) => {
    const products = [
    {
        id: 1,
        name: "hammer",
    },
    {
        id: 2,
        name: "screwdriver",
    },
    {
        id: 3,
        name: "wrench",
    },
    ];

    res.json(products);
    });
    ```

2. Asegúrese de que el archivo app.js es similar a este ejemplo:

    ```JavaScript
    const express = require("express");
    const app = express();
    const port = 3000;

    app.get("/", (req, res) => res.send("Hello World!"));

    app.get("/products", (req,res) => {
    const products = [
        {
        id: 1,
        name: "hammer",
        },
        {
        id: 2,
        name: "screwdriver",
        },
        ,
        {
        id: 3,
        name: "wrench",
        },
    ];

    res.json(products);
    })

    app.listen(port, () => console.log(`Example app listening on port ${port}!`));
    ```

3. Guarde los cambios en el archivo *app.js* y ciérrelo.

4. En el terminal, ejecute el comando siguiente para reiniciar la aplicación web Express:

    ```Bash
    node app.js
    ```

    Debería ver la siguiente salida:

    ```text
    Example app listening on port 3000!
    ```

5. Vaya a `http://localhost:3000/products` en un explorador. Debería ver la siguiente salida:

    ```text
    [{"id":1,"name":"hammer"},{"id":2,"name":"screwdriver"},{"id":3,"name":"wrench"}]
    ```

6. En el terminal, presione Ctrl + C para detener el programa web de Express.

<hr/>

## Administración del ciclo de vida de una solicitud con middleware

En algunos casos, cuando una solicitud llega a una aplicación web, es posible que tenga que comprobar si el usuario ha iniciado sesión o si tiene permiso para ver ese recurso concreto.

### Pasos de la solicitud

La administración de una solicitud se debe considerar como una serie de pasos. Si el usuario tiene que iniciar sesión para administrar un recurso, los pasos podrían ser estos:

1. **Anterior a la solicitud**: investigue si el usuario ha enviado las credenciales adecuadas a través de un encabezado de solicitud. Si las credenciales son correctas, la solicitud se envía al paso siguiente.
2. **Construcción de la respuesta**: se necesita una conexión a algún tipo de origen de datos, como una base de datos o un punto de conexión. Este paso devuelve el recurso, siempre y cuando la solicitud lo solicite correctamente.
3. **Posterior a la solicitud**: es un paso opcional para ejecutar un fragmento de código después de que se controle la solicitud. Puede ejecutar este paso con fines de registro.

El marco Express dispone de compatibilidad integrada para controlar solicitudes de este modo. Para ejecutar una solicitud anterior o posterior, implemente el método `use()` en el objeto de instancia de Express. En Express, una solicitud anterior o posterior se conoce como *middleware*, y tiene la sintaxis siguiente:

```JavaScript
app.use((req, res, next) => {})
```

El método que se pasa en el método `use()` tiene tres parámetros: `req`, `res` y `next`. El significado de estos parámetros es el siguiente:

- `req`: la solicitud entrante que contiene encabezados de solicitud y la dirección URL de llamada. Es posible que también tenga un cuerpo de datos si el cliente ha enviado datos con su solicitud.
- `res`: una secuencia de respuesta para escribir información como encabezados y datos que quiera devolver al cliente que realiza la llamada.
- `next`: un parámetro que indica que la solicitud es Correcta y está lista para procesarse. Si no se llama al parámetro `next()`, se detiene el procesamiento de la solicitud. Además, se recomienda indicar al cliente por qué no se ha procesado la solicitud, por ejemplo, mediante una llamada a `res.send('\<specify a reason why the request is stopped>'\)`.

### Canalización de la solicitud

Si tiene rutas que podrían beneficiarse de que el middleware ejecute una solicitud anterior o posterior, configúrelo para que:

- El middleware que se tiene que ejecutar antes de la solicitud (solicitud anterior) se defina antes de la solicitud real.
- El middleware que se tiene que ejecutar después de la solicitud (solicitud posterior) se defina después de la solicitud real.

Fíjese en este ejemplo:

```JavaScript
app.use((req, res, next) => {
  // Pre request
})
app.get('/protected-resource', () => {
  // Handle the actual request
})
app.use((req, res, next) => {
  // Post request
})

app.get('/login', () => {})
```

También se puede ejecutar código de middleware de solicitud anterior como argumento del control de la solicitud, de esta forma:

```JavaScript
app.get(
  '/<some route>',
 () => {
   // Pre request middleware
 }, () => {
   // Handle the actual request
 })
```

<hr/>

## Ejercicio: Administrar el ciclo de vida de una solicitud

El distribuidor en línea necesita que la aplicación tenga una seguridad básica. La aplicación web Express debe diferenciar entre los clientes registrados que tienen acceso y otros usuarios que no deben tenerlo. Es posible que posteriormente se agreguen otras características, como la administración de roles.

### Adición de autorización básica a un marco Express

La mayoría de las aplicaciones tienen partes a las que cualquiera puede acceder. Pero algunas de ellas se deben proteger. Hay diferentes maneras de proteger una aplicación. En este ejercicio, implementará un sencillo sistema de protección para entender cómo funciona el mecanismo de *middleware* en el marco Express.

### Clonación del proyecto

En este ejercicio, usará un proyecto de ejemplo que tiene archivos de producto y código de aplicación de inicio. Rellenará las partes que faltan del proyecto para completar las actualizaciones de la aplicación para el cliente.

1. En un terminal, clone el repositorio de ejemplo para este ejemplo mediante la ejecución del siguiente comando:

    ```Bash
    git clone https://github.com/MicrosoftDocs/node-essentials
    ```

2. Para inspeccionar el repositorio clonado, cambie a la carpeta e*xercise-express-middleware* del proyecto:

    ```Bash
    cd node-essentials/nodejs-http/exercise-express-middleware
    ```

3. El archivo package.json contiene una dependencia express. Ejecute el comando siguiente para instalar la dependencia:

    ```Bash
    npm install
    ```

4. Abra el archivo *app.js* e inspeccione el contenido:

    ```JavaScript
    const express = require("express");
    const app = express();
    const port = 3000;

    app.get("/", (req, res) => res.send("Hello World!"));

    app.get("/users", (req, res) => {
    res.json([
        {
        id: 1,
        name: "User Userson",
        },
    ]);
    });

    app.get("/products", (req, res) => {
    res.json([
        {
        id: 1,
        name: "The Bluest Eye",
        },
    ]);
    });

    app.listen(port, () => console.log(`Example app listening on port ${port}!`));
    ```

5. Abra el archivo de aplicación *client.js* e inspeccione el contenido:

    ```JavaScript
    const http = require("http");

    http.get(
    {
        port: 3000,
        hostname: "localhost",
        path: "/users",
        headers: {},
    },
    (res) => {
        console.log("connected");
        res.on("data", (chunk) => {
        console.log("chunk", "" + chunk);
        });
        res.on("end", () => {
        console.log("No more data");
        });
        res.on("close", () => {
        console.log("Closing connection");
        });
    }
    );
    ```

    El código de la aplicación cliente se conecta a la dirección `http://localhost:3000/users` de la ruta `/users`. El cliente escucha tres eventos: `data`, `end` y `close`.

### Ejecución del programa de Express

1. En el terminal, escriba este comando para ejecutar el programa de Express:

    ```Bash
    node app.js
    ```

2. En un segundo terminal, ejecute la aplicación cliente:

    - Vaya a la ubicación donde ha clonado el repositorio del proyecto. Debe estar en la carpeta node-essentials.
    
    - Cambie a la carpeta que contiene el archivo client.js:
    
        ```Bash
        cd node-essentials/nodejs-http/exercise-express-middleware
        ```
    
    - Inicie la aplicación cliente:
    
        ```Bash
        node client.js
        ```

    En el segundo terminal, debería ver la siguiente salida del cliente:

    ```text
    connected
    chunk [{"id":1,"name":"User Userson"}]
    No more data
    Closing connection
    ```

    El programa de Express responde con datos de usuario, `chunk [{"id":1,"name":"User Userson"}]`. Todos los elementos de la aplicación funcionan.

    El programa cliente finaliza después de mostrar la salida.

3. En el primer terminal, presione `Ctrl + C` para detener el programa.

### Protección de la ruta

Para proteger esta ruta, se agregará código a la aplicación de Express.

1. Abra el archivo *node-essentials/nodejs-http/exercise-express-middleware/app.js*. Busque la instrucción `const app = express()`. Después de esta instrucción, agregue el código siguiente:

    ```JavaScript
    function isAuthorized(req, res, next) {
    const auth = req.headers.authorization;
    if (auth === 'secretpassword') {
        next();
    } else {
        res.status(401);
        res.send('Not permitted');
    }
    }
    ```

2. A continuación, busque la siguiente sección de código en el mismo archivo:

    ```JavaScript
    app.get("/users", (req, res) => {
    res.json([
        {
        id: 1,
        name: "User Userson",
        },
    ]);
    });
    ```

3. Reemplace esta sección por el código siguiente:

    ```JavaScript
    app.get("/users", isAuthorized, (req, res) => {
    res.json([
        {
        id: 1,
        name: "User Userson",
        },
    ]);
    });
    ```

    En el código actualizado, el middleware `isAuthorized` se agrega como segundo argumento.

4. Guarde y cierre el archivo *app.js*.

### Ejecución del programa de Express e invocación del middleware

1. En el primer terminal, ejecute el comando siguiente para reiniciar el programa de Express:

    ```Bash
    node app.js
    ```

2. En el segundo terminal, reinicie la aplicación cliente:

    ```Bash
    node client.js
    ```

    En el segundo terminal, debería ver la siguiente salida:

    ```text
    connected
    chunk Not permitted
    No more data
    Closing connection
    ```

    Esta vez, se invoca el middleware `isAuthorized()` y se busca un encabezado `authorization` que tenga un valor específico. Como no ha proporcionado un valor concreto como parte de la solicitud, el código no ha respondido con datos de usuario específicos. En su lugar, la respuesta ha sido `chunk Not permitted`. En la sección siguiente agregará una autorización concreta.

3. En el primer terminal, presione `Ctrl + C` para detener el programa.

### Adición del encabezado de autenticación

Tendrá que agregar un encabezado authorization para un valor específico.

1. Vuelva a abrir el archivo *node-essentials/nodejs-http/exercise-express-middleware/client.js*. Busque la instrucción siguiente:

    ```JavaScript
    headers: {},
    ```

2. Reemplace esta instrucción por el código siguiente:

    ```JavaScript
    headers: {
    authorization: 'secretpassword'
    },
    ```

3. Guarde y cierre el archivo client.js.

### Ejecución del programa de Express con autorización de cliente

1. En el primer terminal, ejecute el comando siguiente para reiniciar el programa de Express:

    ```Bash
    node app.js
    ```

2. En el segundo terminal, ejecute el comando siguiente para volver a ejecutar el cliente:

    ```Bash
    node client.js
    ```

    En el segundo terminal, ahora debería ver la siguiente salida:

    ```text
    connected
    chunk [{"id":1,"name":"User Userson"}]
    No more data
    Closing connection
    ``` 

    Se devuelven los datos de usuario, porque se ha pasado un encabezado `authorization` con un valor aceptado.

3. En el primer terminal, presione `Ctrl + C` para detener el programa.

> ❌ **Precaución** <br/>
> Tenga en cuenta que una autenticación o autorización concebidas para usarlas en el mundo real deben ser algo más sólidas que las de este ejemplo. Merece la pena consultar conceptos como OAuth, tokens web JSON, JWT y la biblioteca bcrypt para asegurarse de que la aplicación presenta un nivel de protección aceptable.