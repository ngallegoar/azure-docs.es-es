---
title: Introducción al flujo de datos de limpieza y transformación en Azure Data Factory
description: Tutorial sobre cómo preparar los datos en Azure Data Factory mediante el flujo de datos de limpieza y transformación
author: djpmsft
ms.author: daperlov
ms.reviewer: gamal
ms.service: data-factory
ms.topic: conceptual
ms.date: 11/01/2019
ms.openlocfilehash: e9a4f7266522fc17c33e2086af6cb7367bbed39e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91650687"
---
# <a name="prepare-data-with-wrangling-data-flow"></a>Preparación de datos con un flujo de datos de limpieza y transformación

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

> [!NOTE]
> El flujo de datos de limpieza y transformación está actualmente disponible en versión preliminar pública

## <a name="create-a-wrangling-data-flow"></a>Crear un flujo de datos de limpieza y transformación

Hay dos maneras de crear un flujo de datos de limpieza y transformación en Azure Data Factory. Una forma consiste en hacer clic en el icono del signo más y seleccionar **Flujo de datos** en el panel de recursos de Factory.

![Captura de pantalla que muestra Data Flow en el panel Recursos de fábrica.](media/wrangling-data-flow/tutorial7.png)

El otro método está en el panel de actividades del lienzo de la canalización. Abra el acordeón **Movimiento y transformación** y arrastre la actividad **Flujo de datos** al lienzo.

En ambos métodos, en el panel lateral que se abre, seleccione **Creación de un flujo de datos** y elija **Wrangling data flow** (Flujo de datos de limpieza y transformación). Haga clic en Aceptar.

![Captura de pantalla que resalta la opción de flujo de datos de limpieza y transformación.](media/wrangling-data-flow/tutorial1.png)

## <a name="author-a-wrangling-data-flow"></a>Generar un flujo de datos de limpieza y transformación

Agregue un **Conjunto de datos de origen** para el flujo de datos de limpieza y transformación. Puede elegir un conjunto de datos existente o crear uno. También puede seleccionar un conjunto de datos de receptores. Puede elegir uno o más conjuntos de datos de origen, pero en este momento solo se permite un receptor. Elegir un conjunto de datos de receptor es opcional, pero se requiere al menos un conjunto de datos de origen.

> [!NOTE]
> Solo se admite el texto delimitado de ADLS Gen 2 para la versión preliminar limitada. 

![Limpieza y transformación](media/wrangling-data-flow/tutorial4.png)

Haga clic en **Crear** para abrir el editor de mashup Power Query Online.

![Captura de pantalla que muestra el botón Crear que abre el editor de mashup de Power Query Online.](media/wrangling-data-flow/tutorial5.png)

Genere el flujo de datos de limpieza y transformación con la preparación de datos sin código. Para ver la lista de funciones disponibles, consulte las [funciones de transformación](wrangling-data-flow-functions.md).

![Captura de pantalla que muestra el proceso para crear el flujo de datos de limpieza y transformación.](media/wrangling-data-flow/tutorial6.png)

## <a name="running-and-monitoring-a-wrangling-data-flow"></a>Ejecución y supervisión del flujo de datos de limpieza y transformación

Para una ejecución de depuración de canalización de un flujo de datos de limpieza y transformación, haga clic en **Depurar** en el lienzo de la canalización. Una vez publicado el flujo de datos, **Trigger now** (Desencadenar ahora) ejecuta a petición la última canalización publicada. Los flujos de datos de limpieza y transformación se pueden programar con todos los desencadenadores de Azure Data Factory existentes.

![Captura de pantalla que muestra cómo agregar un flujo de datos de limpieza y transformación.](media/wrangling-data-flow/tutorial3.png)

Vaya a la pestaña **Supervisión** para visualizar la salida de una ejecución de actividad de flujo de datos de limpieza y transformación desencadenada.

![Captura de pantalla que muestra la salida de una ejecución de actividad de flujo de datos de limpieza y transformación desencadenada.](media/wrangling-data-flow/tutorial2.png)

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo [crear un flujo de datos de asignación](tutorial-data-flow.md).