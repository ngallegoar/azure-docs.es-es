---
title: archivo de inclusión
description: archivo de inclusión
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: include
ms.date: 10/09/2019
ms.author: abnarain
ms.openlocfilehash: 6af6795fefb41f1d8f2b56e0aa1fb367fc18cee2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "74559295"
---
| Nombres de dominio                  | Puertos de salida | Descripción                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.windows.net`    | 443            | El entorno de ejecución de integración autohospedado lo necesita para conectarse a los servicios de movimiento de datos de Azure Data Factory. |
| `*.frontend.clouddatahub.net` | 443            | El entorno de ejecución de integración autohospedado lo necesita para conectarse al servicio Data Factory. |
| `download.microsoft.com`    | 443            | Lo necesita el entorno de ejecución de integración autohospedado para descargar las actualizaciones. Si ha deshabilitado la actualización automática, puede omitir la configuración de este dominio. |
| `*.core.windows.net`          | 443            | Lo usa el entorno de ejecución de integración autohospedado para conectarse a la cuenta de Azure Storage cuando se utiliza la característica [Copia almacenada provisionalmente](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#staged-copy). |
| `*.database.windows.net`      | 1433           | Solo es necesario cuando se copia desde o en Azure SQL Database o Azure SQL Data Warehouse, sino es opcional. Use la característica de copia almacenada provisionalmente para copiar datos en SQL Database o SQL Data Warehouse sin abrir el puerto 1433. |
| `*.azuredatalakestore.net`<br>`login.microsoftonline.com/<tenant>/oauth2/token`    | 443            | Solo es necesario cuando se copia desde o en Azure Data Lake Store, sino es opcional. |
