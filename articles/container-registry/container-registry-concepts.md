---
title: Acerca de los repositorios y las imágenes
description: Una introducción a los conceptos clave de los registros de contenedor, repositorios e imágenes de contenedor de Azure.
ms.topic: article
ms.date: 09/10/2019
ms.openlocfilehash: ea6e2577d3eee91626dd613617a0b79e4ff3d6a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79225804"
---
# <a name="about-registries-repositories-and-images"></a>Acerca de los registros, repositorios e imágenes

En este artículo se presentan los conceptos básicos de los registros de contenedor, los repositorios y las imágenes de contenedor, así como los artefactos relacionados. 

## <a name="registry"></a>Registro

Un *registro* de contenedor es un servicio que almacena y distribuye imágenes de contenedor. Docker Hub es un registro de contenedor público que admite la comunidad de código abierto y sirve como catálogo general de imágenes. Azure Container Registry proporciona a los usuarios control directo de sus imágenes, con autenticación integrada, [replicación geográfica](container-registry-geo-replication.md) que admite la distribución global y confiabilidad para implementaciones cercanas a la red, la [configuración virtual de red y firewall](container-registry-vnet.md), el [bloqueo con etiquetas](container-registry-image-lock.md) y muchas otras características mejoradas. 

Además de las imágenes de contenedor de Docker, Azure Container Registry admite [artefactos de contenido](container-registry-image-formats.md) relacionados, incluidos los formatos de imagen de Open Container Initiative (OCI).

## <a name="content-addressable-elements-of-an-artifact"></a>Elementos direccionables del contenido de un artefacto

La dirección de un artefacto en un registro de contenedor de Azure incluye los siguientes elementos. 

`[loginUrl]/[namespace]/[artifact:][tag]`

* **loginUrl**: el nombre completo del host del registro. El host del registro de un registro de contenedor de Azure tiene el formato *myregistry*.azurecr.io (todo en minúsculas). Debe especificar el valor de loginUrl cuando utilice Docker u otras herramientas de cliente para extraer o insertar artefactos a un registro de contenedor de Azure. 
* **namespace**: agrupación lógica delimitada por barras diagonales de imágenes relacionadas o artefactos; por ejemplo, para una aplicación o un grupo de trabajo
* **artifact**: el nombre de un repositorio para una imagen o un artefacto particular
* **tag**: una versión específica de una imagen o un artefacto que se almacenan en un repositorio


Por ejemplo, el nombre completo de una imagen de un registro de contenedor de Azure puede tener este aspecto:

*myregistry.azurecr.io/marketing/campaign10-18/email-sender:v2*

Consulte las siguientes secciones para más información sobre estos elementos.

## <a name="repository-name"></a>Nombre del repositorio

Los registros de contenedor administran *repositorios*, colecciones de imágenes de contenedor u otros artefactos con el mismo nombre, pero con etiquetas diferentes. Por ejemplo, las tres imágenes siguientes están en el repositorio "acr-helloworld":


- *acr-helloworld:latest*
- *acr-helloworld:v1*
- *acr-helloworld:v2*

Los nombres de repositorio también pueden incluir [espacios de nombres](container-registry-best-practices.md#repository-namespaces). Los espacios de nombres le permiten agrupar imágenes con nombres de repositorio delimitados por barras diagonales hacia delante, por ejemplo:

- *marketing/campaign10-18/web:v2*
- *marketing/campaign10-18/api:v3*
- *marketing/campaign10-18/email-sender:v2*
- *product-returns/web-submission:20180604*
- *product-returns/legacy-integrator:20180715*

## <a name="image"></a>Imagen

Una imagen de contenedor u otro artefacto dentro de un registro está asociado con una o varias etiquetas, tiene una o más capas y se identifica mediante un manifiesto. Entender cómo se relacionan entre sí estos componentes puede ayudarle a administrar el registro de forma eficaz.

### <a name="tag"></a>Etiqueta

La *etiqueta* de una imagen u otro artefacto especifica su versión. A un solo artefacto dentro de un repositorio se le pueden asignar una o varias etiquetas y también puede ser "sin etiqueta". Es decir, puede eliminar todas las etiquetas de una imagen, mientras que los datos de la imagen (sus capas) permanecen en el registro.

El repositorio (o repositorio y espacio de nombres) junto con una etiqueta definen el nombre de una imagen. Puede insertar y extraer una imagen especificando su nombre en la operación de inserción o extracción.

La forma de etiquetar las imágenes de contenedor está guiada por los escenarios de desarrollo o implementación. Por ejemplo, se recomiendan etiquetas estables para mantener las imágenes base y etiquetas únicas para implementar las imágenes. Para más información, consulte [Recomendaciones para el etiquetado y control de versiones de las imágenes de contenedor](container-registry-image-tag-version.md).

### <a name="layer"></a>Nivel

Las imágenes de contenedor se componen de una o más *capas*, cada una correspondiente a una línea en el archivo de Docker que define la imagen. Las imágenes de un registro comparten capas comunes para aumentar la eficacia de almacenamiento. Por ejemplo, varias imágenes en distintos repositorios pueden compartir la misma capa base de Alpine Linux, pero solo se almacena en el registro una copia de esa capa.

El uso compartido de las capas optimiza la distribución de la capa en nodos con varias imágenes que comparten capas comunes. Por ejemplo, si una imagen que ya se encuentra en un nodo incluye la capa de Alpine Linux como su base, la extracción posterior de una imagen distinta que haga referencia a la misma capa no transfiere la capa al nodo. En su lugar, hace referencia a la capa que ya existe en el nodo.

Para proporcionar aislamiento seguro y protección frente a posibles manipulaciones de capa, las capas no se comparten entre los registros.

### <a name="manifest"></a>Manifest

Cada imagen de contenedor o artefacto que se inserta en un registro de contenedor está asociado con un *manifiesto*. El manifiesto, generado por el registro cuando se inserta la imagen, identifica de forma única la imagen y especifica sus capas. Puede enumerar los manifiestos de un repositorio con el comando de la CLI de Azure [az acr repository show-manifests][az-acr-repository-show-manifests]:

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName>
```

Por ejemplo, enumere los manifiestos del repositorio "acr-helloworld":

```azurecli
az acr repository show-manifests --name myregistry --repository acr-helloworld
```

```output
[
  {
    "digest": "sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108",
    "tags": [
      "latest",
      "v3"
    ],
    "timestamp": "2018-07-12T15:52:00.2075864Z"
  },
  {
    "digest": "sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57",
    "tags": [
      "v2"
    ],
    "timestamp": "2018-07-12T15:50:53.5372468Z"
  },
  {
    "digest": "sha256:7ca0e0ae50c95155dbb0e380f37d7471e98d2232ed9e31eece9f9fb9078f2728",
    "tags": [
      "v1"
    ],
    "timestamp": "2018-07-11T21:38:35.9170967Z"
  }
]
```

### <a name="manifest-digest"></a>Hash de manifiesto

Los manifiestos se identifican mediante un hash SHA-256 único o *hash de manifiesto*. Cada imagen o artefacto, etiquetados o no, se identifican por el hash. El valor del hash es único, incluso si los datos de la capa de la imagen son idénticos a los de otra imagen. Este mecanismo es lo que le permite insertar varias veces imágenes etiquetadas de forma idéntica a un registro. Por ejemplo, puede insertar varias veces `myimage:latest` en el registro sin errores porque cada imagen se identifica mediante su hash único.

Puede extraer una imagen de un registro especificando su hash en la operación de extracción. Algunos sistemas se pueden configurar para extraer por hash porque se garantiza la versión de la imagen que se va a extraer, incluso si una imagen etiquetada de forma idéntica se inserta posteriormente en el registro.

Por ejemplo, extraiga una imagen desde el repositorio "acr-helloworld" mediante el hash del manifiesto:

`docker pull myregistry.azurecr.io/acr-helloworld@sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108`

> [!IMPORTANT]
> Si inserta repetidamente imágenes modificadas con etiquetas idénticas, puede crear imágenes huérfanas: imágenes que están sin etiquetas, pero consumen espacio en el registro. Las imágenes sin etiquetas no se muestran en la CLI de Azure ni en Azure Portal al enumerar o visualizar las imágenes por etiqueta. Sin embargo, sus capas existen y consumen espacio en el registro. La eliminación de una imagen sin etiqueta libera espacio del registro cuando el manifiesto es el único, o el último, que apunta a una capa determinada. Para más información acerca de cómo liberar el espacio usado mediante imágenes sin etiquetar, consulte [Eliminación de imágenes de contenedor en Azure Container Registry](container-registry-delete.md).

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre el [almacenamiento de imágenes](container-registry-storage.md) y los [formatos de contenido admitidos](container-registry-image-formats.md) en Azure Container Registry.

<!-- LINKS - Internal -->
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az-acr-repository-show-manifests


