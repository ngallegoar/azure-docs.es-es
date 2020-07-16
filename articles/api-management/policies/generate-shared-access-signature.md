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
ms.openlocfilehash: 62a6e8c9fdec3b30bd193e9887d7e0cb7926e73e
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86243008"
---
# <a name="generate-shared-access-signature"></a>Generación de una firma de acceso compartido

Este artículo muestra un ejemplo de directiva de Azure API Management: que muestra cómo generar una [firma de acceso compartido](../../storage/common/storage-sas-overview.md) mediante expresiones y reenviar la solicitud a Azure Storage con la directiva de reescritura del identificador URI. Para establecer o modificar un código de la directiva, siga los pasos descritos en [Establecimiento o modificación de directivas](../set-edit-policies.md). Para ver otros ejemplos, consulte los [ejemplos de directiva](../policy-samples.md).

## <a name="policy"></a>Directiva

Pegue el código en el bloque de **entrada**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Generate Shared Access Signature and forward request to Azure storage.policy.xml)]

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las directivas APIM:

+ [Directivas de transformación](../api-management-transformation-policies.md)
+ [Ejemplos de directivas](../policy-samples.md)
