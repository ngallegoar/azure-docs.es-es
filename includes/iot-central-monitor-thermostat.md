---
title: archivo de inclusión
description: archivo de inclusión
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 03/12/2020
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 28f676892967abbd0da63d7a75ea3d164b87ce97
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96017515"
---
Como operador de la aplicación de Azure IoT Central, puede:

* Ver la telemetría enviada por el dispositivo en la página **Overview** (Información general):

    :::image type="content" source="media/iot-central-monitor-thermostat/view-telemetry.png" alt-text="Visualización de datos de telemetría de dispositivo":::

* Ver las propiedades del dispositivo en la página **About** (Acerca de):

    :::image type="content" source="media/iot-central-monitor-thermostat/about-properties.png" alt-text="Visualización de las propiedades del dispositivo":::

## <a name="customize-the-device-template"></a>Personalización de la plantilla de dispositivo

Como desarrollador de soluciones, puede personalizar la plantilla de dispositivo que IoT Central creó automáticamente cuando al conectar el termostato.

Para agregar una propiedad de nube para almacenar el nombre de cliente asociado al dispositivo:

1. En la aplicación de IoT Central, vaya a **Device templates** (Plantillas de dispositivo) y seleccione la plantilla de dispositivo **Thermostat**.

1. Seleccione **Cloud Properties** (Propiedades de la nube) en la plantilla del dispositivo **Thermostat**.

1. Seleccione **Add Cloud Property** (Agregar propiedad de la nube). Escriba *Customer name* como **Display name** (Nombre para mostrar) y elija **String** en **Schema** (Esquema). Después, seleccione **Guardar**.

Para personalizar la forma en que se muestra el comando **Get Max-Min report** en la aplicación de IoT Central, seleccione **Customize** (Personalizar) en la plantilla del dispositivo. Reemplace **Get Max-Min report** por *Get status report*. Después, seleccione **Guardar**.

El modelo **Thermostat** incluye la propiedad de escritura **Target Temperature**, la plantilla de dispositivo incluye la propiedad de nube **Customer Name**. Cree una vista que pueda usar el operador para editar estas propiedades:

1. Seleccione **Views** (Vistas) y el icono **Editing device and cloud data** (Edición de los datos del dispositivo y de la nube).

1. Escriba _Properties_ (Propiedades) como nombre del formulario.

1. Seleccione las propiedades **Target Temperature** y **Customer Name**. Después, seleccione **Add section** (Agregar sección).

1. Guarde los cambios.

:::image type="content" source="media/iot-central-monitor-thermostat/properties-view.png" alt-text="Vista para actualizar los valores de propiedad":::

## <a name="publish-the-device-template"></a>Publicación de la plantilla de dispositivo

Para que un operador pueda ver y usar las personalizaciones realizadas, debe publicar la plantilla de dispositivo.

Seleccione **Publish** (Publicar) en la plantilla del dispositivo **Thermostat**. En el panel **Publicar esta plantilla de dispositivo en la aplicación**, seleccione **Publicar**.

Ahora los operadores podrán usar la vista **Properties** (Propiedades) para actualizar los valores de propiedad y llamar a un comando llamado **Get status report** en la página comandos del dispositivo:

* Actualizar los valores de las propiedades con posibilidad de escritura en la página **Properties** (Propiedades):

    :::image type="content" source="media/iot-central-monitor-thermostat/update-properties.png" alt-text="Actualización de las propiedades del dispositivo.":::

* Llamar al comando desde la página **Commands** (Comandos):

    :::image type="content" source="media/iot-central-monitor-thermostat/call-command.png" alt-text="Llamada al comando.":::

    :::image type="content" source="media/iot-central-monitor-thermostat/command-response.png" alt-text="Visualización de la respuesta del comando.":::
