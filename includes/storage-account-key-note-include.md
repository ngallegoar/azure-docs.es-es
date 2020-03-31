---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/12/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 95639caddfa627290f4e7153d50f9f7799a1f320
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "75460561"
---
## <a name="protect-your-access-keys"></a>Protección de las claves de acceso

Las claves de acceso de la cuenta de almacenamiento son similares a una contraseña raíz de la cuenta de almacenamiento. Siempre debe proteger las claves de acceso. Use Azure Key Vault para administrar y rotar las claves de forma segura. Evite distribuirlas a otros usuarios, codificarlas de forma rígida o guardarlas en un archivo de texto sin formato al que puedan acceder otros usuarios. Rote sus claves si cree que se han puesto en peligro.

Si es posible, use Azure Active Directory (Azure AD) para autorizar solicitudes a Blob Storage y Queue Storage en lugar de la clave compartida. Azure AD proporciona una mayor seguridad y facilidad de uso a través de la clave compartida. Para obtener más información sobre la autorización de acceso a datos con Azure AD, consulte [Autorización del acceso a los blobs y las colas de Azure con Azure Active Directory](../articles/storage/common/storage-auth-aad.md).
