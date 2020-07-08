---
title: Revocación de una suscripción a un recurso compartido en Azure Data Share
description: Obtenga información sobre cómo revocar una suscripción de recurso compartido de un destinatario mediante Azure Data Share.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: how-to
ms.date: 07/10/2019
ms.openlocfilehash: c1c049d467cdf07962719f0dc257acbf47631aaf
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119691"
---
# <a name="how-to-revoke-a-consumers-share-subscription-in-azure-data-share"></a>Revocación de una suscripción a un recurso compartido de un consumidor en Azure Data Share

En este artículo se explica cómo revocar una suscripción a un recurso compartido de uno o varios de los consumidores mediante Azure Data Share. Esto evita que un consumidor desencadene más instantáneas. Si el consumidor todavía no ha desencadenado una instantánea, nunca recibirá los datos una vez que se haya revocado la suscripción a un recurso compartido. Si anteriormente ha desencadenado una instantánea, los datos más recientes que tenga permanecerán en su cuenta.

## <a name="navigate-to-a-sent-data-share"></a>Desplazamiento a un recurso compartido de datos enviado

En Azure Data Share, vaya a su recurso compartido enviado y seleccione la pestaña **Suscripciones de recursos compartidos**.

![Revocación de una suscripción a un recurso compartido](./media/how-to/how-to-revoke-share-subscription/revoke-share-subscription.png) 

Active las casillas situadas junto a los destinatarios cuyas suscripciones a recursos compartidos desea eliminar y haga clic en **Revocar**. El consumidor ya no recibirá actualizaciones para sus datos.

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre como [exportar los recursos compartidos de datos](how-to-monitor.md).