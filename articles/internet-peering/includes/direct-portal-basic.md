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
ms.openlocfilehash: 00c24212706555667ad4680c086ece24f15ca59a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81678556"
---
1. Seleccione **Crear un recurso** > **Ver todo**.

    > [!div class="mx-imgBorder"]
    > ![Buscar emparejamiento](../media/setup-seeall.png)

1. Busque **Peering** en el cuadro de búsqueda y presione **Entrar** en el teclado. En los resultados, seleccione un recurso **Peering**.

    > [!div class="mx-imgBorder"]
    > ![Iniciar emparejamiento](../media/setup-launch.png)

1. Después de iniciar **Peering**, revise la página para conocer los detalles. Cuando esté listo, seleccione **Crear**.

    > [!div class="mx-imgBorder"]
    > ![Crear emparejamiento](../media/setup-create.png)

1. En la página **Create a Peering** (Crear un emparejamiento), en la pestaña **Basics** (Aspectos básicos), rellene los cuadros como se muestra aquí.

    > [!div class="mx-imgBorder"]
    > ![Pestaña Basics (Aspectos básicos) del emparejamiento](../media/setup-basics-tab.png)

    * Seleccione su **suscripción** de Azure.
    * En **Resource group** (Grupo de recursos), puede seleccionar un grupo de recursos existente en la lista desplegable o crear un grupo en **Create new** (Crear nuevo). En este ejemplo se va a crear un grupo de recursos.
    * **Name** (Nombre) corresponde al nombre del recurso y puede ser el que prefiera.
    * **Region** (Región) se selecciona automáticamente si se elige un grupo de recursos existente. Si ha elegido crear un grupo de recursos, también debe elegir la región de Azure en la que quiere que resida el recurso.

        > [!NOTE]
        > La región donde reside un grupo de recursos es independiente de la ubicación en la que quiere crear el emparejamiento con Microsoft. No obstante, se recomienda organizar los recursos de emparejamiento en grupos de recursos que residan en las regiones más cercanas de Azure. Por ejemplo, para los emparejamientos de Ashburn, puede crear un grupo de recursos en Este de EE. UU. o Este de EE. UU. 2.

    * Seleccione su ASN en el campo **Peer ASN** (ASN del mismo nivel).

        > [!IMPORTANT]
        > * Solo puede elegir un ASN con el valor de ValidationState como aprobado antes de enviar una solicitud de emparejamiento. Si acaba de enviar la solicitud de PeerAsn, espere 12 horas aproximadamente a que el estado de la asociación de ASN sea "aprobado". Si el ASN seleccionado está pendiente de validación, verá un mensaje de error. 
        > * Si no ve el ASN que debe elegir, compruebe que haya seleccionado la suscripción correcta. Si es así, compruebe si ya ha creado PeerAsn con [Asociación de un ASN del mismo nivel a una suscripción de Azure](../howto-subscription-association-portal.md).

        > [!div class="mx-imgBorder"]
        > ![Aspectos básicos del emparejamiento rellenados](../media/setup-direct-basics-filled-tab.png)

    * Seleccione **Siguiente: Configuración >** para continuar.
