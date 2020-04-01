---
title: Uso de Azure Stream Analytics
description: Sugerencias para usar Azure Stream Analytics con almacenamiento de datos en Azure Synapse para el desarrollo de soluciones.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 2/5/2020
ms.author: martinle
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: feb7b52c84e5e702202bc668cfda676d291ea82e
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350434"
---
# <a name="use-azure-stream-analytics-with-azure-synapse-analytics"></a>Uso de Azure Stream Analytics con Azure Synapse Analytics

Azure Stream Analytics es un servicio totalmente administrado que proporciona un procesamiento completo de eventos de baja latencia, alta disponibilidad y escalable a través el streaming de datos en la nube. Para aprender los conceptos básicos, lea [Introducción a Azure Stream Analytics](../../stream-analytics/stream-analytics-introduction.md). Después puede aprender a crear una solución de extremo a extremo siguiendo el tutorial [Introducción al uso de Azure Stream Analytics](../../stream-analytics/stream-analytics-real-time-fraud-detection.md) .

En este artículo, aprenderá a usar el almacenamiento de datos como receptor de salida para los trabajos de Azure Stream Analytics.

## <a name="prerequisites"></a>Prerrequisitos

* Trabajo de Azure Stream Analytics: Para crear un trabajo de Azure Stream Analytics, siga los pasos descritos en el tutorial [Introducción al uso de Azure Stream Analytics](../../stream-analytics/stream-analytics-real-time-fraud-detection.md) relativos a lo siguiente:  

    1. Creación de una entrada de Centro de eventos
    2. Configuración e inicio de la aplicación del generador de eventos
    3. Aprovisionamiento de un trabajo de Stream Analytics
    4. Especificación de una consulta y entrada de trabajo
* Almacenamiento de datos de grupo de SQL de Azure Synapse: Para crear un almacenamiento de datos, siga los pasos descritos en [Inicio rápido: Creación de un almacenamiento de datos](https://docs.microsoft.com/azure/sql-data-warehouse/create-data-warehouse-portal).

## <a name="specify-streaming-output-to-point-to-your-data-warehouse"></a>Especificación de la salida de streaming para que apunte al almacenamiento de datos

### <a name="step-1"></a>Paso 1

En Azure Portal, vaya a su trabajo de Stream Analytics y haga clic en **Salidas** en el menú **Topología de trabajo**.

### <a name="step-2"></a>Paso 2

Haga clic en el botón **Agregar** y elija **SQL Database** en el menú desplegable.

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asaoutput.png)

### <a name="step-3"></a>Paso 3

Escriba los siguientes valores:

* *Alias de salida*: escriba un nombre descriptivo para esta salida de trabajo.
* *Suscripción*:
  * Si el almacenamiento de datos está en la misma suscripción que el trabajo de Stream Analytics, haga clic en ***Seleccionar SQL Database de las suscripciones***.
  * Si la base de datos está en otra suscripción, haga clic en Proporcionar configuración de Base de datos SQL de forma manual.
* *Base de datos*: Seleccione la base de datos de destino en la lista desplegable.
* *Nombre de usuario*: especifique el nombre de usuario de una cuenta que tenga permisos de escritura para la base de datos.
* *Contraseña*: proporcione la contraseña de la cuenta de usuario especificada.
* *Tabla*: especifique el nombre de la tabla de destino en la base de datos.
* Haga clic en el botón **Guardar**.

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asaoutputdbsettings.png)

### <a name="step-4"></a>Paso 4

Antes de poder ejecutar una prueba, deberá crear la tabla en el almacenamiento de datos.  Ejecute el siguiente script de creación de tabla con SQL Server Management Studio (SSMS) o la herramienta de consulta que prefiera.

```sql
CREATE TABLE SensorLog
(
    RecordType VARCHAR(2)
    , SystemIdentity VARCHAR(2)
    , FileNum INT
    , SwitchNum VARCHAR(50)
    , CallingNum VARCHAR(25)
    , CallingIMSI VARCHAR(25)
    , CalledNum VARCHAR(25)
    , CalledIMSI VARCHAR(25)
    , DateS VARCHAR(25)
    , TimeS VARCHAR(25)
    , TimeType INT
    , CallPeriod INT
    , CallingCellID VARCHAR(25)
    , CalledCellID VARCHAR(25)
    , ServiceType VARCHAR(25)
    , [Transfer] INT
    , IncomingTrunk VARCHAR(25)
    , OutgoingTrunk VARCHAR(25)
    , MSRN VARCHAR(25)
    , CalledNum2 VARCHAR(25)
    , FCIFlag VARCHAR(25)
    , callrecTime VARCHAR(50)
    , EventProcessedUtcTime VARCHAR(50)
    , PartitionId int
    , EventEnqueuedUtcTime VARCHAR(50)
    )
WITH (DISTRIBUTION = ROUND_ROBIN)
```

### <a name="step-5"></a>Paso 5

En Azure Portal para su trabajo de Stream Analytics, haga clic en el nombre del trabajo.  Haga clic en el botón ***Probar*** en el panel ***Detalles de salida***.

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asatest.png) Cuando la conexión a la base de datos se realice correctamente, verá una notificación en el portal.

### <a name="step-6"></a>Paso 6

Haga clic en el menú ***Consulta*** en ***Topología de trabajo*** y cambie la consulta para insertar datos en la salida de flujo que ha creado.  Haga clic en el botón ***Probar consulta seleccionada*** para probar la consulta.  Haga clic en el botón ***Guardar consulta*** cuando la prueba de la consulta sea correcta.

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asaquery.png)

### <a name="step-7"></a>Paso 7

Inicie el trabajo de Azure Stream Analytics.  Haga clic en el botón ***Iniciar*** en el menú ***Información general***.

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asastart.png)

Haga clic en el botón ***Iniciar*** en el panel Iniciar trabajo.

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asastartconfirm.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener información general sobre la integración, consulte [Integrar otros servicios](sql-data-warehouse-overview-integrate.md).
Para obtener más consejos de desarrollo, consulte [Decisiones de diseño y técnicas de codificación para almacenamientos de datos](sql-data-warehouse-overview-develop.md).
