---
title: Copia de datos de forma incremental
description: En estos tutoriales se muestra cómo copiar datos de forma incremental de un almacén de datos de origen a un almacén de datos de destino. La primera de ellas copia los datos de una tabla.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
manager: anandsub
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
ms.custom: seo-lt-2019
ms.date: 01/22/2018
ms.openlocfilehash: f1fb209f44ae7922de87277100fbf55add184766
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84194488"
---
# <a name="incrementally-load-data-from-a-source-data-store-to-a-destination-data-store"></a>Carga incremental de datos de un almacén de datos de origen a un almacén de datos de destino

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

En una solución de integración de datos, la carga incremental (o diferencial) de los datos después de una carga completa de los datos es un método ampliamente usado. En los tutoriales de esta sección se muestran distintas formas de cargar datos de forma incremental mediante Azure Data Factory.

## <a name="delta-data-loading-from-database-by-using-a-watermark"></a>Carga diferencial de datos de la base de datos mediante una marca de agua
En este caso, definirá una marca de agua en la base de datos de origen. Una marca de agua es una columna que tiene la marca de tiempo de la última actualización o una clave de incremento. La solución de carga diferencial carga los datos modificados entre una marca de agua antigua y una nueva marca de agua. En el siguiente diagrama se representa el flujo de trabajo de este enfoque: 

![Flujo de trabajo de uso de una marca de agua](media/tutorial-incremental-copy-overview/workflow-using-watermark.png)

Consulte los siguientes temas para obtener instrucciones paso a paso: 
- [Carga de datos de forma incremental de Azure SQL Database a Azure Blob Storage](tutorial-incremental-copy-powershell.md)
- [Copia incremental de datos de varias tablas de una instancia de SQL Server en Azure SQL Database](tutorial-incremental-copy-multiple-tables-powershell.md)

Para las plantillas, consulte lo siguiente:
- [Copia diferencial con la tabla de controles](solution-template-delta-copy-with-control-table.md)

## <a name="delta-data-loading-from-sql-db-by-using-the-change-tracking-technology"></a>Carga diferencial de datos SQL DB mediante la tecnología Change Tracking
La tecnología de control de cambios es una solución ligera de SQL Server y Azure SQL Database que ofrece un mecanismo eficaz de control de cambios para las aplicaciones. Así, permite que una aplicación identifique fácilmente los datos que se insertaron, actualizaron o eliminaron. 

En el siguiente diagrama se representa el flujo de trabajo de este enfoque:

![Flujo de trabajo del control de cambios](media/tutorial-incremental-copy-overview/workflow-using-change-tracking.png)

Para ver instrucciones paso a paso, consulte el siguiente tutorial: <br/>
- [Carga incremental de datos de Azure SQL Database a Azure Blob Storage mediante la información de control de cambios](tutorial-incremental-copy-change-tracking-feature-powershell.md)

## <a name="loading-new-and-changed-files-only-by-using-lastmodifieddate"></a>Carga de archivos nuevos y modificados solo mediante LastModifiedDate
Puede copiar los archivos nuevos y modificados en el almacén de destino utilizando solo LastModifiedDate. ADF examinará todos los archivos del almacén de origen, aplicará el filtro de archivos con LastModifiedDate, y copiará solo los archivos nuevos y actualizados desde la última vez en el almacén de destino.  Tenga en cuenta que si deja que ADF examine enormes cantidades de archivos, aunque solo copie algunos archivos en destino, aún así tendrá que prever que tarde bastante, ya que el examen de archivos puede exigir mucho tiempo.   

Para ver instrucciones paso a paso, consulte el siguiente tutorial: <br/>
- [Copia incremental de archivos nuevos y modificados según LastModifiedDate desde Azure Blob Storage hasta Azure Blob Storage](tutorial-incremental-copy-lastmodified-copy-data-tool.md)

Para las plantillas, consulte lo siguiente:
- [Copia de archivos nuevos por LastModifiedDate](solution-template-copy-new-files-lastmodifieddate.md)

## <a name="loading-new-files-only-by-using-time-partitioned-folder-or-file-name"></a>Carga de archivos nuevos mediante únicamente el nombre de archivo o la carpeta con particiones de tiempo
Puede copiar solamente archivos nuevos, donde ya se ha realizado una partición de tiempo de los archivos o carpetas con información de intervalo de tiempo como parte del nombre de archivo o carpeta (por ejemplo, /aaaa/mm/dd/file.csv). Es el enfoque de mayor rendimiento para la carga incremental de los nuevos archivos. 

Para ver instrucciones paso a paso, consulte el siguiente tutorial: <br/>
- [Copia incremental de nuevos archivos según la carpeta con particiones de tiempo o el nombre de archivo desde Azure Blob Storage hasta Azure Blob Storage](tutorial-incremental-copy-partitioned-file-name-copy-data-tool.md)

## <a name="next-steps"></a>Pasos siguientes
Avance al siguiente tutorial: 

> [!div class="nextstepaction"]
>[Carga de datos de forma incremental de Azure SQL Database a Azure Blob Storage](tutorial-incremental-copy-powershell.md)
