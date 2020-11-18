---
title: Uso de Time Series Insights para almacenar y analizar la telemetría de dispositivos de Azure IoT Plug and Play | Microsoft Docs
description: Configure un entorno de Time Series Insights y conecte el centro de IoT para ver y analizar la telemetría de los dispositivos IoT Plug and Play.
author: lyrana
ms.author: lyhughes
ms.date: 10/14/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: aa99b9059fe8e3cd5b0dfe6f7e62bd02012fd144
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93422270"
---
# <a name="tutorial-create-and-connect-to-time-series-insights-gen2-to-store-visualize-and-analyze-iot-plug-and-play-device-telemetry"></a>Tutorial: Creación de un entorno de Time Series Insights Gen2 y conexión a este para almacenar, visualizar y analizar la telemetría de dispositivos IoT Plug and Play

En este tutorial, aprenderá a crear y configurar correctamente un entorno de [Azure Time Series Insights Gen2](https://docs.microsoft.com/azure/time-series-insights/overview-what-is-tsi) (TSI) para integrarlo con la solución IoT Plug and Play. Con TSI, puede recopilar, procesar, almacenar, consultar y visualizar datos de serie temporal a escala de Internet de las cosas (IoT).

En primer lugar, aprovisionará un entorno de TSI y conectará el centro de IoT como origen de eventos de transmisión. Luego, trabajará en la sincronización del modelo para crear el modelo de serie temporal del entorno de TSI según los archivos del modelo de ejemplo del [lenguaje de definición de gemelos digitales (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl) que usó para los dispositivos de controlador de temperatura y termostato.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

Para evitar el requisito de instalar la CLI de Azure localmente, puede usar Azure Cloud Shell para configurar los servicios en la nube.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

### <a name="time-series-id-selection"></a>Selección del identificador de serie temporal

Al aprovisionar el entorno de TSI, se le pedirá que seleccione un identificador de serie temporal. Es fundamental seleccionar el identificador de serie temporal adecuado, ya que la propiedad es inmutable y no se puede cambiar una vez establecida. Elegir un id. de serie temporal es como elegir una clave de partición para una base de datos. Normalmente, el identificador de serie temporal debe ser el nodo hoja del modelo de recursos. En otras palabras, lo habitual será seleccionar la propiedad de identificador del recurso o sensor más granular que emita la telemetría.

Como usuario de IoT Plug and Play, la pregunta pertinente para seleccionar el identificador de serie temporal es la presencia de [componentes](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md#component) en los modelos de dispositivo. 

![Selección del identificador de serie temporal](./media/tutorial-configure-tsi/ts-id-selection-pnp.png)

* Si ha estado haciendo el inicio rápido y el dispositivo de IoT Hub representa el [termostato](https://raw.githubusercontent.com/Azure/opendigitaltwins-dtdl/master/DTDL/v2/samples/Thermostat.json), use `iot-hub-connection-device-id` como identificador de serie temporal.

* Si ha estado haciendo uno de los tutoriales del elemento [TemperatureController](https://raw.githubusercontent.com/Azure/opendigitaltwins-dtdl/master/DTDL/v2/samples/TemperatureController.json) de varios componentes, use una clave compuesta en la sección siguiente, escrita como `iot-hub-connection-device-id, dt-subject`

## <a name="provision-your-azure-time-series-insights-gen2-environment"></a>Aprovisionamiento del entorno de Azure Time Series Insights Gen2

El comando siguiente funciona de esta manera:

* Crea una cuenta de almacenamiento de Azure para el [almacenamiento en frío](https://docs.microsoft.com/azure/time-series-insights/concepts-storage#cold-store) del entorno, diseñado para la retención a largo plazo y el análisis de los datos históricos.
  * Reemplace `mytsicoldstore` por un nombre único para la cuenta.
* Crea un entorno de Azure Time Series Insights Gen2, que incluye almacenamiento intermedio con un período de retención de 7 días, y almacenamiento en reposo para una retención infinita. 
  * Reemplace `my-tsi-env` por un nombre único del entorno de TSI. 
  * Reemplace `my-pnp-resourcegroup` por el nombre del grupo de recursos que usó durante la configuración.
  * Reemplace `my-ts-id-property` por el valor de propiedad del identificador de serie temporal según los criterios de selección anteriores

```azurecli-interactive
storage=mytsicoldstore
rg=my-pnp-resourcegroup
az storage account create -g $rg -n $storage --https-only
key=$(az storage account keys list -g $rg -n $storage --query [0].value --output tsv)
az timeseriesinsights environment longterm create --name my-tsi-env --resource-group $rg --time-series-id-properties my-ts-id-property --sku-name L1 --sku-capacity 1 --data-retention 7 --storage-account-name $storage --storage-management-key $key --location eastus2
```

Ahora, configurará el centro de IoT que creó anteriormente como [origen del evento](https://docs.microsoft.com/azure/time-series-insights/concepts-streaming-ingestion-event-sources) del entorno. Cuando el origen del evento esté conectado, TSI comenzará a indexar eventos desde el centro, empezando por el primer evento de la cola.

En primer lugar, cree un grupo de consumidores único en el centro de IoT para el entorno de TSI. Reemplace `my-pnp-hub` por el nombre del centro de IoT que usó anteriormente.

```azurecli-interactive
az iot hub consumer-group create --hub-name my-pnp-hub --name tsi-consumer-group 
```

Conecte el centro de IoT. Reemplace `my-pnp-resourcegroup`, `my-pnp-hub` y `my-tsi-env` por sus respectivos valores.

```azurecli-interactive
rg=my-pnp-resourcegroup
iothub=my-pnp-hub
env=my-tsi-env
es_resource_id=$(az iot hub create -g $rg -n $iothub --query id --output tsv)
shared_access_key=$(az iot hub policy list -g $rg --hub-name $iothub --query "[?keyName=='service'].primaryKey" --output tsv)
az timeseriesinsights event-source iothub create -g $rg --environment-name $env -n iot-hub-event-source --consumer-group-name tsi-consumer-group  --key-name iothubowner --shared-access-key $shared_access_key --event-source-resource-id $es_resource_id
```
Vaya al grupo de recursos en [Azure Portal](https://portal.azure.com) y seleccione el entorno de Time Series Insights recién creado. Vaya a la *dirección URL del Explorador de Time Series Insights* que se muestra en la información general de la instancia.

![Página de Información general del portal](./media/tutorial-configure-tsi/view-environment.png)

En el explorador, debería ver los dos termostatos en "All hierarchies" (Todas las jerarquías). A continuación, mantendrá el modelo de serie temporal según el modelo de dispositivo.

![Vista del explorador 1](./media/tutorial-configure-tsi/tsi-env-first-view.png)

## <a name="model-synchronization-between-digital-twins-definition-language-and-time-series-insights-gen2"></a>Sincronización del modelo entre el lenguaje de definición de gemelos digitales y Time Series Insights Gen2

Después, convertirá el modelo de dispositivo de DTDL en el modelo de recursos de Azure Time Series Insights (TSI). El modelo de serie temporal de TSI es una herramienta de modelado semántico para la contextualización de datos en TSI. El modelo de serie temporal tiene tres componentes principales:

* [Instancias del modelo de serie temporal](https://docs.microsoft.com/azure/time-series-insights/concepts-model-overview#time-series-model-instances). Las instancias son representaciones virtuales de las propias series temporales. Se identificarán de forma única mediante el identificador de serie temporal.
* [Jerarquías del modelo de serie temporal](https://docs.microsoft.com/azure/time-series-insights/concepts-model-overview#time-series-model-hierarchies). Las jerarquías organizan instancias especificando los nombres de las propiedades y sus relaciones.
* [Tipos de modelo de serie temporal](https://docs.microsoft.com/azure/time-series-insights/concepts-model-overview#time-series-model-types). Los tipos ayudan a definir [variables](https://docs.microsoft.com/azure/time-series-insights/concepts-variables) o fórmulas para realizar cálculos. Los tipos se asocian con una instancia concreta.

> [!NOTE]
> Los ejemplos siguientes son para el elemento TemperatureController de varios componentes.

### <a name="define-your-types"></a>Definición de los tipos

Puede empezar a ingerir datos en Azure Time Series Insights Gen2 sin tener un modelo predefinido. Cuando llegue la telemetría, TSI intentará resolver automáticamente las instancias de serie temporal según el valor de propiedad del identificador de serie temporal. Todas las instancias tendrán asignado el *tipo predeterminado*. Para representar los modelos, tendrá que crear manualmente un tipo. En la imagen siguiente se muestra un método sencillo para sincronizar un modelo de DTDL y un tipo de TSM:

![DTDL a tipo de TSM](./media/tutorial-configure-tsi/DTDL-to-TSM-Type.png)

* El identificador de modelo del gemelo digital (DTMI) se convierte en el identificador de tipo.
* El nombre de tipo puede ser el nombre del modelo o el nombre para mostrar.
* La descripción del modelo se convierte en la descripción del tipo.
* Se crea al menos una variable de tipo para cada componente de telemetría que tenga un esquema numérico. 
  * Solo se pueden usar tipos de datos numéricos con las variables, pero si un valor se envía como cadena analizable, por ejemplo, `"0"`, se puede usar una función de [conversión](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax#conversion-functions), como `toDouble`.
* El nombre de la variable puede ser el nombre de la telemetría o el nombre para mostrar.
* Al definir la expresión de serie temporal (TSX) de la variable, consulte el nombre de la telemetría de la conexión y su tipo de datos.

> [!NOTE]
> En este ejemplo se muestran solo dos variables: un agregado y un valor numérico, pero cada tipo puede tener hasta 100. Distintas variables pueden hacer referencia al mismo valor de telemetría para realizar diferentes cálculos según sea necesario. Para obtener la lista completa de filtros, agregados y funciones escalares, consulte la documentación de la [sintaxis de expresiones de serie temporal de Time Series Insights Gen2](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax).

Abra el editor de texto que prefiera y guarde el siguiente archivo JSON en la unidad local:

```JSON
{
  "put": [
    {
      "id": "dtmi:com:example:Thermostat;1",
      "name": "Thermostat",
      "description": "Reports current temperature and provides desired temperature control.",
      "variables": {
        "EventCount": {
          "kind": "aggregate",
          "aggregation": {
            "tsx": "count()"
          }
        },
        "Temperature": {
          "kind": "numeric",
          "value": {
            "tsx": "$event.temperature.Double"
          },
          "aggregation": {
            "tsx": "avg($value)"
          }
        }
      }
    }
  ]
}
```

En el Explorador de Time Series Insights, haga clic en el icono de modelo de la izquierda para ir a la pestaña Model (Modelo). Haga clic en **Types** (Tipos) y, luego, en **Upload JSON** (Cargar JSON):

![Cargar](./media/tutorial-configure-tsi/upload-type.png)

Seleccione **Choose file** (Elegir archivo), seleccione el archivo JSON que guardó anteriormente y haga clic en **Upload** (Cargar).

Verá el tipo de termostato recién definido.

### <a name="optional---create-a-hierarchy"></a>Opcional: creación de una jerarquía

Tiene la opción de crear una jerarquía para organizar los dos componentes de termostato que hay en el elemento primario TemeraptureController.

Haga clic en *Hierarchies* (Jerarquías) y seleccione *Add a hierarchy* (Agregar una jerarquía). Escriba `Device Fleet` como nombre y cree un nivel denominado `Device Name`; luego, haga clic en *Save* (Guardar).

![Agregar una jerarquía](./media/tutorial-configure-tsi/add-hierarchy.png)

### <a name="assign-your-instances-to-the-correct-type"></a>Asignación del tipo correcto a las instancias

A continuación, cambiará el tipo de las instancias y, opcionalmente, las situará dentro de la jerarquía.

Seleccione la pestaña *Instances* (Instancias) y haga clic en el icono *Edit* (Editar) en el extremo derecho.

![Edición de las instancias](./media/tutorial-configure-tsi/edit-instance.png)

Haga clic en la lista desplegable Type (Tipo) y seleccione `Thermostat`. 

![Cambio del tipo de la instancia](./media/tutorial-configure-tsi/change-type.png)

Si creó una jerarquía, seleccione *Instance fields* (Campos de instancia) y active la casilla `Device Fleet`. Escriba `Temperature Controller` como valor de su dispositivo primario y, luego, haga clic en *Save* (Guardar).

![Asignación a la jerarquía](./media/tutorial-configure-tsi/assign-to-hierarchy.png)

Repita los pasos anteriores con el segundo termostato.

### <a name="view-your-data"></a>Consulta de los datos

Vuelva al panel de gráficos y expanda Device Fleet y TemperatureController. Haga clic en thermostat1, seleccione la variable `Temperature` y, luego, haga clic en *Add* (Agregar) para representar el valor en el gráfico. Haga lo mismo con thermostat2.

![Cambio del tipo de instancia de thermostat2](./media/tutorial-configure-tsi/charting-values.png)

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre las distintas opciones de gráficos, incluidos el ajuste de tamaño del intervalo y los controles del eje Y, consulte la documentación del [Explorador de Azure Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/concepts-ux-panels).

* Si necesita información más detallada sobre el modelo de serie temporal de su entorno, consulte [este](https://docs.microsoft.com/azure/time-series-insights/concepts-model-overview) artículo.

* Para profundizar en las API de consulta y la sintaxis de expresiones de serie temporal, consulte [aquí](https://docs.microsoft.com/rest/api/time-series-insights/reference-query-apis).




