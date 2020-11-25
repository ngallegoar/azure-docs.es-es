---
title: archivo de inclusión
description: archivo de inclusión
services: iot-accelerators
author: avneet723
ms.service: iot-accelerators
ms.topic: include
ms.date: 01/17/2019
ms.author: avneet723
ms.custom: include file
ms.openlocfilehash: 1f567b3d083853f9bb342bfad462e8545caa6480
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96020265"
---
## <a name="download-the-source-code"></a>Descarga del código fuente

Los repositorios de código fuente de Supervisión remota incluyen el código fuente y los archivos de configuración de Docker que necesita para ejecutar las imágenes de Docker de los microservicios.

Para clonar y crear una versión local del repositorio, use el entorno de línea de comandos para ir a una carpeta adecuada en la máquina local. A continuación, ejecute uno de los siguientes conjuntos de comandos para clonar el repositorio de .NET:

Para descargar la versión más reciente de las implementaciones de microservicios de .NET, ejecute:

```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet.git

# To retrieve the latest submodules, run the following command:

cd azure-iot-pcs-remote-monitoring-dotnet
git submodule foreach git pull origin master
```

> [!NOTE]
> Estos comandos descargan el código fuente de todos los microservicios, además de los scripts que se usan para ejecutar los microservicios localmente. Aunque no necesita el código fuente para ejecutar los microservicios en Docker, el código fuente es útil si más tarde piensa modificar el acelerador de soluciones y probar los cambios localmente.

## <a name="deploy-the-azure-services"></a>Implementación de servicios de Azure

Aunque en este artículo se muestra cómo ejecutar los microservicios localmente, dependen de los servicios de Azure que se ejecutan en la nube. Use el siguiente script para implementar los servicios de Azure. En los siguientes ejemplos de scripts se da por hecho que usa el repositorio de .NET en una máquina Windows. Si está trabajando en otro entorno, ajuste las rutas de acceso, las extensiones de archivo y los separadores de ruta de acceso de la manera correcta.

### <a name="create-new-azure-resources"></a>Crear nuevos recursos de Azure

Si aún no ha creado los recursos de Azure necesarios, siga estos pasos:

1. En el entorno de línea de comandos, vaya a la carpeta **\services\scripts\local\launch** de la copia clonada del repositorio.

1. Ejecute los comandos siguientes para instalar la herramienta de la CLI **pcs** e inicie sesión en su cuenta de Azure:

    ```cmd
    npm install -g iot-solutions
    pcs login
    ```

1. Ejecute el script **start.cmd**. El script le pedirá la siguiente información:
   * Nombre de la solución.
   * La suscripción de Azure que se va a usar.
   * La ubicación del centro de datos de Azure que se va a usar.

     El script crea el grupo de recursos en Azure con el nombre de la solución. Este grupo de recursos contiene los recursos de Azure que usa el acelerador de soluciones. Puede eliminar este grupo de recursos cuando ya no necesite los recursos correspondientes.

     El script también agrega un conjunto de variables de entorno con un prefijo **PCS** en el equipo local. Estas variables de entorno proporcionan los detalles de Supervisión remota para poder leer desde un recurso de Azure Key Vault. Este recurso de Key Vault es desde donde Supervisión remota leerá sus valores de configuración.

     > [!TIP]
     > Cuando el script finaliza, también guarda las variables de entorno en un archivo denominado **\<your home folder\>\\.pcs\\\<solution name\>.env**. Puede usarlos para las implementaciones del acelerador de soluciones futuras. Tenga en cuenta que cualquier variable de entorno establecida en el equipo local invalida los valores del archivo **services\\scripts\\local\\.env** al ejecutar **docker-compose**.

1. Salga del entorno de la línea de comandos.

### <a name="use-existing-azure-resources"></a>Usar recursos de Azure existentes

Si ya ha creado los recursos de Azure necesarios, cree las variables de entorno correspondientes en el equipo local.
Establezca las variables de entorno para lo siguiente:
* **PCS_KEYVAULT_NAME**: nombre del recurso de Azure Key Vault
* **PCS_AAD_APPID**: id. de aplicación de AAD
* **PCS_AAD_APPSECRET**: secreto de aplicación de AAD

Los valores de configuración se leerán desde este recurso de Azure Key Vault. Estas variables de entorno pueden guardarse en el archivo **\<your home folder\>\\.pcs\\\<solution name\>.env** de la implementación. Tenga en cuenta que las variables de entorno establecidas en el equipo local invalidan los valores del archivo **services\\scripts\\local\\.env** al ejecutar **docker-compose**.

Parte de la configuración necesaria para el microservicio se almacena en una instancia de **Key Vault** que se ha creado en la implementación inicial. Las variables correspondientes del almacén de claves deben modificarse según sea necesario.