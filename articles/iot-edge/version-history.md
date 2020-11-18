---
title: 'Navegación e historial de versiones de IoT Edge: Azure IoT Edge'
description: Descubra las novedades de IoT Edge con información sobre las nuevas características y funcionalidades de las versiones más recientes.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/08/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: c5d66355cd68aaaa5f80e938bcacb1b59d12eb70
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94447499"
---
# <a name="azure-iot-edge-versions-and-release-notes"></a>Versiones de Azure IoT Edge y notas de la versión

Azure IoT Edge es un producto creado a partir del proyecto de IoT Edge de código abierto hospedado en GitHub. Todas las versiones nuevas están disponibles en el [proyecto de Azure IoT Edge](https://github.com/Azure/azure-iotedge). Las contribuciones y los informes de errores se pueden realizar en el [proyecto de IoT Edge de código abierto](https://github.com/Azure/iotedge).

## <a name="documented-versions"></a>Versiones documentadas

La documentación de IoT Edge de este sitio está disponible para dos versiones diferentes del producto, de modo que pueda elegir el contenido que se aplica a su entorno de IoT Edge. Actualmente, las versiones compatibles son:

* **IoT Edge 1.0.10** cubre todas las características y funcionalidades hasta la versión más reciente disponible con carácter general: [1.0.10](https://github.com/Azure/azure-iotedge/releases/tag/1.0.10)
* **IoT Edge 1.2 (versión preliminar)** incluye contenido adicional para las características y funcionalidades que se encuentran en la versión preliminar más reciente: [1.2-rc1](https://github.com/Azure/azure-iotedge/releases/tag/1.2-rc1)
  * Mientras IoT Edge 1.2 está en versión preliminar, debe instalar las versiones candidatas para lanzamiento. Para obtener más información, consulte la [Instalación sin conexión o de una versión específica](how-to-install-iot-edge.md?tabs=linux#offline-or-specific-version-installation).

## <a name="version-history"></a>Historial de versiones

En esta tabla se proporciona el historial de versiones recientes de los paquetes de IoT Edge, y se destacan las actualizaciones de documentación realizadas para cada versión.

| Notas de la versión y recursos | Tipo | Date | Aspectos destacados |
| ------------------------ | ---- | ---- | ---------- |
| [1.2-rc1](https://github.com/Azure/azure-iotedge/releases/tag/1.2-rc1) | Versión preliminar | Noviembre de 2020 | [Dispositivos de IoT Edge detrás de puertas de enlace](how-to-connect-downstream-iot-edge-device.md?view=iotedge-2020-11&preserve-view=true)<br>[Agente MQTT de IoT Edge](how-to-publish-subscribe.md?view=iotedge-2020-11&preserve-view=true) |
| [1.0.10](https://github.com/Azure/azure-iotedge/releases/tag/1.0.10) | Stable | Octubre de 2020 | [Método directo UploadSupportBundle](how-to-retrieve-iot-edge-logs.md#upload-support-bundle-diagnostics)<br>[Carga de métricas en tiempo de ejecución](how-to-access-built-in-metrics.md)<br>[Enrutamiento de prioridad y período de vida](module-composition.md#priority-and-time-to-live)<br>[Orden de inicio de módulos](module-composition.md#configure-modules)<br>[Aprovisionamiento manual de X.509](how-to-manual-provision-x509.md) |
| [1.0.9](https://github.com/Azure/azure-iotedge/releases/tag/1.0.9) | Stable | Marzo de 2020 | [Aprovisionamiento automático de X.509 con DPS](how-to-auto-provision-x509-certs.md)<br>[Método directo RestartModule](how-to-edgeagent-direct-method.md#restart-module)<br>[Comando support-bundle](troubleshoot.md#gather-debug-information-with-support-bundle-command) |

## <a name="next-steps"></a>Pasos siguientes

* [Consulte todas las versiones de Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases)
* [Cree o revise solicitudes de características en el foro de comentarios](https://feedback.azure.com/forums/907045-azure-iot-edge)