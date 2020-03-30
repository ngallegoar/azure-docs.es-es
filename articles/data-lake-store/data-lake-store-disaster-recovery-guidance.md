---
title: Guía de recuperación ante desastres para Azure Data Lake Storage Gen1 | Microsoft Docs
description: Guía de recuperación ante desastres para Azure Data Lake Storage Gen1
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: twooley
ms.openlocfilehash: b33977ca5184ea07b5651be18e3a132d30ce4b39
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75966067"
---
# <a name="disaster-recovery-guidance-for-data-in-azure-data-lake-storage-gen1"></a>Guía de recuperación ante desastres para datos de Azure Data Lake Storage Gen1

Azure Data Lake Storage Gen1 proporciona almacenamiento con redundancia local (LRS). Por ello, los datos de su cuenta de Data Lake Storage Gen1 resisten los errores transitorios de hardware dentro de un centro de datos a través de las réplicas automatizadas. Esto garantiza la durabilidad y alta disponibilidad, en cumplimiento con el Acuerdo de Nivel de Servicio de Data Lake Storage Gen1. En este artículo, se proporciona una guía sobre cómo proteger aún más los datos en caso de las poco frecuentes interrupciones de toda la región o de eliminaciones accidentales.

## <a name="disaster-recovery-guidance"></a>Guía de recuperación ante desastres
Es fundamental que todos los clientes preparen su propio plan de recuperación ante desastres. Lea la información de este artículo para generar el plan de recuperación ante desastres. Aquí tiene algunos recursos que pueden ayudarle a crear su propio plan.

* [Recuperación ante desastres y alta disponibilidad para aplicaciones de Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Guía técnica sobre resistencia en Azure](../resiliency/resiliency-technical-guidance.md)

### <a name="best-practices"></a>Procedimientos recomendados
Se recomienda que copie los datos críticos en otra cuenta de Data Lake Storage Gen1 en otra región con una frecuencia que esté en consonancia con las necesidades de su plan de recuperación ante desastres. Hay una variedad de métodos para copiar datos entre los que se incluyen [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) o [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md). Azure Data Factory es un servicio útil para crear e implementar las canalizaciones de movimiento de datos de forma periódica.

Si se produce una interrupción regional, puede acceder a sus datos en la región donde estos se copiaron. Puede supervisar el [panel de Azure Service Health](https://azure.microsoft.com/status/) para determinar el estado del servicio de Azure en todo el mundo.

## <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Guía para la recuperación ante un problema de datos dañados o eliminación accidental
Tenga en cuenta que aunque Data Lake Storage Gen1 ofrece resistencia de datos mediante réplicas automatizadas, esto no evita que su aplicación (o los desarrolladores o usuarios) puedan dañar o eliminar de forma no intencionada los datos.

### <a name="best-practices"></a>Procedimientos recomendados
Para evitar la eliminación por error, se recomienda establecer primero las directivas de acceso correctas para su cuenta de Data Lake Storage Gen1.  Esto incluye la aplicación de [bloqueos de recursos de Azure](../azure-resource-manager/management/lock-resources.md) para recursos importantes, así como la aplicación de control de acceso de nivel de cuenta y archivo mediante las [características de seguridad de Data Lake Storage Gen1](data-lake-store-security-overview.md) disponibles. También se recomienda que establezca una rutina de creación de copias de los datos críticos mediante [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) o [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) en otra cuenta de Data Lake Storage Gen1, otra carpeta o suscripción de Azure.  Esto se puede usar para recuperar después de un incidente de daños o eliminación de datos. Azure Data Factory es un servicio útil para crear e implementar las canalizaciones de movimiento de datos de forma periódica.

Las organizaciones también pueden habilitar un [registro de diagnóstico](data-lake-store-diagnostic-logs.md) para su cuenta de Data Lake Storage Gen1 y recopilar seguimientos de auditoría de acceso a datos que proporcionen información sobre quién puede eliminar o actualizar un archivo.

## <a name="next-steps"></a>Pasos siguientes
* [Introducción Azure Data Lake Storage Gen1](data-lake-store-get-started-portal.md)
* [Protección de datos en Data Lake Storage Gen1](data-lake-store-secure-data.md)

