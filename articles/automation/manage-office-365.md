---
title: Administración de servicios de Office 365 mediante Azure Automation
description: Indica cómo usar Azure Automation para administrar los servicios de suscripción de Office 365.
services: automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: 9cb505ced907b143fbd6a5f4f30c818092005bb8
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2020
ms.locfileid: "80576771"
---
# <a name="manage-office-365-services-using-azure-automation"></a>Administración de servicios de Office 365 mediante Azure Automation

Puede usar Azure Automation para la administración de los servicios de suscripción de Office 365, en productos como Microsoft Word y Microsoft Outlook. Las interacciones con Office 365 se habilitan mediante [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis). Consulte [Uso de Azure AD en Automation para autenticarse en Azure](automation-use-azure-ad.md).

>[!NOTE]
>Este artículo se ha actualizado para usar el nuevo módulo Az de Azure PowerShell. Aún puede usar el módulo de AzureRM que continuará recibiendo correcciones de errores hasta diciembre de 2020 como mínimo. Para más información acerca del nuevo módulo Az y la compatibilidad con AzureRM, consulte [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Presentación del nuevo módulo Az de Azure PowerShell). Para obtener instrucciones sobre la instalación del módulo Az en Hybrid Runbook Worker, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Puede actualizar los módulos de su cuenta de Automation a la versión más reciente mediante [Actualización de módulos de Azure PowerShell en Azure Automation](automation-update-azure-modules.md).

## <a name="prerequisites"></a>Prerrequisitos

Necesita los siguientes elementos para administrar los servicios de suscripción de Office 365 en Azure Automation.

* Suscripción a Azure. Consulte [Guía de decisiones de suscripción](https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/subscriptions/).
* Un objeto de Automation en Azure que contenga las credenciales y los runbooks de la cuenta de usuario. Consulte [Introducción a Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro).
* Azure AD. Consulte [Uso de Azure AD en Automation para autenticarse en Azure](automation-use-azure-ad.md).
* Un inquilino de Office 365, con una cuenta. Consulte [Configurar el espacio empresarial de Office 365](https://docs.microsoft.com/sharepoint/dev/spfx/set-up-your-developer-tenant).

## <a name="installing-the-msonline-and-msonlineext-modules"></a>Instalación de los módulos MSOnline y MSOnlineExt

El uso de Office 365 en Azure Automation requiere Microsoft Azure Active Directory para Windows PowerShell (módulo `MSOnline`). También necesitará el módulo [`MSOnlineExt`](https://www.powershellgallery.com/packages/MSOnlineExt/1.0.35), que simplifica la administración de Azure AD en entornos de uno y de varios inquilinos. Instale los módulos tal como se describe en [Uso de Azure AD en Automation para autenticarse en Azure](automation-use-azure-ad.md).

>[!NOTE]
>Para usar MSOnline PowerShell, debe ser miembro de Azure AD. Los usuarios invitados no pueden usar el módulo.

## <a name="creating-an-azure-automation-account"></a>Creación de una cuenta de Azure Automation

Para completar los pasos de este artículo, necesitará una cuenta en Azure Automation. Consulte [Creación de una cuenta de Azure Automation](automation-quickstart-create-account.md).
 
## <a name="adding-msonline-and-msonlineext-as-assets"></a>Incorporación de MSOnline y MSOnlineExt como recursos

Agregue ahora los módulos MSOnline y MSOnlineExt instalados para habilitar la funcionalidad de Office 365. Consulte [Administración de módulos en Azure Automation](shared-resources/modules.md).

1. En Azure Portal, seleccione **Cuentas de Automation**.
2. Seleccione la cuenta de Automation.
3. Seleccione **Galería de módulos** en **Recursos compartidos**.
4. Busque MSOnline.
5. Seleccione el módulo `MSOnline` de PowerShell y haga clic en **Importar** para importar el módulo como un recurso.
6. Repita los pasos 4 y 5 para buscar e importar el módulo `MSOnlineExt`. 

## <a name="creating-a-credential-asset-optional"></a>Creación de un recurso de credencial (opcional)

La creación de un recurso de credencial para el usuario administrativo de Office 365 que tiene permisos para ejecutar el script es opcional; sin embargo, puede ser útil para evitar la exposición de nombres de usuario y contraseñas dentro de scripts de PowerShell. Para obtener instrucciones, consulte [Creación de un recurso de credencial](automation-use-azure-ad.md#creating-a-credential-asset).

## <a name="creating-an-office-365-service-account"></a>Creación de una cuenta de servicio de Office 365

Para ejecutar los servicios de suscripción de Office 365, necesita una cuenta de servicio de Office 365 con permisos para hacer lo que desee. Puede usar una cuenta de administrador global, una cuenta por servicio o contar con una función o un script que se ejecuten. En cualquier caso, la cuenta de servicio requiere una contraseña compleja y segura. Consulte [Configurar Office 365 para empresas](https://docs.microsoft.com/microsoft-365/admin/setup/setup?view=o365-worldwide). 

## <a name="connecting-to-the-azure-ad-online-service"></a>Conexión al servicio en línea de Azure AD

>[!NOTE]
>Para usar los cmdlets de módulo MSOnline, debe ejecutarlos desde Windows PowerShell. PowerShell Core no es compatible con estos cmdlets.

Puede usar el módulo MSOnline para conectarse a Azure AD desde la suscripción de Office 365. La conexión usa un nombre de usuario y una contraseña de Office 365 o la autenticación multifactor (MFA). Puede conectarse mediante Azure Portal o un símbolo del sistema de Windows PowerShell (no tiene que ser elevado).

A continuación se muestra el ejemplo de PowerShell. El cmdlet [Get-Credential](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential?view=powershell-7) solicita las credenciales y las almacena en la variable `Msolcred`. A continuación, el cmdlet [Connect-MsolService](https://docs.microsoft.com/powershell/module/msonline/connect-msolservice?view=azureadps-1.0) usa las credenciales para conectarse al servicio en línea de Azure AD. Si desea conectarse a un entorno específico de Azure, use el parámetro `AzureEnvironment`.

```powershell
$Msolcred = Get-Credential
Connect-MsolService -Credential $MsolCred -AzureEnvironment "AzureCloud"
```

Si no recibe ningún error, se ha conectado correctamente. Una prueba rápida consiste en ejecutar un cmdlet de Office 365, por ejemplo, `Get-MsolUser`, y ver los resultados. Si recibe errores, tenga en cuenta que un problema habitual es indicar una contraseña incorrecta.

>[!NOTE]
>También puede usar los módulos AzureRM o Az para conectarse a Azure AD desde la suscripción de Office 365. El cmdlet de conexión principal es [Connect-AzureAD](https://docs.microsoft.com/powershell/module/azuread/connect-azuread?view=azureadps-2.0). Este cmdlet admite el parámetro `AzureEnvironmentName` para entornos específicos de Office 365.

## <a name="creating-a-powershell-runbook-from-an-existing-script"></a>Creación de un runbook de PowerShell a partir de un script existente

Puede acceder a la funcionalidad de Office 365 desde un script de PowerShell. A continuación se muestra un ejemplo de un script para una credencial denominada `Office-Credentials` con el nombre de usuario `admin@TenantOne.com`. Utiliza `Get-AutomationPSCredential` para importar la credencial de Office 365.

```powershell
$emailFromAddress = "admin@TenantOne.com" 
$emailToAddress = "servicedesk@TenantOne.com" 
$emailSMTPServer = "outlook.office365.com" 
$emailSubject = "Office 365 License Report" 

$credObject = Get-AutomationPSCredential -Name "Office-Credentials" 
Connect-MsolService -Credential $credObject

$O365Licenses = Get-MsolAccountSku | Out-String 
Send-MailMessage -Credential $credObject -From $emailFromAddress -To $emailToAddress -Subject $emailSubject -Body 

$O365Licenses -SmtpServer $emailSMTPServer -UseSSL
```

## <a name="running-the-script-in-a-runbook"></a>Ejecución del script en un runbook

Puede usar el script en un runbook de Azure Automation. A modo de ejemplo, usaremos el tipo de runbook de PowerShell.

1. Cree un runbook de PowerShell. Consulte [Creación de un runbook de Azure Automation](https://docs.microsoft.com/azure/automation/automation-quickstart-create-runbook).
2. En la cuenta de Automation, seleccione **Runbooks** en **Automatización de procesos**.
3. Seleccione el nuevo runbook y haga clic en **Editar**.
4. Copie el script y péguelo en el editor de texto para el runbook.
5. Seleccione **ASSETS** (Recursos), expanda **Credentials** (Credenciales) y compruebe que la credencial de Office 365 está incluida.
6. Haga clic en **Save**(Guardar).
7. Seleccione **Panel de prueba** y, a continuación, haga clic en **Iniciar** para empezar a probar el runbook. Consulte [Administración de runbooks en Azure Automation](https://docs.microsoft.com/azure/automation/manage-runbooks).
8. Una vez finalizada la prueba, salga del panel prueba.

## <a name="publishing-and-scheduling-the-runbook"></a>Publicación y programación del runbook

Para publicar y programar el runbook, consulte [Administración de runbooks en Azure Automation](https://docs.microsoft.com/azure/automation/manage-runbooks).

## <a name="next-steps"></a>Pasos siguientes

* Puede encontrar más información sobre los recursos de credencial de Automation en [Recursos de credenciales en Azure Automation](shared-resources/credentials.md).
* Consulte [Administración de módulos en Azure Automation](shared-resources/modules.md) para obtener información sobre cómo trabajar con módulos de Automation.
* Para obtener información general sobre la administración de runbooks, consulte [Administración de runbooks en Azure Automation](https://docs.microsoft.com/azure/automation/manage-runbooks).
* Para obtener más información sobre los métodos que se pueden usar para iniciar un runbook en Azure Automation, consulte [Inicio de un runbook en Azure Automation](automation-starting-a-runbook.md).
* Para obtener más información sobre PowerShell, incluidos los módulos de aprendizaje y de referencia de lenguaje, consulte la [documentación de PowerShell](https://docs.microsoft.com/powershell/scripting/overview).