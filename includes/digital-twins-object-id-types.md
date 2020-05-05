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
ms.openlocfilehash: cb43c8b8c952d8db6cf450a7015c22c85e7fe4b5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "76268291"
---
`objectIdType` (o **tipo de identificador de objeto**) hace referencia al tipo de identidad que se asigna a un rol. Además de los tipos `DeviceId` y `UserDefinedFunctionId`, los tipos de identificador de objeto se corresponden con las propiedades de los objetos de Azure Active Directory.

En la tabla siguiente se incluyen los tipos de identificador de objeto admitidos en Azure Digital Twins:

| Tipo | Descripción |
| --- | --- |
| UserId | Asigna un rol a un usuario. |
| deviceId | Asigna un rol a un dispositivo. |
| DomainName | Asigna un rol a un nombre de dominio. Cada usuario con el nombre de dominio especificado tiene los derechos de acceso del rol correspondiente. |
| TenantId | Asigna un rol a un inquilino. Cada usuario al que pertenezca el identificador de inquilino de Azure AD especificado tiene los derechos de acceso del rol correspondiente. |
| ServicePrincipalId | Asigna un rol a un identificador de objeto de entidad de servicio. |
| UserDefinedFunctionId | Asigna un rol a una función definida por el usuario (UDF). |