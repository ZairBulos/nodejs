# Depuración interactiva de aplicaciones de Node.js con el depurador integrado y el de Visual Studio Code

Obtenga información sobre cómo depurar de forma eficaz la aplicación de Node.js con Visual Studio Code para corregir los errores rápidamente.

### Requisitos previos

* Instalaciones locales de Node.js y Visual Studio Code

### Objetivos de aprendizaje

* Usar el depurador de Visual Studio Code con un programa de Node.js.
* Crear puntos de interrupción y ejecutar el código paso a paso para detectar problemas
* Inspeccionar el estado del programa en cualquier paso de ejecución
* Rebobinar la pila de llamadas para buscar el origen de una excepción

<hr/>

## Introducción

Como dijo Edsger Dijkstra:

> "Si la depuración es el proceso de eliminar errores, la programación será el proceso de incluirlos".

Como desarrollador del importante distribuidor en línea Tailwind Traders, seguramente escribirá muchos errores de Node.js, y eso está bien, porque es parte de la codificación.

En este módulo, aprenderá a depurar programas de Node.js de forma eficaz. Cuanto más rápido pueda buscar e identificar errores, más rápido podrá conseguir que el código tenga un estado operativo. Dedicará menos tiempo a intentar descubrir por qué el código funcionaba hace cinco segundos, pero ahora no.

<hr/>

## ¿Qué es un depurador?

Durante su trabajo como desarrollador, es inevitable que en algún momento se pregunte:

> ¿Por qué no funciona el código?

Durante su trabajo en la aplicación de venta al por menor de Tailwind Traders, lo más probable es que se enfrente a esta situación en varias ocasiones. Cuando se produce un error en un programa, cada persona aborda la situación de una manera diferente.

Probablemente ya haya intentado uno o varios de estos enfoques de depuración:

- Intentar volver a ejecutar el programa, porque *debería* funcionar.
- Explique el problema a su compañero.
- Volver a leer el código para averiguar cuál es el problema.
- Ir a dar un paseo.
- Incluir unos cuantos `console.log('here')` en el código.

Aunque es posible que estos métodos funcionen hasta cierto punto, hay otro enfoque que se considera más efectivo: usar un depurador. Pero ¿qué es un depurador exactamente?

Un depurador es una herramienta de software que se usa para observar y controlar el flujo de ejecución de un programa desde una perspectiva analítica. El objetivo de diseño consiste en detectar la causa principal de un error y ayudar a resolverlo. Para ello, hospeda el programa en un proceso de ejecución propio, o bien lo ejecuta como un proceso independiente que se asocia al programa en ejecución, como Node.js.

Hay diferentes tipos de depuradores. Algunos funcionan directamente desde la línea de comandos, mientras que otros tienen una interfaz gráfica de usuario. En este módulo, se usará el depurador de línea de comandos integrado en Node.js y el depurador gráfico integrado de Visual Studio Code.

### ¿Por qué usar un depurador?

Si no ejecuta el código a través de un depurador, significa que probablemente esté *adivinando* lo que ocurre en el programa. La principal ventaja de usar un depurador es que puede ver cómo se ejecuta el programa. Puede seguir línea a línea la ejecución del programa. De esta manera evitará equivocarse.

Cada depurador tiene su propio conjunto de características. Las dos características más importantes que se incluyen con casi todos los depuradores son las siguientes:

- **Control de la ejecución del programa**. Puede pausar el programa y ejecutarlo paso a paso, lo que le permite ver qué código se ejecuta y cómo afecta al estado del programa.
- **Observación del estado del programa**. Por ejemplo, puede examinar los parámetros de valor y función de las variables en cualquier momento durante la ejecución del código.

El dominio del uso del depurador es una habilidad importante para un desarrollador, pero se suele pasar por alto. Facilita la búsqueda de errores en el código. También puede ayudarle a comprender rápidamente cómo funciona un programa.

<hr/>

## Depuración con el depurador integrado de Node.js

La depuración es un proceso de varias fases que normalmente sigue estos pasos:

1. Se identifica un error en el programa.
2. Se busca dónde se encuentra el error en el código.
3. Se analiza por qué se produce el error.
4. Se corrige el error.
5. Se comprueba que la corrección funciona.

Después de identificar un error en el programa de Node.js, el primer reto al que se enfrentará es descubrir dónde se encuentra el error en el código. Para lograrlo, una de las formas más eficaces consiste en ejecutar el código paso a paso para hacerse una idea de dónde se tuercen las cosas.

### Puntos de interrupción

Es posible que la ejecución paso a paso de todo el código sea extremadamente ineficaz si el programa tiene miles de líneas de código. En ese caso, puede usar un *punto de interrupción*. Le permite *interrumpir* la ejecución normal del programa y hacer una pausa en un punto concreto del código.

Con los puntos de interrupción, puede hacer que el programa se ejecute con normalidad hasta llegar a la parte crítica donde sospecha que se encuentra el error. Después, puede cambiar a la ejecución paso a paso.

Hay varias maneras de definir puntos de interrupción en el código, en función del depurador y del editor de código. Hay una manera universal de obligar a cualquier depurador de JavaScript a pausarse en un punto concreto. Se usa la instrucción `debugger`.

Puede agregar esta instrucción en cualquier punto del código, como se muestra aquí:

```JavaScript
function addToBasket(product) {
    // Use "debugger" statement to pause at start of this function
    debugger;
  
    const basket = getCurrentBasket();
    basket.add(product);
}
```

### Modo de inspección de Node.js

Como un depurador tiene acceso total al entorno de ejecución, un actor malintencionado también podría usarlo para insertar código arbitrario en el proceso de Node.js. Por ese motivo, de forma predeterminada, Node.js no permite depurar un programa en ejecución. Tendrá que habilitar un modo especial denominado modo *inspector* para permitir la depuración.

Necesita la opción `--inspect` para permitir que un proceso de Node.js escuche un cliente del depurador que se asociará al proceso y tomará el control de la ejecución del programa.

De forma predeterminada, cuando se inicia Node.js con la opción `--inspect`, escuchará en el host 127.0.0.1 en el puerto 9229. También puede especificar un host y un puerto personalizados mediante la sintaxis `--inspect=<HOST>:<PORT>`.

> ℹ️ **Importante** <br/>
> Evite enlazar el puerto del depurador de Node.js a una dirección IP pública o a 0.0.0.0. De lo contrario, cualquier cliente que pueda conectarse a la dirección IP podría conectarse y controlar el proceso de Node.js. De este modo, un atacante puede ejecutar de forma remota código arbitrario en el entorno de ejecución. Esta acción podría provocar una infracción de seguridad potencialmente grave.

Como alternativa, puede usar la opción `--inspect-brk`. Funciona igual que `--inspect`, pero interrumpe la ejecución del código justo antes de su inicio.

Después de iniciar Node.js con el modo de inspección habilitado, puede usar cualquier cliente del depurador compatible para conectarse al proceso de Node.js.

#### Error de tiempo de espera de la inspección del nodo

Si el comando `node inspect` o `node --inspect` devuelve un error de tiempo de espera, puede intentar instalar el modo de inspección globalmente para que el sistema resuelva el problema. Para ver las opciones de instalación, consulte [Instalación de Node.js](https://nodejs.dev/en/learn/how-to-install-nodejs).

### Depurador integrado

Por ejemplo, puede usar [node-inspect](https://github.com/nodejs/node-inspect). Este depurador de línea de comandos viene incluido en Node.js. Para usarlo, ejecute el programa de esta manera:

```Bash
node inspect <YOUR_SCRIPT>.js
```

El depurador de node-inspect ejecutará Node.js con el modo de inspección habilitado y se iniciará al mismo tiempo que el depurador interactivo integrado. Pausará la ejecución justo antes de que se inicie el código. Debería ver el mensaje del depurador que indica que se ha iniciado correctamente.

```Bash
node inspect myscript.js
< Debugger listening on ws://127.0.0.1:9229/ce3689fa-4433-41ee-9d5d-98b5bc5dfa27
< For help, see: https://nodejs.org/en/docs/inspector
< Debugger attached.
Break on start in myscript.js:1
> 1 const express = require('express');
  2
  3 const app = express();
debug>
```

Ahora puede usar uno de los diversos comandos para controlar la ejecución del programa:

- `cont` o `c`: continuar. Continúa la ejecución hasta el punto de interrupción siguiente o hasta el final del programa.
- `next` o `n`: depura paso a paso hasta la siguiente instrucción. Ejecuta la siguiente línea de código en el contexto actual.
- `step` o `s`: depura paso a paso por instrucciones. Es igual que `next`, salvo que si la siguiente línea de código es una llamada de función, va a la primera línea del código de esta función.
- `out` o `o`: sale de la depuración. Si el contexto de ejecución actual está dentro del código de una función, ejecuta el código restante de esta función y vuelve a la línea de código donde se llamó inicialmente a esta función.
- `restart` o `r`: reiniciar. Reinicia el programa y pausa la ejecución antes del inicio del código.

Para establecer o borrar puntos de interrupción en el código, use los comandos siguientes:

- `setBreakpoint()` o `sb()`: agrega un punto de interrupción en la línea actual.
- `setBreakpoint(<N>)` o `sb(<N>)`: agrega un punto de interrupción en el número de línea N.
- `clearBreakpoint('myscript.js', <N>)` o `cb('myscript.js', <N>)`: borra el punto de interrupción del archivo myscript.js en el número de línea N.

Para obtener información sobre el punto de ejecución actual, ejecute estos comandos:

- `list(<N>)`: muestra el código fuente con N líneas antes y después del punto de ejecución actual.
- `exec <EXPR>`: evalúa una expresión en el contexto de ejecución actual. Este comando es útil para ayudarle a obtener información sobre el estado actual. Por ejemplo, puede obtener el valor de una variable denominada `i` mediante `exec i`.

Son bastantes comandos para recordar. Afortunadamente, también puede usar el comando `help` para mostrar la lista completa de los comandos disponibles. Para salir del depurador en cualquier momento, presione `Ctrl+D`, o bien seleccione el comando .exit.

<hr/>

## Depuración con Visual Studio Code

Ahora verá cómo puede configurar el depurador de Visual Studio Code para usarlo con Node.js.

### Configuración de Visual Studio Code para la depuración de Node.js

En Visual Studio Code, seleccione la pestaña **Ejecutar** para acceder a la herramienta Depurador.

![run-tab](/images/03-run-tab.png)

Si tiene abierto un proyecto de Node.js, puede activar el depurador de tres maneras diferentes:

- Si tiene un archivo `.js` abierto en la ventana del editor, seleccione **Ejecutar y depurar**. Después, seleccione **Node.js** para depurar directamente este archivo de JavaScript.

    ![select-environment](/images/03-select-environment.png)

- También puede optar por abrir un **terminal de depuración de Node.js**. Al seleccionar este botón se abre una ventana de terminal especial en la que se puede ejecutar el programa desde la línea de comandos. Por ejemplo, puede escribir el comando `node myscript.js` en el terminal, y la aplicación se iniciará con el depurador habilitado de forma automática. No tendrá que usar la opción `--inspect`.

    ![terminal](/images/03-terminal.png)

- Puede seleccionar la opción para **crear un archivo launch.json** con el fin de personalizar todavía más la configuración de ejecución y compartirla con los compañeros de trabajo. Revisaremos esta opción más adelante.

#### Agregar puntos de interrupción

A diferencia del depurador integrado de línea de comandos de Node.js, el de Visual Studio Code empieza de inmediato a ejecutar el código. Si el programa finaliza rápidamente, es posible que ni siquiera tenga la oportunidad de interactuar con el depurador. Por esta razón, es posible que le interese agregar algunos puntos de interrupción antes de iniciar el código.

Para agregar un punto de interrupción en el código, busque la línea de código en el archivo JavaScript (`.js`) donde quiere agregar una interrupción. Junto al número de línea de la instrucción de código, haga clic en el margen izquierdo. Cuando se agregue el punto de interrupción, verá un círculo rojo junto al número de línea. Para quitar el punto de interrupción, haga clic en el círculo rojo.

![breakpoint](/images/03-breakpoint.png)

También puede usar el menú contextual con el botón derecho para agregar un punto de interrupción. El menú de contenido incluye la opción **Adición de un punto de interrupción condicional**, donde se especifica una *condición* para interrumpir la ejecución del código. Un punto de interrupción condicional solo estará activo cuando se cumpla la condición especificada.

![conditional-breakpoint](/images/03-conditional-breakpoint.png)

### Información general del depurador de Visual Studio Code

Después de configurar los puntos de interrupción e iniciar la aplicación, en la pantalla aparecen nuevos paneles de información y controles.

1. Controles de inicio del depurador
2. Estado de las variables
3. Estado de las variables inspeccionadas
4. Pila de llamadas actual
5. Archivos de script cargados
6. Pila de llamadas
7. Controles de ejecución
8. Paso de ejecución actual
9. Consola de depuración

![debugger-overview](/images/03-debugger-overview.png)

#### Controles de inicio del depurador

En la parte superior de la barra lateral, encontrará los controles de inicio:

![sidebar-controls](/images/03-sidebar-controls.png)

1. Inicie la depuración.
2. Seleccione la configuración de inicio activa.
3. Edite el archivo `launch.json`. Cree el archivo json si es necesario.
4. Abra la **Consola de depuración** y active la visibilidad de los paneles Variables, Inspección, Pila de llamadas y Puntos de interrupción

#### Visualización y edición del estado de las variables

Al analizar la causa de un defecto del programa, consulte el estado de las variables en busca de cambios inesperados. Puede usar el panel **Variables** para hacerlo.

Las variables se muestran organizadas por ámbito:

- Las **variables locales** son aquellas a las que se puede acceder en el ámbito actual, normalmente la función actual.
- Las **variables globales** son aquellas a las que se puede acceder desde cualquier parte del programa. También se incluyen los objetos del sistema del entorno de ejecución de JavaScript, por lo que no debe sorprenderse si aquí ve una gran cantidad de elementos.
- Las **variables de clausura** son aquellas a las que se accede desde la clausura actual, si hay alguna. Una clausura combina el ámbito local de una función con el ámbito de la función externa a la que pertenece.

Puede desplegar los ámbitos y las variables seleccionando la flecha. Al desplegar objetos, verá todas las propiedades definidas en este objeto.

Es posible cambiar el valor de una variable sobre la marcha haciendo doble clic en ella.

Al mantener el puntero sobre un parámetro de función o una variable directamente en la ventana del editor, también podrá inspeccionar su valor:

![variable-hover](/images/03-variable-hover.png)

#### Inspección de variables

Si quiere realizar el seguimiento del estado de una variable en el tiempo o en distintas funciones, puede resultar tedioso hacer una búsqueda de cada vez. Por eso resulta tan útil el panel **Inspección**.

Puede seleccionar el botón más para escribir un nombre de variable o una expresión que quiera inspeccionar. Como alternativa, puede hacer clic con el botón derecho en una variable en el panel **Variables** y seleccionar **Agregar a inspección**.

Todas las expresiones que se encuentran dentro del panel de inspección se actualizarán automáticamente a medida que se ejecute el código.

#### Pila de llamadas

Cada vez que el programa especifique una función, se agregará una entrada a la pila de llamadas. Cuando la aplicación se vuelva compleja y se llame a funciones dentro de otras funciones de forma repetida, la pila de llamadas representa el rastro de las llamadas a las funciones.

Esto resulta útil para buscar el origen de una excepción. Si se produce un bloqueo inesperado en el programa, verá algo parecido a esto en la consola:

```Bash
/Users/learn/nodejs/index.js:22
  return value.toFixed(2);
               ^
TypeError: Cannot read property 'toFixed' of undefined
    at formatValueForDisplay (/Users/learn/nodejs/index.js:22:16)
    at printForeignValues (/Users/learn/nodejs/index.js:31:28)
    at Object.<anonymous> (/Users/learn/nodejs/index.js:39:1)
    at Module._compile (internal/modules/cjs/loader.js:956:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:973:10)
    at Module.load (internal/modules/cjs/loader.js:812:32)
    at Function.Module._load (internal/modules/cjs/loader.js:724:14)
    at Function.Module.runMain (internal/modules/cjs/loader.js:1025:10)
    at internal/main/run_main_module.js:17:11
```

El grupo de líneas `at [...]` situado debajo del mensaje de error se denomina *seguimiento de la pila*. Proporciona el nombre y el origen de cada función a la que se ha llamado antes de terminar con la excepción. Pero puede ser algo difícil de descifrar, ya que también incluye funciones internas del entorno de ejecución de Node.js.

Aquí es donde resulta práctico el panel **Pila de llamadas** de Visual Studio Code. Filtra la información no deseada y le muestra solo las funciones pertinentes de su código de forma predeterminada. Después, puede desenredar esta pila de llamadas para averiguar dónde se originó la excepción.

Para que le resulte todavía más fácil, puede seleccione el botón **Reiniciar marco** que aparece al mantener el puntero sobre el nombre de una función en la pila. La ejecución se *retrasará* hasta el principio de ese función y el programa se reiniciará en ese punto.

![restart-frame](/images/03-restart-frame.png)

#### Visualización de archivos de script cargados

En este panel se muestran todos los archivos de JavaScript que se hayan cargado hasta ahora. En proyectos de gran tamaño, a veces puede resultar útil comprobar a qué archivo pertenece el código que se ejecuta actualmente.

#### Puntos de interrupción

En el panel **Puntos de interrupción**, puede ver todos los puntos de interrupción que haya colocado en el código y alternar entre ellos. También puede alternar entre las opciones para interrumpir las excepciones, tanto las detectadas como las no detectadas. Puede usar el panel Puntos de interrupción para examinar el estado del programa y localizar el origen de una excepción cuando se produzca mediante la **pila de llamadas**.

#### Control de la ejecución

Puede controlar el flujo de ejecución del programa mediante estos controles.

![debugger-controls](/images/03-debugger-controls.png)

Estos son los controles, de izquierda a derecha:

- **Continuar o pausar la ejecución**. Si la ejecución está en pausa, continuará hasta que se alcance el siguiente punto de interrupción. Si el programa se está ejecutando, el botón se convertirá en un botón de pausa que puede usar para pausar la ejecución.
- **Depurar paso a paso por procedimientos**. Ejecuta la siguiente instrucción de código en el contexto actual (igual que el comando `next` del depurador integrado).
- **Depurar paso a paso por instrucciones**. Es igual que Depurar paso a paso por procedimientos, pero, si la siguiente instrucción es una llamada de función, pasa a la primera instrucción de código de esta función (igual que el comando `step`).
- **Salir de la depuración**. Si está dentro de una función, ejecuta el código restante de esta y vuelve a la instrucción después de la llamada de función inicial (igual que el comando `out`).
- **Reiniciar**. Reinicia el programa desde el principio.
- **Detener**. Finaliza la ejecución y sale del depurador.

#### Uso de la consola de depuración

Puede presionar `Ctrl+Mayús+Y` (Windows y Linux) o `Cmd+Mayús+Y` (Mac) para mostrar u ocultar la consola de depuración. Se puede usar para visualizar los registros de la consola de la aplicación y evaluar expresiones, así como ejecutar código en el contenido de ejecución actual, como el comando `exec` del depurador integrado de Node.js.

Puede escribir una expresión de JavaScript en el campo de entrada de la parte inferior de la consola de depuración. Después, presione `Enter` para evaluarla. El resultado se muestra directamente en la consola.

De este modo, puede comprobar rápidamente el valor de una variable, probar una función con valores diferentes o modificar el estado actual.

#### Adición de puntos de registro

Si quiere seguir la ejecución del programa mediante registros, existe una alternativa al uso de `console.log`. Puede usar *puntos de registro*.

Al hacer clic con el botón derecho en la misma área que se usa para agregar un punto de interrupción, puede seleccionar **Agregar punto de registro**.

![logpoint](/images/03-logpoint.png)

Escriba un mensaje para que se muestre en ese punto del código. Incluso es posible imprimir expresiones si las incluye entre corchetes mediante `{<EXPRESSION>}`.

Como sucede con los puntos de interrupción, los puntos de registro no modifican el código de ningún modo y solo se usan durante la depuración. Ya no tiene ninguna excusa para permitir que `console.log('here')` se use en producción.

<hr/>

## Ejercicio: Depuración con Visual Studio Code

Es el momento de poner en práctica todo lo que acaba de aprender sobre la depuración. De hecho, se nos ha presentado la ocasión perfecta. En la aplicación de Tailwind Traders, se va a desarrollar una nueva característica que permite mostrar el precio de un producto en varias divisas. Un compañero de trabajo ha escrito un poco de código, pero le está costando detectar qué es lo que funciona mal. Hay que ayudarle.

### Creación de un archivo JavaScript en un área de trabajo de Visual Studio

Para este ejercicio, necesita un archivo JavaScript para practicar la depuración. Para usar los controles de inicio del depurador, el archivo JavaScript debe estar en un área de trabajo de Visual Studio.

El objetivo de este programa es establecer la tasa de cambio entre tres divisas: USD, EUR y JPY. Después, se quiere mostrar el valor de 10 EUR en todas las demás divisas, con dos dígitos después del separador decimal. Para cada divisa agregada, se debe calcular la tasa de cambio a todas las demás.

1. En Visual Studio Code, seleccione **Archivo>Nuevo archivo**.

2. Pegue el siguiente código en el editor de nuevo archivo:

    ```JavaScript
    const rates = {};

    function setExchangeRate(rate, sourceCurrency, targetCurrency) {
    if (rates[sourceCurrency] === undefined) {
        rates[sourceCurrency] = {};
    }

    if (rates[targetCurrency] === undefined) {
        rates[targetCurrency] = {};
    }

    rates[sourceCurrency][targetCurrency] = rate;
    rates[targetCurrency][sourceCurrency] = 1 / rate;
    }

    function convertToCurrency(value, sourceCurrency, targetCurrency) {
    const exchangeRate = rates[sourceCurrency][targetCurrency];
    return exchangeRate && value * exchangeRate;
    }

    function formatValueForDisplay(value) {
    return value.toFixed(2);
    }

    function printForeignValues(value, sourceCurrency) {
    console.info(`The value of ${value} ${sourceCurrency} is:`);

    for (const targetCurrency in rates) {
        if (targetCurrency !== sourceCurrency) {
        const convertedValue = convertToCurrency(value, sourceCurrency, targetCurrency);
        const displayValue = formatValueForDisplay(convertedValue);
        console.info(`- ${convertedValue} ${targetCurrency}`);
        }
    }
    }

    setExchangeRate(0.88, 'USD', 'EUR');
    setExchangeRate(107.4, 'USD', 'JPY');
    printForeignValues(10, 'EUR');
    ```

3. Para guardar el archivo, presione `Ctrl+S` (Windows y Linux) o `Cmd+S` (Mac).
    - Vaya a la carpeta donde quiera guardar el archivo nuevo.
    - Para el nombre de archivo, escriba *currency* y, para el tipo, elija JavaScript (`.js`).

4. Seleccione **Archivo>Add Folder to Workspace** (Agregar carpeta al área de trabajo).
    - Vaya a la carpeta donde haya guardado el archivo nuevo.
    - Seleccione **Agregar**.

### Creación de una configuración de inicio

Como vamos a usar mucho el depurador, crearemos una configuración de inicio para la aplicación.

1. En la pestaña **Ejecutar** de Visual Studio Code, seleccione **Agregar configuración**.

    Visual Studio Code crea el archivo de configuración `.vscode/launch.json` en su proyecto y abre el archivo de inicio para su edición.

    ![launch-configuration](/images/03-launch-configuration.png)

    De forma predeterminada, se crea una configuración de inicio para ejecutar el archivo que está abierto actualmente. En este ejemplo, el archivo abierto es `currency.js`. Puede modificar la configuración de inicio para personalizar cómo se debe iniciar el programa al depurar.

2. En la configuración de inicio, actualice el valor de la propiedad `program`. Reemplace `${workspaceFolder}` o `${file}` por la ruta de acceso al archivo `currency.js` de la máquina. Por ejemplo, cambie `"program": "{$workspace}/app.js`" a `"program": "C:/Users/UserName/FolderName/currency.js"`. Asegúrese de guardar los cambios en el archivo de configuración.

3. Cierre el archivo `.vscode/launch.json`.

### Análisis de los problemas

Asegúrese de que el entorno de Visual Studio Code está listo para supervisar el proceso de depuración:

- El panel del depurador debe estar abierto a la izquierda. Use el icono de pestaña Ejecutar de la izquierda para alternar la visibilidad del panel.
- La consola de depuración debe estar abierta en la parte inferior. Para abrir la consola, seleccione **Ver>Consola de depuración** o pulse `Ctrl+Shift+Y` (Windows, Linux) o `Cmd+Shift+Y` (Mac).
- Compruebe las preferencias del registro de errores. Puede agregar `"outputCapture": "std"`, al archivo de configuración de inicio para aumentar la salida del registro.

De este modo, estará listo para iniciar la depuración.

En los controles de inicio del depurador, inicie el programa con la depuración habilitada (la flecha verde).

![start-debugging](/images/03-start-debugging.png)

Debería ver que el programa finaliza rápidamente. Eso es normal porque todavía no ha agregado ningún punto de interrupción.

Debería ver este texto en la consola de depuración, seguido de una excepción.

```text
The value of 10 EUR is:
11.363636363636365
- 11.363636363636365 USD
/app/node-101/currency.js:23
  return value.toFixed(2);
               ^
TypeError: Cannot read property 'toFixed' of undefined
    at formatValueForDisplay (/app/node-101/currency.js:23:16)
    at printForeignValues (/app/node-101/currency.js:32:28)
    at Object.<anonymous> (/app/node-101/currency.js:40:1)
    at Module._compile (internal/modules/cjs/loader.js:959:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:995:10)
    at Module.load (internal/modules/cjs/loader.js:815:32)
    at Function.Module._load (internal/modules/cjs/loader.js:727:14)
    at Function.Module.runMain (internal/modules/cjs/loader.js:1047:10)
    at internal/main/run_main_module.js:17:11
```

Lo que este programa pretende hacer es establecer la tasa de cambio entre tres divisas (USD, EUR y JPY) y mostrar el valor de 10 EUR en las demás divisas, con dos dígitos después del separador decimal.

Aquí se pueden ver dos errores:

- Hay más de dos dígitos después del separador decimal.
- El programa se ha bloqueado con una excepción y no ha mostrado el valor de JPY.

### Corrección de los dígitos mostrados

Para empezar, corregiremos el primer problema. Como no ha escrito este código y en él se llama a diferentes funciones, primero debe intentar entender el flujo de ejecución mediante la ejecución paso a paso.

#### Uso de puntos de interrupción y de la ejecución paso a paso

Para agregar un punto de interrupción, haga clic en el margen izquierdo de la línea 39, en `printForeignValues(10, 'EUR');`.

![add-breakpoint](/images/03-add-breakpoint.png)

Vuelva a iniciar la depuración y vaya a la función `printForeignValues()` con el control de depuración **Depurar paso a paso por instrucciones**:

![step-into](/images/03-step-into.png)

#### Comprobación del estado de las variables

Ahora dedique un tiempo a inspeccionar los diferentes valores de las variables en el panel **Variables**.

![variables-panel](/images/03-variables-panel.png)

- ¿Cuáles son los valores de las variables `value` y `sourceCurrency`?
- Para la variable `rates`, ¿ve las tres claves previstas, USD, EUR y JPY?

Para avanzar paso a paso hasta que se establezca la variable `convertedValue`, use el control de depuración **Depurar paso a paso por procedimientos**:

![step-over](/images/03-step-over.png)

Después de usar el control **Depurar paso a paso por procedimientos** cinco veces, debería ver que el valor de la variable `convertedValue` se establece según lo esperado en `11.363636363636365`.

Si realizamos una depuración paso a paso por procedimientos, veremos el valor de la variable `displayValue`. El valor debe ser la cadena con formato para mostrar con dos dígitos `11.36`.

Llegados a este punto en el programa, podemos afirmar que las funciones `convertToCurrency()` y `formatValueForDisplay()` parecen correctas y devuelven el resultado esperado.

#### Corrección del error

Use el control **Depurar paso a paso por instrucciones** una vez para llegar a la llamada a la función `console.info()`. Examine detenidamente esta línea de código. ¿Ve aquí el error?

Es necesario corregir este error de programa mediante la variable `displayValue` en lugar de la variable `convertedValue` para imprimir el valor.

1. Actualice el archivo `currency.js` para usar el nombre correcto de la variable. Cambie la llamada a la función `console.info()` en la línea 32 para usar la variable `displayValue` en lugar de la variable `convertedValue`:

    ```JavaScript
    console.info(`- ${displayValue} ${targetCurrency}`);
    ```

2. Guarde los cambios en el archivo.

3. Reinicie el programa.

Compruebe que el programa muestra correctamente el valor USD como 11.36. Primer error resuelto.

### Detección de la causa del bloqueo

Veamos ahora por qué se bloquea el programa.

1. En el archivo *currency.js*, quite el punto de interrupción que estableció en la línea 39.
2. Para obligar al programa a pausarse después de que se produzca una excepción, en el panel Puntos de interrupción, active la casilla **Excepciones no detectadas**.
3. Vuelva a ejecutar el programa en el depurador.

El programa se debería detener en la excepción y mostrar un extenso informe de errores en el centro de la ventana del editor.

![exception](/images/03-exception.png)

Fíjese en la línea donde se detuvo la ejecución y observe el mensaje de excepción `TypeError: Cannot read property 'toFixed' of undefined`. A partir de ese mensaje, puede deducir que la función de parámetro `value` tiene el valor undefined en lugar de un número. Este error es lo que provocó la excepción.

#### Rebobinado de la pila de llamadas

El *seguimiento de la pila* que aparece debajo del mensaje de error puede ser algo difícil de descifrar. La buena noticia es que Visual Studio Code procesa de forma automática la pila de llamadas de función. De forma predeterminada, solo muestra la información significativa en el panel **Pila de llamadas**. Vamos a usar la información de pila de llamadas para buscar el código que provocó esta excepción.

Sabemos que la excepción se produjo en la llamada a la función `formatValueForDisplay()`.

1. En el panel del depurador, vaya al panel Pila de llamadas.

2. Para ver dónde se llamó a la función `formatValueForDisplay()`, haga doble clic en la función bajo ella: la función `printForeignValues`.

    Visual Studio Code va a la línea de la función `printForeignValue`s del archivo *currency.js*, donde se llamó a la función `formatValueForDisplay()`:

    ```JavaScript
    const displayValue = formatValueForDisplay(convertedValue);
    ```

Examine detenidamente esta línea de código. El parámetro que provoca la excepción proviene de la variable `convertedValue`. Tendrá que averiguar en qué punto este valor de parámetro se convierte en `undefined`.

Una opción consiste en agregar un punto de interrupción en esta línea e inspeccionar la variable cada vez el punto de interrupción se detenga en esta línea. Sin embargo, no sabemos cuándo puede producirse el valor incorrecto y, en programas complejos, este enfoque de depuración puede ser complicado. Echemos un vistazo a un método alternativo.

#### Adición de un punto de interrupción condicional

Lo que sería útil en nuestro caso es poder detener el depurador en este punto de interrupción solo cuando el valor de la variable `convertedValue` es `undefined`. Afortunadamente, Visual Studio Code puede realizar esta acción con opciones de clic con el botón derecho del mouse.

1. En el archivo *currency.js*, en el margen izquierdo de la línea 31, haga clic con el botón derecho y seleccione **Add Conditional Breakpoint** (Agregar punto de interrupción condicional).

    ![conditional-breakpoint](/images/03-conditional-breakpoint.png)

2. Después de hacer clic con el botón derecho, escriba la siguiente condición para desencadenar el punto de interrupción y presione `Enter`:

    ```JavaScript
    `convertedValue === undefined`
    ```

3. Reinicie el programa.

El programa debería detenerse ahora en la línea 31 y podemos examinar los valores de la pila de llamadas.

#### Observación del estado actual

Dedique un tiempo a analizar el estado actual del programa.

- El valor de la variable `convertedValue` procede de la llamada a la función `convertToCurrency(value, sourceCurrency, targetCurrency)`. Es necesario comprobar los valores de parámetro en esta llamada de función y confirmar que son correctos.
- En concreto, es necesario examinar la variable `value` y confirmar que tiene el valor esperado 10.

Analice el código de la función `convertToCurrency()`.

```JavaScript
function convertToCurrency(value, sourceCurrency, targetCurrency) {
  const exchangeRate = rates[sourceCurrency][targetCurrency];
  return exchangeRate && value * exchangeRate;
}
```

Sabe que el resultado de este código es `undefined`. También sabe que la variable `value` está establecida en 10. Esta información nos ayuda a ver que el problema debe estar en el valor de la variable `exchangeRate`.

En su archivo *currency.js*, mantenga el puntero sobre la variable `rates` para echar un vistazo:

![peek-at-variable](/images/03-peek-at-variable.png)

La intención es obtener la tasa de cambio de EUR a JPY, pero, si despliega el valor EUR, verá que solo hay una tasa de conversión para USD. Falta la de JPY.

### Corrección de las tasas de conversión que faltan

Ahora que sabe que faltan algunas tasas de conversión, veremos los motivos. Para quitar todos los puntos de interrupción existentes, seleccione el icono **Quitar todos los puntos de interrupción** en el panel P**untos de interrupción**.

![remove-all-breakpoints](/images/03-remove-all-breakpoints.png)

#### Inspección de la variable rates

Vamos a establecer un punto de interrupción para ver la variable rates.

1. En el archivo *currency.js*, agregue un punto de interrupción en la línea 37 de la función `setExchangeRate(0.88, 'USD', 'EUR');`.

2. En el panel Inspección, seleccione **Más** y escriba `rates`.

    Cada vez que se cambia el valor de la variable rates, el valor actualizado se muestra en el panel **Inspección**.

3. Reinicie el programa.

4. Cuando el punto de interrupción se detenga en la primera llamada a la función `setExchangeRate()`, use el control **Paso a paso por procedimientos**.

5. En el panel **Inspección**, examine el valor de la variable `rates`.

    En este momento, puede ver que USD y EUR tienen tasas de conversión opuestas coincidentes, como se esperaba.

6.  Realice el paso a paso por procedimiento de nuevo por la segunda llamada a la función `setExchangeRate()`.

    Verá que USD y JPY tienen tasas de conversión opuestas coincidentes, pero no hay nada entre EUR y JPY.

Ha llegado el momento de examinar el código de la función setExchangeRate().

```JavaScript
function setExchangeRate(rate, sourceCurrency, targetCurrency) {
  if (rates[sourceCurrency] === undefined) {
    rates[sourceCurrency] = {};
  }

  if (rates[targetCurrency] === undefined) {
    rates[targetCurrency] = {};
  }

  rates[sourceCurrency][targetCurrency] = rate;
  rates[targetCurrency][sourceCurrency] = 1 / rate;
}
```

Las líneas más importantes en esta función son las dos últimas. Ha encontrado el segundo error. Las tasas de conversión solo se establecen entre las variables `sourceCurrency` y `targetCurrency`. El programa también debe calcular la tasa de conversión para las otras divisas que se agregaron.

#### Corrección del código

Vamos a corregir el código para el problema de tasa de conversión.

1. Actualice el archivo *currency.js* para calcular la tasa de conversión de las otras divisas.

    Reemplace el código de las líneas 12 y 13:

    ```JavaScript
    rates[sourceCurrency][targetCurrency] = rate;
    rates[targetCurrency][sourceCurrency] = 1 / rate;
    ```

    con este código actualizado:

    ```JavaScript`
    for (const currency in rates) {
        if (currency !== targetCurrency) {
            // Use a pivot rate for currencies that don't have the direct conversion rate
            const pivotRate = currency === sourceCurrency ? 1 : rates[currency][sourceCurrency];
            rates[currency][targetCurrency] = rate * pivotRate;
            rates[targetCurrency][currency] = 1 / (rate * pivotRate);
        }
    }
    ```

2. Guarde los cambios en el archivo.

El código actualizado establece la tasa de conversión para cualquier otra divisa que no sea `sourceCurrency` y `targetCurrency`. El programa utiliza la tasa de conversión de `sourceCurrency` para deducir la tasa entre la otra divisa y `targetCurrency`. A continuación, el código establece la tasa de conversión para la otra divisa en consecuencia.

#### Prueba de la corrección

Vamos a probar el cambio que hemos realizado.

1. Quite todos los puntos de interrupción y las variables de inspección.
2. Reinicie el programa.

Ahora debería ver el resultado esperado en la consola, sin ningún bloqueo.

```text
The value of 10 EUR is:
- 11.36 USD
- 1220.45 JPY
```