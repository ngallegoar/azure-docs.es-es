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
ms.openlocfilehash: 79cf4b2edd1a25df427cf15f9ee7f2f331ebd2df
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773771"
---
1. Haga clic en **Crear un recurso** > **Ver todo**.

    > [!div class="mx-imgBorder"]
    > ![Buscar emparejamiento](../media/setup-seeall.png)

1. Busque *Emparejamiento* en el cuadro de búsqueda y presione *Entrar* en el teclado. En los resultados, haga clic en el recurso **Emparejamiento**.

    > [!div class="mx-imgBorder"]
    > ![Iniciar emparejamiento](../media/setup-launch.png)

1. Una vez iniciada la operación **Emparejamiento**, revise la página para conocer los detalles. Cuando esté listo, haga clic en **Crear**.

    > [!div class="mx-imgBorder"]
    > ![Crear emparejamiento](../media/setup-create.png)

1. En la página **Create a Peering** (Crear un emparejamiento), en **Aspectos básicos**, rellene los campos como se muestra a continuación.

    > [!div class="mx-imgBorder"]
    > ![Aspectos básicos del emparejamiento](../media/setup-basics-tab.png)

    * Elija la suscripción de **Azure**.
    * Para **Grupo de recursos**, puede seleccionar un grupo de recursos existente en el menú desplegable o seleccionar la opción **Crear nuevo** para crear uno. Para este ejemplo, crearemos un nuevo grupo de recursos.
    * **Nombre** corresponde al nombre del recurso y puede ser el que prefiera.
    * **Región** se selecciona automáticamente si se elige un grupo de recursos existente en el paso anterior. Si decide crear un nuevo grupo de recursos, también debe elegir la región de Azure en la que desea que resida el recurso. Este de EE. UU.

        > [!NOTE]
        > La región donde reside el grupo de recursos es independiente de la ubicación en la que desea crear el emparejamiento con Microsoft. No obstante, se recomienda organizar los recursos de emparejamiento en grupos de recursos que residan en las regiones más cercanas de Azure. Por ejemplo, para los emparejamientos en Ashburn, puede crear un grupo de recursos en *Este de EE. UU.* o *Este de EE. UU. 2*

    * Elija su ASN en el campo **ASN del mismo nivel**.

        > [!IMPORTANT]
        > * Solo puede elegir un ASN con el valor de ValidationState "Aprobado" antes de enviar una solicitud de emparejamiento. Si acaba de enviar la solicitud de PeerAsn, espere 12 horas aproximadamente para que el estado de la asociación de ASN sea "Aprobado". Si el ASN seleccionado está pendiente de validación, verá un mensaje de error. 
        > * Si no ve el ASN que debe elegir, compruebe si ha seleccionado la suscripción correcta. Si es así, compruebe si ya ha creado PeerAsn con [Asociación de un ASN del mismo nivel a una suscripción de Azure](../howto-subscription-association-portal.md).

        > [!div class="mx-imgBorder"]
        > ![Aspectos básicos del emparejamiento rellenados](../media/setup-direct-basics-filled-tab.png)

    * Haga clic en **Siguiente: Configuración >** para continuar.
