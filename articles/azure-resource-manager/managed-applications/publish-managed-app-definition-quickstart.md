---
title: Creación de la definición de aplicación administrada
description: Se explica cómo crear una aplicación administrada de Azure diseñada para los miembros de su organización.
author: tfitzmac
ms.topic: quickstart
ms.date: 09/13/2019
ms.author: tomfitz
ms.openlocfilehash: 5168dab7501cdb2907b3eede5877dce39e9e5c97
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "79473054"
---
# <a name="quickstart-publish-an-azure-managed-application-definition"></a>Inicio rápido: Publicación de una definición de aplicación administrada de Azure

En esta guía de inicio rápido se proporciona una introducción al uso de aplicaciones administradas. Agregará una definición de aplicación administrada a un catálogo interno para los usuarios de su organización. Para simplificar la introducción, ya se han creado los archivos para la aplicación administrada. Esos archivos están disponibles a través de GitHub. Aprenderá a crear esos archivos en el tutorial [Creación de la aplicación de catálogo de servicio](publish-service-catalog-app.md).

Cuando haya terminado, tendrá un grupo de recursos llamado **appDefinitionGroup** que tiene la definición de aplicación administrada.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group-for-definition"></a>Creación de un grupo de recursos para la definición

La definición de la aplicación administrada existe en un grupo de recursos. El grupo de recursos de Azure es una colección lógica en la que se implementan y administran los recursos de Azure.

Para crear un grupo de recursos, ejecute el comando siguiente:

```azurecli-interactive
az group create --name appDefinitionGroup --location westcentralus
```

## <a name="create-the-managed-application-definition"></a>Creación de la definición de aplicación administrada

Al definir la aplicación administrada, seleccionará un usuario, un grupo o una aplicación que administra los recursos del consumidor. Esta identidad tiene permisos en el grupo de recursos administrado, según el rol asignado. Si es necesario, creará un grupo de usuarios de Azure Active Directory para administrar los recursos. Sin embargo, en este artículo, usará su propia identidad.

Para obtener el identificador de objeto de su identidad, proporcione su nombre principal de usuario en el siguiente comando:

```azurecli-interactive
userid=$(az ad user show --id example@contoso.org --query objectId --output tsv)
```

A continuación, necesitará el identificador de definición de rol del rol RBAC integrado al que desea conceder acceso al usuario. El comando siguiente muestra cómo obtener el identificador de definición de rol para el rol Propietario:

```azurecli-interactive
roleid=$(az role definition list --name Owner --query [].name --output tsv)
```

Ahora, cree el recurso de la definición de aplicación administrada. La aplicación administrada contiene solo una cuenta de almacenamiento.

```azurecli-interactive
az managedapp definition create \
  --name "ManagedStorage" \
  --location "westcentralus" \
  --resource-group appDefinitionGroup \
  --lock-level ReadOnly \
  --display-name "Managed Storage Account" \
  --description "Managed Azure Storage Account" \
  --authorizations "$userid:$roleid" \
  --package-file-uri "https://github.com/Azure/azure-managedapp-samples/raw/master/Managed%20Application%20Sample%20Packages/201-managed-storage-account/managedstorage.zip"
```

Cuando finalice el comando, tendrá una definición de aplicación administrada en el grupo de recursos. 

Algunos de los parámetros usados en el ejemplo anterior son:

* **resource-group**: El nombre del grupo de recursos donde se creó la definición de aplicación administrada.
* **lock-level**: El tipo de bloqueo aplicado al grupo de recursos administrado. Impide que el cliente realice operaciones no deseadas en este grupo de recursos. Actualmente, ReadOnly es el único nivel de bloqueo admitido. Cuando se especifica ReadOnly, el cliente solo puede leer los recursos presentes en el grupo de recursos administrado. Las identidades del publicador a las que se concede acceso al grupo de recursos administrados están exentas del bloqueo.
* **authorizations**: Describe el identificador de entidad de seguridad y el identificador de definición de rol que se usan para conceder el permiso al grupo de recursos administrado. Se especifica en el formato `<principalId>:<roleDefinitionId>`. Si se necesita más de un valor, especifíquelos en el formulario `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`. Los valores van separados por un espacio.
* **package-file-uri**: La ubicación de un paquete .zip que contiene los archivos necesarios. El paquete debe contener los archivos **mainTemplate.json** y **createUiDefinition.json**. **mainTemplate.json** define los recursos de Azure que se crean como parte de la aplicación administrada. La plantilla no difiere de una plantilla habitual de Resource Manager. **createUiDefinition.json** genera la interfaz de usuario para los usuarios que crean la aplicación administrada mediante el portal.

## <a name="next-steps"></a>Pasos siguientes

Ha publicado la definición de aplicación administrada. Ahora, aprenda a implementar una instancia de esa definición.

> [!div class="nextstepaction"]
> [Inicio rápido: Implementación de la aplicación de catálogo de servicios](deploy-service-catalog-quickstart.md)
