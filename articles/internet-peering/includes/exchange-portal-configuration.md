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
ms.openlocfilehash: cd51eca0ea4563e1b56f74677df0829669d9e177
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773727"
---
1. En la página **Create a Peering** (Crear un emparejamiento), en **Aspectos básicos**, rellene los campos como se muestra a continuación.

    > [!div class="mx-imgBorder"]
    > ![Configuración de emparejamiento: Exchange](../media/setup-exchange-conf-tab.png)

    * En **Tipo de emparejamiento**, seleccione *Exchange*.
    * Seleccione **SKU** como *Gratis Básico*.
    * Elija la ubicación de **metro** donde desea configurar el emparejamiento.

        > [!NOTE]
        > Si ya tiene conexiones de emparejamiento con Microsoft en la ubicación de **metro** seleccionada y usa el portal por primera vez para configurar el emparejamiento en esa ubicación, las conexiones de emparejamiento existentes se enumerarán en la sección **Peering connections** (Conexiones de emparejamiento), como se muestra a continuación. Microsoft convertirá automáticamente estas conexiones de emparejamiento en el recurso de Azure para que pueda administrarlas todas junto con las nuevas conexiones, en un solo lugar. Consulte [Conversión de un emparejamiento de Exchange heredado en un recurso de Azure mediante el portal](../howto-legacy-exchange-portal.md) para obtener más información.
        >

1. En **Peering connections** (Conexiones de emparejamiento), haga clic en **Crear nuevo** para agregar una línea para cada nueva conexión que quiera configurar.

    * Para configurar o modificar la configuración de conexión, haga clic en el botón Editar de una línea.

        > [!div class="mx-imgBorder"]
        > ![Editar en Configuración de emparejamiento: Exchange](../media/setup-exchange-conf-tab-edit.png)

    * Para eliminar una línea, haga clic en el botón **...** > **Eliminar**.

        > [!div class="mx-imgBorder"]
        > ![Editar en Configuración de emparejamiento: Exchange](../media/setup-exchange-conf-tab-delete.png)

    * Se le pedirá que proporcione toda la configuración de una conexión, como se muestra a continuación.

         > [!div class="mx-imgBorder"]
         > ![Conexión en Configuración de emparejamiento: Exchange](../media/setup-exchange-conf-tab-connection.png)

        1. Seleccione la **instalación de emparejamiento** en la que debe configurarse la conexión.
        1. En los campos **Dirección IPv4** y **Dirección IPv6**, escriba las direcciones IPv4 e IPv6, respectivamente, que se configurarían en los enrutadores de Microsoft con el comando vecino.
        1. Escriba el número de prefijos IPv4 e IPv6 que anunciará en los campos **Maximum advertised IPv4 addresses** (Máximo de direcciones IPv4 anunciadas) y **Maximum advertised IPv6 addresses** (Máximo de direcciones IPv6 anunciadas), respectivamente.
        1. Haga clic en **Aceptar** para guardar la configuración de la conexión.

1. Repita el paso anterior para agregar más conexiones en cualquier instalación donde Microsoft esté ubicado conjuntamente con la red, dentro del **Metro** seleccionado previamente.

1. Después de agregar todas las conexiones necesarias, haga clic en **Revisar y crear**.

    > [!div class="mx-imgBorder"]
    > ![Configuración de emparejamiento: pestaña Final](../media/setup-exchange-conf-tab-final.png)

1. Observe que el portal ejecuta una validación básica de la información especificada. Esta información se muestra en una cinta en la parte superior, con el texto *Ejecutando la validación final...* .

    > [!div class="mx-imgBorder"]
    > ![Pestaña Validación del emparejamiento](../media/setup-direct-review-tab-validation.png)

1. Cuando el mensaje de la cinta se convierta en *Validación superada*, compruebe la información y envíe la solicitud. Para ello, haga clic en **Crear**. Si necesita modificar la solicitud, haga clic en **Anterior** y repita los pasos anteriores.

    > [!div class="mx-imgBorder"]
    > ![Envío de emparejamiento](../media/setup-exchange-review-tab-submit.png)

1. Después de enviar la solicitud, espere a que finalice la implementación. Si se produce un error en la implementación, póngase en contacto con el [equipo de emparejamiento de Microsoft](mailto:peering@microsoft.com). Una implementación correcta tendrá un aspecto similar al siguiente.

    > [!div class="mx-imgBorder"]
    > ![Emparejamiento correcto](../media/setup-direct-success.png)
