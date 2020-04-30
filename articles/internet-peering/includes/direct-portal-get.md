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
ms.openlocfilehash: e50f53c65c330265d3d9ec1b1804a7910b632b1d
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678849"
---
1. Vaya a **Grupos de recursos** y seleccione el grupo de recursos que eligió al crear el recurso **Peering**. Si tiene demasiados grupos de recursos, use el cuadro **Filtrar**.

    > [!div class="mx-imgBorder"]
    > ![Grupos de recursos](../media/setup-direct-get-resourcegroup.png)

1. Seleccione el recurso **Peering** que creó.

    > [!div class="mx-imgBorder"]
    > ![Vista de recursos de emparejamiento](../media/setup-direct-get-open.png)

1. En la página **Overview** (Información general) se muestra información de alto nivel, como se ilustra aquí.

    > [!div class="mx-imgBorder"]
    > ![Panel de información general del recurso de emparejamiento](../media/setup-direct-get-overview.png)

1. A la izquierda, seleccione **ASN information** (Información de ASN) para ver la información que se envía al crear PeerAsn.

    > [!div class="mx-imgBorder"]
    > ![Información de ASN del recurso de emparejamiento](../media/setup-direct-get-asninfo.png)

1. A la izquierda, seleccione **Connections** (Conexiones). En la parte superior de la pantalla, verá un resumen de las conexiones de emparejamiento entre ASN y Microsoft, en diferentes instalaciones del área metropolitana. También puede acceder al resumen de conexiones desde la página **Overview** (Información general) si selecciona **Connections** (Conexiones) en panel central, como se muestra.

    > [!div class="mx-imgBorder"]
    > ![Conexiones del recurso de emparejamiento](../media/setup-direct-get-connectionssummary.png)

    * El **estado de la conexión** corresponde al estado de la configuración de la conexión de emparejamiento. Los estados mostrados en este campo siguen el diagrama de estado que se muestra en el [tutorial de emparejamiento directo](../walkthrough-direct-all.md).
    * El **estado de sesión IPv4** y el **estado de sesión IPv6** corresponden a los estados de sesión BGP de IPv4 e IPv6, respectivamente. 
    * Al seleccionar una fila en la parte superior de la pantalla, la sección **Connection** (Conexión) de la parte inferior muestra los detalles de cada conexión. Seleccione las flechas para expandir las opciones **Configuration** (Configuración), **IPv4 address** (Dirección IPv4) y **IPv6 address** (Dirección IPv6).
