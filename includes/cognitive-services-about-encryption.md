---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: erindormier
ms.service: cognitive-services
ms.topic: include
ms.date: 03/11/2020
ms.author: egeaney
ms.custom: include
ms.openlocfilehash: 2a348827b1c992e0ef0a4592cc0b9c5c0fa0ca19
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79372017"
---
## <a name="about-cognitive-services-encryption"></a>Información sobre el cifrado de Cognitive Services

Los datos se cifran y descifran con el cifrado [AES de 256 bits](https://en.wikipedia.org/wiki/FIPS_140-2) compatible con [FIPS 140-2](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard). El cifrado y el descifrado son transparentes, lo que significa que el cifrado y el acceso se administran automáticamente. Dado que los datos están protegidos de forma predeterminada, no es necesario modificar el código ni las aplicaciones para utilizar el cifrado.

## <a name="about-encryption-key-management"></a>Información sobre la administración de claves de cifrado

De forma predeterminada, su suscripción usa claves de cifrado administradas por Microsoft. Si usa un plan de tarifa que admita claves administradas por el cliente, puede ver la configuración de cifrado del recurso en la sección **Cifrado** de [Azure Portal](https://portal.azure.com), tal como se muestra en la siguiente imagen.

![Visualización de la configuración de cifrado](../articles/cognitive-services/media/cognitive-services-encryption/encryptionblade.png)

En el caso de las suscripciones que solo admiten claves de cifrado administradas por Microsoft, no tendrá una sección **Cifrado**.

## <a name="customer-managed-keys-with-azure-key-vault"></a>Claves administradas por el cliente con Azure Key Vault

También tiene una opción para administrar su suscripción con sus propias claves. Las claves administradas por el cliente (CMK), también conocidas como Bring Your Own Key (BYOK), ofrecen más flexibilidad para crear, girar, deshabilitar y revocar controles de acceso. También permite auditar las claves de cifrado que se usan para proteger los datos.
