---
title: Cifrado de datos de Azure Device Provisioning Service en reposo mediante claves administradas por el cliente | Microsoft Docs
description: Cifrado de datos en reposo con claves administradas por el cliente para Device Provisioning Service
author: chrissie926
manager: nberdy
ms.service: iot-dps
services: iot-dps
ms.topic: conceptual
ms.date: 02/24/2020
ms.author: menchi
ms.openlocfilehash: e9b5a549d550f117e080fc64193fbe29658f401d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85298232"
---
# <a name="encryption-of-data-at-rest-with-customer-managed-keys-for-device-provisioning-service"></a>Cifrado de datos en reposo con claves administradas por el cliente para Device Provisioning Service

## <a name="overview"></a>Información general

Device Provisioning Service (DPS) admite el cifrado de datos en reposo con claves administradas por el cliente (CMK), también conocido como Bring your own key (BYOK). DPS proporciona cifrado de datos en reposo y en tránsito a medida que se escribe en nuestros centros de datos, y los descifra automáticamente mientras accede a ellos. De manera predeterminada, DPS usa claves administradas por Microsoft para cifrar los datos en reposo. Con CMK, puede lograr un nivel de cifrado adicional al cifrado de plataforma predeterminado optando por cifrar los datos en reposo con una clave de cifrado de claves, administrada mediante [Azure Key Vault](https://azure.microsoft.com/services/key-vault/). Esto le ofrece la flexibilidad de crear, rotar, deshabilitar y revocar las claves. Si CMK se configura para el DPS, esto implica que el cifrado doble está habilitado con dos niveles de protección que protegen activamente los datos. 

Para usar esta funcionalidad, debe crear una nueva instancia de DPS. Para probar esta funcionalidad, póngase en contacto con nosotros a través del [soporte técnico de Microsoft](https://azure.microsoft.com/support/create-ticket/). Mencione el nombre de la empresa y el Id. de suscripción cuando se ponga en contacto con el soporte técnico de Microsoft.


## <a name="next-steps"></a>Pasos siguientes

* [Obtener más información acerca de Device Provisioning Service](https://docs.microsoft.com/azure/iot-dps/)

* [Más información sobre Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
