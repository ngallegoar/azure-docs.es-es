---
title: 'Tutorial: Implementación de un entorno de vista previa y un gráfico espacial con Azure Digital Twins | Microsoft Docs'
description: Aprenda a implementar su instancia de Azure Digital Twins y a configurar los recursos espaciales mediante los pasos de este tutorial.
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 01/10/2020
ms.openlocfilehash: 878b64fe6dd491adbb61c4c74cf4a5fc039858cd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "79371416"
---
# <a name="tutorial-deploy-azure-digital-twins-preview-and-configure-a-spatial-graph"></a>Tutorial: Implementación de la versión preliminar de Azure Digital Twins y configuración de un grafo espacial

[!INCLUDE [digital-twins-preview-limit-alert](../../includes/digital-twins-preview-limit-alert.md)]

Puede usar el servicio en versión preliminar de Azure Digital Twins para reunir personas, lugares y dispositivos en un sistema espacial coherente. En esta serie de tutoriales se muestra cómo usar Azure Digital Twins para detectar la ocupación de salas con condiciones óptimas de temperatura y calidad del aire. 

Estos tutoriales le guían a través de una aplicación de consola .NET para crear un escenario de un edificio de oficinas. El edificio tiene varias plantas y salas dentro de cada planta. Las salas contienen dispositivos, con sensores conectados, que detectan movimiento, la temperatura ambiente y la calidad del aire. 

Aprenderá a replicar las entidades y áreas físicas del edificio como objetos digitales mediante el servicio Azure Digital Twins. Simulará los eventos de los dispositivos mediante otra aplicación de consola. Luego, aprenderá a supervisar los eventos procedentes de estas entidades y áreas físicas casi en tiempo real. 

Los administradores de las oficinas pueden usar esta información para ayudar a los empleados que trabajan en el edificio a reservar salas de reuniones con condiciones óptimas. Los administradores de las instalaciones de oficina pueden usar su configuración para averiguar las tendencias de uso de las salas, y para supervisar las condiciones laborales, con el fin de ajustar el mantenimiento.

En el primer tutorial de esta serie, aprenderá a:

> [!div class="checklist"]
> * Implementar Digital Twins
> * Conceder permisos a una aplicación
> * Modificar una aplicación de ejemplo de Digital Twins
> * Aprovisionar un edificio

Estos tutoriales utilizan y modifican los mismos ejemplos que usa la [guía de inicio rápido para buscar salas disponibles](quickstart-view-occupancy-dotnet.md) para proporcionar una cobertura más detallada y exhaustiva de los conceptos.

## <a name="prerequisites"></a>Prerrequisitos

- Suscripción a Azure. Si no tiene una cuenta de Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

- SDK de .NET Core. Los ejemplos de Azure Digital Twins usados en estos tutoriales están escritos en C#. Para compilar y ejecutar el ejemplo, asegúrese de instalar la [versión 2.1.403 del SDK de .NET Core, o cualquier versión superior](https://www.microsoft.com/net/download) en la máquina de desarrollo. Para comprobar que está instalada la versión correcta en la máquina, ejecute `dotnet --version` en una ventana de comandos.

- Tiene [Visual Studio Code](https://code.visualstudio.com/) para explorar el código de ejemplo. 

## <a name="deploy-digital-twins"></a>Implementar Digital Twins

Use los pasos de esta sección para crear una nueva instancia del servicio Azure Digital Twins. Solo se puede crear una instancia por suscripción. Si ya tiene una en ejecución, vaya a la siguiente sección. 

[!INCLUDE [create-digital-twins-portal](../../includes/digital-twins-create-portal.md)]

## <a name="grant-permissions-to-your-app"></a>Concesión de permisos a una aplicación

Digital Twins usa [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) (Azure AD) para controlar el [acceso de lectura y escritura](../active-directory/develop/v2-permissions-and-consent.md) al servicio. Todas las aplicaciones que necesita para conectarse a una instancia de Digital Twins deben estar registradas en Azure AD. Los pasos de esta sección muestran cómo crear una aplicación de ejemplo.

Si ya tiene el registro de aplicaciones, puede reutilizarlo para el ejemplo. Sin embargo, en esta sección tiene la posibilidad de asegurarse de que el registro de la aplicación está configurado correctamente.

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-permissions.md)]

## <a name="configure-the-digital-twins-sample"></a>Configuración del ejemplo de Digital Twins

En esta sección recorrerá una aplicación de Azure Digital Twins que se comunica con las [API REST de Digital Twins](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index). 

### <a name="download-the-sample"></a>Descarga del ejemplo

Si ya tiene los ejemplos descargados para la [guía de inicio rápido para la búsqueda de salas disponibles](quickstart-view-occupancy-dotnet.md), puede omitir estos pasos.

1. Descargue los [ejemplos de .NET de Digital Twins](https://github.com/Azure-Samples/digital-twins-samples-csharp/archive/master.zip).
2. Extraiga el contenido de la carpeta ZIP en la máquina.

### <a name="explore-the-sample"></a>Exploración del ejemplo

En la carpeta de ejemplo extraída, abra el archivo **digital-twins-samples-csharp\digital-twins-samples.code-workspace** en Visual Studio Code. Esta solución contiene dos proyectos:

* Puede usar el ejemplo de aprovisionamiento **occupancy-quickstart** para configurar y aprovisionar un [grafo de inteligencia espacial](concepts-objectmodel-spatialgraph.md#digital-twins-object-models). Este grafo es la imagen digitalizada de los espacios físicos y de los recursos que hay en ellos. Usa un [modelo de objetos](concepts-objectmodel-spatialgraph.md#digital-twins-object-models), que define los objetos de un edificio inteligente. Para obtener una lista completa de los objetos y las API REST de Digital Twins, visite [esta documentación de la API REST](https://docs.westcentralus.azuresmartspaces.net/management/swagger) o la dirección URL de API Management que se creó para [su instancia](#deploy-digital-twins).

   Para explorar el ejemplo y entender cómo se comunica con su instancia de Digital Twins, puede comenzar con la carpeta **src\actions**. Los archivos de esta carpeta implementan los comandos que usará en estos tutoriales:
    - El archivo **provisionSample.cs** muestra cómo aprovisionar el grafo espacial.
    - El archivo **getSpaces.cs** obtiene información sobre los espacios aprovisionados.
    - El archivo **getAvailableAndFreshSpaces.cs** obtiene los resultados de una función personalizada llamada función definida por el usuario.
    - El archivo **createEndpoints.cs** crea puntos de conexión que interactúan con otros servicios.

* El ejemplo de simulación **device-connectivity** simula los datos del sensor y los envía a la instancia de IoT Hub aprovisionada para la instancia de Digital Twins. Este ejemplo se usará en [el siguiente tutorial después de aprovisionar su grafo espacial](tutorial-facilities-udf.md#simulate-sensor-data). Los identificadores de sensores y dispositivos que se usan para configurar este ejemplo deben ser iguales a los que usará para aprovisionar su grafo.

### <a name="configure-the-provisioning-sample"></a>Configuración del ejemplo de aprovisionamiento

1. Abra una ventana de comandos y vaya al ejemplo descargado. Ejecute el siguiente comando:

    ```cmd/sh
    cd occupancy-quickstart/src
    ```

1. Restaure las dependencias en el proyecto de ejemplo mediante la ejecución de este comando:

    ```cmd/sh
    dotnet restore
    ```

1. En Visual Studio Code, abra el archivo [appSettings.json](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/appSettings.json) en el proyecto **occupancy-quickstart**. Actualice los siguientes valores:
   * **ClientId**: Escriba el identificador de aplicación de su registro de aplicaciones de Azure AD. Este identificador lo anotó en la sección donde se [establecen los permisos de aplicación](#grant-permissions-to-your-app).
   * **Tenant**: Escriba el identificador de directorio de su [inquilino de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant). También anotó este identificador en la sección donde se [establecen los permisos de aplicación](#grant-permissions-to-your-app).
   * **BaseUrl**: Escriba la dirección URL de la instancia de Digital Twins. Para obtener esta dirección URL, sustituya los marcadores de posición de esta por los valores de su instancia: `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`. Para obtener esta dirección URL, también puede modificar la dirección URL de Management API en la [sección de implementación](#deploy-digital-twins). Reemplace **swagger/** por **api/v1.0/** .

1. Revise una lista de las características de Digital Twins que puede explorar mediante el ejemplo. Ejecute el siguiente comando:

    ```cmd/sh
    dotnet run
    ```

## <a name="understand-the-provisioning-process"></a>Descripción del proceso de aprovisionamiento

En esta sección se muestra la forma en que el ejemplo aprovisiona un grafo espacial de un edificio.

En Visual Studio Code, vaya a la carpeta **occupancy-quickstart\src\actions** y abra el archivo **provisionSample.cs**. Anote la siguiente función:

```csharp
public static async Task<IEnumerable<ProvisionResults.Space>> ProvisionSample(HttpClient httpClient, ILogger logger)
{
    IEnumerable<SpaceDescription> spaceCreateDescriptions;
    using (var r = new StreamReader("actions/provisionSample.yaml"))
    {
        spaceCreateDescriptions = await GetProvisionSampleTopology(r);
    }

    var results = await CreateSpaces(httpClient, logger, spaceCreateDescriptions, Guid.Empty);

    Console.WriteLine($"Completed Provisioning: {JsonConvert.SerializeObject(results, Formatting.Indented)}");

    return results;
}
```

Esta función usa el archivo [provisionSample.yaml](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml) de la misma carpeta. Abra este archivo y anote la jerarquía de un edificio de oficinas: *Venue*, *Floor*, *Area* y *Rooms*. Todos estos espacios físicos pueden contener *dispositivos* y *sensores*. Cada entrada tiene un valor de `type`&mdash; predefinido, por ejemplo, Floor o Room.

El archivo **yaml** del ejemplo muestra un grafo espacial que usa el modelo de objetos de Digital Twins `Default`. Este modelo proporciona nombres genéricos para la mayoría de los tipos. Los nombres genéricos son suficientes para un edificio. Algunos ejemplos son Temperature para SensorDataType y Map para SpaceBlobType. Un tipo de espacio de ejemplo es Room con los subtipos FocusRoom, ConferenceRoom, etc. 

Si tuviera que crear un grafo espacial para una ubicación de un tipo diferente, como por ejemplo una fábrica, es posible que necesite un otro modelo de objetos. Para averiguar los modelos que están disponibles para su uso, ejecute el comando `dotnet run GetOntologies` en la línea de comandos del ejemplo de aprovisionamiento. 

Para más información sobre los grafos espaciales y los modelos de objetos, lea [Descripción de los modelos de objetos de Digital Twins y del grafo de inteligencia espacial](concepts-objectmodel-spatialgraph.md).

### <a name="modify-the-sample-spatial-graph"></a>Modificación del grafo espacial de ejemplo

El archivo **provisionSample.yaml** contiene los siguientes nodos:

- **resources**: El nodo `resources` crea un recurso de Azure IoT Hub para comunicarse con los dispositivos en la instalación. Una instancia de IoT Hub en el nodo raíz del grafo puede comunicarse con todos los dispositivos y sensores del grafo.  

- **spaces**: En el modelo de objetos de Digital Twins `spaces` representa las ubicaciones físicas. Cada espacio tiene un valor de `Type`&mdash;, por ejemplo, Region, Venue o Customer&mdash;, y un nombre descriptivo en `Name`. Los espacios pueden pertenecer a otros espacios, lo que crea una estructura jerárquica. El archivo provisionSample.yaml tiene un grafo espacial de un edificio imaginario. Observe el anidamiento lógico de los espacios del tipo `Floor` dentro de `Venue`, `Area` en una planta y los nodos de `Room` en un área. 

- **devices**: Los espacios pueden contener `devices`, que son entidades físicas o virtuales que administran varios sensores. Por ejemplo, un dispositivo puede ser el teléfono de un usuario, un pod de un sensor de Raspberry Pi o una puerta de enlace. En el edificio imaginario del ejemplo, observe que la sala llamada **Focus Room** contiene un dispositivo **Raspberry Pi 3 A1**. Cada nodo de dispositivo se identifica mediante un valor de `hardwareId` único, que está codificado de forma rígida en el ejemplo. Para configurar este ejemplo para una producción real, reemplácelos por los valores de su configuración.  

- **sensors**: Un dispositivo puede contener varios valores de `sensors`. Estos sensores pueden detectar y registrar cambios físicos, como temperatura, movimiento y nivel de batería. Cada nodo de sensor se identifica de forma única mediante `hardwareId`, que aquí está codificado de forma rígida. En una aplicación real, reemplácelos por los identificadores únicos de los sensores de su configuración. El archivo provisionSample.yaml tiene dos sensores para registrar los valores de *Motion* y *CarbonDioxide*. Para agregar otro sensor para registrar el valor de *Temperature*, agregue las líneas siguientes debajo de las líneas del sensor CarbonDioxide. Estas líneas se proporcionan en provisionSample.yaml como líneas comentadas. Puede quitarles las marcas de comentario si elimina el carácter `#` que aparece delante de cada línea. 

    ```yaml
            - dataType: Temperature
              hardwareId: SAMPLE_SENSOR_TEMPERATURE
    ```
    > [!NOTE]
    > Asegúrese de que las claves `dataType` y `hardwareId` se alinean con las instrucciones anteriores a este fragmento de código. Asegúrese también de que el editor no reemplaza los espacios por caracteres de tabulación. 

Guarde y cierre el archivo provisionSample.yaml. En el siguiente tutorial, agregará más información a este archivo y, después, aprovisionará el edificio de ejemplo de Azure Digital Twins.

> [!TIP]
> Puede ver y modificar su grafo espacial con el [visor de grafos de Azure Digital Twins](https://github.com/Azure/azure-digital-twins-graph-viewer).

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere dejar de explorar Azure Digital Twins en este punto, elimine los recursos creados en este tutorial:

1. En el menú izquierdo de [Azure Portal](https://portal.azure.com), seleccione **Todos los recursos**, seleccione el grupo de recurso de Digital Twins y haga clic en **Eliminar**.

    > [!TIP]
    > Si tiene problemas al eliminar una instancia de Digital Twins, se ha incorporado una actualización del servicio con la corrección. Vuelva a intentar eliminar la instancia.

1. Si es necesario, elimine la aplicación de ejemplo en la máquina de trabajo.

## <a name="next-steps"></a>Pasos siguientes

Para aprender a implementar una lógica personalizada para supervisar las condiciones del edificio de ejemplo, vaya al siguiente tutorial de la serie. 
> [!div class="nextstepaction"]
> [Tutorial: Aprovisionamiento del edificio y supervisión de las condiciones de trabajo](tutorial-facilities-udf.md)