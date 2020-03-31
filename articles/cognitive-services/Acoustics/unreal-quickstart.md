---
title: Inicio rápido de Project Acoustics con Unreal
titlesuffix: Azure Cognitive Services
description: Utilice el contenido de ejemplo para experimentar con los controles de diseño de Project Acoustics en Unreal y Wwise, y realice la implementación en el escritorio de Windows.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: quickstart
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: d3afcded894f72626a4f24bcbe85c34ac1329c29
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "72242919"
---
# <a name="project-acoustics-unrealwwise-quickstart"></a>Inicio rápido de Project Acoustics en Unreal y Wwise
En este inicio rápido, experimentará con los controles de diseño de Project Acoustics mediante el contenido de ejemplo para Unreal Engine y Wwise.

Requisitos de software para usar el contenido de ejemplo:
* [Unreal Engine](https://www.unrealengine.com/) 4.22
* [AudioKinetic Wwise](https://www.audiokinetic.com/products/wwise/) 2019.1.2

## <a name="download-the-sample-package"></a>Descarga del paquete de ejemplo
Descargue el [paquete de ejemplo Project Acoustics Unreal and Wwise](https://www.microsoft.com/download/details.aspx?id=58090). El paquete de ejemplo contiene:
- Proyecto de Unreal Engine
- Proyecto de Wwise para el proyecto de Unreal
- Complemento de Project Acoustics para Wwise

## <a name="set-up-the-project-acoustics-sample-project"></a>Configuración del proyecto de ejemplo de Project Acoustics
En primer lugar, instale el complemento de Project Acoustics en Wwise. A continuación, implemente los archivos binarios de Wwise en el proyecto de Unreal. Después, ajuste el complemento Unreal de Wwise para la compatibilidad con Project Acoustics.

### <a name="install-the-project-acoustics-wwise-plug-in"></a>Instalación del complemento Wwise para Project Acoustics
Abra Wwise Launcher. En la pestaña **Plugins** (Complementos), en **Install New Plugins** (Instalar nuevos complementos), seleccione **Add From Directory** (Agregar del directorio). Elija el directorio *AcousticsWwisePlugin\ProjectAcoustics* incluido en el paquete que descargó.

![Opción para instalar el complemento Wwise en Wwise Launcher](media/wwise-install-new-plugin.png)

### <a name="add-wwise-binaries-to-the-project-acoustics-unreal-sample-project"></a>Adición de archivos binarios de Wwise al proyecto de ejemplo de Unreal para Project Acoustics
1. En Wwise Launcher, seleccione la pestaña **Unreal Engine**. 
1. Seleccione el menú de tres barras (icono) situado junto a **Recent Unreal Engine Projects** (Proyectos de Unreal Engine recientes) y, a continuación, seleccione **Browse for project** (Examinar proyecto). Abra el archivo *.uproject* del proyecto de ejemplo de Unreal del paquete *AcousticsSample\AcousticsGame\AcousticsGame.uproject*.

   ![Pestaña Unreal en Wwise Launcher](media/wwise-unreal-tab.png)

3. Junto al proyecto de ejemplo de Project Acoustics, seleccione **Integrate Wwise in Project** (Integrar Wwise en el proyecto).

   ![Wwise Launcher muestra el proyecto Acoustics Game de Unreal con la opción Integrate (Integrar) resaltada.](media/wwise-acoustics-game-project.png)

### <a name="extend-wwise-unreal-plug-in-functionality"></a>Ampliación de la funcionalidad del complemento de Wwise para Unreal
El complemento Unreal de Project Acoustics necesita que se exponga comportamiento adicional de la API del complemento Unreal de Wwise. Ejecute el archivo por lotes incluido con el complemento Unreal de Project Acoustics para automatizar estas modificaciones.
* Dentro de *AcousticsGame\Plugins\ProjectAcoustics\Resources*, ejecute *PatchWwise.bat*.

    ![Script para revisar el proyecto Wwise resaltado en una ventana del Explorador de Windows](media/patch-wwise-script.png)

* Si no tiene instalado el SDK de DirectX: En función de la versión de Wwise que use, es posible que tenga que convertir en comentario la línea que contiene `DXSDK_DIR` en *AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs*:

    ![El editor de código muestra "DXSDK" convertido en comentario](media/directx-sdk-comment.png)

* Si realiza la compilación con Visual Studio 2019: Para solucionar un error de vinculación con Wwise, cambie manualmente el valor predeterminado *VSVersion* de *AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs* a **vc150**:

    ![El editor de código con VSVersion cambiado a "vc150"](media/vsversion-comment.png)

### <a name="open-the-unreal-project"></a>Apertura del proyecto de Unreal 
Al abrir el proyecto de Unreal, se le pedirá que vuelva a generar los módulos. Seleccione **Sí**.

Si no se puede abrir el proyecto debido a errores de compilación, compruebe que ha instalado el complemento Wwise de Project Acoustics en la misma versión de Wwise que se usó en el proyecto de ejemplo Project Acoustics.

Si usa una versión de [AudioKinetic Wwise](https://www.audiokinetic.com/products/wwise/) anterior a la versión 2019.1, no puede generar bancos de sonido mediante el proyecto de ejemplo Project Acoustics. Debe integrar la versión 2019.1 de Wwise en el proyecto de ejemplo.

## <a name="experiment-with-project-acoustics-design-controls"></a>Experimentación con los controles de diseño de Project Acoustics
Para escuchar cómo suena la escena, seleccione el botón de reproducción en el editor de Unreal. Use las teclas W, A, S, D y el mouse para desplazarse. Para ver los métodos abreviados de teclado de otros controles, seleccione F1.

La siguiente información describe algunas actividades de diseño que se pueden probar.

### <a name="modify-occlusion-and-transmission"></a>Modificación de la oclusión y la transmisión
En cada actor de sonido de Unreal, hay controles de diseño de Project Acoustics por cada fuente.

![Controles de diseño de acústica del editor de Unreal](media/demo-scene-sound-source-design-controls.png)

Si el valor de **Occlusion Multiplier** (Multiplicador de oclusión) es mayor que 1 (el valor predeterminado es 1), se exagera la oclusión. Con un valor menor que 1, el efecto de oclusión se hace más sutil.

Para permitir la transmisión por la pared, aleje el control deslizante de **Transmisión Db** (Db de transmisión) de su nivel más bajo.

### <a name="modify-wetness-for-a-source"></a>Modificación del sonido con efectos de una fuente
Para cambiar la rapidez con la que el sonido con efectos varía en función de la distancia, use **Perceptual Distance Warp** (Ajuste del salto perceptual). Project Acoustics calcula los niveles de los efectos en todo el espacio por medio de una simulación. Estos niveles van variando suavemente con la distancia y proporcionan indicaciones para la percepción de la distancia. Para exagerar este efecto, aumente los niveles de los efectos relacionados con la distancia; esto aumenta la distorsión por distancia. Los valores de distorsión inferiores a 1 hacen que el cambio en la reverberación basada en la distancia sea más sutil. También puede realizar ajustes más precisos en este efecto mediante el valor **Wetness Adjustment** (Ajuste de sonidos con efectos).

Para aumentar el tiempo de decadencia a lo largo del espacio, ajuste el valor de **Decay Time Multiplier** (Multiplicador del tiempo de decadencia). Considere el caso donde el resultado de la simulación es un tiempo de decadencia de 1,5 segundos. Al establecer **Decay Time Multiplier** (Multiplicador del tiempo de decadencia) en 2, el resultado será la aplicación a la fuente de un tiempo de decadencia de 3 segundos.

### <a name="modify-distance-based-attenuation"></a>Modificación de la atenuación basada en la distancia
El complemento mezclador de Wwise para Project Acoustics respeta la atenuación basada en la distancia por fuente integrada en Wwise. Al cambiar esta curva, cambia el nivel de vía de sonido sin efectos. El complemento de Project Acoustics ajustará el nivel de sonido con efectos para mantener la mezcla efectos-sin efectos especificada por la simulación y los controles de diseño.

![El panel de curva de atenuación de Wwise muestra la atenuación a 0 antes del límite de simulación](media/demo-sounds-attenuation.png)

Project Acoustics realiza el cálculo en un cuadro de "región de simulación" centrado alrededor de la ubicación de cada jugador simulado. Los recursos acústicos del paquete de ejemplo se simularon mediante "bake" con un radio en la región de simulación de 45 metros. Las atenuaciones se diseñaron para descender hasta 0 antes de los 45 metros. Aunque este descenso no es un requisito estricto, tiene la salvedad de que solo la geometría a 45 metros del oyente ocluirá los sonidos.

## <a name="next-steps"></a>Pasos siguientes
* [Integre el complemento de Project Acoustics](unreal-integration.md) en el proyecto de Unreal.
* [Cree una cuenta de Azure](create-azure-account.md) para sus propias simulaciones.
