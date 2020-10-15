---
title: archivo de inclusión
description: archivo de inclusión
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 05/21/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 069baf627c0230b6a4727c375494352ab3e6a803
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "67186249"
---
Para agregar dos etiquetas a un grupo de recursos, use el comando [Set-AzResourceGroup](/powershell/module/az.resources/set-azresourcegroup):

```azurepowershell-interactive
Set-AzResourceGroup -Name myResourceGroup -Tag @{ Dept="IT"; Environment="Test" }
```

Supongamos que desea agregar una tercera etiqueta. Cada vez que aplique etiquetas a un recurso o grupo de recursos, sobrescribirá las etiquetas existentes en ese recurso o grupo de recursos. Para agregar una nueva etiqueta sin perder las etiquetas existentes, debe recuperar estas, agregar una nueva etiqueta y volver a aplicar la colección de etiquetas:

```azurepowershell-interactive
# Get existing tags and add a new tag
$tags = (Get-AzResourceGroup -Name myResourceGroup).Tags
$tags.Add("Project", "Documentation")

# Reapply the updated set of tags 
Set-AzResourceGroup -Tag $tags -Name myResourceGroup
```

Los recursos no heredan las etiquetas del grupo de recursos. Actualmente, el grupo de recursos tiene tres etiquetas pero los recursos no tienen ninguna. Para aplicar todas las etiquetas de un grupo de recursos a sus recursos y conservar las etiquetas existentes en los recursos que no son duplicados, use el siguiente script:

```azurepowershell-interactive
# Get the resource group
$group = Get-AzResourceGroup myResourceGroup

if ($group.Tags -ne $null) {
    # Get the resources in the resource group
    $resources = Get-AzResource -ResourceGroupName $group.ResourceGroupName

    # Loop through each resource
    foreach ($r in $resources)
    {
        # Get the tags for this resource
        $resourcetags = (Get-AzResource -ResourceId $r.ResourceId).Tags
        
        # If the resource has existing tags, add new ones
        if ($resourcetags)
        {
            foreach ($key in $group.Tags.Keys)
            {
                if (-not($resourcetags.ContainsKey($key)))
                {
                    $resourcetags.Add($key, $group.Tags[$key])
                }
            }

            # Reapply the updated tags to the resource 
            Set-AzResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
        else
        {
            Set-AzResource -Tag $group.Tags -ResourceId $r.ResourceId -Force
        }
    }
}
```

Como alternativa, puede aplicar las etiquetas desde el grupo de recursos a los recursos sin necesidad de mantener las etiquetas existentes:

```azurepowershell-interactive
# Get the resource group
$g = Get-AzResourceGroup -Name myResourceGroup

# Find all the resources in the resource group, and for each resource apply the tags from the resource group
Get-AzResource -ResourceGroupName $g.ResourceGroupName | ForEach-Object {Set-AzResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
```

Para combinar varios valores en una única etiqueta, utilice una cadena JSON.

```azurepowershell-interactive
Set-AzResourceGroup -Name myResourceGroup -Tag @{ CostCenter="{`"Dept`":`"IT`",`"Environment`":`"Test`"}" }
```

Para agregar una nueva etiqueta con varios valores sin perder las etiquetas existentes, debe recuperar estas, usar una cadena JSON para la nueva etiqueta y volver a aplicar la colección de etiquetas:

```azurepowershell-interactive
# Get existing tags and add a new tag
$ResourceGroup = Get-AzResourceGroup -Name myResourceGroup
$Tags = $ResourceGroup.Tags
$Tags.Add("CostCenter", "{`"Dept`":`"IT`",`"Environment`":`"Test`"}")

# Reapply the updated set of tags
$ResourceGroup | Set-AzResourceGroup -Tag $Tags
```

Para quitar todas las etiquetas, pase una tabla hash vacía.

```azurepowershell-interactive
Set-AzResourceGroup -Name myResourceGroup -Tag @{ }
```
