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
ms.openlocfilehash: 605d1bc72406a9aeecc9273f9bd2d7fd2b30ab11
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2020
ms.locfileid: "80618688"
---
# <a name="manage-modules-in-azure-automation"></a>Administración de módulos en Azure Automation

Puede importar los módulos de PowerShell en Azure Automation para que los cmdlets estén disponibles en runbooks y los recursos de DSC estén disponibles en las configuraciones de DSC. Azure Automation almacena estos módulos en segundo plano. En el tiempo de ejecución del trabajo de compilación de DSC y del trabajo de runbook, los carga en espacios aislados de Azure Automation, donde se ejecutan los runbooks y se compilan las configuraciones de DSC. Todos los recursos de DSC en módulos se sitúan automáticamente en el servidor de extracción de DSC de Automatización. Las máquinas pueden extraerlos cuando aplican configuraciones de DSC.

Los módulos que se usan en Azure Automation pueden ser módulos personalizados que ha creado, módulos de la Galería de PowerShell, o los módulos AzureRM y Az de Azure. Cuando se crea una cuenta de Automation, algunos módulos se importan de manera predeterminada.

## <a name="default-modules"></a>Módulos predeterminados

En la siguiente tabla se incluyen los módulos que se importan de manera predeterminada cuando se crea una cuenta de Automation. Automation puede importar versiones más recientes de estos módulos. Sin embargo, no se puede eliminar la versión original de la cuenta de Automation aunque se elimine una versión más reciente.

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

## <a name="internal-cmdlets"></a>Cmdlets internos

La siguiente tabla muestra los cmdlets del módulo `Orchestrator.AssetManagement.Cmdlets` interno que se importa en cada cuenta de Automation. Estos cmdlets son accesibles en los runbooks y configuraciones de DSC y le permiten interactuar con los recursos dentro de la cuenta de Automation. Además, los cmdlets internos le permiten recuperar secretos a partir de variables cifradas, credenciales y conexiones cifradas. Los cmdlets de Azure PowerShell no pueden recuperar estos secretos. Estos cmdlets no requieren que se conecte implícitamente a Azure al usarlos, igual que cuando utiliza una cuenta de ejecución para autenticarse en Azure.

>[!NOTE]
>Estos cmdlets internos están disponibles en las instancias de Hybrid Runbook Worker para Windows, pero no en las de Hybrid Runbook Worker para Linux. Use [AzureRM.Automation](https://docs.microsoft.com/powershell/module/AzureRM.Automation/?view=azurermps-6.13.0) o los cmdlets [Az module](../az-modules.md) según corresponda para los runbooks que se ejecutan directamente en el equipo o en los recursos del entorno. 

|Nombre|Descripción|
|---|---|
|Get-AutomationCertificate|`Get-AutomationCertificate [-Name] <string> [<CommonParameters>]`|
|Get-AutomationConnection|`Get-AutomationConnection [-Name] <string> [-DoNotDecrypt] [<CommonParameters>]` |
|Get-AutomationPSCredential|`Get-AutomationPSCredential [-Name] <string> [<CommonParameters>]` |
|Get-AutomationVariable|`Get-AutomationVariable [-Name] <string> [-DoNotDecrypt] [<CommonParameters>]`|
|Set-AutomationVariable|`Set-AutomationVariable [-Name] <string> -Value <Object> [<CommonParameters>]` |
|Start-AutomationRunbook|`Start-AutomationRunbook [-Name] <string> [-Parameters <IDictionary>] [-RunOn <string>] [-JobId <guid>] [<CommonParameters>]`|
|Wait-AutomationJob|`Wait-AutomationJob -Id <guid[]> [-TimeoutInMinutes <int>] [-DelayInSeconds <int>] [-OutputJobsTransitionedToRunning] [<CommonParameters>]`|

## <a name="importing-modules"></a>Importación de módulos

Hay varias formas de importar un módulo en la cuenta de Automation. Las secciones siguientes muestran las distintas formas de importar un módulo.

> [!NOTE]
> La ruta de acceso máxima de un archivo en un módulo que se usa en Azure Automation es de 140 caracteres. Automation no puede importar ningún archivo con un tamaño de ruta de acceso superior a 140 caracteres en la sesión de PowerShell con `Import-Module`.

### <a name="import-modules-in-azure-portal"></a>Importación de módulos en Azure Portal

Para importar un módulo en Azure Portal:

1. Vaya a su cuenta de Automation.
2. Seleccione **Módulos** en **Recursos compartidos**.
3. Haga clic en **Agregar un módulo**. 
4. Seleccione el archivo **.zip** que contiene el módulo.
5. Haga clic en **Aceptar** para iniciar el proceso de importación.

### <a name="import-modules-using-powershell"></a>Importación de módulos mediante PowerShell

Puede usar el cmdlet [New-AzureRmAutomationModule](/powershell/module/azurerm.automation/new-azurermautomationmodule) para importar un módulo en la cuenta de Automation. El cmdlet toma una dirección URL para un paquete .zip del módulo.

```azurepowershell-interactive
New-AzureRmAutomationModule -Name <ModuleName> -ContentLinkUri <ModuleUri> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName>
```

También puede usar el mismo cmdlet para importar un módulo directamente desde Galería de PowerShell. Asegúrese de tomar `ModuleName` y `ModuleVersion` de la [Galería de PowerShell](https://www.powershellgallery.com).

```azurepowershell-interactive
$moduleName = <ModuleName>
$moduleVersion = <ModuleVersion>
New-AzureRmAutomationModule -AutomationAccountName <AutomationAccountName> -ResourceGroupName <ResourceGroupName> -Name $moduleName -ContentLinkUri "https://www.powershellgallery.com/api/v2/package/$moduleName/$moduleVersion"
```
### <a name="import-modules-from-powershell-gallery"></a>Importación de módulos desde la Galería de PowerShell

Puede importar módulos de la [Galería de PowerShell](https://www.powershellgallery.com) directamente desde la galería o desde la cuenta de Automation.

Para importar un módulo directamente desde la Galería de PowerShell:

1. Vaya a https://www.powershellgallery.com y busque el módulo que desea importar.
2. Haga clic en **Implementar en Azure Automation** en la pestaña **Azure Automation** de **Opciones de instalación**. Esta acción abre Azure Portal. 
3. En la página Importar, seleccione su cuenta de Automation y haga clic en **Aceptar**.

![Módulo de importación de la Galería de PowerShell](../media/modules/powershell-gallery.png)

Para importar un módulo de la Galería de PowerShell directamente desde la cuenta de Automation:

1. Seleccione **Módulos** en **Recursos compartidos**. 
2. En la página Módulos, haga clic en **Examinar galería** y busque el módulo en la galería. 
3. Seleccione el módulo que quiere importar y haga clic en **Importar**. 
4. En la página Importar, haga clic en **Aceptar** para iniciar el proceso de importación.

![Importación de la Galería de PowerShell desde Azure Portal](../media/modules/gallery-azure-portal.png)

## <a name="deleting-modules"></a>Eliminación de módulos

Si tiene problemas con un módulo o necesita revertir a una versión anterior de un módulo, puede eliminarlo de la cuenta de Automation. No se pueden eliminar las versiones originales de los [módulos predeterminados](#default-modules) que se importan al crear una cuenta de Automation. Si el módulo que desea eliminar es una versión más reciente de uno de los [módulos predeterminados](#default-modules), se revertirá a la versión que se instaló con la cuenta de Automation. En caso contrario, se quitará cualquier módulo que se elimine de la cuenta de Automation.

### <a name="delete-modules-in-azure-portal"></a>Eliminación de módulos en Azure Portal

Para eliminar un módulo en Azure Portal:

1. Vaya a la cuenta de Automation y seleccione **Módulos** en **Recursos compartidos**. 
2. Seleccione el módulo que quiera quitar. 
3. En la página **Módulo**, seleccione **Eliminar**. Si este módulo es uno de los [módulos predeterminados](#default-modules), se revertirá a la versión que existía cuando se creó la cuenta de Automation.

### <a name="delete-modules-using-powershell"></a>Eliminación de módulos mediante PowerShell

Para quitar un módulo a través de PowerShell, ejecute el siguiente comando:

```azurepowershell-interactive
Remove-AzureRmAutomationModule -Name <moduleName> -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName>
```
## <a name="adding-a-connection-type-to-your-module"></a>Adición de un tipo de conexión al módulo

Puede proporcionar un [tipo de conexión](../automation-connections.md) personalizada para usar en la cuenta de Automation mediante la adición de un archivo de metadatos opcional al módulo. Este archivo especifica un tipo de conexión de Azure Automation para su uso con los cmdlets del módulo en la cuenta de Automation. Para lograr esto, primero debe saber cómo crear un módulo de PowerShell. Consulte [Cómo escribir un módulo de script de PowerShell](/powershell/scripting/developer/module/how-to-write-a-powershell-script-module).

![Uso de una conexión personalizada en Azure Portal](../media/modules/connection-create-new.png)

El archivo que especifica las propiedades del tipo de conexión se denomina **&lt;ModuleName&gt;-Automation.json** y se encuentra en la carpeta del módulo del archivo **.zip** comprimido. Este archivo contiene los campos de una conexión que se necesita para conectarse al sistema o servicio que representa el módulo. La configuración permite la creación de un tipo de conexión en Azure Automation. Con este archivo, puede configurar los nombres y los tipos de campo del tipo de conexión del módulo, además de especificar si los campos deben cifrarse o el cifrado es opcional. El ejemplo siguiente es una plantilla en el formato de archivo **.json** que define propiedades de nombre de usuario y contraseña:

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

## <a name="best-practices-for-authoring-modules"></a>Procedimientos recomendados para la creación de módulos

Se recomienda que tenga en cuenta los aspectos de esta sección al crear un módulo de PowerShell para su uso en Azure Automation.

### <a name="version-folder"></a>Carpeta de versiones

NO incluya una carpeta de versiones en el paquete **.zip** del módulo.  Este problema es menos importante para los runbooks, pero causará un problema con el servicio State Configuration. Azure Automation crea automáticamente la carpeta de versiones cuando se distribuye el módulo a los nodos que administra DSC. Si existe una carpeta de versiones, terminará con dos instancias. Este es un ejemplo de estructura de carpetas para un módulo de DSC:

```powershell
myModule
  - DSCResources
    - myResourceFolder
      myResourceModule.psm1
      myResourceSchema.mof
  myModuleManifest.psd1
```

### <a name="help-information"></a>Información de ayuda

Incluya una sinopsis, una descripción y un identificador URI de ayuda para todos los cmdlets del módulo. En PowerShell, puede definir la información de ayuda de los cmdlets mediante el cmdlet `Get-Help`. En el ejemplo siguiente se muestra cómo definir una sinopsis y ayudar a los URI en un archivo de módulo **.psm1**:

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

  Proporcionar esta información permite mostrar el texto de ayuda mediante el cmdlet `Get-Help` en la consola de PowerShell. Este texto también se muestra en Azure Portal.

  ![Ayuda para los módulos de integración](../media/modules/module-activity-description.png)

### <a name="connection-type"></a>Tipo de conexión

Si el módulo se conecta a un servicio externo, defina un [tipo de conexión](#adding-a-connection-type-to-your-module). Cada cmdlet del módulo debería aceptar un objeto de conexión (una instancia de ese tipo de conexión) como parámetro. Los usuarios asignan parámetros del recurso de conexión a los parámetros correspondientes del cmdlet cada vez que llaman a un cmdlet. Según el ejemplo de runbook anterior, usa un recurso de conexión de Contoso de ejemplo denominado `ContosoConnection` para acceder a recursos de Contoso y devolver datos desde el servicio externo.

  En el ejemplo siguiente, los campos se asignan a las propiedades `UserName` y `Password` de un objeto `PSCredential` y, a continuación, se pasa al cmdlet.

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

  Para habilitar un comportamiento como este en sus cmdlets, permítales que acepten directamente un objeto de conexión como parámetro en lugar de usar solamente campos de conexión en los parámetros. Normalmente, necesitará un conjunto de parámetros para cada uno de ellos, de forma que los usuarios que no utilicen Azure Automation puedan llamar a los cmdlets sin necesidad de crear una tabla hash que actúe como objeto de conexión. El conjunto de parámetros `UserAccount` se usa para pasar las propiedades de los campos de conexión. `ConnectionObject` permite pasar directamente la conexión.

### <a name="output-type"></a>Tipo de salida

Defina el tipo de salida de todos los cmdlets del módulo. Definir un tipo de salida en un cmdlet permite que IntelliSense le ayude a determinar, en tiempo de diseño, las propiedades de salida del cmdlet, lo que resultará útil durante la creación. Este procedimiento recomendado resulta especialmente útil durante la creación gráfica de un runbook de Automation, para el que la información en tiempo de diseño resulta esencial para facilitar la experiencia del usuario con el módulo.

Agregue `[OutputType([<MyOutputType>])]` en el que `MyOutputType` es un tipo válido. Para más información sobre `OutputType`, consulte [About Functions OutputTypeAttribute](/powershell/module/microsoft.powershell.core/about/about_functions_outputtypeattribute) (Acerca de las funciones OutputTypeAttribute). El siguiente código es un ejemplo de cómo agregar `OutputType` a un cmdlet:

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

### <a name="cmdlet-state"></a>Estado del cmdlet

Cree todos los cmdlets del módulo sin estado. Pueden ejecutarse varios trabajos de runbook simultáneamente en el mismo elemento `AppDomain` y el mismo proceso y espacio aislado. Si no hay ningún estado compartido en esos niveles, los trabajos pueden afectar entre sí. Este comportamiento puede provocar problemas intermitentes y difíciles de diagnosticar. A continuación se muestra un ejemplo de lo que NO hay que hacer:

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

### <a name="module-dependency"></a>Dependencia de los módulos

Asegúrese de que el módulo esté incluido en su totalidad en un paquete compatible con Xcopy. Los módulos de Azure Automation se distribuyen en los espacios aislados de Automation cuando los runbooks se ejecutan. Los módulos deben funcionar de forma independiente con respecto al host que los ejecuta. 

Debe ser capaz de comprimir y mover un paquete de módulo y conseguir que funcione con normalidad cuando se importe en el entorno de PowerShell de otro host. Para que esto ocurra, asegúrese de que el módulo no dependa de ningún archivo que esté fuera de la carpeta del módulo que se comprime cuando el módulo se importa en Azure Automation. 

El módulo no debe depender de ninguna configuración de registro única en un host. Un ejemplo es la configuración que se realiza cuando se instala un producto. 

Asegúrese de que todos los archivos del módulo tienen rutas de acceso con menos de 140 caracteres. Las rutas de acceso con más de 140 caracteres provocarán problemas de importación del runbook. Si no se sigue este procedimiento recomendado, el módulo no podrá utilizarse en Azure Automation.  

### <a name="references-to-azurerm-and-az"></a>Referencias a AzureRM y Az

Si hace referencia al [módulo Az de Azure Powershell](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) en el módulo, asegúrese de que no hace también referencia a AzureRM. No se puede usar el módulo Az junto con el módulo AzureRM. Az se admite en los runbooks, pero no se importa de forma predeterminada. Para más información sobre el módulo de Az y las consideraciones que debe tener en cuenta, consulte [Compatibilidad con módulos de Az en Azure Automation](../az-modules.md).

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre la creación de módulos de PowerShell, consulte [Escritura de un módulo de Windows PowerShell](/powershell/scripting/developer/windows-powershell).
