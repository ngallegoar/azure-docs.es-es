---
title: Cámara de profundidad de Azure Kinect DK
description: Conozca los principios de funcionamiento y las características principales de la cámara de profundidad de Azure Kinect DK.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: kinect, azure, sensor, sdk, depth camera, tof, principles, performance, invalidation
ms.openlocfilehash: 22f04b983ed7c6a2ab19a5c1c709621655ee31c0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85276955"
---
# <a name="azure-kinect-dk-depth-camera"></a>Cámara de profundidad de Azure Kinect DK

En esta página se describe el uso de la cámara de profundidad de Azure Kinect DK. La cámara de profundidad es la segunda de las dos cámaras. Como se explicó en las secciones anteriores, la otra cámara es la cámara RGB.  

## <a name="operating-principles"></a>Principios de funcionamiento

La cámara de profundidad de Azure Kinect DK implementa el principio de tiempo de vuelo (ToF) de onda continua de amplitud modulada (AMCW). La cámara proyecta la iluminación modulada del espectro del infrarrojo cercano (NIR) sobre la escena. Después, graba una medida indirecta del tiempo que tarda la luz en pasar de la cámara a la escena y volver.

Estas medidas se procesan para generar un mapa de profundidad. Un mapa de profundidad es un conjunto de valores de coordenada Z de cada píxel de la imagen, medidos en unidades de milímetro.

Junto con un mapa de profundidad, también se obtiene la conocida lectura IR limpia. El valor de los píxeles de la lectura IR limpia es proporcional a la cantidad de luz que devuelve la escena. La imagen se parece a una imagen IR normal. En la siguiente ilustración se muestra un mapa de profundidad de ejemplo (izquierda) y la imagen IR limpia correspondiente (derecha).

![Profundidad e IR lado a lado](./media/concepts/depth-camera-depth-ir.png)

## <a name="key-features"></a>Principales características

Algunas de las características técnicas de la cámara de profundidad son:

- Chip de imágenes ToF de 1 megapíxel con tecnología avanzada de píxeles que permite unas frecuencias de modulación más altas y una mayor precisión de profundidad.
- Dos diodos láser NIR que permiten los modos de profundidad de campo de visión (FoV) corto y amplio.
- El píxel ToF más pequeño del mundo, con 3,5 μm por 3,5 μm.
- Selección de ganancia automática por píxel que permite un rango dinámico grande de forma que los objetos cercanos y lejanos se capturan limpiamente.
- Obturador global que permite un mejor rendimiento en condiciones de luz solar.
- Método de cálculo de profundidad de varias fases que permite una gran precisión incluso con distintos chips, láseres y fuentes de alimentación.
- Errores sistemáticos y aleatorios bajos.

![Módulo de profundidad](./media/concepts/depth-camera-depth-module.jpg)

La cámara de profundidad transmite imágenes IR moduladas sin formato al equipo host. En el equipo, el software de motor de profundidad acelerado de GPU convierte la señal sin procesar en mapas de profundidad. La cámara de profundidad admite varios modos. Los modos de **campo de visión (FoV) pequeño** son adecuados para escenas con extensiones más pequeñas en las dimensiones X e Y, pero con extensiones mayores en la dimensión Z. Si la escena tiene grandes extensiones X e Y, pero rangos Z más pequeños, los **modos de FoV amplio** son más adecuados.

La cámara de profundidad admite **modos de discretización de 2 x 2** para ampliar el rango Z en comparación con los **modos sin discretización** correspondientes. La discretización se realiza a costa de reducir la resolución de la imagen. Todos los modos se pueden ejecutar hasta a 30 fotogramas por segundo (FPS), a excepción del modo de 1 megapíxel (MP) que se ejecuta a una velocidad de fotogramas máxima de 15 fps. La cámara de profundidad también proporciona un **modo IR pasivo**. En este modo, los iluminadores de la cámara no están activos y solo se observa iluminación ambiente.

## <a name="camera-performance"></a>Rendimiento de la cámara

El rendimiento de la cámara se mide en errores sistemáticos y aleatorios.

### <a name="systematic-error"></a>Error sistemático

El error sistemático se define como la diferencia entre la profundidad medida después de la eliminación del ruido y la profundidad correcta (verdad terreno). Se calcula el promedio temporal de muchos fotogramas de una escena estática para eliminar lo más posible el ruido de profundidad. Más concretamente, el error sistemático se define como:

![Error sistemático de profundidad](./media/concepts/depth-camera-systematic-error.png)

Donde *d<sub>t</sub>* indica la profundidad de medida en el tiempo *t*, *N* es el número de fotogramas usados en el procedimiento de cálculo del promedio y *d<sub>gt</sub>* es la profundidad de verdad terreno.

La especificación de los errores sistemáticos de la cámara de profundidad excluye la interferencia debida a trayectos múltiples (MPI). MPI se produce cuando un píxel del sensor integra la luz que refleja más de un objeto. Para reducir parcialmente este fenómeno en la cámara de profundidad, se emplean frecuencias de modulación más altas, junto con la invalidación de profundidad, que se tratará más adelante.

### <a name="random-error"></a>Error aleatorio

Supongamos que tomamos 100 imágenes del mismo objeto sin mover la cámara. La profundidad del objeto será ligeramente diferente en cada una de las 100 imágenes. Esta diferencia se debe al ruido del disparo. El ruido del disparo es el número de fotones que llegan al sensor y varía con el tiempo según un factor aleatorio. En una escena estática, este error aleatorio se define como la desviación estándar de la profundidad con el tiempo calculada como:

![Error aleatorio de profundidad](./media/concepts/depth-camera-random-error.png)

Donde *N* indica el número de medidas de profundidad, *d<sub>t</sub>* representa la medida de profundidad en el tiempo *t* y *d* indica el valor medio calculado de todas las medidas de profundidad *d<sub>t</sub>* .

## <a name="invalidation"></a>Invalidación

En determinadas situaciones, es posible que la cámara de profundidad no proporcione los valores correctos de algunos píxeles. En estas situaciones, se invalidan los píxeles de profundidad. Los píxeles no válidos se indican con un valor de profundidad igual a 0. Algunos de los motivos por los que el motor de profundidad no puede generar valores correctos son:

- Estar fuera de la máscara de iluminación de IR activa
- Señal de IR saturada
- Señal de IR baja
- Filtro atípico
- Interferencia debida a trayectos múltiples

### <a name="illumination-mask"></a>Máscara de iluminación

Los píxeles se invalidan cuando están fuera de la máscara de iluminación de IR activa. No se recomienda usar la señal de estos píxeles para calcular la profundidad. En la ilustración siguiente, se muestra el ejemplo de invalidación por la máscara de iluminación. Los píxeles invalidados son los píxeles de color negro situados fuera del círculo en los modos de FoV amplio (izquierda) y el hexágono en los modos de FoV corto (derecha).

![Invalidación por estar fuera de la máscara de iluminación](./media/concepts/depth-camera-invalidation-illumination-mask.png)

### <a name="signal-strength"></a>Intensidad de la señal

Los píxeles se invalidan cuando contienen una señal de IR saturada. Cuando los píxeles están saturados, se pierde la información de fase. En la imagen siguiente se muestra el ejemplo de invalidación por una señal de IR saturada. Vea las flechas que apuntan a los píxeles de ejemplo en las imágenes de profundidad e IR.

![Invalidación por saturación](./media/concepts/depth-camera-invalidation-saturation.png)

La invalidación también puede producirse cuando la señal de IR no es lo suficientemente fuerte como para generar profundidad. En la ilustración siguiente, se muestra el ejemplo de invalidación por una señal de IR baja. Vea las flechas que apuntan a los píxeles de ejemplo en las imágenes de profundidad e IR.

![Invalidación por señal baja](./media/concepts/depth-camera-invalidation-low-signal.png)

### <a name="ambiguous-depth"></a>Profundidad ambigua

Los píxeles también se pueden invalidar si reciben señales de más de un objeto de la escena. Este tipo de invalidación se puede observar normalmente en las equinas.  Debido a la geometría de la escena, la luz de IR de la cámara se refleja en una pared y en la otra. Esta luz reflejada produce ambigüedad en la profundidad medida del píxel. Los filtros del algoritmo de profundidad detectan estas señales ambiguas e invalidan los píxeles.

En las figuras siguientes se muestran ejemplos de invalidación mediante la detección de trayectos múltiples. También puede ver cómo la misma área expuesta que se invalidó desde una vista de cámara (fila superior) puede aparecer de nuevo desde una vista de cámara diferente (fila inferior). En esta imagen se muestra que las superficies invalidadas desde una perspectiva pueden estar visibles desde otra.

![Invalidación por trayectos múltiples: esquina](./media/concepts/depth-camera-invalidation-multipath.png)

Otro caso común de trayectos múltiples es el de los píxeles que contienen la señal mixta del primer plano y del segundo plano (por ejemplo, alrededor de los bordes del objeto). Durante un movimiento rápido, puede que vea más píxeles invalidados alrededor de los bordes. Esos otros píxeles invalidados se deben al intervalo de exposición de la captura de profundidad sin procesar.

![Invalidación por trayectos múltiples: bordes](./media/concepts/depth-camera-invalidation-edge.png)

## <a name="next-steps"></a>Pasos siguientes

[Sistemas de coordenadas](coordinate-systems.md)
