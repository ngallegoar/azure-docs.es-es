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
ms.openlocfilehash: 16909e0a5d59b0ae019d91aad00e0168b0c5f433
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773903"
---
1. En la página **Create a Peering** (Crear un emparejamiento), en **Aspectos básicos**, rellene los campos como se muestra a continuación.

    > [!div class="mx-imgBorder"]
    > ![Configuración de emparejamiento: directo](../media/setup-direct-conf-tab.png)

    * En **Tipo de emparejamiento**, seleccione *Directo*.
    * En **Red de Microsoft**, elija *AS8075*. No seleccione ASN 8069. Está reservado para aplicaciones especiales y solo la usa el [emparejamiento de Microsoft](mailto:peering@microsoft.com).
    * Seleccione **SKU** como *Gratis Básico*. No seleccione *Premium Free* (Gratis Premium) porque está reservado para aplicaciones especiales.
    * Elija la ubicación de **Metro** donde desea convertir el emparejamiento a un recurso de Azure. Si tiene conexiones de emparejamiento con Microsoft en la ubicación de **metro** seleccionada que no se han convertido a un recurso de Azure, dichas conexiones aparecerán en la sección **Conexiones de emparejamiento** como se muestra a continuación. Ahora puede convertir estas conexiones de emparejamiento en un recurso de Azure.

        > [!div class="mx-imgBorder"]
        > ![Configuración de emparejamiento: directo: conexiones heredadas](../media/setup-directlegacy-conf-tab.png)

1. Si necesita actualizar el ancho de banda, haga clic en el botón Editar de una línea, como se indica a continuación, para modificar la configuración de la conexión.

    > [!div class="mx-imgBorder"]
    > ![Editar en Configuración de emparejamiento: directo](../media/setup-directlegacy-conf-tab-edit.png)

    > [!NOTE]
    > Si desea agregar conexiones de emparejamiento adicionales con Microsoft en la ubicación de **metro** seleccionada, puede hacerlo con el botón **Crear nuevo**. Consulte [Creación o modificación de un emparejamiento directo mediante el portal](../howto-direct-portal.md) para obtener más información.
    >

1. Haga clic en **Revisar y crear**. Observe que el portal ejecuta una validación básica de la información especificada. Esta información se muestra en una cinta en la parte superior, con el texto *Ejecutando la validación final...* .

    > [!div class="mx-imgBorder"]
    > ![Pestaña Validación del emparejamiento](../media/setup-direct-review-tab-validation.png)

1. Cuando el mensaje de la cinta se convierta en *Validación superada*, compruebe la información y envíe la solicitud. Para ello, haga clic en **Crear**. Si necesita modificar la solicitud, haga clic en **Anterior** y repita los pasos anteriores.

    > [!div class="mx-imgBorder"]
    > ![Envío de emparejamiento](../media/setup-direct-review-tab-submit.png)

1. Después de enviar la solicitud, espere a que finalice la implementación. Si se produce un error en la implementación, póngase en contacto con el [equipo de emparejamiento de Microsoft](mailto:peering@microsoft.com). Una implementación correcta tendrá un aspecto similar al siguiente.

    > [!div class="mx-imgBorder"]
    > ![Emparejamiento correcto](../media/setup-direct-success.png)
