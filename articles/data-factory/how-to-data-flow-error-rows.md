---
title: Controlar las filas de error con la asignación de flujos de datos en Azure Data Factory
description: Aprenda a controlar errores de truncamiento de SQL en Azure Data Factory mediante la asignación de flujos de datos.
services: data-factory
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/20/2020
ms.author: makromer
ms.openlocfilehash: 8225143bb75118620b45c2520bb62ea30501a617
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732685"
---
# <a name="handle-sql-truncation-error-rows-in-data-factory-mapping-data-flows"></a>Controlar las filas de errores de truncamiento de SQL en Data Factory con asignación de flujos de datos

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Un escenario común en Data Factory cuando se usa la asignación de flujos de datos consiste en escribir los datos transformados en una base de datos de Azure SQL. En este escenario, una condición de error común que se debe evitar es un posible truncamiento de columna. Siga estos pasos para proporcionar el registro de columnas que no quepan en una columna de cadena de destino, lo que permite que el flujo de datos continúe en esos escenarios.

## <a name="scenario"></a>Escenario

1. Tenemos una tabla de base de datos de Azure SQL de destino que tiene una columna ```nvarchar(5)``` denominada "Name" (Nombre).

2. Dentro de nuestro flujo de datos, queremos asignar títulos de películas de nuestro receptor a esa columna de "Name" de destino.

    ![Flujo de datos de película 1](media/data-flow/error4.png)
    
3. El problema es que el título de la película no cabe en una columna de receptor que solo pueda contener 5 caracteres. Al ejecutar este flujo de datos, recibirá un error similar al siguiente: ```"Job failed due to reason: DF-SYS-01 at Sink 'WriteToDatabase': java.sql.BatchUpdateException: String or binary data would be truncated. java.sql.BatchUpdateException: String or binary data would be truncated."```

Este vídeo le guía a través de un ejemplo de configuración de la lógica de control de filas de errores en el flujo de datos:
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4uOHj]

## <a name="how-to-design-around-this-condition"></a>Cómo diseñar en torno a esta condición

1. En este escenario, la longitud máxima de la columna "Name" es de cinco caracteres. Por lo tanto, vamos a agregar una transformación de división condicional que nos permitirá registrar filas con "títulos" con más de cinco caracteres, al tiempo que permite que el resto de las filas que caben en ese espacio escriban en la base de datos.

    ![división condicional](media/data-flow/error1.png)

2. Esta transformación de división condicional define la longitud máxima de "title" (título) como cinco. Cualquier fila que sea menor o igual que cinco irá al flujo ```GoodRows```. Cualquier fila que sea mayor a cinco irá al flujo ```BadRows```.

3. Ahora es necesario registrar las filas en las que se produjo un error. Agregue una transformación de receptor al flujo ```BadRows``` para el registro. Aquí, se asignarán automáticamente todos los campos para que se haga el registro de transacciones completo. Se trata de un archivo CSV delimitado por texto que se envía a un solo archivo en Blob Storage. Llamaremos al archivo de registro "badrows.csv".

    ![Filas incorrectas](media/data-flow/error3.png)
    
4. A continuación se muestra el flujo de datos completado. Ahora podemos dividir las filas de error para evitar los errores de truncamiento de SQL y colocar dichas entradas en un archivo de registro. Mientras tanto, las filas correctas pueden continuar escribiendo en nuestra base de datos de destino.

    ![flujo de datos completo](media/data-flow/error2.png)

## <a name="next-steps"></a>Pasos siguientes

* Compile el resto de la lógica del flujo de datos mediante [transformaciones](concepts-data-flow-overview.md) de flujos de datos de asignación.
