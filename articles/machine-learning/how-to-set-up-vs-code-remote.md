---
title: Conexión a una instancia de proceso en Visual Studio Code (versión preliminar)
titleSuffix: Azure Machine Learning
description: Aprenda a conectarse a una instancia de proceso de Azure Machine Learning en Visual Studio Code
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
ms.author: jmartens
author: j-martens
ms.date: 09/03/2020
ms.openlocfilehash: 6ac116b315d4a11b51b37c5b51edf35aa0676713
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91708380"
---
# <a name="connect-to-an-azure-machine-learning-compute-instance-in-visual-studio-code-preview"></a>Conexión a una instancia de proceso de Azure Machine Learning en Visual Studio Code (versión preliminar)

En este artículo, aprenderá a conectarse a una instancia de proceso de Azure Machine Learning mediante Visual Studio Code.

Una [instancia de proceso de Azure Machine Learning](concept-compute-instance.md) es una estación de trabajo basada en la nube totalmente administrada para científicos de datos que proporciona funciones de administración y preparación para empresas a los administradores de TI.

Hay dos maneras de conectarse a una instancia de proceso desde Visual Studio Code:

* Servidor remoto de Jupyter Notebook. Esta opción permite establecer una instancia de proceso como servidor remoto de Jupyter Notebook.
* [Desarrollo remoto de Visual Studio Code](https://code.visualstudio.com/docs/remote/remote-overview). El desarrollo remoto de Visual Studio Code le permite usar un contenedor, una máquina remota o el subsistema de Windows para Linux (WSL) como entorno de desarrollo completo.

## <a name="configure-compute-instance-as-remote-notebook-server"></a>Configuración de la instancia de proceso como servidor de cuadernos remoto

Para configurar una instancia de proceso como servidor remoto de Jupyter Notebook, necesitará cumplir algunos requisitos previos:

* Extensión Azure Machine Learning para Visual Studio Code. Para más información, consulte la [guía de instalación de la extensión Azure Machine Learning para Visual Studio Code](tutorial-setup-vscode-extension.md).
* Área de trabajo de Azure Machine Learning. [Use la extensión Azure Machine Learning para Visual Studio Code para crear un área de trabajo](how-to-manage-resources-vscode.md#create-a-workspace) si aún no tiene una.

Para conectarse a una instancia de proceso:

1. Abra una instancia de Jupyter Notebook en Visual Studio Code.
1. Cuando se cargue la experiencia integrada de Notebook, seleccione **Jupyter Server** (Servidor de Jupyter).

    > [!div class="mx-imgBorder"]
    > ![Inicio de Azure Machine Learning: lista desplegable de servidores remotos de Jupyter Notebook](media/how-to-set-up-vs-code-remote/launch-server-selection-dropdown.png)

    Como alternativa, también puede usar la paleta de comandos:

    1. Seleccione **Ver > Paleta de comandos** en la barra de menús para abrir la paleta de comandos.
    1. Escriba `Azure ML: Connect to Compute instance Jupyter server` en el cuadro de texto.

1. Elija `Azure ML Compute Instances` en la lista de opciones de servidores de Jupyter.
1. En la lista de suscripciones, seleccione la suya. Si ha configurado previamente el área de trabajo de Azure Machine Learning predeterminada, este paso se omitirá.
1. Seleccione su área de trabajo.
1. Seleccione la instancia de proceso de la lista. Si no tiene una, seleccione **Create new Azure ML Compute Instance** (Crear una instancia de proceso de Azute ML) y siga las indicaciones para crearla.
1. Para que los cambios surtan efecto, tiene que volver a cargar Visual Studio Code.
1. Abra un cuaderno de Jupyter Notebook y ejecute una celda.

> [!IMPORTANT]
> Para establecer la conexión, **DEBE** ejecutar una celda.

Llegados a este punto, puede continuar ejecutando las celdas en el cuaderno de Jupyter Notebook.

> [!TIP]
> También puede trabajar con archivos de script de Python (.py) que contengan celdas de código similares a Jupyter. Para más información, consulte la [documentación interactiva de Python para Visual Studio](https://code.visualstudio.com/docs/python/jupyter-support-py).

## <a name="configure-compute-instance-remote-development"></a>Configuración del desarrollo remoto de la instancia de proceso

Para disfrutar de una experiencia de desarrollo remoto completa, deberá cumplir algunos requisitos previos:

* [Extensión Visual Studio Code Remote SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh).
* Instancia de proceso habilitada para SSH. Para más información, [consulte la guía para crear una instancia de proceso](how-to-create-manage-compute-instance.md).

> [!NOTE]
> En las plataformas Windows, debe [instalar un cliente SSH compatible con OpenSSH](https://code.visualstudio.com/docs/remote/troubleshooting#_installing-a-supported-ssh-client) si todavía no lo hay. PuTTY no se admite en Windows, ya que el comando ssh debe estar en la ruta de acceso.

### <a name="get-the-ip-and-ssh-port-for-your-compute-instance"></a>Obtención de la dirección IP y el puerto SSH para la instancia de proceso

1. Vaya a Azure Machine Learning Studio en https://ml.azure.com/.
2. Seleccione el [área de trabajo](concept-workspace.md).
1. Haga clic en la pestaña **Instancias de proceso**.
1. En la columna **URI de la aplicación**, haga clic en el vínculo **SSH** de la instancia de proceso que quiera usar como proceso remoto. 
1. En el cuadro de diálogo, anote la dirección IP y el puerto SSH. 
1. Guarde la clave privada en el directorio ~/.ssh/ del equipo local; por ejemplo, abra un editor para un archivo nuevo y pegue la clave: 

   **Linux**:

   ```sh
   vi ~/.ssh/id_azmlcitest_rsa  
   ```

   **Windows**:

   ```cmd
   notepad C:\Users\<username>\.ssh\id_azmlcitest_rsa
   ```

   La clave privada tendrá un aspecto similar al siguiente:

   ```text
   -----BEGIN RSA PRIVATE KEY-----

   MIIEpAIBAAKCAQEAr99EPm0P4CaTPT2KtBt+kpN3rmsNNE5dS0vmGWxIXq4vAWXD
   ..... 
   ewMtLnDgXWYJo0IyQ91ynOdxbFoVOuuGNdDoBykUZPQfeHDONy2Raw==

   -----END RSA PRIVATE KEY-----
   ```

1. Cambie los permisos en el archivo para asegurarse de que solo usted puede leer el archivo.  

   ```sh
   chmod 600 ~/.ssh/id_azmlcitest_rsa
   ```

### <a name="add-instance-as-a-host"></a>Adición de la instancia como un host

Abra el archivo `~/.ssh/config` (Linux) o `C:\Users<username>.ssh\config` (Windows) en un editor y agregue una nueva entrada similar al contenido siguiente:

```
Host azmlci1 

    HostName 13.69.56.51 

    Port 50000 

    User azureuser 

    IdentityFile ~/.ssh/id_azmlcitest_rsa
```

Estos son algunos detalles de los campos:

|Campo|Descripción|
|----|---------|
|Host|Use la abreviatura que quiera para la instancia de proceso |
|HostName|Esta es la dirección IP de la instancia de proceso |
|Port|Este es el puerto que se muestra en el cuadro de diálogo SSH anterior |
|Usuario|Esto se debe establecer en  `azureuser` |
|IdentityFile|Debe apuntar al archivo en el que ha guardado la clave privada |

Ahora, debería poder acceder con ssh a la instancia de proceso con la abreviatura que ha usado antes, `ssh azmlci1`.

### <a name="connect-vs-code-to-the-instance"></a>Conexión de VS Code a la instancia

1. Haga clic en el icono de Remote-SSH en la barra de actividades de Visual Studio Code para mostrar las configuraciones de SSH.

1. Haga clic con el botón derecho en la configuración del host SSH que acaba de crear.

1. Seleccione **Connect to Host in Current Window** (Conectar al host en la ventana actual). 

Desde aquí, trabajará por completo en la instancia de proceso y ahora puede editar, depurar, usar git, usar extensiones, etc., al igual que con la instancia local de Visual Studio Code.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha configurado Visual Studio Code remoto, puede usar una instancia de proceso como proceso remoto desde Visual Studio Code para [depurar el código de forma interactiva](how-to-debug-visual-studio-code.md).

[Tutorial: Formar su primer modelo de ML](tutorial-1st-experiment-sdk-train.md) muestra cómo usar una instancia de proceso con un cuaderno integrado.