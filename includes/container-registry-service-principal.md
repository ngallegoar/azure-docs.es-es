---
title: archivo de inclusión
description: archivo de inclusión
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 12/14/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 9e4f2e355240ba8682cbe9f86f2be94e7dd0d92d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "70032357"
---
## <a name="create-a-service-principal"></a>Creación de una entidad de servicio

Para crear una entidad de servicio con acceso al registro de contenedor, ejecute el siguiente script en [Azure Cloud Shell](../articles/cloud-shell/overview.md) o en una instalación local de la [CLI de Azure](/cli/azure/install-azure-cli). El script tiene el formato adecuado para el shell de Bash.

Antes de ejecutar el script, actualice la variable `ACR_NAME` con el nombre del registro de contenedor. El valor `SERVICE_PRINCIPAL_NAME` debe ser único dentro del inquilino de Azure Active Directory. Si recibe un error "`'http://acr-service-principal' already exists.`", especifique un nombre diferente para la entidad de servicio.

Opcionalmente, puede modificar el valor `--role` en el comando [az ad sp create-for-rbac][az-ad-sp-create-for-rbac] si desea conceder permisos diferentes. Para obtener una lista completa de roles, consulte [ACR roles and permissions](https://github.com/Azure/acr/blob/master/docs/roles-and-permissions.md) (Roles y permisos de ACR).

Tras ejecutar el script, tome nota del **identificador** y la **contraseña** de la entidad de servicio. Cuando disponga de las credenciales, podrá configurar las aplicaciones y los servicios para autenticarse en su registro de contenedor como la entidad de servicio.

<!-- https://github.com/Azure-Samples/azure-cli-samples/blob/master/container-registry/service-principal-create/service-principal-create.sh -->
[!code-azurecli-interactive[acr-sp-create](~/cli_scripts/container-registry/service-principal-create/service-principal-create.sh)]

### <a name="use-an-existing-service-principal"></a>Uso de una entidad de servicio existente

Para conceder al registro acceso a una entidad de servicio existente, debe asignar un nuevo rol a la entidad de servicio. Al igual que con la creación de una nueva entidad de servicio, puede conceder acceso de extracción, inserción y extracción, así como de propietario, entre otros.

El script siguiente usa el comando [az role assignment create][az-role-assignment-create] para conceder permisos de *extracción* a la entidad de servicio especificada en la variable `SERVICE_PRINCIPAL_ID`. Ajuste el valor `--role` si quiere conceder un nivel de acceso diferente.


<!-- https://github.com/Azure-Samples/azure-cli-samples/blob/master/container-registry/service-principal-assign-role/service-principal-assign-role.sh -->
[!code-azurecli-interactive[acr-sp-role-assign](~/cli_scripts/container-registry/service-principal-assign-role/service-principal-assign-role.sh)]

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
