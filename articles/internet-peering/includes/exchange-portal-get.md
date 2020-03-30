---
title: archivo de inclusión
titleSuffix: Azure
description: archivo de inclusión
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: b967c844ab145074490e931122cbe092d67de0c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773743"
---
Una vez que el recurso de **emparejamiento** se implementa correctamente, puede verlo siguiendo los pasos que se indican a continuación.

1. Vaya a **Grupos de recursos** y haga clic en el grupo de recursos que seleccionó al crear el recurso de **emparejamiento**. Puede usar el campo *Filtro* si se muestran demasiados grupos de recursos.

    > [!div class="mx-imgBorder"]
    > ![Grupo de recursos de emparejamiento](../media/setup-direct-get-resourcegroup.png)

1. Haga clic en el recurso de **emparejamiento** que creó.

    > [!div class="mx-imgBorder"]
    > ![Vista de recurso de emparejamiento](../media/setup-direct-get-open.png)

1. En la página de **información general** se muestra información de alto nivel. Observe la información resaltada a continuación.

    > [!div class="mx-imgBorder"]
    > ![Vista de recurso de emparejamiento](../media/setup-exchange-get-overview.png)

1. A la izquierda, haga clic en **ASN Information** (Información de ASN) para ver la información enviada durante la creación de PeerAsn.

    > [!div class="mx-imgBorder"]
    > ![Vista de recurso de emparejamiento](../media/setup-direct-get-asninfo.png)

1. A la izquierda, haga clic en **Conexiones**. Observe en la parte superior un resumen de las conexiones de emparejamiento entre ASN y Microsoft, en diferentes instalaciones del metro. También puede llegar al resumen de conexiones desde la página de **información general**; para ello, haga clic en **Conexiones** en el panel central, tal y como se resalta.

    > [!div class="mx-imgBorder"]
    > ![Vista de recurso de emparejamiento](../media/setup-exchange-get-connectionssummary.png)

    * El **estado de la conexión** corresponde al estado de la configuración de la conexión de emparejamiento. Los estados mostrados en este campo siguen el diagrama de estado que se muestra en el [tutorial de emparejamiento de Exchange](../walkthrough-exchange-all.md)
    * El **estado de sesión IPv4** y el **estado de sesión IPv6** corresponden a los estados de sesión BGP de IPv4 e IPv6, respectivamente.  
    * Al seleccionar una fila en la parte superior, en la sección ***Conexión*** de la parte inferior se muestran los detalles de cada conexión. Puede hacer clic en las marcas de flecha para expandir las subsecciones ***Configuración***, ***Dirección IPv4*** y ***Dirección IPv6***.

    > [!div class="mx-imgBorder"]
    > ![Vista de recurso de emparejamiento](../media/setup-exchange-get-connectionsipv4.png)
