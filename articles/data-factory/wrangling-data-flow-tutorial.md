---
title: Introducción al flujo de datos de limpieza y transformación en Azure Data Factory
description: Tutorial sobre cómo preparar los datos en Azure Data Factory mediante el flujo de datos de limpieza y transformación
author: djpmsft
ms.author: daperlov
ms.reviewer: gamal
ms.service: data-factory
ms.topic: conceptual
ms.date: 11/01/2019
ms.openlocfilehash: a180a7a0c85b642ac09d1d027c95809c4638dee1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81409007"
---
# <a name="prepare-data-with-wrangling-data-flow"></a>Preparación de datos con un flujo de datos de limpieza y transformación

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

## <a name="create-a-wrangling-data-flow"></a>Crear un flujo de datos de limpieza y transformación

Hay dos maneras de crear un flujo de datos de limpieza y transformación en Azure Data Factory. Una forma consiste en hacer clic en el icono del signo más y seleccionar **Flujo de datos** en el panel de recursos de Factory.

![Limpieza y transformación](media/wrangling-data-flow/tutorial7.png)

El otro método está en el panel de actividades del lienzo de la canalización. Abra el acordeón **Movimiento y transformación** y arrastre la actividad **Flujo de datos** al lienzo.

En ambos métodos, en el panel lateral que se abre, seleccione **Creación de un flujo de datos** y elija **Wrangling data flow** (Flujo de datos de limpieza y transformación). Haga clic en Aceptar.

![Limpieza y transformación](media/wrangling-data-flow/tutorial1.png)

## <a name="author-a-wrangling-data-flow"></a>Generar un flujo de datos de limpieza y transformación

Agregue un **Conjunto de datos de origen** para el flujo de datos de limpieza y transformación. Puede elegir un conjunto de datos existente o crear uno. También puede seleccionar un conjunto de datos de receptores. Puede elegir uno o más conjuntos de datos de origen, pero en este momento solo se permite un receptor. Elegir un conjunto de datos de receptor es opcional, pero se requiere al menos un conjunto de datos de origen.

> [!NOTE]
> Solo se admite el texto delimitado de ADLS Gen 2 para la versión preliminar limitada. 

![Limpieza y transformación](media/wrangling-data-flow/tutorial4.png)

Haga clic en **Crear** para abrir el editor de mashup Power Query Online.

![Limpieza y transformación](media/wrangling-data-flow/tutorial5.png)

Genere el flujo de datos de limpieza y transformación con la preparación de datos sin código. Para ver la lista de funciones disponibles, consulte las [funciones de transformación](wrangling-data-flow-functions.md)/.

![Limpieza y transformación](media/wrangling-data-flow/tutorial6.png)

## <a name="running-and-monitoring-a-wrangling-data-flow"></a>Ejecución y supervisión del flujo de datos de limpieza y transformación

Para una ejecución de depuración de canalización de un flujo de datos de limpieza y transformación, haga clic en **Depurar** en el lienzo de la canalización. Una vez publicado el flujo de datos, **Trigger now** (Desencadenar ahora) ejecuta a petición la última canalización publicada. Los flujos de datos de limpieza y transformación se pueden programar con todos los desencadenadores de Azure Data Factory existentes.

![Limpieza y transformación](media/wrangling-data-flow/tutorial3.png)

Vaya a la pestaña **Supervisión** para visualizar la salida de una ejecución de actividad de flujo de datos de limpieza y transformación desencadenada.

![Limpieza y transformación](media/wrangling-data-flow/tutorial2.png)

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo [crear un flujo de datos de asignación](tutorial-data-flow.md).