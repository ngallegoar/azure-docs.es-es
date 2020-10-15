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
ms.openlocfilehash: fc58651bcb3b266b981fb953fd7341427d47fb2c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "76717576"
---
# <a name="move-data-to-and-from-azure-blob-storage"></a>Movimiento de datos hacia y desde Azure Blob Storage

El proceso de ciencia de datos en equipos requiere que los datos se introduzcan o se cargue en una variedad de entornos de almacenamiento diferentes para que se procesen o analicen de la manera más adecuada en cada fase del proceso.

## <a name="different-technologies-for-moving-data"></a>Distintas tecnologías para mover datos

Los artículos siguientes describen cómo mover datos hacia y desde Azure Blob storage mediante diferentes tecnologías.

* [Explorador de Azure Storage](move-data-to-azure-blob-using-azure-storage-explorer.md)
* [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10)
* [Python](move-data-to-azure-blob-using-python.md)
* [SSIS](move-data-to-azure-blob-using-ssis.md)

El método más adecuado para usted dependerá de su escenario. El artículo [Escenarios para análisis avanzado en Azure Machine Learning](plan-sample-scenarios.md) lo ayudará a determinar los recursos que necesita para una variedad de flujos de trabajo de ciencia de datos utilizados en el proceso de análisis avanzado.

> [!NOTE]
> Para ver una introducción completa a Azure Blob Storage, consulte [Aspectos básicos de Azure Blob](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) y [Azure Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).
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
* Para obtener instrucciones sobre cómo crear una cuenta de almacenamiento y cómo obtener información sobre la cuenta y la clave, consulte [Acerca de las cuentas de Azure Storage](../../storage/common/storage-create-storage-account.md).

