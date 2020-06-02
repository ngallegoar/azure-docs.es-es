---
title: Cifrado de datos en reposo del servicio Form Recognizer
titleSuffix: Azure Cognitive Services
description: Cifrado de datos en reposo de Form Recognizer.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: egeaney
ms.openlocfilehash: cafe170c4f4485791bbd65471a43d1d5f9726775
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83202223"
---
# <a name="form-recognizer-encryption-of-data-at-rest"></a>Cifrado de datos en reposo de Form Recognizer

Form Recognizer de Azure cifra automáticamente los datos al guardarlos en la nube. El cifrado de Form Recognizer protege los datos y le ayuda a satisfacer los requisitos de cumplimiento normativo y de seguridad de la organización.

[!INCLUDE [cognitive-services-about-encryption](../../../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Las claves que administre el cliente solo serán aquellos recursos disponibles que se hayan creado después del 11 de mayo de 2020. Para usar CMK con Form Recognizer, deberá crear un nuevo recurso de Form Recognizer. Una vez creado el recurso, puede usar Azure Key Vault para configurar la identidad administrada.

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk-regions.md)]

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Formulario de solicitud de claves que administra el cliente de Form Recognizer](https://aka.ms/cogsvc-cmk)
* [Más información sobre Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)


