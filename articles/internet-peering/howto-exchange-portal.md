---
title: Creación o modificación de un emparejamiento de Exchange mediante Azure Portal
titleSuffix: Azure
description: Creación o modificación de un emparejamiento de Exchange mediante Azure Portal
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: article
ms.date: 5/2/2020
ms.author: derekol
ms.openlocfilehash: 259ced3032eb43a946de7f1cf4dad9abb99d4a11
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/26/2020
ms.locfileid: "83845293"
---
# <a name="create-or-modify-an-exchange-peering-by-using-the-azure-portal"></a>Creación o modificación de un emparejamiento de Exchange mediante Azure Portal

En este artículo se describe cómo crear un emparejamiento de Microsoft Exchange mediante Azure Portal. También se muestra cómo comprobar el estado del recurso, así como la forma de actualizarlo o eliminarlo y desaprovisionarlo.

Si lo prefiere, puede completar esta guía mediante [PowerShell](howto-exchange-powershell.md).

## <a name="before-you-begin"></a>Antes de empezar
* Revise los [requisitos previos](prerequisites.md) y el [Tutorial del emparejamiento de Exchange](walkthrough-exchange-all.md) antes de comenzar la configuración.
* Si ya dispone de emparejamientos de Exchange con Microsoft que no se convierten en recursos de Azure, consulte el artículo [Conversión de un emparejamiento de Exchange heredado en un recurso de Azure mediante el portal](howto-legacy-exchange-portal.md).

## <a name="create-and-provision-an-exchange-peering"></a>Creación y aprovisionamiento de un emparejamiento de Exchange

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Inicio de sesión en el portal y selección de su suscripción
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="create-an-exchange-peering"></a><a name=create></a>Creación de un emparejamiento de Exchange


Como proveedor de intercambio de Internet, puede crear una solicitud de emparejamiento de Exchange [creando un emparejamiento]( https://go.microsoft.com/fwlink/?linkid=2129593).

1. En la página **Create a Peering** (Crear un emparejamiento), en la pestaña **Basics** (Aspectos básicos), rellene los cuadros como se muestra aquí:

    > [!div class="mx-imgBorder"] 
    > ![Registro de Peering Service](./media/setup-basics-tab.png)

*    Seleccione su suscripción a Azure.

* En Resource group (Grupo de recursos), puede seleccionar un grupo de recursos existente en la lista desplegable o crear un grupo en Create new (Crear nuevo). En este ejemplo se va a crear un grupo de recursos.

* Name (Nombre) corresponde al nombre del recurso y puede ser el que prefiera.

* El valor de Region (Región) se selecciona automáticamente si se elige un grupo de recursos existente. Si ha elegido crear un grupo de recursos, también debe elegir la región de Azure en la que quiere que resida el recurso.

>[!NOTE]
>La región donde reside un grupo de recursos es independiente de la ubicación en la que quiere crear el emparejamiento con Microsoft. No obstante, se recomienda organizar los recursos de emparejamiento en grupos de recursos que residan en las regiones más cercanas de Azure. Por ejemplo, para los emparejamientos de Ashburn, puede crear un grupo de recursos en Este de EE. UU. o Este de EE. UU. 2.

* Seleccione su ASN en el cuadro **Peer ASN** (ASN del mismo nivel).

>[!IMPORTANT] 
>Solo puede elegir un ASN con el valor de ValidationState como aprobado antes de enviar una solicitud de emparejamiento. Si acaba de enviar la solicitud de PeerAsn, espere 12 horas aproximadamente a que el estado de la asociación de ASN sea "aprobado". Si el ASN seleccionado está pendiente de validación, verá un mensaje de error. Si no ve el ASN que debe elegir, compruebe que haya seleccionado la suscripción correcta. Si es así, compruebe si ya ha creado PeerAsn con **[Associate Peer ASN to Azure subscription (Asociar ASN del mismo nivel a suscripción de Azure)](https://go.microsoft.com/fwlink/?linkid=2129592)** .

* Seleccione **Siguiente: Configuración** para continuar.

#### <a name="configure-connections-and-submit"></a>Configuración de conexiones y envío
[!INCLUDE [exchange-peering-configuration](./includes/exchange-portal-configuration.md)]

### <a name="verify-an-exchange-peering"></a><a name=get></a>Comprobación de un emparejamiento de Exchange
[!INCLUDE [peering-exchange-get-portal](./includes/exchange-portal-get.md)]

## <a name="modify-an-exchange-peering"></a><a name="modify"></a>Modificación de un emparejamiento de Exchange
[!INCLUDE [peering-exchange-modify-portal](./includes/exchange-portal-modify.md)]

## <a name="deprovision-an-exchange-peering"></a><a name="delete"></a>Desaprovisionamiento de un emparejamiento de Exchange
[!INCLUDE [peering-exchange-delete-portal](./includes/delete.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Creación o modificación de un emparejamiento directo mediante el portal](howto-direct-portal.md)
* [Conversión de un emparejamiento directo heredado en un recurso de Azure mediante el portal](howto-legacy-direct-portal.md)

## <a name="additional-resources"></a>Recursos adicionales

Para más información, consulte las [preguntas frecuentes sobre el emparejamiento de Internet](faqs.md).
