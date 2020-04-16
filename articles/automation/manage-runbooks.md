---
title: Administración de runbooks en Azure Automation
description: En este artículo se describe cómo administrar runbooks en Azure Automation.
services: automation
ms.subservice: process-automation
ms.date: 02/14/2019
ms.topic: conceptual
ms.openlocfilehash: 29ac9239b8dc87b1ed12fc8333bf5201fe8fa204
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2020
ms.locfileid: "80617136"
---
# <a name="manage-runbooks-in-azure-automation"></a>Administración de runbooks en Azure Automation

Para agregar un runbook a Azure Automation, puede [crear uno](#creating-a-runbook) o [importar uno existente](#importing-a-runbook) desde un archivo o desde la [Galería de runbooks](automation-runbook-gallery.md). Este artículo ofrece información sobre cómo crear e importar runbooks de un archivo. Puede obtener todos los detalles sobre el acceso a los módulos y los runbooks de la comunidad en [Galería de runbooks y módulos para Azure Automation](automation-runbook-gallery.md).

>[!NOTE]
>Este artículo se ha actualizado para usar el nuevo módulo Az de Azure PowerShell. Aún puede usar el módulo de AzureRM que continuará recibiendo correcciones de errores hasta diciembre de 2020 como mínimo. Para más información acerca del nuevo módulo Az y la compatibilidad con AzureRM, consulte [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Presentación del nuevo módulo Az de Azure PowerShell). Para obtener instrucciones sobre la instalación del módulo Az en Hybrid Runbook Worker, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Puede actualizar los módulos de su cuenta de Automation a la versión más reciente mediante [Actualización de módulos de Azure PowerShell en Azure Automation](automation-update-azure-modules.md).

## <a name="creating-a-runbook"></a>Crear un Runbook

Puede crear un nuevo runbook en Azure Automation mediante uno de los portales de Azure o Windows PowerShell. Cuando se haya creado el runbook, puede editarlo con la información de [Aprendizaje del flujo de trabajo de Windows PowerShell](automation-powershell-workflow.md) y [Creación gráfica en Azure Automation](automation-graphical-authoring-intro.md).

### <a name="create-a-runbook-in-the-azure-portal"></a>Creación de un runbook en Azure Portal

1. En Azure Portal, abra su cuenta de Automation.
2. Desde el centro de conectividad, seleccione **Runbooks** en **Automatización de procesos** para abrir la lista de runbooks.
3. Haga clic en **Crear un runbook**.
4. Escriba un nombre para el runbook y seleccione su [Tipo](automation-runbook-types.md). El nombre del runbook debe empezar por una letra y puede tener letras, números, guiones bajos y guiones.
5. Haga clic en **Crear** para crear el runbook y abra el editor.

### <a name="create-a-runbook-with-powershell"></a>Creación de un runbook con PowerShell

Puede usar el cmdlet [New-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationrunbook?view=azps-3.5.0) para crear un [runbook de flujo de trabajo de PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) vacío. Use el parámetro `Type` para especificar uno de los tipos de runbook definidos para `New-AzAutomationRunbook`.

En el ejemplo siguiente se muestra cómo crear un nuevo runbook vacío.

```azurepowershell-interactive
New-AzAutomationRunbook -AutomationAccountName MyAccount `
-Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell
```

## <a name="importing-a-runbook"></a>Importación de un runbook

Puede crear un nuevo runbook en Azure Automation mediante la importación de un script de PowerShell o un flujo de trabajo de PowerShell ( **.ps1**), un runbook gráfico exportado ( **.graphrunbook**) o un script de Python2 ( **.py**).  Necesita especificar el [tipo de runbook](automation-runbook-types.md) que se creará durante la importación teniendo en cuenta las consideraciones siguientes.

* Un archivo **.ps1** que no contenga un flujo de trabajo se puede importar en un [runbook de PowerShell](automation-runbook-types.md#powershell-runbooks) o en un [runbook de flujo de trabajo de PowerShell](automation-runbook-types.md#powershell-workflow-runbooks). Si lo importa en un runbook de flujo de trabajo de PowerShell, se convertirá en un flujo de trabajo. En este caso, se incluyen comentarios en el runbook para describir los cambios realizados.

* Un archivo **.ps1** que contiene un flujo de trabajo de PowerShell solo se puede importar en un [runbook de flujo de trabajo de PowerShell](automation-runbook-types.md#powershell-workflow-runbooks). Si el archivo contiene varios flujos de trabajo de PowerShell, se produce un error en la importación. Debe guardar cada flujo de trabajo en su propio archivo e importar cada uno por separado.

* Un archivo **.ps1** que contiene un flujo de trabajo de PowerShell no se debe importar en un [runbook de PowerShell](automation-runbook-types.md#powershell-runbooks), ya que el motor de scripts de PowerShell no lo reconoce.

* Un archivo **.graphrunbook** solo se puede importar en un nuevo [runbook gráfico](automation-runbook-types.md#graphical-runbooks). Tenga en cuenta que solo puede crear un runbook gráfico a partir de un archivo **.graphrunbook**.

### <a name="import-a-runbook-from-a-file-with-the-azure-portal"></a>Importación de un runbook desde un archivo con Azure Portal

Puede usar el siguiente procedimiento para importar un archivo de script en Azure Automation.

> [!NOTE]
> Solo puede importar un archivo **.ps1** en un runbook de flujo de trabajo de PowerShell mediante el portal.

1. En Azure Portal, abra su cuenta de Automation.
2. Desde el centro de conectividad, seleccione **Runbooks** en **Automatización de procesos** para abrir la lista de runbooks.
3. Haga clic en **Importar un runbook**.
4. Haga clic en **Archivo de runbook** y seleccione el archivo que se va a importar.
5. Si el campo **Nombre** está habilitado, tiene la opción de cambiar el nombre del runbook. El nombre debe empezar por una letra y puede tener letras, números, guiones bajos y guiones.
6. El [tipo de runbook](automation-runbook-types.md) se seleccionará automáticamente, pero puede cambiarlo después de tomar en cuenta las restricciones aplicables.
7. Haga clic en **Crear**. El runbook nuevo aparece en la lista de runbooks de la cuenta de Automation.
8. Debe [publicar el runbook](#publishing-a-runbook) para poder ejecutarlo.

> [!NOTE]
> Después de importar un runbook gráfico o un runbook gráfico de flujo de trabajo de PowerShell, puede convertirlo en otro tipo. Sin embargo, no puede convertir uno de estos runbooks gráficos en un runbook textual.

### <a name="import-a-runbook-from-a-script-file-with-windows-powershell"></a>Importación de un runbook de un archivo de script con Windows PowerShell

Use el cmdlet [Import-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/import-azautomationrunbook?view=azps-3.5.0) para importar un archivo de script como un runbook de flujo de trabajo de PowerShell en borrador. Si ya existe el runbook, se producirá un error de importación, a menos que utilice el parámetro `Force` con el cmdlet.

En los ejemplos siguientes se muestra cómo importar un archivo de script en un runbook.

```azurepowershell-interactive
$automationAccountName =  "AutomationAccount"
$runbookName = "Sample_TestRunbook"
$scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
$RGName = "ResourceGroup"

Import-AzAutomationRunbook -Name $runbookName -Path $scriptPath `
-ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
-Type PowerShellWorkflow
```

## <a name="testing-a-runbook"></a>Pruebas de un runbook

Cuando se prueba un runbook, se ejecuta la [versión de borrador](#publishing-a-runbook) y se completan todas las acciones que realiza. No se crea ningún historial de trabajos, pero los flujos [Salida](automation-runbook-output-and-messages.md#output-stream) y [Advertencia y error](automation-runbook-output-and-messages.md#message-streams) se muestran en el panel de salida de la prueba. Los mensajes del [flujo detallado](automation-runbook-output-and-messages.md#message-streams) solo se muestran en el panel de salida si la variable [VerbosePreference](automation-runbook-output-and-messages.md#preference-variables) está establecida en `Continue`.

Aun cuando se ejecuta la versión de borrador, el runbook se ejecuta con normalidad y realiza las acciones correspondientes en los recursos del entorno. Por este motivo, solo debe probar runbooks en recursos no pertenecientes a entornos de producción.

El procedimiento para probar cada [tipo de runbook](automation-runbook-types.md) es el mismo. No hay diferencias entre realizar las pruebas en el editor de texto o en el editor gráfico de Azure Portal.

1. Abra la versión de borrador del runbook en el [editor de texto](automation-edit-textual-runbook.md) o el [editor gráfico](automation-graphical-authoring-intro.md).
1. Haga clic en el botón **Probar** para abrir la página Prueba.
1. Si el runbook tiene parámetros, se enumeran en el panel izquierdo, donde puede proporcionar los valores que se usarán para la prueba.
1. Si quiere ejecutar la prueba en una instancia de [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md), cambie los **Parámetros de ejecución** a **Hybrid Worker** y seleccione el nombre del grupo de destino.  De lo contrario, mantenga el valor predeterminado **Azure** para ejecutar la prueba en la nube.
1. Haga clic en el botón **Iniciar** para comenzar la prueba.
1. Puede usar los botones en el panel de salida para detener o suspender un runbook de [flujo de trabajo de PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) o [gráfico](automation-runbook-types.md#graphical-runbooks) mientras se está probando. Cuando se suspende el runbook, este completa la actividad que está realizando en ese momento antes de suspenderse. Una vez suspendido, puede detener o reiniciar el runbook.
1. Inspeccione la salida del runbook en el panel de salida.

## <a name="publishing-a-runbook"></a>Publicación de un runbook

Cuando cree o importe un runbook nuevo, debe publicarlo para poder ejecutarlo. Cada runbook de Azure Automation tiene una versión de borrador y una versión publicada. Solo es posible ejecutar la versión publicada y solo es posible editar la versión de borrador. Los cambios realizados en la versión de borrador no afectan la versión publicada. Cuando la versión de borrador deba estar lista para su disponibilidad, puede publicarla, lo que sobrescribirá la versión publicada actual con la versión de borrador.

### <a name="publish-a-runbook-in-the-azure-portal"></a>Publicación de un runbook en Azure Portal

1. Abra el runbook en Azure Portal.
2. Haga clic en **Editar**.
3. Haga clic en el botón **Publicar** y, a continuación, en **Sí** en el mensaje de comprobación.

### <a name="publish-a-runbook-using-powershell"></a>Publicación de un runbook mediante PowerShell

Utilice el cmdlet [Publish-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/Az.Automation/Publish-AzAutomationRunbook?view=azps-3.5.0) para publicar un runbook con Windows PowerShell. En el ejemplo siguiente se muestra cómo publicar un runbook de muestra.

```azurepowershell-interactive
$automationAccountName =  "AutomationAccount"
$runbookName = "Sample_TestRunbook"
$RGName = "ResourceGroup"

Publish-AzAutomationRunbook -AutomationAccountName $automationAccountName `
-Name $runbookName -ResourceGroupName $RGName
```

## <a name="scheduling-a-runbook-in-the-azure-portal"></a>Programación de un runbook en Azure Portal

Una vez publicado el runbook, puede programarlo para que funcione.

1. Abra el runbook en Azure Portal.
2. Seleccione **Programas** en **Recursos**.
3. Seleccione **Agregar una programación**.
4. En el panel Programación de un runbook, seleccione **Vincular una programación a su runbook**.
5. En el panel Programación, seleccione **Crear una nueva programación**.
6. Escriba un nombre, una descripción y otros parámetros en el panel Nueva programación. 
7. Una vez creada la programación, resáltela y haga clic en **Aceptar**. Ahora debería estar vinculada a su runbook.
8. Busque un correo electrónico en el buzón que le informa sobre el estado del runbook.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información acerca de cómo puede beneficiarse de la Galería de runbooks y módulos de PowerShell, consulte [Galerías de runbooks y módulos para Azure Automation](automation-runbook-gallery.md).
* Para obtener más información acerca de la edición de runbooks de PowerShell y flujo de trabajo de PowerShell, consulte [Edición de runbooks de texto en Azure Automation](automation-edit-textual-runbook.md).
* Para obtener más información sobre la creación de runbooks gráficos, consulte [Creación gráfica en Azure Automation](automation-graphical-authoring-intro.md).
