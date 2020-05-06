---
title: archivo de inclusión
description: archivo de inclusión
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 01/15/2020
ms.custom: include file
ms.openlocfilehash: 864d693f2919922b5035f963103b2ce98600a51c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "76044950"
---
En la tabla siguiente se describen los roles que están disponibles en Azure Digital Twins:

| **Rol** | **Descripción** | **Identificador** |
| --- | --- | --- |
| Administrador del espacio | Permiso *CREAR*, *LEER*, *ACTUALIZAR* y *ELIMINAR* para el espacio especificado y todos los nodos inferiores. Permiso global. | 98e44ad7-28d4-4007-853b-b9968ad132d1 |
| Administrador de usuarios| Permiso *CREAR*, *LEER*, *ACTUALIZAR* y *ELIMINAR* para usuarios y objetos relacionados con usuarios. Permiso *LEER* para los espacios. | dfaac54c-f583-4dd2-b45d-8d4bbc0aa1ac |
| Administrador de dispositivos | Permiso *CREAR*, *LEER*, *ACTUALIZAR* y *ELIMINAR* para dispositivos y objetos relacionados con dispositivos. Permiso *LEER* para los espacios. | 3cdfde07-bc16-40d9-bed3-66d49a8f52ae |
| Administrador de claves | Permiso *CREAR*, *LEER*, *ACTUALIZAR* y *ELIMINAR* para las claves de acceso. Permiso *LEER* para los espacios. | 5a0b1afc-e118-4068-969f-b50efb8e5da6 |
| Administrador de tokens |  Permiso *LEER* y *ACTUALIZAR* para las claves de acceso. Permiso *LEER* para los espacios. | 38a3bb21-5424-43b4-b0bf-78ee228840c3 |
| Usuario |  Permiso *LEER* para espacios, sensores y usuarios, incluidos sus correspondientes objetos relacionados. | b1ffdb77-c635-4e7e-ad25-948237d85b30 |
| Especialista de soporte técnico |  Permiso *LEER* para todo, excepto las claves de acceso. | 6e46958b-dc62-4e7c-990c-c3da2e030969 |
| Instalador de dispositivos | Permiso *LEER* y *ACTUALIZAR* para dispositivos y sensores, incluidos sus correspondientes objetos relacionados. Permiso *LEER* para los espacios. | b16dd9fe-4efe-467b-8c8c-720e2ff8817c |
| Dispositivo de puerta de enlace | Permiso *CREAR* para sensores. Permiso *LEER* para dispositivos y sensores, incluidos sus correspondientes objetos relacionados. | d4c69766-e9bd-4e61-bfc1-d8b6e686c7a8 |