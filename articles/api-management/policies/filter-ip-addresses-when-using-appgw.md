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
ms.openlocfilehash: 8249cc543c6334841c8e5152d5d1ceb84d4097dc
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92076120"
---
# <a name="filter-on-request-ip-address-when-using-an-application-gateway"></a>Activación del filtro de solicitudes de direcciones IP al usar Application Gateway

Este artículo incluye una directiva de ejemplo de Azure API Management que muestra cómo activar el filtro de la solicitud de dirección IP cuando se accede a la instancia de API Management mediante Application Gateway u otro intermediario. Para establecer o modificar un código de la directiva, siga los pasos descritos en [Establecimiento o modificación de directivas](../set-edit-policies.md). Para ver otros ejemplos, consulte los [ejemplos de directiva](../policy-reference.md).

## <a name="policy"></a>Directiva

Pegue el código en el bloque de **entrada**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Filter on IP Address when using Application Gateway.policy.xml)]

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las directivas APIM:

+ [Directivas de restricciones de acceso](../api-management-access-restriction-policies.md)
+ [Ejemplos de directivas](../policy-reference.md)