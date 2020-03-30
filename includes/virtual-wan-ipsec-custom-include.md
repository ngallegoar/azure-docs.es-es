---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 10/07/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: f7167cbb26e69941cade01ab8c0b8d9dc633f0d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "72168385"
---
Al trabajar con directivas IPsec personalizadas, tenga en cuenta los siguientes requisitos:

* **IKE**: en el caso de IKE, puede seleccionar cualquier parámetro de Cifrado de IKE, más cualquier parámetro de Integridad de IKE, más cualquier parámetro de Grupo DH.
* **IPsec**: en el caso de IPsec, puede seleccionar cualquier parámetro de Cifrado de IPsec, más cualquier parámetro de Integridad de IPsec, más PFS. Si cualquiera de parámetros de Cifrado de IPsec o Integridad de IPsec es GCM, los parámetros de ambos valores deben ser GCM.

>[!NOTE]
> Con las directivas IPsec personalizadas, no existe el concepto de respondedor y iniciador (a diferencia de las directivas IPsec predeterminadas). Ambos lados (entorno local y puerta de enlace de VPN de Azure) usarán la misma configuración para IKE fase 1 e IKE fase 2. Se admiten los protocolos IKEv1 e IKEv2. No se admite Azure solo como respondedor.
>

**Parámetros y valores disponibles**

| Configuración | Parámetros |
|--- |--- |
| Cifrado de IKE | AES256, AES192, AES128 |
| Integridad de IKE | SHA384, SHA256, SHA1 |
| Grupo DH | DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2 |
| Cifrado IPsec | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128 |
| Integridad de IPsec | GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1 |
| Grupo PFS | PFS24, ECP384, ECP256, PFS2048, PFS2 |
