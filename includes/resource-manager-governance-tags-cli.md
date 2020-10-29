---
title: archivo de inclusión
description: archivo de inclusión
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/20/2018
ms.author: tomfitz
ms.custom: include file, devx-track-azurecli
ms.openlocfilehash: 53d4aa70b55577cdf2f6a1b898b496eb368157f5
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92755523"
---
Para agregar dos etiquetas a un grupo de recursos, use el comando [az group update](/cli/azure/group):

```azurecli-interactive
az group update -n myResourceGroup --set tags.Environment=Test tags.Dept=IT
```

Supongamos que desea agregar una tercera etiqueta. Vuelva a ejecutar el comando con la nueva etiqueta. Se anexa a las etiquetas existentes.

```azurecli-interactive
az group update -n myResourceGroup --set tags.Project=Documentation
```

Los recursos no heredan las etiquetas del grupo de recursos. Actualmente, el grupo de recursos tiene tres etiquetas pero los recursos no tienen ninguna. Para aplicar todas las etiquetas de un grupo de recursos a sus recursos y conservar las etiquetas existentes en los recursos, use el siguiente script:

```azurecli-interactive
# Get the tags for the resource group
jsontag=$(az group show -n myResourceGroup --query tags)

# Reformat from JSON to space-delimited and equals sign
t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')

# Get the resource IDs for all resources in the resource group
r=$(az resource list -g myResourceGroup --query [].id --output tsv)

# Loop through each resource ID
for resid in $r
do
  # Get the tags for this resource
  jsonrtag=$(az resource show --id $resid --query tags)
  
  # Reformat from JSON to space-delimited and equals sign
  rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
  
  # Reapply the updated tags to this resource
  az resource tag --tags $t$rt --id $resid
done
```

Como alternativa, puede aplicar las etiquetas desde el grupo de recursos a los recursos sin necesidad de mantener las etiquetas existentes:

```azurecli-interactive
# Get the tags for the resource group
jsontag=$(az group show -n myResourceGroup --query tags)

# Reformat from JSON to space-delimited and equals sign
t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')

# Get the resource IDs for all resources in the resource group
r=$(az resource list -g myResourceGroup --query [].id --output tsv)

# Loop through each resource ID
for resid in $r
do
  # Apply tags from resource group to this resource
  az resource tag --tags $t --id $resid
done
```

Para combinar varios valores en una única etiqueta, utilice una cadena JSON.

```azurecli-interactive
az group update -n myResourceGroup --set tags.CostCenter='{"Dept":"IT","Environment":"Test"}'
```

Para quitar todas las etiquetas de un grupo de recursos, use:

```azurecli-interactive
az group update -n myResourceGroup --remove tags
```
