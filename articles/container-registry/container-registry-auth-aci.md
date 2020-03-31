---
title: Acceso desde Container Instances
description: Aprenda a proporcionar acceso a las imágenes de su registro de contenedor privado desde Azure Container Instances mediante una entidad de servicio de Azure Active Directory.
ms.topic: article
ms.date: 04/23/2018
ms.openlocfilehash: b1bc8119c495dea99c6bdc4923db198d041a1e9e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74456507"
---
# <a name="authenticate-with-azure-container-registry-from-azure-container-instances"></a>Autenticación con Azure Container Registry desde Azure Container Instances

Puede usar una entidad de servicio de Azure Active Directory (Azure AD) para proporcionar acceso a sus registros de contenedor privados en Azure Container Registry.

En este artículo, aprenderá a crear y configurar una entidad de servicio de Azure AD con permisos de *extracción* para el registro. A continuación, iniciará un contenedor en Azure Container Instances (ACI) que extraerá la imagen de su registro privado usando la entidad de servicio para la autenticación.

## <a name="when-to-use-a-service-principal"></a>Cuándo usar una entidad de servicio

Debe usar una entidad de servicio para la autenticación desde ACI en **escenarios de equipos sin periféricos**, como, por ejemplo aplicaciones o servicios que crean instancias de contenedor de manera automática o desatendida.

Por ejemplo, si tiene un script automatizado que se ejecuta cada noche y crea una [instancia de contenedor basado en tareas](../container-instances/container-instances-restart-policy.md) para procesar determinados datos, este puede usar una entidad de servicio con permisos de solo extracción para autenticarse en el Registro. A continuación, puede intercambiar las credenciales de la entidad de servicio o revocar el acceso por completo sin que ello afecte a otros servicios y aplicaciones.

Las entidades de servicio también deben usarse cuando el [usuario administrador](container-registry-authentication.md#admin-account) del registro está deshabilitado.

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="authenticate-using-the-service-principal"></a>Autenticación mediante la entidad de servicio

Para iniciar un contenedor en Azure Container Instances mediante una entidad de servicio, especifique su identificador para `--registry-username` y su contraseña para `--registry-password`.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerregistry.azurecr.io/myimage:v1 \
    --registry-login-server mycontainerregistry.azurecr.io \
    --registry-username <service-principal-ID> \
    --registry-password <service-principal-password>
```

## <a name="sample-scripts"></a>Muestras de scripts

Puede encontrar los scripts del ejemplo anterior para la CLI de Azure en GitHub, así como versiones para Azure PowerShell:

* [CLI de Azure][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

## <a name="next-steps"></a>Pasos siguientes

Los artículos siguientes contienen detalles adicionales sobre cómo trabajar con entidades de servicio y ACR:

* [Autenticación de Azure Container Registry con entidades de servicio](container-registry-auth-service-principal.md)
* [Autenticación con Azure Container Registry desde Azure Kubernetes Service (AKS)](../aks/cluster-container-registry-integration.md)

<!-- IMAGES -->

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
