---
title: Preparación de Azure Migrate para trabajar con una herramienta de ISV/Movere
description: En este artículo se describe cómo preparar Azure Migrate para trabajar con una herramienta de ISV o Movere, y luego cómo empezar a usar la herramienta.
ms.topic: how-to
ms.date: 05/07/2020
ms.openlocfilehash: 9513e783d4f9d7be83f1434d4dd9011844af8993
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83682653"
---
# <a name="prepare-to-work-with-an-isv-tool-or-movere"></a>Preparación para trabajar con una herramienta de ISV o Movere

Si ha agregado una [herramienta de ISV](migrate-services-overview.md#isv-integration) o Movere a un proyecto de Azure Migrate, debe seguir algunos pasos antes de vincular la herramienta y enviar datos a Azure Migrate. 

## <a name="check-azure-ad-permissions"></a>Comprobación de los permisos de Azure AD

Su cuenta de usuario de Azure necesita estos permisos:

- Permiso para registrar una aplicación de Azure Active Directory (Azure AD) con su inquilino de Azure
- Permiso para asignar un rol a la aplicación de Azure AD en el nivel de suscripción


### <a name="set-permissions-to-register-an-azure-ad-app"></a>Establecimiento de permisos para registrar una aplicación de Azure AD

1. En Azure AD, compruebe el rol de la cuenta.
2. Si tiene el rol de usuario, seleccione **Configuración de usuario** a la izquierda y compruebe si los usuarios pueden registrar aplicaciones. Si se configura en **Sí**, cualquier usuario del inquilino de Azure Active Directory puede registrar una aplicación. Si se establece en **No**, solo los usuarios administradores pueden registrar aplicaciones.   
3. Si no tiene permisos, un usuario administrador puede proporcionar a una cuenta de usuario el rol [Administrador de la aplicación](../active-directory/users-groups-roles/directory-assign-admin-roles.md#application-administrator) para que pueda registrar la aplicación.
4. Una vez que la herramienta está vinculada a Azure Migrate, el administrador puede quitar el rol de la cuenta.

### <a name="set-permissions-to-assign-a-role-to-an-azure-ad-app"></a>Establecimiento de permisos para asignar un rol a una aplicación de Azure AD
 
En la suscripción de Azure, la cuenta necesita el acceso **Microsoft.Authorization/*/Write** para asignar un rol a una aplicación de Azure AD. 

1. En Azure Portal, abra **Suscripciones**.
2. Seleccione la suscripción correspondiente. Si no lo ve, seleccione el **filtro de suscripciones globales**. 
3. Seleccione **Mis permisos**. A continuación, seleccione **Click here to view complete access details for this subscription** (Haga clic aquí para ver los detalles de acceso completos para esta suscripción).
4. En **Asignaciones de roles** > **Ver**, compruebe los permisos. Si su cuenta no tiene permisos, pida al administrador de la suscripción que le agregue al rol [Administrador de acceso de usuario](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#user-access-administrator) o al rol [Propietario](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner).
 

## <a name="start-using-the-tool"></a>Comienzo del uso del portal

1. Si aún no tiene una licencia o una evaluación gratuita de la herramienta, en la entrada de la herramienta en Azure Migrate, en **Registrar**, seleccione **Más información**.
2. En la herramienta, siga las instrucciones para vincular desde la herramienta al proyecto de Azure Migrate y para enviar datos a Azure Migrate.

## <a name="next-steps"></a>Pasos siguientes

Siga las instrucciones de su ISV o Movere para enviar datos a Azure Migrate.

   
