---
title: 'Ejemplo de directiva de administración de API: autorización de la solicitud con un autorizador externo'
titleSuffix: Azure API Management
description: 'Ejemplo de directiva de Azure API Management: muestra cómo autorizar las solicitudes con el autorizador externo mediante la encapsulación de una lógica de autenticación o autorización personalizada o heredada.'
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/06/2018
ms.author: apimpm
ms.openlocfilehash: e38d92a13c9a66defc2d5090990b44a889cfd21c
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92076239"
---
# <a name="authorize-requests-using-external-authorizer"></a>Autorización de solicitudes mediante el autorizador externo

En este artículo se muestra un ejemplo de directiva de Azure API Management que explica cómo proteger el acceso seguro a la API mediante un autorizador externo que encapsula la lógica de autenticación o autorización personalizada. Para establecer o modificar un código de la directiva, siga los pasos descritos en [Establecimiento o modificación de directivas](../set-edit-policies.md). Para ver otros ejemplos, consulte los [ejemplos de directiva](../policy-reference.md).

## <a name="policy"></a>Directiva

Pegue el código en el bloque de **entrada**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Authorize requests using external authorizer.policy.xml)]

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las directivas APIM:

+ [Directivas de restricciones de acceso](../api-management-access-restriction-policies.md)
+ [Ejemplos de directivas](../policy-reference.md)