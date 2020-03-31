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
ms.openlocfilehash: 767c1fddbc3d1f46d4341a70c990c2b57ad40e54
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79227540"
---
# <a name="credential-assets-in-azure-automation"></a>Recursos de credenciales en Azure Automation

Un recurso de credencial de Automation incluye un objeto que contiene credenciales de seguridad, como un nombre de usuario y una contraseña. Los runbooks y las configuraciones de DSC pueden usar cmdlets que aceptan un objeto PSCredential para autenticación, o bien pueden extraer el nombre de usuario y la contraseña el objeto PSCredential para proporcionarlos a alguna aplicación o servicio que requiere autenticación. Las propiedades de una credencial se almacenan de manera segura en Azure Automation y es posible tener acceso a ellas en el runbook o la configuración de DSC con la actividad [Get-AutomationPSCredential](#activities) .

[!INCLUDE [gdpr-dsr-and-stp-note.md](../../../includes/gdpr-dsr-and-stp-note.md)]

> [!NOTE]
> Los recursos protegidos en Azure Automation incluyen credenciales, certificados, conexiones y variables cifradas. Estos recursos se cifran y se almacenan en Azure Automation con una clave única que se genera para cada cuenta de Automation. Esta clave se almacena en Key Vault. Antes de almacenar un recurso seguro, la clave se carga desde Key Vault y luego se usa para cifrar el recurso.

## <a name="azure-powershell-az-cmdlets"></a>Cmdlets de Azure PowerShell Az

En el módulo Azure PowerShell Az, los cmdlets de la tabla siguiente se usan para crear y administrar recursos de credenciales de automatización con Windows PowerShell. Estos cmdlets se incluyen como parte del [módulo AzureAz.Automation](/powershell/azure/new-azureps-module-az?view=azps-1.1.0), que está disponible para utilizarse en las configuraciones de DSC y los runbooks de Automation.

| Cmdlets | Descripción |
|:--- |:--- |
| [Get-AzAutomationCredential](/powershell/module/az.automation/get-azautomationcredential?view=azps-3.3.0) |Recupera información acerca de un recurso de credencial. No devuelve un objeto PSCredential.  |
| [New-AzAutomationCredential](/powershell/module/az.automation/new-azautomationcredential?view=azps-3.3.0) |Crea una nueva credencial de Automation. |
| [Remove-AzAutomationCredential](/powershell/module/az.automation/remove-azautomationcredential?view=azps-3.3.0) |Quita una credencial de Automation. |
| [Set-AzAutomationCredential](/powershell/module/az.automation/set-azautomationcredential?view=azps-3.3.0) |Establece las propiedades para una credencial de Automatización existente. |

## <a name="activities"></a>Actividades

Las actividades de la tabla siguiente se usan para tener acceso a las credenciales de un runbook y configuraciones de DSC.

| Actividades | Descripción |
|:--- |:--- |
| Get-AutomationPSCredential |Obtiene una credencial para usarla en un runbook o una configuración de DSC. Devuelve un objeto [System.Management.Automation.PSCredential](/dotnet/api/system.management.automation.pscredential) . |

> [!NOTE]
> Debe evitar el uso de variables en el parámetro –Name de Get-AutomationPSCredential, debido a que esto podría complicar la detección de las dependencias entre runbooks o configuraciones de DSC, y recursos de credenciales en tiempo de diseño.

## <a name="python2-functions"></a>Funciones de Python2

La función de la tabla siguiente se usa para obtener acceso a las credenciales de un runbook de Python2.

| Función | Descripción |
|:---|:---|
| automationassets.get_automation_credential | Recupera información acerca de un recurso de credencial. |

> [!NOTE]
> Debe importar el módulo "automationassets" en la parte superior del runbook de Python para poder tener acceso a las funciones del recurso.

## <a name="creating-a-new-credential-asset"></a>Creación de un nuevo recurso de credencial

### <a name="to-create-a-new-credential-asset-with-the-azure-portal"></a>Para crear un nuevo recurso de credencial con el Portal de Azure

1. En la cuenta de Automation, seleccione **Credenciales** en **Recursos compartidos**.
1. Seleccione **Agregar una credencial**.
1. Complete el formulario y seleccione **Crear** para guardar la nueva credencial.

> [!NOTE]
> No se admiten cuentas de usuario que usen la autenticación multifactor en Azure Automation.

### <a name="to-create-a-new-credential-asset-with-windows-powershell"></a>Para crear un nuevo recurso de credencial con Windows PowerShell

Los comandos de ejemplo siguientes muestran cómo crear una nueva credencial de automatización. Primero se crea un objeto PSCredential con el nombre y la contraseña y, a continuación, se usa para crear el recurso de credencial. De manera alternativa, podría usar el cmdlet **Get-Credential** para que se le solicite escribir un nombre y una contraseña.

```powershell
$user = "MyDomain\MyUser"
$pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
$cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred
```

## <a name="using-a-powershell-credential"></a>Uso de una credencial de PowerShell

Puede recuperar un activo de credencial en un runbook o una configuración de DSC con la actividad **Get-AutomationPSCredential** . Esto devuelve un [objeto PSCredential](/dotnet/api/system.management.automation.pscredential) que puede usar con una actividad o cmdlet que requiere un parámetro PSCredential. También puede recuperar las propiedades del objeto de credencial para usarlas individualmente. El objeto tiene una propiedad para el nombre de usuario y la contraseña segura, o bien puede usar el método **GetNetworkCredential** para devolver un objeto [NetworkCredential](/dotnet/api/system.net.networkcredential) que proporcionará una versión no protegida de la contraseña.

> [!NOTE]
> **Get-AzAutomationCredential** no devuelve ningún objeto **PSCredential** que pueda utilizarse para la autenticación. Solo proporciona información acerca de la credencial. Si necesita usar una credencial en un runbook, debe usar el cmdlet **Get-AutomationPSCredential** para recuperar el objeto **PSCredential**.

### <a name="textual-runbook-sample"></a>Ejemplo de runbook de texto

Los comandos de ejemplo siguientes muestran cómo usar una credencial de PowerShell en un runbook. En este ejemplo, se recupera la contraseña y su nombre de usuario y contraseña se asignan a variables.

```azurepowershell
$myCredential = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCredential.UserName
$securePassword = $myCredential.Password
$password = $myCredential.GetNetworkCredential().Password
```

También puede usar una credencial para autenticarse en Azure con [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-3.3.0). En la mayoría de los casos, deberá usar una [cuenta de ejecución](../manage-runas-account.md) y recuperarla con [Get-AzAutomationConnection](../automation-connections.md).

```azurepowershell
$myCred = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCred.UserName
$securePassword = $myCred.Password
$password = $myCred.GetNetworkCredential().Password

$myPsCred = New-Object System.Management.Automation.PSCredential ($userName,$password)

Connect-AzureRmAccount -Credential $myPsCred
```

### <a name="graphical-runbook-sample"></a>Ejemplo de runbook gráfico

Para agregar una actividad **Get-AutomationPSCredential** a un runbook gráfico, haga clic con el botón derecho en la credencial en el panel Biblioteca del editor gráfico y, después, seleccione **Agregar a lienzo**.

![Agregar credencial a lienzo](../media/credentials/credential-add-canvas.png)

La imagen siguiente muestra un ejemplo de cómo usar una credencial en un runbook gráfico. En este caso, se usa para proporcionar la autenticación de un runbook a los recursos de Azure, tal como se describe en [Autenticación de Runbooks con Administración de servicios de Azure AD y Resource Manager](../automation-create-aduser-account.md). La primera actividad recupera la credencial que tiene acceso a la suscripción de Azure. A continuación, la actividad **Connect-AzureRmAccount** usa esta credencial para realizar la autenticación en cualquier actividad que venga después. Aquí se encuentra un [vínculo de canalización](../automation-graphical-authoring-intro.md#links-and-workflow) , debido a que **Get-AutomationPSCredential** espera un solo objeto.  

![Agregar credencial a lienzo](../media/credentials/get-credential.png)

## <a name="using-a-powershell-credential-in-dsc"></a>Uso de una credencial de PowerShell en DSC

Aunque las configuraciones DSC en Azure Automation pueden hacer referencia a los activos de credenciales con **Get-AutomationPSCredential**, los activos de credenciales también se pueden pasar, si se desea, con los parámetros. Para obtener más información, consulte [Compilación de configuraciones en DSC de Azure Automation](../automation-dsc-compile.md#credential-assets).

## <a name="using-credentials-in-python2"></a>Uso de credenciales en Python2

En el ejemplo siguiente se muestra un acceso a credenciales en runbooks de Python2.

```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a credential
cred = automationassets.get_automation_credential("credtest")
print cred["username"]
print cred["password"]
```

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información acerca de los vínculos en creación gráfica, consulte [Vínculos en Creación gráfica](../automation-graphical-authoring-intro.md#links-and-workflow)
* Para entender los distintos métodos de autenticación con la automatización, vea [Seguridad de Azure Automation](../automation-security-overview.md)
* Para empezar a trabajar con runbooks gráficos, consulte [Mi primer runbook gráfico](../automation-first-runbook-graphical.md)
* Para empezar a trabajar con runbooks de flujo de trabajo de PowerShell, consulte [Mi primer runbook de flujo de trabajo de PowerShell](../automation-first-runbook-textual.md)
* Para empezar a trabajar con runbooks de Python2, vea [My first Python2 runbook](../automation-first-runbook-textual-python2.md) (Mi primer runbook de Python2). 
