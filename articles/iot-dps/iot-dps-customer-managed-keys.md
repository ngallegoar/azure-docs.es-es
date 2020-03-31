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
ms.openlocfilehash: b065135715868094b7e248aebfca35f3b55a4c48
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77674786"
---
# <a name="encryption-of-data-at-rest-with-customer-managed-keys-for-device-provisioning-service"></a>Cifrado de datos en reposo con claves administradas por el cliente para Device Provisioning Service

## <a name="overview"></a>Información general

Device Provisioning Service (DPS) admite el cifrado de datos en reposo con claves administradas por el cliente (CMK), también conocido como Bring your own key (BYOK). DPS ofrece cifrado de datos en reposo y en tránsito. De manera predeterminada, DPS usa claves administradas por Microsoft para cifrar los datos. Gracias a la compatibilidad con CMK, ahora los clientes tienen la opción de cifrar los datos en reposo con una clave de cifrado (administrada por los clientes) mediante [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).

Para usar esta funcionalidad, debe crear una nueva instancia de DPS en una de las siguientes regiones: Este de EE. UU., Oeste de EE. UU. 2 o Centro-sur de EE. UU. Para probar esta funcionalidad, póngase en contacto con nosotros a través del [soporte técnico de Microsoft](https://azure.microsoft.com/support/create-ticket/). Mencione el nombre de la empresa y el Id. de suscripción cuando se ponga en contacto con el soporte técnico de Microsoft.

## <a name="next-steps"></a>Pasos siguientes

* [Obtener más información acerca de Device Provisioning Service](https://docs.microsoft.com/azure/iot-dps/)

* [Más información sobre Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)