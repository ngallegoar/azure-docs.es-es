---
title: 'Inicio rápido: Configuración del seguimiento corporal de Azure Kinect'
description: En este inicio rápido, configurará el SDK de Body Tracking para Azure Kinect.
author: qm13
ms.author: quentinm
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: quickstart
keywords: kinect, azure, sensor, access, depth, sdk, body, tracking, joint, setup, cuda, nvidia
ms.openlocfilehash: 2cf4c1097730f88fc4bd66c28e1bdddd7fea8640
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "85277847"
---
# <a name="quickstart-set-up-azure-kinect-body-tracking"></a>Inicio rápido: Configuración del seguimiento corporal de Azure Kinect

Este inicio rápido le guiará a través del proceso de ejecución del seguimiento corporal en Azure Kinect DK.

## <a name="system-requirements"></a>Requisitos del sistema

El SDK de Body Tracking requiere una GPU NVIDIA instalada en el equipo host. El requisito de equipo host de seguimiento corporal recomendado se describe en la página de [requisitos del sistema](system-requirements.md).

## <a name="install-software"></a>Instalación del software

### <a name="install-the-latest-nvidia-driver"></a>[Instalación del controlador NVIDIA más reciente](https://www.nvidia.com/Download/index.aspx?lang=en-us)

Descargue e instale el controlador NVIDIA más reciente de la tarjeta gráfica. Es posible que los controladores antiguos no sean compatibles con los archivos binarios de CUDA redistribuidos con el SDK de Body Tracking.

### <a name="visual-c-redistributable-for-visual-studio-2015"></a>[Visual C++ Redistributable para Visual Studio 2015](https://www.microsoft.com/en-us/download/details.aspx?id=48145)

Descargue e instale Visual C++ Redistributable para Visual Studio 2015. 

## <a name="set-up-hardware"></a>Configuración del hardware

### <a name="set-up-azure-kinect-dk"></a>[Configuración de Azure Kinect DK](set-up-azure-kinect-dk.md)

Inicie el [visor de Azure Kinect](azure-kinect-viewer.md) para comprobar que Azure Kinect DK está configurado correctamente.

## <a name="download-the-body-tracking-sdk"></a>Descarga del SDK de Body Tracking
 
1. Seleccione el vínculo para [descargar el SDK de Body Tracking](body-sdk-download.md).
2. Instale el SDK de Body Tracking en el equipo.

## <a name="verify-body-tracking"></a>Comprobación del seguimiento corporal

Inicie el **visor de seguimiento corporal de Azure Kinect** para comprobar que el SDK de Body Tracking está configurado correctamente. El visor se instala con el instalador MSI del SDK. Puede encontrarlo en el menú Inicio o en `<SDK Installation Path>\tools\k4abt_simple_3d_viewer.exe`.

Si no dispone de una GPU suficientemente eficaz, pero desea probar el resultado, puede iniciar el **visor de seguimiento corporal de Azure Kinect**  en la línea de comandos con el siguiente comando: `<SDK Installation Path>\tools\k4abt_simple_3d_viewer.exe CPU`

Si todo está configurado correctamente, aparecerá una ventana con una nube de puntos 3D y los cuerpos que se han seguido.


![Visor 3D de seguimiento corporal](./media/quickstarts/samples-simple3dviewer.png)

## <a name="examples"></a>Ejemplos

[Aquí](https://github.com/microsoft/Azure-Kinect-Samples/tree/master/body-tracking-samples) puede encontrar los ejemplos sobre cómo usar el SDK de seguimiento corporal.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
>[Creación de la primera aplicación de seguimiento corporal](build-first-body-app.md)

