---
title: 'Implementación de la solución de supervisión remota mediante la CLI: Azure | Microsoft Docs'
description: En esta guía paso a paso, se muestra cómo aprovisionar el acelerador de soluciones de supervisión remota mediante la CLI.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: conceptual
ms.openlocfilehash: f9dcf19f5318021df5d9fdde777b8786942e33d8
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92072261"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-using-the-cli"></a>Implementación del acelerador de soluciones de supervisión remota mediante la CLI

En esta guía paso a paso, se explica cómo implementar el acelerador de soluciones de supervisión remota. La solución se implementa mediante la CLI. También puede implementar la solución con la interfaz de usuario basada en web de azureiotsolutions.com. Para más información sobre esta opción, vea la guía de inicio rápido [Implementación del acelerador de soluciones de supervisión remota](quickstart-remote-monitoring-deploy.md).

## <a name="prerequisites"></a>Prerrequisitos

Para implementar el acelerador de soluciones de supervisión remota, necesita una suscripción de Azure activa.

En caso de no tener ninguna, puede crear una cuenta de evaluación gratuita en tan solo unos minutos. Para obtener más información, consulte [Evaluación gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/).

Para ejecutar la CLI, necesita tener instalado [Node.js](https://nodejs.org/) en su máquina local.

## <a name="install-the-cli"></a>Instalación de la CLI de Azure

Para instalar la CLI, ejecute el siguiente comando en el entorno de la línea de comandos:

```cmd/sh
npm install iot-solutions -g
```

## <a name="sign-in-to-the-cli"></a>Inicio de sesión en la CLI

Para poder implementar el acelerador de la solución, debe iniciar sesión en su suscripción de Azure mediante la CLI:

```cmd/sh
pcs login
```

Siga las instrucciones que aparecen en pantalla para completar el proceso de inicio de sesión.

## <a name="deployment-options"></a>Opciones de implementación

Al implementar el acelerador de la solución, hay varias opciones que permiten configurar el proceso de implementación:

| Opción | Valores | Descripción |
| ------ | ------ | ----------- |
| SKU    | `basic`, `standard`, `local` | Una implementación _básica_ está destinada a pruebas y demostraciones. En este tipo de implementación, todos los microservicios se implementan en una única máquina virtual. Una implementación _estándar_ está destinada a producción. En este tipo de implementación, los microservicios se implementan en varias máquinas virtuales. Una implementación _local_ configura un contenedor de Docker para que ejecute los microservicios en la máquina local y usa los servicios de nube de Azure, como Storage y Cosmos DB. |
| Tiempo de ejecución | `dotnet`, `java` | Selecciona la implementación del lenguaje de los microservicios. |

Para obtener información sobre cómo usar la opción de implementación local, consulte el artículo sobre la [ejecución local de la solución de supervisión remota](iot-accelerators-remote-monitoring-deploy-local.md).

## <a name="basic-and-standard-deployments"></a>Implementaciones estándar y básica

En esta sección se resumen las diferencias claves entre una implementación básica y estándar.

### <a name="basic"></a>Básica

Se puede hacer una implementación básica desde [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators) o mediante la CLI.

La implementación básica está orientada a presentar la solución. Para reducir los costos, todos los microservicios se implementan en una sola máquina virtual. Esta implementación no usa una arquitectura lista para producción.

Una implementación básica crea los siguientes servicios en la suscripción de Azure:

| Count | Resource                       | Tipo         | Se usa para |
|-------|--------------------------------|--------------|----------|
| 1     | [Máquina virtual con Linux](https://azure.microsoft.com/services/virtual-machines/) | Estándar D1 v2  | Hospedaje de microservicios |
| 1     | [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)                  | S1: nivel Estándar | Comunicación y administración de dispositivos |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)              | Estándar        | Almacenamiento de datos de configuración, reglas, alertas y otro tipo de almacenamiento en frío |  
| 1     | [Cuenta de Azure Storage](../storage/common/storage-introduction.md#types-of-storage-accounts)  | Estándar        | Almacenar puntos de comprobación de máquina virtual y streaming |
| 1     | [Aplicación web](https://azure.microsoft.com/services/app-service/web/)        |                 | Hospedaje de aplicación web front-end |
| 1     | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)        |                 | Administración de identidades y seguridad de usuarios |
| 1     | [Azure Maps](https://azure.microsoft.com/services/azure-maps/)        | Estándar                | Visualización de ubicaciones de recursos |
| 1     | [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)        |   3 unidades              | Habilitación de análisis en tiempo real |
| 1     | [Servicio de Azure Device Provisioning](../iot-dps/index.yml)        |       S1          | Aprovisionamiento de dispositivos a escala |
| 1     | [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/)        |   S1: 1 unidad              | Almacenamiento para datos de mensajes y permite el análisis detallado de telemetría |

### <a name="standard"></a>Estándar

Se puede hacer una implementación estándar solo mediante la CLI.

Una implementación estándar es una implementación lista para producción que un desarrollador puede personalizar y ampliar. Use la opción de implementación estándar cuando esté listo para personalizar una arquitectura para entornos de producción, creada para fines de escalabilidad y extensibilidad. Los microservicios de aplicación se crean como contenedores de Docker y se implementan con Azure Kubernetes Service. El orquestador de Kubernetes implementa, escala y administra los microservicios.

Una implementación estándar crea los siguientes servicios en la suscripción de Azure:

| Count | Resource                                     | SKU / Tamaño      | Se usa para |
|-------|----------------------------------------------|-----------------|----------|
| 1     | [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service)| Use un servicio de orquestación de contenedores de Kubernetes totalmente administrados, con un valor predeterminado de tres agentes.|
| 1     | [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)                     | S2: nivel Estándar | Comando, control y administración de dispositivos |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)                 | Estándar        | Almacenar datos de configuración y telemetría de dispositivos como reglas, alertas y mensajes |
| 5     | [Cuentas de Azure Storage](../storage/common/storage-introduction.md#types-of-storage-accounts)    | Estándar        | 4 para el almacenamiento de máquina virtual y 1 para los puntos de comprobación de streaming |
| 1     | [App Service](https://azure.microsoft.com/services/app-service/web/)             | S1 Estándar     | Application Gateway a través de TLS |
| 1     | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)        |                 | Administración de identidades y seguridad de usuarios |
| 1     | [Azure Maps](https://azure.microsoft.com/services/azure-maps/)        | Estándar                | Visualización de ubicaciones de recursos |
| 1     | [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)        |   3 unidades              | Habilitación de análisis en tiempo real |
| 1     | [Servicio de Azure Device Provisioning](../iot-dps/index.yml)        |       S1          | Aprovisionamiento de dispositivos a escala |
| 1     | [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/)        |   S1: 1 unidad              | Almacenamiento para datos de mensajes y permite el análisis detallado de telemetría |

> [!NOTE]
> Puede encontrar información sobre los precios de estos servicios en [https://azure.microsoft.com/pricing](https://azure.microsoft.com/pricing). Puede encontrar los detalles de uso y facturación de la suscripción en [Azure Portal](https://portal.azure.com/).

## <a name="deploy-the-solution-accelerator"></a>Implementación del acelerador de solución

Ejemplos de implementación:

### <a name="example-deploy-net-version"></a>Ejemplo: Implementación de la versión de .NET

En el ejemplo siguiente, se muestra cómo implementar la versión de .NET básica del acelerador de soluciones de supervisión remota:

```cmd/sh
pcs -t remotemonitoring -s basic -r dotnet
```

### <a name="example-deploy-java-version"></a>Ejemplo: Implementación de la versión de Java

En el ejemplo siguiente, se muestra cómo implementar la versión de Java estándar del acelerador de soluciones de supervisión remota:

```cmd/sh
pcs -t remotemonitoring -s standard -r java
```

### <a name="pcs-command-options"></a>Opciones del comando pcs

Al ejecutar el comando `pcs` para implementar una solución, se le pide:

- Un nombre para la solución. El nombre debe ser único.
- La suscripción de Azure que se va a usar.
- Una ubicación.
- Las credenciales de las máquinas virtuales que hospedan los microservicios. Puede usar estas credenciales para tener acceso a las máquinas virtuales de cara a la solución de problemas.

Cuando el comando `pcs` finaliza, muestra la dirección URL del nuevo acelerador de soluciones. El comando `pcs` también crea un archivo `{deployment-name}-output.json` que contiene información como el nombre de la instancia de IoT Hub que se creó.

Para más información sobre los parámetros de la línea de comandos, ejecute:

```cmd/sh
pcs -h
```

Para más información sobre la CLI, consulte [Uso de la CLI](https://github.com/Azure/pcs-cli/blob/master/README.md).

## <a name="next-steps"></a>Pasos siguientes

En esta guía paso a paso, ha aprendido lo siguiente:

> [!div class="checklist"]
> * Configuración del acelerador de la solución
> * Implementación del acelerador de solución
> * Inicio de sesión en el acelerador de la solución

Ahora que ha implementado la solución de supervisión remota, el paso siguiente es [explorar las funcionalidades del panel de soluciones](./quickstart-remote-monitoring-deploy.md).

<!-- Next how-to guides in the sequence -->