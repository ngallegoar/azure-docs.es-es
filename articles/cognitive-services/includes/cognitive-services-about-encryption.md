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
ms.openlocfilehash: 90d5aae559a317ed509d04c8db3310ff8a5de026
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "89069927"
---
## <a name="about-cognitive-services-encryption"></a>Información sobre el cifrado de Cognitive Services

Los datos se cifran y descifran con el cifrado [AES de 256 bits](https://en.wikipedia.org/wiki/FIPS_140-2) compatible con [FIPS 140-2](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard). El cifrado y el descifrado son transparentes, lo que significa que el cifrado y el acceso se administran automáticamente. Dado que los datos están protegidos de forma predeterminada, no es necesario modificar el código ni las aplicaciones para utilizar el cifrado.

## <a name="about-encryption-key-management"></a>Información sobre la administración de claves de cifrado

De forma predeterminada, su suscripción usa claves de cifrado administradas por Microsoft. También tiene la opción de administrar la suscripción con sus propias claves, denominadas claves administradas por el cliente (CMK). Las CMK ofrecen más flexibilidad para crear, rotar, deshabilitar y revocar controles de acceso. También permite auditar las claves de cifrado que se usan para proteger los datos. Si la opción CMK está configurada para su suscripción, se proporciona un cifrado doble, que ofrece una segunda capa de protección, a la vez que permite controlar la clave de cifrado a través de Azure Key Vault.