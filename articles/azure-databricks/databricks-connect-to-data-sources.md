---
title: 'Conexión a diferentes orígenes de datos de Azure Databricks '
description: Aprenda a conectarse a Azure SQL Database, Azure Data Lake Store, Blob Storage, Cosmos DB, Event Hubs y Azure SQL Data Warehouse desde Azure Databricks.
services: azure-databricks
author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 03/21/2018
ms.author: mamccrea
ms.openlocfilehash: 79a821a4c8fe4cb2d048f0dcb0a6e091462a1779
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548791"
---
# <a name="connect-to-data-sources-from-azure-databricks"></a>Conectarse a orígenes de datos de Azure Databricks

En este artículo se proporcionan vínculos a todos los diferentes orígenes de datos de Azure que pueden conectarse a Azure Databricks. Siga los ejemplos que se muestran en estos vínculos para extraer datos de los orígenes de datos de Azure (por ejemplo, Azure Blob Storage, Azure Event Hubs, etc.) en un clúster de Azure Databricks, y ejecutar trabajos analíticos en ellos. 

## <a name="prerequisites"></a>Prerequisites

* Debe disponer de un área de trabajo de Azure Databricks y de un clúster de Spark. Siga las instrucciones que se muestran en [Introducción a Azure Databricks](quickstart-create-databricks-workspace-portal.md).

## <a name="data-sources-for-azure-databricks"></a>Orígenes de datos de Azure Databricks

En la lista siguiente se proporcionan los orígenes de datos de Azure que puede usar con Azure Databricks. Para obtener una lista completa de los orígenes de datos que pueden usarse con Azure Databricks, consulte los [orígenes de datos de Azure Databricks](/azure/databricks/data/data-sources/index).

- [Azure SQL Database](/azure/databricks/data/data-sources/sql-databases)

    Este vínculo permite que la API de DataFrame se conecte a bases de datos SQL con JDBC e indica cómo controlar el paralelismo de lecturas a través de la interfaz JDBC. En este tema se proporcionan ejemplos detallados con la API de Scala, con ejemplos abreviados de Python y Spark al final.
- [Almacén de Azure Data Lake](/azure/databricks/data/data-sources/azure/azure-datalake-gen2)

    En este vínculo se proporcionan ejemplos acerca de cómo utilizar la entidad de servicio de Azure Active Directory para autenticarse con Azure Data Lake Storage. También proporciona instrucciones acerca de cómo obtener acceso a los datos de Azure Data Lake Storage desde Azure Databricks.

- [Azure Blob Storage](/azure/databricks/data/data-sources/azure/azure-storage)

    En este vínculo se proporcionan ejemplos acerca de cómo obtener acceso directo a Azure Blob Storage desde Azure Databricks con la clave de acceso o la SAS de un determinado contenedor. El vínculo también proporciona información acerca de cómo obtener acceso a Azure Blob Storage desde Azure Databricks mediante la API de RDD.

- [Azure Cosmos DB](/azure/databricks/data/data-sources/azure/cosmosdb-connector)

    En este vínculo se proporcionan instrucciones acerca de cómo usar el [conector de Azure Cosmos DB Spark](https://github.com/Azure/azure-cosmosdb-spark) desde Azure Databricks para obtener acceso a los datos en de Azure Cosmos DB.

- [Azure Event Hubs](/azure/event-hubs/event-hubs-spark-connector)

    En este vínculo se proporcionan instrucciones acerca de cómo usar el [conector de Azure Event Hubs Spark](https://github.com/Azure/azure-event-hubs-spark) desde Azure Databricks para obtener acceso a los datos de Azure Event Hubs.

- [Azure SQL Data Warehouse](/azure/synapse-analytics/sql-data-warehouse/)

    En este vínculo se proporcionan instrucciones acerca de cómo usar el conector de Azure SQL Data Warehouse para conectarse desde Azure Databricks.
    

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de los orígenes desde donde se pueden importar datos a Azure Databricks, consulte los [orígenes de datos de Azure Databricks](/azure/databricks/data/data-sources/index).


