---
title: ¿Qué es Azure Synapse Analytics (anteriormente SQL Data Warehouse)?
description: Azure Synapse Analytics (anteriormente SQL Data Warehouse) es un servicio de análisis ilimitado que reúne el almacenamiento de datos empresariales y el análisis de macrodatos.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql-dw
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 8840791c7b18d1efa499c2826a6eaf041a6da787
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93317482"
---
# <a name="what-is-azure-synapse-analytics-formerly-sql-dw"></a>¿Qué es Azure Synapse Analytics (anteriormente SQL Data Warehouse)?

> [!NOTE]
>Explore la [documentación de Azure Synapse (versión preliminar de áreas de trabajo)](../overview-what-is.md).
>

Azure Synapse es un servicio de análisis que engloba el almacenamiento de datos empresariales y el análisis de macrodatos. Le ofrece la libertad de consultar los datos como prefiera, ya sea a petición sin servidor o con recursos aprovisionados, a escala. Azure Synapse reúne estos dos mundos con una experiencia unificada para ingerir, preparar, administrar y servir datos para las necesidades inmediatas de inteligencia empresarial y aprendizaje automático.

Azure Synapse tiene cuatro componentes:

- SQL de Synapse: Análisis basado en T-SQL completo: disponible con carácter general
  - Grupo de SQL dedicado (pago por DWU aprovisionado)
  - Grupo de SQL sin servidor (pago por TB procesado) (versión preliminar)
- Spark: profunda integración con Apache Spark (versión preliminar)
- Canalizaciones de Synapse: integración de datos híbridos (versión preliminar)
- Studio: Experiencia de usuario unificada (versión preliminar)

## <a name="dedicated-sql-pool-in-azure-synapse"></a>Grupo de SQL dedicado en Azure Synapse

El grupo de SQL dedicado hace referencia a las características de almacenamiento de datos empresariales que están disponibles con carácter general en Azure Synapse.

El grupo de SQL dedicado representa una colección de recursos de análisis que se aprovisionan al usar Synapse SQL. El tamaño de un grupo de SQL dedicado se determina mediante las unidades de almacenamiento de datos (DWU).

Importe macrodatos con consultas T-SQL de [PolyBase](/sql/relational-databases/polybase/polybase-guide?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) simples y, después, use la potencia del motor de consultas distribuidas para realizar un análisis de alto rendimiento. Al realizar la integración y el análisis de los datos, Synapse SQL pasará a ser la versión única de certeza con la que puede contar su empresa para obtener información. 

## <a name="key-component-of-a-big-data-solution"></a>Componente clave de una solución de macrodatos

El almacenamiento de datos un componente clave de una solución de macrodatos de un extremo a otro en la nube.

![Solución de almacenamiento de datos](./media/sql-data-warehouse-overview-what-is/data-warehouse-solution.png)

En una solución de datos en la nube, los datos provenientes de varios orígenes se ingieren en almacenes de macrodatos. Una vez que están en un almacén de macrodatos, Hadoop, y Spark y los algoritmos de aprendizaje automático preparan y entrenan los datos. Cuando los datos están listos para el análisis complejo, el grupo de SQL dedicado usa PolyBase para consultar los almacenes de macrodatos. PolyBase usa las consultas T-SQL estándar para llevar los datos a tablas del grupo de SQL dedicado.

El grupo de SQL dedicado almacena los datos en tablas relacionales con almacenamiento en columnas. Este formato reduce considerablemente los costos de almacenamiento de datos y mejora el rendimiento de las consultas. Una vez que los datos están almacenados, se pueden realizar análisis a gran escala. En comparación con los sistemas de bases de datos tradicionales, las consultas de análisis finalizan en segundos, en lugar de minutos, o en horas, en lugar de días.

Los resultados del análisis pueden ir a aplicaciones o bases de datos de informes mundiales. En ellas, los analistas empresariales pueden obtener información útil para tomar decisiones empresariales bien informadas.

## <a name="next-steps"></a>Pasos siguientes

- Exploración de la [arquitectura de Azure Synapse](massively-parallel-processing-mpp-architecture.md)
- [Creación de un grupo de SQL dedicado](create-data-warehouse-portal.md) de forma rápida
- [Cargue datos de ejemplo](load-data-from-azure-blob-storage-using-polybase.md).
- Explore [vídeos](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse).

O bien, examine algunos de estos otros recursos de Azure Synapse.

- Busque información en [Blogs](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/).
- Envíe una [solicitud de característica](https://feedback.azure.com/forums/307516-sql-data-warehouse).
- [Creación de una incidencia de soporte técnico](sql-data-warehouse-get-started-create-support-ticket.md)
- Buscar en la [página de preguntas y respuestas de Microsoft](https://docs.microsoft.com/answers/topics/azure-synapse-analytics.html)
- Busque en el [foro Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sqldw).
