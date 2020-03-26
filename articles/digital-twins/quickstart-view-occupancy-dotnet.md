---
title: 'Inicio rápido: Búsqueda de habitaciones disponibles - Azure Digital Twins | Microsoft Docs'
description: En esta guía de inicio rápido, ejecutará dos aplicaciones de ejemplo de .NET Core para enviar información de telemetría de movimiento simulado y dióxido de carbono a un espacio de Azure Digital Twins. El objetivo es encontrar habitaciones disponibles con aire fresco desde las API de administración tras el procesamiento calculado en la nube.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc seodec18
ms.date: 01/10/2020
ms.openlocfilehash: de8611e53339d38a03836bc9272ad42ff88a59b8
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "79371433"
---
# <a name="quickstart-find-available-rooms-by-using-azure-digital-twins"></a>Inicio rápido: Búsqueda de salas disponibles mediante Azure Digital Twins

[!INCLUDE [digital-twins-preview-limit-alert](../../includes/digital-twins-preview-limit-alert.md)]

El servicio Azure Digital Twins le permite recrear una imagen digital de su entorno físico. Así, puede recibir notificaciones mediante eventos en su entorno y personalizar las respuestas a estos.

Esta guía de inicio rápido usa [un par de ejemplos de .NET](https://github.com/Azure-Samples/digital-twins-samples-csharp) para digitalizar un edificio de oficinas imaginario. Le muestra cómo encontrar salas disponibles en ese edificio. Con Digital Twins, puede asociar muchos sensores con el entorno. También puede averiguar si la calidad del aire de la sala disponible es óptima, con ayuda de un sensor simulado de dióxido de carbono. Una de las aplicaciones de ejemplo generará datos de sensor aleatorios para ayudarle a visualizar este escenario.

El siguiente vídeo resume la configuración del inicio rápido:

>[!VIDEO https://www.youtube.com/embed/1izK266tbMI]

## <a name="prerequisites"></a>Prerrequisitos

1. Antes de comenzar, si no tiene una cuenta de Azure, [cree una gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

1. Las dos aplicaciones de consola que se ejecutan en esta guía de inicio rápido se escriben con C#. Instale la [versión 2.1.403 del SDK de .NET Core, o cualquier versión superior](https://www.microsoft.com/net/download) en la máquina de desarrollo. Si tiene instalado el SDK de .NET Core, compruebe la versión actual de C# en la máquina de desarrollo. Ejecute `dotnet --version` en un símbolo del sistema.

1. Descargue el [proyecto de ejemplo en C#](https://github.com/Azure-Samples/digital-twins-samples-csharp/archive/master.zip). Extraiga el archivo digital-twins-samples-csharp-master.zip.

## <a name="create-a-digital-twins-instance"></a>Creación de una instancia de Digital Twins

Cree una instancia de Digital Twins en el [portal](https://portal.azure.com) mediante los siguientes pasos de esta sección.

[!INCLUDE [create-digital-twins-portal](../../includes/digital-twins-create-portal.md)]

## <a name="set-permissions-for-your-app"></a>Definición de permisos para la aplicación

Esta sección registra la aplicación de ejemplo en Azure Active Directory (Azure AD), para que pueda acceder a la instancia de Digital Twins. Si ya tiene el registro de una aplicación de Azure AD, puede reutilizarlo para el ejemplo. Asegúrese de que está configurado como se describe en esta sección.

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-permissions.md)]

## <a name="build-application"></a>Compilación de la aplicación

Compile la aplicación de ocupación mediante estos pasos.

1. Abra un símbolo del sistema. Vaya a la carpeta donde se extrajeron los archivos `digital-twins-samples-csharp-master.zip`.
1. Ejecute `cd occupancy-quickstart/src`.
1. Ejecute `dotnet restore`.
1. Edite [appSettings.json](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/appSettings.json) para actualizar las siguientes variables:
    - **ClientId**: escriba el identificador de aplicación de su registro de aplicación de Azure AD, mencionado en la sección anterior.
    - **Tenant**: escriba el identificador de directorio del inquilino de Azure AD, también indicado en la sección anterior.
    - **BaseUrl**: la dirección URL de la API de administración de la instancia de Digital Twins que estará en el siguiente formato `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`. Reemplace los marcadores de posición de esta dirección URL por los valores de la instancia de la sección anterior.

    Guarde el archivo actualizado.

## <a name="provision-graph"></a>Aprovisionamiento del gráfico

Este paso permite aprovisionar el grafo espacial de Digital Twins con:

- Varios espacios.
- Un dispositivo.
- Dos sensores.
- Una función personalizada.
- Una asignación de rol.

El grafo espacial se aprovisiona con el archivo [provisionSample.yaml](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml).

1. Ejecute `dotnet run ProvisionSample`.

    >[!NOTE]
    >Se usa la herramienta de inicio de sesión de dispositivos de la CLI de Azure para autenticar al usuario en Azure AD. El usuario debe escribir un código determinado para autenticarse mediante la página de [inicio de sesión de Microsoft](https://microsoft.com/devicelogin). Una vez que se escriba el código, siga estos pasos para autenticarse. El usuario debe autenticarse cuando se esté ejecutando la aplicación.

    >[!TIP]
    > Al ejecutar este paso, asegúrese de que las variables se han copiado correctamente si aparece siguiente el mensaje de error: `EXIT: Unexpected error: The input is not a valid Base-64 string ...`

1. El paso de aprovisionamiento puede tardar varios minutos. También aprovisiona IoT Hub en la instancia de Digital Twins. Esto se realizará en bucle hasta que la instancia de IoT Hub muestre el estado = `Running`.

    [![Aprovisione el ejemplo: Status=Running](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-provision-sample.png)](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-provision-sample.png#lightbox)

1. Al final de la ejecución, copie el valor de `ConnectionString` del dispositivo para usarlo en el ejemplo del simulador de dispositivos. Copie solo la cadena indicada en esta imagen.

    [![Copia de la cadena de conexión](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-connection-string.png)](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-connection-string.png#lightbox)

    >[!TIP]
    > Puede ver y modificar su grafo espacial con el [visor de grafos de Azure Digital Twins](https://github.com/Azure/azure-digital-twins-graph-viewer).

Mantenga abierta la ventana de la consola para usarla más adelante.

## <a name="send-sensor-data"></a>Envío de datos de sensor

Compile y ejecute la aplicación de dispositivo simulador del sensor mediante estos pasos.

1. Abra un nuevo símbolo del sistema. Vaya al proyecto que descargó en la carpeta `digital-twins-samples-csharp-master`.
1. Ejecute `cd device-connectivity`.
1. Ejecute `dotnet restore`.
1. Edite [appsettings.json](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/device-connectivity/appsettings.json) para actualizar **DeviceConnectionString** con el valor de `ConnectionString` anterior. Guarde el archivo actualizado.
1. Ejecute `dotnet run` para empezar a enviar los datos del sensor. Verá que se envían a Azure Digital Twins tal como se muestra en la siguiente imagen.

     [![Conectividad de dispositivos](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-device-connectivity.png)](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-device-connectivity.png#lightbox)

1. Vamos a dejar que se ejecute este simulador para que pueda ver los resultados en paralelo con la acción del paso siguiente. Esta ventana muestra los datos del sensor simulado que se envían a Digital Twins. En el siguiente paso se puede consultar en tiempo real las salas disponibles con aire fresco.

    >[!TIP]
    > Al ejecutar este paso, asegúrese de que `DeviceConnectionString` se ha copiado correctamente si aparece siguiente el mensaje de error: `EXIT: Unexpected error: The input is not a valid Base-64 string ...`

## <a name="find-available-spaces-with-fresh-air"></a>Búsqueda de espacios disponibles con aire fresco

El ejemplo del sensor simula los valores de datos aleatorios de dos sensores. Son el movimiento y el dióxido de carbono. En nuestro ejemplo, los espacios disponibles con aire fresco se definen en el ejemplo por la no presencia de nadie en la sala. Y también viene definida por un nivel de dióxido de carbono inferior a 1000 ppm. Si no se cumple la condición, el espacio no aparece como disponible o la calidad del aire es deficiente.

1. Abra el símbolo del sistema que se usó para ejecutar el paso de aprovisionamiento anteriormente.
1. Ejecute `dotnet run GetAvailableAndFreshSpaces`.
1. Examine este símbolo del sistema y el símbolo del sistema de los datos del sensor en paralelo.

    El símbolo del sistema de datos del sensor envía datos simulados de movimiento y dióxido de carbono a Digital Twins cada cinco segundos. El otro símbolo del sistema lee el grafo en tiempo real para encontrar las salas disponibles con aire fresco en función de los datos simulados aleatorios. Se mostrará una de estas condiciones casi en tiempo real en función de los datos del sensor que se enviaron por última vez:
   - `Room is available and air is fresh`
   - `Room is not available or air quality is poor`

     [![Obtención de espacios disponibles con aire fresco](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-get-available.png)](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-get-available.png#lightbox)

Para comprender qué ha sucedido en esta guía de inicio rápido y a qué API se ha llamado, abra [Visual Studio Code](https://code.visualstudio.com/Download) con el proyecto de área de trabajo de código que se encuentra en `digital-twins-samples-csharp`. Use el comando siguiente:

```cmd
<path>\occupancy-quickstart\src>code ..\..\digital-twins-samples.code-workspace
```

Los tutoriales incluyen un análisis más detallado del código. Le enseñan cómo modificar los datos de configuración y a qué API se llama. Para más información sobre las API de administración, vaya a la página Digital Twins Swagger:

```URL
https://YOUR_INSTANCE_NAME.YOUR_LOCATION.azuresmartspaces.net/management/swagger
```

| Nombre | Reemplazar por |
| --- | --- |
| YOUR_INSTANCE_NAME | El nombre de la instancia de Digital Twins. |
| YOUR_LOCATION | La región del servidor donde está hospedada la instancia |

O para mayor comodidad, vaya a [Digital Twins Swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger).

## <a name="clean-up-resources"></a>Limpieza de recursos

En los tutoriales se analiza en detalle cómo:

- Compilar una aplicación para que los administradores de instalaciones puedan aumentar la productividad de los ocupantes.
- Operar el edificio con mayor eficacia.

Para poder seguir con los tutoriales, no elimine los recursos que se crearon en esta guía de inicio rápido. Si no tiene pensado hacerlo, elimine todos los recursos que se crearon en esta guía de inicio rápido.

1. Elimine la carpeta que se creó al descargar el repositorio de ejemplo.
1. Seleccione **Todos los recursos** en el menú de la izquierda de [Azure Portal](https://portal.azure.com). A continuación, seleccione el recurso de Digital Twins. En la parte superior del panel **Todos los recursos**, seleccione **Eliminar**.

    > [!TIP]
    > Si tuvo problemas para eliminar una instancia de Digital Twins, se ha incorporado una actualización del servicio con la corrección. Vuelva a intentar eliminar la instancia.

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido se usaba un escenario sencillo y aplicaciones de ejemplo para mostrar cómo se puede usar Digital Twins para buscar salas con buenas condiciones de trabajo. Para un análisis detallado de este escenario, lea este tutorial:

>[!div class="nextstepaction"]
>[Tutorial: Implementación de Azure Digital Twins y configuración de un grafo espacial](tutorial-facilities-setup.md)
