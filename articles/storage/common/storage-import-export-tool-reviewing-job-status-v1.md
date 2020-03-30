---
title: Revisión del estado de un trabajo de Azure Import/Export (versión 1) | Microsoft Docs
description: Obtenga información sobre cómo usar los archivos de registro creados cuando se ejecutó el trabajo de importación o exportación para ver el estado del trabajo de importación y exportación.
author: twooley
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/26/2017
ms.author: twooley
ms.subservice: common
ms.openlocfilehash: 33e6ecca0eb4ca05b1f6bf53cdb3bd4ac8c8d8a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75978436"
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a>Revisión del estado de un trabajo de Azure Import/Export con archivos de registro de copia
Cuando el servicio Microsoft Azure Import/Export procesa las unidades asociadas a un trabajo de importación o exportación, escribe archivos de registro de copia en la cuenta de almacenamiento con origen o destino en la ubicación donde está importando o exportando blobs. El archivo de registro contiene el estado detallado de cada archivo que se importó o exportó. La dirección URL de cada archivo de registro de copia se devuelve al consultar el estado de un trabajo completado; vea el artículo de [obtención de trabajos](https://docs.microsoft.com/rest/api/storageimportexport/Jobs/Get) para obtener más información.  

## <a name="example-urls"></a>URL de ejemplo

Las siguientes son direcciones URL de ejemplo de los archivos de registro de copia de un trabajo de importación con dos unidades:  

 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  

 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  

 Vea [Formato del archivo de registro del servicio Import-Export](../storage-import-export-file-format-log.md) para consultar el formato de los registros de copia y la lista completa de códigos de estado.  

## <a name="next-steps"></a>Pasos siguientes

 * [Configuración de la herramienta Azure Import/Export](storage-import-export-tool-setup-v1.md)   
 * [Preparación de unidades de disco duro para un trabajo de importación](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [Reparación de un trabajo de importación](../storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [Reparación de un trabajo de exportación](../storage-import-export-tool-repairing-an-export-job-v1.md)   
 * [Solución de problemas de la herramienta Azure Import/Export](storage-import-export-tool-troubleshooting-v1.md)
