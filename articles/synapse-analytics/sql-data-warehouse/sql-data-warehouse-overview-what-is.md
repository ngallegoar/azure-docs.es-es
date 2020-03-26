---
title: ¿Qué es Azure Synapse Analytics (anteriormente SQL Data Warehouse)?
description: Azure Synapse Analytics (anteriormente SQL Data Warehouse) es un servicio de análisis ilimitado que reúne el almacenamiento de datos empresariales y el análisis de macrodatos.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: overview
ms.subservice: design
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: b6a718b45ee834bbd0b75c56e0a187c197dd7fa0
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80148204"
---
# <a name="what-is-azure-synapse-analytics-formerly-sql-dw"></a>¿Qué es Azure Synapse Analytics (anteriormente SQL Data Warehouse)?

Azure Synapse es un servicio de análisis ilimitado que reúne el almacenamiento de datos empresariales y el análisis de macrodatos. Le ofrece la libertad de consultar los datos como prefiera, ya sea a petición sin servidor o con recursos aprovisionados, a escala. Azure Synapse reúne estos dos mundos con una experiencia unificada para ingerir, preparar, administrar y servir datos para las necesidades inmediatas de inteligencia empresarial y aprendizaje automático.

Azure Synapse tiene cuatro componentes:
- SQL Analytics: Análisis basado en T-SQL completo: disponible con carácter general
    - Grupo de SQL (pago por DWU aprovisionado) 
    - SQL a petición (pago por TB procesados): (versión preliminar).
- Spark: profunda integración con Apache Spark (versión preliminar). 
- Integración de datos: integración de datos híbridos (versión preliminar).
- Studio: Experiencia de usuario unificada  (versión preliminar)

> [!NOTE]
> Para acceder a las características en versión preliminar de Azure Synapse, solicite acceso [aquí](https://aka.ms/synapsepreview). Microsoft evaluará todas las solicitudes y responderá lo antes posible.

## <a name="sql-analytics-and-sql-pool-in-azure-synapse"></a>SQL Analytics y el grupo de SQL en Azure Synapse

SQL Analytics hace referencia a las características de almacenamiento de datos empresariales que están disponibles con carácter general en Azure Synapse. 

El grupo de SQL representa una colección de recursos de análisis que se aprovisionan al usar SQL Analytics. El tamaño del grupo de SQL lo determinan las unidades de almacenamiento de datos (DWU).

Importe macrodatos con consultas T-SQL de [PolyBase](/sql/relational-databases/polybase/polybase-guide?view=sql-server-2017&viewFallbackFrom=azure-sqldw-latest) simples y, después, use la potencia de MPP para realizar análisis de alto rendimiento. Al realizar la integración y el análisis, SQL Analytics pasará a ser la versión única de certeza con la que puede contar su empresa para obtener información.  

## <a name="key-component-of-a-big-data-solution"></a>Componente clave de una solución de macrodatos

El almacenamiento de datos un componente clave de una solución de macrodatos de un extremo a otro en la nube.

![Solución de almacenamiento de datos](./media/sql-data-warehouse-overview-what-is/data-warehouse-solution.png) 

En una solución de datos en la nube, los datos provenientes de varios orígenes se ingieren en almacenes de macrodatos. Una vez que están en un almacén de macrodatos, Hadoop, y Spark y los algoritmos de aprendizaje automático preparan y entrenan los datos. Cuando los datos están listos para el análisis complejo, SQL Analytics usa PolyBase para consultar los almacenes de macrodatos. PolyBase usa las consultas T-SQL estándar para llevar los datos a tablas de SQL Analytics.
 
SQL Analytics almacena los datos en tablas relacionales con almacenamiento en columnas. Este formato reduce considerablemente los costos de almacenamiento de datos y mejora el rendimiento de las consultas. Una vez que los datos están almacenados, se pueden realizar análisis a gran escala. En comparación con los sistemas de bases de datos tradicionales, las consultas de análisis finalizan en segundos, en lugar de minutos, o en horas, en lugar de días. 

Los resultados del análisis pueden ir a aplicaciones o bases de datos de informes mundiales. En ellas, los analistas empresariales pueden obtener información útil para tomar decisiones empresariales bien informadas.

## <a name="next-steps"></a>Pasos siguientes

- Exploración de la [arquitectura de Azure Synapse](massively-parallel-processing-mpp-architecture.md)
- [Creación rápida de un grupo de SQL](create-data-warehouse-portal.md)
- [Cargue datos de ejemplo](load-data-from-azure-blob-storage-using-polybase.md).
- Explore [vídeos](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse).

O bien, examine algunos de estos otros recursos de Azure Synapse.  
* Busque información en [Blogs](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/).
* Envíe una [solicitud de característica](https://feedback.azure.com/forums/307516-sql-data-warehouse).
* [Creación de una incidencia de soporte técnico](sql-data-warehouse-get-started-create-support-ticket.md)
* Busque en el [foro de MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureSQLDataWarehouse).
* Busque en el [foro Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sqldw).
