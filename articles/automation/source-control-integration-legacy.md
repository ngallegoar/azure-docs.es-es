---
title: 'Uso de la integración del control de código fuente en Azure Automation: heredado'
description: En este artículo se explica cómo usar la integración del control de código fuente.
services: automation
ms.subservice: process-automation
ms.date: 12/04/2019
ms.topic: conceptual
ms.openlocfilehash: f1e4e288b5b95f355221188a45f1e6c764fde77c
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86187343"
---
# <a name="use-source-control-integration-in-azure-automation---legacy"></a>Uso de la integración del control de código fuente en Azure Automation: heredado

> [!NOTE]
> Hay una nueva experiencia de control de código fuente. Para más información acerca de la nueva experiencia, consulte [Control de código fuente (versión preliminar)](source-control-integration.md).

La integración del control de código fuente permite asociar runbooks de su cuenta de Automation a un repositorio de control de código fuente de GitHub. El control de código fuente le permite colaborar fácilmente con su equipo, realizar el seguimiento de los cambios y revertir a versiones anteriores de los runbooks. Por ejemplo, le permite sincronizar distintas ramas de control de código fuente con sus cuentas de Automation de desarrollo, prueba o producción, lo que facilita la promoción de código que se ha probado en el entorno de desarrollo a la cuenta de Automation de producción.

El control de código fuente le permite insertar código de Azure Automation en el control de código fuente o extraer sus runbooks del control de código fuente para llevarlos a Azure Automation. En este artículo se describe cómo configurar el control de código fuente en su entorno de Azure Automation. Para comenzar, vamos a configurar Azure Automation para acceder al repositorio de GitHub y a recorrer diferentes operaciones que pueden realizarse mediante la integración del control de código fuente. 

> [!NOTE]
> El control de código fuente admite la extracción e inserción de [runbooks del flujo de trabajo de PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) y de [runbooks de PowerShell](automation-runbook-types.md#powershell-runbooks). Los [runbooks gráficos](automation-runbook-types.md#graphical-runbooks) aún no se admiten.

## <a name="configure-source-control"></a>Configurar el control de código fuente

Hay dos pasos sencillos necesarios para configurar el control de código fuente para su cuenta de Automation y solo uno si ya tiene una cuenta GitHub. 

### <a name="create-a-github-repository"></a>Crear un repositorio de GitHub

Si ya tiene una cuenta de GitHub y un repositorio que desea vincular a Azure Automation, inicie sesión en la cuenta y comience desde el paso 2. De lo contrario, vaya a [GitHub](https://github.com/), suscríbase a una cuenta nueva y [cree un nuevo repositorio](https://help.github.com/articles/create-a-repo/).

### <a name="set-up-source-control"></a>Configurar el control de código fuente

1. En la página de la cuenta de Automation de Azure Portal, en **Configuración de cuenta**, haga clic en **Control de código fuente**.

2. Se abre la página Control de código fuente, donde puede configurar los detalles de la cuenta de GitHub. A continuación se muestra la lista de parámetros de configuración:  

   | **Parámetro** | **Descripción** |
   |:--- |:--- |
   | Elegir origen |Seleccione el origen. Actualmente, solo se admite **GitHub** . |
   | Authorization |Haga clic en el botón **Autorizar** para conceder acceso a Azure Automation al repositorio de GitHub. Si ya inició sesión con su cuenta de GitHub en una ventana diferente, se usan las credenciales de dicha cuenta. Cuando la autorización es correcta, en la página se muestra su nombre de usuario de GitHub en **Authorization Property** (Propiedad de autorización). |
   | Selección del repositorio |Seleccione un repositorio de GitHub en la lista de repositorios disponibles. |
   | Elegir rama |Seleccione una rama en la lista de ramas disponibles. Solo se muestra la rama **principal** si no creó ninguna rama. |
   | Ruta de acceso de la carpeta de runbook |La ruta de acceso de la carpeta de runbook especifica la ruta de acceso en el repositorio de GitHub desde el que desea insertar o extraer el código. Debe especificarse en formato **/nombreCarpeta/nombreDeSubcarpeta**. Solo se pueden sincronizar los runbooks de la ruta de acceso de la carpeta de runbook con la cuenta de Automation. Los runbooks de las subcarpetas de la ruta de acceso de la carpeta de runbook **no** se sincronizarán. Use **/** para sincronizar todos los runbooks en el repositorio. |
3. Por ejemplo, si tiene un repositorio denominado **scriptsDePowerShell** que contiene una carpeta denominada **carpetaRaíz**, que a su vez contiene una carpeta denominada **subCarpeta**. Puede usar las siguientes cadenas para cada nivel de la carpeta de sincronización:

   1. Para sincronizar runbooks desde el **repositorio**, la ruta de acceso de la carpeta de runbook es **/** .
   2. Para sincronizar runbooks desde la **carpetaRaíz**, la ruta de acceso de la carpeta de runbook es **/carpetaRaíz**
   3. Para sincronizar runbooks desde la **subCarpeta**, la ruta de acceso de la carpeta de runbook es **/carpetaRaíz/subCarpeta**.
4. Después de configurarlos, los parámetros se muestran en la página Configurar control de código fuente.  

    ![La página de control de código fuente que muestra la configuración](media/source-control-integration-legacy/automation-SourceControlConfigure.png)
5. Al hacer clic en **Aceptar**, la integración del control de código fuente estará configurada para la cuenta de Automation y debe actualizarse con la información de GitHub. Ahora puede hacer clic en este elemento para ver todo el historial de trabajos de sincronización del control de código fuente.  

    ![Valores para la configuración del control de código fuente configurado actual](media/source-control-integration-legacy/automation-RepoValues.png)
6. Después de configurar el control de código fuente, se crean dos [recursos de variables](./shared-resources/variables.md) en la cuenta de Automation. Además, una aplicación autorizada se agrega a la cuenta de GitHub.

   * La variable **Microsoft.Azure.Automation.SourceControl.Connection** contiene los valores de la cadena de conexión, tal como se muestra a continuación.  

     | **Parámetro** | **Valor** |
     |:--- |:--- |
     | `Name`  |Microsoft.Azure.Automation.SourceControl.Connection |
     | `Type`  |String |
     | `Value` |{"Branch":\<*Your branch name*>,"RunbookFolderPath":\<*Runbook folder path*>,"ProviderType":\<*has a value 1 for GitHub*>,"Repository":\<*Name of your repository*>,"Username":\<*Your GitHub user name*>} |

   * La variable **Microsoft.Azure.Automation.SourceControl.OAuthToken**contiene el valor cifrado seguro de OAuthToken.  

     |**Parámetro**            |**Valor** |
     |:---|:---|
     | `Name`  | `Microsoft.Azure.Automation.SourceControl.OAuthToken` |
     | `Type`  | `Unknown(Encrypted)` |
     | `Value` | <`Encrypted OAuthToken`> |  

     ![Una ventana que muestra las variables de control de código fuente.](media/source-control-integration-legacy/automation-Variables.png)  

   * **control de código fuente de Automation** como una aplicación autorizada a su cuenta de GitHub. Para ver la aplicación, desde la página principal de GitHub, vaya a **perfil** > **Configuración** > **Aplicaciones**. Esta aplicación permite que Azure Automation sincronice el repositorio de GitHub con una cuenta de Automation.  

     ![Configuración de la aplicación en GitHub](media/source-control-integration-legacy/automation-GitApplication.png)

## <a name="use-source-control-in-automation"></a>Uso del control de código fuente en Automation

La inserción de runbooks en el repositorio permite insertar los cambios realizados en un runbook de Azure Automation en el repositorio de control de código fuente. A continuación se muestran los pasos necesarios para insertar un runbook en el repositorio:

1. Desde su cuenta de Automation, [cree un nuevo runbook textual](./learn/automation-tutorial-runbook-textual.md) o [edite un runbook textual existente](automation-edit-textual-runbook.md). Este runbook puede ser un flujo de trabajo de PowerShell o un runbook de scripts de PowerShell.  
2. Después de editar el runbook, guárdelo y haga clic en **Insertar en el repositorio** en la página Editar.  

    ![Una ventana que muestra el botón Check-in to GitHub (Inserción en el repositorio en GitHub)](media/source-control-integration-legacy/automation-CheckinButton.png)

     > [!NOTE] 
     > Con la inserción en el repositorio de Azure Automation se sobrescribe el código existente en el control de código fuente. La instrucción de la línea de comandos de Git equivalente para la inserción en el repositorio es **git add + git commit + git push**  

3. Al hacer clic en **Insertar en el repositorio**, se muestra un mensaje de confirmación. Haga clic en **Sí** para continuar.  

    ![Un cuadro de diálogo que confirma la inserción en el repositorio para el control de código fuente](media/source-control-integration-legacy/automation-CheckinMessage.png)
4. La protección se inicia en el runbook de control de código fuente: **Sync-MicrosoftAzureAutomationAccountToGitHubV1**. Este runbook se conecta a GitHub y aplica los cambios realizados de Azure Automation al repositorio. Para ver el historial de trabajos de inserción en el repositorio, vuelva a la pestaña **Integración del control de código fuente** y haga clic para abrir la página Sincronización de repositorios. Esta página muestra todos los trabajos de control de código fuente. Seleccione el trabajo que quiere ver y haga clic para ver los detalles.  

    ![Una ventana que muestra los resultados de un trabajo de sincronización](media/source-control-integration-legacy/automation-CheckinRunbook.png)

   > [!NOTE]
   > Los runbooks de control de código fuente son runbooks de Automation especiales que no se pueden ver ni editar. Aunque no se muestran en la lista de runbooks, verá que se muestran los trabajos de sincronización en la lista de trabajos.

5. El nombre del runbook modificado se envía como parámetro de entrada al runbook de inserción en el repositorio. También puede [ver los detalles del trabajo](automation-runbook-execution.md#job-statuses) si expande el runbook en la página Sincronización de repositorios.  

    ![Una ventana que muestra la entrada de un trabajo de sincronización](media/source-control-integration-legacy/automation-CheckinInput.png)
6. Actualice el repositorio de GitHub cuando finalice el trabajo para ver los cambios.  Debe haber una confirmación en el repositorio con un mensaje de confirmación: ***Nombre de runbook* actualizado en Azure Automation**.  

### <a name="sync-runbooks-from-source-control-to-azure-automation"></a>Sincronización de runbooks de control de código fuente a Azure Automation

El botón de sincronización que se encuentra en la página Sincronización de repositorios permite extraer todos los runbooks desde la ruta de acceso de la carpeta de runbooks del repositorio y llevarlos a la cuenta de Automation. El mismo repositorio puede sincronizarse con más de una cuenta de Automation. A continuación se muestran los pasos necesarios para sincronizar un runbook:

1. En la cuenta de Automation donde se configuró el control de código fuente, abra la página Integración del control de código fuente/Sincronización de repositorios y haga clic en **Sincronizar**.  En el mensaje de confirmación que se muestra, haga clic en **Sí** para continuar.  

    ![Botón de sincronización con un mensaje que confirma que se sincronizarán todos los runbooks](media/source-control-integration-legacy/automation-SyncButtonwithMessage.png)

2. La sincronización inicia el runbook **Sync-MicrosoftAzureAutomationAccountFromGitHubV1**, que se conecta a GitHub y extrae los cambios del repositorio a Azure Automation. Debería ver un nuevo trabajo en la página Sincronización de repositorios para esta acción. Para ver detalles sobre el trabajo de sincronización, haga clic para abrir la página de detalles del trabajo.  

    ![Una ventana que muestra los resultados de un trabajo de sincronización en un repositorio de GitHub](media/source-control-integration-legacy/automation-SyncRunbook.png)

    > [!NOTE]
    > Una sincronización del control de código fuente sobrescribe la versión de borrador de los runbooks que existen actualmente en la cuenta de Automation para **TODOS** los runbooks que están actualmente en el control del código fuente. La instrucción de la línea de comandos de Git equivalente para realizar la sincronización es **git pull**

![Una ventana que muestra todos los registros a partir de un trabajo de sincronización de control de código fuente suspendido](media/source-control-integration-legacy/automation-AllLogs.png)

## <a name="disconnect-source-control"></a>Desconexión del control de código fuente

Para desconectarse de la cuenta de GitHub, abra la página Sincronización de repositorios y haga clic en **Desconectar**. Cuando desconecte el control de código fuente, los runbooks que se sincronizaron seguirán estando en su cuenta de Automation, pero no se habilitará la página Sincronización de repositorios.  

  ![Una ventana que muestra el botón de desconexión para desconectar el control de código fuente](media/source-control-integration-legacy/automation-Disconnect.png)

## <a name="next-steps"></a>Pasos siguientes

* Para integrar el control de código fuente en Azure Automation, vea [Azure Automation: integración del control de código fuente en Azure Automation](https://azure.microsoft.com/blog/azure-automation-source-control-13/).  
* Para integrar el control de código fuente de runbook con Visual Studio Online, vea [Azure Automation: integración del control de código fuente de runbook mediante Visual Studio Online](https://azure.microsoft.com/blog/azure-automation-integrating-runbook-source-control-using-visual-studio-online/).  
