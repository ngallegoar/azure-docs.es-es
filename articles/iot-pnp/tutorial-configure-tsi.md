---
title: Uso de Time Series Insights para almacenar y analizar la telemetría de dispositivos de Azure IoT Plug and Play | Microsoft Docs
description: Configure un entorno de Time Series Insights y conecte el centro de IoT para ver y analizar los datos de telemetría de los dispositivos IoT Plug and Play.
author: lyrana
ms.author: lyhughes
ms.date: 10/14/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: ca2319a78fb4c0c720a21e97944d5b75ada9d008
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96015033"
---
# <a name="preview-tutorial-create-and-connect-to-time-series-insights-gen2-to-store-visualize-and-analyze-iot-plug-and-play-device-telemetry"></a>Tutorial de la versión preliminar: Creación de un entorno de Time Series Insights Gen2 y conexión a este para almacenar, visualizar y analizar la telemetría de dispositivos IoT Plug and Play

En este tutorial, aprenderá a crear y configurar correctamente un entorno de [Azure Time Series Insights Gen2](https://docs.microsoft.com/azure/time-series-insights/overview-what-is-tsi) (TSI) para integrarlo con su solución IoT Plug and Play. Con TSI, puede recopilar, procesar, almacenar, consultar y visualizar datos de serie temporal en la escala de Internet de las cosas (IoT).

En primer lugar, se aprovisiona un entorno de TSI y se conecta el centro de IoT como origen de eventos de transmisión. Luego, se trabaja en la sincronización del modelo para crear el [modelo de serie temporal](../time-series-insights/concepts-model-overview.md) según los archivos del modelo de ejemplo del [lenguaje de definición de Digital Twins (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl) que se usaron para los dispositivos de controlador de temperatura y termostato.

> [!NOTE]
> Esta integración está en versión preliminar. El modo en que los modelos de dispositivos DTDL se asignan al modelo de serie temporal puede cambiar.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

En este punto, debe tener:

* Un centro de Azure IoT.
* Una instancia de DPS vinculada a su centro de IoT, con una inscripción de dispositivo individual para el dispositivo IoT Plug and Play.
* Una conexión al centro de IoT desde un dispositivo de un solo componente o de varios componentes, que transmite datos simulados.

Para evitar el requisito de instalar la CLI de Azure localmente, puede usar Azure Cloud Shell para configurar los servicios en la nube.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prepare-your-event-source"></a>Preparación del origen del evento

El centro de IoT que creó anteriormente será el [origen de eventos](https://docs.microsoft.com/azure/time-series-insights/concepts-streaming-ingestion-event-sources) del entorno de TSI.

> [!IMPORTANT]
> Deshabilite las rutas de IoT Hub existentes. Existe un problema conocido que surge cuando se usa un centro de IoT como origen de eventos de TSI con [enrutamiento](../iot-hub/iot-hub-devguide-messages-d2c.md#routing-endpoints) configurado. Deshabilite temporalmente los puntos de conexión de enrutamiento; cuando el centro de IoT esté conectado a TSI, podrá volver a habilitarlos.

Cree un grupo de consumidores único en su centro de IoT para que lo consuma TSI. Reemplace `my-pnp-hub` por el nombre del centro de IoT que usó anteriormente:

```azurecli-interactive
az iot hub consumer-group create --hub-name my-pnp-hub --name tsi-consumer-group
```

## <a name="choose-your-time-series-id"></a>Selección del identificador de serie temporal

Al aprovisionar el entorno de TSI, debe seleccionar un *identificador de serie temporal*. Es importante seleccionar el identificador de serie temporal adecuado, ya que esta propiedad es inmutable y no se puede cambiar una vez establecida. Un identificador de serie temporal es como una clave de partición de base de datos. El identificador de serie temporal actúa como clave principal para el modelo de serie temporal. Para más información, consulte [Procedimientos recomendados al elegir un id. de serie temporal](../time-series-insights/how-to-select-tsid.md).

Como usuario de IoT Plug and Play, especifique una _clave compuesta_ que conste de `iothub-connection-device-id` y `dt-subject` como identificador de serie temporal. IoT Hub agrega estas propiedades del sistema, que contienen el identificador del dispositivo IoT Plug and Play y los nombres de componentes del dispositivo, respectivamente.

Aunque los modelos de dispositivo IoT Plug and Play no usan actualmente componentes, debe incluir `dt-subject` como parte de una clave compuesta para poder usarlos en el futuro. Dado que el identificador de serie temporal es inmutable, Microsoft recomienda habilitar esta opción como previsión en caso de que la necesite en el futuro.

> [!NOTE]
> Los ejemplos siguientes corresponden al dispositivo **TemperatureController** de varios componentes, pero los conceptos son los mismos para el dispositivo **Thermostat**, que no tiene componentes.

## <a name="provision-your-tsi-environment"></a>Aprovisionamiento del entorno de TSI

En esta sección se describe cómo aprovisionar el entorno de Azure Time Series Insights Gen2.

El siguiente comando:

* Crea una cuenta de almacenamiento de Azure para el [almacenamiento en frío](https://docs.microsoft.com/azure/time-series-insights/concepts-storage#cold-store) del entorno, diseñado para la retención a largo plazo y el análisis de los datos históricos.
  * Sustituya `mytsicoldstore` por un nombre único para la cuenta de almacenamiento en frío.
* Crea un entorno de Azure Time Series Insights Gen2, que incluye almacenamiento intermedio, con un período de retención de siete días, y almacenamiento en frío, para una retención infinita.
  * Reemplace `my-tsi-env` por un nombre único para el entorno de TSI.
  * Reemplace `my-pnp-resourcegroup` por el nombre del grupo de recursos que usó durante la configuración.
  * `iothub-connection-device-id, dt-subject` es la propiedad del identificador de serie temporal.

```azurecli-interactive
storage=mytsicoldstore
rg=my-pnp-resourcegroup
az storage account create -g $rg -n $storage --https-only
key=$(az storage account keys list -g $rg -n $storage --query [0].value --output tsv)
az timeseriesinsights environment longterm create --name my-tsi-env --resource-group $rg --time-series-id-properties iothub-connection-device-id, dt-subject --sku-name L1 --sku-capacity 1 --data-retention 7 --storage-account-name $storage --storage-management-key $key --location eastus2
```

Conecte el origen del evento de IoT Hub. Reemplace `my-pnp-resourcegroup`, `my-pnp-hub` y `my-tsi-env` por los valores que eligió. El comando siguiente hace referencia al grupo de consumidores de TSI que creó anteriormente:

```azurecli-interactive
rg=my-pnp-resourcegroup
iothub=my-pnp-hub
env=my-tsi-env
es_resource_id=$(az iot hub create -g $rg -n $iothub --query id --output tsv)
shared_access_key=$(az iot hub policy list -g $rg --hub-name $iothub --query "[?keyName=='service'].primaryKey" --output tsv)
az timeseriesinsights event-source iothub create -g $rg --environment-name $env -n iot-hub-event-source --consumer-group-name tsi-consumer-group  --key-name iothubowner --shared-access-key $shared_access_key --event-source-resource-id $es_resource_id
```

Vaya al grupo de recursos en [Azure Portal](https://portal.azure.com) y seleccione el nuevo entorno de Time Series Insights. Vaya a la *dirección URL del Explorador de Time Series Insights* que se muestra en la información general de la instancia:

![Página de Información general del portal](./media/tutorial-configure-tsi/view-environment.png)

En el explorador, verá tres instancias:

* &lt;la identidad del dispositivo PnP&gt;, thermostat1
* &lt;la identidad del dispositivo PnP&gt;, thermostat2
* &lt;la identidad del dispositivo PnP&gt;, `null`

> [!NOTE]
> La tercera etiqueta representa la telemetría del propio dispositivo **TemperatureController**, como el espacio de trabajo de la memoria del dispositivo. Dado que se trata de una propiedad de nivel superior, el valor del nombre del componente es NULL. En un paso posterior, lo actualizará a un nombre más descriptivo.

![Vista del explorador 1](./media/tutorial-configure-tsi/tsi-env-first-view.png)

## <a name="configure-model-translation"></a>Configuración de la conversión del modelo

A continuación, convertirá el modelo de dispositivo de DTDL en el modelo de recursos de Azure Time Series Insights (TSI). El modelo de serie temporal de TSI es una herramienta de modelado semántico para la contextualización de datos en TSI. El modelo de serie temporal tiene tres componentes principales:

* [Instancias del modelo de serie temporal](../time-series-insights/concepts-model-overview.md#time-series-model-instances). Las instancias son representaciones virtuales de las propias series temporales. Las instancias se identifican de forma única mediante el identificador de serie temporal.
* [Jerarquías del modelo de serie temporal](../time-series-insights/concepts-model-overview.md#time-series-model-hierarchies). Las jerarquías organizan instancias especificando los nombres de las propiedades y sus relaciones.
* [Tipos de modelo de serie temporal](../time-series-insights/concepts-model-overview.md#time-series-model-types). Los tipos ayudan a definir [variables](../time-series-insights/concepts-variables.md) o fórmulas para cálculos. Los tipos se asocian con una instancia concreta.

### <a name="define-your-types"></a>Definición de los tipos

Puede empezar a ingerir datos en Azure Time Series Insights Gen2 sin tener un modelo predefinido. Cuando llegan los datos de telemetría, TSI intenta resolver automáticamente las instancias de serie temporal en función de los valores de propiedad del identificador de serie temporal. Todas las instancias tienen asignado el *tipo predeterminado*. Debe crear manualmente un nuevo tipo para clasificarlas correctamente. Los detalles siguientes muestran el método más sencillo para sincronizar los modelos DTDL del dispositivo con los tipos del modelo de serie temporal:

* El identificador de modelo del gemelo digital se convierte en el identificador de tipo.
* El nombre de tipo puede ser el nombre del modelo o el nombre para mostrar.
* La descripción del modelo se convierte en la descripción del tipo.
* Se crea al menos una variable de tipo para cada dato de telemetría que tenga un esquema numérico.
  * Solo se pueden usar tipos de datos numéricos con las variables, pero si un valor se envía como otro tipo que se puede convertir (por ejemplo, `"0"`), se puede usar una función de [conversión](/rest/api/time-series-insights/reference-time-series-expression-syntax.md#conversion-functions), como `toDouble`.
* El nombre de la variable puede ser el nombre de la telemetría o el nombre para mostrar.
* Cuando defina la expresión de serie temporal de la variable, consulte el nombre de la telemetría en la conexión y su tipo de datos.

| JSON de DTDL | JSON de tipo de modelo de serie temporal | Valor de ejemplo |
|-----------|------------------|-------------|
| `@id` | `id` | `dtmi:com:example:TemperatureController;1` |
| `displayName`    | `name`   |   `Temperature Controller`  |
| `description`  |  `description`  |  `Device with two thermostats and remote reboot.` |  
|`contents` (matriz)| `variables` (objeto)  | Consulte el ejemplo siguiente

![DTDL a tipo de modelo de serie temporal](./media/tutorial-configure-tsi/DTDL-to-TSM-Type.png)

> [!NOTE]
> En este ejemplo se muestran tres variables, pero cada tipo puede tener hasta 100. Distintas variables pueden hacer referencia al mismo valor de telemetría para realizar diferentes cálculos, según sea necesario. Para obtener la lista completa de filtros, agregados y funciones escalares, consulte la [sintaxis de expresiones de serie temporal de Time Series Insights Gen2](/rest/api/time-series-insights/reference-time-series-expression-syntax.md).

Abra un editor de texto y guarde el siguiente código JSON en la unidad local:

```JSON
{
  "put": [
    {
      "id": "dtmi:com:example:TemperatureController;1",
      "name": "Temperature Controller",
      "description": "Device with two thermostats and remote reboot.",
      "variables": {
        "workingSet": {
          "kind": "numeric",
          "value": {
            "tsx": "coalesce($event.workingSet.Long, toLong($event.workingSet.Double))"
          }, 
          "aggregation": {
            "tsx": "avg($value)"
          }
        },
        "temperature": {
          "kind": "numeric",
          "value": {
            "tsx": "coalesce($event.temperature.Long, toLong($event.temperature.Double))"
          },
          "aggregation": {
            "tsx": "avg($value)"
          }
        },
        "eventCount": {
          "kind": "aggregate",
          "aggregation": {
            "tsx": "count()"
          }
        }
      }
    }
  ]
}
```

En el Explorador de Time Series Insights, seleccione el icono de modelo de la izquierda para ir a la pestaña **Modelo**. Seleccione **Tipos** y, a continuación, seleccione **Cargar JSON**:

![Cargar](./media/tutorial-configure-tsi/upload-type.png)

Seleccione **Elegir archivo**, después el archivo JSON que guardó anteriormente y, por último, **Cargar**.

Verá el tipo **Temperature Controller** que se acaba de definir.

### <a name="create-a-hierarchy"></a>Creación de una jerarquía

Cree una jerarquía para organizar las etiquetas bajo su dispositivo **TemperatureController** primario. Este sencillo ejemplo que se incluye a continuación tiene un solo nivel, pero las soluciones de IoT suelen tener muchos niveles de anidamiento, para contextualizar las etiquetas en su posición física y semántica dentro de una organización.

Seleccione **Jerarquías** y **Agregar jerarquía**. Escriba *Device Fleet* (Flota de dispositivos) como nombre y cree un nivel denominado *Device Name* (Nombre de dispositivo). Luego seleccione **Guardar**.

![Agregar una jerarquía](./media/tutorial-configure-tsi/add-hierarchy.png)

### <a name="assign-your-instances-to-the-correct-type"></a>Asignación del tipo correcto a las instancias

A continuación, debe cambiar el tipo de las instancias y colocarlas en la jerarquía.

Seleccione la pestaña **Instancias**, busque la instancia que representa el espacio de trabajo del dispositivo y seleccione el icono **Editar** en el extremo derecho:

![Edición de las instancias](./media/tutorial-configure-tsi/edit-instance.png)

Seleccione la lista desplegable **Tipo** y en ella **Temperature Controller**. Escriba *defaultComponent, <your device name>* para actualizar el nombre de la instancia que representa todas las etiquetas de nivel superior asociadas con el dispositivo.

![Cambio del tipo de la instancia](./media/tutorial-configure-tsi/change-type.png)

Antes de seleccionar Guardar, seleccione la pestaña **Campos de instancia** y active la casilla **Device Fleet** (Flota de dispositivos). Escriba `<your device name> - Temp Controller` para agrupar los datos de telemetría y, a continuación, seleccione **Guardar**.

![Asignación a la jerarquía](./media/tutorial-configure-tsi/assign-to-hierarchy.png)

Repita los pasos anteriores para asignar a las etiquetas del termostato el tipo y la jerarquía correctos.

## <a name="view-your-data"></a>Consulta de los datos

Vuelva al panel de gráficos y expanda **Device Fleet (Flota de dispositivos) > su dispositivo**. Seleccione **thermostat1**, seleccione la variable **Temperatura** y, a continuación, seleccione **Agregar** para representar el valor en el gráfico. Haga lo mismo para **thermostat2** y el valor **defaultComponent** **workingSet**.

![Cambio del tipo de instancia de thermostat2](./media/tutorial-configure-tsi/charting-values.png)

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre las distintas opciones de gráficos, incluidos el ajuste de tamaño del intervalo y los controles del eje Y, consulte [Explorador de Azure Time Series Insights](../time-series-insights/concepts-ux-panels.md).

* Para obtener información detallada sobre el modelo de serie temporal de su entorno, consulte el artículo [Modelo de serie temporal en Azure Time Series Insights Gen2](../time-series-insights/concepts-model-overview.md).

* Para profundizar en las API de consulta y la sintaxis de las expresiones de serie temporal, consulte [API de consulta de Azure Time Series Insights de la segunda generación](/rest/api/time-series-insights/reference-query-apis.md).
