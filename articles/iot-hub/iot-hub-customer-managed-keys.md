---
title: Cifrado de datos en reposo de Azure IoT Hub a través de claves administradas por el cliente | Microsoft Docs
description: Cifrado de datos en reposo con claves administradas por el cliente para IoT Hub
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 06/17/2020
ms.author: asrastog
ms.openlocfilehash: 83d2fa59654e038586a7f23eedbe7c656873f35c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84976581"
---
# <a name="encryption-of-data-at-rest-with-customer-managed-keys-for-iot-hub"></a>Cifrado de datos en reposo con claves administradas por el cliente para IoT Hub

IoT Hub admite el cifrado de datos en reposo con claves administradas por el cliente (CMK), también conocido como Bring your own key (BYOK). Azure IoT Hub proporciona cifrado de datos en reposo y en tránsito a medida que se escribe en nuestros centros de datos y los descifra automáticamente mientras accede a ellos. De manera predeterminada, IoT Hub usa claves administradas por Microsoft para cifrar los datos en reposo. Con CMK, puede lograr otro nivel de cifrado adicional al cifrado predeterminado y puede optar por cifrar los datos en reposo con una clave de cifrado de claves, administrada a través de [Azure Key Vault](https://azure.microsoft.com/services/key-vault/). Esto le ofrece más flexibilidad para crear, rotar, deshabilitar y revocar controles de acceso. Si BYOK está configurado para IoT Hub, también proporcionamos el cifrado doble, que ofrece una segunda capa de protección, a la vez que permite controlar la clave de cifrado a través de Azure Key Vault.

Para usar esta funcionalidad, debe crear una nueva instancia de IoT Hub (nivel básico o estándar). Para probar esta funcionalidad, póngase en contacto con nosotros a través del [soporte técnico de Microsoft](https://azure.microsoft.com/support/create-ticket/). Mencione el nombre de la empresa y el Id. de suscripción cuando se ponga en contacto con el soporte técnico de Microsoft.


## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre IoT Hub](https://docs.microsoft.com/azure/iot-hub/about-iot-hub)

* [Más información sobre Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
