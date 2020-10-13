---
title: Concesión del acceso al almacenamiento mediante una excepción de servicio de confianza
titleSuffix: Azure Cognitive Search
description: Guía de procedimientos que describe cómo configurar una excepción de servicio de confianza para tener acceso a los datos de las cuentas de almacenamiento de forma segura.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: 30fc71e6f59766a759cdb8e4e503123623f48bd9
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91320479"
---
# <a name="accessing-data-in-storage-accounts-securely-via-trusted-service-exception"></a>Acceso a los datos de las cuentas de almacenamiento de forma segura mediante una excepción de servicio de confianza

Los indexadores que acceden a los datos de las cuentas de almacenamiento pueden utilizar la funcionalidad de [excepción de servicio de confianza](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions) para acceder a los datos de forma segura. Este mecanismo ofrece a los clientes que no pueden conceder [acceso al indexador mediante las reglas de firewall de IP](search-indexer-howto-access-ip-restricted.md) una alternativa sencilla, segura y gratuita para acceder a los datos de las cuentas de almacenamiento.

> [!NOTE]
> La compatibilidad con el acceso a los datos de las cuentas de almacenamiento a través de una excepción de servicio de confianza se limita a Azure Blob Storage y al almacenamiento de Azure Data Lake Gen2. El almacenamiento de tablas de Azure no se admite.

## <a name="step-1-configure-connection-to-the-storage-account-via-identity"></a>Paso 1: Configurar la conexión a la cuenta de almacenamiento mediante la identidad

Siga la información descrita en [la guía de acceso a identidad administrada](search-howto-managed-identities-storage.md) para configurar los indexadores para acceder a las cuentas de almacenamiento mediante la identidad administrada del servicio de búsqueda.

## <a name="step-2-allow-trusted-microsoft-services-to-access-the-storage-account"></a>Paso 2: Permitir que los servicios de Microsoft de confianza accedan a la cuenta de almacenamiento

En Azure Portal, vaya a la pestaña "Firewalls y redes virtuales" de la cuenta de almacenamiento. Asegúrese de que la opción "Permitir que los servicios de Microsoft de confianza accedan a esta cuenta de almacenamiento" esté habilitada. Esta opción solo permitirá que la instancia de servicio de búsqueda específica tenga el acceso basado en rol adecuado a la cuenta de almacenamiento (autenticación segura) para acceder a los datos de dicha cuenta, aunque estén protegidos por las reglas de firewall de IP.

![Excepción de servicio de confianza](media\search-indexer-howto-secure-access\exception.png "Excepción de servicio de confianza")

Los indexadores ahora podrán acceder a los datos de la cuenta de almacenamiento, aunque la cuenta esté protegida mediante reglas de firewall de IP.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre los indexadores de Azure Storage:

- [Indexador de blobs de Azure](search-howto-indexing-azure-blob-storage.md)
- [Indexador de Azure Data Lake Storage Gen2](search-howto-index-azure-data-lake-storage.md)
- [Indexador de tablas de Azure](search-howto-indexing-azure-tables.md)
