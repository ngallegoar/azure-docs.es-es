---
author: baanders
description: archivo de inclusión para información general de los pasos de la configuración de Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 7/22/2020
ms.author: baanders
ms.openlocfilehash: b8751685cfd273171cb8a5b500e4bed41ce2f59e
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2020
ms.locfileid: "87407595"
---
>[!NOTE]
>Estas operaciones están pensadas para que las complete un usuario con un rol *Propietario* en la suscripción de Azure. Aunque se pueden completar algunas partes sin este permiso elevado, se requerirá la cooperación de un propietario para configurar completamente una instancia utilizable. Para más información, consulte la sección [*Requisitos previos: permisos necesarios*](#prerequisites-permission-requirements) que aparece a continuación.

La configuración completa de una instancia nueva de Azure Digital Twins consta de tres partes:
1. **Creación de la instancia**
2. **Configuración de permisos de acceso de usuarios**: los usuarios de Azure deben tener el rol *Propietario de Azure Digital Twins (versión preliminar)* en la instancia de Azure Digital Twins para poder administrarla y administrar sus datos. En este paso, como propietario de la suscripción de Azure, se asignará este rol a la persona que vaya a administrar la instancia de Azure Digital Twins. Puede ser usted mismo o alguna otra persona de la organización.
3. **Configuración de los permisos de acceso para aplicaciones cliente**: es habitual escribir una aplicación cliente que se utilizará para interactuar con la instancia. Para que la aplicación cliente acceda a su instancia de Azure Digital Twins, debe configurar un *registro de aplicaciones* en [Azure Active Directory](../articles/active-directory/fundamentals/active-directory-whatis.md) que la aplicación cliente usará para autenticarse en la instancia.

Para continuar, necesitará una suscripción de Azure. [Aquí](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) puede configurar una gratis.
