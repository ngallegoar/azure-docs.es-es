---
title: Descarga de Sensor SDK de Azure Kinect
description: Obtenga información sobre cómo descargar e instalar Sensor SDK de Azure Kinect en Windows y Linux.
author: Brent-A
ms.author: brenta
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: azure, kinect,sdk, download update, latest, available, install
ms.openlocfilehash: 48cdd35a80d68a7ec0d900639c0fca3a2c66787e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86171795"
---
# <a name="azure-kinect-sensor-sdk-download"></a>Descarga de Sensor SDK de Azure Kinect

Esta página incluye vínculos de descarga para cada versión de Sensor SDK de Azure Kinect. El instalador proporciona todos los archivos necesarios para desarrollar para Azure Kinect.

## <a name="azure-kinect-sensor-sdk-contents"></a>Contenido de Sensor SDK de Azure Kinect

- Encabezados y bibliotecas para compilar una aplicación mediante Azure Kinect DK.
- Archivos DLL redistribuibles necesarios para las aplicaciones que usan Azure Kinect DK.
- [Azure Kinect Viewer](azure-kinect-viewer.md)
- [Azure Kinect Recorder](azure-kinect-recorder.md)
- [Azure Kinect Firmware Tool](azure-kinect-firmware-tool.md)

## <a name="windows-download-link"></a>Vínculo de descarga pàra Windows

[Instalador de Microsoft](https://download.microsoft.com/download/3/d/6/3d6d9e99-a251-4cf3-8c6a-8e108e960b4b/Azure%20Kinect%20SDK%201.4.1.exe) | [Código fuente de GitHub](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/1093)

> [!NOTE]
> Cuando instale el SDK, recuerde la ruta de acceso de instalación. Por ejemplo, "C:\Archivos de programa\Azure Kinect SDK 1.2". Encontrará las herramientas a las que se hace referencia en los artículos en esta ruta de acceso.

Puede encontrar versiones anteriores del firmware y de Sensor SDK de Azure Kinect en [GitHub](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/docs/usage.md).

## <a name="linux-installation-instructions"></a>Instrucciones de instalación para Linux

Actualmente, la única distribución admitida es Ubuntu 18.04. Para solicitar soporte técnico para otras distribuciones, consulte [esta página](https://aka.ms/azurekinectfeedback).

En primer lugar, deberá configurar el [repositorio de paquetes de Microsoft](https://packages.microsoft.com/), siguiendo las instrucciones de [este artículo](https://docs.microsoft.com/windows-server/administration/linux-package-repository-for-microsoft-software).

Ahora puede instalar los paquetes necesarios. El paquete `k4a-tools` incluye [Azure Kinect Viewer](azure-kinect-viewer.md), [Azure Kinect Recorder](record-sensor-streams-file.md) y [Azure Kinect Firmware Tool](azure-kinect-firmware-tool.md). Para instalarlo, ejecute

 `sudo apt install k4a-tools`

 El paquete `libk4a<major>.<minor>-dev` contiene los encabezados y los archivos CMake que se van a compilar en `libk4a`.
El paquete `libk4a<major>.<minor>` contiene los objetos compartidos necesarios para ejecutar archivos ejecutables que dependen de `libk4a`.

 Los tutoriales básicos requieren el paquete `libk4a<major>.<minor>-dev`. Para instalarlo, ejecute

 `sudo apt install libk4a1.1-dev`

Si el comando se ejecuta correctamente, el SDK está listo para su uso.

## <a name="change-log-and-older-versions"></a>Registro de cambios y versiones anteriores

Puede encontrar el registro de cambios de Sensor SDK de Azure Kinect [aquí](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/CHANGELOG.md).

Si necesita una versión anterior de Sensor SDK de Azure Kinect, busque [aquí](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/docs/usage.md).

## <a name="next-steps"></a>Pasos siguientes

[Configuración de Azure Kinect DK](set-up-azure-kinect-dk.md)
