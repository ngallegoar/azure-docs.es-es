---
title: Niveles de servicio y SKU
description: Obtenga información sobre las características y los límites de los niveles de servicio Básico, Estándar y Premium (SKU) de Azure Container Registry.
ms.topic: article
ms.date: 11/05/2019
ms.openlocfilehash: 1ebe5339b7523a4463dee45b126244d7ec5b2e4b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74456272"
---
# <a name="azure-container-registry-skus"></a>SKU de Azure Container Registry

Azure Container Registry (ACR) está disponible en varios niveles de servicio, conocidos como SKU. Estas SKU proporcionan precios predecibles y varias opciones para alinearse con la capacidad y patrones de uso de su registro de Docker privado en Azure.

| SKU | Descripción |
| --- | ----------- |
| **Basic** | Un punto de entrada optimizado para los costos para que los desarrolladores aprendan sobre Azure Container Registry. Los registros básicos tienen las mismas funcionalidades de programación que Estándar y Premium (como [integración de la autenticación](container-registry-authentication.md#individual-login-with-azure-ad) de Azure Active Directory, la [eliminación de imágenes][container-registry-delete] y [webhooks][container-registry-webhook]). Sin embargo, el almacenamiento incluido y el rendimiento de las imágenes son más adecuadas para escenarios de uso inferior. |
| **Estándar** | Los registros estándar ofrecen las mismas funcionalidades que los básicos, pero con más almacenamiento y un mayor rendimiento de las imágenes. Los registros estándar deberían satisfacer las necesidades de la mayoría de los escenarios de producción. |
| **Premium** | Los registros premium proporcionan la mayor cantidad de almacenamiento incluido y operaciones simultáneas, por lo que permiten trabajar con escenarios de mayor volumen. Además de la mayor capacidad de rendimiento de las imágenes, el nivel Premium agrega características, incluida la [replicación geográfica][container-registry-geo-replication] para la administración de un único registro en varias regiones, la [confianza del contenido](container-registry-content-trust.md) para la firma de etiquetas de imagen, y [firewalls y redes virtuales (versión preliminar)](container-registry-vnet.md) para restringir el acceso al registro. |

Las SKU Básico, Estándar y Premium proporcionan las mismas funcionalidades de programación. También aprovechan el [almacenamiento de imágenes][container-registry-storage] administrado totalmente por Azure. Elegir una SKU de nivel superior proporciona más rendimiento y escalabilidad. Con varios niveles de servicio, puede empezar a trabajar con el nivel Básico y, después, cambiar a Estándar y a Premium a medida que aumente su uso del registro.

## <a name="sku-features-and-limits"></a>Características y límites de las SKU

En la tabla siguiente se detallan las características y los límites de los niveles de servicio Básico, Estándar y Premium.

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="changing-skus"></a>Cambio de SKU

Puede cambiar la SKU de un registro con la CLI de Azure o en Azure Portal. Puede moverse libremente entre SKU siempre que la SKU a la que se vaya a cambiar tenga la capacidad de almacenamiento máximo necesaria. 

### <a name="azure-cli"></a>Azure CLI

Para moverse entre las SKU en la CLI de Azure, use el comando [az acr update][az-acr-update]. Por ejemplo, para cambiar a Premium:

```azurecli
az acr update --name myregistry --sku Premium
```

### <a name="azure-portal"></a>Portal de Azure

En la **información general** del registro de contenedor en Azure Portal, seleccione **Actualizar** y después seleccione una **SKU** nueva en la lista desplegable de SKU.

![Actualización de la SKU del registro de contenedor en Azure Portal][update-registry-sku]

## <a name="pricing"></a>Precios

Para obtener más información sobre cada una de las SKU de Azure Container Registry, consulte [Precios de Container Registry][container-registry-pricing].

Para obtener más información acerca de los precios de las transferencias de datos, consulte [Detalles de precios de Bandwidth](https://azure.microsoft.com/pricing/details/bandwidth/). 

## <a name="next-steps"></a>Pasos siguientes

**Mapa de ruta de Azure Container Registry**

Visite el [mapa de ruta de ACR][acr-roadmap] en GitHub para obtener información sobre las próximas características del servicio.

**UserVoice en Azure Container Registry**

Envíe y vote las sugerencias de nuevas características de [UserVoice en ACR][container-registry-uservoice].

<!-- IMAGES -->
[update-registry-sku]: ./media/container-registry-skus/update-registry-sku.png

<!-- LINKS - External -->
[acr-roadmap]: https://aka.ms/acr/roadmap
[container-registry-pricing]: https://azure.microsoft.com/pricing/details/container-registry/
[container-registry-uservoice]: https://feedback.azure.com/forums/903958-azure-container-registry

<!-- LINKS - Internal -->
[az-acr-update]: /cli/azure/acr#az-acr-update
[container-registry-geo-replication]: container-registry-geo-replication.md
[container-registry-storage]: container-registry-storage.md
[container-registry-delete]: container-registry-delete.md
[container-registry-webhook]: container-registry-webhook.md
