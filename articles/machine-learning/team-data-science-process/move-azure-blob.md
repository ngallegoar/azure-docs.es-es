---
title: 'Mover datos hacia y desde Azure Blob Storage: proceso de ciencia de datos en equipos'
description: Movimiento de los datos hacia y desde Azure Blob Storage con el Explorador de Azure Storage, AzCopy, Python y SSIS
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
ms.openlocfilehash: 5148084fa22266b1352046c7d8737b9804c5f4d0
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93311851"
---
# <a name="move-data-to-and-from-azure-blob-storage"></a>Movimiento de datos hacia y desde Azure Blob Storage

El proceso de ciencia de datos en equipos requiere que los datos se introduzcan o se cargue en una variedad de entornos de almacenamiento diferentes para que se procesen o analicen de la manera más adecuada en cada fase del proceso.

## <a name="different-technologies-for-moving-data"></a>Distintas tecnologías para mover datos

Los artículos siguientes describen cómo mover datos hacia y desde Azure Blob storage mediante diferentes tecnologías.

* [Explorador de Azure Storage](move-data-to-azure-blob-using-azure-storage-explorer.md)
* [AzCopy](../../storage/common/storage-use-azcopy-v10.md)
* [Python](../../storage/blobs/storage-quickstart-blobs-python.md)
* [SSIS](move-data-to-azure-blob-using-ssis.md)

El método más adecuado para usted dependerá de su escenario. El artículo [Escenarios para análisis avanzado en Azure Machine Learning](plan-sample-scenarios.md) lo ayudará a determinar los recursos que necesita para una variedad de flujos de trabajo de ciencia de datos utilizados en el proceso de análisis avanzado.

> [!NOTE]
> Para ver una introducción completa a Azure Blob Storage, consulte [Aspectos básicos de Azure Blob](../../storage/blobs/storage-quickstart-blobs-dotnet.md) y [Azure Blob Service](/rest/api/storageservices/Blob-Service-Concepts).
> 
> 

## <a name="using-azure-data-factory"></a>Uso de Azure Data Factory

Como alternativa, puede usar [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) para: 

* crear y programar una canalización que descarga los datos desde Azure Blob Storage, 
* pasarla a un servicio web Azure Machine Learning publicado, 
* recibir los resultados de análisis predictivo y 
* cargar los resultados al almacenamiento. 

Consulte [Creación de canalizaciones predictivas mediante Factoría de datos de Azure y Azure Machine Learning](../../data-factory/transform-data-using-machine-learning.md)para obtener más información.

## <a name="prerequisites"></a>Prerequisites
En este artículo se presupone que tiene una suscripción de Azure, una cuenta de almacenamiento y la clave de almacenamiento correspondiente para dicha cuenta. Antes de cargar o descargar datos, debe conocer su nombre de cuenta de Azure Storage y la clave de cuenta.

* Para configurar una suscripción a Azure, consulte [Prueba gratuita de un mes](https://azure.microsoft.com/pricing/free-trial/).
* Para obtener instrucciones sobre cómo crear una cuenta de almacenamiento y cómo obtener información sobre la cuenta y la clave, consulte [Acerca de las cuentas de Azure Storage](../../storage/common/storage-account-create.md).