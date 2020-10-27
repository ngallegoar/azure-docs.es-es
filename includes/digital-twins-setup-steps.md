---
author: baanders
description: archivo de inclusión para información general de los pasos de la configuración de Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 10/14/2020
ms.author: baanders
ms.openlocfilehash: 5ab6812d144122c61018ad740892db869a7ba43d
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92205508"
---
>[!NOTE]
>Estas operaciones están pensadas para que las realice un usuario con la capacidad de administrar los recursos y el acceso de usuario en la suscripción de Azure. Aunque algunos pasos se pueden realizar con permisos inferiores, será necesaria la colaboración de alguien con estos permisos para configurar por completo una instancia que se pueda usar. Para más información, consulte la sección [*Requisitos previos: permisos necesarios*](#prerequisites-permission-requirements) que aparece a continuación.

La configuración completa de una instancia nueva de Azure Digital Twins consta de dos partes:
1. **Creación de la instancia**
2. **Configuración de permisos de acceso de usuarios** : los usuarios de Azure deben tener el rol *Propietario de Azure Digital Twins (versión preliminar)* en la instancia de Azure Digital Twins para poder administrarla y administrar sus datos. En este paso, como propietario o administrador de la suscripción de Azure, se asignará este rol a la persona que vaya a administrar la instancia de Azure Digital Twins. Puede ser usted mismo o alguna otra persona de la organización.