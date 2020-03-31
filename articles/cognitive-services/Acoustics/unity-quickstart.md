---
title: Inicio rápido de Project Acoustics con Unity
titlesuffix: Azure Cognitive Services
description: Con el contenido de ejemplo, experimente con los controles de diseño de Project Acoustics en Unity y realice la implementación en el escritorio de Windows.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: quickstart
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: fabdd221ef99414eae0156babbd76dedb1f0745d
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "72243018"
---
# <a name="project-acoustics-unity-quickstart"></a>Inicio rápido de Project Acoustics con Unity
Use el contenido de ejemplo de Project Acoustics para Unity para experimentar con los controles de diseño basado en la simulación.

Requisitos de software:
* [Unity 2018.2+](https://unity3d.com) para Windows
* [Paquete de contenido de ejemplo de Project Acoustics](https://www.microsoft.com/download/details.aspx?id=57346)

¿Qué se incluye en el paquete de ejemplo?
* Escena de Unity con geometría, orígenes de sonido y controles de juego
* Complemento de Project Acoustics
* Recursos acústicos de simulación mediante "bake" para la escena

## <a name="import-the-sample-package"></a>Importación del paquete de ejemplo
Importe el paquete de ejemplo en un nuevo proyecto de Unity.
1. En Unity, vaya a **Assets** > **Import Package** > **Custom Package** (Recursos > Importar paquete > Paquete personalizado).

    ![Opciones de importación de paquete de Unity](media/import-package.png)  

1. Elija **ProjectAcoustics.unitypackage**.

1. Seleccione el botón **Import** (Importar) para integrar el paquete de Unity en el proyecto.  
  
    ![Cuadro de diálogo Unity Import Package (Importar paquete de Unity)](media/import-dialog.png)  

Para importar el paquete en un proyecto existente, consulte [Integración con Unitiy](unity-integration.md) para pasos adicionales y notas.

>[!NOTE]
>Una vez completada la importación, aparecerán varios mensajes de error en el registro de la consola. Continúe con el paso siguiente y reinicie Unity.

## <a name="restart-unity"></a>Reinicio de Unity
La parte de elaboración del kit de herramientas de elementos acústicos requiere la versión del entorno de ejecución de scripting .NET 4.*x*. La importación del paquete actualiza la configuración del reproductor de Unity. Reinicie Unity para que esta configuración surta efecto. Para comprobar que la configuración ha surtido efecto, abra la configuración de **Reproductor**:

![Menú Project Settings (Configuración del proyecto) de Unity](media/player-settings.png)  

![Panel de configuración del reproductor de Unity con la versión .NET 4.x seleccionada](media/net45.png)  

>[!NOTE]
>La captura de pantalla pertenece a Unity 2018.*x*. La imagen puede diferir en las versiones más recientes de Unity.

## <a name="open-the-project-acoustics-bake-window"></a>Apertura de la ventana de simulación mediante "bake" de Project Acoustics
En Unity, seleccione **Acoustics** (Acústica) en el menú **Window** (Ventana).

![Editor de Unity con la opción del menú Window (Ventana) Acoustics (Acústica) resaltada](media/window-acoustics.png)

Se abrirá una ventana flotante **Acoustics** (Acústica). En esta ventana se configuran las propiedades de la simulación acústica.

![Editor de Unity con la ventana Acoustics (Acústica) abierta](media/unity-editor-plugin-window.png)  

## <a name="experiment-with-the-design-controls"></a>Experimento con los controles de diseño
Abra la escena de ejemplo de la carpeta *ProjectAcousticsSample* y seleccione el botón de reproducción en el editor de Unity. Use las teclas W, A, S, D y el mouse para desplazarse. Para comparar cómo suena la escena con y sin acústica, seleccione la tecla R hasta que el texto superpuesto cambie a color rojo en indique "Acoustics: Disabled" (Acústica: deshabilitada). Para ver los métodos abreviados de teclado de otros controles, seleccione F1. También puede hacer clic con el botón derecho para seleccionar una acción y, a continuación, con el botón izquierdo para realizarla.

El script *AcousticsAdjust* se adjunta a los orígenes de sonido de la escena de ejemplo. Habilita los parámetros de diseño por origen.

![Script AcousticsAdjust de Unity](media/acoustics-adjust.png)

En las secciones siguientes se exploran algunos de los efectos que se pueden crear mediante los controles disponibles. Para información detallada acerca de cada control, consulte el [Tutorial de diseño de Project Acoustics en Unity](unity-workflow.md).

### <a name="modify-distance-based-attenuation"></a>Modificación de la atenuación basada en la distancia
El procesamiento de señal digital de audio del complemento espacializador de **Project Acoustics** para Unity respeta la atenuación basada en la distancia de cada origen integrada en el editor de Unity. Los controles de la atenuación basada en la distancia están en el componente **Audio Source** (Origen de datos) que se encuentra en el panel **Inspector** (Inspector) de orígenes de sonido en **3D Sound Settings** (Ajustes de sonido 3D):

![Panel de opciones de atenuación de distancia de Unity](media/distance-attenuation.png)

Project Acoustics realiza el cálculo en un cuadro de "región de simulación" centrado alrededor de la ubicación del reproductor. Los recursos acústicos del paquete de ejemplo se han incorporado a un tamaño de región de simulación de 45 metros alrededor del reproductor. Por lo tanto, la atenuación de sonido debe estar diseñada para pasar a 0 a aproximadamente 45 m.

### <a name="modify-occlusion-and-transmission"></a>Modificación de la oclusión y la transmisión
* Si el valor de **Occlusion Multiplier** (Multiplicador de oclusión) es mayor que 1 (el valor predeterminado es 1), se exagera la oclusión. Si se establece en menos de 1, el efecto de oclusión se hace más sutil.

* Para permitir la transmisión por la pared, mueva el control deslizante **Transmisison (dB)** (Transmisión [dB]) lejos de la menor configuración.

### <a name="modify-wetness-for-a-source"></a>Modificación del sonido con efectos de una fuente
* Para cambiar la rapidez con la que el sonido con efectos varía en función de la distancia, use **Perceptual Distance Warp** (Ajuste del salto perceptual). Con la simulación, Project Acoustics calcula los niveles de efecto, lo cual proporciona indicios de la distancia perceptual y varían ligeramente con la distancia. Al aumentar el salto de distancia se exagera este efecto ya que aumentan los niveles de efectos relacionados con la distancia. Los valores de distorsión inferiores a 1 hacen que el cambio en la reverberación basada en la distancia sea más sutil.

   Para realizar ajustes más precisos en este efecto, cambie el valor **Wetness (dB)**  (Sonido con efectos [dB]).

* Para aumentar el tiempo de decadencia a lo largo del espacio mediante el ajuste del valor **Decay Time Scale** (Escala de tiempo de decadencia). Si el resultado de la simulación de un determinado par de ubicación de cliente de escucha de origen es un tiempo de decadencia de 1,5 segundos y el valor de **Decay Time Scale** (Escala de tiempo de decadencia) está establecido en 2, el tiempo de decadencia aplicado en el origen es 3 segundos.

## <a name="next-steps"></a>Pasos siguientes
* Lea los detalles sobre los [controles de diseño de Project Acoustics basados en Unity](unity-workflow.md).
* Explore los conceptos que subyacen al [proceso de diseño](design-process.md).
* [Cree una cuenta de Azure](create-azure-account.md) para explorar los procesos previos a la simulación mediante "bake" y la simulación en sí.
