---
title: Cifrado de datos en reposo del servicio Face
titleSuffix: Azure Cognitive Services
description: Cifrado de datos en reposo del servicio Face.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: egeaney
ms.openlocfilehash: 1e0275c91b2243132650be7af256071589091c4b
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83201948"
---
# <a name="face-service-encryption-of-data-at-rest"></a>Cifrado de datos en reposo del servicio Face

El servicio Face cifra automáticamente los datos al guardarlos en la nube. El cifrado del servicio Face protege los datos y le ayuda a satisfacer los requisitos de cumplimiento y de seguridad de la organización.

[!INCLUDE [cognitive-services-about-encryption](../../../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Las claves administradas por el cliente solo están disponibles en el plan de tarifa E0. Para solicitar la capacidad de usar claves administradas por el cliente, rellene y envíe el [formulario de solicitud de claves administradas por el cliente del servicio Face](https://aka.ms/cogsvc-cmk). Tardará de 3 a 5 días hábiles aproximadamente en recibir una respuesta sobre el estado de la solicitud. En función de la demanda, es posible que se coloque en una cola y se apruebe a medida que haya espacio disponible. Una vez recibida la aprobación para usar CMK con el servicio Face, deberá crear un recurso de Face y seleccionar el plan de tarifa E0. Después de crear el recurso de Face con el plan de tarifa E0, puede usar Azure Key Vault para configurar la identidad administrada.

### <a name="regional-availability"></a>Disponibilidad regional

Actualmente, las claves administradas por el cliente están disponibles en estas regiones:

* Centro y Sur de EE. UU.
* Oeste de EE. UU. 2
* Este de EE. UU.
* US Gov - Virginia

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Formulario de solicitud de claves administradas por el cliente del servicio Face](https://aka.ms/cogsvc-cmk)
* [Más información sobre Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)


