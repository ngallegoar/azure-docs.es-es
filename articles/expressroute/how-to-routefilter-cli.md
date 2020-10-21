---
title: 'Tutorial: Configuración de filtros de ruta para el emparejamiento de Microsoft: CLI de Azure'
description: En este tutorial se describe cómo configurar filtros de ruta para el emparejamiento de Microsoft mediante la CLI de Azure.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: tutorial
ms.date: 10/08/2020
ms.author: duau
ms.custom: devx-track-azurecli
ms.openlocfilehash: ac7fc5af21f11699331d41a074e88ae757170664
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91976001"
---
# <a name="tutorial-configure-route-filters-for-microsoft-peering-azure-cli"></a>Tutorial: Configuración de filtros de ruta para el emparejamiento de Microsoft: Azure CLI

> [!div class="op_single_selector"]
> * [Azure Portal](how-to-routefilter-portal.md)
> * [Azure PowerShell](how-to-routefilter-powershell.md)
> * [CLI de Azure](how-to-routefilter-cli.md)
> 

Los filtros de ruta son una forma de consumir un subconjunto de servicios admitidos mediante el emparejamiento de Microsoft. Los pasos que se describen en este artículo le ayudan a configurar y administrar filtros de ruta para circuitos ExpressRoute.

Los servicios de Microsoft 365, como Exchange Online, SharePoint Online y Skype Empresarial, están accesibles mediante el emparejamiento de Microsoft. Cuando se configura el emparejamiento de Microsoft en un circuito ExpressRoute, todos los prefijos relacionados con estos servicios se anuncian mediante las sesiones BGP establecidas. A cada prefijo se asocia un valor de la comunidad de BGP para identificar el servicio que se ofrece mediante el prefijo en cuestión. Para ver una lista de valores de la comunidad de BGP y los servicios a los que se asignan, consulte [Comunidades de BGP](expressroute-routing.md#bgp).

La conectividad a todos los servicios de Azure y Microsoft 365 genera un gran número de prefijos que se anuncian a través de BGP. Como consecuencia, aumenta considerablemente el tamaño de las tablas de rutas que mantienen los enrutadores dentro de la red. Si tiene previsto consumir solo un subconjunto de los servicios ofrecidos mediante el emparejamiento de Microsoft, puede reducir el tamaño de las tablas de rutas de dos maneras. Puede:

* Filtrar los prefijos no deseados mediante la aplicación de filtros de ruta en las comunidades de BGP. Los filtros de ruta son un procedimiento de red estándar y se usa normalmente en muchas redes.

* Definir filtros de ruta y aplicarlos al circuito ExpressRoute. Un filtro de ruta es un nuevo recurso que le permite seleccionar la lista de servicios que se va a consumir mediante el emparejamiento de Microsoft. Los enrutadores de ExpressRoute solo envían la lista de prefijos que pertenecen a los servicios identificados en el filtro de ruta.

En este tutorial, obtendrá información sobre cómo:
> [!div class="checklist"]
> - Conseguir valores de comunidad de BGP.
> - Creación de un filtro de ruta y una regla de filtro.
> - Asociación del filtro de ruta a un circuito ExpressRoute.

### <a name="about-route-filters"></a><a name="about"></a>Acerca de los filtros de ruta

Cuando se configura el emparejamiento de Microsoft en el circuito ExpressRoute, los enrutadores de Microsoft Edge establecen un par de sesiones BGP con los enrutadores perimetrales suyos o del proveedor de conectividad. No se anuncia ningún enrutador en la red. Para habilitar los anuncios de rutas en la red, debe asociar un filtro de ruta.

Un filtro de ruta permite identificar los servicios que desea consumir mediante el emparejamiento de Microsoft del circuito ExpressRoute. Se trata básicamente de una lista de todos los valores permitidos de la comunidad de BGP. Una vez que se define un recurso de filtro de ruta y se asocia a un circuito ExpressRoute, todos los prefijos que se asignan a valores de la comunidad de BGP se anuncian en la red.

Para asociar filtros de ruta con servicios de Microsoft 365, debe tener autorización para consumir tales servicios mediante ExpressRoute. Si no la tiene, la operación para asociar filtros de ruta producirá un error. Para obtener más información sobre el proceso de autorización, consulte [Azure ExpressRoute para Microsoft 365](/microsoft-365/enterprise/azure-expressroute).

> [!IMPORTANT]
> Se anunciarán todos los prefijos de servicio para el emparejamiento de Microsoft de los circuitos ExpressRoute que se configuraron antes del 1 de agosto de 2017, incluso si no se definen filtros de ruta. No se anunciará ningún prefijo para el emparejamiento de Microsoft de los circuitos ExpressRoute que se configuraron el 1 de agosto de 2017 o con posterioridad, hasta que se asocie un filtro de ruta al circuito.
> 

## <a name="prerequisites"></a>Requisitos previos

Para conectarse correctamente a los servicios mediante el emparejamiento de Microsoft, debe realizar los siguientes pasos de configuración:

* Debe tener un circuito ExpressRoute activo que tenga el emparejamiento de Microsoft aprovisionado. Para realizar estas tareas, puede usar las siguientes instrucciones:
  * [Cree un circuito ExpressRoute](howto-circuit-cli.md) y pida a su proveedor de conectividad que se lo habilite antes de proceder. El circuito ExpressRoute debe estar en estado habilitado y aprovisionado.
  * Si administra la sesión BGP directamente, [cree el emparejamiento de Microsoft](howto-routing-cli.md). O bien, pida a su proveedor de conectividad que aprovisione el emparejamiento de Microsoft para su circuito.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Si decide instalar y usar la CLI localmente, para esta guía de inicio rápido se necesita la versión 2.0.28 de la CLI de Azure o una versión posterior. Para encontrar la versión, ejecute `az --version`. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli).

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Iniciar sesión en la cuenta de Azure y seleccione la suscripción

Para empezar la configuración, inicie sesión en la cuenta de Azure. Si usa "Probarlo", su sesión se iniciará automáticamente y podrá omitir el paso de inicio de sesión. Use los siguientes ejemplos para conectarse:

```azurecli
az login
```

Compruebe las suscripciones para la cuenta.

```azurecli-interactive
az account list
```

Seleccione la suscripción para la que desea crear un circuito ExpressRoute.

```azurecli-interactive
az account set --subscription "<subscription ID>"
```

## <a name="get-a-list-of-prefixes-and-bgp-community-values"></a><a name="prefixes"></a>Obtención de una lista de prefijos y valores de la comunidad de BGP

1. Use el siguiente cmdlet para obtener la lista de valores y prefijos de la comunidad de BGP asociados con los servicios accesibles mediante emparejamiento de Microsoft:

    ```azurecli-interactive
    az network route-filter rule list-service-communities
    ```

1. Cree una lista de valores de la comunidad de BGP que quiera usar en el filtro de ruta.

## <a name="create-a-route-filter-and-a-filter-rule"></a><a name="filter"></a>Creación de un filtro de ruta y una regla de filtro

Un filtro de ruta puede tener una única regla y la regla debe ser de tipo "Permitir". Esta regla puede tener una lista de valores de la comunidad de BGP asociados a ella. El comando `az network route-filter create` solo crea un recurso de filtro de ruta. Después de crear el recurso, debe crear una regla y asociarla al objeto de filtro de ruta.

1. Para crear un recurso de filtro de ruta, ejecute el comando siguiente:

    ```azurecli-interactive
    az network route-filter create -n MyRouteFilter -g MyResourceGroup
    ```

1. Para crear una regla de filtro de ruta, ejecute el comando siguiente:
 
    ```azurecli-interactive
    az network route-filter rule create --filter-name MyRouteFilter -n CRM --communities 12076:5040 --access Allow -g MyResourceGroup
    ```

## <a name="attach-the-route-filter-to-an-expressroute-circuit"></a><a name="attach"></a>Asociación del filtro de ruta a un circuito ExpressRoute

Ejecute el siguiente comando para asociar el filtro de ruta al circuito ExpressRoute:

```azurecli-interactive
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroupName --name MicrosoftPeering --route-filter MyRouteFilter
```

## <a name="common-tasks"></a><a name="tasks"></a>Tareas comunes

### <a name="to-get-the-properties-of-a-route-filter"></a><a name="getproperties"></a>Obtención de las propiedades de un filtro de ruta

Para obtener las propiedades de un filtro de ruta, utilice el siguiente comando:

```azurecli-interactive
az network route-filter show -g ExpressRouteResourceGroupName --name MyRouteFilter 
```

### <a name="to-update-the-properties-of-a-route-filter"></a><a name="updateproperties"></a>Actualización de las propiedades de un filtro de ruta

Si el filtro de ruta ya está asociado a un circuito, las actualizaciones de la lista de la comunidad de BGP propagan automáticamente los cambios en los anuncios de prefijos mediante las sesiones BGP establecidas. Puede actualizar la lista de la comunidad de BGP de su filtro de ruta con el comando siguiente:

```azurecli-interactive
az network route-filter rule update --filter-name MyRouteFilter -n CRM -g ExpressRouteResourceGroupName --add communities '12076:5040' --add communities '12076:5010'
```

### <a name="to-detach-a-route-filter-from-an-expressroute-circuit"></a><a name="detach"></a>Desasociación de un filtro de ruta de un circuito ExpressRoute

Una vez que un filtro de ruta se desasocia del circuito ExpressRoute, no se anuncia ningún prefijo mediante la sesión BGP. Puede desasociar un filtro de ruta de un circuito ExpressRoute con el comando siguiente:

```azurecli-interactive
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroupName --name MicrosoftPeering --remove routeFilter
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Solo se puede eliminar un filtro de ruta si no está asociado a ningún circuito. Asegúrese de que el filtro de ruta no esté asociado a ningún circuito antes de intentar eliminarlo. Puede eliminar un filtro de ruta con el comando siguiente:

```azurecli-interactive
az network route-filter delete -n MyRouteFilter -g MyResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las muestras de configuración de ruta, vea:

> [!div class="nextstepaction"]
> [Ejemplos de configuración de enrutadores para configurar y administrar enrutamiento](expressroute-config-samples-routing.md)
