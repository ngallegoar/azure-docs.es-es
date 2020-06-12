---
title: Cifrado de datos en reposo de Content Moderator
titleSuffix: Azure Cognitive Services
description: Cifrado de datos en reposo de Content Moderator.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: egeaney
ms.openlocfilehash: 38fc21ee45db25f015a6b8b534b0d922efa636f2
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84310553"
---
# <a name="content-moderator-encryption-of-data-at-rest"></a>Cifrado de datos en reposo de Content Moderator

Content Moderator cifra automáticamente sus datos cuando se guardan en la nube, lo que ayuda a cumplir los objetivos de seguridad y cumplimiento normativo de la organización.

[!INCLUDE [cognitive-services-about-encryption](../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Las claves administradas por el cliente solo están disponibles en el plan de tarifa E0. Para solicitar la capacidad de usar claves administradas por el cliente, rellene y envíe el [formulario de solicitud de claves administradas por el cliente de Content Moderator ](https://aka.ms/cogsvc-cmk). Tardará de 3 a 5 días hábiles aproximadamente en recibir una respuesta sobre el estado de la solicitud. En función de la demanda, es posible que se coloque en una cola y se apruebe a medida que haya espacio disponible. Una vez recibida la aprobación para usar CMK con el servicio Content Moderator, deberá crear un nuevo recurso de Content Moderator y seleccionar el plan de tarifa E0. Una vez creado el recurso de Content Moderator con el plan de tarifa E0, puede usar Azure Key Vault para configurar la identidad administrada.

[!INCLUDE [cognitive-services-cmk](../includes/cognitive-services-cmk-regions.md)]

[!INCLUDE [cognitive-services-cmk](../includes/configure-customer-managed-keys.md)]

## <a name="enable-data-encryption-for-your-content-moderator-team"></a>Habilitación del cifrado de datos para el equipo de Content Moderator

Para habilitar el cifrado de datos para su equipo de revisión de Content Moderator, consulte [Inicio rápido: Cómo familiarizarse con Content Moderator](quick-start.md#create-a-review-team).  

> [!NOTE]
> Tendrá que proporcionar un _identificador de recurso_ con el plan de tarifa E0 de Content Moderator.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener una lista completa de los servicios que admiten CMK, consulte [Claves administradas por el cliente para Cognitive Services](../encryption/cognitive-services-encryption-keys-portal.md).
* [¿Qué es Azure Key Vault?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
* [Formulario de solicitud de claves administradas por el cliente de Cognitive Services](https://aka.ms/cogsvc-cmk)

