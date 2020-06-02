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
ms.openlocfilehash: 287344b325237843a549973ae61f569eae6dac93
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83202231"
---
# <a name="custom-vision-encryption-of-data-at-rest"></a>Cifrado de datos en reposo de Custom Vision

Custom Vision de Azure cifra automáticamente los datos al guardarlos en la nube. El cifrado de Custom Vision protege los datos y le ayuda a satisfacer los requisitos de cumplimiento normativo y seguridad de la organización.

[!INCLUDE [cognitive-services-about-encryption](../../../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Las claves que administre el cliente solo serán aquellos recursos disponibles que se hayan creado después del 11 de mayo de 2020. Para usar CMK con Custom Vision, tendrá que crear un nuevo recurso de Custom Vision. Una vez creado el recurso, puede usar Azure Key Vault para configurar la identidad administrada.

### <a name="regional-availability"></a>Disponibilidad regional

Actualmente, las claves administradas por el cliente están disponibles en estas regiones:

* Centro y Sur de EE. UU.
* Oeste de EE. UU. 2
* Este de EE. UU.
* US Gov - Virginia

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Formulario de solicitud de claves que administra el cliente de Custom Vision](https://aka.ms/cogsvc-cmk)
* [Más información sobre Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)


