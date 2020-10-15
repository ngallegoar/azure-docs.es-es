---
title: archivo de inclusión
description: archivo de inclusión
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: include
ms.date: 10/09/2019
ms.author: abnarain
ms.openlocfilehash: 0b2831321c9f4d855a19605c1ce5ace9fa11c88b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "89596128"
---
| Nombres de dominio                  | Puertos de salida | Descripción                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.windows.net`    | 443            | El entorno de ejecución de integración autohospedado lo necesita para conectarse a los servicios de movimiento de datos de Azure Data Factory. |
| `{datafactory}.{region}.datafactory.azure.net`<br> o bien `*.frontend.clouddatahub.net` | 443            | El entorno de ejecución de integración autohospedado lo necesita para conectarse al servicio Data Factory. <br>En el caso de los nuevos Data Factory creados, busque el nombre de dominio completo de la clave del entorno de ejecución de integración autohospedado que se encuentra en el formato {datafactory}.{region}.datafactory.azure.net. En el caso de Data Factory anterior, si no ve el FQDN en la clave del entorno de ejecución de integración autohospedado, use *.frontend.clouddatahub.net en su lugar. |
| `download.microsoft.com`    | 443            | Lo necesita el entorno de ejecución de integración autohospedado para descargar las actualizaciones. Si ha deshabilitado la actualización automática, puede omitir la configuración de este dominio. |
| `*.core.windows.net`          | 443            | Lo usa el entorno de ejecución de integración autohospedado para conectarse a la cuenta de Azure Storage cuando se utiliza la característica [Copia almacenada provisionalmente](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#staged-copy). |
| `*.database.windows.net`      | 1433           | Solo es obligatorio cuando se copia desde o en Azure SQL Database o Azure Synapse Analytics. En caso contrario, es opcional. Use la característica de copia almacenada provisionalmente para copiar datos en SQL Database o Synapse Analytics sin abrir el puerto 1433. |
| `*.azuredatalakestore.net`<br>`login.microsoftonline.com/<tenant>/oauth2/token`    | 443            | Solo es necesario cuando se copia desde o en Azure Data Lake Store, sino es opcional. |
