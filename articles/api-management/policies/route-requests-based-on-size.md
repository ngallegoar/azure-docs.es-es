---
title: 'Directiva de muestra de API Management: enrutamiento de solicitudes basado en el tamaño del cuerpo del mensaje'
titleSuffix: Azure API Management
description: 'Ejemplo de directiva de Azure API Management: muestra cómo enrutar las solicitudes en función del tamaño de su cuerpo.'
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
ms.openlocfilehash: e59aece0be39bf671e3d1e62f1a0e4c2a42a1ec8
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92076477"
---
# <a name="route-the-request-based-on-the-size-of-its-body"></a>Enrutamiento de la solicitud en función del tamaño de su cuerpo

Este artículo incluye un ejemplo de directiva de Azure API Management que muestra cómo enrutar las solicitudes en función del tamaño del cuerpo. Para establecer o modificar un código de la directiva, siga los pasos descritos en [Establecimiento o modificación de directivas](../set-edit-policies.md). Para ver otros ejemplos, consulte los [ejemplos de directiva](../policy-reference.md).

## <a name="policy"></a>Directiva

Pegue el código en el bloque de **entrada**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Route requests based on size.policy.xml)]

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las directivas APIM:

+ [Directivas de transformación](../api-management-transformation-policies.md)
+ [Ejemplos de directivas](../policy-reference.md)