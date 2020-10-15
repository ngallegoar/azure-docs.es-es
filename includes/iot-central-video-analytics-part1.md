---
title: archivo de inclusión
description: archivo de inclusión
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 10/06/2020
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 7246375468b3419c3d52ee3d5a51a95aa20050a9
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91876722"
---
La aplicación de ejemplo incluye dos dispositivos simulados y una puerta de enlace IoT Edge. En los siguientes tutoriales se muestran dos enfoques para experimentar y comprender las funcionalidades de la puerta de enlace:

* Creación de una puerta de enlace IoT Edge en una máquina virtual de Azure y conexión de una cámara simulada.
* Creación de una puerta de enlace IoT Edge en un dispositivo real, como un Intel NUC y conexión de una cámara real.

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Usar la plantilla de aplicación análisis de vídeo de Azure IoT Central para crear una aplicación de comercio minorista
> * Personalizar la configuración de la aplicación
> * Crear una plantilla de dispositivo para un dispositivo de puerta de enlace IoT Edge
> * Agregar un dispositivo de puerta de enlace a la aplicación de IoT Central

## <a name="prerequisites"></a>Prerrequisitos

Para completar esta serie de tutoriales, necesitará lo siguiente:

* Suscripción a Azure. Si no tiene una suscripción de Azure, puede crear una en la [página de suscripción a Azure](https://aka.ms/createazuresubscription).
* Si usa una cámara real, necesita conectividad entre el dispositivo IoT Edge y la cámara y necesita el canal **Protocolo de streaming en tiempo real**.

## <a name="initial-setup"></a>Instalación inicial

En estos tutoriales, se actualizan y se usan varios archivos de configuración. Las versiones iniciales de estos archivos están disponibles en el repositorio de GitHub [LVA-gateway](https://github.com/Azure/live-video-analytics/tree/master/ref-apps/lva-edge-iot-central-gateway). El repositorio también incluye un archivo de texto para anotaciones rápidas que puede descargar y usar para registrar los valores de configuración de los servicios que implemente.

Cree una carpeta llamada *lva-configuration* en el equipo local para guardar copias de estos archivos. A continuación, haga clic con el botón derecho en cada uno de los vínculos siguientes y elija **Guardar como** para guardar el archivo en la carpeta *lva-configuration*:
