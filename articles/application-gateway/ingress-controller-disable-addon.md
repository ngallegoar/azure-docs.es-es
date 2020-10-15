---
title: Deshabilitación y nueva habilitación del complemento del controlador de entrada de Application Gateway para el clúster de Azure Kubernetes Service
description: En este artículo se proporciona información sobre cómo deshabilitar y volver a habilitar el complemento de AGIC para el clúster de AKS.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: how-to
ms.date: 06/10/2020
ms.author: caya
ms.openlocfilehash: fe4da0435731c536a723cb2cb43428166456360b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84807950"
---
# <a name="disable-and-re-enable-agic-add-on-for-your-aks-cluster"></a>Deshabilitación y nueva habilitación del complemento de AGIC para el clúster de AKS
El controlador de entrada de Application Gateway (AGIC), implementado como un complemento de AKS, permite habilitar y deshabilitar el complemento con una línea en la CLI de Azure. El ciclo de vida de la instancia de Application Gateway será diferente cuando deshabilite el complemento de AGIC, en función de si el complemento de AGIC creó la instancia de Application Gateway o si esta se implementó de forma separada desde el complemento de AGIC. Puede ejecutar el mismo comando para volver a habilitar el complemento de AGIC si alguna vez lo deshabilita o para habilitar el complemento de AGIC con un clúster de AKS y una instancia de Application Gateway existentes.

## <a name="disabling-agic-add-on-with-associated-application-gateway"></a>Deshabilitación del complemento de AGIC con la instancia de Application Gateway asociada 
Si el complemento de AGIC implementó automáticamente la instancia de Application Gateway cuando se configuró por primera vez, al deshabilitar el complemento de AGIC se eliminará de forma predeterminada la instancia de Application Gateway en función de un par de criterios. El complemento de AGIC examina dos criterios para determinar si se debe eliminar la instancia de Application Gateway asociada al deshabilitarlo:
- ¿La instancia de Application Gateway asociada al complemento de AGIC se ha implementado en el grupo de recursos del nodo MC_*? 
- ¿La instancia de Application Gateway asociada al complemento de AGIC tiene la etiqueta "created-by: ingress-appgw"? AGIC usa la etiqueta para determinar si el complemento implementó la instancia de Application Gateway. 

Si se cumplen ambos criterios, el complemento de AGIC eliminará la instancia de Application Gateway creada cuando se deshabilite el complemento; sin embargo, no eliminará la dirección IP pública ni la subred en la que se implementó la instancia de Application Gateway. Si no se cumple el primer criterio, no importa si la instancia de Application Gateway tiene la etiqueta "created-by: ingress-appgw": no se eliminará la instancia de Application Gateway al deshabilitar el complemento. Del mismo modo, si no se cumple el segundo criterio, es decir, la instancia de Application Gateway no tiene esa etiqueta, no se eliminará la instancia de Application Gateway del grupo de recursos del nodo MC_* al deshabilitar el complemento. 

> [!TIP] 
> Si no desea que se elimine la instancia de Application Gateway al deshabilitar el complemento, pero se cumplen ambos criterios, elimine la etiqueta "created-by: ingress-appgw" para evitar que el complemento elimine la instancia de Application Gateway. 

Para deshabilitar el complemento de AGIC, ejecute el comando siguiente: 
```azurecli-interactive
az aks disable-addons -n <AKS-cluster-name> -g <AKS-resource-group-name> -a ingress-appgw 
```

## <a name="enable-agic-add-on-on-existing-application-gateway-and-aks-cluster"></a>Habilitación del complemento de AGIC en un clúster de AKS y una instancia de Application Gateway existentes
Si alguna vez deshabilita el complemento de AGIC y necesita volver a habilitar el complemento o si desea habilitar el complemento con un clúster de AKS y una instancia de Application Gateway existentes, ejecute el comando siguiente:

```azurecli-interactive
appgwId=$(az network application-gateway show -n <application-gateway-name> -g <resource-group-name> -o tsv --query "id") 
az aks enable-addons -n <AKS-cluster-name> -g <AKS-cluster-resource-group> -a ingress-appgw --appgw-id $appgwId
```

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre cómo habilitar el complemento de AGIC con un clúster de AKS y una instancia de Application Gateway existentes, consulte [Implementación Brownfield del complemento de AGIC](tutorial-ingress-controller-add-on-existing.md).