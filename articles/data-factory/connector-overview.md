---
title: Información general sobre el conector de Azure Data Factory
description: Obtenga información sobre los conectores compatibles en Data Factory.
services: data-factory
author: linda33wj
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 07/08/2020
ms.author: jingwang
ms.reviewer: craigg
ms.openlocfilehash: f22b4bec17abe95be2a17d3e0e3c41fab3483fe0
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2020
ms.locfileid: "86224829"
---
# <a name="azure-data-factory-connector-overview"></a>Información general sobre el conector de Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Data Factory admite los siguientes almacenes de datos y formatos a través de las actividades Copiar, Flujo de datos, Buscar, Obtener metadatos y Eliminar. Haga clic en cada almacén de datos para obtener información detallada sobre las funcionalidades admitidas y las configuraciones correspondientes.

## <a name="supported-data-stores"></a>Almacenes de datos compatibles

[!INCLUDE [Connector overview](../../includes/data-factory-v2-connector-overview.md)]

## <a name="supported-file-formats"></a>Formatos de archivos admitidos

Azure Data Factory admite los siguientes formatos de archivo. Consulte los artículos para conocer la configuración basada en el formato.

- [Formato Avro](format-avro.md)
- [Formato binario](format-binary.md)
- [Formato Common Data Model](format-common-data-model.md)
- [Formato de texto delimitado](format-delimited-text.md)
- [Formato Delta](format-delta.md)
- [Formato Excel](format-excel.md)
- [Formato JSON](format-json.md)
- [Formato ORC](format-orc.md)
- [Formato Parquet](format-parquet.md)

## <a name="next-steps"></a>Pasos siguientes

- [Actividad de copia](copy-activity-overview.md)
- [Asignación de Data Flow](concepts-data-flow-overview.md)
- [Actividad Lookup](control-flow-lookup-activity.md)
- [Actividad GetMetadata](control-flow-get-metadata-activity.md)
- [Eliminar actividad](delete-activity.md)
