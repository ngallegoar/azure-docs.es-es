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
ms.openlocfilehash: a36e15a56a5a4c8a637120ca730ae1da764d376d
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93422910"
---
# <a name="common-error-codes-for-azure-key-vault"></a>Códigos de error comunes para Azure Key Vault

Una operación en un almacén de claves de Azure puede devolver los códigos de error que se muestran en la tabla siguiente.

| Código de error | Mensaje de usuario |
|--|--|
| VaultAlreadyExists |  Se ha producido un error al intentar crear un nuevo almacén de claves con el nombre especificado porque ese nombre ya está en uso. Si ha eliminado recientemente un almacén de claves con este nombre, es posible que todavía se encuentre en estado de eliminación temporal. Puede comprobar [aquí](https://docs.microsoft.com/azure/key-vault/general/key-vault-recovery?tabs=azure-portal#list-recover-or-purge-a-soft-deleted-key-vault) si existe en el estado de eliminación temporal. |
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
