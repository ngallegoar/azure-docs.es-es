---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/20/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: aec1faa4de1149f08fb6fbc1cc5bf3aa2ab6becd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75371790"
---
Una firma de acceso compartido (SAS) le permite conceder acceso limitado a los contenedores y blobs de la cuenta de almacenamiento. Cuando crea una SAS, especifica sus restricciones, incluidos los recursos de Azure Storage a los que puede acceder un cliente, los permisos que tiene en esos recursos y el tiempo durante el cual la SAS es válida.

Cada SAS se firma con una clave. Puede firmar una SAS de dos maneras:

- Con una clave creada con las credenciales de Azure Active Directory (Azure AD). Una SAS firmada con credenciales de Azure AD es una SAS de *delegación de usuario*.
- Con la clave de la cuenta de almacenamiento. Tanto una *SAS de servicio* como una *SAS de cuenta* se firman con la clave de la cuenta de almacenamiento.

Una SAS de delegación de usuario ofrece seguridad superior para una SAS firmada con la clave de la cuenta de almacenamiento. Microsoft recomienda el uso de una SAS de delegación de usuario siempre que sea posible. Para más información, consulte [Concesión de acceso limitado a datos con firmas de acceso compartido (SAS)](../articles/storage/common/storage-sas-overview.md).
