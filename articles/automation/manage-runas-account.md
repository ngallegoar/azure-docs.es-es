---
title: Administración de una cuenta de ejecución de Azure Automation
description: En este artículo se describe cómo administrar la cuenta de ejecución con PowerShell o desde Azure Portal.
services: automation
ms.subservice: shared-capabilities
ms.date: 09/28/2020
ms.topic: conceptual
ms.openlocfilehash: 0849eb0c421883ecb0510451ff81b604538c9cc3
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92069898"
---
# <a name="manage-an-azure-automation-run-as-account"></a>Administración de una cuenta de ejecución de Azure Automation

Las cuentas de ejecución de Azure Automation proporcionan autenticación para administrar recursos en el modelo de implementación clásico de Azure o Azure Resource Manager mediante runbooks de Automation y otras características de Automation. En este artículo se proporcionan instrucciones sobre cómo administrar una cuenta de ejecución o de ejecución clásica.

Para obtener más información acerca de la autenticación de cuentas de Azure Automation e instrucciones relacionadas con los escenarios de automatización de procesos, consulte [Introducción a la autenticación de cuentas de Automation](automation-security-overview.md).

## <a name="run-as-account-permissions"></a><a name="permissions"></a>Permisos de las cuentas de ejecución

En esta sección se definen los permisos para las cuentas de ejecución normales y las cuentas de ejecución clásicas.

Para crear o actualizar una cuenta de ejecución, debe tener los permisos y privilegios específicos. Un administrador de aplicaciones en Azure Active Directory y un propietario en una suscripción pueden completar todas las tareas. En una situación en la que tenga separación de funciones, la siguiente tabla muestra una lista de las tareas, el cmdlet equivalente y los permisos necesarios:

|Tarea|Cmdlet  |Permisos mínimos  |Donde se establecen los permisos|
|---|---------|---------|---|
|Crear una aplicación de Azure AD|[New-AzADApplication](/powershell/module/az.resources/new-azadapplication)     | Rol de desarrollador de aplicaciones<sup>1</sup>        |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app)</br>Inicio > Azure AD > Registros de aplicaciones |
|Agregar una credencial a la aplicación.|[New-AzADAppCredential](/powershell/module/az.resources/new-azadappcredential)     | Administrador de la aplicación o administrador global<sup>1</sup>         |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app)</br>Inicio > Azure AD > Registros de aplicaciones|
|Crear y obtener una entidad de servicio de Azure AD|[New-AzADServicePrincipal](/powershell/module/az.resources/new-azadserviceprincipal)</br>[Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal)     | Administrador de la aplicación o administrador global<sup>1</sup>        |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app)</br>Inicio > Azure AD > Registros de aplicaciones|
|Asignar u obtener el rol de Azure para la entidad de seguridad especificada|[New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment)</br>[Get-AzRoleAssignment](/powershell/module/Az.Resources/Get-AzRoleAssignment)      | Administrador de acceso de usuario o Propietario, o bien tener los permisos siguientes:</br></br><code>Microsoft.Authorization/Operations/read</br>Microsoft.Authorization/permissions/read</br>Microsoft.Authorization/roleDefinitions/read</br>Microsoft.Authorization/roleAssignments/write</br>Microsoft.Authorization/roleAssignments/read</br>Microsoft.Authorization/roleAssignments/delete</code></br></br> | [Suscripción](../role-based-access-control/role-assignments-portal.md)</br>Inicio > Suscripciones > \<subscription name\> Access Control (IAM)|
|Crear o quitar un certificado de Automation|[New-AzAutomationCertificate](/powershell/module/Az.Automation/New-AzAutomationCertificate)</br>[Remove-AzAutomationCertificate](/powershell/module/az.automation/remove-azautomationcertificate)     | Colaborador en el grupo de recursos         |Grupo de recursos de la cuenta de Automation|
|Crear o quitar una conexión de Automation|[New-AzAutomationConnection](/powershell/module/az.automation/new-azautomationconnection)</br>[Remove-AzAutomationConnection](/powershell/module/az.automation/remove-azautomationconnection)|Colaborador en el grupo de recursos |Grupo de recursos de la cuenta de Automation|

<sup>1</sup> Los usuarios que no son administradores en el inquilino de Azure AD pueden [registrar aplicaciones de AD](../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app) si la opción **Los usuarios pueden registrar aplicaciones** del inquilino de Azure AD en la página Configuración de usuario está establecida en **Sí**. Si el registro de la aplicación está configurado en **No**, el usuario que realiza esta acción debe ser como se define en esta tabla.

Si no es miembro de la instancia de Active Directory de la suscripción antes de que se le agregue al rol Administrador global de la suscripción, se le agregará como invitado. En este caso, recibirá una advertencia `You do not have permissions to create…` en la página **Agregar cuenta de Automation**.

Si es miembro de la instancia de Active Directory de la suscripción cuando se asigna el rol Administrador global, también puede recibir una advertencia `You do not have permissions to create…` en la página **Agregar cuenta de Automation**. En este caso, puede solicitar la eliminación de la instancia de Active Directory de la suscripción y, a continuación, solicitar que se vuelva a agregar, a fin de convertirse en usuario completo de Active Directory.

Para comprobar que se ha corregido la situación causante del mensaje de error:

1. En el panel de Azure Active Directory de Azure Portal, seleccione **Usuarios y grupos**.
2. Seleccione **Todos los usuarios**.
3. Elija su nombre y, a continuación, seleccione **Perfil**.
4. Asegúrese de que el valor del atributo **Tipo de usuario**  en el perfil de usuario no esté establecido en **Invitado**.

### <a name="permissions-required-to-create-or-manage-classic-run-as-accounts"></a><a name="permissions-classic"></a>Permisos necesarios para crear o administrar cuentas de ejecución clásicas

Para configurar o renovar las cuentas de ejecución clásicas, debe tener el rol Coadministrador en el nivel de suscripción. Para más información sobre los permisos de suscripción clásicos, consulte [Administradores de la suscripción clásica de Azure](../role-based-access-control/classic-administrators.md#add-a-co-administrator).

## <a name="create-a-run-as-account-in-azure-portal"></a>Creación de una cuenta de ejecución en Azure Portal

Realice los pasos que se describen a continuación para actualizar su cuenta de Azure Automation en Azure Portal. Cree las cuentas de ejecución y de ejecución clásica de forma individual. Si no es necesario administrar los recursos clásicos, basta con crear la cuenta de ejecución de Azure.

1. Inicie sesión en el Portal de Azure con una cuenta que sea miembro del rol Administradores de suscripciones y coadministrador de la suscripción.

2. Busque y seleccione **Cuentas de Automation**.

3. En la página Cuentas de Automation, seleccione su cuenta de Automation en la lista.

4. En el panel izquierdo, seleccione **Cuentas de ejecución** en la sección **Configuración de la cuenta**.

    :::image type="content" source="media/manage-runas-account/automation-account-properties-pane.png" alt-text="Seleccione la opción Cuenta de ejecución.":::

5. En función de la cuenta que necesite, utilice el panel **+Cuenta de ejecución de Azure** o **+Cuenta de ejecución de Azure clásica**. Después de revisar la información general, haga clic en **Crear**.

    :::image type="content" source="media/manage-runas-account/automation-account-create-runas.png" alt-text="Seleccione la opción Cuenta de ejecución.":::

6. Mientras Azure crea la cuenta de ejecución, se puede seguir el progreso en **Notificaciones** en el menú. También se muestra un banner que indica que se está creando la cuenta. El proceso puede tardar unos minutos en completarse.

## <a name="create-a-run-as-account-using-powershell"></a>Creación de una cuenta de ejecución con PowerShell

En la lista siguiente se proporcionan los requisitos para crear una cuenta de ejecución en PowerShell mediante un script proporcionado. Estos requisitos se aplican a ambos tipos de cuentas de ejecución.

* Windows 10 o Windows Server 2016 con módulos de Azure Resource Manager 3.4.1 y versiones posteriores. El script de PowerShell no admite versiones anteriores de Windows.
* Azure PowerShell 6.2.4 o posterior. Para más información, consulte [Instalación y configuración de Azure PowerShell](/powershell/azure/install-az-ps).
* Una cuenta de Automation, a la que se hace referencia como el valor de los parámetros `AutomationAccountName` y `ApplicationDisplayName`.
* Permisos equivalentes a los que se muestran en [Permisos para configurar cuentas de ejecución](#permissions).

Para obtener los valores de `AutomationAccountName`, `SubscriptionId` y `ResourceGroupName`, que son los parámetros necesarios para el script de PowerShell, complete los pasos siguientes.

1. En Azure Portal, seleccione **Cuentas de Automation**.

1. En la página Cuentas de Automation, seleccione su cuenta de Automation.

1. En la sección Configuración de la cuenta, seleccione **Propiedades**.

1. Anote los valores de **Nombre**, **Id. de suscripción** y **Grupo de recursos** en la página **Propiedades**.

   ![Página de propiedades de la cuenta de Automation](media/manage-runas-account/automation-account-properties.png)

### <a name="powershell-script-to-create-a-run-as-account"></a>Script de PowerShell para crear una cuenta de ejecución

El script de PowerShell incluye compatibilidad con varias configuraciones.

* Creación de una cuenta de ejecución mediante un certificado autofirmado.
* Creación de una cuenta de ejecución y una cuenta de ejecución clásica mediante un certificado autofirmado.
* Cree una cuenta de ejecución y una cuenta de ejecución clásica mediante un certificado emitido por su entidad de certificación (CA) empresarial.
* Creación de una cuenta de ejecución y una cuenta de ejecución clásica mediante un certificado autofirmado en la nube de Azure Government.

1. Descargue y guarde el script en una carpeta local con el siguiente comando.

    ```powershell
    wget https://raw.githubusercontent.com/azureautomation/runbooks/master/Utility/AzRunAs/Create-RunAsAccount.ps1 -outfile Create-RunAsAccount.ps1
    ```

2. Inicie PowerShell con permisos de usuario elevados y navegue hasta la carpeta que contiene el script.

3. Ejecute uno de los siguientes comandos para crear una cuenta de ejecución o de ejecución clásica en función de sus requisitos.

    * Cree una cuenta de ejecución mediante un certificado autofirmado.

        ```powershell
        .\Create-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false
        ```

    * Creación de una cuenta de ejecución y una cuenta de ejecución clásica mediante un certificado autofirmado.

        ```powershell
        .\Create-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true
        ```

    * Creación de una cuenta de ejecución y una cuenta de ejecución clásica mediante un certificado de empresa.

        ```powershell
        .\Create-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>
        ```

        Si creó una cuenta de ejecución clásica con un certificado público de empresa (archivo .cer) , use este certificado. El script la crea y la guarda en la carpeta de archivos temporales del equipo, en el perfil de usuario `%USERPROFILE%\AppData\Local\Temp` que usó para ejecutar la sesión de PowerShell. Consulte [Carga de un certificado de API de administración en Azure Portal](../cloud-services/cloud-services-configure-ssl-certificate-portal.md).

    * Creación de una cuenta de ejecución y una cuenta de ejecución clásica mediante un certificado autofirmado en la nube de Azure Government

        ```powershell
        .\Create-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnvironmentName AzureUSGovernment
        ```

4. Después de ejecutar el script, se le pedirá que se autentique en Azure. Inicie sesión con una cuenta que sea miembro del rol de administradores de la suscripción. Si va a crear una cuenta de ejecución clásica, su cuenta debe ser un coadministrador de la suscripción.

## <a name="delete-a-run-as-or-classic-run-as-account"></a>Eliminación de una cuenta de ejecución o de ejecución clásica

En esta sección se describe cómo eliminar una cuenta de ejecución o una cuenta de ejecución clásica. Al realizar esta acción, la cuenta de Automation se conserva. Después de eliminar la cuenta de ejecución, puede volver a crearla en Azure Portal o con el script de PowerShell proporcionado.

1. Abra la cuenta de Automation en Azure Portal.

2. En el panel izquierdo, seleccione **Cuentas de ejecución** en la sección Configuración de la cuenta.

3. En la página de propiedades Cuentas de ejecución, seleccione la cuenta de ejecución o la cuenta de ejecución clásica que quiere eliminar.

4. En el panel Propiedades de la cuenta seleccionada, haga clic en **Eliminar**.

   ![Eliminación de una cuenta de ejecución](media/manage-runas-account/automation-account-delete-runas.png)

5. Mientras se está eliminando la cuenta, puede seguir el progreso desde el menú, en **Notificaciones**.

## <a name="renew-a-self-signed-certificate"></a><a name="cert-renewal"></a>Renovación de un certificado autofirmado

El certificado autofirmado que creó para la cuenta de ejecución expira un año después de la fecha de creación. En algún momento antes de que expire su cuenta de ejecución, debe renovar el certificado. Se puede renovar en cualquier momento antes de que expire.

Cuando se renueva el certificado autofirmado, el certificado válido actual se conserva para garantizar que los runbooks que se ponen en la cola o que se ejecutan activamente, y que se autentican con la cuenta de ejecución, no se ven negativamente afectados. El certificado es válido hasta la fecha de expiración.

>[!NOTE]
>Si cree que se ha puesto en peligro la cuenta de ejecución, puede eliminarla y volver a crearla.

>[!NOTE]
>Si ha configurado la cuenta de ejecución para usar un certificado emitido por la entidad de certificación de la empresa y usa esta opción para renovar un certificado autofirmado, el certificado de empresa se reemplaza por uno autofirmado.

Realice los pasos siguientes para renovar el certificado autofirmado.

1. Abra la cuenta de Automation en Azure Portal.

1. Seleccione **Cuentas de ejecución** en la sección Configuración de la cuenta.

    ![Panel de propiedades de la cuenta de Automation](media/manage-runas-account/automation-account-properties-pane.png)

1. En la página de propiedades de Cuentas de ejecución, seleccione la cuenta de ejecución o la cuenta de ejecución clásica para la que quiere renovar el certificado.

1. En el panel Propiedades de la cuenta seleccionada, haga clic en **Renovar certificado**.

    ![Renovación del certificado para una cuenta de ejecución](media/manage-runas-account/automation-account-renew-runas-certificate.png)

1. Mientras se está renovando el certificado, puede seguir el progreso desde el menú, en **Notificaciones**.

## <a name="limit-run-as-account-permissions"></a>Limitación de permisos de cuentas de ejecución

Para controlar la orientación de Automation hacia los recursos de Azure, puede ejecutar el script [Update-AutomationRunAsAccountRoleAssignments.ps1](https://aka.ms/AA5hug8). Este script cambia la entidad de servicio de la cuenta de ejecución existente para crear y usar una definición de rol personalizada. Este rol tiene permisos para todos los recursos excepto para [Key Vault](../key-vault/index.yml).

>[!IMPORTANT]
>Después de ejecutar el script **Update-AutomationRunAsAccountRoleAssignments.ps1**, los runbooks que acceden a Key Vault mediante cuentas de ejecución ya no funcionan. Antes de ejecutar el script, debe revisar los runbooks de su cuenta para las llamadas a Azure Key Vault. Para permitir el acceso a Key Vault desde los runbooks de Azure Automation, debe [agregar la cuenta de ejecución a los permisos de Key Vault](#add-permissions-to-key-vault).

Si necesita restringir aún más lo que la entidad de servicio de ejecución puede hacer, puede agregar otros tipos de recursos al elemento `NotActions` de la definición de roles personalizada. El siguiente ejemplo restringe el acceso a `Microsoft.Compute/*`. Si agrega este tipo definición de recurso a `NotActions` para la definición de roles, el rol no podrá acceder a ningún recurso de proceso. Para más información sobre las definiciones de roles, consulte [Descripción de definiciones de roles para los recursos de Azure](../role-based-access-control/role-definitions.md).

```powershell
$roleDefinition = Get-AzRoleDefinition -Name 'Automation RunAs Contributor'
$roleDefinition.NotActions.Add("Microsoft.Compute/*")
$roleDefinition | Set-AzRoleDefinition
```

Puede determinar si la entidad de servicio que se usa en la cuenta de ejecución está en la definición del rol Colaborador o en una personalizada.

1. Vaya a la cuenta de Automation y seleccione **Cuentas de ejecución** en la sección Configuración de la cuenta.
2. Seleccione **Cuenta de ejecución de Azure**.
3. Seleccione **Rol** para localizar la definición de roles que se está usando.

:::image type="content" source="media/manage-runas-account/verify-role.png" alt-text="Seleccione la opción Cuenta de ejecución." lightbox="media/manage-runas-account/verify-role-expanded.png":::.

También puede determinar la definición de roles que usan las cuentas de ejecución para varias suscripciones o cuentas de Automation. Para ello, use el script [Check-AutomationRunAsAccountRoleAssignments.ps1](https://aka.ms/AA5hug5) en la Galería de PowerShell.

### <a name="add-permissions-to-key-vault"></a>Incorporación de permisos a Key Vault

Puede permitir que Azure Automation compruebe si Key Vault y la entidad de servicio de la cuenta de ejecución usan una definición de roles personalizada. Debe:

* Conceder permisos a Key Vault.
* Establecer la directiva de acceso.

Puede usar el script [Extend-AutomationRunAsAccountRoleAssignmentToKeyVault.ps1](https://aka.ms/AA5hugb) en el Galería de PowerShell para asignar sus permisos de la cuenta de ejecución a KeyVault. Consulte [Asignación de una directiva de acceso de Key Vault](../key-vault/general/assign-access-policy-powershell.md) para obtener más detalles sobre cómo establecer permisos en Key Vault.

## <a name="resolve-misconfiguration-issues-for-run-as-accounts"></a>Solución de problemas de configuración incorrecta de las cuentas de ejecución

Puede que algunos elementos de configuración necesarios para la cuenta de ejecución o la cuenta de ejecución clásica se hayan eliminado o no se hayan creado correctamente durante la configuración inicial. Algunos errores de configuración posibles son:

* Recurso de certificado
* Recurso de conexión
* Cuenta de ejecución quitada del rol Colaborador
* Entidad de servicio o aplicación en Azure AD

En estos casos de errores de configuración, la cuenta de Automation detecta los cambios y muestra el estado *Incompleto* en el panel de propiedades de Cuentas de ejecución de la cuenta.

![Estado de configuración incompleta de la cuenta de ejecución](media/manage-runas-account/automation-account-runas-config-incomplete.png)

Al seleccionar la cuenta de ejecución, el panel de propiedades de la cuenta muestra el mensaje de error siguiente:

```text
The Run As account is incomplete. Either one of these was deleted or not created - Azure Active Directory Application, Service Principal, Role, Automation Certificate asset, Automation Connect asset - or the Thumbprint is not identical between Certificate and Connection. Please delete and then re-create the Run As Account.
```

Para resolver rápidamente estos problemas de la cuenta de ejecución solo tiene que eliminarla y volver a crearla.

## <a name="next-steps"></a>Pasos siguientes

* [Objetos de aplicación y de entidad de servicio](../active-directory/develop/app-objects-and-service-principals.md).
* [Introducción a los certificados para Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).