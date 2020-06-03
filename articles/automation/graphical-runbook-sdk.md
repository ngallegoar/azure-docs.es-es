---
title: Uso del SDK de runbooks gráficos de Azure Automation (versión preliminar)
description: En este artículo se describe cómo usar el SDK de runbooks gráficos de Azure Automation. (versión preliminar).
services: automation
ms.subservice: process-automation
ms.date: 07/20/2018
ms.topic: conceptual
ms.openlocfilehash: 969e60cd08a65adb1dd731aa7c6c3f9872e288fd
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83835043"
---
# <a name="use-the-azure-automation-graphical-runbook-sdk-preview"></a>Uso del SDK de runbooks gráficos de Azure Automation (versión preliminar)

Los [runbooks gráficos](automation-graphical-authoring-intro.md) ayudan a administrar las complejidades del código subyacente de Windows PowerShell o del flujo de trabajo de PowerShell. El SDK de creación de gráficos en Microsoft Azure Automation permite a los desarrolladores crear y editar runbooks gráficos para usarlos con Azure Automation. En este artículo se describen los pasos básicos para crear un runbook gráfico a partir del código.

## <a name="prerequisites"></a>Prerrequisitos

Importación del paquete `Orchestrator.GraphRunbook.Model.dll` descargando el [SDK](https://www.microsoft.com/download/details.aspx?id=50734).

## <a name="create-a-runbook-object-instance"></a>Creación de una instancia de objeto de runbook

Haga referencia al ensamblado `Orchestrator.GraphRunbook.Model` y cree una instancia de la clase `Orchestrator.GraphRunbook.Model.GraphRunbook`:

```csharp
using Orchestrator.GraphRunbook.Model;
using Orchestrator.GraphRunbook.Model.ExecutableView;

var runbook = new GraphRunbook();
```

## <a name="add-runbook-parameters"></a>Adición de parámetros de runbook

Cree una instancia de objetos `Orchestrator.GraphRunbook.Model.Parameter` y agréguelos al runbook:

```csharp
runbook.AddParameter(
 new Parameter("YourName") {
  TypeName = typeof(string).FullName,
   DefaultValue = "World",
   Optional = true
 });

runbook.AddParameter(
 new Parameter("AnotherParameter") {
  TypeName = typeof(int).FullName, ...
 });
```

## <a name="add-activities-and-links"></a>Adición de actividades y vínculos

Cree una instancia de actividades de tipos apropiados y agréguelas al runbook:

```csharp
var writeOutputActivityType = new CommandActivityType {
 CommandName = "Write-Output",
  ModuleName = "Microsoft.PowerShell.Utility",
 InputParameterSets = new [] {
  new ParameterSet {
   Name = "Default",
    new [] {
     new Parameter("InputObject"), ...
    }
  },
  ...
 }
};

var outputName = runbook.AddActivity(
 new CommandActivity("Output name", writeOutputActivityType) {
  ParameterSetName = "Default",
   Parameters = new ActivityParameters {
    {
     "InputObject",
     new RunbookParameterValueDescriptor("YourName")
    }
   },
   PositionX = 0,
   PositionY = 0
 });

var initializeRunbookVariable = runbook.AddActivity(
 new WorkflowScriptActivity("Initialize variable") {
  Process = "$a = 0",
   PositionX = 0,
   PositionY = 100
 });
```

Las actividades las implementan las clases siguientes en el espacio de nombres `Orchestrator.GraphRunbook.Model`.

|Clase  |Actividad  |
|---------|---------|
|CommandActivity     | Invoca un comando de PowerShell (cmdlet, función, etc.).        |
|InvokeRunbookActivity     | Invoca otro runbook alineado.        |
|JunctionActivity     | Espera a que todas las ramas entrantes finalicen.        |
|WorkflowScriptActivity     | Ejecuta un bloque de código de PowerShell o del flujo de trabajo de PowerShell (según el tipo de runbook) en el contexto del runbook. Se trata de una herramienta eficaz, pero no se debe abusar de su uso: la interfaz de usuario mostrará este bloque de script como texto; el motor de ejecución tratará el bloque proporcionado como una caja negra y no intentará analizar su contenido, excepto para una comprobación de sintaxis básica. Si desea invocar un único comando de PowerShell, se prefiere CommandActivity.        |

> [!NOTE]
> No derive sus propias actividades de las clases proporcionadas. Azure Automation no puede usar runbooks con tipos de actividades personalizadas.

Debe proporcionar los parámetros `CommandActivity` y `InvokeRunbookActivity` como descriptores de valor, y no como valores directos. Los descriptores de valor especifican cómo se deben crear los valores de parámetros reales. Actualmente se proporcionan los siguientes descriptores de valor:


|Descriptor  |Definición  |
|---------|---------|
|ConstantValueDescriptor     | Hace referencia a un valor constante codificado de forma rígida.        |
|RunbookParameterValueDescriptor     | Hace referencia a un parámetro de runbook por nombre.        |
|ActivityOutputValueDescriptor     | Hace referencia a una salida de actividad ascendente, lo que permite que una actividad "se suscriba" a los datos creados por otra actividad.        |
|AutomationVariableValueDescriptor     | Hace referencia a un recurso de variable de Automation por nombre.         |
|AutomationCredentialValueDescriptor     | Hace referencia a un recurso de certificado de Automation por nombre.        |
|AutomationConnectionValueDescriptor     | Hace referencia a un recurso de conexión de Automation por nombre.        |
|PowerShellExpressionValueDescriptor     | Especifica una expresión de PowerShell de forma libre que se va a evaluar justo antes de invocar la actividad.  <br/>Se trata de una herramienta eficaz, pero no se debe abusar de su uso: la interfaz de usuario mostrará esta expresión como texto; el motor de ejecución tratará el bloque proporcionado como una caja negra y no intentará analizar su contenido, excepto para una comprobación de sintaxis básica. Cuando sea posible, se prefieren descriptores de valor más específicos.      |

> [!NOTE]
> No derive sus propios descriptores de valores de las clases proporcionadas. Azure Automation no puede usar runbooks con tipos de descriptores de valores personalizados.

Cree instancias de los vínculos que conectan las actividades y agréguelos al runbook:

```csharp
runbook.AddLink(new Link(activityA, activityB, LinkType.Sequence));

runbook.AddLink(
 new Link(activityB, activityC, LinkType.Pipeline) {
  Condition = string.Format("$ActivityOutput['{0}'] -gt 0", activityB.Name)
 });
```

## <a name="save-the-runbook-to-a-file"></a>Guardar el runbook en un archivo

Use `Orchestrator.GraphRunbook.Model.Serialization.RunbookSerializer` para serializar un runbook en una cadena:

```csharp
var serialized = RunbookSerializer.Serialize(runbook);
```

Puede guardar esta cadena en un archivo con la extensión **.graphrunbook**. El runbook correspondiente se puede importar en Azure Automation.
El formato serializado puede cambiar en las futuras versiones de `Orchestrator.GraphRunbook.Model.dll`. Prometemos compatibilidad con versiones anteriores: cualquier runbook serializado con una versión anterior de `Orchestrator.GraphRunbook.Model.dll` se puede deserializar en cualquier versión más reciente. No se garantiza la compatibilidad con versiones posteriores: puede que un runbook serializado con una versión más reciente no se deserialice en versiones anteriores.

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte [Control de errores en runbooks gráficos de Azure Automation](automation-graphical-authoring-intro.md).