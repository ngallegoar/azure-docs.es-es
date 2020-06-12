---
title: Cifrado de datos en reposo de Custom Vision
titleSuffix: Azure Cognitive Services
description: Cifrado de datos en reposo de Custom Vision.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: egeaney
ms.openlocfilehash: 85cf251db69d33f02e928eaea6a848f20ae7d923
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84310298"
---
# <a name="custom-vision-encryption-of-data-at-rest"></a>Cifrado de datos en reposo de Custom Vision

Custom Vision de Azure cifra automáticamente los datos al guardarlos en la nube. El cifrado de Custom Vision protege los datos y le ayuda a satisfacer los requisitos de cumplimiento normativo y seguridad de la organización.

[!INCLUDE [cognitive-services-about-encryption](../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Las claves que administre el cliente solo serán aquellos recursos disponibles que se hayan creado después del 11 de mayo de 2020. Para usar CMK con Custom Vision, tendrá que crear un nuevo recurso de Custom Vision. Una vez creado el recurso, puede usar Azure Key Vault para configurar la identidad administrada.

## <a name="regional-availability"></a>Disponibilidad regional

Actualmente, las claves administradas por el cliente están disponibles en estas regiones:

* Centro y Sur de EE. UU.
* Oeste de EE. UU. 2
* Este de EE. UU.
* US Gov - Virginia

[!INCLUDE [cognitive-services-cmk](../includes/configure-customer-managed-keys.md)]

## <a name="next-steps"></a>Pasos siguientes

* Para obtener una lista completa de los servicios que admiten CMK, consulte [Claves administradas por el cliente para Cognitive Services](../encryption/cognitive-services-encryption-keys-portal.md).
* [¿Qué es Azure Key Vault?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
* [Formulario de solicitud de claves administradas por el cliente de Cognitive Services](https://aka.ms/cogsvc-cmk)
