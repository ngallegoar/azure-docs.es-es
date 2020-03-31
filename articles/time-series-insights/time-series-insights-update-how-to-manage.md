---
title: 'Aprovisionamiento y administración de un entorno de versión preliminar: Azure Time Series | Microsoft Docs'
description: Obtenga información sobre cómo aprovisionar y administrar un entorno de versión preliminar de Azure Time Series Insights.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 02/07/2020
ms.custom: seodec18
ms.openlocfilehash: 1ec0d9c7ecf16c60c32abdf08b358268f460edb0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77087214"
---
# <a name="provision-and-manage-azure-time-series-insights-preview"></a>Aprovisionamiento y administración de la versión preliminar de Azure Time Series Insights

En este artículo se describe cómo crear y administrar un entorno en la versión preliminar de Azure Time Series Insights con [Azure Portal](https://portal.azure.com/).

## <a name="overview"></a>Información general

Los entornos de versión preliminar de Azure Time Series Insights son entornos de *pago por uso* (PAYG).

Al aprovisionar un entorno de versión preliminar de Azure Time Series Insights, creará estos recursos de Azure:

* Un entorno de la versión preliminar de Azure Time Series Insights.  
* Una cuenta de uso general v1 de Azure Storage.
* Un almacenamiento intermedio opcional para consultas más rápidas e ilimitadas.

> [!TIP]
> * Aprenda [a planificar su entorno ](./time-series-insights-update-plan.md).
> * Obtenga información sobre cómo [agregar un origen de centro de eventos](./time-series-insights-how-to-add-an-event-source-eventhub.md) o cómo [agregar un origen de IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md).

Aprenderá a:

1. Asocie cada entorno de la versión preliminar de Azure Time Series Insights con un origen del evento **(opcional)** . También proporcionará una propiedad de identificador de marca de tiempo y un grupo de consumidores único para asegurarse de que el entorno tiene acceso a los eventos correspondientes.

   > [!NOTE]
   > El paso anterior es opcional al aprovisionar un entorno. Si se salta este paso, debe adjuntar un origen de eventos al entorno más adelante para que se pueda tener acceso a los datos en el entorno.

1. Una vez completado el aprovisionamiento, puede modificar sus directivas de acceso y otros atributos del entorno para adaptarse a sus requisitos comerciales.

## <a name="create-the-environment"></a>Creación del entorno

Para crear un entorno de versión preliminar de Azure Time Series Insights:

1. Seleccione **PAYG** como **nivel**. Proporcione un nombre de entorno y elija el grupo de suscripciones y el grupo de recursos que se va a usar. A continuación, seleccione una ubicación compatible para hospedar el entorno.

   [![Cree una instancia de Azure Time Series Insights.](media/v2-update-manage/create-and-manage-configuration.png)](media/v2-update-manage/create-and-manage-configuration.png#lightbox)

1. Introduzca un id. de serie temporal.

    > [!NOTE]
    > * El identificador de serie temporal es *inmutable* y *distingue mayúsculas de minúsculas*. (Una vez establecido, no se puede cambiar).
    > * Los identificadores de serie temporal pueden tener hasta *tres* claves.
    > * Más información sobre [cómo elegir un identificador de serie temporal](time-series-insights-update-how-to-id.md)

1. Cree una cuenta de Azure Storage seleccionando un nombre de cuenta de almacenamiento y designando una opción de replicación. Al hacerlo, se crea automáticamente una cuenta de uso general v1 de Azure Storage. La cuenta se crea en la misma región que el entorno de versión preliminar de Azure Time Series Insights que seleccionó anteriormente.

    [![Configuración de almacenamiento](media/v2-update-manage/create-and-manage-cold-store.png)](media/v2-update-manage/create-and-manage-cold-store.png#lightbox)

1. **(Opcional)** Habilite el almacenamiento intermedio para el entorno si quiere consultas más rápidas e ilimitadas sobre los datos más recientes del entorno. También puede crear o eliminar un almacenamiento intermedio a través de la opción **Configuración de almacenamiento** en el panel de navegación de la izquierda, después de crear un entorno de versión preliminar de Time Series Insights.

    [![Configuración de almacenamiento intermedio](media/v2-update-manage/create-and-manage-warm-storage.png)](media/v2-update-manage/create-and-manage-warm-storage.png#lightbox)

1. **(Opcional)** Ahora puede agregar un origen del evento. También puede esperar hasta que se haya aprovisionado la instancia.

   * Time Series Insights admite [Azure IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md) y [Azure Event Hubs](./time-series-insights-how-to-add-an-event-source-eventhub.md) como opciones de origen del evento. Si bien solo puede agregar un origen del evento único en el momento de crear el entorno, puede agregar otro origen del evento más adelante. 
   
     Puede seleccionar un grupo de consumidores existente o crear uno nuevo cuando agregue el origen del evento. Es mejor crear un grupo de consumidores único para asegurarse de que todos los eventos sean visibles en el entorno de versión preliminar de Azure Time Series Insights.

   * Elija la propiedad Timestamp adecuada. De manera predeterminada, Azure Time Series Insights usa el tiempo de puesta en cola del mensaje para cada origen del evento.

     > [!TIP]
     > Es posible que el tiempo de puesta en cola del mensaje no sea la mejor opción de configuración para usar en escenarios de carga de datos históricos o de eventos por lotes. En tales casos, asegúrese de comprobar correctamente la decisión de usar o no usar una propiedad Timestamp.

     [![Pestaña Configuración de origen de eventos](media/v2-update-manage/create-and-manage-event-source.png)](media/v2-update-manage/create-and-manage-event-source.png#lightbox)

1. Confirme que el entorno se ha aprovisionado y configurado de la forma deseada.

    [![Pestaña Revisar y crear](media/v2-update-manage/create-and-manage-review-and-confirm.png)](media/v2-update-manage/create-and-manage-review-and-confirm.png#lightbox)

## <a name="manage-the-environment"></a>Administrar el entorno

Puede administrar su entorno de versión preliminar de Azure Time Series Insights con Azure Portal. Hay algunas diferencias clave entre un entorno de versión preliminar de Azure Time Series Insights de pago por uso y los entornos S1 o S2 disponibles con carácter general que debe tener en cuenta al administrar el entorno a través de Azure Portal:

* La hoja **Resumen** de la versión preliminar de Azure Portal tiene los siguientes cambios:

  * La capacidad se quita porque no se aplica a los entornos de pago por uso.
  * Se agrega la propiedad **Identificador de serie temporal**. Determina cómo se particionan los datos.
  * Se eliminan los conjuntos de datos de referencia.
  * La dirección URL mostrada le dirige al [Explorador de versión preliminar de Azure Time Series Insights](./time-series-insights-update-explorer.md).
  * Su nombre de la cuenta de Azure Storage se agrega a la lista.

* La hoja **Configurar** de Azure Portal se quitó en la versión preliminar de Azure Time Series Insights, porque los entornos PAYG no son configurables. Sin embargo, puede usar **Configuración de almacenamiento** para configurar el almacenamiento intermedio recién incorporado.

* La hoja **Datos de referencia** de Azure Portal se quitó en la versión preliminar de Azure Time Series Insights, porque los datos de referencia no forman parte de los entornos de pago por uso.

[![Entorno de la versión preliminar de Time Series Insights en Azure Portal](media/v2-update-manage/create-and-manage-overview-confirm.png)](media/v2-update-manage/create-and-manage-overview-confirm.png#lightbox)

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre los entornos de versión preliminar y los entornos de disponibilidad general de Time Series Insights, lea [Planeamiento del entorno](./time-series-insights-update-plan.md).

- Obtenga información sobre cómo [agregar un origen de Event Hub](./time-series-insights-how-to-add-an-event-source-eventhub.md).

- Configure un [origen de centro de IoT](./time-series-insights-how-to-add-an-event-source-iothub.md).
