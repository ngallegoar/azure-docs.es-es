---
title: Cómo personalizar una regla de sincronización en Azure AD Connect | Microsoft Docs
description: En este tema se proporcionan los pasos para solucionar problemas relacionados con la instalación de Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: a65d4c477d0e3aa9d5feea53e3e667ece651c83f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "60351075"
---
# <a name="how-to-customize-a-synchronization-rule"></a>Cómo personalizar una regla de sincronización

## <a name="recommended-steps"></a>**Pasos recomendados**

Puede usar el editor de reglas de sincronización para editar o crear una nueva regla de sincronización. Debe ser un usuario avanzado para realizar cambios en las reglas de sincronización. Cualquier cambio incorrecto puede provocar la eliminación de objetos en el directorio de destino. Lea los [documentos recomendados](#recommended-documents) para adquirir experiencia sobre las reglas de sincronización. Para modificar una regla de sincronización, realice los pasos siguientes:

* Inicie el editor de sincronización desde el menú de la aplicación de escritorio, tal como se muestra a continuación:

    ![Menú del editor de reglas de sincronización](media/how-to-connect-create-custom-sync-rule/how-to-connect-create-custom-sync-rule/syncruleeditormenu.png)

* Con el fin de personalizar una regla de sincronización predeterminada, haga clic en el botón "Editar" del editor de reglas de sincronización para clonar la regla existente. De esta forma, se creará una copia de la regla predeterminada estándar y la original se deshabilitará. Guarde la regla clonada con una prioridad inferior a 100.  La prioridad determina qué regla gana (valor numérico inferior) en caso de que exista un conflicto en el flujo de atributos.

    ![Editor de reglas de sincronización](media/how-to-connect-create-custom-sync-rule/how-to-connect-create-custom-sync-rule/clonerule.png)

* Lo ideal es que, al modificar un atributo determinado, solo se conserve el atributo modificado en la regla clonada.  Después, se debe habilitar la regla predeterminada, de forma que el atributo modificado proceda de la regla clonada y los otros atributos se tomen de la regla estándar predeterminada. 

* Tenga en cuenta que en los casos en los que el valor calculado del atributo modificado sea NULL en la regla clonada y no sea NULL en la regla estándar predeterminada, el valor que no es NULL ganará y reemplazará al valor NULL. Si no quiere que al valor NULL lo reemplace el valor que no es NULL, asigne el valor AuthoritativeNull en la regla clonada.

* Para modificar una regla **de salida**, cambie el filtro en el editor de reglas de sincronización.

## <a name="recommended-documents"></a>**Documentos recomendados**
* [Sincronización de Azure AD Connect: conceptos técnicos](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-technical-concepts)
* [Sincronización de Azure AD Connect: descripción de la arquitectura](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-architecture)
* [Sincronización de Azure AD Connect: Descripción del aprovisionamiento declarativo](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-declarative-provisioning)
* [Sincronización de Azure AD Connect: descripción de las expresiones de aprovisionamiento declarativas](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-declarative-provisioning-expressions)
* [Sincronización de Azure AD Connect: descripción de la configuración predeterminada](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-default-configuration)
* [Sincronización de Azure AD Connect: descripción de usuarios, grupos y contactos](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-user-and-contacts)
* [Sincronización de Azure AD Connect: Atributos paralelos](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-syncservice-shadow-attributes)

## <a name="next-steps"></a>Pasos siguientes
- [Sincronización de Azure AD Connect](how-to-connect-sync-whatis.md).
- [¿Qué es la identidad híbrida?](whatis-hybrid-identity.md)
