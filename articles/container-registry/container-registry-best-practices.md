---
title: Procedimientos recomendados del registro
description: Obtenga información sobre cómo de forma eficaz Azure Container Registry mediante los siguientes procedimientos recomendados.
ms.topic: article
ms.date: 09/27/2018
ms.openlocfilehash: fc84fb8cb98f58e28570095370d55a7358ce3a99
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83682687"
---
# <a name="best-practices-for-azure-container-registry"></a>Procedimientos recomendados para Azure Container Registry

Si sigue estos procedimientos recomendados, puede ayudar a maximizar el rendimiento y rentabilizar el uso del registro de Docker privado en Azure.

Vea también [Recomendaciones para el etiquetado y el control de versiones de las imágenes de contenedor](container-registry-image-tag-version.md) para las estrategias de etiquetado y versión de las imágenes en el registro. 

## <a name="network-close-deployment"></a>Implementación cercana a la red

Cree el registro de contenedor en la misma región de Azure en la que implementa los contenedores. Colocar el registro en una región que esté cercana a la red para los hosts del contenedor puede ayudar a reducir tanto la latencia como el costo.

La implementación cercana a la red es una de las razones principales para utilizar un registro de contenedor privado. Las imágenes de Docker tienen una [construcción en capas](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/) eficaz que permite realizar implementaciones incrementales. Sin embargo, los nuevos nodos tienen que extraer todas las capas necesarias para una imagen determinada. Este `docker pull` inicial puede agregar rápidamente hasta varios gigabytes. Tener un registro privado próximo a la implementación minimiza la latencia de red.
Además, todas las nubes públicas, Azure incluido, implementan cuotas de salida de red. La extracción de imágenes desde un centro de datos a otro agrega cuotas de salida de red, además de latencia.

## <a name="geo-replicate-multi-region-deployments"></a>Implementación de la replicación geográfica en varias regiones

Utilice la característica de [replicación geográfica](container-registry-geo-replication.md) de Azure Container Registry si está implementando contenedores en varias regiones. Si se está prestando servicio a clientes globales de centros de datos locales o su equipo de desarrollo se encuentra en distintas ubicaciones, puede simplificar la administración del registro y minimizar la latencia mediante la replicación geográfica del registro. La replicación geográfica solo está disponible con los registros [Premium](container-registry-skus.md).

Para aprender a usar la replicación geográfica, vea el tutorial de tres partes [Geo-replication in Azure Container Registry](container-registry-tutorial-prepare-registry.md) (Replicación geográfica en Azure Container Registry).

## <a name="repository-namespaces"></a>Espacios de nombres del repositorio

Al aprovechar los espacios de nombres del repositorio, puede permitir el uso compartido de un registro único por varios grupos dentro de su organización. Los registros se pueden compartir por las implementaciones y los equipos. Azure Container Registry admite espacios de nombres anidados, lo que permite el aislamiento del grupo.

Por ejemplo, considere las siguientes etiquetas de imagen de contenedor. Las imágenes que se usan en toda la organización, como `aspnetcore`, se colocan en el espacio de nombres raíz, mientras que las imágenes de contenedor que pertenecen a los grupos de producción y marketing usan sus propios espacios de nombres.

- *contoso.azurecr.io/aspnetcore:2.0*
- *contoso.azurecr.io/products/widget/web:1*
- *contoso.azurecr.io/products/bettermousetrap/refundapi:12.3*
- *contoso.azurecr.io/marketing/2017-fall/concertpromotions/campaign:218.42*

## <a name="dedicated-resource-group"></a>Grupo de recursos dedicado

Dado que los registros de contenedor son recursos que se utilizan en varios hosts de contenedor, un registro debe residir en su propio grupo de recursos.

Aunque puede experimentar con un tipo de host específico, como Azure Container Instances, es probable que desee eliminar la instancia del contenedor cuando haya terminado. Sin embargo, también puede mantener la colección de imágenes que ha insertado en Azure Container Registry. Al colocar el registro en su propio grupo de recursos, se minimiza el riesgo de eliminar de manera accidental la colección de imágenes del registro cuando se elimina el grupo de recursos de la instancia de contenedor.

## <a name="authentication"></a>Authentication

Al autenticar con un registro de contenedor de Azure, hay dos escenarios principales: la autenticación individual y la autenticación de servicio (o "sin periféricos"). En la tabla siguiente se proporciona una breve descripción de estos escenarios y el método recomendado de autenticación para cada uno.

| Tipo | Escenario de ejemplo | Método recomendado |
|---|---|---|
| Identidad individual | Un desarrollador que extrae imágenes de su máquina de desarrollo, o las inserta en ella. | [az acr login](/cli/azure/acr?view=azure-cli-latest#az-acr-login) |
| Identidad de servicio o sin periféricos | Canalizaciones de compilación e implementación donde el usuario no está directamente implicado. | [Entidad de servicio](container-registry-authentication.md#service-principal) |

Para obtener información detallada acerca de la autenticación de Azure Container Registry, consulte [Autenticación con un registro de contenedor privado de Docker](container-registry-authentication.md).

## <a name="manage-registry-size"></a>Administración del tamaño del registro

Las restricciones de almacenamiento de cada [nivel de servicio del registro de contenedor][container-registry-skus] están diseñadas para adaptarse a un escenario típico: **Básico** para los primeros pasos, **Estándar** para la mayoría de las aplicaciones de producción, y **Premium** para un rendimiento a gran escala y con [replicación geográfica][container-registry-geo-replication]. Durante todo el ciclo de vida del registro, debe administrar su tamaño mediante la eliminación periódica del contenido sin usar.

Use el comando [az acr show-usage][az-acr-show-usage] de la CLI de Azure para mostrar el tamaño actual del registro:

```azurecli
az acr show-usage --resource-group myResourceGroup --name myregistry --output table
```

```output
NAME      LIMIT         CURRENT VALUE    UNIT
--------  ------------  ---------------  ------
Size      536870912000  185444288        Bytes
Webhooks  100                            Count
```

También puede encontrar el almacenamiento actual usado en **Información general** del registro en Azure Portal:

![Información del uso del registro en Azure Portal][registry-overview-quotas]

### <a name="delete-image-data"></a>Eliminación de los datos de la imagen

Azure Container Registry admite varios métodos para eliminar datos de imagen del registro de contenedor. Puede eliminar imágenes por etiqueta o síntesis de manifiesto o eliminar un repositorio entero.

Para más información sobre cómo eliminar datos de imagen del registro, como imágenes no etiquetadas (a veces llamadas "pendientes" o "huérfanas"), consulte [Eliminación de imágenes de contenedor en Azure Container Registry](container-registry-delete.md).

## <a name="next-steps"></a>Pasos siguientes

Azure Container Registry está disponible en varios niveles (también denominados SKU), y cada uno de ellos proporciona distintas funcionalidades. Para obtener más información sobre los niveles de servicio disponibles, consulte [SKU de Azure Container Registry](container-registry-skus.md).

<!-- IMAGES -->
[delete-repository-portal]: ./media/container-registry-best-practices/delete-repository-portal.png
[registry-overview-quotas]: ./media/container-registry-best-practices/registry-overview-quotas.png

<!-- LINKS - Internal -->
[az-acr-repository-delete]: /cli/azure/acr/repository#az-acr-repository-delete
[az-acr-show-usage]: /cli/azure/acr#az-acr-show-usage
[azure-cli]: /cli/azure
[azure-portal]: https://portal.azure.com
[container-registry-geo-replication]: container-registry-geo-replication.md
[container-registry-skus]: container-registry-skus.md
