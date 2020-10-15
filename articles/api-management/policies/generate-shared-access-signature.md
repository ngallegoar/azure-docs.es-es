---
title: 'Ejemplo de directiva de API Management: generación de una firma de acceso compartido'
titleSuffix: Azure API Management
description: 'Ejemplo de directiva de Azure API Management: muestra cómo generar una firma de acceso compartido mediante expresiones y cómo reenviar la solicitud a Azure Storage con la directiva de reescritura del identificador URI.'
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
ms.openlocfilehash: f90c1e492d6da4f85fe86e4c5c76b89bdfaa4797
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92078432"
---
# <a name="generate-shared-access-signature"></a>Generación de una firma de acceso compartido

Este artículo muestra un ejemplo de directiva de Azure API Management: que muestra cómo generar una [firma de acceso compartido](../../storage/common/storage-sas-overview.md) mediante expresiones y reenviar la solicitud a Azure Storage con la directiva de reescritura del identificador URI. Para establecer o modificar un código de la directiva, siga los pasos descritos en [Establecimiento o modificación de directivas](../set-edit-policies.md). Para ver otros ejemplos, consulte los [ejemplos de directiva](../policy-reference.md).

## <a name="policy"></a>Directiva

Pegue el código en el bloque de **entrada**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Generate Shared Access Signature and forward request to Azure storage.policy.xml)]

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las directivas APIM:

+ [Directivas de transformación](../api-management-transformation-policies.md)
+ [Ejemplos de directivas](../policy-reference.md)