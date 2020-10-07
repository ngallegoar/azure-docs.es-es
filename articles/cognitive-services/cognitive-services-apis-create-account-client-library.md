---
title: Creación de recursos de Cognitive Services mediante la biblioteca cliente de Administración de Azure
titleSuffix: Azure Cognitive Services
description: Cree y administre los recursos de Azure Cognitive Services mediante la biblioteca cliente de Administración de Azure.
services: cognitive-services
keywords: cognitive services, inteligencia cognitiva, soluciones cognitivas, servicios de inteligencia artificial, servicios de IA
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: quickstart
ms.date: 09/14/2020
ms.author: pafarley
zone_pivot_groups: programming-languages-set-ten
ms.openlocfilehash: e8628d051db7f5066a81171567f6f7e54fb0ab97
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91262457"
---
# <a name="quickstart-create-a-cognitive-services-resource-using-the-azure-management-client-library"></a>Inicio rápido: Creación de un recurso de Cognitive Services mediante la biblioteca cliente de Administración de Azure

Use este inicio rápido para crear y administrar recursos de Azure Cognitive Services mediante la biblioteca cliente de Administración de Azure.

Azure Cognitive Services es un conjunto de servicios en la nube con API de REST y SDK de biblioteca cliente que ayudan a los desarrolladores a integrar la inteligencia cognitiva en las aplicaciones sin tener inteligencia artificial (IA) directa ni habilidades o conocimientos sobre ciencia de datos. Azure Cognitive Services permite a los desarrolladores agregar fácilmente características cognitivas en sus aplicaciones con soluciones cognitivas que pueden ver, oír, hablar, comprender e incluso empezar a pensar.

Los servicios de inteligencia artificial individuales se representan por medio de los [recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal) de Azure que se crean en la suscripción de Azure. Después de crear un recurso, puede usar las claves y el punto de conexión generados para autenticar las aplicaciones.

::: zone pivot="programming-language-csharp"

[!INCLUDE [C# SDK quickstart](includes/quickstarts/management-csharp.md)]

::: zone-end

::: zone pivot="programming-language-java"

[!INCLUDE [Java SDK quickstart](includes/quickstarts/management-java.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

[!INCLUDE [NodeJS SDK quickstart](includes/quickstarts/management-node.md)]

::: zone-end

::: zone pivot="programming-language-python"

[!INCLUDE [Python SDK quickstart](includes/quickstarts/management-python.md)]

::: zone-end
