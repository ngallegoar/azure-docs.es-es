---
title: Conversión de un emparejamiento directo heredado en un recurso de Azure mediante Azure Portal
titleSuffix: Azure
description: Conversión de un emparejamiento directo heredado en un recurso de Azure mediante Azure Portal
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: how-to
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 496774f034c861131624b552b1acefca9f399ea8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84700270"
---
# <a name="convert-a-legacy-direct-peering-to-an-azure-resource-by-using-the-azure-portal"></a>Conversión de un emparejamiento directo heredado en un recurso de Azure mediante Azure Portal

En este artículo se describe cómo convertir un emparejamiento directo heredado existente en un recurso de Azure mediante Azure Portal.

Si lo prefiere, puede completar esta guía mediante [PowerShell](howto-legacy-direct-powershell.md).

## <a name="before-you-begin"></a>Antes de empezar
* Revise los [requisitos previos](prerequisites.md) y el [Tutorial del emparejamiento directo](walkthrough-direct-all.md) antes de comenzar la configuración.


## <a name="convert-a-legacy-direct-peering-to-an-azure-resource"></a>Conversión de un emparejamiento directo heredado en un recurso de Azure

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Inicio de sesión en el portal y selección de su suscripción
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="convert-a-legacy-direct-peering"></a><a name=create></a>Conversión de un emparejamiento directo heredado

Como proveedor de servicios de Internet, puede convertir las conexiones de emparejamiento directo heredadas mediante la [creación de un emparejamiento]( https://go.microsoft.com/fwlink/?linkid=2129593).

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

#### <a name="launch-the-resource-and-configure-basic-settings"></a>Inicio de un recurso y configuración de las opciones básicas
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Configuración de conexiones y envío
[!INCLUDE [direct-peering-configuration](./includes/direct-portal-configuration-legacy.md)]

### <a name="verify-direct-peering"></a><a name=get></a>Verificación de un emparejamiento directo
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

## <a name="additional-resources"></a>Recursos adicionales

Para obtener más información, consulte las [preguntas más frecuentes sobre el emparejamiento de Internet](faqs.md).

## <a name="next-steps"></a>Pasos siguientes

* [Creación o modificación de un emparejamiento directo mediante el portal](howto-direct-portal.md)
