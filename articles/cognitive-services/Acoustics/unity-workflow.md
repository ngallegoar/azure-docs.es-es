---
title: Tutorial de diseño de Project Acoustics en Unity
titlesuffix: Azure Cognitive Services
description: En este tutorial se describe el flujo de trabajo de diseño de Project Acoustics en Unity.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: fd00e4105ce4edae9d014df2a83c5ae3aaf778da
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "68854261"
---
# <a name="project-acoustics-unity-design-tutorial"></a>Tutorial de diseño de Project Acoustics en Unity
En este tutorial se describen las herramientas y el flujo de trabajo de diseño de Project Acoustics en Unity.

Requisitos previos:
* Unity 2018.2+ para Windows
* Escena de Unity con un recurso acústico con simulación acústica mediante "bake"

En este tutorial, tiene dos maneras de obtener una escena de Unity con un recurso acústico con simulación acústica mediante "bake":
* [Agregar Project Acoustics al proyecto de Unity](unity-integration.md), luego [obtener una cuenta de Azure Batch](create-azure-account.md) y después [realizar la simulación mediante "bake" de la escena de Unity](unity-baking.md)
* O bien, puede usar el [contenido de ejemplo de Project Acoustics para Unity](unity-quickstart.md).

## <a name="review-design-process-concepts"></a>Revisión de los conceptos del proceso de diseño
Project Acoustics emplea métodos comunes de procesamiento de señales digitales (DSP) de audio para procesar sus fuentes, y le proporciona control sobre propiedades acústicas conocidas, como oclusión, mezcla de sonidos con efectos/sin efectos y longitud de cola de reverberación (RT60). Pero el [concepto sobre el que se asienta el proceso de diseño de Project Acoustics](design-process.md) es que en lugar de configurar estas propiedades directamente, puede controlar cómo se usan los resultados de la simulación para determinar estas propiedades. La configuración predeterminada de cada control representa una acústica físicamente precisa.

## <a name="design-acoustics-for-each-source"></a>Diseño de la acústica de cada fuente
Project Acoustics proporciona una serie de controles de diseño acústico específicos de la fuente. Esto le permite controlar la mezcla en una escena y hacer que algunas fuentes destaquen más que otras.

### <a name="adjust-distance-based-attenuation"></a>Ajuste de la atenuación basada en la distancia
El DSP de audio que proporciona el complemento espacializador de **Project Acoustics** para Unity respeta la atenuación basada en la distancia de cada origen, integrada en el editor de Unity. Los controles de la atenuación basada en la distancia están en el componente **Audio Source** (Origen de datos) que se encuentra en el panel **Inspector** de orígenes de sonido, en **3D Sound Settings** (Ajustes de sonido 3D):

![Captura de pantalla del panel de opciones de atenuación de distancia de Unity](media/distance-attenuation.png)

Acoustics realiza el cálculo en un cuadro de "región de simulación" centrado en la ubicación del reproductor. Si un origen de sonido está lejos del reproductor, ubicado fuera de esta región de simulación, solo la geometría dentro del cuadro afectará a la propagación del sonido (por ejemplo, provocará una oclusión), lo que funciona razonablemente bien cuando hay elementos oclusores próximos al reproductor. Pero en casos en los que el reproductor está en un espacio abierto pero hay elementos oclusores cerca de la fuente de sonido lejana, el sonido se puede desbloquear de forma poco realista. En estos casos, la solución alternativa que se recomienda es garantizar que la atenuación del sonido descienda a 0 a aproximadamente 45 m, la distancia horizontal predeterminada del reproductor con respecto el borde del cuadro.

![Captura de pantalla del panel de opciones de SpeakerMode de Unity](media/speaker-mode.png)

### <a name="adjust-occlusion-and-transmission"></a>Ajuste de la oclusión y la transmisión
Adjuntar el script **AcousticsAdjust** a un origen permite ajustar los parámetros de ese origen. Para adjuntar el script, haga clic en **Add Component** (Agregar componente) en la parte inferior del panel **Inspector** y navegue a **Scripts > Acoustics Adjust** (Scripts > Ajuste de acústica). El script tiene seis controles:

![Captura de pantalla del script AcousticsAdjust de Unity](media/acoustics-adjust.png)

* **Enable Acoustics** (Habilitación de acústica): controla si se aplica acústica a este origen. Cuando está opción está desactivada, el origen se espacializa con HRTF o se panoramiza, pero no habrá ninguna acústica. Es decir, no hay obstrucción, oclusión o parámetros de reverberación dinámica, como nivel y tiempo de decadencia. La reverberación se sigue aplicando con un nivel fijo un tiempo de decadencia.
* **Occlusion** (Oclusión): aplica un multiplicador al nivel de decibelios de oclusión calculado por el sistema acústico. Si este multiplicador es mayor que 1, la oclusión se exagera, mientras que los valores menores de 1 hacen que el efecto de oclusión sea más sutil, y un valor de 0 deshabilita la oclusión.
* **Transmission (dB)** (Transmisión (decibelios)): establece la atenuación (en decibelios) causada por la transmisión a través de la geometría. Establezca este control deslizante en su nivel más bajo para deshabilitar la transmisión. Acoustics espacializa el audio original inicial cuando se aproxima a la geometría de la escena (creación de portales). La transmisión proporciona una llegada adicional del sonido original que se espacializa en la dirección de la línea de visión. Tenga en cuenta que también se aplica la curva de atenuación de distancia para el origen.

### <a name="adjust-reverberation"></a>Ajuste de la reverberación
* **Wetness (dB)** (Sonido con efectos (decibelios)): ajusta la potencia de reverberación, en decibelios, según la distancia con respecto al origen. Los valores positivos emiten un sonido con más reverberación, mientras que los valores negativos emiten un sonido más seco. Haga clic en el control de curva (la línea de color verde) para abrir el editor de curvas. Para modificar la curva, haga clic para agregar puntos y arrástrelos para formar la función que quiera. El eje x es la distancia desde el origen, y el eje y es el ajuste de la reverberación en decibelios. Para obtener más información sobre la edición de curvas, vea este [manual de Unity](https://docs.unity3d.com/Manual/EditingCurves.html). Para restablecer la curva al valor predeterminado, haga clic con el botón derecho en **Wetness** (Sonido con efectos) y seleccione **Reset** (Restablecer).
* **Decay Time Scale** (Escala de tiempo de decadencia): ajusta un multiplicador para el tiempo de decadencia. Por ejemplo, si el resultado de la elaboración especifica un tiempo de decadencia de 750 milisegundos, pero este valor se establece en 1,5, el tiempo de decadencia aplicado en el origen es de 1125 milisegundos.
* **Outdoorness** (En exteriores): un ajuste aditivo de la estimación del sistema acústico sobre cómo debe sonar la reverberación en un origen "en exteriores". Si este valor se establece en 1, el origen siempre sonará como si estuviera totalmente en el exterior, mientras que si se establece en -1, sonará como si estuviera en el interior.

Adjuntar el script **AcousticsAdjustExperimental** a un origen habilita parámetros de ajuste experimentales adicionales para ese origen. Para adjuntar el script, haga clic en **Add Component** (Agregar componente) en la parte inferior del panel **Inspector** y navegue a **Scripts > Acoustics Adjust Experimental** (Scripts > Ajuste de acústica experimental). Actualmente hay un control experimental:

![Captura de pantalla del script AcousticsAdjustExperimental de Unity](media/acoustics-adjust-experimental.png)

* **Perceptual Distance Warp** (Distorsión de la distancia perceptual): aplica una distorsión exponencial a la distancia que se usa para calcular la proporción entre el sonido original y con efectos. El sistema acústico calcula los niveles de efecto en todo el espacio, que varían ligeramente con la distancia y proporcionan indicaciones de distancia perceptual. Los valores de distorsión superiores a 1 exageran este efecto al aumentar los niveles de reverberación relacionados con la distancia, y hacen que el sonido sea "distante". Los valores de distorsión inferiores a 1 hacen que el cambio de reverberación basado en la distancia sea más sutil, haciendo que el sonido sea más "presente".

## <a name="design-acoustics-for-all-sources"></a>Diseño de la acústica de todas las fuentes
Para ajustar los parámetros de todos los orígenes, haga clic en la fila de canales de **Audio Mixer** (Mezclador de audio) de Unity y ajuste los parámetros en el efecto **Project Acoustics Mixer** (Mezclador acústico del proyecto).

![Captura de pantalla del panel de personalización del mezclador de Project Acoustics para Unity](media/mixer-parameters.png)

* **Wetness Adjust** (Ajuste de humedad): ajusta la potencia de reverberación, en dB, en todos los orígenes de la escena en función de la distancia a la que se sitúa el agente de escucha de origen. Los valores positivos emiten un sonido con más reverberación, mientras que los valores negativos emiten un sonido más seco.
* **RT60 Scale** (Escala RT60): escalar multiplicativo para el tiempo de reverberación.
* **Use Panning** (Uso de panoramización): controlea si el sonido se emite como binaural (0) o con panoramización multicanal (1). Cualquier valor que sea diferente a 1 indica binaural. La salida binaural se espacializa con HRTF para su uso con auriculares y la salida multicanal se espacializa con VBAP para su uso con sistemas de altavoces de sonido envolvente multicanal. Si usa la panoramización multicanal, no olvide seleccionar el modo del altavoz que coincida con la configuración del dispositivo, lo encontrará en **Configuración del proyecto** > **Audio**.

## <a name="check-proper-sound-source-placement"></a>Comprobación de la colocación adecuada de la fuente de sonido
Las fuentes de sonido colocadas dentro de vóxels ocupados no recibirán tratamiento acústico. Como los vóxels se extienden más allá de la geometría de escena visible, se puede colocar una fuente dentro de un vóxel y que aparezca sin ocluir por la geometría visual. Para ver los vóxels de Project Acoustics, active o desactive la casilla de cuadrícula del menú **Gizmos**, en la parte superior derecha de la vista **Scene** (Escena).

![Captura de pantalla del menú Gizmos de Unity](media/gizmos-menu.png)  

La visualización del vóxel puede ayudar a determinar si los componentes visuales del juego tienen aplicada una transformación. Si es así, aplique la misma transformación al GameObject que hospeda al **Administrador de acústica**.

### <a name="bake-time-vs-run-time-voxels"></a>Vóxels de tiempo de simulación mediante "bake" frente a vóxels de tiempo de ejecución
Se pueden ver los vóxels en la ventana del editor durante el diseño del juego y en la ventana del juego en tiempo de ejecución. El tamaño de los vóxels es diferente en estas vistas. Esto se debe a que la interpolación del tiempo de ejecución de la acústica usa una cuadrícula de vóxels más precisa para obtener resultados de interpolación más perfectos. La colocación de la fuente de sonido se debe comprobar mediante los vóxels de tiempo de ejecución.

Vóxeles en tiempo de diseño:

![Captura de pantalla de vóxeles en tiempo de diseño de Project Acoustics](media/voxels-design-time.png)

Vóxeles en tiempo de ejecución:

![Captura de pantalla de vóxeles en tiempo de ejecución de Project Acoustics](media/voxels-runtime.png)

## <a name="next-steps"></a>Pasos siguientes
* Explore casos prácticos donde se resaltan los conceptos que subyacen al [proceso de diseño](design-process.md).

