---
title: Preguntas frecuentes sobre Azure Synapse Analytics (anteriormente SQL DW)
description: En este artículo se muestran las preguntas más frecuentes sobre Azure Synapse Analytics (anteriormente SQL DW) para clientes y desarrolladores
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: ef8196cebfbdda72f98fee0e9c75bcb80172a55e
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/05/2020
ms.locfileid: "82791466"
---
# <a name="azure-synapse-analytics-formerly-sql-dw-frequently-asked-questions"></a>Preguntas frecuentes sobre Azure Synapse Analytics (anteriormente SQL DW)

## <a name="general"></a>General

Q. ¿Qué es Azure Synapse?

A. Azure Synapse es un servicio de análisis que engloba el almacenamiento de datos y el análisis de macrodatos. Azure Synapse reúne estos dos mundos con una experiencia unificada para ingerir, preparar, administrar y servir datos para las necesidades de inteligencia empresarial y aprendizaje automático. Para más información, consulte [¿Qué es Azure Synapse Analytics?](sql-data-warehouse-overview-what-is.md)

Q. ¿Qué ha pasado con Azure SQL Data Warehouse?

A. Azure Synapse es Azure SQL Data Warehouse (SQL DW) evolucionado. Hemos llevado el mismo almacenamiento de datos líder del sector a un nuevo nivel de rendimiento y funcionalidades. Puede seguir ejecutando las cargas de trabajo de almacenamiento de datos existentes en producción con Azure Synapse. Para más información, consulte [¿Qué es Azure Synapse Analytics?](sql-data-warehouse-overview-what-is.md)

Q. ¿Qué es un grupo de SQL de Synapse?

A. El grupo de SQL de Synapse hace referencia a las características de almacenamiento de datos empresariales que están disponibles con carácter general en Azure Synapse. Para más información, consulte [¿Qué es Azure Synapse Analytics?](sql-data-warehouse-overview-what-is.md)

Q. ¿Cómo puedo empezar a usar Azure Synapse?

A. Puede empezar a trabajar con una [cuenta gratuita de Azure](https://azure.microsoft.com/free/sql-data-warehouse/) o [ponerse en contacto con Ventas para más información](https://info.microsoft.com/ww-landing-azure-sql-data-warehouse-contactme.html).

Q. ¿Qué ofrece Azure Synapse en cuanto a la seguridad de datos?

A. Azure Synapse ofrece varias soluciones para proteger datos, como TDE y auditorías. Para obtener más información, consulte [Seguridad](sql-data-warehouse-overview-manage-security.md).

Q. ¿Dónde puedo averiguar sobre los estándares empresariales o legales que cumple Azure Synapse?

A. Visite la página [Cumplimiento normativo](https://www.microsoft.com/trustcenter/compliance/complianceofferings) para consultar diversas ofertas de cumplimiento por producto, como SOC e ISO. En primer lugar, elija por título Cumplimiento. Luego, expanda Azure en la sección de servicios en la nube de Microsoft del lado derecho de la página para ver qué servicios son compatibles con Azure Synapse.

Q. ¿Puedo conectar Power BI?

A. Sí. Aunque Power BI admite la consulta directa con Azure Synapse, no se ha diseñado para administrar muchos usuarios ni datos en tiempo real. Para optimizar aún más el rendimiento de Power BI, considere la posibilidad de usar Power BI en Azure Analysis Services o la infraestructura como servicio de Analysis Services.

Q. ¿Cuáles son los límites de capacidad del grupo de SQL de Synapse?

A. Consulte nuestra página de [límites de capacidad](sql-data-warehouse-service-capacity-limits.md) actuales.

Q. ¿Por qué tardan tanto tiempo en completarse las operaciones de escalado, pausar y reanudación?

A. Varios factores pueden influir en el tiempo que tardan en completarse las operaciones de administración de procesos. Un caso habitual de operaciones de larga duración es la reversión de transacciones. Cuando se inicia una operación de escalado o pausa, se bloquean todas las sesiones entrantes y se purgan las consultas. Para dejar el sistema en un estado estable, las transacciones se deben revertir antes de que pueda comenzar una operación. Cuanto mayor sea su número y el tamaño del registro de las transacciones, más tiempo estará detenida la operación al restaurar el sistema a un estado estable.

## <a name="user-support"></a>Asistencia técnica de usuario

Q. Tengo una solicitud de característica, ¿dónde puede enviarla?

A. Si tiene una solicitud de característica, envíala a nuestra página [UserVoice](https://feedback.azure.com/forums/307516-sql-data-warehouse).

Q. ¿Cómo puedo hacer una determinada acción?

A. Para obtener ayuda con las tareas de desarrollo con Azure Synapse, puede hacer preguntas en nuestra página [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sqldw).

Q. ¿Cómo puedo enviar una vale de asistencia técnica?

A. Los [Vales de asistencia técnica](sql-data-warehouse-get-started-create-support-ticket.md) puede presentarse a través de Azure Portal.

## <a name="sql-languagefeature-support"></a>Compatibilidad con características o lenguajes de SQL

Q. ¿Qué tipos de datos se admiten?

A. Consulte [Tipos de datos](sql-data-warehouse-tables-data-types.md).

Q. ¿Qué características de tablas se admiten?

A. Se admiten muchas características. Las características que no se admiten se pueden encontrar en la [tabla de características no admitidas](sql-data-warehouse-tables-data-types.md).

## <a name="tooling-and-administration"></a>Administración y herramientas

Q. ¿Admite el grupo de SQL de Synapse las API REST?

A. Sí. La mayoría de las funciones de REST que se pueden utilizar con SQL Database también están disponibles en el grupo de SQL de Synapse. Puede encontrar información sobre las API en las páginas de documentación de REST o las [bases de datos](/rest/api/sql/databases?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

## <a name="loading"></a>Carga

Q. ¿Qué controladores de cliente se admiten?

A. La compatibilidad de los controladores con el grupo de SQL de Synapse puede encontrarse en la página [Cadenas de conexión](../sql/connection-strings.md).

P: ¿Qué formatos de archivo admite PolyBase?

A. Orc, RC, Parquet y texto delimitado sin formato.

P: ¿A qué orígenes de datos puedo conectarme mediante PolyBase?

A. [Azure Data Lake Storage](sql-data-warehouse-load-from-azure-data-lake-store.md) y [Azure Storage Blobs](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md).

P: ¿Se puede usar el cálculo de aplicación al conectarse a instancias de Azure Storage Blob o ADLS?

A. No, PolyBase interactúa solo con los componentes de almacenamiento.

P: ¿Puedo conectarlo a HDI?

A. HDI puede usar ADLS o WASB como capa HDFS. Si tiene uno de los dos como la capa HDFS, puede cargar datos en el grupo de SQL de Synapse. Sin embargo, no se puede generar el cálculo de aplicación en la instancia HDI.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre Azure Synapse, vea nuestra página [Introducción](sql-data-warehouse-overview-faq.md).
