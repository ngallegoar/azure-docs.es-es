---
title: Creación o modificación de un emparejamiento directo mediante Azure Portal
titleSuffix: Azure
description: Creación o modificación de un emparejamiento directo mediante Azure Portal
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: article
ms.date: 5/19/2020
ms.author: derekol
ms.openlocfilehash: 59b9079b500817c31586c0a566082a867d7e7f41
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684005"
---
# <a name="create-or-modify-a-direct-peering-by-using-the-azure-portal"></a>Creación o modificación de un emparejamiento directo mediante Azure Portal

En este artículo se describe cómo crear un emparejamiento directo de Microsoft para un proveedor de servicios de Internet o un proveedor de intercambio de Internet mediante Azure Portal. También se muestra cómo comprobar el estado del recurso, así como la forma de actualizarlo o eliminarlo y desaprovisionarlo.

Si lo prefiere, puede completar esta guía mediante Azure [PowerShell](howto-direct-powershell.md).

## <a name="before-you-begin"></a>Antes de empezar
* Antes de comenzar la configuración, revise los [requisitos previos](prerequisites.md) y el [tutorial del emparejamiento directo](walkthrough-direct-all.md).
* Si ya dispone de conexiones de emparejamiento directo con Microsoft que no convierten en recursos de Azure, consulte [Conversión de un emparejamiento directo heredado en un recurso de Azure mediante el portal](howto-legacy-direct-portal.md).

## <a name="create-and-provision-a-direct-peering"></a>Creación y aprovisionamiento de un emparejamiento directo

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Inicio de sesión en el portal y selección de su suscripción
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="create-a-direct-peering"></a><a name=create></a>Creación de un emparejamiento directo

Como proveedor de servicios de Internet o proveedor de intercambio de Internet, puede crear una nueva solicitud de emparejamiento directo [creando un emparejamiento]( https://go.microsoft.com/fwlink/?linkid=2129593).

1. En la página **Create a Peering** (Crear un emparejamiento), en la pestaña **Basics** (Aspectos básicos), rellene los cuadros como se muestra aquí:


    ![Registro de Peering Service](./media/setup-basics-tab.png)

2. Seleccione su suscripción a Azure.

3. En Resource group (Grupo de recursos), puede seleccionar un grupo de recursos existente en la lista desplegable o crear un grupo en Create new (Crear nuevo). En este ejemplo se va a crear un grupo de recursos.

4. Name (Nombre) corresponde al nombre del recurso y puede ser el que prefiera.

5. El valor de Region (Región) se selecciona automáticamente si se elige un grupo de recursos existente. Si ha elegido crear un grupo de recursos, también debe elegir la región de Azure en la que quiere que resida el recurso.

    >[!NOTE]
    > La región donde reside un grupo de recursos es independiente de la ubicación en la que quiere crear el emparejamiento con Microsoft. No obstante, se recomienda organizar los recursos de emparejamiento en grupos de recursos que residan en las regiones más cercanas de Azure. Por ejemplo, para los emparejamientos de Ashburn, puede crear un grupo de recursos en Este de EE. UU. o Este de EE. UU. 2.

6. Seleccione su ASN en el cuadro **Peer ASN** (ASN del mismo nivel).

    >[!IMPORTANT]
    >Solo puede elegir un ASN con el valor de ValidationState como aprobado antes de enviar una solicitud de emparejamiento. Si acaba de enviar la solicitud de PeerAsn, espere 12 horas aproximadamente a que el estado de la asociación de ASN sea "aprobado". Si el ASN seleccionado está pendiente de validación, verá un mensaje de error. Si no ve el ASN que debe elegir, compruebe que haya seleccionado la suscripción correcta. Si es así, compruebe si ya ha creado PeerAsn con **[Associate Peer ASN to Azure subscription (Asociar ASN del mismo nivel a suscripción de Azure)](https://go.microsoft.com/fwlink/?linkid=2129592)** .

7. Seleccione **Siguiente: Configuración** para continuar.



    ![Registro de Peering Service](./media/setup-direct-basics-filled-tab.png)


#### <a name="configure-connections-and-submit"></a>Configuración de conexiones y envío
[!INCLUDE [direct-peering-configuration](./includes/direct-portal-configuration.md)]

### <a name="verify-direct-peering"></a><a name=get></a>Comprobación del emparejamiento directo
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

## <a name="modify-a-direct-peering"></a><a name="modify"></a>Modificación de un emparejamiento directo
[!INCLUDE [peering-direct-modify-portal](./includes/direct-portal-modify.md)]

## <a name="deprovision-a-direct-peering"></a><a name="delete"></a>Desaprovisionamiento de un emparejamiento directo
[!INCLUDE [peering-direct-delete-portal](./includes/delete.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Creación o modificación de un emparejamiento de Exchange mediante el portal](howto-exchange-portal.md)
* [Conversión de un emparejamiento de Exchange heredado en un recurso de Azure mediante el portal](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Recursos adicionales

Para más información, consulte las [preguntas frecuentes sobre el emparejamiento de Internet](faqs.md).
