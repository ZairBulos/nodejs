# Trabajo con archivos y directorios en una aplicación Node.js

Crearemos una aplicación que manipule archivos y directorios con Node.js.

### Requisitos previos

* Conocimientos del lenguaje de programación JavaScript.
* Familiaridad con las construcciones de programación básicas, como los bucles o las instrucciones "if".

### Objetivos de aprendizaje

* Trabajar con directorios
* Crear y eliminar archivos
* Leer archivos
* Escribir en archivos
* Analizar datos en archivos

<hr/>

## Introducción

Node.js tiene mecanismos integrados tremendamente eficaces para trabajar con el sistema de archivos.

Imaginemos por un momento que trabaja para una empresa llamada Tailwind Traders. Tailwind Traders es el segundo minorista en línea más grande del mundo y, como tal, debe lidiar con ingentes cantidades de datos y archivos. La empresa le ha contratado para ayudarle a administrar sus datos y archivos mediante Node.js.

En este módulo, se escribe un programa que busque archivos de ventas en las carpetas. Cuando se encuentren esos archivos, se usará Node.js para leer y analizar los datos totales de ventas que haya en ellos. Por último, resumiremos esos totales de ventas en un total general y escribiremos ese valor en un archivo nuevo.

<hr/>

## Trabajo con el sistema de archivos

A menudo, los grandes minoristas escriben datos en archivos para que se puedan procesar posteriormente en lotes.

En Tailwind Traders, cada una de sus tiendas escribe sus totales de ventas en un archivo y ese archivo se envía a una ubicación central. Para usar esos archivos, la compañía debe crear un proceso por lotes que pueda funcionar con el sistema de archivos.

Aquí obtendrá información sobre cómo usar Node.js para leer el sistema de archivos con el propósito de descubrir archivos y directorios.

### Inclusión del módulo fs

Node.js proporciona un módulo integrado para trabajar con el sistema de archivos. Este módulo se denomina *fs*. El nombre es la abreviatura de "sistema de archivos" (file system en inglés).

El módulo fs se incluye de forma predeterminada en Node.js, por lo que no es necesario instalarlo desde npm.

Si el módulo fs no se puede ver ni en el sistema de archivos ni en la carpeta node_modules, puede resultar un poco confuso. Así pues, ¿cómo se incluye el módulo fs en un proyecto? Hay que hacer referencia a él, como haríamos con cualquier otra dependencia.

El módulo fs tiene un espacio de nombres `promises` que tiene versiones de promesas de todos los métodos. El uso del espacio de nombres prometido es la forma preferida de trabajar con el módulo fs, ya que le permite usar `async`. Evita el lío de las devoluciones de llamada o el bloqueo de métodos sincrónicos.

```JavaScript
const fs = require("fs").promises;
```

Puede usar el módulo fs para realizar varias operaciones en archivos y directorios. Tiene varios métodos entre los que elegir. Por el momento, nos centraremos únicamente en lo que necesitamos saber para trabajar con directorios mediante el módulo fs.

### Muestra del contenido en un directorio

Una de las tareas que se harán bastante a menudo con el módulo fs es mostrar o *enumerar* el contenido en un directorio. Por ejemplo, Tailwind Traders tiene una carpeta raíz denominada stores. En esa carpeta hay subcarpetas organizadas por número de tienda. Dentro de esas carpetas están los archivos de totales de ventas. El aspecto de esta estructura es el siguiente:

```text
📂 stores
    📄 sales.json
    📄 totals.txt
    📂 201
    📂 202
```

Para leer el contenido de la carpeta, se puede usar el método `readdir`. La mayoría de las operaciones del módulo fs tienen opciones sincrónicas y asincrónicas.

El método `readdir` devuelve una lista de elementos:

```JavaScript
const items = await fs.readdir("stores");
console.log(items); // [ 201, 202, sales.json, totals.txt ]
```

Los métodos `readdir` y `readdirsync` devuelven resultados en orden alfabético.

### Distinción del tipo de contenido

Cuando se lee el contenido de un directorio, se obtienen las carpetas y los archivos como una matriz de cadenas. Para distinguir cuáles son archivos y cuáles directorios, se puede pasar la opción `withFileTypes`. Esta opción devuelve una matriz de objetos `Dirent`, en lugar de una matriz de cadenas. El objeto `Dirent` tiene los métodos `isFile` y `isDirectory`, que se pueden usar para determinar con qué tipo de objeto estamos tratando.

```JavaScript
const items = await fs.readdir("stores", { withFileTypes: true });
for (let item of items) {
  const type = item.isDirectory() ? "folder" : "file";
  console.log(`${item.name}: ${type}`);
  // 201: folder, 202: folder, sales.json: file, totals.txt: file
}
```

### Un apunte sobre recursividad

Un requisito bastante habitual es tener carpetas con subcarpetas, que a su vez también tienen subcarpetas. En alguna parte de este árbol de carpetas anidadas están los archivos que necesita. Se necesita un programa capaz de encontrar los archivos en el árbol de carpetas. Para ello, debe determinar si un elemento es una carpeta y, después, buscar los archivos en esa carpeta. Repita esta operación con cada carpeta que encuentre.

Para realizar búsquedas en estructuras de directorio anidadas, podemos usar un método que encuentre carpetas y que, después, se llame a sí mismo para encontrar carpetas dentro de esas carpetas. De esta manera, el programa nos "guía" por el árbol de directorios hasta que lea todas las carpetas en su interior. Cuando un método se llama a sí mismo, a esto se le denomina *recursividad*.

```JavaScript
function findFiles(folderName) {
  const items = await fs.readdir(folderName, { withFileTypes: true });
  items.forEach((item) => {
    if (item.isDirectory()) {
      // this is a folder, so call this method again and pass in
      // the path to the folder
      findFiles(`${folderName}/${item.name}`);
    } else {
      console.log(`Found file: ${item.name} in folder: ${folderName}`);
    }
  });
}

findFiles("stores");
```

La recursividad es una característica potente de muchos lenguajes de programación. Probablemente se usará mucho en el mundo real.

En el siguiente ejercicio, usaremos el módulo fs para leer dinámicamente el directorio principal stores de Tailwind Traders con el fin de encontrar todos los archivos sales.json.

<hr/>

## Trabajo con rutas de acceso de archivo en Node.js

Node.js tiene un mecanismo integrado para trabajar con las rutas de acceso del sistema de archivos.

Si se tuvieran muchos archivos o carpetas, crear las rutas de acceso manualmente puede resultar muy tedioso. Node.js proporciona algunas constantes y utilidades integradas para que crear rutas de acceso de archivos sea una tarea más sencilla.

Aquí se obtiene información sobre el módulo *path* de Node.js y la constante `__dirname`, que nos permiten obtener un programa más inteligente y resistente.

### Distinción del directorio actual

A veces no sabe en qué directorio se está ejecutando el programa. Solo lo necesita para usar la ruta de acceso del directorio actual. Node.js expone la ruta de acceso completa al directorio actual a través de la constante `__dirname`.

```JavaScript
console.log(__dirname);
```

Si se ejecuta ese código desde la carpeta *sales* en la estructura de carpetas siguiente, el valor de `_dirname` es `/stores/201/sales`.

```text
📂 stores
    📂 201
        📂 sales
```

### Trabajo con rutas de acceso

Las rutas de acceso es un tema que surge tan a menudo que Node.js incluye un módulo denominado *path* dedicado expresamente para funcionar con rutas de acceso.

Al igual que el módulo *fs*, el módulo *path* se distribuye junto con Node.js y no es necesario instalarlo. Solo se tiene que hacer referencia a dicho módulo al inicio del archivo.

```JavaScript
const path = require("path");
```

#### Rutas de combinación

El módulo *path* funciona con el concepto de rutas de acceso de archivos y carpetas, que son simplemente cadenas. Por ejemplo, si se quiere obtener la ruta de acceso a la carpeta *stores/201*, se puede usar el módulo *path* para ello.

```JavaScript
console.log(path.join("stores", "201")); // stores/201
```

El motivo por el que usaríamos el módulo *path* en lugar de concatenar cadenas reside en que, de este modo, el programa puede ejecutarse en Windows o Linux. El módulo *path* aplica el formato adecuado a las rutas de acceso para cualquier sistema operativo en el que se ejecute. En el ejemplo anterior, `path.join` devolvería `stores\201` en Windows, con una barra diagonal inversa en lugar de una barra diagonal.

#### Determinación de extensiones de nombre de archivo

El módulo path también puede identificar la extensión de un nombre de archivo. Si tiene un archivo y quiere identificar si es JSON, puede usar el método path.extname.

```JavaScript
console.log(path.extname("sales.json"));
```

> 💡 **Sugerencia** <br/>
> El módulo *path* no se preocupa de si algo realmente existe o no. Las rutas de acceso son algo conceptual, no físico. Solo se crean y analizan cadenas automáticamente.

#### Obtención de todo lo que es necesario saber sobre un archivo o ruta de acceso

El módulo *path* contiene muchos métodos diferentes que realizan diversas acciones. Sin embargo, con el método `parse` podemos obtener la mayor parte de la información que se necesita sobre una ruta de acceso o un archivo. Este método devuelve un objeto que contiene el directorio actual en el que se encuentra, el nombre del archivo, la extensión de nombre de archivo e incluso el nombre del archivo sin la extensión.

```JavaScript
console.log(path.parse("stores/201/sales.json"));
// { root: '', dir: 'stores/201', base: 'sales.json', ext: '.json', name: 'sales' }
```

Aunque existen muchos métodos más útiles en el módulo *path*, los métodos que se explican aquí son los conceptos básicos que probablemente se usen con más frecuencia.

<hr/>

## Creación de archivos y directorios

Crear y eliminar archivos y directorios nuevos mediante programación es un requisito habitual de las aplicaciones de línea de negocio.

Hasta ahora, ha obtenido información sobre cómo trabajar con archivos y directorios mediante el módulo fs. También se puede usar el módulo fs para crear, eliminar, copiar, mover o manipular de cualquier otra forma archivos y directorios en un sistema mediante programación.

Aquí obtiene información sobre cómo usar el módulo fs para crear directorios y archivos.

### Creación de directorios

El método `mkdir` permite crear directorios. Con el siguiente método se crea una carpeta denominada *newDir* dentro de la carpeta 201.

```JavaScript
const fs = require("fs").promises;
const path = require("path");

await fs.mkdir(path.join(__dirname, "stores", "201", "newDir"));
```

La estructura de archivos */stores/201* ya debe existir; de lo contrario, se produce un error en este método. Se puede pasar una marca `recursive` opcional si la estructura de archivos no existe y quiere que la operación la cree.

```JavaScript
await fs.mkdir(path.join(__dirname, "newDir", "stores", "201", "newDir"), {
  recursive: true
});
```

### Garantía de que los directorios existen

Si el directorio que intenta crear ya existe, el método `mkdir` genera un error. Esa situación no es buena, porque el error hace que el programa se detenga abruptamente. Para evitar esta situación, Node.js recomienda ajustar el método `mkdir` en un elemento `try/catch` si tiene previsto manipular el archivo o el directorio después de abrirlo, como nosotros.

```JavaScript
const pathToCreate = path.join(__dirname, "stores", "201", "newDirectory");

// create the salesTotal directory if it doesn't exist
try {
  await fs.mkdir(salesTotalsDir);
} catch {
  console.log(`${salesTotalsDir} already exists.`);
}
```

### Creación de archivos

Se pueden crear archivos mediante el método `fs.writeFile`. Este método toma una ruta de acceso al archivo y los datos que se van a escribir en él. Si el archivo ya existe, se sobrescribe.

Por ejemplo, este código crea un archivo denominado *greeting.txt* con el texto "Hola mundo" dentro.

```JavaScript
await fs.writeFile(path.join(__dirname, "greeting.txt", "Hello World!"));
```

Si se omite el tercer parámetro, que son los datos que se van a escribir en el archivo, Node.js escribe "undefined" en el archivo. Probablemente esta situación no sea lo que busca. Para escribir un archivo vacío, pasaremos una cadena vacía. Una opción aún mejor sería pasar la función `String`, que en la práctica hace lo mismo, pero sin dejar comillas vacías en el código.

```JavaScript
await fs.writeFile(path.join(__dirname, "greeting.txt", String()));
```

<hr/>

## Lectura y escritura en archivos

La lectura de datos de archivos y la escritura de datos en archivos son conceptos básicos en Node.js.

Tailwind Traders necesita escribir el total de todos sus archivos de ventas de tiendas individuales en un nuevo archivo. Posteriormente, este archivo se carga en el sistema de ventas de la empresa.

Aquí obtiene información sobre cómo usar el módulo fs para leer archivos y escribir en ellos.

### Lectura de datos de archivos

Los archivos se leen a través del método `readFile` del módulo fs.

```JavaScript
await fs.readFile("stores/201/sales.json");
```

El objeto devuelto del método `readFile` es un objeto `Buffer`. Incluye el contenido del archivo que se ha leído, pero en formato binario. Por ejemplo, supongamos que tiene el siguiente archivo llamado *sales.json* con el contenido siguiente.

```text
{
  "total": 22385.32
}
```

Al cerrar sesión, el valor devuelto del método `readFile` proporcionará el valor `Buffer`.

```JavaScript
console.log(await fs.readFile("stores/201/sales.json"));
// <Buffer 7b 0a 20 20 22 74 6f 74 61 6c 22 3a 20 32 32 33 38 35 2e 33 32 0a 7d>
```

Ese resultado no resulta útil. Posiblemente se haya leído el archivo, pero es cierto que se pueden "leer" estos datos. No pasa nada. JavaScript puede convertir un valor `Buffer` en un valor de cadena que se puede leer. Para ello, invoque el objeto `String` y pase el búfer.

```JavaScript
const bufferData = await fs.readFile("stores/201/sales.json");
console.log(String(bufferData));
// {
//   "total": 22385.32
// }
```

### Análisis de datos en archivos

Estos datos en su formato de cadena no nos aportan demasiado. Son solo caracteres, si bien ahora tienen un formato que se puede leer. Lo que se quiere es la capacidad de analizar estos datos en un formato que podamos usar mediante programación.

JavaScript incluye un analizador integrado de archivos JSON. No es necesario incluir nada para usarlo, solo el objeto `JSON`. Como ventaja extra, tampoco es necesario convertir un valor `Buffer` en una cadena antes de analizarlo. De eso se encarga el método `JSON.parse`.

```JavaScript
const data = JSON.parse(await fs.readFile("stores/201/sales.json"));
console.log(data.total);
// 22385.32
```

> 💡 **Sugerencia** <br/>
> Los archivos presentan una gran variedad de formatos. Los archivos JSON son los que más nos conviene usar debido a su compatibilidad integrada en el lenguaje, pero se puede encontrar con archivos .csv, de ancho fijo o con cualquier otro formato. En tal caso, lo mejor es buscar en npmjs.org un analizador acorde a ese tipo de archivo.

### Escritura de datos en archivos

Ha obtenido información sobre cómo escribir archivos en el ejercicio anterior, solo que escribimos en uno vacío. Para escribir datos en un archivo, deberemos usar el mismo método `writeFile`, pero pasando los datos que queremos escribir como tercer parámetro.

```JavaScript
const data = JSON.parse(await fs.readFile("stores/201/sales.json"));

// write the total to the "totals.json" file
await fs.writeFile("salesTotals/totals.txt"), data.total);

// totals.txt
// 22385.32
```

#### Anexión de datos en archivos

En el ejemplo anterior, el archivo se sobrescribe cada vez que se escribe en él. A veces, esto no es lo que se busca, A veces, lo que quiere es anexar datos al archivo, no reemplazarlo por completo. Para ello, se puede pasar una marca al método `writeFile`. De forma predeterminada, la marca se establece en `w`, lo que significa "reemplazar el archivo". Para anexar al archivo en su lugar, pase la marca `a`, que significa "anexar".

```JavaScript
const data = JSON.parse(await fs.readFile("stores/201/sales.json"));

// write the total to the "totals.json" file
await fs.writeFile(path.join("salesTotals/totals.txt"), `${data.total}\r\n`, {
  flag: "a"
});

// totals.json
// 22385.32
// 22385.32
```

> 💡 **Sugerencia** <br/>
> En el código de ejemplo anterior, `\r\n` indica a JavaScript que ponga el valor en su propia línea. Si no se pasara este valor (conocido como *avance de línea de retorno de carro*), se obtendrían todos los números en la misma línea, apelotonados.