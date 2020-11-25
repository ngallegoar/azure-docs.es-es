---
title: 'Inicio rápido: Creación de un almacén de claves de Azure Key Vault con la CLI de Azure'
description: Inicio rápido que muestra cómo crear un almacén de claves de Azure Key Vault mediante la CLI de Azure
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: quickstart
ms.date: 07/20/2020
ms.author: mbaldwin
ms.openlocfilehash: d84f500560031322cff32005b537037dc897919e
ms.sourcegitcommit: 5831eebdecaa68c3e006069b3a00f724bea0875a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94516771"
---
# <a name="quickstart-create-a-key-vault-using-the-azure-cli"></a>Inicio rápido: Creación de un almacén de claves mediante la CLI de Azure

Azure Key Vault es un servicio de almacenamiento seguro en la nube para [claves](../keys/index.yml), [secretos](../secrets/index.yml) y [certificados](../certificates/index.yml). Para obtener más información sobre Key Vault, consulte el artículo [Acerca de Azure Key Vault](overview.md); para obtener más información sobre lo que se puede almacenar en un almacén de claves, consulte el artículo [Acerca de las claves, secretos y certificados](about-keys-secrets-certificates.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Para realizar este inicio rápido es necesaria la versión 2.0.4 o posterior de la CLI de Azure. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Un grupo de recursos es un contenedor lógico en el que se implementan y se administran los recursos de Azure. En el ejemplo siguiente, se crea un grupo de recursos denominado *ContosoResourceGroup* en la ubicación *eastus*.

```azurecli
az group create --name "myResourceGroup" -l "EastUS"
```

## <a name="create-a-key-vault"></a>Creación de un Almacén de claves

Cree un almacén de claves en el grupo de recursos del paso anterior. Tendrá que proporcionar algo de información:

- Nombre del almacén de claves: cadena de entre 3 y 24 caracteres que puede contener solo números (0-9), letras (a-z, A-Z) y guiones (-).

  > [!Important]
  > Cada almacén de claves debe tener un nombre único. Reemplace <nombre-almacén de claves-único> por el nombre del almacén de claves en los ejemplos siguientes.

- Nombre del grupo de recursos: **myResourceGroup**.
- Ubicación: **EastUS**.

```azurecli
az keyvault create --name "<your-unique-keyvault-name>" --resource-group "myResourceGroup" --location "EastUS"
```

La salida de este cmdlet muestra las propiedades del almacén de claves que acaba de crear. Tome nota de las dos propiedades siguientes:

- **Nombre del almacén**: nombre que proporcionó al parámetro --name anterior.
- **URI de almacén**: en el ejemplo, es https://&lt;su-nombre-de-almacén-de-claves-único&gt;.vault.azure.net/. Las aplicaciones que utilizan el almacén a través de su API de REST deben usar este identificador URI.

En este momento, su cuenta de Azure es la única autorizada para realizar operaciones en este nuevo almacén.

## <a name="clean-up-resources"></a>Limpieza de recursos

Otras guías de inicio rápido y tutoriales de esta colección se basan en los valores de esta. Si tiene pensado seguir trabajando en otras guías de inicio rápido y tutoriales, considere la posibilidad de dejar estos recursos activos.

Cuando ya no se necesiten, puede usar el comando [az group delete](/cli/azure/group) de la CLI de Azure para quitar el grupo de recursos y todos los recursos relacionados.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido ha creado un almacén de claves y lo ha eliminado. Para más información sobre Key Vault y cómo integrarlo con las aplicaciones, continúe con los artículos siguientes.

- Lea una [introducción a Azure Key Vault](overview.md).
- Consulte la referencia de los [comandos az keyvault de la CLI de Azure](/cli/azure/keyvault?view=azure-cli-latest).
- Consulte los [procedimientos recomendados de Azure Key Vault](best-practices.md).
