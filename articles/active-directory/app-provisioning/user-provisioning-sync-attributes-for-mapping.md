---
title: Sincronización de atributos en Azure AD para la asignación | Microsoft Docs
description: Obtenga información sobre cómo sincronizar los atributos de Active Directory local en Azure AD. Al configurar el aprovisionamiento de usuarios para aplicaciones SaaS, use la característica de extensión de directorio para agregar atributos de origen que no están sincronizados de manera predeterminada.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: mimart
ms.custom: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 09d1efaf54bee65bd3274987e68e643f887baade
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77522278"
---
# <a name="sync-an-attribute-from-your-on-premises-active-directory-to-azure-ad-for-provisioning-to-an-application"></a>Sincronización de un atributo de Active Directory local en Azure AD para el aprovisionamiento en una aplicación

Al personalizar las asignaciones de atributos para el aprovisionamiento de usuarios, es posible que el atributo que quiera asignar no aparezca en la lista **Atributo de origen**. En este artículo se muestra cómo agregar el atributo que falta mediante la sincronización de Active Directory (AD) local en Azure Active Directory (Azure AD).

Azure AD debe contener todos los datos necesarios para crear un perfil de usuario al aprovisionar cuentas de usuario de Azure AD en una aplicación SaaS. En algunos casos, para que los datos estén disponibles es posible que tenga que sincronizar atributos de AD local en Azure AD. Azure AD Connect sincroniza automáticamente determinados atributos en Azure AD, pero no todos. Además, algunos atributos (por ejemplo, SAMAccountName) que se sincronizan de manera predeterminada no pueden exponerse mediante Microsoft Graph API. En estos casos, puede usar la característica de extensión de directorio de Azure AD Connect para sincronizar el atributo en Azure AD. De este modo, el atributo será visible para Microsoft Graph API y el servicio de aprovisionamiento de Azure AD.

Si los datos que necesita para el aprovisionamiento están en Active Directory, pero no están disponibles para el aprovisionamiento debido a las razones descritas anteriormente, siga estos pasos.
 
## <a name="sync-an-attribute"></a>Sincronización de un atributo 

1. Abra el asistente de Azure AD Connect, elija Tareas y, a continuación, elija **Personalizar las opciones de sincronización**.

   ![Página de tareas adicionales del asistente de Azure Active Directory Connect](./media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-customize.png)
 
2. Inicie sesión como administrador global de Azure AD. 

3. En la página **Características opcionales**, seleccione **Sincronización de atributos de las extensiones de directorios**.
 
   ![Página de características opcionales del asistente de Azure Active Directory Connect](./media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-directory-extension-attribute-sync.png)

4. Seleccione los atributos que quiere extender a Azure AD.
   > [!NOTE]
   > La búsqueda en **Atributos disponibles** distingue mayúsculas de minúsculas.

   ![Página de selección de las extensiones de directorios del asistente de Azure Active Directory Connect](./media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-directory-extensions.png)

5. Finalice el asistente de Azure AD Connect y permita que se ejecute un ciclo de sincronización completo. Cuando se complete el ciclo, se extiende el esquema y los nuevos valores se sincronizan entre AD local y Azure AD.
 
6. En Azure Portal, mientras [edita las asignaciones de atributos de usuario](customize-application-attributes.md), la lista **Atributo de origen** ahora contendrá el atributo agregado en el formato `<attributename> (extension_<appID>_<attributename>)`. Seleccione el atributo y asígnelo a la aplicación de destino para el aprovisionamiento.

   ![Página de selección de las extensiones de directorios del asistente de Azure Active Directory Connect](./media/user-provisioning-sync-attributes-for-mapping/attribute-mapping-extensions.png)

> [!NOTE]
> La capacidad de aprovisionar atributos de referencia desde AD local, como **managedby** o **DN/DistinguishedName**, no se admite actualmente. Puede solicitar esta característica en [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory). 

## <a name="next-steps"></a>Pasos siguientes

* [Definir quién está en el ámbito de aprovisionamiento](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)
