---
title: 'Movimiento de datos de Blob Storage con conectores SSIS: Team Data Science Process'
description: Aprenda a mover los datos hacia o desde Azure Blob Storage mediante SQL Server Integration Services Feature Pack para Azure.
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
ms.openlocfilehash: 9572d612e7ee8e2fd72850ba14447e8449f0f371
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93322012"
---
# <a name="move-data-to-or-from-azure-blob-storage-using-ssis-connectors"></a>Movimiento de datos desde o hacia Azure Blob Storage con conectores SSIS
El [Feature Pack de SQL Server Integration Services para Azure](/sql/integration-services/azure-feature-pack-for-integration-services-ssis) ofrece componentes para conectarse a Azure, transferir datos entre Azure y orígenes de datos locales y procesar datos almacenados en Azure.

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

Una vez que los clientes han movido los datos locales a la nube, pueden acceder a sus datos desde cualquier servicio de Azure para aprovechar al máximo el conjunto de tecnologías de Azure. Posteriormente, los datos se pueden usar, por ejemplo, en Azure Machine Learning o en un clúster de HDInsight.

Los ejemplos de uso de estos recursos de Azure se encuentran en los tutoriales de [SQL](sql-walkthrough.md) y [HDInsight](hive-walkthrough.md).

Si desea ver un análisis de los escenarios canónicos que usan SSIS para satisfacer las necesidades de negocio comunes en escenarios de integración de datos híbridos, visite el blog [Doing more with SQL Server Integration Services Feature Pack for Azure](https://techcommunity.microsoft.com/t5/sql-server-integration-services/doing-more-with-sql-server-integration-services-feature-pack-for/ba-p/388238) (Aprovechar más el Feature Pack de SQL Server Integration Services para Azure).

> [!NOTE]
> Para ver una introducción completa a Azure Blob Storage, consulte [Aspectos básicos de Azure Blob](../../storage/blobs/storage-quickstart-blobs-dotnet.md) y [Azure Blob Service](/rest/api/storageservices/Blob-Service-Concepts).
> 
> 

## <a name="prerequisites"></a>Prerrequisitos
Para realizar las tareas que se describen en este artículo, debe tener configurada una suscripción a Azure y una cuenta de Azure Storage. El nombre y la clave de la cuenta de Azure Storage se necesitan para cargar o descargar datos.

* Para configurar una **suscripción a Azure** , consulte [Prueba gratuita de un mes](https://azure.microsoft.com/pricing/free-trial/).
* Para obtener instrucciones sobre cómo crear una **cuenta de almacenamiento** , así como para obtener información sobre la cuenta y la clave, consulte [Acerca de las cuentas de Azure Storage](../../storage/common/storage-account-create.md).

Para usar los **conectores SSIS** , debe descargar:

* **SQL Server 2014 o 2016 Standard (o superior)** : la instalación incluye SQL Server Integration Services.
* **Microsoft SQL Server 2014 o 2016 Integration Services Feature Pack para Azure** : estos conectores se pueden descargar, respectivamente, en las páginas [SQL Server 2014 Integration Services](https://www.microsoft.com/download/details.aspx?id=47366) y [SQL Server 2016 Integration Services](https://www.microsoft.com/download/details.aspx?id=49492).

> [!NOTE]
> SSIS se instala con SQL Server, pero no está incluido en la versión Express. Para obtener información sobre qué aplicaciones se incluyen en las distintas versiones de SQL Server, consulte [Ediciones de SQL Server](https://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)
> 
> 

Para obtener materiales de aprendizaje sobre SSIS, consulte [Aprendizaje práctico para SSIS](https://www.microsoft.com/sql-server/training-certification)

Si desea obtener información sobre cómo trabajar con SISS para crear paquetes de extracción, transformación y carga (ETL) sencillos, consulte [Tutorial de SSIS: Creación de un paquete ETL sencillo](/sql/integration-services/ssis-how-to-create-an-etl-package).

## <a name="download-nyc-taxi-dataset"></a>Descargar el conjunto de datos de taxis de la ciudad de Nueva York
El ejemplo aquí descrito usa un conjunto de datos disponible para todo público, el conjunto de datos [NYC Taxi Trips](https://www.andresmh.com/nyctaxitrips/) (Viajes en taxi en la ciudad de Nueva York). El conjunto de datos consta de alrededor de 173 millones de viajes en taxi que se realizaron en Nueva York durante el año 2013. Existen dos tipos de datos: datos de los detalles de las carreras y datos sobre las tarifas. Como existe un archivo correspondiente a cada mes, hay 24 archivos, cada uno de los cuales tiene un tamaño aproximado de 2 GB sin comprimir.

## <a name="upload-data-to-azure-blob-storage"></a>Carga de datos en el almacenamiento de blobs de Azure
Para mover los datos con el Feature Pack de SSIS desde un almacenamiento local a Blob Storage de Azure, usamos una instancia de la [**tarea de carga de blobs de Azure**](/sql/integration-services/control-flow/azure-blob-upload-task), que aparece a continuación:

![configure-data-science-vm](./media/move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)

A continuación, se describen los parámetros que usa la tarea:

| Campo | Descripción |
| --- | --- |
| **AzureStorageConnection** |Especifica un Administrador de conexiones de Azure Storage existente o crea uno nuevo que hace referencia a una cuenta de Azure Storage que apunta a la ubicación en que se almacenan los archivos de blob. |
| **BlobContainer** |Especifica el nombre del contenedor de blobs que contiene los archivos cargados como blobs. |
| **BlobDirectory** |Especifica el directorio de blobs donde se almacena el archivo cargado como un blob en bloques. El directorio de blobs es una estructura jerárquica virtual. Si el blob ya existe, se reemplaza. |
| **LocalDirectory** |Especifica el directorio local que contiene los archivos que se cargarán. |
| **FileName** |Especifica un nombre de filtro para seleccionar archivos con el patrón de nombre especificado. Por ejemplo, MySheet\*.xls\* incluye archivos como MySheet001.xls y MySheetABC.xlsx |
| **TimeRangeFrom/TimeRangeTo** |Especifica un filtro de intervalo de tiempo. Se incluyen los archivos modificados después de *TimeRangeFrom* y antes de *TimeRangeTo*. |

> [!NOTE]
> Es necesario corregir las credenciales de **AzureStorageConnection** y se debe salir de **BlobContainer** antes de intentar realizar la transferencia.
> 
> 

## <a name="download-data-from-azure-blob-storage"></a>Cargar datos desde el almacenamiento de blobs de Azure
Para descargar datos desde Azure Blob Storage a un almacenamiento local con SSIS, use una instancia de [Tarea de descarga de blobs de Azure](/sql/integration-services/control-flow/azure-blob-download-task).

## <a name="more-advanced-ssis-azure-scenarios"></a>Escenarios de SSIS-Azure más avanzados
El Feature Pack de SSIS permite administrar flujos más complejos al empaquetar juntas las tareas. Por ejemplo, los datos de blobs se podrían ingresar directamente en un clúster de HDInsight cuya salida se podría descargar nuevamente a un blob y, luego, a un almacenamiento local. SSIS puede ejecutar trabajos de Hive y Pig en un clúster de HDInsight, mediante el uso de conectores SSIS adicionales:

* Para ejecutar un script de Hive en un clúster de HDInsight de Azure con SSIS, use [Tarea de Hive para HDInsight de Azure](/sql/integration-services/control-flow/azure-hdinsight-hive-task).
* Para ejecutar un script de Pig en un clúster de HDInsight de Azure con SSIS, use [Tarea de Pig para HDInsight de Azure](/sql/integration-services/control-flow/azure-hdinsight-pig-task).