---
title: Administración de variables en Azure Automation
description: Los activos de variables son valores que están disponibles para todos los runbooks y configuraciones de DSC en Azure Automation.  En este artículo se explican los detalles de las variables y cómo trabajar con ellas en la creación de texto y gráficos.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 05/14/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 4778e9b2c0d3b442b214966ab69810d2f42b70b8
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732759"
---
# <a name="manage-variables-in-azure-automation"></a>Administración de variables en Azure Automation

Los recursos de variables son valores que están disponibles para todos los runbooks y configuraciones de DSC en su cuenta de Automation. Puede administrarlos desde Azure Portal, PowerShell, un runbook o en una configuración de DSC.

Las variables de Automation son útiles para los siguientes escenarios:

- Uso compartido de un valor entre varios runbooks o configuraciones de DSC.

- Uso compartido de un valor entre varios trabajos del mismo runbook o configuración de DSC.

- Administración de un valor que usan los runbooks o las configuraciones de DSC desde el portal o la línea de comandos de PowerShell. Un ejemplo es un conjunto de elementos de configuración comunes, como una lista de nombres de máquinas virtuales específica, un grupo de recursos específico, un nombre de dominio de AD y más.  

Azure Automation conserva las variables y hace que estén disponibles aunque se produzca un error en un runbook o en la configuración de DSC. Este comportamiento permite que un runbook o configuración de DSC establezca un valor para que otro runbook lo use, o bien que lo use el mismo runbook o la misma configuración de DSC la siguiente vez que se ejecute.

Azure Automation almacena cada variable cifrada de forma segura. Al crear una variable, puede especificar su cifrado y almacenamiento de Azure Automation como recursos seguros. Otros recursos seguros incluyen credenciales, certificados y conexiones. Azure Automation cifra estos recursos y los almacena con una clave única que se genera para cada cuenta de Automation. La clave se almacena en una instancia de Key Vault administrada por el sistema. Antes de almacenar un recurso seguro, Azure Automation carga la clave desde Key Vault y después la usa para cifrar el recurso. 

>[!NOTE]
>Este artículo se ha actualizado para usar el nuevo módulo Az de Azure PowerShell. Aún puede usar el módulo de AzureRM que continuará recibiendo correcciones de errores hasta diciembre de 2020 como mínimo. Para más información acerca del nuevo módulo Az y la compatibilidad con AzureRM, consulte [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Presentación del nuevo módulo Az de Azure PowerShell). Para obtener instrucciones sobre la instalación del módulo Az en Hybrid Runbook Worker, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Puede actualizar los módulos de su cuenta de Automation a la versión más reciente mediante [Actualización de módulos de Azure PowerShell en Azure Automation](../automation-update-azure-modules.md).

## <a name="variable-types"></a>Tipos de variables

Cuando se crea una variable con Azure Portal, debe especificar un tipo de datos en la lista desplegable para que el portal pueda mostrar el control adecuado para escribir el valor de la variable. Los siguientes tipos de variable están disponibles en Azure Automation:

* String
* Entero
* DateTime
* Boolean
* Null

La variable no se limita al tipo de datos designado. Debe establecer la variable mediante Windows PowerShell si se desea especificar un valor de un tipo diferente. Si indica `Not defined`, el valor de la variable se establece en NULL. Debe establecer el valor con el cmdlet [Set-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/set-azautomationvariable?view=azps-3.5.0) o la actividad `Set-AutomationVariable`.

Azure Portal no se puede usar para crear o cambiar el valor de un tipo de variable complejo. Sin embargo, puede proporcionar un valor de cualquier tipo mediante Windows PowerShell. Los tipos complejos se recuperan como un objeto [PSCustomObject](/dotnet/api/system.management.automation.pscustomobject).

Puede almacenar varios valores en una única variable mediante la creación de una matriz o tabla hash y guardarlos en la variable.

>[!NOTE]
>Las variables de nombre de máquina virtual pueden tener un máximo de 80 caracteres. Las variables de grupo de recursos pueden tener un máximo de 90 caracteres. Consulte [Reglas y restricciones de nomenclatura para los recursos de Azure](https://docs.microsoft.com/azure/azure-resource-manager/management/resource-name-rules).

## <a name="powershell-cmdlets-that-create-and-manage-variable-assets"></a>Cmdlets de PowerShell que crean y administran recursos de variable

En el módulo Az, los cmdlets de la tabla siguiente se usan para crear y administrar recursos de variable de Automation con Windows PowerShell. Estos cmdlets se incluyen como parte del [módulo Az.Automation](/powershell/azure/overview), que está disponible para usarse en las configuraciones de DSC y los runbooks de Automation.

| Cmdlet | Descripción |
|:---|:---|
|[Get-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationvariable?view=azps-3.5.0) | Recupera el valor de una variable existente. Este cmdlet no se puede usar para recuperar el valor de una variable cifrada. La única forma de hacerlo es usando la actividad `Get-AutomationVariable` en un runbook o una configuración de DSC. |
|[New-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationvariable?view=azps-3.5.0) | Crea una nueva variable y establece su valor.|
|[Remove-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationvariable?view=azps-3.5.0)| Quita una variable existente.|
|[Set-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/set-azautomationvariable?view=azps-3.5.0)| Establece el valor de una variable existente. |

## <a name="activities-to-access-variables-in-runbooks-and-dsc-configurations"></a>Actividades para acceder a variables en runbooks y configuraciones de DSC

Las actividades de la tabla siguiente se usan para acceder a las variables en runbooks y configuraciones de DSC. Los cmdlets de estas actividades vienen con el módulo global `Orchestrator.AssetManagement.Cmdlets`.

| Actividad | Descripción |
|:---|:---|
|`Get-AutomationVariable`|Recupera el valor de una variable existente.|
|`Set-AutomationVariable`|Establece el valor de una variable existente.|

> [!NOTE]
> Evite usar variables en el parámetro `Name` de `Get-AutomationVariable` en un runbook o configuración de DSC. El uso de dicho parámetro puede complicar la detección de dependencias entre runbooks o configuraciones de DSC y variables de Automation durante el tiempo de diseño.

Cabe mencionar que `Get-AutomationVariable` no funciona en PowerShell, sino solo en un runbook o una configuración de DSC. Por ejemplo, para ver el valor de una variable cifrada, puede crear un runbook para obtener la variable y escribirla en el flujo de salida:
 
```powershell
$mytestencryptvar = Get-AutomationVariable -Name TestVariable
Write-output "The encrypted value of the variable is: $mytestencryptvar"
```

## <a name="functions-to-access-variables-in-python-2-runbooks"></a>Funciones para acceder a variables en runbooks de Python 2

Las funciones de la siguiente tabla sirven para acceder a las variables de un runbook de Python 2.

|Funciones de Python 2|Descripción|
|:---|:---|
|`automationassets.get_automation_variable`|Recupera el valor de una variable existente. |
|`automationassets.set_automation_variable`|Establece el valor de una variable existente. |

> [!NOTE]
> Para acceder a las funciones del activo, debe importar el módulo `automationassets` en la parte superior del runbook de Python.

## <a name="working-with-automation-variables"></a>Uso de variables de Automation

>[!NOTE]
>Si va a quitar el cifrado de una variable, debe eliminarla y volver a crearla sin cifrado.

### <a name="create-a-new-variable-using-the-azure-portal"></a>Creación de una nueva variable mediante Azure Portal

1. En la cuenta de Automation, haga clic en el mosaico **Activos** y en la hoja **Activos** y, luego, seleccione **Variables**.
2. En la hoja **Variables**, seleccione **Agregar una variable**.
3. Complete las opciones en la hoja **Nueva variable** y después haga clic en **Crear** para guardar la nueva variable.

> [!NOTE]
> Una vez que una variable cifrada se guarde, no podrá verse en el portal. Solo se podrá actualizar.

### <a name="create-and-use-a-variable-in-windows-powershell"></a>Creación y uso de una variable en Windows PowerShell

Un script de PowerShell usa el cmdlet `New-AzAutomationVariable`, o su módulo de AzureRM equivalente, para crear una variable y establecer su valor inicial. Si la variable está cifrada, la llamada debe usar el parámetro `Encrypted`.

El script recupera el valor de la variable por medio de [Get-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationvariable?view=azps-3.5.0). Si el valor es de un tipo simple, el cmdlet recupera ese mismo tipo. Si es un tipo complejo, se recupera un tipo `PSCustomObject`.

>[!NOTE]
>Un script de PowerShell no puede recuperar un valor cifrado. La única forma de hacerlo es usando una actividad `Get-AutomationVariable` en un runbook o una configuración de DSC.

En el siguiente ejemplo se muestra cómo crear una variable de tipo cadena y después se devuelve su valor.

```powershell
New-AzAutomationVariable -ResourceGroupName "ResourceGroup01" 
–AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
–Encrypted $false –Value 'My String'
$string = (Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value
```

En el siguiente ejemplo se muestra cómo crear una variable con un tipo complejo y después se recuperan sus propiedades. En este caso, se usa un objeto de máquina virtual de [Get-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Get-AzVM?view=azps-3.5.0).

```powershell
$vm = Get-AzVM -ResourceGroupName "ResourceGroup01" –Name "VM01"
New-AzAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm

$vmValue = (Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
$vmName = $vmValue.Name
$vmIpAddress = $vmValue.IpAddress
```

### <a name="create-and-use-a-variable-in-a-runbook-or-dsc-configuration"></a>Creación y uso de una variable en un runbook o una configuración de DSC

La única forma de crear una variable nueva desde un runbook o una configuración de DSC es usando el cmdlet `New-AzAutomationVariable` o su módulo de AzureRM equivalente. El script usa este cmdlet para establecer el valor inicial de la variable. Luego, el script puede recuperar el valor usando `Get-AzAutomationVariable`. Si el valor es de un tipo simple, se recupera ese mismo tipo. Si es un tipo complejo, se recupera un tipo `PSCustomObject`.

>[!NOTE]
>La única forma de recuperar una variable en un runbook o una configuración de DSC es usando la actividad `Get-AutomationVariable`. 

### <a name="textual-runbook-samples"></a>Ejemplos de runbook textual

#### <a name="set-and-retrieve-a-simple-value-from-a-variable"></a>Establecimiento y recuperación de un valor simple de una variable

Los comandos de ejemplo siguientes muestran cómo establecer y recuperar una variable en un runbook textual. En este ejemplo se da por hecho que se han creado las variables de entero `NumberOfIterations` y `NumberOfRunnings` y una variable de cadena denominada `SampleMessage`.

```powershell
$NumberOfIterations = Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
$NumberOfRunnings = Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
$SampleMessage = Get-AutomationVariable -Name 'SampleMessage'

Write-Output "Runbook has been run $NumberOfRunnings times."

for ($i = 1; $i -le $NumberOfIterations; $i++) {
    Write-Output "$i`: $SampleMessage"
}
Set-AzAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)
```

#### <a name="set-and-retrieve-a-variable-in-a-python-2-runbook"></a>Establecimiento y recuperación de una variable en un runbook de Python 2

En el siguiente ejemplo se muestra cómo usar una variable, establecer una variable y controlar una excepción de una variable inexistente en un runbook de Python 2.

```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a variable
value = automationassets.get_automation_variable("test-variable")
print value

# set a variable (value can be int/bool/string)
automationassets.set_automation_variable("test-variable", True)
automationassets.set_automation_variable("test-variable", 4)
automationassets.set_automation_variable("test-variable", "test-string")

# handle a non-existent variable exception
try:
    value = automationassets.get_automation_variable("nonexisting variable")
except AutomationAssetNotFound:
    print "variable not found"
```

### <a name="graphical-runbook-samples"></a>Ejemplos de runbook gráfico

En un runbook gráfico, puede agregar la actividad `Get-AutomationVariable` o `Set-AutomationVariable`. Basta con hacer clic derecho en la variable desde el panel Biblioteca del editor gráfico y seleccionar la actividad que se va a agregar.

![Adición de una variable al lienzo](../media/variables/runbook-variable-add-canvas.png)

#### <a name="set-values-in-a-variable"></a>Establecimiento de valores en una variable

La imagen siguiente muestra las actividades de ejemplo para actualizar una variable con un valor simple en un runbook gráfico. En este ejemplo, `Get-AzVM` recupera una única máquina virtual de Azure y guarda el nombre del equipo en una variable de cadena de Automation existente. No importa si el [vínculo es una canalización o una secuencia](../automation-graphical-authoring-intro.md#links-and-workflow), ya que el código solo espera un único objeto en la salida.

![Establecimiento de una variable simple](../media/variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre cómo conectar actividades en la creación gráfica, consulte [Vínculos en Creación gráfica](../automation-graphical-authoring-intro.md#links-and-workflow).
- Para empezar a trabajar con runbooks gráficos, consulte [Mi primer runbook gráfico](../automation-first-runbook-graphical.md).
