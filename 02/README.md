# Creación de un proyecto de Node.js y trabajo con dependencias

Use las dependencias del registro npm para desarrollar más rápido aplicaciones de Node.js. Obtenga información sobre cómo administrar las dependencias del proyecto.

### Requisitos previos

* Tener Git y Node.js instalados en el equipo
* Conocimientos básicos de edición de texto y archivos de código en un editor de texto
* Experiencia en el uso de la línea de comandos, incluidas las operaciones de Git

### Objetivos de aprendizaje

* Inicializar proyectos de Node.js
* Entender en qué consiste el archivo de manifiesto package.json y usarlo para su beneficio
* Agregar y eliminar paquetes del proyecto de Node.js
* Administrar las dependencias de paquete y actualizarlas de forma predecible

<hr/>

## Introducción

Cuando trabaja en una aplicación, escribe código para cumplir los requisitos empresariales. Por motivos de velocidad y confiabilidad, es posible que usted y el equipo no escriban todo el código por su cuenta, Puede que recurra a código externo, o *bibliotecas*, escrito por otra persona.

Una manera de enfocar la compilación de la aplicación con bibliotecas externas consiste en usar un ecosistema de bibliotecas existente del que se puedan realizar descargas y que, posiblemente, se pueda ampliar. Mediante el uso de estas bibliotecas, podrá acabar de compilar la aplicación antes y comercializarla mucho antes que la competencia. Otra ventaja de usar bibliotecas podría ser para garantizar que la aplicación siga los procedimientos recomendados para la autenticación y la autorización. Al fin y al cabo, la protección de los datos propios y los de los clientes es un aspecto muy importante.

En este módulo, usará la herramienta de línea de comandos npm y el registro global npm para agregar bibliotecas al código de la aplicación. También aprenderá a administrar actualizaciones y mitigar problemas.

<hr/>

## Configuración del archivo package.json

El archivo package.json es un archivo de manifiesto para el proyecto de Node.js. Contiene información de metadatos sobre el proyecto. También rige aspectos como el modo en que se administran las dependencias, qué archivos se incluyen en un paquete destinado a npm y mucho más. Ahora se verán con más detalle las propiedades y su significado.

### Inicialización de un archivo package.json

Un archivo package.json no es algo que cree manualmente. Es el resultado de ejecutar el comando `init` de npm. Hay dos formas principales de ejecutar este comando:

- `npm init`: este comando inicia un asistente que le pide información sobre el nombre, la versión, la descripción, el punto de entrada, el comando de prueba, el repositorio de Git, las palabras clave, el autor y la licencia de un proyecto.
- `npm init -y`: este comando usa la marca `-y` y es una versión más rápida del comando npm init porque no es interactivo. En su lugar, este comando asigna automáticamente valores predeterminados a todos los valores que se le piden mediante npm init.
Los comandos npm init y npm init -y generan un archivo package.json. Este es un ejemplo:

```JSON
{
  "name": "my project",
  "version": "1.0.0",
  "description": "",
  "main": "script.js",
  "dependencies": {},
  "devDependencies": {},
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

### Contenido del archivo package.json

Puede considerar todas las propiedades posibles en el archivo package.json como pertenecientes a los grupos siguientes:

- **Metainformación**: las propiedades de este grupo definen la metainformación del proyecto. Las propiedades incluyen el nombre del proyecto, la descripción, el autor, las palabras clave, etc.
- **Dependencias**: hay dos propiedades que describen las bibliotecas que se usan: `dependencies` y `devDependencies`. Más adelante en el módulo, aprenderá a usar estas propiedades para instalar, actualizar y separar las dependencias.
- **Scripts**: en esta sección, puede enumerar scripts para las tareas del proyecto, como el inicio, la compilación, las pruebas y lint.

#### Scripts para administrar el proyecto

Con independencia de que use Node.js, es probable que le interese disponer de una manera de ejecutar, probar y compilar cualquier proyecto. El tiempo de ejecución de Node.js reconoce esta necesidad y proporciona una guía sobre cómo asignar un nombre a los scripts. La idea es asegurarse de que se usen nombres de script coherentes en todos los proyectos de Node.js. Es una mejor experiencia de desarrollador si es capaz de moverse entre proyectos de Node.js y orientarse rápidamente, ya que verá un conjunto coherente de acciones. Varias herramientas para DevOps e instrumentación pueden aprovechar esta coherencia de nomenclatura.

Debe configurar cuatro scripts y asignarles un nombre de una forma específica. Los siguientes nombres específicos son los que esperan la comunidad de desarrolladores, y varias herramientas:

- `start`: invoca el comando `node` con el archivo de entrada como argumento. Un ejemplo podría ser `node ./src/index.js`. Esta acción invoca el comando `node` y usa el archivo de entrada `index.js`.
- `build`: describe cómo compilar el proyecto. El proceso de compilación debe producir algo que se pueda distribuir. Por ejemplo, un comando de compilación podría ejecutar un compilador de TypeScript para generar la versión de JavaScript del proyecto que quiere distribuir.
- `test`: ejecuta las pruebas del proyecto. Si usa una biblioteca de pruebas de terceros, el comando debe invocar el archivo ejecutable de la biblioteca.
- `lint`: invoca un programa linter como ESLint. Mediante *linting* se buscan incoherencias en el código. Por lo general, un linter también ofrece una forma de corregir las incoherencias. Tener un código coherente puede aumentar enormemente la legibilidad, lo que acelera el desarrollo de características y adiciones al código.

#### Nomenclatura de los scripts

Ahora que conoce los tipos de scripts que debe tener, ¿cómo les asignará un nombre?

Primero se examinará la sintaxis general. Después, se verá cómo debe asignar un nombre a los scripts.

La sintaxis de una sección `scripts` suele tener este aspecto:

```JSON
"scripts" : {
  "<action>" : "<command>"
}
```

A continuación se muestra un ejemplo más realista:

```JSON
"scripts" : {
  "start" : "node ./dist/index.js",
  "test": "jest",
  "build": "tsc",
  "lint": "eslint"
}
```

En este ejemplo se usa la nomenclatura que hemos revisado anteriormente. En primer lugar, se produce la acción `start` que inicia la aplicación. Después, la acción `test` ejecuta las pruebas con el marco de pruebas jest. A continuación, tiene la acción `build`, que usa el compilador de TypeScript tsc. Este comando compila el código de TypeScript en un formulario que el explorador puede interpretar, como ES6. Por último, hay la herramienta de linting `eslint`, que busca incoherencias y posibles errores en el código.

Para invocar las acciones, escriba el comando `npm run <action>`. Por ejemplo, `npm run lint`.

Las acciones `start` y `test` son especiales en que puede omitir la palabra `run` en el comando. En lugar de escribir el comando `npm run start`, puede escribir `npm start`.

Estas cuatro acciones son un buen punto inicial para cualquier proyecto de Node.js, pero es probable que cada proyecto tenga algunos scripts adicionales específicos del propio proyecto.

<hr/>

## Ejercicio: Configuración del archivo package.json

Es un desarrollador de Node.js en Tailwind Traders. Le interesa mucho saber cómo configurar un nuevo proyecto de Node.js. La instalación incluye la generación de un archivo de manifiesto y la creación de algunos scripts comunes que probablemente se usarán durante el ciclo de vida del proyecto.

### Configuración de un nuevo proyecto de Node.js

1. Abra una ventana de terminal.

2. Ejecute este comando para clonar el repositorio `https://github.com/MicrosoftDocs/node-essentials/`:

```Bash
git clone https://github.com/MicrosoftDocs/node-essentials/
```

3. Cambie a la carpeta que tiene los archivos clonados para este ejercicio:

```Bash
cd node-essentials/node-dependencies/3-exercise-package-json
```

En esta carpeta, debería ver una subcarpeta src con un archivo index.js:

```text
-| src/
---| index.js
```

4. Ejecute el comando siguiente para inicializar un proyecto de Node.js:

```Bash
npm init -y
```

Este comando genera un archivo package.json que tiene un aspecto similar al de este ejemplo:

```JSON
{
  "name": "<your project>",
  "version": "1.0.0",
  "description": "",
  "main": "script.js",
  "dependencies": {},
  "devDependencies": {},
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

5. Edite el archivo package.json y modifique estos valores de propiedad:

    - `name`: "tailwind-trader-api"
    - `description`: "API HTTP para administrar elementos de la base de datos de Tailwind Traders"
    - `main`: "index.js"
    - `keywords`: ["api", "base de datos"]
    - `author`: "Sergio"

6. En la sección `scripts`, agregue esta definición para la acción `start` antes de la definición de la acción `test`:

```JSON
"start": "node ./src/index.js",
```

7. Para iniciar el proyecto con la acción `start`, escriba este comando:

```Bash
npm start
```

Debería ver este resultado:

```text
Welcome to this application
```

<hr/>

## Incorporación de paquetes al proyecto de Node.js

Node.js incluye muchas bibliotecas principales que lo controlan todo, desde la administración de archivos hasta HTTP, la compresión de archivos y mucho más. Pero existe un ecosistema enorme de bibliotecas de terceros. Gracias a npm, el Administrador de paquetes de Node, puede instalar fácilmente estas bibliotecas y usarlas en la aplicación.

En Node.js y en su ecosistema se usa mucho la palabra *dependencia*. Se trata de una biblioteca de terceros, es decir, un fragmento de código reutilizable que lleva a cabo una tarea y se puede agregar a la aplicación. La biblioteca de terceros es algo de lo que la aplicación *depende* para funcionar, de ahí la palabra *dependencia*.

La biblioteca de terceros se puede considerar como un paquete y almacenarse en un registro. Por tanto, un paquete es una colección de archivos necesarios para ejecutar uno o varios módulos. Un paquete consta de uno o más módulos que se pueden importar en la aplicación para aprovechar sus características.

### Determinación de la necesidad de un paquete

¿Cómo puede saber si necesita un paquete para el proyecto? Esta es una pregunta complicada que implica varios factores:

- **Obtener mejor código**: ¿el programa necesita admitir una tarea específica (por ejemplo, seguridad) en la que debe implementar autenticación y autorización? La seguridad es un tipo de tarea que hay que realizar bien para proteger los datos personales y los de los clientes. Muchos desarrolladores usan patrones de seguridad y bibliotecas estándar. Estas bibliotecas implementan características que probablemente siempre necesitará y los problemas se corrigen en las bibliotecas a medida que se producen. Debería usar estas bibliotecas en lugar de "reinventar la rueda". La razón principal es que probablemente no haga un buen trabajo si escribe el código personalmente, ya que hay muchos casos extremos que se deben tener en cuenta.
- **Ahorrar tiempo**: probablemente podría codificar por su cuenta la mayor parte del programa y crear bibliotecas de componentes de interfaz de usuario o utilidades. Pero la programación lleva su tiempo. Incluso si el resultado final es comparable a lo que ya hay disponible, no vale la pena perder tiempo en replicar el trabajo de escribir el código si no tiene que hacerlo.
- **Mantenimiento**: tarde o temprano, debe realizarse un mantenimiento de todas las bibliotecas y aplicaciones. El mantenimiento implica agregar nuevas características y corregir errores. ¿El mantenimiento de una biblioteca es un uso correcto del tiempo, el suyo o el del equipo? ¿O es mejor permitir que un equipo de software de código abierto lo controle?

### Evaluación de un paquete

Antes de instalar una biblioteca, es posible que le interese inspeccionar las dependencias en las que se basa. Estas dependencias pueden animarle a usar el paquete, o bien podrían disuadirle. Estos son algunos factores que tener en cuenta al seleccionar una dependencia para el proyecto:

- **Tamaño**: el número de dependencias puede crear una superficie de memoria grande. Si dispone de ancho de banda limitado o tiene otras limitaciones de hardware, el tamaño puede ser un factor importante.
- **Concesión de licencias**: la concesión de licencias puede ser un factor si diseña software con la intención de venderlo. Si tiene una licencia de una biblioteca de terceros y el creador de la biblioteca no permite que se incluya en el software destinado a la venta, podría tener problemas legales.
- **Mantenimiento activo**: si el paquete se basa en una dependencia que está en desuso o que no se ha actualizado desde hace mucho tiempo, podría ser difícil realizar el mantenimiento del programa.

Para obtener más información sobre un paquete antes de instalarlo, vaya a `https://www.npmjs.com/package/<package name>`. Esta dirección URL muestra la página de información detallada de un paquete. Seleccione la pestaña Dependencias para ver cuántos paquetes y de cuáles depende para funcionar. Otra manera de lograr un resultado similar consiste en ejecutar este comando de npm: `npm view <package name>`.

En lo que respecta a las dependencias enumeradas, es posible que el número de paquetes no resulte muy ilustrativo. Si descarga un paquete, podría acabar con una carpeta **node_modules** con miles de paquetes. ¿A qué se debe? Cada paquete tiene una lista de dependencias. Para asegurarse de poder usar un paquete, todas las dependencias se rastrean y se descargan cuando se ejecuta el comando `npm install <package name>`.

### Instalación de un paquete

Para instalar un paquete, se usa una herramienta de línea de comandos integrada denominada `npm`. Puede agregar un paquete al proyecto de Node.js mediante la invocación de un comando en el terminal.

Un comando de instalación típico es similar a este: `npm install <name of package>`.

Al ejecutar el comando `install`, la herramienta de línea de comandos se conecta a un registro global, captura el código y lo coloca en una carpeta node_modules del proyecto. Después de la instalación, el directorio del proyecto tiene un aspecto similar a este:

```text
-| node_modules/
---| <name of dependency>/
------| <files included in the dependency>
```

### Búsqueda de un paquete

Es posible que desarrolladores individuales usen el registro global en npm para buscar y descargar los paquetes que necesitan para sus aplicaciones. Es posible que una empresa aplique una estrategia para saber qué paquetes se pueden usar y dónde buscarlos. Los paquetes se podrían encontrar en muchos lugares diferentes. Algunos de estos orígenes podrían estar disponibles de manera pública. Otros podrían estar restringidos y ser accesibles solo para los empleados de una empresa concreta. Estos son algunos lugares donde podrían estar los paquetes:

- **Registros**: un ejemplo podría ser un registro global, como el registro npm. Puede hospedar registros propios, ya sean privados o públicos.
- **Repositorios**: en lugar de apuntar a un registro, se puede hacer a una dirección URL de GitHub e instalar un paquete desde allí.
- **Archivos**: puede instalar un paquete desde una carpeta local o un archivo comprimido. La instalación desde un paquete es habitual cuando se intentan desarrollar bibliotecas propias de Node.js y se quiere probar el paquete de forma local, o bien cuando no se quiere usar un registro por el motivo que sea.
- **Directorios**: también se puede instalar directamente desde un directorio.

#### Herramienta y registro npm

Al ejecutar el comando `npm install <name of dependency>`, Node.js va a un registro global, denominado registro npm, y busca el código que se va a descargar. Se encuentra en `http://npmjs.org`. Desde un explorador, también puede consultar los paquetes en esta página. El sitio contiene paquetes, que son versiones comprimidas del código fuente. Cada paquete tiene un sitio web dedicado al que puede ir. En estos sitios, puede obtener más información sobre dónde reside el código fuente y encontrar otra información, como métricas sobre las descargas y datos sobre el mantenimiento.

#### Comandos de npm

Hasta ahora, ha obtenido información sobre cómo puede instalar dependencias con la herramienta npm. Pero esta herramienta puede hacer mucho más. La herramienta de línea de comandos npm cuenta con bastantes comandos. Los comandos le ayudan con tareas como la instalación o la creación de paquetes, y la inicialización de proyectos de Node.js. No es necesario conocer todos los comandos en detalle. Cuando empiece con Node.js, es probable que ejecute solo un subconjunto de los comandos. A medida que aumente el uso de Node.js, es posible que cada vez ejecute más comandos de diversas categorías.

Para ayudarle a recordar qué hacen los comandos, puede considerarlos como pertenecientes a categorías:

- **Administración de dependencias**: hay muchos comandos relacionados con la instalación, eliminación y limpieza después de las instalaciones de paquetes. También hay comandos para actualizar paquetes.
- **Ejecución de scripts**: la herramienta npm puede ayudarle a administrar flujos en el desarrollo de la aplicación. Ejemplos de flujos de aplicación son la ejecución de pruebas, la compilación de código o la ejecución de herramientas de mejora de la calidad para realizar pruebas de lint en el código.
- **Configuración del entorno**: se pueden configurar muchos elementos, como el lugar en el que se ubican las instalaciones y el origen desde el que se instalan los paquetes. Es habitual tener una configuración que permita instalar paquetes desde el registro npm global y también de un registro específico de la empresa.
- **Creación y publicación de paquetes**: hay varios comandos que pueden ayudarle con tareas como la creación de un paquete comprimido y su inserción en un registro.

Si quiere obtener una lista detallada de todos los comandos, escriba el comando siguiente en el terminal:

```Bash
npm --help
```

### Diferencias entre dependencias de producción y de desarrollo

Las dependencias pertenecen a una de estas dos categorías:

- **Dependencias de producción**: son las que necesita ejecutar cuando la aplicación está en producción. Una dependencia de producción debe estar integrada en la aplicación para que la funcionalidad esté disponible cuando se ejecute la aplicación. Algunos ejemplos son un marco de trabajo web con el que puede compilar una aplicación web.
- **Dependencias de desarrollo**: son las que solo se necesitan al desarrollar la aplicación. Imagine que estas dependencias son los andamios de un edificio. Cuando se termine de construir el edificio, ya no los necesitará. Ejemplos de estas dependencias son las bibliotecas de prueba, las herramientas de linting o las de empaquetado. Estas dependencias son un elemento importante para garantizar que la aplicación funciona bien, pero no es necesario incluirlas en la aplicación.

Esta clasificación no es solo conceptual. La herramienta npm escribe en un archivo de manifiesto, al que agrega entradas cuando se descargan las dependencias. La herramienta permite diferenciar entre los dos tipos de dependencias mediante la adición de una marca en el comando de instalación. La marca coloca el nombre de la dependencia y su versión en una sección denominada `dependencies` o `devDependencies`. Esta diferenciación permite entender con claridad las dependencias que tiene la aplicación y de qué tipo son. Sea cual sea el tipo de dependencia que instale, se almacena en el directorio node_modules. La marca solo afecta al archivo de manifiesto.

Además, esta separación de los diferentes tipos de dependencias está integrada en la herramienta de línea de comandos npm. Si especifica la marca `--production` al instalar una dependencia, solo se instalarán dependencies de producción. Por ejemplo, las canalizaciones de integración e implementación continuas (CI/CD) usan esta marca a fin de asegurarse de que solo se instalan las dependencias necesarias para ejecutar la aplicación.

#### Procedimiento para instalar un paquete

El comando `npm install <dependency name>` permite instalar una dependencia normal diseñada para usarse como parte de la aplicación. Las dependencias de desarrollador no están pensadas para incluirse en producción. Para instalar una dependencia de desarrollador, agregue la marca `--save-dev`.

También hay paquetes que se pueden instalar *globalmente*. Estos paquetes no suelen estar pensados para importarse en el proyecto. Por este motivo, muchos paquetes globales son herramientas de la CLI. Algunos paquetes, como `http-server`, permiten que se puedan importar.

Si instala algo globalmente, no se instala en la carpeta node_modules del proyecto. En su lugar, se instala en un directorio específico del equipo, por lo que está disponible para todos los proyectos de Node.js en el equipo. Para instalar un paquete global, agregue la marca `-g` a `install command`, de modo que el comando tenga este aspecto: `npm install <dependency name> -g`.

La herramienta npx se ha creado en parte para abordar el hecho de que las dependencias instaladas globalmente ocupan espacio cuando se instalan muchas con el paso del tiempo. Además, en muchos sistemas operativos, una herramienta instalada de forma global necesita permisos elevados para la instalación. La cuestión principal es que las dependencias globales realizan tareas que probablemente no quiera que se lleven a cabo frecuentemente. Por tanto, no es necesario que estas herramientas estén en el equipo más tiempo de lo normal.

La herramienta npx permite cargar la dependencia en el proceso de Node.js y ejecutar el comando desde allí. Después de ejecutar el comando, la dependencia se limpia y se quita del sistema. La herramienta npx se ha incluido en todas las versiones principales de npm desde la versión 5.2. Es la manera preferida de usar las dependencias que están diseñadas para ejecutarse con poca frecuencia. Para usar la herramienta npx, escriba `npx <name of package>`. Capturará la dependencia, ejecutará el comando y realizará la limpieza.

#### Después de la instalación

Los paquetes enumerados en la sección `dependencies` del archivo package.json serán distintos de los que aparecen en la carpeta node_modules. Si quiere ver qué paquetes hay en la carpeta, puede escribir el comando `npm list`. Pero es posible que este comando genere una lista larga. Puede resultar complicado hacerse una idea de lo que tiene en la carpeta. Para solucionar este problema, puede enumerar los paquetes con diferentes profundidades. Al hacerlo, el comando `list` tiene el aspecto siguiente:

```Bash
npm list --depth=<depth>
```

En la profundidad `0`, el comando muestra el mismo contenido que tiene en la sección `dependencies` del archivo package.json. Si ha instalado el paquete Jest, `npm list --depth=0` producirá una salida similar a la siguiente:

```text
├── jest@26.0.1
```

El comando `npm list` muestra todos los paquetes del directorio node_modules que se han instalado. Cada paquete instalado también tiene instaladas todas las dependencias en las que se basa.

La profundidad `1` permite ver un nivel inferior en la instalación, con los paquetes instalados y las dependencias principales de las que constan. Si ejecuta el comando npm list --depth=1, es posible que vea este resultado:

```text
├─┬ jest@26.0.1
│ ├── @jest/core@26.0.1
│ ├── import-local@3.0.2
│ └── jest-cli@26.0.1
```

A medida que aumenta el número de `depth`, verá más paquetes dependientes.

### Limpieza de las dependencias

Tarde o temprano es probable que se dé cuenta de que ya no necesita un paquete. O bien, es posible que el paquete que ha instalado no sea el que necesita. Puede que haya encontrado uno que realizará mejor una tarea. Sea cual sea el motivo, debe quitar las dependencias que no use. Al hacerlo, todo se mantiene limpio. Además, las dependencias ocupan espacio.

El espacio es más importante si va a compilar una aplicación de página única con un marco como Angular, React o Vue. Estas aplicaciones también son proyectos de Node.js. La creación de estas aplicaciones implica un proceso de *agrupación* y *minificación* en el que se concatenan distintos módulos de Node.js y se comprime el código fuente. Finalmente, las agrupaciones resultantes se sirven desde un explorador. Cuanto mayor sea la agrupación, más tiempo se tarda en enviar a través de la red, antes de convertirse en algo con el que los clientes puedan interactuar. Cuanto más tiempo tengan que esperar los clientes, mayor es la probabilidad de que eviten usar la aplicación.

Hay dos maneras de limpiar las dependencias que ya no se necesitan:

- **Desinstalar**: para desinstalar un paquete, ejecute `npm uninstall <name of dependency>`. Este comando quita el paquete del archivo de manifiesto y de la carpeta node_modules.
- **Eliminar**: también puede ejecutar el comando `npm prune`. Este comando quita todas las dependencias de la carpeta node_modules que no aparecen como dependencias en el archivo de manifiesto. Este comando es una opción correcta si quiere quitar más de una dependencia y no ejecutar el comando `uninstall` en cada una. Para usar este comando con el fin de quitar las dependencias sin usar, quite primero las entradas de la sección `dependencies` o `devDependencies` del archivo de manifiesto y, luego, ejecute el comando `npm prune`.

<hr/>

## Ejercicio: Instalación de paquetes

Los desarrolladores de Tailwind Traders asumen que están a punto de invertir una gran cantidad de recursos en el desarrollo de aplicaciones para la plataforma Node.js. Estas aplicaciones van a necesitar pruebas. Tras un análisis, comprueban que las funciones de prueba de Node.js son limitadas. Por tanto, deben usar un marco. Han encontrado Jest en el registro npm. Parece que se usa mucho y promete ser un marco de pruebas que no requiere configuración. En este punto, solo le piden que instale Jest, que escriba un par de pruebas y las ejecute para ver si el marco está a la altura de las expectativas.

### Adición de un paquete de pruebas mediante la herramienta npm

Le han proporcionado un código que analiza una dirección de una cadena. La tarea debe ser bastante sencilla. Implica la instalación del marco de pruebas, la escritura de un par de pruebas y su ejecución.

1. En la ventana del terminal, cambie a la carpeta que tiene los archivos clonados para este ejercicio:

```Bash
cd node-essentials/node-dependencies/5-exercise-dependency
```

En esta carpeta, debería ver dos archivos JavaScript:

```text
-| address-parser.js
-| package.json
```

2. Abra el archivo *address-parser.js*. Debería ser similar a este archivo:

```JavaScript
exports.parse = function parseOrder(order) {
  const match = order.match(/order:\s(?<order>\w+\s\w+).*address:\s(?<address>\w+\s\w+\s\w+).*payment info:\s(?<payment>\w+)/)
return match.groups;
}
```

Esta función toma una cadena y analiza la información relativa al pedido que realiza un cliente, el lugar donde debe entregarse y el método de pago. Ahora se agregará Jest y se escribirán algunas pruebas para la función.

Cierre el archivo address-parser.js.

3. Ejecute este comando para instalar la biblioteca de Jest:

```Bash
npm install jest --save-dev
```

4. Después de instalar la biblioteca de Jest, abra el archivo package.json y busque la sección `devDependencies`. Ahora debería ver una entrada similar a la de este ejemplo:

```JSON
"devDependencies": {
   "jest": "^29.5.0"
}
```

Observe que la biblioteca de Jest tiene una dependencia de desarrollo identificada.

5. En el archivo package.json, busque la sección `scripts`. Reemplace la entrada de la acción test por el código siguiente:

```JSON
"test": "jest"
```

6. En el terminal, cree una carpeta denominada **__tests__**.

7. En la carpeta **__tests__**, cree un archivo denominado address-parser.js y agregue el contenido siguiente al archivo:

```JavaScript
const { parse } = require("../address-parser");

describe('Address parser', () => {
  test('should parse correctly', () => {
    expect(
      parse(
         "I want to to order: 3 books to address: 112 street city here is my payment info: cardnumber"
      )
      ).toEqual({
        order: "3 books",
        address: "112 street city",
        payment: "cardnumber",
      });
  })
})
```

Guarde los cambios y cierre el archivo.

La estructura del proyecto debería tener un aspecto similar al de este ejemplo:

```text
-| package.json
-| address-parser.js
-| __tests__/
---| address-parser.js
```

El script de prueba comprueba la capacidad de análisis de la función address-parser.js. La prueba garantiza que la función pueda analizar correctamente la información necesaria.

8. Escriba este comando en el terminal para ejecutar las pruebas:

```Bash
npm run test
```

Debería obtener la salida siguiente:

```text
Address parser
 ✓ should parse correctly (2 ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        1.216 s
Ran all test suites.
```

<hr/>

## Administración de actualizaciones de dependencias en el proyecto de Node.js

Tarde o temprano, querrá actualizar a una nueva versión de una biblioteca. Es posible que una función se haya marcado como en desuso. O bien que haya una nueva característica en una versión posterior de un paquete que usa.

Hay algunas consideraciones que se deben tener en cuenta antes de intentar actualizar una biblioteca:

- **El tipo de actualización**. ¿Qué tipo de actualización está disponible? ¿Se trata de una pequeña corrección de errores? ¿Agrega una característica nueva que necesita? ¿Interrumpirá el código? Puede comunicar el tipo de actualización mediante un sistema denominado *versionamiento semántico*. La manera en que se expresa el número de versión de la biblioteca indica al desarrollador el tipo de actualización con el que trata.
- **Si el proyecto está configurado correctamente**. Puede configurar el proyecto de Node.js para que obtenga solo los tipos de actualizaciones que quiere. Solo se realizará una actualización si hay disponible un tipo de actualización específico. Este es el enfoque recomendado para evitar sorpresas.
- **Problemas de seguridad**. La administración de las dependencias del proyecto en el tiempo implica tener en cuenta los problemas que se pueden producir. Pueden surgir problemas cuando se detectan vulnerabilidades. Si todo va bien, se publicarán revisiones que se pueden descargar. La herramienta npm le ayuda a ejecutar una *auditoría* en las bibliotecas para averiguar si tiene paquetes que se deben actualizar. También le ayuda a realizar las acciones adecuadas para solucionar un problema.

### Uso del versionamiento semántico

Existe un estándar del sector denominado *versionamiento semántico*. Se trata de un sistema que adoptan numerosas empresas y desarrolladores. Si tiene previsto publicar paquetes e insertarlos en el registro npm, debe seguir el versionamiento semántico. Es lo que se espera. Incluso si solo descarga paquetes del registro npm, lo más probable es que sigan el versionamiento semántico.

¿Por qué es tan importante? Los cambios en un paquete pueden introducir riesgos. Riesgo de que se pueda introducir un error que podría dañar a la empresa. Riesgo de que tenga que volver a escribir parte del código. La reescritura de código lleva tiempo y cuesta dinero.

El versionamiento semántico es la forma de expresar el tipo de cambio que usted u otro desarrollador introducen en una biblioteca. El versionamiento semántico garantiza que un paquete tenga un número de versión dividido en estas secciones:

- **Versión principal**: número situado más a la izquierda. Por ejemplo, el 1 en 1.0.0. Un cambio en este número significa que puede esperar cambios importantes en el código. Es posible que tenga que volver a escribir parte del código.
- **Versión secundaria**: número del medio. Por ejemplo, el 2 en 1.2.0. Un cambio en este número significa que se han agregado características. El código debería seguir funcionando. Por lo general, es seguro aceptar la actualización.
- **Versión de revisión**: número más a la derecha. Por ejemplo, el 3 en 1.2.3. Un cambio en este número significa que se ha aplicado un cambio que corrige una parte del código que debería haber funcionado. Debería ser seguro aceptar la actualización.

En esta tabla se muestra cómo cambia el número de versión de cada tipo de versión:

| Tipo                | ¿Qué sucede?         |
| ------------------- | -------------------- |
| Versión principal   | 1.0.0 cambia a 2.0.0 |
| Versión secundaria  | 1.2.9 cambia a 1.3.0 |
| Versíon de revisión | 1.0.7 cambia a 1.0.8 |

### Actualización de un paquete mediante npm

Hay dos maneras de instalar el paquete. Puede ejecutar el comando `install` o el comando `update`. Antes, había diferencias entre los dos comandos, pero ahora funcionan más bien como alias. Un comando típico para actualizar un paquete podría ser `npm update <name of package>@<optional argument with version number>`.

La acción específica del proceso de actualización depende de dos condiciones:

- **Si el argumento de versión se especifica como parte del comando.** Si se especifica un argumento de versión (el último argumento) en el comando `npm update`, se captura e instala la versión del paquete especificada.
- **La entrada en el archivo de manifiesto**. La entrada en el archivo de manifiesto incluye el nombre de la dependencia y un valor que expresa un patrón de reglas sobre cómo se actualiza la dependencia. Por ejemplo: `"<name of dependency>": "1.1.x"`. La herramienta npm respeta el patrón de regla e intenta capturar la versión de la dependencia que coincide con ese patrón.

#### Enfoque de actualización

Como desarrollador de Node.js, puede comunicarle a Node.js el comportamiento de actualización que quiere. Piense en la actualización en términos del riesgo. Estos son algunos enfoques:

- **Versión principal**: estoy de acuerdo con la actualización a la versión principal más reciente en cuanto se publique. Acepto el hecho de que es posible que tenga que cambiar el código.
- **Versión secundaria:** me parece bien que se incorpore una característica nueva. No me gusta que el código se interrumpa.
- **Versión de revisión**: las únicas actualizaciones que me interesan son las correcciones de errores.

Si va a administrar un proyecto de Node.js nuevo o más pequeño, puede permitirse cierta flexibilidad en el modo de definir la estrategia de actualización. De forma predeterminada, puede optar por actualizar siempre a la versión más reciente. Pero si se trata de proyectos complejos, existen más matices que debe tener en cuenta. En ese caso, es posible que no realice la actualización de forma predeterminada. Por lo general, cuanto menor sea la dependencia que se va a actualizar, menos dependencias tendrá y más probabilidades de que el proceso de actualización sea fácil.

#### Configuración del archivo package.json para la actualización

Antes de actualizar una o más dependencias, debe configurar el archivo de manifiesto para obtener un comportamiento predecible al ejecutar el comando npm update `<name of dependency>`. Puede comunicar el enfoque que quiere adoptar para un paquete. Node.js dispone de un conjunto de símbolos que le permiten definir cómo quiere que se actualicen los paquetes.

El proceso consiste en agregar prefijos diferentes a las entradas del paquete en el archivo package.json. Se pueden configurar muchos elementos, además de la versión principal, la secundaria y la de revisión. También puede expresar que solo quiere los paquetes que se encuentren dentro de un intervalo determinado, o bien los que tengan una etiqueta concreta, como `alpha` o `beta`, por ejemplo.

Estos son algunos de los patrones que puede configurar para la versión principal, secundaria o de revisión:

| Patrón                         | Nivel de actualización                              |
| ------------------------------ | --------------------------------------------------- |
| x.0.0 o * (asterisco)          | Actualización a la *versión principal* más reciente |
| 1.x.1 o ^ (acento circunflejo) | Actualización solo a la *versión secundaria*        |
| 1.1.x o ~ (tilde)              | Actualización a la versión de revisión más reciente |

### package-lock.json

Además del archivo de manifiesto package.json, también tiene el archivo package-lock.json. Este archivo se genera cuando se realiza una acción que modifica el directorio node_modules o que cambie las dependencias en el archivo package-lock.json. Este archivo no se crea al ejecutar el comando `npm init`. El archivo se crea al instalar un paquete, por ejemplo.

El archivo package-lock.json debe confirmarse en el repositorio.

Una razón para confirmar este archivo en el repositorio es que garantiza instalaciones exactas. Recordará que en el archivo package.json define patrones para los tipos de instalaciones que quiere, como revisiones, versiones secundarias o versiones principales. Los patrones no son exactos. Si usa el patrón `1.x`, no sabrá si instaló la versión 1.4 o 1.5.

Es posible que necesite saber qué versión ha instalado. Imagine que especifica el patrón `1.x`, usa la versión 1.2 en el código y, luego, se publica la versión 1.4. La nueva versión termina interrumpiendo el código. Un usuario que instale la aplicación se encontrará con que no funciona. Si hay un archivo package-lock.json que indica que se usa la versión 1.2, se instalará la versión 1.2. Por tanto, ¿a quién le interesa este comportamiento? A quienes usen la aplicación y las herramientas de integración continua (CI).

Es importante comprender el proceso y, además, qué archivo determina cuándo se produce una instalación. Funciona de esta manera:

- Si los archivos package.json y package-lock.json coinciden en un nivel de regla semántica, no hay ningún conflicto. Si el patrón indica `1.x` en el archivo package.json y en el archivo package-lock.json se especifica que se ha instalado la versión 1.4, se instalará la versión 1.4.
- Si en el archivo package.json se especifica un patrón como `1.8.x`, no se implementarán las instrucciones del archivo package-lock.json. Se instalará la versión secundaria 1.8.0 o posterior, o bien una versión de revisión posterior, si existe.

El archivo package-lock.json también proporciona otras características. Facilita ver lo que ha cambiado entre confirmaciones y ayuda a optimizar el proceso de instalación.

### Búsqueda y actualización de paquetes obsoletos

El comando `npm outdated` muestra los paquetes obsoletos. Este comando puede ayudarle a identificar cuándo hay disponibles versiones más recientes de los paquetes. Esta es una salida típica del comando:

```text
Package       Current    Wanted   Latest     Location     Depended by
lodash        1.0.0      1.0.0    4.17.19    lock-test    main-code-file
node-fetch    1.2.0      1.2.0    2.6.0      lock-test    function-code-file
```

Las columnas de la salida incluyen lo siguiente:

- **Paquete**: el paquete obsoleto.
- **Actual**: la versión instalada actual del paquete.
- **Deseado**: la versión más reciente que coincide con el patrón semántico especificado en el archivo package.json.
- **Último**: la última versión del paquete.
- **Ubicación**: la ubicación de la dependencia del paquete. El comando outdated rastrea todos los paquetes instalados en las diversas carpetas node_modules.
- **Dependiente**: el paquete que tiene la dependencia.

El flujo de trabajo recomendado es ejecutar esos comandos en este orden:

1. Ejecute el comando `npm outdated` para enumerar todos los paquetes obsoletos. Este comando proporciona información en las columnas Deseado, Último y Ubicación.
2. Ejecute el comando `npm update <optional package name>` para actualizar los paquetes instalados. Si ejecuta este comando con un nombre de paquete especificado, el comando intenta actualizar solo el paquete especificado. Si no especifica un paquete, el comando intenta actualizar todos los paquetes del archivo package.json.

### Administración de incidencias de seguridad

Cada vez que actualice o instale un paquete, obtendrá una respuesta de registro después de que se complete la instalación. La respuesta indica qué versión se ha instalado y si hay alguna vulnerabilidad. Un registro podría ser similar al de este ejemplo:

```text
+ lodash@1.3.1
added 1 package from 4 contributors and audited 1 package in 0.949s
found 3 vulnerabilities (1 low, 2 high)
  run `npm audit fix` to fix them, or `npm audit` for details
```

En el registro se enumeran las vulnerabilidades con niveles de gravedad alto y bajo. Si se detecta alguna vulnerabilidad de nivel alto, debería actualizar el paquete. Para corregir un problema y aplicar una actualización, puede ejecutar el comando `npm audit`, como se indica en la respuesta del registro. Este comando enumera cada vulnerabilidad. Una respuesta de `npm audit` podría ser similar a la de este ejemplo:

```text
# Run  npm install lodash@4.17.15  to resolve 3 vulnerabilities

| Low            | Prototype Pollution               |
|----------------|-----------------------------------|
| Package        | lodash                            |
| Dependency of  | lodash                            |
| Path           | lodash                            |
| More info      | https://npmjs.com/advisories/577  |  

and so on..
```

El comando `npm audit fix` intenta corregir el problema. Intenta actualizar a una versión secundaria en la que no exista el problema. Esta acción podría no ser suficiente. Es posible que se le pida que ejecute el comando `npm audit fix --force` para corregir el problema. Esta acción implica un cambio importante. Es decir, se actualizará la versión principal del paquete.

Una *vulnerabilidad* es un punto débil en el código que los atacantes pueden aprovechar para cometer acciones malintencionadas. Los atacantes pueden usar estos puntos débiles para obtener acceso a los datos y a los sistemas. Debe tomarse las vulnerabilidades en serio.

En todo momento se detectan vulnerabilidades. Son tan comunes que GitHub ha implementado una función que examina repositorios y crea solicitudes de incorporación de cambios de forma automática en las que se sugiere que actualice a una versión más segura debido a una vulnerabilidad. Debería ejecutar el comando de auditoría npm audit ocasionalmente. La seguridad es trabajo de todos los usuarios. Un gran proveedor de repositorios como GitHub hace su parte. Puede hacer la suya mediante la auditoría y corrección de vulnerabilidades si encuentra alguna.

<hr/>

## Ejercicio: Administración de actualizaciones de dependencias en el proyecto de Node.js

Tailwind Traders le ha asignado la tarea de trabajar en una aplicación que tiene algunas dependencias no actualizadas. La aplicación es pequeña y solo tiene algunas dependencias. La actualización del código debería ser sencilla. Vea si puede actualizar la aplicación para aprovechar las características más recientes. Durante el proceso, si encuentra alguna vulnerabilidad, corríjala.

### Actualización de las dependencias de la aplicación

1. En la ventana del terminal, cambie a la carpeta que tiene los archivos clonados para este ejercicio:

```Bash
cd node-essentials/node-dependencies/7-exercise-dependency-management
```

2. Ejecute este comando para instalar las dependencias:

```Bash
Ejecute este comando para instalar las dependencias:
```

```text
added 5 packages, and audited 6 packages in 3s

found 0 vulnerabilities
```

> ℹ️ **Importante**  <br/>
> Si el resultado muestra una vulnerabilidad crítica, ejecute el comando `npm audit fix --force` para resolver las vulnerabilidades.

3. Abra el archivo index.js:

```JavaScript
const fetch = require('node-fetch')
const _ = require('lodash');
const path = require('path');
const fs = require('fs');

async function run() {
  const response = await fetch("https://dev.to/api/articles?state=rising");
  const json = await response.json();
  const sorted = _.sortBy(json, ["public_reactions_count"], ['desc']);
  const top3 = _.take(sorted, 3);

  const filePrefix = new Date().toISOString().split('T')[0];
  fs.writeFileSync(path.join(__dirname, `${filePrefix}-feed.json`), JSON.stringify(top3, null, 2));
 }

run();
```

Este código extrae datos de una API REST mediante la biblioteca `node-fetch`. Se ordena la respuesta para procesarla y se toman los tres primeros resultados mediante la biblioteca lodash. El resultado se almacena en un archivo.

4. Abra el archivo package.json y busque la sección `dependencies`:

```JSON
"lodash": "^1.1.0",
"node-fetch": "^1.0.2"
```

Observe que los patrones especifican el carácter de inserción (^), que indica actualizaciones de la versión secundaria para admitir dependencias.

5. En el terminal, ejecute este comando para comprobar si hay dependencias obsoletas:

```Bash
npm outdated
```

Debería mostrarse una salida similar a esta de ejemplo:

```text
Package       Current    Wanted    Latest    Location                   Depended by
lodash        1.1.0      1.3.1     4.17.21   node_modules/lodash        7-exercise-dependency-management
node-fetch    1.0.2      1.7.3     3.2.3     node_modules/node-fetch    7-exercise-dependency-management
```

6. Con cierto nivel de confianza, puede actualizar los paquetes obsoletos a la versión **deseada**. Este nivel de actualización garantiza que las dependencias obtengan las características y revisiones más recientes en la versión secundaria. Ejecute el siguiente comando para realizar la actualización:

```Bash
npm update
```

Debería mostrarse una salida similar a esta de ejemplo:

```text
Updating lodash to 1.3.1, which is a SemVer minor change.
Updating node-fetch to 1.7.3, which is a SemVer minor change.

changed 2 packages, and audited 6 packages in 6s

found 0 vulnerabilities
```

En la salida se indica que las dependencias del proyecto se han actualizado.

Hasta ahora, ha actualizado las dependencias en la medida en que los patrones de package.json se lo permiten.

Ahora puede instalar la versión más reciente ejecutando el comando npm install `<name of package>@<known latest version>` o usando la palabra clave latest: `npm install <name of package>@latest`.

7. En el terminal, ejecute este comando:

```Bash
npm install node-fetch@latest lodash@latest
```

La salida se parecerá a este ejemplo:


```text
Updating lodash to 4.17.21, which is a SemVer major change.
Updating node-fetch to 3.2.3, which is a SemVer major change.

added 5 packages, removed 3 packages, changed 2 packages, and audited 8 packages in 10s

found 0 vulnerabilities
```

Es posible que los resultados sean ligeramente distintos. Las versiones indicadas se deben corresponder a las versiones disponibles más recientes de los paquetes.