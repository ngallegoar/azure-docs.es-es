---
title: 'Implementación local de la solución de supervisión remota: IDE de VS (Azure) | Microsoft Docs'
description: En esta guía paso a paso se muestra cómo implementar el acelerador de soluciones de supervisión remota en la máquina local mediante Visual Studio para pruebas y desarrollo.
author: avneet723
manager: hegate
ms.author: avneets
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/17/2019
ms.topic: conceptual
ms.openlocfilehash: a1eba1fceb959bd475d205176c2c53f6409fdc77
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "73890885"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally---visual-studio"></a>Implementación local del acelerador de soluciones de supervisión remota: Visual Studio

[!INCLUDE [iot-accelerators-selector-local](../../includes/iot-accelerators-selector-local.md)]

En este artículo, se explica cómo implementar el acelerador de soluciones de supervisión remota en la máquina local para pruebas y desarrollo. Aprenderá a ejecutar los microservicios en Visual Studio. Una implementación local de microservicios usa los siguientes servicios en la nube: IoT Hub, Cosmos DB, Azure Streaming Analytics y Azure Time Series Insights.

Si quiere ejecutar el acelerador de soluciones de supervisión remota en Docker en el equipo local, consulte [Deploy the Remote Monitoring solution accelerator locally - Docker](iot-accelerators-remote-monitoring-deploy-local-docker.md) (Implementación local del acelerador de soluciones de supervisión remota: Docker).

## <a name="prerequisites"></a>Prerequisites

Para implementar los servicios de Azure que usa el acelerador de soluciones de supervisión remota, necesita una suscripción de Azure activa.

En caso de no tener ninguna, puede crear una cuenta de evaluación gratuita en tan solo unos minutos. Para obtener más información, consulte [Evaluación gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/).

### <a name="machine-setup"></a>Configuración de la máquina

Para completar la implementación local, necesita que las herramientas siguientes estén instaladas en la máquina de desarrollo local:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Visual Studio](https://visualstudio.microsoft.com/)
* [Nginx](https://nginx.org/en/download.html)
* [Node.js v8](https://nodejs.org/): este software es un requisito previo para la CLI de PCS que usan los scripts para crear recursos de Azure. No use Node.js v10.

> [!NOTE]
> Visual Studio está disponible para Windows y Mac.

[!INCLUDE [iot-accelerators-local-setup](../../includes/iot-accelerators-local-setup.md)]

## <a name="run-the-microservices"></a>Ejecución de los microservicios

En esta sección, ejecutará los microservicios de Supervisión remota. Ejecute la interfaz de usuario web de forma nativa, el servicio de simulación de dispositivo en Docker y los microservicios en Visual Studio.

### <a name="run-the-device-simulation-service"></a>Ejecución del servicio de simulación de dispositivos

Abra una nueva ventana del símbolo del sistema para asegurarse de tener acceso a las variables de entorno establecidas por el script **start.cmd** en la sección anterior.

Ejecute el siguiente comando para iniciar el contenedor de Docker para el servicio de simulación de dispositivos. El servicio simula dispositivos para la solución de supervisión remota.

```cmd
<path_to_cloned_repository>\services\device-simulation\scripts\docker\run.cmd
```

### <a name="deploy-all-other-microservices-on-local-machine"></a>Implementación de los demás microservicios en la máquina local

En los pasos siguientes se muestra cómo ejecutar los microservicios de Supervisión remota en Visual Studio:

1. Inicie Visual Studio.
1. Abra la solución **remote-monitoring.sln** en la carpeta **Servicios** de la copia local del repositorio.
1. En el **Explorador de soluciones**, haga clic con el botón derecho en la solución y, a continuación, haga clic en **Propiedades**.
1. Seleccione **Propiedades comunes > Proyecto de inicio**.
1. Seleccione **Varios proyectos de inicio** y establezca **Acción** en **Inicio** para los proyectos siguientes:
    * WebService (asa-manager\WebService)
    * WebService (auth\WebService)
    * WebService (config\WebService)
    * WebService (device-telemetry\WebService)
    * WebService (iothub-manager\WebService)
    * WebService (storage-adapter\WebService)
1. Haga clic en **Aceptar** para guardar las opciones.
1. Haga clic en **Depurar > Iniciar depuración** para compilar y ejecutar los servicios web en la máquina local.

Cada servicio web abre una ventana del explorador web y del símbolo del sistema. En el símbolo del sistema, verá el resultado del servicio en ejecución y la ventana del explorador le permitirá supervisar el estado. No cierre el símbolo del sistema ni las páginas web, ya que esta acción detiene el servicio web.

### <a name="start-the-stream-analytics-job"></a>Inicio del trabajo de Stream Analytics

Siga estos pasos para iniciar el trabajo de Stream Analytics:

1. Acceda a [Azure Portal](https://portal.azure.com).
1. Vaya al **grupo de recursos** creado para la solución. El nombre del grupo de recursos es el nombre que eligió para la solución al ejecutar el script **start.cmd**.
1. Haga clic en el **trabajo de Stream Analytics** de la lista de recursos.
1. En la página de **información general** del trabajo de Stream Analytics, haga clic en el botón **Inicio**. A continuación, haga clic en **Inicio** para iniciar ya el trabajo.

### <a name="run-the-web-ui"></a>Ejecución de la interfaz de usuario web

En este paso, inicie la interfaz de usuario web. Abra una nueva ventana del símbolo del sistema para asegurarse de tener acceso a las variables de entorno establecidas por el script **start.cmd**. Navegue hasta la carpeta **webui** de la copia local del repositorio y ejecute los siguientes comandos:

```cmd
npm install
npm start
```

Una vez completado el inicio, aparecerá en el explorador la página **http:\//localhost:3000/dashboard**. Los errores de esta página son de esperar. Para ver la aplicación sin errores, complete el paso siguiente.

### <a name="configure-and-run-nginx"></a>Configuración y ejecución de NGINX

Configure un servidor proxy inverso para vincular la aplicación web y los microservicios que se ejecutan en el equipo local:

* Copie el archivo **nginx.conf** de la carpeta **webui\scripts\localhost** de la copia local del repositorio en el directorio **nginx\conf** de instalación.
* Ejecute **nginx**.

Para obtener más información acerca de cómo ejecutar **nginx**, consulte [nginx for Windows](https://nginx.org/en/docs/windows.html) (nginx para Windows).

### <a name="connect-to-the-dashboard"></a>Conexión al panel

Para acceder al panel de soluciones de supervisión remota, vaya a http:\//localhost:9000 en el explorador.

## <a name="clean-up"></a>Limpieza

Para evitar cargos innecesarios, cuando haya terminado las pruebas, elimine los servicios en la nube de su suscripción a Azure. Para quitar los servicios, vaya a [Azure Portal](https://ms.portal.azure.com) y elimine el grupo de recursos que creó el script **start.cmd**.

También puede eliminar la copia local del repositorio de Supervisión remota que se ha creado al clonar el código fuente de GitHub.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha implementado la solución de supervisión remota, el paso siguiente es [explorar las funcionalidades del panel de soluciones](quickstart-remote-monitoring-deploy.md).
