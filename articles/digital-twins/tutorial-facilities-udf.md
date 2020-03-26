---
title: 'Tutorial: Supervisión de un espacio de dispositivo IoT con Azure Digital Twins | Microsoft Docs'
description: Use los pasos de este tutorial para aprender a aprovisionar los recursos espaciales y a supervisar las condiciones de trabajo con Azure Digital Twins.
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 01/10/2020
ms.openlocfilehash: 6cf6a8f7de181a81d60028e33ba2631815c8ca04
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "75895368"
---
# <a name="tutorial-provision-your-building-and-monitor-working-conditions-with-azure-digital-twins-preview"></a>Tutorial: Aprovisionamiento del edificio y supervisión de las condiciones de trabajo con la versión preliminar de Azure Digital Twins

En este tutorial se muestra cómo usar la versión preliminar de Azure Digital Twins para supervisar las condiciones de temperatura y el nivel de comodidad deseados en los distintos espacios. Una vez que haya [configurado el edificio de ejemplo](tutorial-facilities-setup.md), use los pasos de este tutorial para aprovisionarlo y ejecutar funciones personalizadas en los datos de los sensores.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Definir las condiciones que se van a supervisar
> * Crear una función definida por el usuario (UDF)
> * Simular los datos de los sensores
> * Obtener los resultados de una función definida por el usuario

## <a name="prerequisites"></a>Prerrequisitos

En este tutorial se supone que ha [terminado de configurar Azure Digital Twins](tutorial-facilities-setup.md). Antes de continuar, asegúrese de que:

- Una [cuenta de Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Hay una instancia de Digital Twins en ejecución. 
- Los [ejemplos en C# de Digital Twins](https://github.com/Azure-Samples/digital-twins-samples-csharp) se han descargado y extraído en su equipo de trabajo. 
- [Versión 2.1.403 del SDK de .NET Core, o cualquier versión superior,](https://www.microsoft.com/net/download) en la máquina de desarrollo para compilar y ejecutar el ejemplo. Ejecute `dotnet --version` para comprobar que está instalada la versión correcta. 
- Tiene [Visual Studio Code](https://code.visualstudio.com/) para explorar el código de ejemplo. 

>[!TIP]
> Use un nombre de instancia de Digital Twins único si está aprovisionando una nueva instancia.

## <a name="define-conditions-to-monitor"></a>Definición de las condiciones que se van a supervisar

Puede definir un conjunto de condiciones específicas para supervisar en los datos del dispositivo o del sensor, llamado *buscador de coincidencias*. Luego, puede definir funciones llamadas *funciones definidas por el usuario*. Las funciones definidas por el usuario ejecutan lógica personalizada en los datos que proceden de los espacios y los dispositivos, cuando se producen las condiciones especificadas por los buscadores de coincidencias. Para más información, lea [Procesamiento de datos y funciones definidas por el usuario](concepts-user-defined-functions.md). 

En el proyecto de ejemplo **occupancy-quickstart**, abra el archivo **src\actions\provisionSample.yaml** en Visual Studio Code. Observe la sección que comienza con el tipo **matchers**. Cada entrada con este tipo crea un buscador de coincidencias con el **nombre** especificado. El buscador de coincidencias supervisará un sensor de tipo **dataTypeValue**. Observe cómo se relaciona con el espacio denominado *Focus Room A1*, que tiene un nodo **devices**, que contiene algunos sensores. Para aprovisionar un buscador de coincidencias que realice el seguimiento de uno de estos sensores, asegúrese de que su valor de **dataTypeValue** coincida con el valor de **dataType** del sensor. 

Agregue el siguiente buscador de coincidencias debajo de los buscadores de coincidencias existentes. Asegúrese de que las claves estén alineadas y que los espacios no se reemplacen por pestañas. Estas líneas también están presentes en el archivo *provisionSample.yaml* como líneas con comentarios. Para quitarles la marca de comentario, elimine el carácter `#` delante de cada línea.

```yaml
      - name: Matcher Temperature
        dataTypeValue: Temperature
```

Este buscador de coincidencias realizará el seguimiento del sensor `SAMPLE_SENSOR_TEMPERATURE` que agregó en el [primer tutorial](tutorial-facilities-setup.md). 

## <a name="create-a-user-defined-function"></a>Creación de una función definida por el usuario

Puede usar funciones definidas por el usuario para personalizar el procesamiento de los datos de los sensores. Estas funciones son código JavaScript personalizado que se puede ejecutar en la instancia de Azure Digital Twins cuando se producen determinadas condiciones que describen los buscadores de coincidencias. Se pueden crear buscadores de coincidencias y funciones definidas por el usuario para todos los sensores que quiera supervisar. Para más información, lea [Procesamiento de datos y funciones definidas por el usuario](concepts-user-defined-functions.md). 

En el archivo *provisionSample.yaml* de ejemplo, busque una sección que comience por el tipo **userdefinedfunctions**. En esta sección se proporciona una función definida por el usuario con un **nombre** determinado. Dicha función actúa sobre la lista de buscadores de coincidencias en **matcherNames**. Tenga en cuenta que puede especificar su propio archivo de JavaScript para la UDF como **script**.

Fíjese también en la sección denominada **roleassignments**. Esta sección asigna el rol Administrador de espacios a la función definida por el usuario y le permite acceder a los eventos que proceden de cualquiera de los espacios aprovisionados. 

1. Configure la UDF para incluir el buscador de coincidencias de temperatura. Para ello, debe agregar o quitar el comentario de la línea siguiente del nodo `matcherNames` del archivo *provisionSample.yaml*:

    ```yaml
            - Matcher Temperature
    ```

1. Abra el archivo **src\actions\userDefinedFunctions\availability.js** en el editor. Este es el archivo al que se hace referencia en el elemento **script** de *provisionSample.yaml*. La función definida por el usuario en este archivo busca condiciones cuando no se detecta movimiento en la sala, así como cuando los niveles de dióxido de carbono están por debajo de 1000 ppm. 

   Modifique el archivo de JavaScript para supervisar la temperatura y otras condiciones. Agregue las siguientes líneas de código para buscar condiciones cuando no se detecte movimiento en la sala, los niveles de dióxido de carbono no lleguen a 1000 ppm y la temperatura sea inferior a 78 ºF.

   >[!NOTE]
   > En esta sección se modifica el archivo *src\actions\userDefinedFunctions\availability.js* para explicar detalladamente una manera de escribir una función definida por el usuario. Sin embargo, puede elegir usar directamente el archivo [src\actions\userDefinedFunctions\availabilityForTutorial.js](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availabilityForTutorial.js) en su configuración. Dicho archivo contiene todos los cambios necesarios para este tutorial. Si lo usa, asegúrese de utilizar el nombre de archivo correcto para la clave del **script** en [src\actions\provisionSample.yaml](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml).

    a. Al principio del archivo, agregue las siguientes líneas para la temperatura debajo del comentario `// Add your sensor type here`:

    ```JavaScript
        var temperatureType = "Temperature";
        var temperatureThreshold = 78;
    ```

    b. Agregue las líneas siguientes después de la instrucción que define `var motionSensor`, debajo del comentario `// Add your sensor variable here`:

     ```JavaScript
        var temperatureSensor = otherSensors.find(function(element) {
            return element.DataType === temperatureType;
        });
    ```

    c. Agregue la línea siguiente después de la instrucción que define `var carbonDioxideValue`, debajo del comentario `// Add your sensor latest value here`:

    ```JavaScript
        var temperatureValue = getFloatValue(temperatureSensor.Value().Value);
    ```

    d. Quite las siguientes líneas de código de debajo del comentario `// Modify this line to monitor your sensor value`:

     ```JavaScript
        if(carbonDioxideValue === null || motionValue === null) {
            sendNotification(telemetry.SensorId, "Sensor", "Error: Carbon dioxide or motion are null, returning");
            return;
        }
    ```

    Reemplácelas por estas:

    ```JavaScript
        if(carbonDioxideValue === null || motionValue === null || temperatureValue === null){
            sendNotification(telemetry.SensorId, "Sensor", "Error: Carbon dioxide, motion, or temperature are null, returning");
            return;
        }
    ```

    e. Quite las siguientes líneas de código de debajo del comentario `// Modify these lines as per your sensor`:

    ```JavaScript
        var availableFresh = "Room is available and air is fresh";
        var noAvailableOrFresh = "Room is not available or air quality is poor";
    ```

    Reemplácelas por estas:

    ```JavaScript
        var alert = "Room with fresh air and comfortable temperature is available.";
        var noAlert = "Either room is occupied, or working conditions are not right.";
    ```

    f. Quite el siguiente bloque de código *if-else* después del comentario `// Modify this code block for your sensor`:

    ```JavaScript
        // If carbonDioxide less than threshold and no presence in the room => log, notify and set parent space computed value
        if(carbonDioxideValue < carbonDioxideThreshold && !presence) {
            log(`${availableFresh}. Carbon Dioxide: ${carbonDioxideValue}. Presence: ${presence}.`);
            setSpaceValue(parentSpace.Id, spaceAvailFresh, availableFresh);
        }
        else {
            log(`${noAvailableOrFresh}. Carbon Dioxide: ${carbonDioxideValue}. Presence: ${presence}.`);
            setSpaceValue(parentSpace.Id, spaceAvailFresh, noAvailableOrFresh);

            // Set up custom notification for poor air quality
            parentSpace.Notify(JSON.stringify(noAvailableOrFresh));
        }
    ```

    Y sustitúyala por el siguiente bloque *if-else*:

    ```JavaScript
        // If sensor values are within range and room is available
        if(carbonDioxideValue < carbonDioxideThreshold && temperatureValue < temperatureThreshold && !presence) {
            log(`${alert}. Carbon Dioxide: ${carbonDioxideValue}. Temperature: ${temperatureValue}. Presence: ${presence}.`);

            // log, notify and set parent space computed value
            setSpaceValue(parentSpace.Id, spaceAvailFresh, alert);

            // Set up notification for this alert
            parentSpace.Notify(JSON.stringify(alert));
        }
        else {
            log(`${noAlert}. Carbon Dioxide: ${carbonDioxideValue}. Temperature: ${temperatureValue}. Presence: ${presence}.`);

            // log, notify and set parent space computed value
            setSpaceValue(parentSpace.Id, spaceAvailFresh, noAlert);
        }
    ```

    La UDF modificada buscará una condición en la que una sala esté disponible y tenga el dióxido de carbono y la temperatura dentro de unos límites tolerables. Cuando dicha condición se cumpla, generará una notificación con la instrucción `parentSpace.Notify(JSON.stringify(alert));`. Establece el valor del espacio supervisado, independientemente de si se cumple la condición, con el mensaje correspondiente.

    g. Guarde el archivo.

1. Abra una ventana de comandos y vaya a la carpeta **occupancy-quickstart\src**. Ejecute el siguiente comando para aprovisionar el grafo de inteligencia espacial y la función definida por el usuario:

    ```cmd/sh
    dotnet run ProvisionSample
    ```

   >[!IMPORTANT]
   > Para evitar el acceso no autorizado a Management API de Digital Twins, la aplicación **occupancy-quickstart** le exige que inicie sesión con las credenciales de su cuenta de Azure. Como las credenciales se guardan durante un breve periodo, es posible que no tenga que iniciar sesión cada vez que lo ejecute. La primera vez que se ejecuta este programa y, luego, cuándo expiran las credenciales guardadas, la aplicación le dirige a una página de inicio de sesión y le proporciona un código específico de la sesión para entrar en la página. Siga las indicaciones para iniciar sesión con su cuenta de Azure.

1. Después de que se autentica la cuenta, la aplicación empieza a crear un grafo espacial de ejemplo como se ha configurado en *provisionSample.yaml*. Espere hasta que finalice el aprovisionamiento. Esta operación puede tardar unos minutos. Cuando se haya completado, observe los mensajes de la ventana de comandos y vea cómo se crea el grafo espacial. Observe cómo la aplicación crea una instancia de IoT Hub en el nodo raíz o en `Venue`.

1. En la salida de la ventana de comandos, copie el valor de `ConnectionString`, que aparece en la sección `Devices`, en el Portapapeles. Necesitará este valor para simular la conexión del dispositivo en la sección siguiente.

    [![Ejemplo de aprovisionamiento](./media/tutorial-facilities-udf/run-provision-sample.png)](./media/tutorial-facilities-udf/run-provision-sample.png#lightbox)

>[!TIP]
> Si recibe un mensaje de error que dice algo como que la operación de E/S se anuló debido a una salida de subproceso o una solicitud de aplicación en medio del aprovisionamiento, pruebe a volver a ejecutar el comando. Esto puede ocurrir si el cliente HTTP ha agotado el tiempo de espera debido a algún problema en la red.

## <a name="simulate-sensor-data"></a>Simulación de los datos de los sensores

En esta sección, usará el proyecto denominado *device-connectivity* en el ejemplo. Simulará los datos de los sensores para la detección de movimiento, temperatura y dióxido de carbono. Este proyecto genera valores aleatorios para los sensores y los envía a la instancia de IoT Hub mediante la cadena de conexión del dispositivo.

1. En otra ventana de comandos, vaya al ejemplo de Azure Digital Twins y, después, a la carpeta **device-connectivity**.

1. Ejecute este comando para asegurarse de que las dependencias del proyecto son correctas:

    ```cmd/sh
    dotnet restore
    ```

1. Abra el archivo [appSettings.json](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/device-connectivity/appsettings.json) en el editor y edite los valores siguientes:

   a. **DeviceConnectionString**: asigne el valor de `ConnectionString` en la ventana de salida de la sección anterior. Copie esta cadena completamente, entre comillas, para que el simulador se conecte correctamente con la instancia de IoT Hub.

   b. **HardwareId** en la matriz **Sensors**: dado que va a simular eventos procedentes de sensores aprovisionados en una instancia de Azure Digital Twins, tanto el identificador de hardware como los nombres de los sensores de este archivo deben coincidir con el nodo `sensors` del archivo *provisionSample.yaml*.

      Agregue una nueva entrada para el sensor de temperatura. El nodo **Sensors** de *appSettings.json* debe parecerse a este:

      ```JSON
      "Sensors": [{
        "DataType": "Motion",
        "HardwareId": "SAMPLE_SENSOR_MOTION"
      },{
        "DataType": "CarbonDioxide",
        "HardwareId": "SAMPLE_SENSOR_CARBONDIOXIDE"
      },{
        "DataType": "Temperature",
        "HardwareId": "SAMPLE_SENSOR_TEMPERATURE"
      }]
      ```

1. Ejecute este comando para iniciar la simulación de eventos de temperatura, movimiento y dióxido de carbono del de dispositivo:

    ```cmd/sh
    dotnet run
    ```

   >[!NOTE]
   > Como el ejemplo de simulación no se comunica directamente con la instancia de Digital Twin, no requiere autenticación.

## <a name="get-results-of-the-user-defined-function"></a>Obtención de los resultados de la función definida por el usuario

La función definida por el usuario se ejecuta cada vez que una instancia recibe datos del dispositivo y de los sensores. En esta sección se consulta su instancia de Azure Digital Twins para obtener los resultados de la función definida por el usuario. Se le notificará casi en tiempo real cuando una sala está disponible, el aire sea puro y la temperatura, la adecuada. 

1. Abra la ventana de comandos que usó para aprovisionar el ejemplo, o una nueva, y vaya otra vez a la carpeta **occupancy-quickstart\src** del ejemplo.

1. Ejecute el comando siguiente e inicie sesión cuando se le solicite:

    ```cmd/sh
    dotnet run GetAvailableAndFreshSpaces
    ```

La ventana de salida muestra cómo se ejecuta la función definida por el usuario e intercepta los eventos de la simulación de dispositivos. 

   [![Salida de la función definida por el usuario](./media/tutorial-facilities-udf/adt-tutorial-udf-running.png)](./media/tutorial-facilities-udf/adt-tutorial-udf-running.png#lightbox)

Si se cumple la condición supervisada, la función definida por el usuario establece el valor del espacio con el mensaje pertinente, como se ha visto [anteriormente](#create-a-user-defined-function). La función `GetAvailableAndFreshSpaces` imprime el mensaje en la consola.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere dejar de explorar Azure Digital Twins en este punto, elimine los recursos creados en este tutorial:

1. En el menú izquierdo de [Azure Portal](https://portal.azure.com), seleccione **Todos los recursos**, seleccione el grupo de recurso de Digital Twins y haga clic en **Eliminar**.

    >[!TIP]
    > Si tiene problemas al eliminar una instancia de Digital Twins, se ha incorporado una actualización del servicio con la corrección. Vuelva a intentar eliminar la instancia.

2. Si es necesario, elimine las aplicaciones de ejemplo en la máquina de trabajo.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprovisionado los espacios y ha creado una plataforma para desencadenar notificaciones personalizadas, puede continuar con cualquiera de los siguientes tutoriales:

> [!div class="nextstepaction"]
> [Tutorial: Recepción de notificaciones de los espacios de Azure Digital Twins mediante Logic Apps](tutorial-facilities-events.md)

> [!div class="nextstepaction"]
> [Tutorial: Visualización y análisis de eventos de los espacios de Azure Digital Twins mediante Time Series Insights](tutorial-facilities-analyze.md)
