---
title: Desarrollo y depuración iterativos en Azure Data Factory
description: Obtenga información sobre cómo desarrollar y depurar de manera iterativa las canalizaciones de Data Factory en Azure Portal.
ms.date: 09/26/2018
ms.topic: conceptual
ms.service: data-factory
services: data-factory
documentationcenter: ''
ms.workload: data-services
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.openlocfilehash: 3a771181f8f2785339cbc47e0a0234b9c4e39adc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74926844"
---
# <a name="iterative-development-and-debugging-with-azure-data-factory"></a>Desarrollo y depuración iterativos con Azure Data Factory

Azure Data Factory permite desarrollar y depurar de manera iterativa las canalizaciones de Data Factory.

Si desea una introducción y demostración de ocho minutos de esta característica, vea el siguiente vídeo:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Iterative-development-and-debugging-with-Azure-Data-Factory/player]

## <a name="iterative-debugging-features"></a>Características de la depuración iterativa
Cree canalizaciones y realice series de pruebas con la funcionalidad **Depurar** en el lienzo de la canalización sin escribir ninguna línea de código.

![Funcionalidad de depuración en el lienzo de la canalización](media/iterative-development-debugging/iterative-development-image1.png)

Consulte los resultados de las series de pruebas en la ventana **Salida** del lienzo de la canalización.

![Ventana de salida del lienzo de la canalización](media/iterative-development-debugging/iterative-development-image2.png)

Después de realizar correctamente una serie de pruebas, agregue más actividades a la canalización y continúe con la depuración de manera iterativa. También puede **cancelar** una serie de pruebas mientras está en curso.

![Cancelación de una serie de pruebas](media/iterative-development-debugging/iterative-development-image3.png)

Cuando realiza las series de pruebas, no es necesario que publique los cambios que se realizaron en la factoría de datos antes de seleccionar **Depurar**. Esta característica resulta útil en escenarios en los que se quiere garantizar que los cambios funcionen según lo esperado antes de actualizar el flujo de trabajo de la factoría de datos.

> [!IMPORTANT]
> Al seleccionar **Depurar**, se ejecuta la canalización. Así, por ejemplo, si la canalización contiene actividad de copia, la serie de pruebas copia datos del origen al destino. Como resultado, se recomienda usar las carpetas de prueba en las actividades de copia y otras actividades cuando se realice la depuración. Una vez que se depure la canalización, cambie a las carpetas reales que desea usar en las operaciones normales.

## <a name="visualizing-debug-runs"></a>Visualizar ejecuciones de depuración

Puede visualizar todas las ejecuciones de depuración que están en curso para la factoría de datos desde un solo lugar. Seleccione **View debug runs** (Ver ejecuciones de depuración) en la esquina superior derecha de la página. Esta característica es útil en escenarios donde tiene canalizaciones maestras que inician ejecuciones de depuración para canalizaciones secundarias y quiere tener una sola vista para ver que las ejecuciones de depuración activas.

![Seleccione el icono de vista de ejecuciones de depuraciones activas](media/iterative-development-debugging/view-debug-runs-image1.png)

![Lista de ejemplos de ejecuciones de depuraciones activas](media/iterative-development-debugging/view-debug-runs-image2.png)

Si tiene sesiones de depuración de Data Flow activas, las verá en la parte inferior de la ventana de depuración activa. Puede seleccionar una sesión de flujo de datos activa y detener el clúster correspondiente.

![Lista de ejemplos de ejecuciones de depuración de flujo de datos activas](media/data-flow/dfsessions.png)

## <a name="monitoring-debug-runs"></a>Supervisión de ejecuciones de depuración

Las series de pruebas iniciadas con la funcionalidad **Depurar** no están disponibles en la lista de la pestaña **Supervisar**. Solo puede ver las series que se desencadenan con los desencadenadores **Trigger Now** (Desencadenar ahora), **Schedule** (Programar) o **Tumbling Window** (Ventana de saltos de tamaño constante) de la pestaña **Supervisar**. Puede ver la última serie de pruebas iniciada con la funcionalidad **Depurar** en la ventana **Salida** del lienzo de la canalización.

## <a name="setting-breakpoints-for-debugging"></a>Establecimiento de puntos de interrupción para la depuración

Data Factory también permite realizar la depuración hasta alcanzar una actividad concreta en el lienzo de la canalización. Simplemente ubique un punto de interrupción en la actividad que indique hasta donde desea probar y seleccione **Depurar**. Data Factory garantiza que las pruebas se ejecutan solo hasta la actividad de punto de interrupción en el lienzo de la canalización. Esta característica *Debug Until* (Depurar hasta) resulta útil cuando no desea probar toda la canalización, sino solo un subconjunto de actividades dentro de la canalización.

![Puntos de interrupción en el lienzo de la canalización](media/iterative-development-debugging/iterative-development-image4.png)

Para establecer un punto de interrupción, seleccione un elemento en el lienzo de la canalización. La opción *Depurar hasta* aparece como un círculo rojo vacío en la esquina superior derecha del elemento.

![Antes de establecer un punto de interrupción en el elemento seleccionado](media/iterative-development-debugging/iterative-development-image5.png)

Tras seleccionar la opción *Depurar hasta*, cambia a un círculo de color rojo con relleno para indicar que el punto de interrupción está habilitado.

![Después de establecer un punto de interrupción en el elemento seleccionado](media/iterative-development-debugging/iterative-development-image6.png)

## <a name="next-steps"></a>Pasos siguientes
[Integración e implementación continuas en Azure Data Factory](continuous-integration-deployment.md)
