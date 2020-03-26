---
title: 'Tutorial: Configuración de la extensión de Visual Studio Code para Azure Terraform'
description: Obtenga información sobre cómo instalar y usar la extensión de Azure Terraform en Visual Studio Code.
ms.topic: tutorial
ms.date: 10/26/2019
ms.openlocfilehash: fcb47024fd26f061ca4475b01d00f1ae13303b61
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "77472288"
---
# <a name="tutorial-configure-the-azure-terraform-visual-studio-code-extension"></a>Tutorial: Configuración de la extensión de Visual Studio Code para Azure Terraform

La extensión de Visual Studio Code para Azure Terraform permite trabajar con Terraform desde el editor. Con esta extensión, puede crear, probar y ejecutar configuraciones de Terraform. La extensión también admite la visualización de gráficos de recursos.

En este artículo aprenderá a:
> [!div class="checklist"]
> * Automatizar el aprovisionamiento de servicios de Azure mediante Terraform.
> * Instalar y usar la extensión de Visual Studio Code para Terraform para servicios de Azure.
> * Usar Visual Studio Code para escribir, planear y ejecutar planes de Terraform.

## <a name="prerequisites"></a>Prerrequisitos
- **Suscripción de Azure**: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.

- **Terraform**: [Instalación y configuración de Terraform](terraform-install-configure.md).

- **Visual Studio Code**: Instale la versión de [Visual Studio Code](https://code.visualstudio.com/download) adecuada para su entorno.

## <a name="prepare-your-dev-environment"></a>Preparación del entorno de desarrollo

### <a name="install-git"></a>Instalar Git

Para completar los ejercicios del artículo, deberá [instalar Git](https://git-scm.com/).

### <a name="install-hashicorp-terraform"></a>Instalar HashiCorp Terraform

Siga las instrucciones de la página web [Install Terraform](https://www.terraform.io/intro/getting-started/install.html) (Instalar Terraform) de HashiCorp, en las que se incluye:

- Descarga de Terraform
- Instalación de Terraform
- Comprobación de que Terraform está instalado correctamente

>[!Tip]
>Asegúrese de seguir las instrucciones sobre la configuración de la variable del sistema PATH.

### <a name="install-nodejs"></a>Instalación de Node.js

Para usar Terraform en Cloud Shell, deberá [instalar Node.js](https://nodejs.org/) 6.0 o posterior.

>[!NOTE]
>Para comprobar si se ha instalado Node.js, abra una ventana de terminal y escriba `node -v`.

### <a name="install-graphviz"></a>Instalar GraphViz

Para usar la función de visualización de Terraform, deberá [instalar GraphViz](https://graphviz.org/).

>[!NOTE]
>Para comprobar si se ha instalado GraphViz, abra una ventana de terminal y escriba `dot -V`.

### <a name="install-the-azure-terraform-visual-studio-code-extension"></a>Instalación de la extensión de Visual Studio Code para Azure Terraform

1. Inicie Visual Studio Code.

1. Seleccione **Extensiones**.

    ![Botón Extensiones](media/terraform-vscode-extension/tf-vscode-extensions-button.png)

1. Use el cuadro de texto **Search Extensions in Marketplace** (Buscar extensiones en Marketplace) para buscar la extensión de Azure Terraform:

    ![Búsqueda de extensiones de Visual Studio Code en Marketplace](media/terraform-vscode-extension/tf-search-extensions.png)

1. Seleccione **Instalar**.

    >[!NOTE]
    >Al seleccionar **Instalar** para la extensión de Azure Terraform, Visual Studio Code instalará automáticamente la extensión de la cuenta de Azure. La cuenta de Azure es un archivo de dependencia para la extensión de Azure Terraform, que se usa para realizar las autenticaciones de suscripción a Azure y extensiones de código relacionado con Azure.

#### <a name="verify-the-terraform-extension-is-installed-in-visual-studio-code"></a>Compruebe que está instalada la extensión de Terraform en Visual Studio Code

1. Seleccione **Extensiones**.

1. Escriba `@installed` en el cuadro de texto de búsqueda.

    ![Extensiones instaladas](media/terraform-vscode-extension/tf-installed-extensions.png)

La extensión de Azure Terraform aparecerá en la lista de extensiones instaladas.

![Extensiones instaladas de Terraform](media/terraform-vscode-extension/tf-installed-terraform-extension-button.png)

Ahora puede ejecutar todos los comandos admitidos de Terraform en su entorno de Cloud Shell desde dentro de Visual Studio Code.

## <a name="exercise-1-basic-terraform-commands-walk-through"></a>Ejercicio 1: Tutorial de comandos básicos de Terraform

En este ejercicio, se crea y ejecuta un archivo de configuración básica de Terraform que aprovisiona un nuevo grupo de recursos de Azure.

### <a name="prepare-a-test-plan-file"></a>Preparar un archivo de plan de pruebas

1. En Visual Studio Code, seleccione **Archivo > Nuevo archivo** en la barra de menús.

1. En el explorador, vaya a la [página azurerm_resource_group de Terraform](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html#) y copie el código del bloque de código **Example Usage** (Ejemplo de uso):

    ![Ejemplo de uso](media/terraform-vscode-extension/tf-azurerm-resource-group-example-usage.png)

1. Pegue el código copiado en el nuevo archivo que creó en Visual Studio Code.

    ![Pegar el código de ejemplo de uso](media/terraform-vscode-extension/tf-paste-example-usage-code.png)

    >[!NOTE]
    >Puede cambiar el valor de **nombre** del grupo de recursos, pero debe ser único para su suscripción a Azure.

1. En la barra de menús, seleccione **Archivo > Guardar como**.

1. En el cuadro de diálogo **Guardar como**, navegue hasta una ubicación de su elección y, a continuación, seleccione **Nueva carpeta**. (Cambie el nombre de la nueva carpeta por otro que sea más descriptivo que *Nueva carpeta*).

    >[!NOTE]
    >En este ejemplo, la carpeta se denomina TERRAFORM-TEST-PLAN.

1. Asegúrese de que la nueva carpeta este resaltada (seleccionada) y seleccione **Abrir**.

1. En el cuadro de diálogo **Guardar como**, cambie el nombre predeterminado del archivo por *main.tf*.

    ![Guardar como main.tf](media/terraform-vscode-extension/tf-save-as-main.png)

1. Seleccione **Guardar**.
1. En la barra de menús, seleccione **Archivo > Abrir carpeta**. Navegue hasta la nueva carpeta que creó y selecciónela.

### <a name="run-terraform-init-command"></a>Ejecutar el comando *init* de Terraform

1. Inicie Visual Studio Code.

1. En la barra de menús de Visual Studio Code, seleccione **Archivo > Abrir carpeta...** y busque y seleccione su archivo *main.tf*.

    ![Archivo main.tf](media/terraform-vscode-extension/tf-main-tf.png)

1. En la barra de menús, seleccione **Vista > Paleta de comandos... > Azure Terraform: Init**.

1. Cuando aparezca la confirmación, seleccione **Aceptar**.

    ![¿Quiere abrir Cloud Shell?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloud-shell.png)

1. La primera vez que inicie Cloud Shell desde una carpeta nueva, se le pedirá que cree una aplicación web. seleccione **Open**(Abrir).

    ![Primer inicio de Cloud Shell](media/terraform-vscode-extension/tf-first-launch-of-cloud-shell.png)

1. Se abre la página de bienvenida a Azure Cloud Shell. Seleccione Bash o PowerShell.

    ![Bienvenido a Azure Cloud Shell](media/terraform-vscode-extension/tf-welcome-to-azure-cloud-shell.png)

    >[!NOTE]
    >En este ejemplo, se seleccionó Bash (Linux).

1. Si no ha configurado una cuenta de almacenamiento de Azure, aparecerá la pantalla siguiente. Seleccione **Crear almacenamiento**.

    ![No tiene ningún almacenamiento montado](media/terraform-vscode-extension/tf-you-have-no-storage-mounted.png)

1. Azure Cloud Shell se inicia en el shell que seleccionó anteriormente y muestra la información de la unidad en la nube que acaba de crear para usted.

    ![Se ha creado su unidad en la nube.](media/terraform-vscode-extension/tf-your-cloud-drive-has-been-created-in.png)

1. Ahora puede salir de Cloud Shell.

1. En la barra de menús, seleccione **Vista** > **Paleta de comandos** > **Azure Terraform: init**.

    ![Terraform se ha inicializado correctamente.](media/terraform-vscode-extension/tf-terraform-has-been-successfully-initialized.png)

### <a name="visualize-the-plan"></a>Visualizar el plan

Anteriormente en este tutorial, ha instalado GraphViz. Terraform puede usar GraphViz para generar una representación visual de una configuración o plan de ejecución. La extensión de Visual Studio Code para Azure Terraform implementa esta característica mediante el comando *visualize*.

- En la barra de menús, seleccione **Vista > Paleta de comandos > Azure Terraform: Visualize**.

    ![Visualizar el plan](media/terraform-vscode-extension/tf-graph.png)

### <a name="run-terraform-plan-command"></a>Ejecutar el comando *plan* de Terraform

El comando *plan* de Terraform se usa para comprobar si el plan de ejecución de un conjunto de cambios hará lo que está previsto.

>[!NOTE]
>El comando *plan* de Terraform no realiza cambios en los recursos reales de Azure. Para realizar realmente los cambios incluidos en el plan, se usa el comando *apply* de Terraform.

- En la barra de menús, seleccione **Vista** > **Paleta de comandos** > **Azure Terraform: plan**.

    ![Plan de Terraform](media/terraform-vscode-extension/tf-terraform-plan.png)

### <a name="run-terraform-apply-command"></a>Ejecutar el comando *apply* de Terraform

Cuando esté satisfecho con los resultados del comando *plan* de Terraform, puede ejecutar el comando *apply*.

1. En la barra de menús, seleccione **Vista** > **Paleta de comandos** > **Azure Terraform: apply**.

    ![Terraform apply](media/terraform-vscode-extension/tf-terraform-apply.png)

1. Escriba `yes`.

    ![Terraform apply yes](media/terraform-vscode-extension/tf-terraform-apply-yes.png)

### <a name="verify-your-terraform-plan-was-executed"></a>Comprobar que se ejecutó el plan de Terraform

Para ver si el nuevo grupo de recursos de Azure se creó correctamente:

1. Abra Azure Portal.

1. Seleccione **Grupos de recursos** en el panel de navegación de la izquierda.

    ![Comprobar el nuevo recurso](media/terraform-vscode-extension/tf-verify-resource-group-created.png)

El nuevo grupo de recursos debe aparecer en la columna **NOMBRE**.

>[!NOTE]
>Puede dejar abierta la ventana de Azure Portal por ahora; la usaremos en el paso siguiente.

### <a name="run-terraform-destroy-command"></a>Ejecutar el comando *destroy* de Terraform

1. En la barra de menús, seleccione **Vista** > **Paleta de comandos** > **Azure Terraform: destroy**.

    ![Terraform destroy](media/terraform-vscode-extension/tf-terraform-destroy.png)

1. Escriba *yes* (sí).

    ![Terraform destroy yes](media/terraform-vscode-extension/tf-terraform-destroy-yes.png)

### <a name="verify-your-resource-group-was-destroyed"></a>Comprobar que se ha destruido el grupo de recursos

Para confirmar que Terraform ha destruido correctamente el nuevo grupo de recursos:

1. Seleccione **Actualizar** en la página **Grupos de recursos** de Azure Portal.

1. Ya no se mostrará el grupo de recursos.

    ![Comprobar que se ha destruido el grupo de recursos](media/terraform-vscode-extension/tf-refresh-resource-groups-button.png)

## <a name="exercise-2-terraform-compute-module"></a>Ejercicio 2: Módulo *compute* de Terraform

En este ejercicio, obtendrá información sobre cómo cargar el módulo *compute*  de Terraform en el entorno de Visual Studio Code.

### <a name="clone-the-terraform-azurerm-compute-module"></a>Clonar el módulo terraform-azurerm-compute

1. Use [este vínculo](https://github.com/Azure/terraform-azurerm-compute) para obtener acceso al módulo Terraform Azure Rm Compute en GitHub.

1. Seleccione **Clone or download**(Clonar o descargar).

    ![Clonar o descargar](media/terraform-vscode-extension/tf-clone-with-https.png)

    >[!NOTE]
    >En este ejemplo, la carpeta se denomina *terraform-azurerm-compute*.

### <a name="open-the-folder-in-visual-studio-code"></a>Abrir la carpeta en Visual Studio Code

1. Inicie Visual Studio Code.

1. En la barra de menús, seleccione **Archivo > Abrir carpeta**, vaya hasta la carpeta que creó en el paso anterior y selecciónela.

    ![Carpeta terraform-azurerm-compute](media/terraform-vscode-extension/tf-terraform-azurerm-compute-folder.png)

### <a name="initialize-terraform"></a>Inicialización de Terraform

Antes de que pueda empezar a usar los comandos de Terraform desde dentro de Visual Studio Code, descargue los complementos para dos proveedores de Azure: random y azurerm.

1. En el panel Terminal del IDE de Visual Studio Code, escriba `terraform init`.

    ![Comando init de Terraform](media/terraform-vscode-extension/tf-terraform-init-command.png)

1. Escriba `az login`, presione **<Entrar** y siga las instrucciones que aparecen en pantalla.

### <a name="module-test-lint"></a>Prueba del módulo: *lint*

1. En la barra de menús, seleccione **Vista > Paleta de comandos > Azure Terraform: Ejecutar prueba**.

1. En la lista de opciones de tipo de prueba, seleccione **lint**.

    ![Seleccionar el tipo de prueba](media/terraform-vscode-extension/tf-select-type-of-test-lint.png)

1. Cuando aparezca la confirmación, seleccione **Aceptar** y siga las instrucciones que se muestran en pantalla.

    ![¿Quiere abrir CloudShell?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloudshell-small.png)

>[!NOTE]
>Cuando se ejecuta la prueba **lint** o **end to end**, Azure usa un servicio de contenedor para aprovisionar un equipo de prueba para llevar a cabo la prueba real. Por este motivo, los resultados de la prueba normalmente pueden tardar varios minutos en devolverse.

Transcurridos unos instantes, verá una lista en el panel Terminal similar a este ejemplo:

![Resultados de la prueba Lint](media/terraform-vscode-extension/tf-lint-test-results.png)

### <a name="test-the-module"></a>Prueba del módulo

1. En la barra de menús, seleccione **Vista > Paleta de comandos > Azure Terraform: Ejecutar prueba**.

1. En la lista de opciones de tipo de prueba, seleccione **end to end**.

    ![Seleccionar el tipo de prueba](media/terraform-vscode-extension/tf-select-type-of-test-end-to-end.png)

1. Cuando aparezca la confirmación, seleccione **Aceptar** y siga las instrucciones que se muestran en pantalla.

    ![¿Quiere abrir CloudShell?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloudshell-small.png)

>[!NOTE]
>Cuando se ejecuta la prueba **lint** o **end to end**, Azure usa un servicio de contenedor para aprovisionar un equipo de prueba para llevar a cabo la prueba real. Por este motivo, los resultados de la prueba normalmente pueden tardar varios minutos en devolverse.

Transcurridos unos instantes, verá una lista en el panel Terminal similar a este ejemplo:

![Resultados de prueba](media/terraform-vscode-extension/tf-end-to-end-test-results.png)

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Enumeración de todos los módulos de Terraform disponibles para Azure (y otros proveedores compatibles)](https://registry.terraform.io/)