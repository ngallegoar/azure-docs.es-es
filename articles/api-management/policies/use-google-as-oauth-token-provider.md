---
title: 'Directiva de administración de API de muestra: autorización de acceso mediante el token de OAuth de Google'
titleSuffix: Azure API Management
description: 'Ejemplo de directiva de Azure API Management: muestra cómo autorizar el acceso a los puntos de conexión con Google como proveedor de tokens de OAuth.'
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: 0f6c9fe2146414f78e90d6ade1a00045cdf3a04f
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92078024"
---
# <a name="authorize-access-using-google-oauth-token"></a>Autorización de acceso con el token de OAuth de Google

Este artículo muestra un ejemplo de directiva de Azure API Management que demuestra cómo autorizar el acceso a los puntos de conexión con Google como proveedor de tokens de OAuth. Para establecer o modificar un código de la directiva, siga los pasos descritos en [Establecimiento o modificación de directivas](../set-edit-policies.md). Para ver otros ejemplos, consulte los [ejemplos de directiva](../policy-reference.md).

## <a name="policy"></a>Directiva

Pegue el código en el bloque de **entrada**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Simple Google OAuth validate-jwt.policy.xml)]

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las directivas APIM:

+ [Directivas de transformación](../api-management-transformation-policies.md)
+ [Ejemplos de directivas](../policy-reference.md)