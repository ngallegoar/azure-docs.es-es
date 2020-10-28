---
title: 'Tutorial: Configuración del enrutamiento de tráfico prioritario con Azure Traffic Manager'
description: En este tutorial se explica cómo configurar el método de enrutamiento de tráfico prioritario en Traffic Manager
services: traffic-manager
documentationcenter: ''
author: duongau
ms.service: traffic-manager
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/16/2020
ms.author: duau
ms.openlocfilehash: 1835377f4690097c8390957bf7d897242ba7aace
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92208063"
---
# <a name="tutorial-configure-priority-traffic-routing-method-in-traffic-manager"></a>Tutorial: Configuración del método de enrutamiento de tráfico por prioridad en Traffic Manager

En este tutorial se describe cómo usar Azure Traffic Manager para enrutar el tráfico de usuario a puntos de conexión específicos mediante el método de enrutamiento prioritario. En este método de enrutamiento, definirá el orden de cada punto de conexión que entra en la configuración del perfil de Traffic Manager. El tráfico de los usuarios se enrutará al punto de conexión en el orden de enumeración. Este método de enrutamiento es útil cuando se desea realizar una configuración para la conmutación por error del servicio. El punto de conexión principal obtiene un número de prioridad de "1" y dará servicio a todas las solicitudes entrantes. Mientras, los puntos de conexión de menor prioridad actuarán como reserva.

En este tutorial, obtendrá información sobre cómo:

> [!div class="checklist"]
> - Crear un perfil de Traffic Manager con enrutamiento prioritario.
> - Agregar puntos de conexión.
> - Configurar la prioridad de los puntos de conexión.
> - Usar el perfil de Traffic Manager.
> - Eliminar el perfil de Traffic Manager.

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="to-configure-the-priority-traffic-routing-method"></a>Para configurar el método de enrutamiento de tráfico por prioridad
1. En un explorador, inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **+ Crear un recurso** en el lado izquierdo. Busque **Perfil de Traffic Manager** y seleccione **Crear** .

    :::image type="content" source="./media/traffic-manager-priority-routing-method/create-traffic-manager-priority-profile.png" alt-text="Creación de un perfil de prioridad de Traffic Manager":::

1. En la página *Crear perfil de Traffic Manager* , defina la siguiente configuración:

    | Configuración         | Valor                                              |
    | ---             | ---                                                |
    | Nombre            | Proporcione un nombre para el perfil. Este nombre debe ser único dentro de la zona trafficmanager.net. Para acceder a su perfil de Traffic Manager, use el nombre DNS `<profilename>.trafficmanager.net`. |    
    | Método de enrutamiento  | Seleccione **Prioridad** . |
    | Subscription    | Seleccione su suscripción. |
    | Resource group   | Use un grupo de recursos existente o cree un grupo de recursos nuevo en el cual colocar este perfil. Si decide crear un nuevo grupo de recursos, use la lista desplegable *Ubicación del grupo de recursos* para especificar la ubicación del grupo de recursos. Esta configuración hace referencia a la ubicación del grupo de recursos y no tiene efecto alguno sobre el perfil de Traffic Manager que se implementará globalmente. |

1. Seleccione **Crear** para implementar el perfil de Traffic Manager.

    :::image type="content" source="./media/traffic-manager-priority-routing-method/create-traffic-manager-profile-priority.png" alt-text="Creación de un perfil de prioridad de Traffic Manager":::

## <a name="add-endpoints"></a>Incorporación de puntos de conexión

1. Seleccione el perfil de Traffic Manager en la lista.

    :::image type="content" source="./media/traffic-manager-priority-routing-method/traffic-manager-profile-list.png" alt-text="Creación de un perfil de prioridad de Traffic Manager":::

1. Seleccione **Puntos de conexión** en *Configuración* y seleccione **+ Agregar** para agregar un nuevo punto de conexión.

    :::image type="content" source="./media/traffic-manager-priority-routing-method/traffic-manager-add-endpoints.png" alt-text="Creación de un perfil de prioridad de Traffic Manager":::

1. Seleccione o escriba los siguientes valores: 

    | Configuración                | Valor                                              |
    | ---                    | ---                                                |
    | Tipo                   | Seleccione el tipo de punto de conexión. |    
    | Nombre                   | Asigne un nombre para identificar este punto de conexión. |
    | Tipo de recurso de destino   | Seleccione el tipo de recurso para el destino. |
    | Recurso de destino        | Seleccione el recurso en la lista. |
    | Priority               | Asigne un número de prioridad a este punto de conexión. 1 es la prioridad más alta. |


1. Seleccione **Agregar** para agregar el punto de conexión. Repita los pasos 2 y 3 para agregar puntos de conexión adicionales. Recuerde establecer el número de prioridad adecuado.

    :::image type="content" source="./media/traffic-manager-priority-routing-method/add-endpoint.png" alt-text="Creación de un perfil de prioridad de Traffic Manager":::

1. En la página **Puntos de conexión** , revise el orden de prioridad de los puntos de conexión. Cuando selecciona el método de enrutamiento de tráfico de **Prioridad** , el orden de los extremos seleccionados es importante. Compruebe el orden de prioridad de los puntos de conexión.  El extremo principal se encuentra en primer lugar. Vuelva a comprobar el orden en que se muestra. Todas las solicitudes se enrutarán al primer punto de conexión y, si Traffic Manager detecta que no funciona correctamente, se realiza la conmutación por error del tráfico automáticamente al siguiente punto de conexión. 

    :::image type="content" source="./media/traffic-manager-priority-routing-method/endpoints-list.png" alt-text="Creación de un perfil de prioridad de Traffic Manager":::

1. Para cambiar el orden de prioridad del punto de conexión, seleccione el punto de conexión, cambie el valor de la prioridad y seleccione **Guardar** para guardar la configuración del punto de conexión.

## <a name="use-the-traffic-manager-profile"></a>Uso del perfil de Traffic Manager

1.  En la barra de búsqueda del portal, busque el nombre del **perfil de Traffic Manager** que creó en la sección anterior y seleccione el perfil de Traffic Manager en los resultados que aparecen.

    :::image type="content" source="./media/traffic-manager-priority-routing-method/search-traffic-manager-profile.png" alt-text="Creación de un perfil de prioridad de Traffic Manager":::

1.  La página de información general del **perfil de Traffic Manager** muestra el nombre DNS del perfil de Traffic Manager que acaba de crear. Cualquier cliente puede usar este perfil (por ejemplo, mediante un explorador web) para enrutar el tráfico al punto de conexión correcto según el tipo de enrutamiento. En este caso, todas las solicitudes se enrutan al primer punto de conexión y, si Traffic Manager detecta que no funciona correctamente, se realiza la conmutación por error del tráfico automáticamente al siguiente punto de conexión.

    :::image type="content" source="./media/traffic-manager-priority-routing-method/traffic-manager-profile-dns-name.png" alt-text="Creación de un perfil de prioridad de Traffic Manager":::

1. Una vez que el perfil de Traffic Manager esté en funcionamiento, edite el registro DNS en el servidor DNS relevante para redireccionar el nombre de dominio de la empresa al nombre de dominio del Administrador de tráfico.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ya no necesita el perfil de Traffic Manager, busque el perfil y seleccione **Eliminar perfil** .

:::image type="content" source="./media/traffic-manager-priority-routing-method/traffic-manager-delete-priority-profile.png" alt-text="Creación de un perfil de prioridad de Traffic Manager":::

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el método de enrutamiento prioritario, consulte:

> [!div class="nextstepaction"]
> [Método de enrutamiento prioritario](traffic-manager-routing-methods.md#priority-traffic-routing-method)
