---
title: 'Carga de datos en entornos de Azure Storage: proceso de ciencia de datos en equipos'
description: Obtenga información sobre cómo ingerir datos en varios entornos de destino donde los datos se almacenan y se procesan.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 46e911ad6bdf5a478d46e425f8700740ece56c6e
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96452664"
---
# <a name="load-data-into-storage-environments-for-analytics"></a>Carga de datos en entornos de almacenamiento para el análisis

El proceso de ciencia de datos en equipos requiere que los datos se ingieran o se carguen de la manera más adecuada en cada fase. Entre los destinos de datos se incluyen Azure Blob Storage, instancias de Azure SQL Database, SQL Server en VM de Azure, HDInsight (Hadoop), Azure Synapse Analytics y Azure Machine Learning. 

En los artículos siguientes se describe cómo introducir datos en varios entornos de destino donde los datos se almacenan y se procesan.

* Hacia o desde [Azure Blob Storage](move-azure-blob.md)
* Hacia [SQL Server en máquinas virtuales de Azure](move-sql-server-virtual-machine.md)
* Hacia [Azure SQL Database](move-sql-azure.md)
* Hacia [tablas de Hive](move-hive-tables.md)
* Hacia [tablas SQL con particiones](parallel-load-sql-partitioned-tables.md)
* Desde [SQL Server local](move-sql-azure-adf.md)

Las necesidades técnicas y empresariales, así como la ubicación inicial, el formato y el tamaño de los datos, determinarán el mejor plan de ingesta de datos. No es raro que un plan más adecuado tenga varios pasos. En esta secuencia de tareas se puede incluir, por ejemplo, la exploración de datos, el procesamiento previo, la limpieza, el muestreo inferior y el entrenamiento del modelo.  Azure Data Factory es un recurso de Azure recomendado para orquestar el movimiento y la transformación de los datos.
