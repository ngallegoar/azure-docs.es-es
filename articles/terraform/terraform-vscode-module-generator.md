---
title: 'Tutorial: Creación de una plantilla base de Terraform en Azure mediante Yeoman'
description: Obtenga información sobre cómo crear una plantilla base de Terraform en Azure mediante Yeoman.
ms.topic: tutorial
ms.date: 10/26/2019
ms.openlocfilehash: 82c3f5e640789547abb716b55959e1821a61e6d0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "77472152"
---
# <a name="tutorial-create-a-terraform-base-template-in-azure-using-yeoman"></a>Tutorial: Creación de una plantilla base de Terraform en Azure mediante Yeoman

En este tutorial, aprenderá a usar la combinación de [Terraform](/azure/terraform/) y [Yeoman](https://yeoman.io/). Terraform es una herramienta para la creación de una infraestructura en Azure. Yeoman facilita la creación de módulos de Terraform.

En este artículo, aprenderá a realizar las siguientes tareas:
> [!div class="checklist"]
> * Crear una plantilla de Terraform base mediante el generador de módulos de Yeoman.
> * Probar la plantilla de Terraform con dos métodos diferentes.
> * Probar el módulo de Terraform mediante un archivo de Docker.
> * Ejecutar el módulo de Terraform de forma nativa en Azure Cloud Shell.

## <a name="prerequisites"></a>Prerrequisitos

- **Suscripción de Azure**: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
- **Visual Studio Code**: [descargue Visual Studio Code](https://code.visualstudio.com/download) para su plataforma.
- **Terraform**: [Instale Terraform](terraform-install-configure.md) para ejecutar el módulo creado por Yeoman.
- **Docker**: [Instale Docker](https://www.docker.com/get-started) para ejecutar el módulo creado por el generador de Yeoman.
- **Lenguaje de programación Go**: [Instale Go](https://golang.org/) ya que los casos de prueba generados por Yeoman son código que usa el lenguaje Go.

>[!NOTE]
>La mayoría de los procedimientos de este tutorial implican a la interfaz de la línea de comandos. Los pasos descritos aquí se aplican a todos los sistemas operativos y herramientas de línea de comandos. En nuestros ejemplos, se eligió PowerShell para el entorno local y Bash de Git para el entorno de Cloud Shell.

## <a name="prepare-your-environment"></a>Preparación del entorno

### <a name="install-nodejs"></a>Instalación de Node.js

Para usar Terraform en Cloud Shell, deberá [instalar Node.js](https://nodejs.org/en/download/) 6.0 o posterior.

>[!NOTE]
>Para comprobar si se ha instalado Node.js, abra una ventana de terminal y escriba `node --version`.

### <a name="install-yeoman"></a>Instalación de Yeoman

Ejecute el siguiente comando:

```bash
npm install -g yo
```

![Instalación de Yeoman](media/terraform-vscode-module-generator/ymg-npm-install-yo.png)

### <a name="install-the-yeoman-template-for-terraform-module"></a>Instalar la plantilla de Yeoman para el módulo de Terraform

Ejecute el siguiente comando:

```bash
npm install -g generator-az-terra-module
```

![Instalar generator-az-terra-module](media/terraform-vscode-module-generator/ymg-pm-install-generator-module.png)

Para comprobar si Yeoman está instalado, ejecute el comando siguiente:

```bash
yo --version
```

### <a name="create-a-directory-for-the-yeoman-generated-module"></a>Creación de un directorio para el módulo generado por Yeoman

La plantilla de Yeoman genera archivos en el directorio actual. Por este motivo, debe crear un directorio.

Es necesario poner este directorio vacío en $GOPATH/src. Para más información acerca de esta ruta de acceso, consulte el artículo [Configuración de GOPATH](https://github.com/golang/go/wiki/SettingGOPATH).

1. Vaya al directorio primario desde el que se va a crear un nuevo directorio.

1. Ejecute el comando siguiente, reemplazando el marcador de posición. En este ejemplo, se utiliza el nombre de directorio `GeneratorDocSample`.

    ```bash
    mkdir <new-directory-name>
    ```

    ![mkdir](media/terraform-vscode-module-generator/ymg-mkdir-GeneratorDocSample.png)

1. Vaya al nuevo directorio:

    ```bash
    cd <new-directory-name>
    ```

    ![Vaya a su nuevo directorio](media/terraform-vscode-module-generator/ymg-cd-GeneratorDocSample.png)

## <a name="create-a-base-module-template"></a>Crear una plantilla de módulo básica

1. Ejecute el siguiente comando:

    ```bash
    yo az-terra-module
    ```

1. Siga las instrucciones en pantalla para proporcionar la siguiente información:

    - **Nombre del proyecto del módulo Terraform**: se usa un valor de `doc-sample-module` para el ejemplo.

        ![Nombre de proyecto](media/terraform-vscode-module-generator/ymg-project-name.png)       


    - **¿Desea incluir el archivo de imagen de Docker?** - Escriba `y`. Si selecciona `n`, el código de módulo generado solo admitirá la ejecución en modo nativo.

        ![¿Incluir el archivo de imagen de Docker?](media/terraform-vscode-module-generator/ymg-include-docker-image-file.png) 

1. Enumere el contenido del directorio para ver los archivos resultantes que se crean:

    ```bash
    ls
    ```

    ![Incluir los archivos creados](media/terraform-vscode-module-generator/ymg-ls-GeneratorDocSample-files.png)

## <a name="review-the-generated-module-code"></a>Revisar el código de módulo generado

1. Iniciar Visual Studio Code

1. En la barra de menús, seleccione **Archivo > Abrir carpeta**, además de la carpeta que creó.

    ![Visual Studio Code](media/terraform-vscode-module-generator/ymg-open-in-vscode.png)

El generador de módulos de Yeoman creó los siguientes archivos. Para más información de estos archivos y de su uso, consulte [Terratest en los módulos de Terraform](https://mseng.visualstudio.com/VSJava/_git/Terraform?path=%2FTerratest%20Introduction.md&version=GBmaster).

- `main.tf`: define un módulo llamado `random-shuffle`. La entrada es de tipo `string_list`. El resultado es el recuento de las permutaciones.
- `variables.tf`: define las variables de entrada y salida usadas por el módulo.
- `outputs.tf`: define lo que genera el módulo. Aquí, es el valor devuelto por `random_shuffle`, que es un módulo de Terraform integrado.
- `Rakefile`: define los pasos de compilación. Estos pasos incluyen:
    - `build`: valida el formato del archivo main.tf.
    - `unit`: el esqueleto del módulo generado no incluye el código para una prueba unitaria. Si desea especificar un escenario de prueba unitaria, agregaría ese código aquí.
    - `e2e`: ejecuta una prueba de un extremo a otro del módulo.
- `test`
    - Los casos de prueba están escritos en Go.
    - Todos los códigos de la prueba son pruebas de un extremo a otro.
    - Las pruebas de un extremo a otro intentan aprovisionar todos los elementos definidos en `fixture`. Los resultados en el archivo `template_output.go` se comparan con los valores predefinidos esperados.
    - `Gopkg.lock` y `Gopkg.toml`: define las dependencias. 

## <a name="test-your-new-terraform-module-using-a-docker-file"></a>Prueba del nuevo módulo de Terraform mediante un archivo de Docker

Esta sección muestra cómo probar un módulo de Terraform mediante un archivo de Docker.

>[!NOTE]
>En este ejemplo se ejecuta el módulo localmente, no en Azure.

### <a name="confirm-docker-is-installed-and-running"></a>Confirmar que Docker está instalado y en ejecución

Desde un símbolo del sistema, escriba `docker version`.

![Versión de Docker](media/terraform-vscode-module-generator/ymg-docker-version.png)

El resultado que se obtiene confirma que Docker está instalado.

Para confirmar que Docker se está ejecutando realmente, escriba `docker info`.

![Información de Docker](media/terraform-vscode-module-generator/ymg-docker-info.png)

### <a name="set-up-a-docker-container"></a>Configurar un contenedor de Docker

1. Desde un símbolo del sistema, escriba

    `docker build --build-arg BUILD_ARM_SUBSCRIPTION_ID= --build-arg BUILD_ARM_CLIENT_ID= --build-arg BUILD_ARM_CLIENT_SECRET= --build-arg BUILD_ARM_TENANT_ID= -t terra-mod-example .`.

    Se mostrará el mensaje **Successfully built** (La compilación se realizó correctamente).

    ![Mensaje que indica que la compilación es correcta](media/terraform-vscode-module-generator/ymg-successfully-built.png)

1. En el símbolo del sistema, escriba `docker image ls` para ver el módulo creado `terra-mod-example` enumerado.

    ![Lista que contiene el nuevo módulo](media/terraform-vscode-module-generator/ymg-repository-results.png)

1. Escriba `docker run -it terra-mod-example /bin/sh`. Después de ejecutar el comando `docker run`, se encuentra en el entorno de Docker. En ese momento, puede detectar el archivo mediante el comando `ls`.

    ![Lista de archivos en Docker](media/terraform-vscode-module-generator/ymg-list-docker-file.png)

### <a name="build-the-module"></a>Compilación del módulo

1. Ejecute el siguiente comando:

    ```bash
    bundle install
    ```

1. Ejecute el siguiente comando:

    ```bash
    rake build
    ```

    ![Amontonar la compilación](media/terraform-vscode-module-generator/ymg-rake-build.png)

### <a name="run-the-end-to-end-test"></a>Ejecución de la prueba de un extremo a otro

1. Ejecute el siguiente comando:

    ```bash
    rake e2e
    ```

1. Poco después, aparecerá el mensaje **PASS** (Sin errores).

    ![PASS](media/terraform-vscode-module-generator/ymg-pass.png)

1. Escriba `exit` para finalizar la prueba y salir del entorno de Docker.

## <a name="use-yeoman-generator-to-create-and-test-a-module-in-cloud-shell"></a>Uso del generador Yeoman para crear y probar un módulo en Cloud Shell

En esta sección, el generador de Yeoman se usa para crear y probar un módulo en Cloud Shell. El uso de Cloud Shell en lugar de usar un archivo de Docker simplifica en gran medida el proceso. Con Cloud Shell, todos los productos siguientes están preinstalados:

- Node.js
- Yeoman
- Terraform

### <a name="start-a-cloud-shell-session"></a>Inicio de una sesión de Cloud Shell

1. Inicie una sesión de Azure Cloud Shell mediante [Azure Portal](https://portal.azure.com/), [shell.azure.com](https://shell.azure.com) o la [aplicación móvil de Azure](https://azure.microsoft.com/features/azure-portal/mobile-app/).

1. Se abre la página de **bienvenida a Azure Cloud Shell**. Seleccione **Bash (Linux)** .

    ![Bienvenido a Azure Cloud Shell](media/terraform-vscode-module-generator/ymg-welcome-to-azure-cloud-shell.png)

1. Si no ha configurado una cuenta de almacenamiento de Azure, aparecerá la pantalla siguiente. Seleccione **Crear almacenamiento**.

    ![No tiene ningún almacenamiento montado](media/terraform-vscode-module-generator/ymg-you-have-no-storage-mounted.png)

1. Azure Cloud Shell se inicia en el shell que seleccionó anteriormente y muestra la información de la unidad en la nube que acaba de crear para usted.

    ![Se ha creado su unidad en la nube.](media/terraform-vscode-module-generator/ymg-your-cloud-drive-has-been-created-in.png)

### <a name="prepare-a-directory-to-hold-your-terraform-module"></a>Preparación de un directorio para almacenar el módulo de Terraform

1. En este momento, Cloud Shell ya habrá configurado GOPATH en las variables de entorno. Para ver la ruta de acceso, escriba `go env`.

1. Cree el directorio $GOPATH, si aún no existe: Escriba `mkdir ~/go`.

1. Cree un directorio en el directorio $GOPATH. Se usa para almacenar los distintos directorios del proyecto creados en este ejemplo. 

    ```bash
    mkdir ~/go/src
    ```

1. Cree un directorio para contener el módulo Terraform reemplazando el marcador de posición. En este ejemplo, se utiliza el nombre de directorio `my-module-name`.

    ```bash
    mkdir ~/go/src/<your-module-name>
    ```

1. Vaya al directorio del módulo: 

    ```bash
    cd ~/go/src/<your-module-name>
    ```

### <a name="create-and-test-your-terraform-module"></a>Creación y prueba del módulo de Terraform

1. Ejecute el siguiente comando y siga las instrucciones. Cuando se le pregunte si desea crear los archivos de Docker, escriba `N`.

    ```bash
    yo az-terra-module
    ```

1. Ejecute el siguiente comando para instalar las dependencias:

    ```bash
    bundle install
    ```

1. Ejecute el siguiente comando para compilar el módulo:

    ```bash
    rake build
    ```

    ![Amontonar la compilación](media/terraform-vscode-module-generator/ymg-rake-build.png)

1. Ejecute el siguiente comando para ejecutar el texto:

    ```bash
    rake e2e
    ```

    ![Resultados de la prueba superada](media/terraform-vscode-module-generator/ymg-pass.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Instalación y uso de la extensión de Visual Studio Code para Azure Terraform](/azure/terraform/terraform-vscode-extension).