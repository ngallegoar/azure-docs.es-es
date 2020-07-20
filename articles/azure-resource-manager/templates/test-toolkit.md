---
title: Kit de herramientas para pruebas de plantillas de ARM
description: Describe cómo ejecutar el kit de herramientas para pruebas de la plantilla de ARM en su plantilla. El kit de herramientas le permite ver si ha implementado los procedimientos recomendados.
ms.topic: conceptual
ms.date: 06/19/2020
ms.author: tomfitz
author: tfitzmac
ms.openlocfilehash: 7b88096dfdd1c7fb3e2671a369132e75a8885b8d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85255835"
---
# <a name="use-arm-template-test-toolkit"></a>Uso del kit de herramientas para pruebas de plantillas de ARM

El [kit de herramientas para pruebas de plantillas de ARM](https://aka.ms/arm-ttk) comprueba si la plantilla usa los procedimientos recomendados. Cuando la plantilla no es compatible con los procedimientos recomendados, devuelve una lista de advertencias con los cambios sugeridos. Con el kit de herramientas para pruebas, puede obtener información sobre cómo evitar los problemas comunes en el desarrollo de plantillas.

El kit de herramientas para pruebas proporciona un [conjunto de pruebas predeterminadas](test-cases.md). Estas pruebas son recomendaciones, pero no requisitos. Puede decidir qué pruebas son adecuadas para sus objetivos y personalizar qué pruebas se ejecutan.

En este artículo se describe cómo ejecutar el kit de herramientas para pruebas y cómo agregar o eliminar pruebas. Para obtener descripciones de las pruebas predeterminadas, consulte [Casos de prueba del kit de herramientas](test-cases.md).

El kit de herramientas es un conjunto de scripts de PowerShell que se puede ejecutar desde un comando de PowerShell o la CLI.

## <a name="download-test-toolkit"></a>Descarga del kit de herramientas para pruebas

Para usar el kit de herramientas para pruebas, puede bifurcar y clonar el [repositorio](https://aka.ms/arm-ttk) que contiene los scripts o [descargar el archivo ZIP más reciente](https://aka.ms/arm-ttk-latest).

En función de la directiva de ejecución del equipo en el que se ejecuta el script, es posible que reciba un error sobre la ejecución de scripts desde Internet. Debe cambiar la [directiva de ejecución](/powershell/module/microsoft.powershell.core/about/about_execution_policies) o [desbloquear los archivos del script](/powershell/module/microsoft.powershell.utility/unblock-file).

## <a name="run-on-powershell"></a>Ejecución en PowerShell

Antes de ejecutar las pruebas, importe el módulo.

```powershell
Import-Module .\arm-ttk.psd1 # from the same directory as .\arm-ttk.psd1
```

Para ejecutar las pruebas en **PowerShell**, use el comando siguiente:

```powershell
Test-AzTemplate -TemplatePath $TemplateFolder
```

## <a name="run-on-linux"></a>Ejecución en Linux

Antes de ejecutar las pruebas, instale [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-linux).

Para ejecutar las pruebas en Bash en **Linux**, use el comando siguiente:

```bash
Test-AzTemplate.sh -TemplatePath $TemplateFolder
```

También puede ejecutar la prueba en pwsh.exe.

## <a name="run-on-macos"></a>Ejecución en macOS

Antes de ejecutar las pruebas, instale [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-macos). 

Instale `coreutils`:

```bash
brew install coreutils
```

Para ejecutar las pruebas en **macOS**, use el comando siguiente:

```bash
Test-AzTemplate.sh -TemplatePath $TemplateFolder
```

## <a name="result-format"></a>Formato de resultado

Las pruebas que se superan se muestran en color **verde** y precedidas de **[+]** .

Las pruebas con errores se muestran en color **rojo** y precedidas de **[-]** .

:::image type="content" source="./media/template-test-toolkit/view-results.png" alt-text="Ver resultados de la prueba":::

Los resultados de texto son:

```powershell
[+] adminUsername Should Not Be A Literal (24 ms)
[+] apiVersions Should Be Recent (18 ms)
[+] artifacts parameter (16 ms)
[+] DeploymentTemplate Schema Is Correct (17 ms)
[+] IDs Should Be Derived From ResourceIDs (15 ms)
[-] Location Should Not Be Hardcoded (41 ms)
     azuredeploy.json must use the location parameter, not resourceGroup().location (except when used as a default value in the main template)
```

## <a name="test-parameters"></a>Parámetros de prueba

Cuando se proporciona el parámetro **-TemplatePath**, el kit de herramientas busca en esa carpeta una plantilla con el nombre azuredeploy.json o maintemplate.json. Primero se prueba esta plantilla y, a continuación, se prueban todas las demás plantillas de la carpeta y sus subcarpetas. Las otras plantillas se prueban como plantillas vinculadas. Si la ruta de acceso incluye un archivo llamado [CreateUiDefinition.json](../managed-applications/create-uidefinition-overview.md), se ejecutan las pruebas que corresponden a la definición de la interfaz de usuario.

```powershell
Test-AzTemplate -TemplatePath $TemplateFolder
```

Para probar un archivo de esa carpeta, agregue el parámetro **-File**. Sin embargo, la carpeta debe tener una plantilla principal llamada azuredeploy.json o maintemplate.json.

```powershell
Test-AzTemplate -TemplatePath $TemplateFolder -File cdn.json
```

De forma predeterminada, se ejecutan todas las pruebas. Para especificar las pruebas individuales que se van a ejecutar, use el parámetro **-Test**. Proporcione el nombre de la prueba. Para obtener los nombres, consulte [Casos de prueba para el kit de herramientas](test-cases.md).

```powershell
Test-AzTemplate -TemplatePath $TemplateFolder -Test "Resources Should Have Location"
```

## <a name="customize-tests"></a>Personalización de las pruebas

En el caso de las plantillas de ARM, el kit de herramientas ejecuta todas las pruebas de la carpeta **\arm-ttk\testcases\deploymentTemplate**. Si desea eliminar permanentemente una prueba, elimine el archivo correspondiente de la carpeta.

En el caso de los archivos [CreateUiDefinition](../managed-applications/create-uidefinition-overview.md), se ejecutan todas las pruebas de la carpeta **\arm-ttk\testcases\CreateUiDefinition**.

Para agregar su propia prueba, cree un archivo con la convención de nomenclatura: **Your-Custom-Test-Name.test.ps1**.

La prueba puede obtener la plantilla como un parámetro de objeto o un parámetro de cadena. Normalmente se usa uno u otro, pero se pueden usar ambos.

Use el parámetro de objeto cuando necesite obtener una sección de la plantilla y recorrer en iteración sus propiedades. Una prueba que usa el parámetro de objeto tiene el siguiente formato:

```powershell
param(
    [Parameter(Mandatory=$true,Position=0)]
    [PSObject]
    $TemplateObject
)

# Implement test logic that evaluates parts of the template.
# Output error with: Write-Error -Message
```

El objeto de la plantilla tiene las siguientes propiedades:

* $schema
* contentVersion
* parámetros
* variables
* resources
* outputs

Por ejemplo, puede obtener la colección de parámetros con `$TemplateObject.parameters`.

Use el parámetro de cadena cuando necesite realizar una operación de cadena en toda la plantilla. Una prueba que usa el parámetro de cadena tiene el siguiente formato:

```powershell
param(
    [Parameter(Mandatory)]
    [string]
    $TemplateText
)

# Implement test logic that performs string operations.
# Output error with: Write-Error -Message
```

Por ejemplo, puede ejecutar una expresión regular del parámetro de cadena para ver si se utiliza una sintaxis específica.

Para más información sobre la implementación de la prueba, examine las otras pruebas de esa carpeta.

## <a name="integrate-with-azure-pipelines"></a>Integración con Azure Pipelines

Puede agregar el kit de herramientas para pruebas a su canalización de Azure. Con una canalización, puede ejecutar la prueba cada vez que se actualiza la plantilla o ejecutarla como parte del proceso de implementación.

La forma más fácil de agregar el kit de herramientas para pruebas a la canalización es con extensiones de terceros. Están disponibles las dos extensiones siguientes:

* [Ejecutar pruebas de ARM TTK](https://marketplace.visualstudio.com/items?itemName=Sam-Cogan.ARMTTKExtension)
* [Comprobación de plantillas de ARM](https://marketplace.visualstudio.com/items?itemName=maikvandergaag.maikvandergaag-arm-ttk)

O bien, puede implementar sus propias tareas. En el ejemplo siguiente se muestra cómo descargar el kit de herramientas para pruebas.

```json
{
    "environment": {},
    "enabled": true,
    "continueOnError": false,
    "alwaysRun": false,
    "displayName": "Download TTK",
    "timeoutInMinutes": 0,
    "condition": "succeeded()",
    "task": {
        "id": "e213ff0f-5d5c-4791-802d-52ea3e7be1f1",
        "versionSpec": "2.*",
        "definitionType": "task"
    },
    "inputs": {
        "targetType": "inline",
        "filePath": "",
        "arguments": "",
        "script": "New-Item '$(ttk.folder)' -ItemType Directory\nInvoke-WebRequest -uri '$(ttk.uri)' -OutFile \"$(ttk.folder)/$(ttk.asset.filename)\" -Verbose\nGet-ChildItem '$(ttk.folder)' -Recurse\n\nWrite-Host \"Expanding files...\"\nExpand-Archive -Path '$(ttk.folder)/*.zip' -DestinationPath '$(ttk.folder)' -Verbose\n\nWrite-Host \"Expanded files found:\"\nGet-ChildItem '$(ttk.folder)' -Recurse",
        "errorActionPreference": "stop",
        "failOnStderr": "false",
        "ignoreLASTEXITCODE": "false",
        "pwsh": "true",
        "workingDirectory": ""
    }
}
```

En el ejemplo siguiente se muestra cómo ejecutar las pruebas.

```json
{
    "environment": {},
    "enabled": true,
    "continueOnError": true,
    "alwaysRun": false,
    "displayName": "Run Best Practices Tests",
    "timeoutInMinutes": 0,
    "condition": "succeeded()",
    "task": {
        "id": "e213ff0f-5d5c-4791-802d-52ea3e7be1f1",
        "versionSpec": "2.*",
        "definitionType": "task"
    },
    "inputs": {
        "targetType": "inline",
        "filePath": "",
        "arguments": "",
        "script": "Import-Module $(ttk.folder)/arm-ttk/arm-ttk.psd1 -Verbose\n$testOutput = @(Test-AzTemplate -TemplatePath \"$(sample.folder)\")\n$testOutput\n\nif ($testOutput | ? {$_.Errors }) {\n   exit 1 \n} else {\n    Write-Host \"##vso[task.setvariable variable=result.best.practice]$true\"\n    exit 0\n} \n",
        "errorActionPreference": "continue",
        "failOnStderr": "true",
        "ignoreLASTEXITCODE": "false",
        "pwsh": "true",
        "workingDirectory": ""
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre las pruebas predeterminadas, consulte [Casos de prueba para el kit de herramientas](test-cases.md).
