---
title: Implementación remota de la solución de supervisión remota, Docker, Azure | Microsoft Docs
description: En esta guía paso a paso se muestra cómo implementar el acelerador de soluciones de supervisión remota en la máquina local mediante Docker para pruebas y desarrollo.
author: avneet723
manager: hegate
ms.author: avneets
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/25/2018
ms.topic: conceptual
ms.openlocfilehash: 32b47d9d6d45ff471961f55f8159dbe85eae2dce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73888830"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally---docker"></a>Implementación local del acelerador de soluciones de supervisión remota: Docker

[!INCLUDE [iot-accelerators-selector-local](../../includes/iot-accelerators-selector-local.md)]

En este artículo, se explica cómo implementar el acelerador de soluciones de supervisión remota en la máquina local para pruebas y desarrollo. Aprenderá a implementar los microservicios en contenedores locales de Docker. Una implementación local de microservicios usa los siguientes servicios en la nube: IoT Hub, Cosmos DB, Azure Streaming Analytics y Azure Time Series Insights.

Si quiere ejecutar el acelerador de soluciones de supervisión remota en un IDE del equipo local, consulte [Implementación local del acelerador de soluciones de supervisión remota: Visual Studio](iot-accelerators-remote-monitoring-deploy-local.md).

## <a name="prerequisites"></a>Prerequisites

Para implementar los servicios de Azure que usa el acelerador de soluciones de supervisión remota, necesita una suscripción de Azure activa.

En caso de no tener ninguna, puede crear una cuenta de evaluación gratuita en tan solo unos minutos. Para obtener más información, consulte [Evaluación gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/).

### <a name="machine-setup"></a>Configuración de la máquina

Para completar la implementación local, necesita que las herramientas siguientes estén instaladas en la máquina de desarrollo local:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Visual Studio](https://visualstudio.microsoft.com/): si tiene previsto realizar cambios en los microservicios.
* [Node.js v8](https://nodejs.org/): este software es un requisito previo para la CLI de PCS que usan los scripts para crear recursos de Azure. No use Node.js v10.

> [!NOTE]
> Estas herramientas están disponibles en varias plataformas, como Windows, Linux e iOS.

[!INCLUDE [iot-accelerators-local-setup](../../includes/iot-accelerators-local-setup.md)]

## <a name="run-the-microservices-in-docker"></a>Ejecución de los microservicios en Docker

Abra un nuevo símbolo del sistema para asegurarse de tener acceso a las variables de entorno establecidas por el script **start.cmd**. En Windows, puede comprobar que se han establecido las variables de entorno con la ejecución del comando siguiente:

```cmd
set PCS
```

El comando muestra todas las variables de entorno establecidas por el script **start.cmd**.

Asegúrese de que Docker está en ejecución en el equipo local.
> [!NOTE]
> Docker debe ejecutarse en los [contenedores de Linux](https://docs.docker.com/docker-for-windows/) si se está ejecutando en Windows.

Los microservicios que se ejecutan en los contenedores locales de Docker necesitan acceso a los servicios en la nube de Azure. Puede probar la conectividad a Internet de su entorno de Docker con el siguiente comando que hace ping en una dirección de Internet desde dentro de un contenedor:

```cmd/sh
docker run --rm -ti library/alpine ping google.com
```

Para ejecutar el acelerador de soluciones, vaya a la carpeta **services\\scripts\\local** en el entorno de línea de comandos y ejecute el comando siguiente:

```cmd/sh
docker-compose up
```

> [!NOTE] 
> Asegúrese de [compartir una unidad local](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/issues/115) con Docker antes de ejecutar `docker-compose up`.

La primera vez que ejecuta este comando, Docker descarga las imágenes de microservicios desde Docker Hub para crear los contenedores localmente. En las ejecuciones posteriores, Docker ejecuta los contenedores inmediatamente.

> [!TIP]
> Microsoft publica con frecuencia nuevas imágenes de Docker con la nueva funcionalidad. Puede usar el siguiente conjunto de comandos para limpiar los contenedores locales de Docker y las imágenes correspondientes antes de extraer las más recientes:

```cmd/sh
docker list
docker rm <list_of_containers>
docker rmi <list_of_images>
```

Puede utilizar un shell independiente para ver los registros desde el contenedor. En primer lugar, busque el identificador del contenedor mediante el comando `docker ps`. A continuación, utilice `docker logs {container-id} --tail 1000` para ver las 1000 entradas más recientes del contenedor especificado.

### <a name="start-the-stream-analytics-job"></a>Inicio del trabajo de Stream Analytics

Siga estos pasos para iniciar el trabajo de Stream Analytics:

1. Acceda a [Azure Portal](https://portal.azure.com).
1. Vaya al **grupo de recursos** creado para la solución. El nombre del grupo de recursos es el nombre que eligió para la solución al ejecutar el script **start.cmd**.
1. Haga clic en el **trabajo de Stream Analytics** de la lista de recursos.
1. En la página de **información general** del trabajo de Stream Analytics, haga clic en el botón **Inicio**. A continuación, haga clic en **Inicio** para iniciar ya el trabajo.

### <a name="connect-to-the-dashboard"></a>Conexión al panel

Para acceder al panel de la solución de supervisión remota, vaya a `http://localhost:8080` en el explorador. Ahora puede usar la interfaz de usuario web y los microservicios locales.

## <a name="clean-up"></a>Limpieza

Para evitar cargos innecesarios, cuando haya terminado las pruebas, elimine los servicios en la nube de su suscripción a Azure. Para quitar los servicios, vaya a [Azure Portal](https://ms.portal.azure.com) y elimine el grupo de recursos que creó el script **start.cmd**.

Utilice el comando `docker-compose down --rmi all` para quitar las imágenes de Docker y liberar espacio en la máquina local. También puede eliminar la copia local del repositorio de Supervisión remota que se ha creado al clonar el código fuente de GitHub.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha implementado la solución de supervisión remota, el paso siguiente es [explorar las funcionalidades del panel de soluciones](quickstart-remote-monitoring-deploy.md).
