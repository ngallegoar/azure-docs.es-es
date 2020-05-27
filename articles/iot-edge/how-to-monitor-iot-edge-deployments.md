---
title: 'Supervisión de las implementaciones de IoT Edge: Azure IoT Edge'
description: Supervisión de alto nivel que incluye las propiedades informadas de EdgeHub y EdgeAgent y las métricas de implementación automática.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/21/2020
ms.topic: conceptual
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 97bc3c8571793ec8c8b67fe0e7c5cb3b6a56fde4
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83726050"
---
# <a name="monitor-iot-edge-deployments"></a>Supervisión de las implementaciones de IoT Edge

Azure IoT Edge proporciona informes que permiten supervisar la información en tiempo real en los módulos implementados en los dispositivos IoT Edge. El servicio IoT Hub recupera el estado de los dispositivos y hace que estén disponibles para el operador. La supervisión también es importante para las [implementaciones hechas a escala](module-deployment-monitoring.md) que incluyen implementaciones automáticas e implementaciones superpuestas.

Tanto los dispositivos como los módulos tienen datos similares, como la conectividad, de modo que los valores se obtienen según el id. del dispositivo o el id. del módulo.

El servicio IoT Hub recopila los datos que informa el dispositivo y los módulos gemelos y proporciona recuentos de los distintos estados que pueden tener los dispositivos. El servicio IoT Hub organiza estos datos en cuatro grupos de métricas:

| Tipo | Descripción |
| --- | ---|
| Destino | Muestra los dispositivos IoT Edge que coinciden con la condición de destino de implementación. |
| Aplicado | Muestra los dispositivos IoT Edge de destino que no están destinados a otra implementación de mayor prioridad. |
| Informe correcto | Muestra los dispositivos IoT Edge que han informado de que los módulos se han implementado correctamente. |
| Error en el informe | muestra los dispositivos IoT Edge que han informado de que uno o más módulos no se implementaron correctamente. Para investigar más el error, conéctese de forma remota a esos dispositivos para examinar los archivos de registros. |

El servicio IoT Hub hace que estos datos estén disponibles para que los supervise en Azure Portal y en la CLI de Azure.

## <a name="monitor-a-deployment-in-the-azure-portal"></a>Supervisión de una implementación en Azure Portal

Para ver los detalles de una implementación y supervisar los dispositivos que la ejecutan, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a su instancia de IoT Hub.
1. Seleccione **IoT Edge** en el menú del panel de la izquierda.
1. Seleccione la pestaña **Implementaciones de IoT Edge**.
1. Examine la lista de implementaciones. Para cada implementación, puede ver los detalles siguientes:

    | Columna | Descripción |
    | --- | --- |
    | id | Nombre de la implementación. |
    | Tipo | El tipo de implementación, ya sea **Implementación** o **Implementación superpuesta**. |
    | Condición de destino | La etiqueta que se usa para definir los dispositivos de destino. |
    | Priority | El número de prioridad asignado a la implementación. |
    | Métricas de sistema | El número de dispositivos gemelos en IoT Hub que coinciden con la condición de destino. **Applied** especifica la cantidad de dispositivos cuyo contenido de implementación se ha aplicado a sus módulos gemelos en IoT Hub. |
    | Métricas de dispositivos | El número de dispositivos IoT Edge que notifican el éxito o los errores del runtime del cliente de IoT Edge. |
    | Métricas personalizadas | El número de dispositivos IoT Edge que notifican datos de cualquiera de las métricas que haya definido para la implementación. |
    | Hora de creación | La marca de tiempo de cuando se creó la implementación. Esta marca de tiempo se utiliza para dirimir cuando dos implementaciones tienen la misma prioridad. |

1. Seleccione la implementación que desea supervisar.  
1. En la página **Detalles de la implementación**, desplácese hacia abajo hasta la sección inferior y seleccione la pestaña **Condición de destino**. Seleccione **Ver** para enumerar los dispositivos que coinciden con la condición de destino. Puede cambiar la condición y también la **Prioridad**. Seleccione **Guardar** si hizo cambios.

   ![Visualización de los dispositivos de destino de una implementación](./media/how-to-monitor-iot-edge-deployments/target-devices.png)

1. Seleccione la pestaña **Métricas**. Si elige una métrica en el menú desplegable **Seleccionar métrica**, aparecerá un botón **Ver** para mostrar los resultados. También puede seleccionar **Editar métricas** para ajustar los criterios de las métricas personalizadas que haya definido. Seleccione **Guardar** si hizo cambios.

   ![Visualización de las métricas de una implementación](./media/how-to-monitor-iot-edge-deployments/deployment-metrics-tab.png)


Si quiere hacer cambios en la implementación, consulte [Modificación de una implementación](how-to-deploy-at-scale.md#modify-a-deployment).

## <a name="monitor-a-deployment-with-azure-cli"></a>Supervisión de una implementación con la CLI de Azure

Use el comando [az IoT Edge deployment show](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/edge/deployment?view=azure-cli-latest#ext-azure-iot-az-iot-edge-deployment-show) para mostrar los detalles de una sola implementación:

```cli
az iot edge deployment show --deployment-id [deployment id] --hub-name [hub name]
```

El comando deployment show toma los parámetros siguientes:

* **--deployment-id**: el nombre de la implementación que está en IoT Hub. Parámetro obligatorio.
* **--hub-name**: nombre de la instancia de IoT Hub en la que está la implementación. El centro debe estar en la suscripción actual. Cambie a la suscripción que quiera usar con el comando `az account set -s [subscription name]`.

Inspeccione la implementación en la ventana de comandos. La propiedad **metrics** enumera un recuento para cada métrica que evalúa cada centro:

* **targetedCount**: es una métrica del sistema que especifica la cantidad de dispositivos gemelos que hay en IoT Hub que coinciden con las condiciones del destino.
* **appliedCount**: es una métrica del sistema que especifica la cantidad de dispositivos cuyo contenido de implementación se ha aplicado a sus módulos gemelos en IoT Hub.
* **reportedSuccessfulCount**: es una métrica de dispositivo que especifica la cantidad de dispositivos IoT Edge en el informe de implementación correcto, a partir del tiempo de ejecución del cliente de IoT Edge.
* **reportedFailedCount**: es una métrica de dispositivo que especifica la cantidad de dispositivos IoT Edge en el informe de implementación con errores, a partir del tiempo de ejecución del cliente de IoT Edge.

Puede mostrar una lista de identificadores u objetos de dispositivos para cada una de las métricas mediante el comando [az IoT Edge deployment show-metric](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/edge/deployment?view=azure-cli-latest#ext-azure-iot-az-iot-edge-deployment-show-metric):

```cli
az iot edge deployment show-metric --deployment-id [deployment id] --metric-id [metric id] --hub-name [hub name]
```

El comando deployment show-metric toma los parámetros siguientes:

* **--deployment-id**: el nombre de la implementación que está en IoT Hub.
* **--metric-id**: el nombre de la métrica de la cual quiere ver la lista de identificadores de dispositivos, por ejemplo `reportedFailedCount`.
* **--hub-name**: nombre de la instancia de IoT Hub en la que está la implementación. El centro debe estar en la suscripción actual. Cambie a la suscripción que quiera usar con el comando `az account set -s [subscription name]`.

Si quiere hacer cambios en la implementación, consulte [Modificación de una implementación](how-to-deploy-cli-at-scale.md#modify-a-deployment).

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [comunicarse con EdgeAgent mediante métodos directos integrados](how-to-edgeagent-direct-method.md).
