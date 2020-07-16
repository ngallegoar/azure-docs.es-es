---
title: Supervisión de Azure Data Share
description: Aprenda a supervisar el estado de las invitaciones, las suscripciones de recursos compartidos y el historial de instantáneas en Azure Data Share
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: how-to
ms.date: 07/10/2019
ms.openlocfilehash: 3ee3f0d37c8e35972a1fc2b8c5d04504d2e065f3
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86120201"
---
# <a name="monitor-azure-data-share"></a>Supervisión de Azure Data Share  

En este artículo se explica cómo puede supervisar los recursos compartidos de datos mediante Azure Data Share. Como proveedor de datos, puede supervisar diversos aspectos de las relaciones del uso compartido de datos. Aspectos como saber si los consumidores de datos han aceptado la invitación al recurso compartido de datos, si han creado una suscripción al mismo o si han empezado a utilizar los datos, están todos disponibles para su supervisión. 

Como consumidor de datos, puede supervisar las instantáneas que se han desencadenado en la suscripción de Azure. 

## <a name="monitor-invitation-status"></a>Supervisión del estado de la invitación

Vea el estado de las invitaciones a los recursos compartidos de datos. Para ello vaya a Sent shares (Recursos compartidos enviados) -> Invitaciones. 

![Estado de la invitación](./media/invitation-status.png "Estado de la invitación") 

La invitación puede tener uno de los tres estados siguientes:

* Pendiente: el destinatario del recurso compartido de datos no ha aceptado todavía la invitación.
* Aceptada: el destinatario del recurso compartido de datos ha aceptado la invitación.
* Rechazada: el destinatario del recurso compartido de datos ha rechazado la invitación.

> [!IMPORTANT]
> Eliminar una invitación una vez aceptada no equivale a revocar el acceso. Si desea impedir que se copien las instantáneas futuras en la cuenta de almacenamiento de los consumidores de datos, debe revocar el acceso mediante la pestaña *Share subscriptions* (Suscripciones a recursos compartidos). 

## <a name="monitor-share-subscriptions"></a>Supervisión de suscripciones a recursos compartidos

Vea el estado de las suscripciones a recursos compartidos de datos. Para ello vaya a Sent shares (Recursos compartidos enviados) -> Share Subscriptions (Suscripciones a recursos compartidos). Esto le proporcionará detalles sobre las suscripciones activas que han creado los consumidores de datos después de aceptar la invitación. Puede detener las actualizaciones futuras para los consumidores de datos seleccionando la suscripción a recurso compartido y seleccionando *Revocar*. 

## <a name="snapshot-history"></a>Historial de instantáneas 

En la pestaña **Historial** de un recurso compartido, puede ver cuándo se copian los datos del proveedor de datos al almacén de datos del consumidor de datos. Es posible supervisar la frecuencia, duración y estado de cada instantánea. 

![Historial de instantáneas](./media/sent-shares.png "Historial de instantáneas") 

Para ver más información sobre la ejecución de cada instantánea, haga clic en la fecha de inicio de la ejecución. A continuación, haga clic en el estado de cada conjunto de datos para ver la cantidad de datos transferidos, el número de archivos o registros copiados, la duración de la instantánea, el número de núcleos virtuales usados y el mensaje de error, si hay alguno. 

Se muestra un máximo de 30 días de historial de instantáneas. Si tiene que guardar y ver más de 30 días de historial, puede aprovechar la configuración de diagnóstico.

## <a name="diagnostic-setting"></a>Configuración de diagnóstico

Puede establecer la configuración de diagnóstico para guardar eventos o datos de registro. Vaya a Supervisión -> Configuración de diagnóstico y seleccione **Agregar configuración de diagnóstico**. Seleccione los eventos o datos de registro que le interesen, así como el lugar donde quiera almacenarlos o enviarlos. 

![Historial de instantáneas](./media/diagnostic-settings.png "Configuración de diagnóstico") 

## <a name="next-steps"></a>Pasos siguientes 

Más información sobre [terminología de Azure Data Share](terminology.md)