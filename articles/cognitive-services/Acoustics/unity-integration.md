---
title: Integración e implementación de Unity para Project Acoustics
titlesuffix: Azure Cognitive Services
description: En este artículo se describe cómo integrar el complemento de Unity para Project Acoustics en un proyecto de Unity.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: a8ddb0e4ca2ee4396a25a70c8b60b653aebb72d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "72243012"
---
# <a name="project-acoustics-unity-integration"></a>Integración de Unity para Project Acoustics
En este artículo se describe cómo integrar el complemento de Unity para Project Acoustics en un proyecto de Unity.

Requisitos de software:
* [Unity 2018.2+](https://unity3d.com) para Windows
* [Paquete de Unity de Project Acoustics](https://www.microsoft.com/download/details.aspx?id=57346)

## <a name="import-the-plug-in"></a>Importación del complemento
1. Importe el paquete UnityPackage de Acoustics en el proyecto. 
 En Unity, vaya a **Assets** > **Import Package** > **Custom Package** (Recursos > Importar paquete > Paquete personalizado).

    ![Menú Import Package (Importar paquete) de Unity](media/import-package.png)  

1. Elija **ProjectAcoustics.unitypackage**.

1. Seleccione el botón **Import** (Importar) para integrar el paquete de Unity en el proyecto.

    ![Cuadro de diálogo Unity Import Package (Importar paquete de Unity)](media/import-dialog.png)  

Si va a importar el complemento a un proyecto existente, es posible que el proyecto ya tenga un archivo *mcs.rsp* en la carpeta raíz del proyecto. Este archivo especifica distintas opciones para el compilador de C#. Combine el contenido de ese archivo con el archivo mcs.rsp que se incluye con el complemento de Project Acoustics.

## <a name="enable-the-plug-in"></a>Habilitación del complemento
La parte de elaboración del kit de herramientas de elementos acústicos requiere la versión del entorno de ejecución de scripting .NET 4.*x*. La importación del paquete actualiza la configuración del reproductor de Unity. Reinicie Unity para que esta configuración surta efecto.

![Panel de configuración del reproductor de Unity](media/player-settings.png)

![Panel de configuración del reproductor de Unity con la versión de 4.5 de .NET seleccionada](media/net45.png)

## <a name="set-up-audio-dsp"></a>Configuración del DSP de audio
Project Acoustics incluye un DSP de audio en tiempo de ejecución que se integra en el marco del espacializador del motor de audio de Unity. Proporciona tanto espacialización de panoramización como basada en HRTF. Para habilitar el DSP de Project Acoustics, vaya a **Edit** > **Project Settings** > **Audio** (Editar > Configuración del proyecto > Audio) para abrir la configuración de audio de Unity. Seleccione **Project Acoustics** en **Spatializer Plugin** (Complemento espacializador) para el proyecto. Asegúrese de que en **DSP Buffer Size** (Tamaño de búfer de DSP) se ha seleccionado *Best performance* (Rendimiento óptimo).

![Menú Project Settings (Configuración del proyecto) de Unity](media/project-settings.png)  

![Panel de configuración del espacializador de Unity con el espacializador de Project Acoustics seleccionado](media/choose-spatializer.png)

A continuación, abra el mezclador de audio en **Window** > **Audio Mixer** (Ventana > Mezclador de audio). Asegúrese de tener al menos un mezclador con un grupo. Si no lo tiene, seleccione el botón **+** que está a la derecha de **Mixers** (Mezcladores). Haga clic con el botón secundario en la parte inferior de la fila de canales en la sección de efectos y agregue el efecto **Microsoft Acoustics Mixer** (Mezclador de Microsoft Acoustics). No se admite más de un mezclador de Project Acoustics a la vez.

![Mezclador de audio de Unity que hospeda el mezclador de Project Acoustics](media/audio-mixer.png)

## <a name="enable-acoustics-on-sound-sources"></a>Habilitación de la acústica en los orígenes de sonido
Cree un origen de audio: Active la casilla **Spatialize** (Espacializar) en la parte inferior del panel del inspector de AudioSource. Asegúrese de que en **Spatial Blend** (Combinación espacial) se ha seleccionado *3D*.  

![Panel de origen de audio de Unity](media/audio-source.png)

## <a name="enable-acoustic-design"></a>Habilitación del diseño acústico
Adjunte el script *AcousticsAdjust* a un origen de sonido de la escena para habilitar otros parámetros de diseño de origen adicionales: Seleccione **Add Component** (Agregar componente) y elija **Scripts** > **AcousticsAdjust**.

![Script AcousticsAdjust de Unity](media/acoustics-adjust.png)

## <a name="next-steps"></a>Pasos siguientes
* [Simulación mediante "bake" de la escena con Project Acoustics para Unity](unity-baking.md).
* [Creación de una cuenta de Azure Batch](create-azure-account.md) para realizar una simulación mediante "bake" de la escena en la nube.
* Explore el [proceso de diseño de Unity para Project Acoustics](unity-workflow.md).
