---
title: 'Creación de un entorno: Azure Time Series Insights | Microsoft Docs'
description: Aprenda a usar Azure Portal para crear un entorno de Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 01/31/2020
ms.custom: seodec18
ms.openlocfilehash: 2c946c49884ef0de6843028976d4ec00ccfbcdfe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76934845"
---
# <a name="create-a-new-time-series-insights-environment-in-the-azure-portal"></a>Creación de un nuevo entorno de Time Series Insights en Azure Portal

En este artículo se describe cómo crear un nuevo entorno de Time Series Insights mediante Azure Portal.

Time Series Insights permite empezar a visualizar y consultar en cuestión de minutos los datos que fluyen en centros de Azure IoT Hub y Event Hubs, de modo que se pueden consultar grandes volúmenes de datos de series temporales de consulta en segundos.  Se diseñó para la escala de Internet de las cosas (IoT) y puede controlar terabytes de datos.

## <a name="steps-to-create-the-environment"></a>Pasos para crear el entorno

Siga estos pasos para crear un entorno:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione el botón **+ Crear un recurso**.

1. Seleccione la categoría **Internet de las cosas** y elija **Time Series Insights**.

   [![Creación del entorno de Time Series Insights](media/time-series-insights-get-started/tsi-create-new-environment.png)](media/time-series-insights-get-started/tsi-create-new-environment.png#lightbox)

1. En la página **Time Series Insights**, seleccione **Crear**.

1. Rellene todos los campos obligatorios. La siguiente tabla explica cada parámetro:
   
   [![Creación del grupo de recursos de Time Series Insights](media/time-series-insights-get-started/tsi-configure-and-create.png)](media/time-series-insights-get-started/tsi-configure-and-create.png#lightbox)
   
   Configuración|Valor sugerido|Descripción
   ---|---|---
   Nombre del entorno | Un nombre único | Este nombre representará al entorno en el [explorador de Time Series Insights](https://insights.timeseries.azure.com).
   Subscription | Su suscripción | Si tiene varias suscripciones, elija la suscripción que contiene el origen de eventos si es posible. Time Series Insights puede detectar automáticamente los recursos de Azure IoT Hub y Event Hubs existentes en la misma suscripción.
   Resource group | Cree uno o use uno existente | Un grupo de recursos es una colección de recursos de Azure que se usan juntos. Puede elegir un grupo de recursos existente, por ejemplo, la carpeta que contenga el centro de eventos o IoT Hub. También puede crear un disco si este recurso no está relacionado con los otros recursos.
   Location | La más cercana al origen de su evento | Si es posible, elija la misma ubicación del centro de datos que contiene los datos de origen de eventos para evitar costos adicionales de ancho de banda entre regiones y zonas, y un aumento de la latencia cuando se mueven datos fuera de la región.
   Plan de tarifa | S1 | Elija el rendimiento requerido. Para costos más bajos y una capacidad de inicio menor, seleccione S1.
   Capacity | 1 | La capacidad es el multiplicador que se aplica a la tasa de entrada, la capacidad de almacenamiento y el costo asociado con la SKU seleccionada.  Puede cambiar la capacidad de un entorno después de su creación. Para costos más bajo, seleccione una capacidad de 1. 
  
1. Haga clic en **Crear** para comenzar el proceso de aprovisionamiento. Este proceso tardará unos minutos.

1. Seleccione el símbolo de **Notificaciones** (icono de campana) para supervisar el proceso de implementación.

   [![Visualización de notificaciones](media/time-series-insights-get-started/tsi-deploy-notifications.png)](media/time-series-insights-get-started/tsi-deploy-notifications.png#lightbox)

1. En **Información general** del recurso, compruebe las opciones de configuración de la implementación.

   [![Anclaje de Time Series Insights al panel](media/time-series-insights-get-started/tsi-verify-deployment.png)](media/time-series-insights-get-started/tsi-verify-deployment.png#lightbox)

1. **(Opcional)** Seleccione el **icono de anclaje** en la esquina superior derecha para acceder fácilmente a su entorno de Time Series Insights en el futuro.

## <a name="next-steps"></a>Pasos siguientes

* [Defina directivas de acceso de datos](time-series-insights-data-access.md) para proteger su entorno.

* [Agregue un origen de eventos de Event Hubs](time-series-insights-how-to-add-an-event-source-eventhub.md) al entorno de Azure Time Series Insights.

* Realice el [envío de eventos](time-series-insights-send-events.md) al origen del evento.

* Vea el entorno en el [explorador de Time Series Insights](https://insights.timeseries.azure.com).
