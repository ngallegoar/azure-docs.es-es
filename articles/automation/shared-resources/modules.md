---
title: Administración de módulos en Azure Automation
description: En este artículo se describe cómo administrar módulos en Azure Automation.
services: automation
ms.service: automation
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: e300bc0f29808215673407d21b65fe329e50ad45
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79235428"
---
# <a name="manage-modules-in-azure-automation"></a>Administración de módulos en Azure Automation

Azure Automation proporciona la capacidad de importar los módulos de PowerShell en su cuenta de Automation para que los usen los runbooks basados en PowerShell. Estos módulos pueden ser módulos personalizados que ha creado, desde la Galería de PowerShell, o los módulos AzureRM y Az para Azure. Cuando se crea una cuenta de Automation, algunos módulos se importan de manera predeterminada.

## <a name="import-modules"></a>Importación de módulos

Hay varias formas de importar un módulo en la cuenta de Automation. Las secciones siguientes muestran las distintas formas de importar un módulo.

> [!NOTE]
> La ruta de acceso máxima de un archivo en un módulo que se usará en Azure Automation es de 140 caracteres. Cualquier ruta de acceso con más de 140 caracteres no podrá importarse en la sesión de PowerShell con `Import-Module`.

### <a name="powershell"></a>PowerShell

Puede usar [New-AzureRmAutomationModule](/powershell/module/azurerm.automation/new-azurermautomationmodule) para importar un módulo en la cuenta de Automation. El cmdlet toma una dirección URL a un paquete zip del módulo.

```azurepowershell-interactive
New-AzureRmAutomationModule -Name <ModuleName> -ContentLinkUri <ModuleUri> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName>
```

También puede usar el mismo cmdlet para importar un módulo directamente desde Galería de PowerShell. Asegúrese de capturar **ModuleName** y **ModuleVersion** de [Galería de PowerShell](https://www.powershellgallery.com).

```azurepowershell-interactive
$moduleName = <ModuleName>
$moduleVersion = <ModuleVersion>
New-AzAutomationModule -AutomationAccountName <AutomationAccountName> -ResourceGroupName <ResourceGroupName> -Name $moduleName -ContentLinkUri "https://www.powershellgallery.com/api/v2/package/$moduleName/$moduleVersion"
```

### <a name="azure-portal"></a>Portal de Azure

En Azure Portal, vaya a la cuenta de Automation y seleccione **Módulos** en **Recursos compartidos**. Haga clic en **+ Agregar un módulo**. Seleccione un archivo **.zip** que contenga el módulo y haga clic en **Aceptar** para empezar a importar el proceso.

### <a name="powershell-gallery"></a>Galería de PowerShell

Los módulos de la Galería de PowerShell pueden importarse desde la [Galería de PowerShell](https://www.powershellgallery.com) directamente o desde su cuenta de Automation.

Para importar un módulo desde la Galería de PowerShell, vaya a https://www.powershellgallery.com y busque el módulo que quiere importar. Haga clic en **Implementar en Azure Automation** en la pestaña **Azure Automation** de **Opciones de instalación**. Esta acción abre Azure Portal. En la página **Importar**, seleccione su cuenta de Automation y haga clic en **Aceptar**.

![Módulo de importación de la Galería de PowerShell](../media/modules/powershell-gallery.png)

También puede importar módulos desde la Galería de PowerShell directamente desde su cuenta de Automation. En la cuenta de Automation, seleccione **Módulos** en **Recursos compartidos**. En la página Módulos, haga clic en **Examinar galería** y busque la Galería de PowerShell de un módulo. Seleccione el módulo que quiere importar y haga clic en **Importar**. En la página **Importar**, haga clic en **Aceptar** para iniciar el proceso de importación.

![Importación de la Galería de PowerShell desde Azure Portal](../media/modules/gallery-azure-portal.png)

## <a name="delete-modules"></a>Eliminación de los módulos

Si tiene problemas con un módulo o necesita revertir a una versión anterior de un módulo, puede eliminarlo de la cuenta de Automation. No se puede eliminar la versión original de los [módulos predeterminados](#default-modules) que se importaron al crear una cuenta de Automation. Si el módulo que desea eliminar es una versión más reciente de uno de los [módulos predeterminados](#default-modules) instalados, se revertirá a la versión que se instaló con la cuenta de Automation. En caso contrario, se quitará cualquier módulo que se elimine de la cuenta de Automation.

### <a name="azure-portal"></a>Portal de Azure

En Azure Portal, vaya a la cuenta de Automation y seleccione **Módulos** en **Recursos compartidos**. Seleccione el módulo que quiera quitar. En la página **Módulo**, seleccione **Eliminar**. Si este módulo es uno de los [módulos predeterminados](#default-modules), se revertirá a la versión que estaba presente cuando se creó la cuenta de Automation.

### <a name="powershell"></a>PowerShell

Para quitar un módulo a través de PowerShell, ejecute el siguiente comando:

```azurepowershell-interactive
Remove-AzureRmAutomationModule -Name <moduleName> -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName>
```

## <a name="internal-cmdlets"></a>Cmdlets internos

La siguiente es una lista de cmdlets del módulo `Orchestrator.AssetManagement.Cmdlets` interno que se importa en cada cuenta de Automation. Estos cmdlets son accesibles en los runbooks y en configuraciones de DSC y le permiten interactuar con los recursos dentro de la cuenta de Automation. Además, los cmdlets internos le permiten recuperar secretos de campos de valores de **Variable** cifrados, **Credenciales** y **Conexión** cifrada. Los cmdlets de Azure PowerShell no pueden recuperar estos secretos. Estos cmdlets no requieren que se conecte implícitamente a Azure al usarlos, como al usar una cuenta de ejecución para autenticarse en Azure.

>[!NOTE]
>Estos cmdlets internos están disponibles en las instancias de Hybrid Runbook Worker para Windows, pero no en las instancias de Hybrid Runbook Worker para Linux. Use [AzureRM.Automation](https://docs.microsoft.com/powershell/module/AzureRM.Automation/?view=azurermps-6.13.0) o los [módulos Az](../az-modules.md) según corresponda para los runbooks que se ejecutan directamente en el equipo o en los recursos del entorno. 
>

|Nombre|Descripción|
|---|---|
|Get-AutomationCertificate|`Get-AutomationCertificate [-Name] <string> [<CommonParameters>]`|
|Get-AutomationConnection|`Get-AutomationConnection [-Name] <string> [-DoNotDecrypt] [<CommonParameters>]` |
|Get-AutomationPSCredential|`Get-AutomationPSCredential [-Name] <string> [<CommonParameters>]` |
|Get-AutomationVariable|`Get-AutomationVariable [-Name] <string> [-DoNotDecrypt] [<CommonParameters>]`|
|Set-AutomationVariable|`Set-AutomationVariable [-Name] <string> -Value <Object> [<CommonParameters>]` |
|Start-AutomationRunbook|`Start-AutomationRunbook [-Name] <string> [-Parameters <IDictionary>] [-RunOn <string>] [-JobId <guid>] [<CommonParameters>]`|
|Wait-AutomationJob|`Wait-AutomationJob -Id <guid[]> [-TimeoutInMinutes <int>] [-DelayInSeconds <int>] [-OutputJobsTransitionedToRunning] [<CommonParameters>]`|

## <a name="add-a-connection-type-to-your-module"></a>Adición de un tipo de conexión al módulo

Puede proporcionar un [tipo de conexión](../automation-connections.md) personalizada para usar en la cuenta de Automation mediante la adición de un archivo opcional al módulo. Este archivo es un archivo de metadatos que especifica un tipo de conexión de Azure Automation para su uso con los cmdlets del módulo en la cuenta de Automation. Para lograr esto, primero debe saber cómo crear un módulo de PowerShell. Para obtener más información sobre la creación del módulo, consulte [Cómo escribir un módulo de script de PowerShell](/powershell/scripting/developer/module/how-to-write-a-powershell-script-module).

![Uso de una conexión personalizada en Azure Portal](../media/modules/connection-create-new.png)

Para agregar un tipo de conexión de Azure Automation, el módulo debe incluir un archivo llamado `<ModuleName>-Automation.json` que especifique las propiedades del tipo de conexión. El archivo json se coloca dentro de la carpeta del módulo del archivo .zip comprimido. Este archivo contiene los campos de una conexión que se necesita para conectarse al sistema o servicio que representa el módulo. La configuración finaliza la creación de un tipo de conexión en Azure Automation. Con este archivo, puede configurar los nombres y los tipos de campo del tipo de conexión del módulo, además de especificar si los campos deben cifrarse o el cifrado es opcional. El ejemplo siguiente es una plantilla en el formato de archivo json que define una propiedad de nombre de usuario y contraseña:

```json
{
   "ConnectionFields": [
   {
      "IsEncrypted":  false,
      "IsOptional":  true,
      "Name":  "Username",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  true,
      "IsOptional":  false,
      "Name":  "Password",
      "TypeName":  "System.String"
   }
   ],
   "ConnectionTypeName":  "MyModuleConnection",
   "IntegrationModuleName":  "MyModule"
}
```

## <a name="module-best-practices"></a>Procedimientos recomendados del módulo

Los módulos de PowerShell pueden importarse en Azure Automation para que sus cmdlets estén disponibles y puedan usarse en runbooks y para que sus recursos de DSC estén disponibles y puedan usarse en las configuraciones de DSC. En segundo plano, Azure Automation almacena estos módulos y en el tiempo de ejecución del trabajo de compilación de DSC y del trabajo de runbook, los carga en espacios aislados de Azure Automation, donde se ejecutan los runbooks y se compilan las configuraciones de DSC. Todos los recursos de DSC en módulos se sitúan automáticamente en el servidor de extracción de DSC de Automatización. Las máquinas los pueden extraer cuando se aplican las configuraciones de DSC.

Se recomienda que considere lo siguiente al crear un módulo de PowerShell para su uso en Azure Automation:

* NO incluya una carpeta de la versión en el paquete. zip.  Este problema es menos importante para los runbooks, pero causará un problema con el servicio State Configuration.  Azure Automation creará la carpeta de la versión automáticamente cuando el módulo se distribuya a los nodos administrados por DSC y, si ya existe una carpeta de la versión, acabará teniendo dos instancias.  Ejemplo de estructura de carpetas para un módulo de DSC:

```powershell
myModule
  - DSCResources
    - myResourceFolder
      myResourceModule.psm1
      myResourceSchema.mof
  myModuleManifest.psd1
```

* Incluya una sinopsis, una descripción y un identificador URI de ayuda para todos los cmdlets del módulo. En PowerShell, puede definir cierta información de ayuda para los cmdlets de forma que al usuario le resulte más fácil usarlos con el cmdlet **Get-Help** . En el ejemplo siguiente se muestra cómo definir una sinopsis y ayudar a los URI en un archivo de módulo .psm1:

  ```powershell
  <#
       .SYNOPSIS
        Gets a Contoso User account
  #>
  function Get-ContosoUser {
  [CmdletBinding](DefaultParameterSetName='UseConnectionObject', `
  HelpUri='https://www.contoso.com/docs/information')]
  [OutputType([String])]
  param(
     [Parameter(ParameterSetName='UserAccount', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [string]
     $UserName,

     [Parameter(ParameterSetName='UserAccount', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [string]
     $Password,

     [Parameter(ParameterSetName='ConnectionObject', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [Hashtable]
     $Connection
  )

  switch ($PSCmdlet.ParameterSetName) {
     "UserAccount" {
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $UserName, $Password
        Connect-Contoso -Credential $cred
     }
     "ConnectionObject" {
        Connect-Contoso -Connection $Connection
    }
  }
  }
  ```

  Al proporcionar esta información se muestra esta ayuda mediante el cmdlet **Get-Help** en la consola de PowerShell. Esta descripción también se muestra en Azure Portal.

  ![Ayuda para los módulos de integración](../media/modules/module-activity-description.png)

* Si el módulo se conecta a un servicio externo, debe contener un [tipo de conexión](#add-a-connection-type-to-your-module). En segundo lugar, cada cmdlet del módulo debería poder tomar como parámetro un objeto de conexión (una instancia del tipo de conexión). Los usuarios asignan parámetros del recurso de conexión a los parámetros correspondientes del cmdlet cada vez que llaman a un cmdlet. Según el ejemplo de runbook anterior, usa un recurso de conexión de Contoso de ejemplo denominado ContosoConnection para tener acceso a recursos de Contoso y devolver datos desde el servicio externo.

  En el ejemplo siguiente, los campos se asignan a las propiedades de nombre de usuario y contraseña de un objeto `PSCredential` y, a continuación, se pasa al cmdlet.

  ```powershell
  $contosoConnection = Get-AutomationConnection -Name 'ContosoConnection'

  $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $contosoConnection.UserName, $contosoConnection.Password
  Connect-Contoso -Credential $cred
  }
  ```

  Existe otra forma más fácil y eficaz de afrontar este comportamiento, y consiste en pasar directamente el objeto de conexión al cmdlet:

  ```powershell
  $contosoConnection = Get-AutomationConnection -Name 'ContosoConnection'

  Connect-Contoso -Connection $contosoConnection
  }
  ```

  Para habilitar un comportamiento como el del ejemplo anterior en sus cmdlets, permítales que acepten directamente un objeto de conexión como parámetro en lugar de usar solamente campos de conexión en los parámetros. Normalmente, necesitará un conjunto de parámetros para cada uno de ellos, de forma que los usuarios que no utilicen Azure Automation puedan llamar a los cmdlets sin necesidad de crear una tabla hash que actúe como objeto de conexión. El conjunto de parámetros `UserAccount` se usa para pasar las propiedades de los campos de conexión. `ConnectionObject` permite pasar directamente la conexión.

* Defina el tipo de salida de todos los cmdlets del módulo. Definir un tipo de salida en un cmdlet permite que IntelliSense le ayude a determinar, en tiempo de diseño, las propiedades de salida del cmdlet, lo que resultará útil durante la creación. Esto resulta especialmente útil durante la creación gráfica de un runbook de Automation, donde la información en tiempo de diseño resulta esencial para facilitar la experiencia del usuario con el módulo.

Agregue `[OutputType([<MyOutputType>])]`, donde MyOutputType es un tipo válido. Para obtener más información sobre OutputType, consulte [About Functions OutputTypeAttribute](/powershell/module/microsoft.powershell.core/about/about_functions_outputtypeattribute) (Acerca de las funciones OutputTypeAttribute). El siguiente código es un ejemplo de cómo agregar `OutputType` a un cmdlet:

  ```powershell
  function Get-ContosoUser {
  [OutputType([String])]
  param(
     [string]
     $Parameter1
  )
  # <script location here>
  }
  ```

  ![Tipo de salida de Runbooks gráficos](../media/modules/runbook-graphical-module-output-type.png)

  Este comportamiento es similar a la funcionalidad "type ahead" de la salida de los cmdlets de PowerShell ISE sin necesidad de ejecutarla.

  ![POSH IntelliSense](../media/modules/automation-posh-ise-intellisense.png)

* Cree todos los cmdlets del módulo sin estado. Pueden ejecutarse varios trabajos de runbook simultáneamente en el mismo elemento AppDomain y el mismo proceso y espacio aislado. Si no hay ningún estado compartido en esos niveles, los trabajos pueden afectar entre sí. Este comportamiento puede provocar problemas intermitentes y difíciles de diagnosticar.  A continuación se muestra un ejemplo de lo que no hay que hacer:

  ```powershell
  $globalNum = 0
  function Set-GlobalNum {
     param(
         [int] $num
     )

     $globalNum = $num
  }
  function Get-GlobalNumTimesTwo {
     $output = $globalNum * 2

     $output
  }
  ```

* El módulo debe estar incluido en su totalidad en un paquete compatible con Xcopy. Los módulos de Azure Automation se distribuyen en los espacios aislados de Automation cuando los runbooks tienen que ejecutarse. Los módulos deben trabajar independientemente del host en que se están ejecutando. Debe ser capaz de comprimir y mover un paquete de módulo y conseguir que funcione con normalidad cuando se importe en el entorno de PowerShell de otro host. Para que esto ocurra, el módulo no debería depender de los archivos de fuera de la carpeta del módulo. Esta carpeta es la carpeta que se comprime al importar el módulo en Azure Automation. El módulo tampoco debe depender de ninguna configuración única del Registro en un host, como los parámetros que se establecen cuando se instala un producto. Todos los archivos del módulo deben tener una ruta de acceso con menos de 140 caracteres. Las rutas de acceso con más de 140 caracteres provocará problemas de importación del runbook. Si no se sigue este procedimiento recomendado, el módulo no podrá utilizarse en Azure Automation.  

* Si hace referencia a [módulos de Azure Powershell Az](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) en el módulo, asegúrese de que no hace también referencia a `AzureRM`. El módulo `Az` no se puede usar junto con los módulos `AzureRM`. `Az` se admite en los runbooks, pero no se importa de forma predeterminada. Para obtener información sobre los módulos de `Az` y las consideraciones que debe tener en cuenta, consulte [Az module support in Azure Automation](../az-modules.md) (Soporte técnico para módulos de Az en Azure Automation).

## <a name="default-modules"></a>Módulos predeterminados

En la siguiente tabla se incluyen los módulos que se importan de manera predeterminada cuando se crea una cuenta de Automation. Los módulos que se muestran a continuación pueden tener versiones importadas más recientes. Sin embargo, la versión original no se puede quitar de la cuenta de Automation aunque se elimine su versión más reciente.

|Nombre del módulo|Versión|
|---|---|
| AuditPolicyDsc | 1.1.0.0 |
| Azure | 1.0.3 |
| Azure.Storage | 1.0.3 |
| AzureRM.Automation | 1.0.3 |
| AzureRM.Compute | 1.2.1 |
| AzureRM.Profile | 1.0.3 |
| AzureRM.Resources | 1.0.3 |
| AzureRM.Sql | 1.0.3 |
| AzureRM.Storage | 1.0.3 |
| ComputerManagementDsc | 5.0.0.0 |
| GPRegistryPolicyParser | 0,2 |
| Microsoft.PowerShell.Core | 0 |
| Microsoft.PowerShell.Diagnostics |  |
| Microsoft.PowerShell.Management |  |
| Microsoft.PowerShell.Security |  |
| Microsoft.PowerShell.Utility |  |
| Microsoft.WSMan.Management |  |
| Orchestrator.AssetManagement.Cmdlets | 1 |
| PSDscResources | 2.9.0.0 |
| SecurityPolicyDsc | 2.1.0.0 |
| StateConfigCompositeResources | 1 |
| xDSCDomainjoin | 1.1 |
| xPowerShellExecutionPolicy | 1.1.0.0 |
| xRemoteDesktopAdmin | 1.1.0.0 |

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre la creación de módulos de PowerShell, consulte [Writing a Windows PowerShell Module](/powershell/scripting/developer/windows-powershell)
