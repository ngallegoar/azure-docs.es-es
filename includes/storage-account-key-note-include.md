---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/06/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: ba715d510dc296ffa8f9c0ee58841f284416a118
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86027345"
---
## <a name="protect-your-access-keys"></a>Protección de las claves de acceso

Las claves de acceso de la cuenta de almacenamiento son similares a una contraseña raíz de la cuenta de almacenamiento. Siempre debe proteger las claves de acceso. Use Azure Key Vault para administrar y rotar las claves de forma segura. Evite distribuirlas a otros usuarios, codificarlas de forma rígida o guardarlas en un archivo de texto sin formato al que puedan acceder otros usuarios. Rote sus claves si cree que se han puesto en peligro.

> [!NOTE]
> Microsoft recomienda el uso de Azure Active Directory (Azure AD) para autorizar solicitudes de datos para blobs y colas, si es posible, en lugar de la clave compartida. Azure AD proporciona una mayor seguridad y facilidad de uso a través de la clave compartida. Para obtener más información sobre la autorización de acceso a datos con Azure AD, consulte [Autorización del acceso a los blobs y las colas de Azure con Azure Active Directory](../articles/storage/common/storage-auth-aad.md).
