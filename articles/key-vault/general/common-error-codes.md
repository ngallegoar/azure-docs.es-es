---
title: Códigos de error comunes para Azure Key Vault | Microsoft Docs
description: Códigos de error comunes para Azure Key Vault
services: key-vault
author: sebansal
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: reference
ms.date: 09/29/2020
ms.author: mbaldwin
ms.openlocfilehash: 25c79229a09db912903fba825f0d48f571880745
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91876842"
---
# <a name="common-error-codes-for-azure-key-vault"></a>Códigos de error comunes para Azure Key Vault

Una operación en un almacén de claves de Azure puede devolver los códigos de error que se muestran en la tabla siguiente.

| Código de error | Mensaje de usuario |
|--|--|
| VaultAlreadyExists |  El almacén de claves especificado ya existe (en estado de eliminación temporal o en otra suscripción). |
| VaultNameNotValid |  El nombre del almacén debe tener 24 caracteres alfanuméricos y empezar por una letra. |
| AccessDenied |  Es posible que falten permisos en la directiva de acceso para realizar esa operación. |
| ForbiddenByFirewall |  La dirección de cliente no está autorizada y el autor de llamada no es un servicio de confianza. |
| ConflictError |  Está solicitando varias operaciones en el mismo elemento.  |
| RegionNotSupported |  No se admite la región de Azure especificada para este recurso. |
| SkuNotSupported |  No se admite el tipo de SKU especificado para este recurso. |
| ResourceNotFound |  No se encuentra el recurso de Azure especificado. |
| CertificateExpired |  Compruebe la fecha de expiración y el período de validez del certificado. |


## <a name="next-steps"></a>Pasos siguientes

- Consulte la [guía del desarrollador de Azure Key Vault](developers-guide.md).
- Más información acerca de la [Autenticación en Key Vault](authentication.md).
