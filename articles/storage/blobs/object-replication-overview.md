---
title: Información general sobre la replicación de objetos
titleSuffix: Azure Storage
description: La replicación de objetos copia asincrónicamente los blobs en bloques entre una cuenta de almacenamiento de origen y una de destino. Utilice la replicación de objetos para minimizar la latencia de las solicitudes de lectura, aumentar la eficacia de las cargas de trabajo de proceso, optimizar la distribución de los datos y minimizar los costos.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 09/08/2020
ms.author: tamram
ms.subservice: blobs
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 4105698198e6fb7f4e3d3526ff9590ebca4898f1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91612173"
---
# <a name="object-replication-for-block-blobs"></a>Replicación de objetos para blobs en bloques

La replicación de objetos copia asincrónicamente los blobs en bloques entre una cuenta de almacenamiento de origen y una de destino. Algunos escenarios que admite la replicación de objetos son:

- **Minimización de latencia.** La replicación de objetos puede reducir la latencia de las solicitudes de lectura al permitir a los clientes consumir datos de una región que esté más cerca físicamente.
- **Aumentar la eficiencia de las cargas de trabajo de proceso.** Con la replicación de objetos, las cargas de trabajo de proceso pueden procesar los mismos conjuntos de blob en bloques en diferentes regiones.
- **Optimizar la distribución de datos.** Puede procesar o analizar los datos en una sola ubicación y luego replicar sólo los resultados en regiones adicionales.
- **Optimizar los costos.** Después de replicar los datos, puede reducir los costos moviéndolos al nivel de archivo mediante las directivas de administración del ciclo de vida.

El siguiente diagrama muestra cómo la replicación de objetos replica blob en bloques de una cuenta de almacenamiento de origen en una región a cuentas de destino en dos regiones diferentes.

:::image type="content" source="media/object-replication-overview/object-replication-diagram.svg" alt-text="Diagrama que muestra cómo funciona la replicación de objetos":::

Para información sobre cómo configurar la replicación de objetos, consulte [Configuración de la replicación de objetos](object-replication-configure.md).

[!INCLUDE [storage-data-lake-gen2-support](../../../includes/storage-data-lake-gen2-support.md)]

## <a name="prerequisites-for-object-replication"></a>Requisitos previos de la replicación de objetos

La replicación de objetos requiere que también estén habilitadas las características de Azure Storage siguientes:

- [Fuente de cambios](storage-blob-change-feed.md): debe estar habilitada en la cuenta de origen. Para información sobre cómo habilitar la fuente de cambios, consulte [Habilitar y deshabilitar la fuente de cambios](storage-blob-change-feed.md#enable-and-disable-the-change-feed).
- [Control de versiones de blobs](versioning-overview.md): debe estar habilitado en la cuenta de origen y en la cuenta de destino. Para información sobre cómo habilitar el control de versiones, consulte [Habilitación y administración del control de versiones de blobs](versioning-enable.md).

Habilitar la fuente de cambios y el control de versiones de blob puede suponer costos adicionales. Para más información, consulte la página [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

## <a name="object-replication-policies-and-rules"></a>Reglas y directivas de replicación de objetos

Al configura la replicación de objetos, se crea una política de replicación que especifica la cuenta de almacenamiento de origen y la cuenta de destino. Una directiva de replicación incluye una o más reglas que especifican un contenedor de origen y un contenedor de destino e indican qué blob en bloques del contenedor de origen se replicarán.

Después de configurar la replicación de objetos, Azure Storage comprueba periódicamente la fuente de cambios de la cuenta de origen y replica asincrónicamente cualquier operación de escritura o borrada a la cuenta de destino. La latencia de replicación depende del tamaño del blob en bloques que se está replicando.

> [!IMPORTANT]
> Debido a que los datos de blob en bloques se replican asincrónicamente, la cuenta de origen y la de destino no están inmediatamente sincronizadas. Actualmente no hay ningún acuerdo de nivel de servicio sobre cuánto tiempo se tarda en replicar los datos en la cuenta de destino.

### <a name="replication-policies"></a>Directivas de replicación

Al configurar la replicación de objetos, se crea una directiva de replicación tanto en la cuenta de origen como en la de destino a través del proveedor de recursos de Azure Storage. La directiva de replicación se identifica mediante un id. de directiva. La directiva en las cuentas de origen y de destino debe tener el mismo id. de directiva para que tenga lugar la replicación.

Una cuenta de almacenamiento puede servir como cuenta de origen para un máximo dos cuentas de destino. Las cuentas de origen y de destino pueden estar en la misma región o en regiones diferentes. También pueden residir en diferentes suscripciones y en distintos inquilinos de Azure Active Directory (Azure AD). Solo se puede crear una directiva de replicación para cada par de cuentas de origen/cuentas de destino.

### <a name="replication-rules"></a>Reglas de replicación

Las reglas de replicación especifican el modo en que Azure Storage replicará los blobs de un contenedor de origen a un contenedor de destino. Puede especificar un máximo de 10 reglas de replicación para cada directiva de replicación. Cada regla de replicación define un único contenedor de origen y de destino, y cada contenedor de origen y de destino puede utilizarse en una sola regla.

Al crear una regla de replicación, de forma predeterminada solo se copian los blobs en bloques nuevos que se agreguen posteriormente al contenedor de origen. Puede especificar que se copien tanto los blob en bloques nuevos como los ya existentes, o bien puede definir un ámbito de copia personalizado que copie los blobs en bloques creados a partir de un momento determinado.

También puede especificar uno o varios filtros como parte de una regla de replicación para filtrar los blobs en bloques por prefijo. Cuando se especifica un prefijo, solo los blobs que coinciden con ese prefijo en el contenedor de origen se copiarán en el contenedor de destino.

Los contenedores de origen y de destino deben existir antes de poder especificarlos en una regla. Después de crear la directiva de replicación, el contenedor de destino pasa a ser de solo lectura. Cualquier intento de escribir en el contenedor de destino producirá un error con el código de error 409 (Conflicto). Sin embargo, puede llamar a la operación [Establecer el nivel del blob](/rest/api/storageservices/set-blob-tier) en un blob en el contenedor de destino para moverla al nivel de archivo. Para más información sobre el nivel de archivo, consulte [Azure Blob Storage: niveles de acceso frecuente, esporádico y de archivo](storage-blob-storage-tiers.md#archive-access-tier).

## <a name="billing"></a>Facturación

La replicación de objetos incurre en costos adicionales por las transacciones de lectura y escritura en las cuentas de origen y de destino, así como los cargos de salida por la replicación de datos de la cuenta de origen a la cuenta de destino y cargos de lectura para procesar la fuente de cambios.

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de la replicación de objetos](object-replication-configure.md)
- [Compatibilidad con la fuente de cambios en Azure Blob Storage](storage-blob-change-feed.md)
- [Habilitar y administrar las versiones de blob](versioning-enable.md)
