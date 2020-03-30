---
title: 'Directiva de ejemplo de API Management: filtrado activado en la dirección IP al usar Application Gateway'
titleSuffix: Azure API Management
description: 'Directiva de ejemplo de Azure API Management: muestra cómo activar el filtro de solicitudes de direcciones IP al usar Application Gateway.'
services: api-management
documentationcenter: ''
author: jftl6y
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/13/2020
ms.author: joscot
ms.custom: fasttrack-new
ms.openlocfilehash: 45e16c9aa9e4b04e7225320951e9f839fae75ba3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75942365"
---
# <a name="filter-on-request-ip-address-when-using-an-application-gateway"></a>Activación del filtro de solicitudes de direcciones IP al usar Application Gateway

Este artículo incluye una directiva de ejemplo de Azure API Management que muestra cómo activar el filtro de la solicitud de dirección IP cuando se accede a la instancia de API Management mediante Application Gateway u otro intermediario. Para establecer o modificar un código de la directiva, siga los pasos descritos en [Establecimiento o modificación de directivas](../set-edit-policies.md). Para ver otros ejemplos, consulte los [ejemplos de directiva](../policy-samples.md).

## <a name="policy"></a>Directiva

Pegue el código en el bloque de **entrada**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Filter on IP Address when using Application Gateway.policy.xml)]

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las directivas APIM:

+ [Directivas de restricciones de acceso](../api-management-access-restriction-policies.md)
+ [Ejemplos de directivas](../policy-samples.md)
