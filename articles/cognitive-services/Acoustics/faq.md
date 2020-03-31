---
title: Preguntas frecuentes sobre Project Acoustics
titlesuffix: Azure Cognitive Services
description: En esta página se proporcionan respuestas a las preguntas más frecuentes sobre Elementos acústicos de un proyecto, incluida la descarga de instrucciones y el proceso de elaboración.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: fa4b6499260219b0eb8ea4df4b4ccfd5263b57bb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "75770210"
---
# <a name="project-acoustics-frequently-asked-questions"></a>Preguntas frecuentes sobre Project Acoustics

## <a name="what-is-project-acoustics"></a>¿Qué son los elementos acústicos de un proyecto?

El conjunto de complementos de Project Acoustics es un sistema acústico que calcula el comportamiento de las ondas de sonido antes del tiempo de ejecución, de manera similar a la luz estática. La nube hace el trabajo pesado de los cálculos de física de ondas, por lo que el costo de CPU en tiempo de ejecución es bajo.  

## <a name="where-can-i-download-the-plugin"></a>¿Dónde puedo descargar el complemento?

Puede descargar el [complemento de Unity de Project Acoustics](https://www.microsoft.com/download/details.aspx?id=57346) o el [complemento de Unreal de Project Acoustics](https://www.microsoft.com/download/details.aspx?id=58090).

## <a name="does-project-acoustics-support-ltxgt-platform"></a>¿Admite Project Acoustics la plataforma &lt;x&gt;?

La compatibilidad con la plataforma de Project Acoustics evoluciona en función de las necesidades del cliente. Para información sobre la compatibilidad con otras plataformas, póngase en contacto con nosotros en el [foro de problemas de Project Acoustics](https://github.com/microsoft/ProjectAcoustics/issues).

## <a name="is-azure-used-at-runtime"></a>¿Se usa Azure en tiempo de ejecución?

No, la integración de la nube solo se usa durante la fase de proceso previo como parte de la configuración de la escena.
 
## <a name="what-is-simulation-input"></a>¿Qué es la entrada de simulación? 

La entrada de simulación es la escena 3D, el entorno virtual o el nivel del juego. Los Elementos acústicos de un proyecto llevan a cabo simulaciones de ondas volumétricas en 3D que modelan de manera cercana la física del archivo de sonido, incluida la dispersión y la oclusión fluida.
 
## <a name="what-is-the-runtime-cost"></a>¿Cuál es el costo del entorno de ejecución?

La acústica usa aproximadamente el 0,01 % de CPU por origen por fotograma. El uso de RAM depende del tamaño de la escena y puede oscilar entre 10 y 100 MB.
 
## <a name="do-i-need-to-simplify-the-level-geometry-control-triangle-count-make-meshes-watertight"></a>¿Es necesario simplificar la geometría de nivel? ¿Controlar el recuento de triángulos? ¿Hacer que las mallas sean estancas?

No. El sistema ingerirá directamente la geometría de nivel detallada. Se voxelizará para procesamiento interno.
 
## <a name="whats-in-the-runtime-lookup-table"></a>¿Qué hay en la tabla de búsqueda del entorno de ejecución?

El archivo ACE incluye una tabla de parámetros acústicos entre numerosos pares de fuente y ubicación del cliente de escucha, así como geometría de escena voxelizada que se usa para la interpolación de parámetros.
 
## <a name="can-project-acoustics-handle-moving-sources"></a>¿Puede gestionar Project Acoustics orígenes móviles?

Sí, Project Acoustics consulta la tabla de búsqueda y actualiza el DSP de audio en cada marca, para que pueda administrar los orígenes y el cliente de escucha en movimiento.
 
## <a name="can-project-acoustics-handle-dynamic-geometry-closing-doors-walls-blown-away"></a>¿Puede gestionar Project Acoustics la geometría dinámica? ¿El cierre de puertas? ¿Las paredes destruidas?

No. Los parámetros acústicos se procesan previamente en función del estado estático de un nivel de juego. Se recomienda dejar la geometría de las puertas fuera de la acústica y luego aplicar oclusión adicional en función del estado de los objetos de juego destructibles y movibles mediante el uso de técnicas establecidas.
 
## <a name="does-project-acoustics-use-acoustic-materials"></a>¿Usa Project Acoustics materiales acústicos?

Sí. Los materiales se seleccionan de los nombres de materiales físicos en su nivel, lo que impulsa la capacidad de absorción.
 
## <a name="what-do-the-probes-represent"></a>¿Qué representan los "sondeos"?

Los sondeos son un muestreo de posibles ubicaciones del reproductor. Cada sondeo representa una simulación de onda independiente de la escena que se origina en la ubicación del sondeo. En entorno de ejecución, los parámetros acústicos de la ubicación del oyente se interpolan desde ubicaciones de sondeo cercanas.
 
## <a name="why-spend-so-much-compute-in-the-cloud-what-does-it-buy-me"></a>¿Por qué gastar tanto proceso en la nube? ¿Qué beneficio se obtiene?

Elementos acústicos de un proyecto brinda parámetros acústicos precisos y confiables incluso para entornos virtuales extremadamente complejos, que toma en cuenta cada aspecto de la arquitectura. Ofrece obstrucción y oclusión fluidas y variación de reverberación dinámica sin todo el trabajo manual de extraer volúmenes. Todo esto mientras se mantiene el uso ligero de la CPU durante el entorno de tiempo de ejecución.

## <a name="what-exactly-happens-during-baking"></a>¿Qué ocurre exactamente durante la "elaboración"?

La simulación mediante "bake" consiste en simulaciones de ondas acústicas de regiones de simulación cuboides centradas en cada sondeo del cliente de escucha.

## <a name="is-my-source-content-secure"></a>¿Mi contenido de origen está protegido?

Project Acoustics no carga la geometría de la escena de origen en la nube. En su lugar, la simulación funciona en una voxelización de la escena, que se combina con los datos de la ubicación de sondeo y se almacena con un formato de propiedad.     

## <a name="next-steps"></a>Pasos siguientes
* Pruebe el [contenido de ejemplo de Unity de Project Acoustics](unity-quickstart.md) o el [contenido de ejemplo de Unreal](unreal-quickstart.md).

