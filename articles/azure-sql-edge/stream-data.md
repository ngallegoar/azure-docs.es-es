---
title: Streaming de datos en Azure SQL Edge (versión preliminar)
description: Información sobre el streaming de datos en Azure SQL Edge (versión preliminar)
keywords: ''
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 15b6b80f9924b050b388d74adc1d67db6a386134
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83594514"
---
# <a name="data-streaming-in-azure-sql-edge-preview"></a>Streaming de datos en Azure SQL Edge (versión preliminar)

Azure SQL Edge (versión preliminar) proporciona dos opciones diferentes para implementar el streaming de datos. 

1. Implementación de trabajos de Edge de Azure Streaming Analytics creados en Azure. Para más información sobre cómo implementar trabajos de Edge de Azure Streaming Analytics en Azure SQL Edge, consulte [Implementación de trabajos de Azure Stream Analytics](deploy-dacpac.md).
2. Uso de la nueva característica de **streaming de T-SQL** para crear trabajos de streaming en SQL Edge sin necesidad de configurar trabajos de streaming en Azure. 

Aunque es posible usar ambas opciones para implementar el streaming de datos en SQL Edge, se recomienda encarecidamente usar solo una. Cuando se usan ambas, puede haber posibles condiciones de carrera que afecten al funcionamiento de las operaciones de streaming de datos.

El resto de este documento hace referencia a la nueva característica, **streaming de T-SQL**, que proporciona streaming de datos en tiempo real, análisis y procesamiento de eventos para analizar y procesar grandes volúmenes de datos de streaming rápidos de varios orígenes simultáneamente. *Streaming de T-SQL* se crea con el mismo motor de streaming de alto rendimiento que sustenta [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-introduction) en Microsoft Azure y admite un conjunto similar de funcionalidades que ofrece Azure Stream Analytics que se ejecutan en el borde.

Al igual que con Azure Stream Analytics, el streaming de T-SQL permite el reconocimiento de patrones y relaciones en la información extraída de varios orígenes de entrada de IoT, incluidos dispositivos, sensores y aplicaciones. Estos patrones se pueden usar para desencadenar acciones e iniciar flujos de trabajo, como la creación de alertas, la provisión de información a una solución de visualización o generación de informes o el almacenamiento de datos para usarlos posteriormente. 

Los escenarios siguientes son ejemplos de cuándo puede usar el streaming de T-SQL:

* Análisis de los flujos de telemetría en tiempo real desde dispositivos IoT.
* Análisis en tiempo real de los datos generados a partir de vehículos autónomos y sin controlador.
* Supervisión remota y mantenimiento predictivo de recursos industriales o de fabricación de gran valor.
* Detección de anomalías o reconocimiento de patrones de lecturas del sensor de IoT en una granja agrícola o energética.

## <a name="how-does-t-sql-streaming-work"></a>¿Cómo funciona el streaming de T-SQL?

El streaming de T-SQL funciona exactamente de la misma manera que [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-introduction#how-does-stream-analytics-work), por ejemplo, usa el concepto de trabajos de streaming para el procesamiento de streaming de datos en tiempo real. 

Un trabajo de Stream Analytics consta de lo siguiente:

- Entrada de flujo: una entrada de flujo define las conexiones a un origen de datos del que se va a leer el flujo de datos. Actualmente Azure SQL Edge admite los siguientes tipos de entradas de flujo:
    - Centro de Microsoft Edge
    - Kafka: la compatibilidad con las entradas de Kafka solo está disponible actualmente en las versiones Intel/AMD64 de Azure SQL Edge.

- Salida de flujo: una salida de flujo define las conexiones a un origen de datos en el que se va a escribir el flujo de datos. Azure SQL Edge actualmente admite los siguientes tipos de salidas de flujo:
    - Centro de Microsoft Edge
    - SQL: la salida de SQL puede ser una base de datos local dentro de la instancia de SQL Edge o una instancia remota de SQL Server o Azure SQL Database. 
    - Azure Blob Storage

- Consulta de flujo: la consulta de flujo define la transformación, las agregaciones, el filtro, la ordenación y las combinaciones que deben aplicarse al flujo de entrada antes de que se escriba en la salida de flujo. La consulta de flujo se basa en el mismo lenguaje de consulta que utiliza Azure Stream Analytics. Para obtener más información sobre el lenguaje de consulta de Azure Stream Analytics, consulte [Lenguaje de consulta de Stream Analytics del](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference?).

> [!IMPORTANT]
> El streaming de T-SQL, a diferencia de Azure Stream Analytics, actualmente no admite el [uso de datos de referencia para búsquedas](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-use-reference-data) ni el [uso de UDF y UDA en el trabajo de streaming](https://docs.microsoft.com/azure/stream-analytics/streaming-technologies#you-want-to-write-udfs-udas-and-custom-deserializers-in-a-language-other-than-javascript-or-c).

> [!NOTE]
> El streaming de T-SQL solo admite un subconjunto del área expuesta de lenguaje compatible con Azure Stream Analytics. Para obtener más información sobre el lenguaje de consulta de Azure Stream Analytics, consulte [Lenguaje de consulta de Stream Analytics del](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference?).

## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones

Las limitaciones y restricciones siguientes se aplican al streaming de T-SQL. 

- Solo puede haber un trabajo de streaming activo en un momento dado. Los trabajos que ya se están ejecutando deben detenerse antes de iniciar otro trabajo.
- Cada ejecución de trabajo de streaming tiene un único subproceso. Si el trabajo de streaming contiene varias consultas, cada consulta se evaluará en orden de serie.

## <a name="next-steps"></a>Pasos siguientes

- [Creación de un trabajo de Stream Analytics en Azure SQL Edge (versión preliminar)](create-stream-analytics-job.md)
- [Visualización de los metadatos asociados a los trabajos de streaming en Azure SQL Edge (versión preliminar)](streaming-catalog-views.md)
- [Creación de un flujo externo](create-external-stream-transact-sql.md)