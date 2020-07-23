---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/10/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: c303527c7411ead585e70fc2e31db4dd2d35e996
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259815"
---
- Solo se admiten [software y claves RSA de HSM](../articles/key-vault/keys/about-keys.md) con un tamaño de 2048 bits, 3072 bits y 4096 bits, ninguna otra clave o tamaño.
    - Las claves de [HSM](../articles/key-vault/keys/hsm-protected-keys.md) requieren el nivel **premium** de los almacenes Azure Key Vault.
- Los discos creados a partir de imágenes personalizadas que están cifradas con cifrado del lado servidor y las claves administradas por el cliente deben cifrarse con las mismas claves administradas por el cliente y deben estar en la misma suscripción.
- Las instantáneas creadas a partir de discos que están cifrados con cifrado del lado servidor y claves administradas por el cliente deben cifrarse con las mismas claves administradas por el cliente.
- Todos los recursos relacionados con las claves administradas por el cliente (instancias de Azure Key Vault, conjuntos de cifrado de disco, máquinas virtuales, discos e instantáneas) deben estar en la misma suscripción y región.
- Los discos, las instantáneas y las imágenes cifrados con claves administradas por el cliente no se pueden trasladar a otra suscripción.
- Los discos administrados cifrados mediante claves administradas por el cliente tampoco se pueden cifrar con Azure Disk Encryption.
- Solo puede crear hasta 50 conjuntos de cifrado de disco por región y por suscripción.
- Para obtener información sobre el uso de claves administradas por el cliente con galerías de imágenes compartidas, consulte [Versión preliminar: uso de claves administradas por el cliente para el cifrado de imágenes](../articles/virtual-machines/image-version-encryption.md).
