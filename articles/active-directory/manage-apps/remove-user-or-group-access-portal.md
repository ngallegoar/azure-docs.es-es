---
title: Eliminación de asignaciones de usuario o grupo de una aplicación en Azure AD
description: Cómo eliminar la asignación del acceso de un usuario o grupo de una aplicación empresarial en Azure Active Directory
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 04/12/2019
ms.author: kenwith
ms.reviewer: asteen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a09b3307403cb78da3bd22f3cc85bd75d7ebd858
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84763064"
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a>Eliminación de asignaciones de usuario o grupo de una aplicación empresarial en Azure Active Directory

Evitar que se asigne acceso a un usuario o grupo a una de sus aplicaciones empresariales en Azure Active Directory (Azure AD) es fácil. Debe tener los permisos adecuados para administrar la aplicación empresarial. Y tiene que ser el administrador global del directorio.

> [!NOTE]
> Con aplicaciones de Microsoft (por ejemplo, aplicaciones de Office 365), use PowerShell para eliminar usuarios de una aplicación empresarial.

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-in-the-azure-portal"></a>¿Cómo puedo eliminar asignaciones de usuario o grupo de una aplicación empresarial en Azure Portal?

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta que tenga el rol de administrador global en el directorio.
1. Seleccione **Todos los servicios**, escriba **Azure Active Directory** en el cuadro de texto y, después, seleccione **Entrar**.
1. En la página **Azure Active Directory - *nombreDeDirectorio*** (es decir, la página de Azure AD del directorio que está administrando), seleccione **Aplicaciones empresariales**.
1. En el panel **Aplicaciones empresariales - Todas las aplicaciones**, verá una lista de las aplicaciones que puede administrar. Seleccione una aplicación.
1. En la página de introducción ***nombreDeAplicación*** (es decir, la página con el nombre de la aplicación seleccionada en el título), seleccione **Usuarios y grupos**.
1. En la página ***nombreDeAplicación*** **- User & Group Assignment** (Asignación de usuarios y grupos), seleccione uno o varios usuarios o grupos y, luego, seleccione el comando **Quitar**. Confirme la decisión en el símbolo del sistema.

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-using-powershell"></a>¿Cómo puedo eliminar asignaciones de usuario o grupo de una aplicación empresarial con PowerShell?

1. Abra un símbolo del sistema de Windows PowerShell con privilegios elevados.

   > [!NOTE]
   > Debe instalar el módulo de AzureAD (use el comando `Install-Module -Name AzureAD`). Si se le pide que instale un módulo de NuGet o el nuevo módulo de PowerShell para Azure Active Directory V2, escriba S y presione ENTRAR.

1. Ejecute `Connect-AzureAD` e inicie sesión con una cuenta de usuario administrador global.
1. Use el siguiente script para eliminar un usuario y un rol de una aplicación:

    ```powershell
    # Store the proper parameters
    $user = get-azureaduser -ObjectId <objectId>
    $spo = Get-AzureADServicePrincipal -ObjectId <objectId>

    #Get the ID of role assignment 
    $assignments = Get-AzureADServiceAppRoleAssignment -ObjectId $spo.ObjectId | Where {$_.PrincipalDisplayName -eq $user.DisplayName}

    #if you run the following, it will show you what is assigned what
    $assignments | Select *

    #To remove the App role assignment run the following command.
    Remove-AzureADServiceAppRoleAssignment -ObjectId $spo.ObjectId -AppRoleAssignmentId $assignments[assignment #].ObjectId
    ```

## <a name="next-steps"></a>Pasos siguientes

- [Ver todos mis grupos](../fundamentals/active-directory-groups-view-azure-portal.md)
- [Asignar un usuario o grupo a una aplicación empresarial](assign-user-or-group-access-portal.md)
- [Deshabilitar los inicios de sesión de los usuarios de una aplicación empresarial](disable-user-sign-in-portal.md)
- [Cambio del nombre o el logotipo de una aplicación empresarial](change-name-or-logo-portal.md)
