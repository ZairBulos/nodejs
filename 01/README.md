# Introducción a Node.js

Obtenga información sobre Node.js: qué es, cómo funciona y cuándo usarlo.

### Requisitos previos

* Conocimientos básicos de edición de texto y archivos de código en cualquier editor de texto
* Experiencia de nivel principiante en la escritura de código de JavaScript
* Experiencia con el uso de la línea de comandos

### Objetivos de aprendizaje

* Explicar qué es Node.js
* Describir cómo funciona Node.js
* Identificar cuándo usar Node.js
* Crear y ejecutar un script de Node.js desde la línea de comandos

<hr/>

## Introducción

Ha sido contratado por Tailwind Traders, una gran empresa comercializadora que maneja muchos datos y archivos y tiene muchos problemas que resolver. La empresa realiza muchas operaciones mediante Node.js, pero usted no sabe cómo usar este entorno de tiempo de ejecución de JavaScript.

En este módulo va a aprender qué es Node.js y cómo puede usarlo para ejecutar aplicaciones y código de JavaScript fuera de un explorador. Al final de este módulo podrá decidir si Node.js es la herramienta adecuada para el proyecto de Tailwind Traders.

<hr/>

## ¿Qué es Node.js?

Node.js, o Node para abreviar, es un entorno de ejecución de JavaScript del lado servidor de código abierto. Puede usar Node.js para ejecutar aplicaciones y código de JavaScript en muchas ubicaciones fuera de un explorador, como un servidor.

Node.js es un contenedor en torno a un motor de JavaScript denominado [V8](https://nodejs.dev/en/learn/the-v8-javascript-engine/) que alimenta muchos exploradores, como Google Chrome, Opera y Microsoft Edge. Puede usar Node.js para ejecutar JavaScript mediante el motor V8 fuera de un explorador. Node.js también contiene muchas optimizaciones de V8 que podrían ser necesarias para las aplicaciones que se ejecutan en un servidor. Por ejemplo, Node.js agrega una clase **Buffer** que permite a V8 trabajar con archivos. Esta característica convierte a Node.js en una buena opción para compilar algo como un servidor web.

Aunque nunca haya usado JavaScript como lenguaje de programación principal, podría ser la opción adecuada para escribir aplicaciones sólidas y modulares. JavaScript también ofrece algunas ventajas exclusivas. Por ejemplo, como los exploradores usan JavaScript, puede emplear Node.js para compartir lógica como reglas de validación de formularios entre el explorador y el servidor.

JavaScript se ha vuelto más relevante con el auge de las aplicaciones de página única y admite el formato de intercambio de datos de notación de objetos JavaScript (JSON) de uso generalizado. Muchas tecnologías de base de datos NoSQL como CouchDB y MongoDB también usan JavaScript y JSON como formato para consultas y esquemas. Node.js usa idénticos lenguaje y tecnologías que muchos servicios y marcos modernos.

Con Node.js puede compilar los siguientes tipos de aplicaciones:

- Servidores web HTTP
- Microservicios o back-ends de API sin servidor
- Controladores para acceso a bases de datos y consulta
- Interfaces de línea de comandos interactivas
- Aplicaciones de escritorio
- Bibliotecas de cliente y servidor de Internet de las cosas (IoT) en tiempo real
- Complementos para aplicaciones de escritorio
- Scripts de shell para manipulación de archivos o acceso a la red
- Modelos y bibliotecas de aprendizaje automático

![npm](/images/01-npm.png)

El entorno de Node.js también ofrece un registro npm que puede usar para compartir una biblioteca de Node.js propia.

Node.js es rápido, ofrece un alto rendimiento y puede controlar aplicaciones en tiempo real y flujos de datos intensivos. Un caso de uso de ejemplo podría ser la compilación de un dispositivo que puede enviar comandos de control al escritorio permanente. Puede instalar Node.js en el panel de IoT o usar un dispositivo con Node.js preinstalado. Luego se escribiría la lógica de aplicación en JavaScript y se implementaría en el dispositivo.

![iot-example](/images/01-iot-example.jpeg)

<hr/>

## Funcionamiento de Node.js

Como ha visto en la unidad anterior, el entorno de ejecución de JavaScript Node.js es un contenedor en torno a un motor de JavaScript denominado V8. Dado que Node.js puede interpretar y ejecutar código de JavaScript en un equipo host fuera de un explorador, el entorno de ejecución tiene acceso directo a la E/S del sistema operativo, al sistema de archivos y a la red. En esta unidad se explica cómo controla Node.js las tareas entrantes.

Node.js se basa en un bucle de eventos de un solo subproceso. Este modelo de arquitectura controla de forma eficaz las operaciones simultáneas. *Simultaneidad* significa la capacidad del bucle de eventos de realizar funciones de devolución de llamada de JavaScript una vez completado otro trabajo.

En este modelo de arquitectura:

* Un *solo subproceso* significa que JavaScript solo tiene una pila de llamadas y únicamente puede realizar una operación a la vez.
* El *bucle de eventos* ejecuta el código, recopila y procesa eventos y ejecuta las subtareas siguientes de la cola de eventos.

Un *subproceso* en este contexto es una única secuencia de instrucciones programadas que el sistema operativo puede administrar de forma independiente.

En Node.js, operaciones de E/S como la lectura o escritura en un archivo en el disco o la realización de una llamada de red a un servidor remoto se consideran operaciones de bloqueo. Una *operación de bloqueo* bloquea todas las tareas subsiguientes hasta que finaliza la operación, momento en que continúa con la siguiente operación. En un modelo *sin bloqueo*, el bucle de eventos puede ejecutar varias operaciones de E/S al mismo tiempo.

El nombre *bucle de eventos* describe el uso del mecanismo de "ocupación en espera", que espera de forma sincrónica a que llegue un mensaje y lo procesa. A continuación se muestra la implementación de un bucle de eventos:

```JavaScript
while (queue.wait()) {
  queue.process();
}
```

### Arquitectura de Node.js

Node.js usa una arquitectura controlada por eventos en la que un bucle de eventos controla la orquestación y un grupo de trabajo bloquea las tareas. El bucle de eventos permite a Node.js controlar las operaciones simultáneas. En el siguiente diagrama se muestra cómo funciona un bucle de eventos, en general:

![event-loop](/images/01-event-loop.svg)

Las fases principales de un bucle de eventos son:

- Los **temporizadores**: procesan las devoluciones de llamada programadas por `setTimeout()` y `setInterval()`.
- Las **devoluciones de llamada** ejecutan devoluciones de llamada pendientes.
- El **sondeo** recupera eventos de E/S entrantes y ejecuta devoluciones de llamada relacionadas con la E/S.
- La **comprobación** permite que las devoluciones de llamada se ejecuten inmediatamente después de que se haya completado la fase de sondeo.
- El **cierre de devoluciones de llamada** cierra eventos (por ejemplo, `socket.destroy()`) y devoluciones de llamada (por ejemplo, `socket.on('close', ...)`).

Node.js usa el grupo de trabajo para controlar tareas de bloqueo, como el bloqueo de operaciones de E/S y tareas con uso intensivo de CPU.

En resumen, el bucle de eventos ejecuta las devoluciones de llamada de JavaScript registradas para los eventos y también es responsable de satisfacer solicitudes asincrónicas sin bloqueo como la E/S de red.

#### Rendimiento

JavaScript puede generar los mismos resultados de rendimiento que lenguajes de bajo nivel como **C** debido a las mejoras de rendimiento que posibilita el motor V8. Node.js también aprovecha la naturaleza única controlada por eventos de JavaScript que hace que la creación de tareas del servidor sea rápida y de alto rendimiento.

#### Programación asincrónica

Para admitir el eficaz modelo de programación controlado por eventos, Node.js tiene un conjunto integrado de API de E/S sin bloqueo para controlar tareas comunes como la manipulación del sistema de archivos o de bases de datos. La biblioteca libuv proporciona estas API. Al realizar una solicitud de Node.js para leer el contenido de un archivo desde un disco, Node.js no se bloquea a la espera de que el disco y los descriptores del archivo estén listos, sino que la interfaz de E/S sin bloqueo avisa a Node.js cuando el archivo está listo. Esta E/S sin bloqueo funciona de la misma manera cuando el explorador notifica al código que se ha desencadenado un evento de mouse o teclado o cuando se ha recibido una respuesta XMLHttpRequest (XHR) desde un punto de conexión remoto.

![architecture](/images/01-architecture.svg)

### Instalación y uso de Node.js

Hay muchas formas de instalar Node.js. Estas son algunas de las opciones más comunes:

- **Instalación mediante un archivo ejecutable:** la página Descargas de Node.js en `https://nodejs.org/en/download/` proporciona paquetes de instalación para distintos sistemas operativos.
- **Instalación mediante brew**: [Homebrew](https://brew.sh/), o brew, es un conocido administrador de paquetes para Linux y macOS.
- **Instalación mediante NVM**: [Node Version Manager (nvm)](https://nodejs.org/en/download/package-manager/#nvm?azure-portal=true) no solo ayuda a instalar la versión de Node.js deseada, sino también a administrar la instalación. La opción nvm no se describe en esta sección.

Vamos a echar un vistazo más detallado a los pasos necesarios para descargar e instalar Node.js y comprobar la corrección de la instalación.

#### Instalación mediante un archivo ejecutable

Este es un fragmento de la página de instalación que se encuentra en la ubicación de descarga `https://nodejs.org/en/download/`:

![install-page](/images/01-install-page.png)

Observe los distintos instaladores disponibles para diferentes sistemas operativos como Windows, macOS y Linux. También puede descargar dos versiones de código fuente diferentes:

- **LTS** son las siglas (en inglés) de "compatibilidad a largo plazo" y se describe como "Recomendado para la mayoría de los usuarios". LTS se ha diseñado para el uso empresarial en el que es posible que las actualizaciones frecuentes no sean posibles ni deseadas.
- **Actual** significa código fuente que está en desarrollo activo. Es posible que se produzcan adiciones de características y cambios importantes. El código se debe adherir al control de versiones semántico.

Base la elección de versión en los requisitos de la empresa. Por ejemplo, si actualiza con frecuencia, la versión Actual puede ser la adecuada.

Para obtener más información sobre los distintos tipos de versión, vea [Tipos de versión](https://github.com/nodejs/node#release-types?azure-portal=true).

#### Comprobación de la instalación

Una vez finalizada la instalación de Node.js, ejecute el siguiente comando en el terminal para comprobar que la instalación se ha realizado correctamente:

```Bash
node --version
```

El comando debe mostrar la versión actual en el formato siguiente:

```Bash
v[major version].[minor version].[patch version]
```

Los corchetes `[]` de este ejemplo indican que los resultados pueden variar en función de la versión instalada en el sistema.

<hr/>

## ¿Por qué podría necesitar Node.js?

Node.js es una de las tecnologías de uso más generalizado en compañías, startups y organizaciones gubernamentales. Entre ellas se incluyen importantes empresas como Netflix, Trello, Walmart, Uber, eBay y NASA.

Node.js es un entorno de ejecución de JavaScript que puede ejecutar aplicaciones y código de JavaScript en un servidor en concreto, y fuera de un explorador en general. Node.js es un entorno de tiempo de ejecución sin bloqueo de un solo subproceso que se basa en el paradigma de E/S controlado por eventos. *Sin bloqueo* significa que, por ejemplo, si un cliente remoto realiza una solicitud, un servidor escrito en JavaScript y que se ejecuta en Node.js controla la solicitud, elabora y devuelve una respuesta y luego pasa a la solicitud siguiente, sin bloquear ni esperar a que finalicen otras tareas.

En esta unidad se describen las ventajas principales del uso de Node.js y cuándo usarlo.

### Tecnología multiuso

Puede usar Node.js para compilar una amplia variedad de aplicaciones listas para producción. Estas aplicaciones abarcan desde programas de chat tradicionales ligeros con tráfico elevado a herramientas de línea de comandos y servidores web. Node.js se ha diseñado desde cero para controlar un gran número de solicitudes simultáneas.

### JavaScript

*"Toda aplicación que se pueda escribir en JavaScript, se escribirá finalmente en JavaScript"*. – Jeff Atwood, escritor, empresario, cofundador de StackOverflow.

Hoy en día, muchas aplicaciones escritas fuera del explorador están en JavaScript o admiten este lenguaje como ciudadano de primera clase, incluidas:

- Editores de código como Visual Studio Code y Atom, que están escritos en JavaScript o TypeScript (un superconjunto de JavaScript con tipos estáticos). Estos editores ejecutan una versión insertada del entorno de ejecución Node.js.
- Muchas aplicaciones de Internet de las cosas (IoT) y en tiempo real que están escritas en JavaScript y dependen de Node.js para ejecutarse, ya sea en el servidor o a través de microcontroladores y plataformas SoC (Sistema en un chip), como Puck.js o Tessel.
- Tecnologías como NativeScript que pueden usar JavaScript o TypeScript para compilar aplicaciones para dispositivos móviles nativas de alto rendimiento.
- Muchas aplicaciones que usan JavaScript para su sistema de complementos, como Sketch, Adobe XD y Google Apps Script.

### Comunidad

La comunidad ya ha compilado más de un millón de módulos y bibliotecas para Node.js y las ha publicado en el administrador de paquetes de Node (npm). Los desarrolladores pueden descargar e integrar fácilmente esos módulos en los proyectos existentes. Las aplicaciones que se pueden ejecutar en Node.js incluyen herramientas de línea de comandos, marcos, servidores web, etc.

### Código abierto

Node.js es una tecnología de código abierto admitida por OpenJS Foundation. Cuenta con una numerosa y activa comunidad de código abierto y colaboradores que trabajan sin descanso para mejorar y optimizar la tecnología. Un comité de la comunidad de alto nivel tiene autoridad sobre los esfuerzos de promoción de la comunidad.