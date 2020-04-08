---
title: Importación de imágenes de contenedor
description: Importe imágenes de contenedor en un registro de contenedor de Azure mediante las API de Azure, sin necesidad de ejecutar comandos de Docker.
ms.topic: article
ms.date: 03/16/2020
ms.openlocfilehash: caf7a47ac8f7ff0e72d2e049a7013542d274a225
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80051922"
---
# <a name="import-container-images-to-a-container-registry"></a>Importación de imágenes de contenedor en un registro de contenedor

Puede importar (copiar) fácilmente imágenes de contenedor (copia) en un registro de contenedor de Azure, sin usar comandos de Docker. Por ejemplo, importar imágenes de un registro de desarrollo en un registro de producción, o copiar las imágenes base desde un registro público.

Azure Container Registry controla una serie de escenarios comunes para copiar imágenes de un registro existente:

* Importación desde un registro público

* Importación desde otro registro de contenedor de Azure, en el mismo o en una suscripción de Azure diferente

* Importación desde un registro de contenedor privado de Azure

La importación de imágenes en un registro de contenedor de Azure tiene las siguientes ventajas con respecto al uso de comandos de la CLI de Docker:

* Dado que el entorno de cliente no necesita una instalación local de Docker, importe cualquier imagen de contenedor, independientemente del tipo de sistema operativo compatible.

* Al importar imágenes de varias arquitecturas (por ejemplo, las imágenes oficiales de Docker), se copian las imágenes de todas las arquitecturas y plataformas especificadas en la lista de manifiesto.

Para importar imágenes de contenedor, este artículo requiere que ejecute la CLI de Azure en Azure Cloud Shell o localmente (se recomienda la versión 2.0.55 o posterior). Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][azure-cli].

> [!NOTE]
> Si tiene que distribuir imágenes de contenedor idénticas en varias regiones de Azure, Azure Container Registry también admite la [replicación geográfica](container-registry-geo-replication.md). Mediante la replicación geográfica de un registro (es necesario el nivel de servicio Premium), puede atender varias regiones con nombres idénticos de imagen y etiqueta desde un único registro.
>

## <a name="prerequisites"></a>Prerrequisitos

Si aún no tiene un registro de contenedor de Azure, cree un registro. Para obtener instrucciones, consulte [Guía de inicio rápido: Creación de un registro de contenedor privado con la CLI de Azure](container-registry-get-started-azure-cli.md).

Para importar una imagen en un registro de contenedor de Azure, la identidad debe tener permisos de escritura en el registro de destino (al menos el rol de colaborador). Consulte [Roles y permisos de Azure Container Registry](container-registry-roles.md). 

## <a name="import-from-a-public-registry"></a>Importación desde un registro público

### <a name="import-from-docker-hub"></a>Importación desde Docker Hub

Por ejemplo, use el comando [az acr import][az-acr-import] para importar la imagen `hello-world:latest` de varias arquitecturas desde Docker Hub en un registro denominado *myregistry*. Dado que `hello-world` es una imagen oficial de Docker Hub, esta imagen está en el repositorio `library` predeterminado. Incluya el nombre del repositorio y, opcionalmente, una etiqueta en el valor del parámetro de imagen `--source`. (Puede, opcionalmente, identificar una imagen por su hash de manifiesto en vez de por etiqueta, lo que garantiza una versión determinada de una imagen).
 
```azurecli
az acr import \
  --name myregistry \
  --source docker.io/library/hello-world:latest \
  --image hello-world:latest
```

Para comprobar que varios manifiestos estén asociados a esta imagen, ejecute el comando `az acr repository show-manifests`:

```azurecli
az acr repository show-manifests \
  --name myregistry \
  --repository hello-world
```

El siguiente ejemplo importa una imagen pública del repositorio `tensorflow` en el Docker Hub:

```azurecli
az acr import \
  --name myregistry \
  --source docker.io/tensorflow/tensorflow:latest-gpu \
  --image tensorflow:latest-gpu
```

### <a name="import-from-microsoft-container-registry"></a>Importación desde el Registro de contenedor de Microsoft

Por ejemplo, importe la imagen de Windows Server Core más reciente desde el repositorio de `windows` en el Registro de contenedor de Microsoft.

```azurecli
az acr import \
--name myregistry \
--source mcr.microsoft.com/windows/servercore:latest \
--image servercore:latest
```

## <a name="import-from-another-azure-container-registry"></a>Importación desde otro registro de contenedor de Azure

Puede importar una imagen desde otro registro de contenedor de Azure con los permisos integrados de Azure Active Directory.

* La identidad debe tener permisos de Azure Active Directory para leer desde el registro de origen (rol de lector) y escribir en el registro de destino (rol de colaborador).

* El registro puede estar en la misma suscripción de Azure, u otra diferente, en el mismo inquilino de Active Directory.

### <a name="import-from-a-registry-in-the-same-subscription"></a>Importación desde un registro en la misma suscripción

Por ejemplo, importe la imagen `aci-helloworld:latest` desde un registro de origen *mysourceregistry* a *myregistry* en la misma suscripción de Azure.

```azurecli
az acr import \
  --name myregistry \
  --source mysourceregistry.azurecr.io/aci-helloworld:latest \
  --image aci-helloworld:latest
```

En el ejemplo siguiente se importa una imagen mediante el hash del manifiesto (hash de SHA-256, representado como `sha256:...`) en lugar de mediante etiqueta:

```azurecli
az acr import \
  --name myregistry \
  --source mysourceregistry.azurecr.io/aci-helloworld@sha256:123456abcdefg 
```

### <a name="import-from-a-registry-in-a-different-subscription"></a>Importación desde un registro en una suscripción distinta

En el ejemplo siguiente, *mysourceregistry* está en una suscripción diferente de *myregistry* en el mismo inquilino de Active Directory. Proporcione el identificador de recurso del registro de origen con el parámetro `--registry`. Tenga en cuenta que el parámetro `--source` especifica solo el repositorio de origen y el nombre de la imagen, no el nombre del servidor de inicio de sesión del registro.

```azurecli
az acr import \
  --name myregistry \
  --source samples/aci-helloworld:latest \
  --image aci-hello-world:latest \
  --registry /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/sourceResourceGroup/providers/Microsoft.ContainerRegistry/registries/mysourceregistry
```

### <a name="import-from-a-registry-using-service-principal-credentials"></a>Importación desde un registro con credenciales de entidad de servicio

Para importar desde un registro al que no se puede tener acceso mediante permisos de Active Directory, puede usar credenciales de entidad de servicio (si están disponibles). Proporcione el appID y la contraseña de la [entidad de servicio](container-registry-auth-service-principal.md) de Active Directory que tiene acceso ACRPull al registro de origen. El uso de una entidad de servicio es útil para sistemas de compilación y otros sistemas desatendidos que deban importar imágenes en el registro.

```azurecli
az acr import \
  --name myregistry \
  --source sourceregistry.azurecr.io/sourcerrepo:tag \
  --image targetimage:tag \
  --username <SP_App_ID> \
  –-password <SP_Passwd>
```

## <a name="import-from-a-non-azure-private-container-registry"></a>Importación desde un registro de contenedor privado de Azure

Importe una imagen desde un registro privado mediante la especificación de credenciales que permiten el acceso de extracción en el registro. Por ejemplo, extraiga una imagen de un registro privado de Docker: 

```azurecli
az acr import \
  --name myregistry \
  --source docker.io/sourcerepo/sourceimage:tag \
  --image sourceimage:tag \
  --username <username> \
  --password <password>
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido sobre la importación de imágenes de contenedor en un registro de contenedor de Azure desde un registro público u otro registro privado. Para obtener opciones de importación de imágenes adicionales, vea la referencia del comando [az acr import][az-acr-import]. 


<!-- LINKS - Internal -->
[az-login]: /cli/azure/reference-index#az-login
[az-acr-import]: /cli/azure/acr#az-acr-import
[azure-cli]: /cli/azure/install-azure-cli
