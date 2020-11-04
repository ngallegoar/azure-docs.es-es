---
title: Cifrado del servicio Metrics Advisor
titleSuffix: Azure Cognitive Services
description: Cifrado de datos en reposo del servicio Metrics Advisor.
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: conceptual
ms.date: 09/10/2020
ms.author: mbullwin
ms.openlocfilehash: 5d41500a9c53e38cd36f0feba602e0e1baa5da2c
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2020
ms.locfileid: "92909750"
---
# <a name="metrics-advisor-service-encryption-of-data-at-rest"></a>Cifrado de datos en reposo del servicio Metrics Advisor

El servicio Metrics Advisor cifra automáticamente los datos al guardarlos en la nube. El cifrado del servicio Metrics Advisor protege los datos y lo ayuda a satisfacer los requisitos de cumplimiento y de seguridad de la organización.

[!INCLUDE [cognitive-services-about-encryption](../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Las claves administradas por el cliente solo están disponibles en el plan de tarifa E0. Para solicitar la capacidad de usar claves administradas por el cliente, rellene y envíe el [formulario de solicitud de claves administradas por el cliente del servicio Metrics Advisor](https://aka.ms/cogsvc-cmk). Tardará de tres a cinco días hábiles aproximadamente en recibir una respuesta sobre el estado de la solicitud. En función de la demanda, es posible que se coloque en una cola y se apruebe a medida que haya espacio disponible. Una vez recibida la aprobación para usar CMK con el servicio Metrics Advisor, deberá crear un nuevo recurso de Metrics Advisor y seleccionar el plan de tarifa E0. Después de crear el recurso de Metrics Advisor con el plan de tarifa E0, puede usar Azure Key Vault para configurar la identidad administrada.

[!INCLUDE [cognitive-services-cmk](../includes/cognitive-services-cmk-regions.md)]

[!INCLUDE [cognitive-services-cmk](../includes/configure-customer-managed-keys.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Formulario de solicitud de claves administradas por el cliente del servicio Metrics Advisor](https://aka.ms/cogsvc-cmk)
* [Más información sobre Azure Key Vault](../../key-vault/general/overview.md)