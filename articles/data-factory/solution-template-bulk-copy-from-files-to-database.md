---
title: Copia masiva desde archivos a una base de datos
description: Aprenda a usar una plantilla de solución para copiar datos de forma masiva desde Azure Data Lake Storage Gen2 a Azure Synapse Analytics/Azure SQL Database.
services: data-factory
author: linda33wj
ms.author: jingwang
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: ae250c7d15801789ad22955845cfa535ed91f2c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75921153"
---
# <a name="bulk-copy-from-files-to-database"></a>Copia masiva desde archivos a una base de datos

Este artículo describe una plantilla de solución para copiar datos de forma masiva desde Azure Data Lake Storage Gen2 a Azure Synapse Analytics/Azure SQL Database.

## <a name="about-this-solution-template"></a>Acerca de esta plantilla de solución

Esta plantilla recupera archivos de un origen de Azure Data Lake Storage Gen2. A continuación, recorre en iteración cada archivo de origen y copia el archivo al almacén de datos de destino. 

Actualmente, esta plantilla solo admite la copia de datos en formato **DelimitedText**. Los archivos de otros formatos de datos también se pueden recuperar del almacén de datos de origen, pero no se pueden copiar en el almacén de datos de destino.  

La plantilla contiene tres actividades:
- La actividad **Obtener metadatos** recupera archivos de Azure Data Lake Storage Gen2 y los envía a la actividad *ForEach* posterior.
- La actividad **ForEach** obtiene los archivos de la actividad *Obtener metadatos* e itera cada archivo en la actividad *Copiar*.
- La actividad **Copiar** reside en la actividad *ForEach* para copiar cada archivo del almacén de datos de origen al almacén de datos de destino.

La plantilla define los dos parámetros siguientes:
- *SourceContainer* es la ruta de acceso del contenedor raíz en el que se copian los datos en el Azure Data Lake Storage Gen2. 
- *SourceDirectory* es la ruta de acceso del directorio en el contenedor raíz del que se copian los datos en el Azure Data Lake Storage Gen2.

## <a name="how-to-use-this-solution-template"></a>Uso de esta plantilla de solución

1. Vaya a la plantilla **Copia masiva desde archivos a una base de datos**. Cree una **nueva** conexión al almacén Gen2 de origen. Tenga en cuenta que "GetMetadataDataset" y "SourceDataset" son referencias a la misma conexión del almacén de archivos de origen.

    ![Creación de una nueva conexión al almacén de datos de origen](media/solution-template-bulk-copy-from-files-to-database/source-connection.png)

2. Cree una conexión **nueva** al almacén de datos receptor al que está copiando datos.

    ![Creación de una nueva conexión al almacén de datos receptor](media/solution-template-bulk-copy-from-files-to-database/destination-connection.png)
    
3. Seleccione **Usar esta plantilla**.

    ![Uso de esta plantilla](media/solution-template-bulk-copy-from-files-to-database/use-template.png)
    
4. Verá una canalización creada tal y como se muestra en el ejemplo siguiente:

    ![Revisión de la canalización](media/solution-template-bulk-copy-from-files-to-database/new-pipeline.png)

    > [!NOTE]
    > Si eligió **Azure Synapse Analytics (antes, SQL DW)** como destino de datos en el **paso 2** descrito anteriormente, debe escribir una conexión a una instancia de Azure Blob Storage como almacenamiento provisional, porque así lo requiere SQL Data Warehouse Polybase. Como se muestra en la siguiente captura de pantalla, la plantilla generará automáticamente una *ruta de acceso de almacenamiento* para Blob Storage. Consulte si el contenedor se ha creado después de la ejecución de la canalización.
        
    ![Configuración de PolyBase](media/solution-template-bulk-copy-from-files-to-database/staging-account.png)

5. Seleccione **Depurar**, escriba los **parámetros** y, a continuación, seleccione **Finalizar**.

    ![Clic en **Depurar**](media/solution-template-bulk-copy-from-files-to-database/debug-run.png)

6. Cuando la ejecución de la canalización se complete correctamente, verá resultados similares al ejemplo siguiente:

    ![Revisión del resultado](media/solution-template-bulk-copy-from-files-to-database/run-succeeded.png)

       
## <a name="next-steps"></a>Pasos siguientes

- [Introducción al servicio Azure Data Factory](introduction.md)