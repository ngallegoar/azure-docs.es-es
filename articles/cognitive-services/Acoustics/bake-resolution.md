---
title: Resolución de la simulación mediante "bake" de Project Acoustics
titlesuffix: Azure Cognitive Services
description: En esta información general conceptual se describe la diferencia entre las resoluciones gruesas y finas al realizar la simulación acústica mediante "bake".
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: d8eb3b2cbaf7b4e842d8338eefde756f6d381111
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "68854349"
---
# <a name="project-acoustics-bake-resolution"></a>Resolución de la simulación mediante "bake" de Project Acoustics
En esta información general conceptual se describe la diferencia entre las resoluciones gruesas y finas al realizar la simulación acústica mediante "bake". Elija esta opción durante el paso de sondeos del flujo de trabajo de simulación mediante "bake".

## <a name="coarse-vs-fine-resolution"></a><a name="Coarse-vs-Fine-Resolution"></a>Resolución gruesa frente a fina

La única diferencia entre la configuración de resolución gruesa y la fina es la frecuencia con la que se realiza la simulación. La fina usa una frecuencia el doble de alta que la gruesa. Esta elección tiene una serie de implicaciones sobre la simulación acústica:

* La longitud de onda de la gruesa es el doble de larga que la fina y, por lo tanto, los vóxeles son el doble de grandes.
* El tiempo de simulación está directamente relacionado con el tamaño de vóxel, lo que hace que la simulación mediante "bake" gruesa sea 16 veces más rápida que la simulación mediante "bake" fina.
* Los portales (por ejemplo, puertas o ventanas) más pequeños que el tamaño de vóxel no se pueden simular. La configuración gruesa puede hacer que algunos de estos portales más pequeños no se simulen; por lo tanto, no pasarán sonidos en tiempo de ejecución. Para ver si sucede esto, puede visualizar los vóxeles.
* La frecuencia de simulación inferior da como resultado menos difracción en torno a los bordes y esquinas.
* No se pueden encontrar orígenes de sonido dentro de vóxels "rellenados" (es decir, vóxels que contienen geometría). El resultado es la ausencia de sonido. Es más difícil situar los orígenes de sonido si no están dentro de los vóxeles más grandes de la configuración gruesa que cuando se usa la configuración fina.
* Los vóxeles más grandes se meten más en los portales, como se muestra a continuación. La primera imagen se creó con la configuración gruesa, mientras que la segunda es la misma entrada con resolución fina. Como indican las marcas de color rojo, hay mucha menos intrusión en la entrada cuando se usa la configuración fina. La línea azul es la entrada, como se define en la geometría, mientras que la línea roja es el portal acústico efectivo definido por el tamaño de vóxel. La evolución esta intrusión en una situación determinada dependerá completamente de cómo los vóxeles se alineen con la geometría del portal, que viene determinado por el tamaño y las ubicaciones de los objetos en la escena.

![Captura de pantalla de vóxeles gruesos rellenando una entrada en Unreal](media/unreal-coarse-bake.png)

![Captura de pantalla de vóxeles finos en una entrada en Unreal](media/unreal-fine-bake.png)

## <a name="next-steps"></a>Pasos siguientes

Pruebe por su cuenta la configuración de resolución gruesa y fina con nuestros complementos de [Unreal](unreal-baking.md) o [Unity](unity-baking.md).
