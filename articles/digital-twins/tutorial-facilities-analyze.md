---
title: 'Tutorial: Análisis de eventos de Time Series Insights con Azure Digital Twins | Microsoft Docs'
description: Siga los pasos de este tutorial para aprender a visualizar y analizar eventos de los espacios de Azure Digital Twins con Azure Time Series Insights.
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 01/10/2020
ms.openlocfilehash: 38bd1755ed87050cf8b91a0a82f6e5f1d2af9db5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "75933842"
---
# <a name="tutorial-visualize-and-analyze-events-from-azure-digital-twins-by-using-time-series-insights"></a>Tutorial: Visualización y análisis de eventos de Azure Digital Twins mediante Time Series Insights

Después de implementar la instancia de Azure Digital Twins, aprovisionar los espacios e implementar una función personalizada para supervisar condiciones concretas, puede visualizar los eventos y los datos procedentes de los espacios para buscar tendencias y anomalías.

En [el primer tutorial](tutorial-facilities-setup.md), configuró el grafo espacial de un edificio imaginario con una sala que contiene sensores de movimiento, temperatura y dióxido de carbono. En [el segundo tutorial](tutorial-facilities-udf.md), aprovisionó el grafo y una función definida por el usuario. La función supervisa estos valores del sensor y desencadena notificaciones cuando se dan las condiciones adecuadas. Es decir, la sala está vacía y los niveles de temperatura y dióxido de carbono son normales.

En este tutorial se muestra cómo puede integrar las notificaciones y los datos procedentes de la configuración de Azure Digital Twins con Azure Time Series Insights. Luego, puede visualizar los valores del sensor con el paso del tiempo. Puede buscar tendencias, como qué sala se usa más y cuáles son las horas más ocupadas del día. También puede detectar anomalías como qué salas están más cargadas y calientes o, si un área del edificio está enviando constantemente valores de temperatura alta que indican que el aire acondicionado está defectuoso.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Transmitir datos mediante Azure Event Hubs
> * Analizar con Time Series Insights

## <a name="prerequisites"></a>Prerrequisitos

En este tutorial se supone que ha [configurado](tutorial-facilities-setup.md) y [aprovisionado](tutorial-facilities-udf.md) Azure Digital Twins. Antes de continuar, asegúrese de que:

- Una [cuenta de Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Hay una instancia de Digital Twins en ejecución.
- Los [ejemplos en C# de Digital Twins](https://github.com/Azure-Samples/digital-twins-samples-csharp) se han descargado y extraído en su equipo de trabajo.
- [Versión 2.1.403 del SDK de .NET Core, o cualquier versión superior](https://www.microsoft.com/net/download) en la máquina de desarrollo para ejecutar el ejemplo. Ejecute `dotnet --version` para comprobar que está instalada la versión correcta.

> [!TIP]
> Use un nombre de instancia de Digital Twins único si está aprovisionando una nueva instancia.

## <a name="stream-data-by-using-event-hubs"></a>Transmisión de datos mediante Event Hubs

Puede usar el servicio [Event Hubs](../event-hubs/event-hubs-about.md) para crear una canalización para transmitir sus datos. En esta sección se muestra cómo crear un centro de eventos como conector entre las instancias de Azure Digital Twins y Time Series Insights.

### <a name="create-an-event-hub"></a>Creación de un centro de eventos

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. En el panel izquierdo, seleccione **Crear un recurso**.

1. Busque **Event Hubs** y selecciónelo. Seleccione **Crear**.

    [![Creación de un espacio de nombres de Event Hubs](./media/tutorial-facilities-analyze/tutorial-create-event-hubs.png)](./media/tutorial-facilities-analyze/tutorial-create-event-hubs.png#lightbox)

1. Escriba un **nombre** para el espacio de nombres de Event Hubs. Elija **Estándar** en **Plan de tarifa**, la **suscripción**, el **grupo de recursos** que usó para la instancia de Digital Twins y la **ubicación**. Seleccione **Crear**.

1. En la implementación del espacio de nombres de Event Hubs, seleccione el panel **Introducción** y después seleccione **Ir al recurso**.

    [![Espacio de nombres de Event Hubs después de la implementación](./media/tutorial-facilities-analyze/tutorial-event-hub-ns.png)](./media/tutorial-facilities-analyze/tutorial-event-hub-ns.png#lightbox)

1. En el panel **Información general** del espacio de nombres de Event Hubs, haga clic en el botón **Centro de eventos** de la parte superior.
    [![Botón Centro de eventos](./media/tutorial-facilities-analyze/tutorial-create-event-hub.png)](./media/tutorial-facilities-analyze/tutorial-create-event-hub.png#lightbox)

1. Escriba el **nombre** del centro de eventos y haga clic en **Crear**.

   Una vez implementado el centro de eventos, aparece en el panel **Event Hubs** del espacio de nombres de Event Hubs con un estado **Activo**. Seleccione este centro de eventos para abrir su panel **Información general**.

1. Haga clic en el botón **Grupo de consumidores** de la parte superior y escriba un nombre como **tsievents** para el grupo de consumidores. Seleccione **Crear**.

    [![Grupo de consumidores de Event Hubs](./media/tutorial-facilities-analyze/event-hub-consumer-group.png)](./media/tutorial-facilities-analyze/event-hub-consumer-group.png#lightbox)

   Una vez creado el grupo de consumidores, aparece en la lista de la parte inferior del panel **Información general** del centro de eventos.

1. Abra el panel **Directivas de acceso compartido** del centro de eventos y haga clic en el botón **Agregar**. Escriba **ManageSend** como nombre de la directiva, asegúrese de que todas las casillas estén activadas y seleccione **Crear**.

    [![Cadenas de conexión de un centro de eventos](./media/tutorial-facilities-analyze/event-hub-connection-strings.png)](./media/tutorial-facilities-analyze/event-hub-connection-strings.png#lightbox)

    > [!TIP]
    > Compruebe que va a crear una directiva de SAS para la instancia de Event Hub en lugar de su espacio de nombres.

1. Abra la directiva **ManageSend** que ha creado y copie los valores de **Cadena de conexión: clave principal** y **Cadena de conexión: clave secundaria** en un archivo temporal. Necesitará estos valores para crear un punto de conexión para el centro de eventos en la sección siguiente.

### <a name="create-an-endpoint-for-the-event-hub"></a>Creación de un punto de conexión para el centro de eventos

1. En la ventana de comandos, asegúrese de que se encuentra en la carpeta **occupancy-quickstar\src** del ejemplo de Azure Digital Twins.

1. Abra el archivo **actions\createEndpoints.yaml** en el editor. Reemplace el contenido por el siguiente:

    ```yaml
    - type: EventHub
      eventTypes:
      - SensorChange
      - SpaceChange
      - TopologyOperation
      - UdfCustom
      connectionString: Primary_connection_string_for_your_event_hub
      secondaryConnectionString: Secondary_connection_string_for_your_event_hub
      path: Name_of_your_Event_Hub
    - type: EventHub
      eventTypes:
      - DeviceMessage
      connectionString: Primary_connection_string_for_your_event_hub
      secondaryConnectionString: Secondary_connection_string_for_your_event_hub
      path: Name_of_your_Event_Hub
    ```

1. Reemplace los marcadores de posición `Primary_connection_string_for_your_event_hub` por el valor de **Cadena de conexión: clave principal** del centro de eventos. Asegúrese de que el formato de esta cadena de conexión sea como este:

   ```ConnectionString
   Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey1GUID;EntityPath=nameOfYourEventHub
   ```

1. Reemplace los marcadores de posición `Secondary_connection_string_for_your_event_hub` por el valor de **Cadena de conexión: clave secundaria** del centro de eventos. Asegúrese de que el formato de esta cadena de conexión sea como este: 

   ```ConnectionString
   Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey2GUID;EntityPath=nameOfYourEventHub
   ```

1. Reemplace los marcadores de posición `Name_of_your_Event_Hub` por el nombre de su centro de eventos.

    > [!IMPORTANT]
    > Escriba todos los valores sin comillas. Asegúrese de que hay al menos un carácter de espacio después de los signos de dos puntos en el archivo YAML. También puede validar el contenido del archivo YAML mediante cualquier control de servidor de validación de YAML, como [esta herramienta](https://onlineyamltools.com/validate-yaml).

1. Guarde y cierre el archivo. Ejecute el comando siguiente en la ventana de comandos, e inicie sesión con su cuenta de Azure cuando se le solicite.

    ```cmd/sh
    dotnet run CreateEndpoints
    ```

   Este comando crea dos puntos de conexión para el centro de eventos.

   [![Puntos de conexión de Event Hubs](./media/tutorial-facilities-analyze/dotnet-create-endpoints.png)](./media/tutorial-facilities-analyze/dotnet-create-endpoints.png#lightbox)

## <a name="analyze-with-time-series-insights"></a>Análisis con Time Series Insights

1. En el panel izquierdo de [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso**. 

1. Busque y seleccione un recurso de disponibilidad general (GA) de **Time Series Insights**. Seleccione **Crear**.

1. Escriba el **nombre** de la instancia de Time Series Insights seleccione su **suscripción**. Seleccione el **grupo de recursos** que usó o para su instancia de Digital Twins y su **ubicación**. Seleccione **Siguiente: Origen del evento** o la pestaña **Origen del evento**.

    [![Selecciones para crear una instancia de Time Series Insights](./media/tutorial-facilities-analyze/tutorial-create-tsi-environment.png)](./media/tutorial-facilities-analyze/tutorial-create-tsi-environment.png#lightbox)

1. En la pestaña **Origen del evento**, escriba un **nombre**, seleccione **Centro de eventos** como **Tipo de origen** y asegúrese de que los demás valores se seleccionan correctamente para hacer referencia al centro de eventos que creó. Seleccione **ManageSend** en **Nombre de la directiva de acceso del centro de eventos** y, después, seleccione el grupo de consumidores que creó en la sección anterior en **Grupo de consumidores del Centro de eventos**. Seleccione **Revisar + crear**.

    [![Selecciones para la creación de un origen de eventos](./media/tutorial-facilities-analyze/tutorial-tsi-event-source.png)](./media/tutorial-facilities-analyze/tutorial-tsi-event-source.png#lightbox)

1. En el panel **Revisar y crear**, revise la información que ha especificado y seleccione **Crear**.

1. En el panel de la implementación, seleccione el recurso de Time Series Insights que ha creado. Se abre el panel **Introducción** para el entorno de Time Series Insights.

1. Seleccione el botón **Ir al entorno**, situado en la parte superior. Si recibe una advertencia sobre el acceso a los datos, abra el panel **Data Access Policies** (Directivas de acceso de datos) de la instancia de Time Series Insights, haga clic en **Add** (Agregar), seleccione **Contributor** (Colaborador) como rol y seleccione el usuario apropiado.

1. El botón **Go to Environment** (Ir al entorno) abre el [explorador de Time Series Insights](../time-series-insights/time-series-insights-explorer.md). Si no se muestra ningún evento, simule eventos del dispositivo; para ello, vaya al proyecto **device-connectivity** del ejemplo de Digital Twins y ejecute `dotnet run`.

1. Cuando se hayan generado algunos eventos simulados, vuelva al explorador de Time Series Insights y haga clic en el botón de actualización de la parte superior. Verá que se están creando gráficos analíticos para los datos de los sensores simulados. 

    [![Gráfico del explorador de Time Series Insights](./media/tutorial-facilities-analyze/tsi-explorer-with-adt-telemetry.png)](./media/tutorial-facilities-analyze/tsi-explorer-with-adt-telemetry.png#lightbox)

1. En el explorador de Time Series Insights, puede generar gráficos y mapas térmicos para diferentes eventos y datos de las salas, los sensores y otros recursos. En el lado izquierdo, use los cuadros desplegables **MEASURE** (MEDIDA) y **SPLIT BY** (DIVIDIR POR) para crear sus propias visualizaciones. 

   Por ejemplo, seleccione **Events** (Eventos) en **MEASURE** (MEDIDA) y **DigitalTwins-SensorHardwareId** en **SPLIT BY** (DIVIDIDO POR), para generar un mapa térmico para cada uno de los sensores. El mapa térmico será similar a la siguiente imagen:

   [![Mapa térmico del explorador de Time Series Insights](./media/tutorial-facilities-analyze/tsi-explorer-heatmap.png)](./media/tutorial-facilities-analyze/tsi-explorer-heatmap.png#lightbox)

## <a name="clean-up-resources"></a>Limpieza de recursos

Si desea dejar de explorar Azure Digital Twins en este punto, elimine los recursos creados en este tutorial:

1. En el menú izquierdo de [Azure Portal](https://portal.azure.com), haga clic en **Todos los recursos**, seleccione el grupo de recurso de Digital Twins y haga clic en **Eliminar**.

    > [!TIP]
    > Si tiene problemas al eliminar una instancia de Digital Twins, se ha incorporado una actualización del servicio con la corrección. Vuelva a intentar eliminar la instancia.

2. Si es necesario, elimine las aplicaciones de ejemplo en la máquina de trabajo.

## <a name="next-steps"></a>Pasos siguientes

Vaya al siguiente artículo para aprender más sobre los grafos de inteligencia espacial y los modelos de objetos en Azure Digital Twins.

> [!div class="nextstepaction"]
> [Modelos de objetos de Digital Twins y grafo de inteligencia espacial](concepts-objectmodel-spatialgraph.md)
