---
title: Configuración del acceso a un registro público
description: Configure reglas de IP para permitir el acceso a un registro de contenedor de Azure desde intervalos de direcciones o direcciones IP públicas seleccionadas.
ms.topic: article
ms.date: 05/19/2020
ms.openlocfilehash: dc0514fbe7d3e01914965cee5dc547172d4435a4
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83702091"
---
# <a name="configure-public-ip-network-rules"></a>Configuración de reglas de red de dirección IP pública

De manera predeterminada los registros de contenedor de aceptan las conexiones a través de Internet de hosts de cualquier red. En este artículo se muestra cómo configurar el registro de contenedor para permitir el acceso solo desde intervalos de direcciones o direcciones IP públicas específicas. Se proporcionan los pasos equivalentes si se usa mediante la CLI de Azure y Azure Portal.

Las reglas de red IP se configuran en el punto de conexión del registro público. Las reglas de red IP no se aplican a los puntos de conexión privados configurados con [Private Link](container-registry-private-link.md).

La configuración de reglas de acceso IP está disponible en el nivel de servicio de un registro de contenedor **Premium**. Para obtener información sobre los límites y niveles del servicio de registro, vea [Niveles de Azure Container Registry](container-registry-skus.md).

## <a name="access-from-selected-public-network---cli"></a>Acceso desde una red pública seleccionada: CLI

### <a name="change-default-network-access-to-registry"></a>Cambio del acceso de red predeterminado al registro

Para limitar el acceso a una red pública seleccionada, primero cambie la acción predeterminada para denegar el acceso. Sustituya el nombre del registro en el siguiente comando [az acr update][az-acr-update]:

```azurecli
az acr update --name myContainerRegistry --default-action Deny
```

### <a name="add-network-rule-to-registry"></a>Incorporación de una regla de red al registro

Use el comando [az acr network-rule add][az-acr-network-rule-add] para agregar una regla de red al registro que permita el acceso desde una dirección IP pública o un intervalo. Por ejemplo, sustituya el nombre del registro de contenedor y la dirección IP pública de una máquina virtual de una red virtual.

```azurecli
az acr network-rule add \
  --name mycontainerregistry \
  --ip-address <public-IP-address>
```

> [!NOTE]
> Después de agregar una regla, esta tarda unos minutos en aplicarse.

## <a name="access-from-selected-public-network---portal"></a>Acceso desde una red pública seleccionada: portal

1. En el portal, vaya al registro de contenedor.
1. En **Configuración**, seleccione **Redes**.
1. En la pestaña **Acceso público**, permita el acceso público desde **Redes seleccionadas**.
1. En **Firewall**, escriba una dirección IP pública, como la dirección IP pública de una máquina virtual de una red virtual. O bien, escriba un rango de direcciones en notación CIDR que contenga la dirección IP de la máquina virtual.
1. Seleccione **Guardar**.

![Configurar una regla de firewall para el registro de contenedor][acr-access-selected-networks]

> [!NOTE]
> Después de agregar una regla, esta tarda unos minutos en aplicarse.

> [!TIP]
> De forma opcional, habilite el acceso al registro desde un equipo cliente local o un intervalo de direcciones IP. Para permitir este acceso, necesita la dirección IPv4 pública del equipo. Para encontrar esta dirección, busque "cuál es mi dirección IP" en un explorador de Internet. La dirección IPv4 del cliente actual también aparece automáticamente cuando se establece la configuración del firewall en la página **Redes** del portal.

## <a name="disable-public-network-access"></a>Deshabilitación del acceso a una red pública

De forma opcional, deshabilite el punto de conexión público en el registro. Al deshabilitar el punto de conexión público, se invalidan todas las configuraciones del firewall. Por ejemplo, puede que quiera deshabilitar el acceso público a un registro protegido de una red virtual mediante [Private Link](container-registry-private-link.md).

### <a name="disable-public-access---cli"></a>Deshabilitación del acceso público: CLI

Para deshabilitar el acceso público mediante la CLI de Azure, ejecute [az acr update][az-acr-update] y establezca `--public-network-enabled` en `false`. 

> [!NOTE]
> El argumento `public-network-enabled` requiere una versión 2.6.0 o posterior de la CLI de Azure. 

```azurecli
az acr update --name myContainerRegistry --public-network-enabled false
```

### <a name="disable-public-access---portal"></a>Deshabilitación del acceso público: portal

1. En el portal, vaya al registro de contenedor y seleccione **Configuración > Redes**.
1. En la pestaña **Acceso público**, en **Permitir el acceso de red público**, seleccione **Deshabilitado**. Después, seleccione **Guardar**.

![Deshabilitación del acceso público][acr-access-disabled]


## <a name="restore-public-network-access"></a>Restauración del acceso a una red pública

Para volver a habilitar el punto de conexión público, actualice la configuración de redes para permitir el acceso público. Al habilitar el punto de conexión público, se invalidan todas las configuraciones del firewall. 

### <a name="restore-public-access---cli"></a>Restauración del acceso público: CLI

Ejecute [az acr update][az-acr-update] y establezca `--public-network-enabled` en `true`. 

> [!NOTE]
> El argumento `public-network-enabled` requiere una versión 2.6.0 o posterior de la CLI de Azure. 

```azurecli
az acr update --name myContainerRegistry --public-network-enabled true
```

### <a name="restore-public-access---portal"></a>Restauración del acceso público: portal

1. En el portal, vaya al registro de contenedor y seleccione **Configuración > Redes**.
1. En la pestaña **Acceso público**, en **Permitir el acceso de red público**, seleccione **Todas las redes**. Después, seleccione **Guardar**.

![Acceso público desde todas las redes][acr-access-all-networks]

## <a name="next-steps"></a>Pasos siguientes

* Para restringir el acceso a un registro mediante un punto de conexión privado de una red virtual, vea [Configuración de Azure Private Link para un registro de contenedor de Azure](container-registry-private-link.md).
* Si necesita configurar reglas de acceso a un registro desde detrás de un firewall cliente, vea [Configuración de reglas para acceder a un registro de contenedor de Azure desde detrás de un firewall](container-registry-firewall-access-rules.md).

[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-network-rule-add]: /cli/azure/acr/network-rule/#az-acr-network-rule-add
[az-acr-network-rule-remove]: /cli/azure/acr/network-rule/#az-acr-network-rule-remove
[az-acr-network-rule-list]: /cli/azure/acr/network-rule/#az-acr-network-rule-list
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-acr-update]: /cli/azure/acr#az-acr-update
[quickstart-portal]: container-registry-get-started-portal.md
[quickstart-cli]: container-registry-get-started-azure-cli.md
[azure-portal]: https://portal.azure.com

[acr-access-selected-networks]: ./media/container-registry-access-selected-networks/acr-access-selected-networks.png
[acr-access-disabled]: ./media/container-registry-access-selected-networks/acr-access-disabled.png
[acr-access-all-networks]: ./media/container-registry-access-selected-networks/acr-access-all-networks.png
