---
title: Desduplicación de filas y búsqueda de valores NULL mediante fragmentos de flujo de datos
description: Aprenda cómo desduplicar filas y buscar valores NULL fácilmente mediante fragmentos de código en flujos de datos.
services: data-factory
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/30/2020
ms.author: makromer
ms.openlocfilehash: 841484a647d2737d621c75ebe63f65f2de829a26
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91666520"
---
# <a name="dedupe-rows-and-find-nulls-using-data-flow-snippets"></a>Desduplicación de filas y búsqueda de valores NULL mediante fragmentos de flujo de datos

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Si utiliza fragmentos de código en los flujos de datos de asignación, puede realizar de manera muy sencilla tareas comunes, como la desduplicación de datos y el filtrado de valores NULL. En esta guía paso a paso se explica cómo agregar esas funciones a las canalizaciones de un modo muy fácil mediante fragmentos de código de script de flujo de datos.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4GnhH]

## <a name="create-a-pipeline"></a>Crear una canalización

1. Seleccione **+Nueva canalización** para iniciar una nueva canalización.

2. Agregue una actividad de flujo de datos.

3. Agregue una transformación de origen y conéctela a uno de los conjuntos de datos.

    ![Fragmento de origen 2](media/data-flow/snippet-adf-2.png)

4. Los fragmentos de código de desduplicación y comprobación de valores NULL usan patrones genéricos que aprovechan el desfase del esquema de flujo de datos, por lo que funcionarán con cualquier esquema de su conjunto de datos o con conjuntos de datos que no tengan ningún esquema predefinido.

5. [Vaya a la página de documentación Script de flujo de datos y copie el fragmento de código de filas distintas](https://docs.microsoft.com/azure/data-factory/data-flow-script#distinct-row-using-all-columns).

6. En la interfaz de usuario del diseñador de flujo de datos, haga clic en el botón Script situado en la parte superior derecha para abrir el editor de script tras el gráfico de flujo de datos.

    ![Fragmento de origen 3](media/data-flow/snippet-adf-3.png)

7. Después de la definición de ```source1``` en el script, presione Intro y pegue el fragmento de código.

8. Conectará este fragmento de código pegado a la transformación de origen anterior que creó en el gráfico escribiendo "source1" antes del código pegado.

9. También puede conectar la transformación nueva en el diseñador seleccionando la secuencia entrante del nuevo nodo de transformación en el gráfico.

    ![Fragmento de origen 4](media/data-flow/snippet-adf-4.png)

10. Ahora, el flujo de datos quitará las filas duplicadas del origen mediante la transformación de agregado, que agrupa todas las filas usando un hash general en todos los valores de columna.
    
11. A continuación, agregaremos un fragmento de código para dividir los datos en una secuencia que contiene filas con valores NULL y otra secuencia que no tiene valores NULL.

12. [Vuelva a la biblioteca de fragmentos de código y esta vez copie el código para las comprobaciones de valores NULL](https://docs.microsoft.com/azure/data-factory/data-flow-script#check-for-nulls-in-all-columns).

13. En el diseñador de flujo de datos, haga clic de nuevo en Script y pegue este nuevo código de transformación en la parte inferior, conectándolo a la transformación anterior; para ello, escriba el nombre de esa transformación antes del fragmento de código pegado.

14. El gráfico de flujo de datos debería tener ahora un aspecto similar al siguiente:

    ![Fragmento de origen 1](media/data-flow/snippet-adf-1.png)

  Ahora tiene un flujo de datos en funcionamiento con desduplicación y comprobación de valores NULL genéricos, que ha creado tomando fragmentos de código existentes de la biblioteca de Scripts de flujo de datos y agregándolos al diseño existente.

## <a name="next-steps"></a>Pasos siguientes

* Compile el resto de la lógica del flujo de datos mediante [transformaciones](concepts-data-flow-overview.md) de flujos de datos de asignación.
