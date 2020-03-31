---
title: Implementación local de la solución de supervisión remota (Visual Studio Code) en Azure | Microsoft Docs
description: En esta guía paso a paso se muestra cómo implementar el acelerador de soluciones de supervisión remota en la máquina local mediante Visual Studio Code para pruebas y desarrollo.
author: avneet723
manager: hegate
ms.author: avneets
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/17/2019
ms.topic: conceptual
ms.openlocfilehash: 8f1d20e9a6a78d99a23fe4b98aeb4f3eb8359da7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73890956"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally---visual-studio-code"></a>Implementación local del acelerador de soluciones de supervisión remota con Visual Studio Code

[!INCLUDE [iot-accelerators-selector-local](../../includes/iot-accelerators-selector-local.md)]

En este artículo, se explica cómo implementar el acelerador de soluciones de supervisión remota en la máquina local para pruebas y desarrollo. Aprenderá a ejecutar los microservicios en Visual Studio Code. Una implementación local de microservicios utiliza los siguientes servicios en la nube: IoT Hub, Cosmos DB, Azure Streaming Analytics y Azure Time Series Insights.

## <a name="prerequisites"></a>Prerrequisitos

Para implementar los servicios de Azure que usa el acelerador de soluciones de supervisión remota, necesita una suscripción de Azure activa.

En caso de no tener ninguna, puede crear una cuenta de evaluación gratuita en tan solo unos minutos. Para obtener más información, consulte [Evaluación gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/).

### <a name="machine-setup"></a>Configuración de la máquina

Para completar la implementación local, necesita que las herramientas siguientes estén instaladas en la máquina de desarrollo local:

* [Git](https://git-scm.com/)
* [.NET Core](https://dotnet.microsoft.com/download)
* [Docker](https://www.docker.com)
* [Nginx](https://nginx.org/en/download.html)
* [Visual Studio Code](https://code.visualstudio.com/)
* [Extensión de VS Code para C#](https://code.visualstudio.com/docs/languages/csharp)
* [Node.js v8](https://nodejs.org/): este software es un requisito previo para la CLI de PCS que usan los scripts para crear recursos de Azure. No use Node.js v10.

> [!NOTE]
> Visual Studio Code está disponible para Windows, Mac y Ubuntu.

[!INCLUDE [iot-accelerators-local-setup](../../includes/iot-accelerators-local-setup.md)]

## <a name="run-the-microservices"></a>Ejecución de los microservicios

En esta sección, ejecutará los microservicios de Supervisión remota. Ejecute la interfaz de usuario web de forma nativa, el servicio de simulación de dispositivo en Docker y los microservicios en Visual Studio Code.

### <a name="build-the-code"></a>Compilación del código

Vaya a azure-iot-pcs-remote-monitoring-dotnet\services en el símbolo del sistema y ejecute los siguientes comandos para compilar el código.

```cmd
dotnet restore
dotnet build -c Release
```

### <a name="deploy-all-other-microservices-on-local-machine"></a>Implementación de los demás microservicios en la máquina local

En los pasos siguientes, se muestra cómo se ejecutan los microservicios de supervisión remota en Visual Studio Code:

1. Inicie Visual Studio Code.
1. En VS Code, abra la carpeta **azure-iot-pcs-remote-monitoring-dotnet**.
1. Cree una nueva carpeta llamada **.vscode** en **azure-iot-pcs-remote-monitoring-dotnet**.
1. Copie los archivos **launch.json** y **tasks.json** de services\scripts\local\launch\idesettings\vscode en la carpeta **.vscode** que acaba de crear.
1. Abra el **panel de depuración** en VS Code y ejecute la opción **Run All microservices** (Ejecutar todos los microservicios). Esta configuración ejecuta el microservicio de simulación de dispositivos en Docker y ejecuta los demás microservicios en el depurador.

Después de ejecutar **Run All microservices** (Ejecutar todos los microservicios) en la Consola de depuración, el resultado debería ser similar al siguiente:

[![Implementación de microservicios locales](./media/deploy-locally-vscode/auth-debug-results-inline.png)](./media/deploy-locally-vscode/auth-debug-results-expanded.png#lightbox)

### <a name="run-the-web-ui"></a>Ejecución de la interfaz de usuario web

En este paso, inicie la interfaz de usuario web. Vaya a la carpeta **azure-iot-pcs-remote-monitoring-dotnet\webui** en su copia local y ejecute los siguientes comandos:

```cmd
npm install
npm start
```

Una vez completado el inicio, aparecerá en el explorador la página **http:\//localhost:3000/dashboard**. Los errores de esta página son de esperar. Para ver la aplicación sin errores, complete el paso siguiente.

### <a name="configure-and-run-nginx"></a>Configuración y ejecución de NGINX

Configure un servidor proxy inverso para vincular la aplicación web y los microservicios que se ejecutan en el equipo local:

* Copie el archivo **nginx.conf** de la carpeta **webui\scripts\localhost** en el directorio **nginx\conf** de instalación.
* Ejecute **nginx**.

Para obtener más información acerca de cómo ejecutar **nginx**, consulte [nginx for Windows](https://nginx.org/en/docs/windows.html) (nginx para Windows).

### <a name="connect-to-the-dashboard"></a>Conexión al panel

Para acceder al panel de soluciones de supervisión remota, vaya a http:\//localhost:9000 en el explorador.

## <a name="clean-up"></a>Limpieza

Para evitar cargos innecesarios, cuando haya terminado las pruebas, elimine los servicios en la nube de su suscripción a Azure. Para quitar los servicios, vaya a [Azure Portal](https://ms.portal.azure.com) y elimine el grupo de recursos que creó el script **start.cmd**.

También puede eliminar la copia local del repositorio de Supervisión remota que se ha creado al clonar el código fuente de GitHub.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha implementado la solución de supervisión remota, el paso siguiente es [explorar las funcionalidades del panel de soluciones](quickstart-remote-monitoring-deploy.md).
