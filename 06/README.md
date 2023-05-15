# Introducción a la administración de rutas en Node.js con JavaScript

Aprenda a configurar una API de Node.js con varias rutas y a controlar las solicitudes HTTP entrantes mediante JavaScript.

### Requisitos previos

* Tener Git y Node.js instalado en el equipo
* Conocimientos básicos de edición de texto y archivos de código en cualquier editor de texto
* Conocimientos básicos de HTTP
* Experiencia en el uso de la línea de comandos, incluidas las operaciones de Git

### Objetivos de aprendizaje

* Información sobre la administración de rutas de API
* Implementación del control de rutas en una aplicación de Node.js con JavaScript

<hr/>

## Introducción

Las API web de Node.js permiten que cualquier usuario que use un explorador o un software cliente compatible con HTTP acceda a todas sus aplicaciones. Las distintas rutas disponibles en la API ofrecen más funcionalidades a estos usuarios.

En este módulo, es el desarrollador de un distribuidor en línea. El distribuidor va a crear un conjunto de API HTTP para su aplicación. La aplicación se basa en Node.js. Su trabajo es crear una API que enumere los productos que vende el distribuidor. La API permite que las aplicaciones trabajen con los datos de los productos.

Si bien Node.js presenta algunas funcionalidades básica a través del módulo HTTP para crear una API, muchos marcos web simplifican esta configuración. En concreto, muchos desarrolladores usan Express, que ha estado disponible desde hace mucho. Puede usarlo para centrarse en la creación de las rutas necesarias para admitir sus aplicaciones.

En este módulo, aprenderá a crear rutas de API con Node.js y Express mediante JavaScript. También aprenderá sobre las partes de las direcciones URL, los componentes de una ruta y cómo funcionan todos estos elementos.

<hr/>

## Información sobre las direcciones URL y las rutas

Una aplicación tiene varios recursos, como productos o pedidos. Divida la aplicación en secciones para los recursos. El uso de secciones le ayuda a mantener y ampliar la aplicación.

Una forma sencilla de ampliar una aplicación web consiste en asegurarse de que se pueda acceder a los recursos mediante direcciones URL dedicadas. Dos direcciones URL desencadenan dos partes diferentes del código en la aplicación web.

En esta unidad se describe qué es una dirección URL, junto con otros conceptos para crear una API.

### URL

Una dirección URL es una dirección que un usuario escribe en un cliente, como un explorador, para encontrar un servidor específico y un recurso específico. Saber cómo funciona la dirección URL le ayuda a organizar la aplicación en torno a ella.

Esta es una dirección URL típica:

```text
http://localhost:8000/products/1?page=1&pageSize=20
```

La dirección URL se ajusta a una sintaxis similar a esta:

```text
scheme:[//authority]path[?query][#fragment]
```

A continuación se explicarán las partes:

#### Scheme

La parte del esquema de una dirección URL indica el protocolo. En el ejemplo anterior de una dirección URL típica, el esquema es `http`. Otros ejemplos de esquemas son `https`, `ftp`, `irc` y `file`.

#### Authority

La autoridad consta de información de usuario opcional (*username@password*) y un host. En la dirección URL de ejemplo, `localhost` es la parte del host y apunta a su propia máquina como servidor web. En la web, la parte del host suele ser un nombre de dominio, como `google` o `microsoft`.

Es un nombre descriptivo que se especifica en lugar de una dirección IP. La dirección IP es la dirección web real. Es una serie de números, como `127.0.0.1`. Una dirección IP hace que los enrutadores puedan enrutar solicitudes fácilmente de una parte de la web a otra. Pero no son descriptivas. Los hosts y los nombres de dominio existen para que se puedan recordar.

#### Ruta de acceso

La parte de la ruta de acceso de la dirección URL puede estar compuesta por muchos segmentos o ninguno. Cada segmento está separado por una barra diagonal (`/`). En la dirección URL de ejemplo, el único segmento es `/products`. Un segmento filtra los recursos exactos que le interesan.

#### Consultar

Una consulta es una parte opcional de la dirección URL, que se define después del carácter de signo de interrogación (`?`). Consta de pares de parámetro/valor de consulta delimitados por una Y comercial (`&`) o un punto y coma (`;`). Filtra aún más los datos mediante la solicitud de varios registros de una página concreta.

La consulta en la dirección URL de ejemplo es `?page=1&pageSize=20`. Imagine que tiene 2 millones de registros en un recurso. Se tardaría mucho tiempo en devolverlos todos. Si especifica que quiere 20 registros, los datos se devuelven rápidamente y tienen un tamaño pequeño.

#### Fragmento

La parte del fragmento de la dirección URL ayuda a ser incluso más específico. Por ejemplo, un fragmento puede ordenar los datos que se solicitan por un criterio en particular.

### Ruta

Una ruta es una subsección de una dirección URL que normalmente apunta a un recurso específico. Express maneja los siguientes conceptos para las rutas como ayuda para que sea más intencionado.

#### Parámetro de ruta

Un parámetro de ruta en una dirección URL indica que quiere acceder a un recurso específico de una colección.

Examine la ruta `/orders/1/items/2`. Los parámetros de la ruta son 1 y 2. 1 indica que buscamos un pedido específico con la clave única 1. mientras que 2 solicita un elemento de pedido específico con la clave única 2. Estos parámetros de ruta devuelven un recurso específico, en lugar de todos los recursos de un tipo específico.

Express define rutas, a las que asocia diferentes *controladores*. Los controladores son código que se invoca cuando existe una coincidencia con una ruta de acceso determinada. Express tiene un mecanismo de control de patrones para administrar rutas diferentes. En la tabla siguiente se muestran rutas diferentes expresadas como patrones de ruta.

| Ruta              | Patrón de ruta de Express    |
| ----------------- | ---------------------------- |  
| /products         | products/                    |
| /products/1       | products/id:                 |
| /orders/1/items/2 | order/:orderId/items/:itemId |
| xyz               | **

Escriba el código para que coincida con `/products/114` en la tabla, de esta forma:

```JavaScript
app.get('/products/:id', (req, res) => {
  // handle this request `req.params.id`
})
```

Los parámetros de ruta se escriben en una propiedad params en el objeto de solicitud `req`. Una solicitud de `/products/114` tendría r`eq.params.id` que contiene 114.

#### Parámetro de consulta

La parte de la consulta de la dirección URL es un conjunto de pares clave-valor que aparecen después del carácter `?`. En el ejemplo de ruta, `/products?page=1&pageSize=20`, se muestran los parámetros de consulta `page` y `pageSize`. Estos dos parámetros trabajan de forma conjunta para ayudar a filtrar el tamaño de una respuesta devuelta.

Imagine que la ruta `/products` ha devuelto 2 millones de registros de una base de datos. Esa respuesta sería enorme y tardaría mucho tiempo en aparecer. Eso provoca una experiencia de usuario deficiente y supone presión para la aplicación. Un enfoque más indicado consiste en usar parámetros de consulta para limitar el tamaño de la respuesta.

Express tiene una forma sencilla de controlar los parámetros de consulta. Dada la ruta `/products?page=1&pageSize=20`, los parámetros de consulta se escriben en un objeto de consulta en el objeto de solicitud `res`, como en el ejemplo siguiente:

```JavaScript
app.get('/products', (req, res) => {
  // handle this request `req.query.page` and `req.query.pageSize`
})
```

Para crear una solicitud con la ruta `/products?page=1&pageSize=20`, `req.query` tiene el siguiente valor:

```JavaScript
{
  page: 1,
  pageSize: 20
}
```

#### Administración de patrones generales

Hasta ahora, ha visto rutas sencillas, como `/products` y `/orders/1/items/2`. Hay otros patrones, como `**`, que podrían significar *catch-all*. Normalmente definiría una ruta de este tipo para asegurarse de que las solicitudes inesperadas, como los errores tipográficos, se controlan de forma estable. Esta ruta ayuda a ofrecer una buena experiencia del usuario.

#### Verbo HTTP

Un verbo HTTP expresa el *qué*. Los verbos HTTP, como `get` y `post`, representan distintas intenciones. Al usar el verbo `get`, expresa que quiere leer datos del recurso. El verbo `post` significa que quiere escribir datos en el recurso.

Express tiene métodos específicos que permiten asociar código con un fragmento de dirección URL y un verbo HTTP específicos.

<hr/>

## Ejercicio: Parámetros de ruta y consulta

Normalmente, los datos residen en una base de datos o en un punto de conexión. El tamaño de los datos puede ser enorme. Cuando un usuario solicita todos los datos de un recurso concreto, la respuesta puede contener miles o incluso millones de registros. Una solicitud como esta puede generar una enorme presión en una base de datos. También tarda mucho tiempo en proporcionar la respuesta.

Para evitar este escenario, se recomienda limitar el tamaño de la respuesta:

- Use parámetros de ruta para solicitar registros específicos.
- Use parámetros de consulta para especificar un subconjunto de registros.

En este ejercicio se enseñan ambas técnicas.

### Configuración de archivos en el repositorio

1. Clone el repositorio node-essentials mediante el comando siguiente:

    ```Bash
    git clone https://github.com/MicrosoftDocs/node-essentials
    ```

2. Para inspeccionar el repositorio que ha clonado, ejecute el comando siguiente:

    ```Bash
    cd node-essentials/nodejs-http/exercise-express-routing/parameters
    ```

    El esquema del directorio debe ser similar al siguiente:

    ```text
    -| app.js
    -| package.json
    -| package-lock.json
    ```

3. El archivo *package.json* contiene la dependencia express. Ejecute el comando siguiente en el terminal para instalarlo:

    ```Bash
    npm install
    ```

4. Abra *app.js* para inspeccionarlo. El archivo debería tener este aspecto:

    ```JavaScript
    const express = require('express')
    const app = express()
    const port = 3000

    const products = [
    {
    id: 1,
    name: "Ivanhoe",
    author: "Sir Walter Scott",
    },
    {
    id: 2,
    name: "Colour Magic",
    author: "Terry Pratchett",
    },
    {
    id: 3,
    name: "The Bluest eye",
    author: "Toni Morrison",
    },
    ];

    app.get('/', (req, res) => res.send('Hello API!'))

    app.get("/products/:id", (req, res) => {
    res.json(products.find(p => p.id === +req.params.id));
    });

    app.get('/products', (req, res) => {})

    app.listen(port, () => console.log(`Example app listening on port ${port}!`))
    ```

### Implementación de dos rutas

El código contiene una aplicación Express. El paso siguiente consiste en implementar dos rutas:

- `/products/:id`: esta ruta debe devolver un único producto.
- `/products` esta ruta debe devolver todos los productos o tantos como hayan solicitado los parámetros de consulta.

1. Para implementar la ruta `/products/:id`, busque el código siguiente:

    ```JavaScript
    app.get("/products/:id", (req, res) => {});
    ```

    Reemplácelo por este código:

    ```JavaScript
    app.get("/products/:id", (req, res) => {
    res.json(products.find(p => p.id === +req.params.id));
    });
    ```

2. En el terminal, ejecute el comando siguiente para ejecutar la aplicación:

    ```Bash
    node app.js
    ```

3. Abra un explorador y vaya a `http://localhost:3000/products/1`. Debería ver la salida siguiente:

    ```JSON
    {
    "id": 1,
    "name": "Ivanhoe",
    "author": "Sir Walter Scott"
    }
    ```

4. Para implementar la ruta /products, busque el código siguiente:

    ```JavaScript
    app.get('/products', (req, res) => {})
    ```

    Reemplácelo por este código:

    ```JavaScript
    app.get('/products', (req, res) => {
    const page = +req.query.page;
    const pageSize = +req.query.pageSize;

    if (page && pageSize) {
        const start = (page - 1) * pageSize;
        const end = start + pageSize;
        res.json(products.slice(start, end));
    } else {
        res.json(products);
    }
    });
    ```

5. En el terminal, ejecute el comando siguiente para iniciar la aplicación y probar el código:

    ```Bash
    node app.js
    ```

6. Abra un explorador y vaya a `http://localhost:3000/products?page=1&pageSize=2`. Debería ver la salida siguiente en el explorador:

    ```JSON
    [{
    "id": 1,
    "name": "Ivanhoe",
    "author": "Sir Walter Scott"
    },
    {
    "id": 2,
    "name": "Colour Magic",
    "author": "Terry Pratchett"
    }]
    ```

    De los tres registros, en la respuesta se muestran los dos primeros. Esta respuesta significa que los parámetros de consulta `page` y `pageSize` han filtrado el tamaño de la respuesta.

7. Cambie la dirección URL por `http://localhost:3000/products?page=2&pageSize=2` para cambiar el número de páginas de uno a dos. La respuesta debería tener el siguiente aspecto:

    ```JSON
    [{
    "id": 3,
    "name": "The Bluest eye",
    "author": "Toni Morrison"
    }]
    ```

    Como solo hay tres registros, la segunda página solo debe contener uno.

<hr/>

## Lectura y escritura

Hasta ahora, ha visto ejemplos de solicitudes realizadas a una aplicación web cuando el cliente quiere leer datos. Sin embargo, es probable que también quiera escribir datos.

Para escribir datos, use un verbo HTTP que coincida con su intención. Dado que los datos entrantes pueden tener varias formas, configure la aplicación de Express para que coincida con cómo llegan los datos a la aplicación:

```JavaScript
app.get('/<path>', (req, res) => {
  // handling the request
})
```

### Configuración de la aplicación para recibir datos

Para controlar un cliente que envía datos a la aplicación web, configure Express de forma diferente en función del formato de los datos entrantes. Por ejemplo, los datos pueden estar en formato HTML o JSON. Con independencia del formato de los datos, hay pasos en común.

1. **Importe un analizador de cuerpo**. Tiene que convertir los datos entrantes en un formato legible. Importe la biblioteca `body-parser` que se instala con Express:

    ```JavaScript
    let bodyParser = require('body-parser');
    ```

2. **Configure el tipo de datos**. Configure Express para analizar los datos del cuerpo entrante en el formato previsto. El código siguiente convierte los datos en JSON:

    ```JavaScript
    app.use(bodyParser.json({ extended: false }));
    ```

Los datos que envía un cliente ahora están disponibles en la propiedad `body` en el objeto de solicitud `req`. Ahora puede leer estos datos y comunicarse con un origen de datos. A continuación, puede crear un recurso a partir de los datos o actualizar un recurso, en función de si la solicitud usa un verbo POST o PUT.

### Control de los datos de solicitud

Para controlar una solicitud entrante, use el método `post()` o `put()` en la instancia de Express. Los dos métodos funcionan, pero `post()` le dice a Express que se quiere crear un recurso. El método `put()` se usa para informar de que un recurso se debe actualizar con los datos entrantes. Este es un ejemplo:

```JavaScript
app.post('/<path>', (req, res) => {
  console.log('req.body', req.body) // contains incoming data
})
```

<hr/>

## Ejercicio: Lectura y escritura

El minorista en línea está impresionado con la primera aplicación web. Ahora quiere que cree una API en la que se pueda leer y escribir. Es posible que los datos estén almacenados en una base de datos y que contengan millones de registros. Por este motivo, la empresa quiere una aplicación en la que se usen técnicas para limitar la cantidad de datos que se solicitan.

### Implementación de la compatibilidad para leer y escribir datos

Es habitual construir una API con muchos recursos. Cada recurso puede tener varias operaciones disponibles para la lectura y la escritura. La organización por recurso y por operaciones como las de lectura o escritura se denomina CRUD (acrónimo del inglés *create*, *read*, *update*, *delete*). Implemente la API CRUD en el recurso `products`:

1. Para inspeccionar el repositorio que ha clonado e ir a los archivos que necesita, ejecute este comando:

    ```Bash
    cd node-essentials/nodejs-http/exercise-express-routing/reading-writing
    ```

    El esquema del directorio debe ser similar al siguiente:

    ```text
    -| app.js
    -| client-get.js
    -| client-post.js
    -| client-put.js
    -| client-delete.js
    -| client-delete-route.js
    -| package.json
    ```

2. El archivo *package.json* contiene una dependencia `express`. Ejecute el comando siguiente en el terminal para instalarlo:

    ```Bash
    npm install
    ```

3. Abra *app.js* para inspeccionarlo. El archivo debería tener este aspecto:

    ```JavaScript
    const express = require('express')
    const app = express()
    const port = 3000

    let bodyParser = require('body-parser');
    app.use(bodyParser.json());

    let products = [];

    app.post('/products', function(req, res) {
    // implement
    });

    app.put('/products', function(req, res) {
    // implement
    });

    app.delete('/products/:id', function(req, res) {
    // implement
    });

    app.get('/products', (req, res) => {
    // implement
    })
    app.listen(port, () => console.log(`Example app listening on port ${port}!`));
    ```

### Implementación de rutas

Para implementar rutas, agregue un poco de código y después pruébelo. Realice la operación método por método hasta que tenga una API totalmente funcional.

1. Admita la lectura desde la API. Busque la parte del código con este aspecto:

    ```JavaScript
    app.get('/products', (req, res) => {
    // implement
    })
    ```

    Reemplácelo por este código:

    ```JavaScript
    app.get('/products', (req, res) => {
    res.json(products);
    })
    ```

2. Para comprobar que el código funciona, inicie la API ejecutando este comando:

    ```Bash
    node app.js
    ```

3. En un terminal aparte, ejecute este comando:

    ```Bash
    node client-get.js
    ```

    Debería obtener la siguiente salida:

    ```text
    Received data []
    Connection closed
    ```

La API responde con una matriz vacía porque todavía no hemos escrito datos en ella. Vamos a cambiar eso a continuación.

### Implementación de la escritura

1. Para implementar la escritura, busque este código:

    ```JavaScript
    app.post('/products', function(req, res) {
    // implement
    });
    ```

    Reemplácelo por este código:

    ```JavaScript
    app.post('/products', function(req, res) {
    const newProduct = { ...req.body, id: products.length + 1 }
    products = [ ...products, newProduct]
    res.json(newProduct);
    });
    ```

    El nuevo código lee los datos entrantes de `req.body` y los usa para crear un objeto de JavaScript. Tras ello, se agrega a la matriz `products`. Por último, el nuevo producto se devuelve al usuario.

2. Para probar el código, ejecute el programa de servidor ejecutando este comando:

    ```Bash
    node app.js
    ```

3. En un terminal aparte, ejecute este comando:

    ```Bash
    node client-post.js
    ```

La salida debe ser parecida a la siguiente:

    ```text
    response {"name":"product","id":1}
    Closed connection
    ```

4. Para comprobar que los datos se escriben en la API, ejecute el comando siguiente:

    ```Bash
    node client-get.js
    ```

    Debería ver la siguiente salida:

    ```text
    Received data [{"name":"product","id":1}]
    Connection closed
    ```

    La respuesta le indica que, cuando se ha ejecutado client-post.js, se han escrito datos en la API. Además, ha ejecutado *client-get.js* para consultar los datos de la API. La API ha respondido con los datos que acaba de escribir en ella.

### Implementación de la capacidad de actualizar datos

1. Para implementar la capacidad de actualizar los datos, busque este código:

    ```JavaScript
    app.put('/products', function(req, res) {});
    ```

    Reemplácelo por este código:

    ```JavaScript
    app.put('/products', function(req, res) {
    let updatedProduct;
    products = products.map(p => {
        if (p.id === req.body.id) {
        updatedProduct = { ...p, ...req.body };
        return updatedProduct;
        }
        return p;
    })
    res.json(updatedProduct);
    });
    ```

    El nuevo código busca el registro de la matriz `products` que coincide con la propiedad `id` y lo actualiza.

2. Para probar el código, inicie la aplicación de servidor:

    ```Bash
    node app.js
    ```

3. En el otro terminal, ejecute este comando para crear un registro:

    ```Bash
    node client-post.js
    ```

4. Ejecute este comando para actualizar el registro recién creado:

    ```Bash
    node client-put.js
    ```

    Debería aparecer la siguiente actualización en el terminal:

    ```text
    response {"name":"product-updated","id":1}
    Closed connection
    ```

5. Para comprobar que la actualización funciona, ejecute este comando:

    ```Bash
    node client-get.js
    ```

    Debería ver esta actualización:

    ```text
    Received data [{"name":"product-updated","id":1}]
    Connection closed
    ```

### Implementación de la eliminación

1. Para implementar la eliminación, busque un código similar a este:

    ```JavaScript
    app.delete('/products/:id', function(req, res) {});
    ```

    Reemplácelo por este código:

    ```JavaScript
    app.delete('/products/:id', function(req, res) {
    const deletedProduct = products.find(p => p.id === +req.params.id);
    products = products.filter(p => p.id !== +req.params.id);
    res.json(deletedProduct);
    });
    ```

    El nuevo código busca el elemento de producto que se va a eliminar. y, después, lo filtra de la matriz `products` y responde con una versión filtrada de `products`.

2. Para probar el código, inicie la aplicación de servidor:

    ```Bash
    node app.js
    ```

3. En un terminal aparte, ejecute este comando para crear un registro:

    ```Bash
    node client-post.js
    ```

4. Ejecute este comando para quitar el registro:

    ```Bash
    node client-delete.js
    ```

    Debería ver la salida siguiente:

    ```text
    Received data {"name":"product","id":1}
    Connection closed
    ```

5. Para comprobar el código, ejecute este comando:

    ```Bash
    node client-get.js
    ```

    Debe proporcionar esta salida:

    ```text
    Received data []
    Connection closed
    ```

### Implementación de CRUD

La implementación de CRUD para un recurso es una tarea habitual. Express tiene un método `route()` concebido para este propósito. Cuando se usa el método `route()`, el código se agrupa para que sea más fácil de leer.

Para implementar CRUD, reemplace el código de *app.js* con este código:

```JavaScript
const express = require('express')
const app = express()
const port = 3000

let bodyParser = require('body-parser');
app.use(bodyParser.json());

let products = [];

app.route('/products')
 .get((req, res) => {
   res.json(products);
 })
 .post((req, res) => {
   const newProduct = { ...req.body, id: products.length + 1 }
   products = [...products, newProduct]
   res.json(newProduct);
 })
.put((req, res) => {
   let updatedProduct;
   products = products.map(p => {
     if (p.id === req.body.id) {
       updatedProduct = { ...p, ...req.body };
       return updatedProduct;
     }
     return p;
   })
   res.json(updatedProduct);
 })
 .delete((req, res) => {
   const deletedProduct = products.find(p => p.id === +req.body.id);
   products = products.filter(p => p.id !== +req.body.id);
   res.json(deletedProduct);
 })

app.listen(port, () => console.log(`Example app listening on port ${port}!`))
```

Ha usado *client-delete-route.js* en lugar de *client-delete.js* en el ejercicio anterior. La diferencia radica en cómo se implementa la ruta. La primera versión de app.js se basa en eliminaciones realizadas en una ruta como `/products/<id>`, donde el identificador único se envía como parámetro de ruta.

Cuando se usa el método `route()`, implementa la ruta de eliminación de manera diferente. Hace que el identificador único se envíe a través del cuerpo en vez de como parámetro de ruta. No hay una manera correcta o incorrecta de implementar una ruta de eliminación.