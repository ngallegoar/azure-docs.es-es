---
title: Recursos de credenciales en Azure Automation
description: Los recursos de credenciales en Azure Automation contienen credenciales de seguridad que se pueden usar para realizar la autenticación a los recursos a los que tiene acceso el runbook o configuración de DSC. En este artículo se describe cómo crear recursos de credenciales y usarlos en un runbook o una configuración de DSC.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: c8b63a2676690004d23094b490fea0ef150ab9cb
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2020
ms.locfileid: "80546398"
---
# <a name="credential-assets-in-azure-automation"></a>Recursos de credenciales en Azure Automation

Un recurso de credencial de Automation incluye un objeto que contiene credenciales de seguridad, como un nombre de usuario y una contraseña. Los runbooks y las configuraciones de DSC usan cmdlets que aceptan un objeto [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?view=pscore-6.2.0) para la autenticación. Como alternativa, pueden extraer el nombre de usuario y la contraseña del objeto `PSCredential` para proporcionarlos a alguna aplicación o servicio que requiera autenticación. 

Azure Automation almacena de forma segura las propiedades de una credencial. Acceda a las propiedades mediante un runbook o una configuración de DSC que usa la actividad [Get-AutomationPSCredential](#activities-used-to-access-credentials).

> [!NOTE]
> Los recursos protegidos en Azure Automation incluyen credenciales, certificados, conexiones y variables cifradas. Estos recursos se cifran y se almacenan en Azure Automation con una clave única que se genera para cada cuenta de Automation. Esta clave se almacena en Key Vault. Antes de almacenar un recurso seguro, la clave se carga desde Key Vault y luego se usa para cifrar el recurso.

[!INCLUDE [gdpr-dsr-and-stp-note.md](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="azure-powershell-az-cmdlets-used-for-credential-assets"></a>Cmdlets de Az de Azure PowerShell usados para los recursos de credenciales

En el módulo Az de Azure PowerShell, los cmdlets de la tabla siguiente se usan para crear y administrar recursos de credenciales de Automation con Windows PowerShell. Estos cmdlets se incluyen como parte del [módulo Az.Automation](/powershell/azure/new-azureps-module-az?view=azps-1.1.0), que está disponible para usarse en las configuraciones de DSC y los runbooks de Automation. Consulte [Compatibilidad con módulos de Az en Azure Automation](https://docs.microsoft.com/azure/automation/az-modules).

| Cmdlet | Descripción |
|:--- |:--- |
| [Get-AzAutomationCredential](/powershell/module/az.automation/get-azautomationcredential?view=azps-3.3.0) |Recupera información acerca de un recurso de credencial. Este cmdlet no devuelve un objeto `PSCredential`.  |
| [New-AzAutomationCredential](/powershell/module/az.automation/new-azautomationcredential?view=azps-3.3.0) |Crea una nueva credencial de Automation. |
| [Remove-AzAutomationCredential](/powershell/module/az.automation/remove-azautomationcredential?view=azps-3.3.0) |Quita una credencial de Automation. |
| [Set-AzAutomationCredential](/powershell/module/az.automation/set-azautomationcredential?view=azps-3.3.0) |Establece las propiedades para una credencial de Automatización existente. |

## <a name="activities-used-to-access-credentials"></a>Actividades usadas para acceder a las credenciales

Las actividades de la tabla siguiente se usan para acceder a las credenciales en los runbook y las configuraciones de DSC.

| Actividad | Descripción |
|:--- |:--- |
| `Get-AutomationPSCredential` |Obtiene una credencial para usarla en un runbook o una configuración de DSC. La credencial tiene el formato de un objeto `PSCredential`. |
| [Get-Credential](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential?view=powershell-7) |Obtiene una credencia con una solicitud de nombre de usuario y contraseña. |
| [New-AzureAutomationCredential](https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azureautomationcredential?view=azuresmps-4.0.0) | Crea un recurso de credencial. |

Para el desarrollo local mediante la herramienta de creación de Azure Automation, el cmdlet `Get-AutomationPSCredential` forma parte del ensamblado [AzureAutomationAuthoringToolkit](https://www.powershellgallery.com/packages/AzureAutomationAuthoringToolkit/0.2.3.9). Para que Azure funcione con el contexto de Automation, el cmdlet está en `Orchestrator.AssetManagement.Cmdlets`. Consulte [Administración de módulos en Azure Automation](modules.md).

Para recuperar objetos `PSCredential` en el código, puede instalar el [complemento ISE de Microsoft Azure Automation para el ISE de PowerShell](https://github.com/azureautomation/azure-automation-ise-addon).

```azurepowershell
Install-Module AzureAutomationAuthoringToolkit -Scope CurrentUser -Force
```

El script también puede importar el módulo necesario cuando sea necesario, como en el ejemplo siguiente: 

```azurepowershell
Import-Module Orchestrator.AssetManagement.Cmdlets -ErrorAction SilentlyContinue
```

> [!NOTE]
> Debe evitar el uso de variables en el parámetro `Name` de `Get-AutomationPSCredential`. Su uso puede complicar la detección de dependencias entre runbooks o configuraciones de DSC y recursos de credenciales durante el tiempo de diseño.

## <a name="python2-functions-that-access-credentials"></a>Funciones de Python2 que tienen acceso a las credenciales

La función de la tabla siguiente se usa para obtener acceso a las credenciales de un runbook de Python2.

| Función | Descripción |
|:---|:---|
| `automationassets.get_automation_credential` | Recupera información acerca de un recurso de credencial. |

> [!NOTE]
> Para acceder a las funciones del recurso, importe el módulo `automationassets` en la parte superior del runbook de Python.

## <a name="creating-a-new-credential-asset"></a>Creación de un nuevo recurso de credencial

Puede crear un recurso de credencial mediante Azure Portal o Windows PowerShell.

### <a name="create-a-new-credential-asset-with-the-azure-portal"></a>Creación de un nuevo recurso de credencial con Azure Portal

1. En la cuenta de Automation, seleccione **Credenciales** en **Recursos compartidos**.
1. Seleccione **Agregar una credencial**.
2. En el panel Nueva credencial, escriba un nombre de credencial adecuado que siga los estándares de nomenclatura. 
3. Escriba el identificador de acceso en el campo **Nombre de usuario**. 
4. En ambos campos de contraseña, escriba la clave de acceso secreta.

    ![Creación de una credencial](../media/credentials/credential-create.png)

5. Si la casilla de autenticación multifactor está activada, desactívela. 
6. Haga clic en **Crear** para guardar el nuevo recurso de credencial.

> [!NOTE]
> Azure Automation no admite cuentas de usuario que usen la autenticación multifactor.

### <a name="create-a-new-credential-asset-with-windows-powershell"></a>Creación de un nuevo recurso de credencial con Windows PowerShell

En el ejemplo siguiente se muestra cómo crear un recurso de credencial de Automation. Primero se crea un objeto `PSCredential` con el nombre y la contraseña y, luego, se usa para crear el recurso de credencial. En su lugar, puede usar el cmdlet `Get-Credential` para solicitar al usuario que escriba un nombre y una contraseña.

```powershell
$user = "MyDomain\MyUser"
$pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
$cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred
```

## <a name="using-a-powershell-credential"></a>Uso de una credencial de PowerShell

Un runbook o una configuración de DSC recupera un recurso de credencial con la actividad `Get-AutomationPSCredential`. Esta actividad recupera un objeto `PSCredential` que puede usar con una actividad o un cmdlet que requieren una credencial. También puede recuperar las propiedades del objeto de credencial para usarlas individualmente. El objeto tiene propiedades para el nombre de usuario y la contraseña segura. Como alternativa, puede usar el método [GetNetworkCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential.getnetworkcredential?view=pscore-6.2.0) para recuperar un objeto [NetworkCredential](/dotnet/api/system.net.networkcredential) que representa una versión no segura de la contraseña.

> [!NOTE]
> `Get-AzAutomationCredential` no recupera un objeto `PSCredential` que se pueda usar para la autenticación. Solo proporciona información acerca de la credencial. Si necesita usar una credencial en un runbook, debe recuperarla como un objeto `PSCredential` mediante `Get-AutomationPSCredential`.

### <a name="textual-runbook-example"></a>Ejemplo de un runbook textual

En el ejemplo siguiente se muestra cómo usar una credencial de PowerShell en un runbook. Se recupera la credencial y se asigna su nombre de usuario y contraseña a las variables.


```azurepowershell
$myCredential = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCredential.UserName
$securePassword = $myCredential.Password
$password = $myCredential.GetNetworkCredential().Password
```

También puede usar una credencial para autenticarse en Azure con [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-3.3.0). En la mayoría de las circunstancias, deberá usar una [cuenta de ejecución](../manage-runas-account.md) y recuperar la conexión con [Get-AzAutomationConnection](../automation-connections.md).


```azurepowershell
$myCred = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCred.UserName
$securePassword = $myCred.Password
$password = $myCred.GetNetworkCredential().Password

$myPsCred = New-Object System.Management.Automation.PSCredential ($userName,$password)

Connect-AzAccount -Credential $myPsCred
```

### <a name="graphical-runbook-example"></a>Ejemplos de un runbook gráfico

Para agregar una actividad `Get-AutomationPSCredential` a un runbook gráfico, haga clic con el botón derecho en la credencial en el panel Biblioteca del editor gráfico y, después, seleccione **Add to canvas** (Agregar a lienzo).

![Agregar credencial a lienzo](../media/credentials/credential-add-canvas.png)

La imagen siguiente muestra un ejemplo de cómo usar una credencial en un runbook gráfico. En este caso, la credencial proporciona la autenticación de un runbook en los recursos de Azure, como se describe en [Uso de Azure AD en Azure Automation para autenticarse en Azure](../automation-use-azure-ad.md). La primera actividad recupera la credencial que tiene acceso a la suscripción de Azure. A continuación, la actividad de conexión de la cuenta usa esta credencial para proporcionar autenticación para cualquier actividad que venga después. Aquí se usa un [vínculo de canalización](../automation-graphical-authoring-intro.md#links-and-workflow), debido a que `Get-AutomationPSCredential` espera un solo objeto.  

![Agregar credencial a lienzo](../media/credentials/get-credential.png)

## <a name="using-credentials-in-a-dsc-configuration"></a>Uso de credenciales en una configuración de DSC

Aunque las configuraciones de DSC en Azure Automation pueden funcionar con recursos de credenciales mediante `Get-AutomationPSCredential`, también los pueden pasar a través de parámetros. Para obtener más información, consulte [Compilación de configuraciones en DSC de Azure Automation](../automation-dsc-compile.md#credential-assets).

## <a name="using-credentials-in-python2"></a>Uso de credenciales en Python2

En el ejemplo siguiente se muestra un ejemplo de acceso a credenciales en runbooks de Python2.


```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a credential
cred = automationassets.get_automation_credential("credtest")
print cred["username"]
print cred["password"]
```

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre los vínculos en la creación gráfica, consulte [Vínculos en la creación gráfica](../automation-graphical-authoring-intro.md#links-and-workflow).
* Para entender los distintos métodos de autenticación para Automation, consulte [Seguridad de Azure Automation](../automation-security-overview.md).
* Para empezar a trabajar con runbooks gráficos, consulte [Mi primer runbook gráfico](../automation-first-runbook-graphical.md).
* Para empezar a trabajar con Runbooks de flujo de trabajo de PowerShell, consulte [Mi primer Runbook de flujo de trabajo de PowerShell](../automation-first-runbook-textual.md).
* Para empezar a trabajar con runbooks Python2, consulte [Mi primer runbook Python2](../automation-first-runbook-textual-python2.md). 
