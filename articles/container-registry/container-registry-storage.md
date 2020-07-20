---
title: Almacenamiento de imágenes de contenedor
description: Información detallada acerca de cómo se almacenan las imágenes del contenedor de Docker en Azure Container Registry, incluida la seguridad, la redundancia y la capacidad.
ms.topic: article
ms.date: 06/18/2020
ms.openlocfilehash: d51014e9e0769091aba42682cce3a6a01cfa19de
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85214067"
---
# <a name="container-image-storage-in-azure-container-registry"></a>Almacenamiento de imágenes en Azure Container Registry

Cada instancia de Azure Container Registry, ya sea [Básico, Estándar o Premium](container-registry-skus.md), goza de características avanzadas de almacenamiento de Azure, como el cifrado en reposo y redundancia geográfica para proteger la seguridad de los datos de la imagen. En las secciones siguientes se describen las características y los límites del almacenamiento de imágenes en Azure Container Registry (ACR).

## <a name="encryption-at-rest"></a>Cifrado en reposo

Todas las imágenes del contenedor en el registro se cifran en reposo. Azure cifra automáticamente una imagen antes de almacenarla y la descifra sobre la marcha cuando usted o sus aplicaciones y servicios extraen la imagen. O bien, aplique una capa de cifrado adicional con una [clave administrada por el cliente](container-registry-customer-managed-keys.md).

## <a name="geo-redundant-storage"></a>Almacenamiento con redundancia geográfica

Azure usa un esquema de almacenamiento con redundancia geográfica para proteger contra la pérdida de las imágenes del contenedor. Azure Container Registry replica automáticamente las imágenes del contenedor en varios centros de datos geográficamente distantes, para evitar que se pierdan si se produce un error de almacenamiento regional.

## <a name="geo-replication"></a>Replicación geográfica

Para escenarios que requieren una garantía de alta disponibilidad aún mayor, considere el uso de la característica [Replicación geográfica](container-registry-geo-replication.md) de los registros Premium. La replicación geográfica ayuda a protegerse contra la pérdida del acceso a su registro en caso de un error regional *total*, no solo de un error de almacenamiento. La replicación geográfica ofrece otras ventajas, como almacenamiento de imágenes cercano a la red para una inserción y extracción más rápida en escenarios de desarrollo o implementación distribuidos.

## <a name="scalable-storage"></a>Almacenamiento escalable

Azure Container Registry le permite crear tantos repositorios, imágenes, capas o etiquetas como necesite, hasta el [límite de almacenamiento del registro](container-registry-skus.md#service-tier-features-and-limits). 

Unas cifras muy altas de etiquetas y repositorios pueden afectar al rendimiento del registro. Elimine periódicamente repositorios, etiquetas e imágenes no utilizados como parte de la rutina de mantenimiento del registro y, si lo desea, establezca una [directiva de retención](container-registry-retention-policy.md) para los manifiestos no etiquetados. Los recursos de registro eliminados, como los repositorios, imágenes o etiquetas *no pueden* recuperarse después de la eliminación. Para obtener más información acerca de cómo eliminar recursos del registro, consulte [Eliminación de imágenes de contenedor en Azure Container Registry](container-registry-delete.md).

## <a name="storage-cost"></a>Coste del almacenamiento

Para información completa acerca de los precios, consulte los [precios de Azure Container Registry][pricing].

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre los registros de contenedor Básico, Estándar y Premium, consulte [SKU de Azure Container Registry](container-registry-skus.md).

<!-- IMAGES -->

<!-- LINKS - External -->
[portal]: https://portal.azure.com
[pricing]: https://aka.ms/acr/pricing

<!-- LINKS - Internal -->
