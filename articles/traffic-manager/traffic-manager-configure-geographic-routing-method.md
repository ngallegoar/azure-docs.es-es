---
title: 'Tutorial: Configuración del enrutamiento geográfico del tráfico con Azure Traffic Manager'
description: En este tutorial se explica cómo configurar el método de enrutamiento geográfico del tráfico mediante Azure Traffic Manager
services: traffic-manager
author: duongau
manager: kumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/15/2020
ms.author: duau
ms.openlocfilehash: 29b3cdde328a994e5806df810db15b529a6da9af
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96188673"
---
# <a name="tutorial-configure-the-geographic-traffic-routing-method-using-traffic-manager"></a>Tutorial: Configuración del método de enrutamiento de tráfico geográfico con Traffic Manager

El método de enrutamiento de tráfico geográfico le permite dirigir el tráfico a puntos de conexión específicos según la ubicación geográfica en que se originan las solicitudes. En este tutorial se muestra cómo crear un perfil de Traffic Manager con este método de enrutamiento y configurar los puntos de conexión para recibir el tráfico proveniente de geografías específicas.

En este tutorial, obtendrá información sobre cómo:
> [!div class="checklist"]
> - Crear un perfil de Traffic Manager con enrutamiento geográfico.
> - Configurar un punto de conexión anidado.
> - Usar el perfil de Traffic Manager.
> - Eliminar el perfil de Traffic Manager.

## <a name="prerequisites"></a>Requisitos previos

* Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-a-traffic-manager-profile"></a>Creación de un perfil del Administrador de tráfico

1. En un explorador, inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **+ Crear un recurso** en el lado izquierdo. Busque **Perfil de Traffic Manager** y seleccione **Crear**.

    :::image type="content" source="./media/traffic-manager-geographic-routing-method/create-traffic-manager.png" alt-text="Crear un perfil de Traffic Manager":::

1. En la página *Crear perfil de Traffic Manager*, defina la siguiente configuración:

    | Configuración         | Value                                              |
    | ---             | ---                                                |
    | Nombre            | Proporcione un nombre para el perfil. Este nombre debe ser único dentro de la zona trafficmanager.net. Para acceder a su perfil de Traffic Manager, use el nombre DNS `<profilename>.trafficmanager.net`. |    
    | Método de enrutamiento  | Seleccione **Geográfico**. |
    | Subscription    | Seleccione su suscripción. |
    | Resource group   | Use un grupo de recursos existente o cree un grupo de recursos nuevo en el cual colocar este perfil. Si decide crear un nuevo grupo de recursos, use la lista desplegable *Ubicación del grupo de recursos* para especificar la ubicación del grupo de recursos. Esta configuración hace referencia a la ubicación del grupo de recursos y no tiene efecto alguno sobre el perfil de Traffic Manager que se implementará globalmente. |

1. Seleccione **Crear** para implementar el perfil de Traffic Manager.

    :::image type="content" source="./media/traffic-manager-geographic-routing-method/create-traffic-manager-profile.png" alt-text="Página de creación de un perfil de Traffic Manager":::

## <a name="add-endpoints"></a>Incorporación de puntos de conexión

1. Seleccione el perfil de Traffic Manager en la lista.

    :::image type="content" source="./media/traffic-manager-geographic-routing-method/traffic-manager-list-geographic.png" alt-text="Lista geográfica de Traffic Manager":::

1. Seleccione **Puntos de conexión** en *Configuración* y seleccione **+ Agregar** para agregar un nuevo punto de conexión.

    :::image type="content" source="./media/traffic-manager-geographic-routing-method/add-geographic-endpoint.png" alt-text="Incorporación de puntos de conexión":::

1. Seleccione o escriba los siguientes valores: 

    | Configuración                | Value                                              |
    | ---                    | ---                                                |
    | Tipo                   | Seleccione el tipo de punto de conexión. En el caso de los perfiles de enrutamiento geográfico que se usan en producción, se recomienda usar tipos de punto de conexión anidado que contengan un perfil secundario con más de un punto de conexión. Para más información, consulte [Preguntas frecuentes sobre los métodos de enrutamiento geográfico del tráfico](traffic-manager-FAQs.md). |    
    | Nombre                   | Asigne un nombre para identificar este punto de conexión. |
    | Tipo de recurso de destino   | Seleccione el tipo de recurso para el destino. |
    | Recurso de destino        | Seleccione el recurso en la lista. |

    > [!Note]
    > Algunos de los campos de esta página dependen del tipo de punto de conexión que agrega:
    > 1. Si agrega un punto de conexión de Azure, seleccione el **tipo de recurso de destino** y el **destino** según el recurso al que desea dirigir el tráfico.
    > 1. Si agrega un punto de conexión **externo**, proporcione el **nombre de dominio completo (FQDN)** del punto de conexión.
    > 1. Si agrega un **punto de conexión anidado**, seleccione el **recurso de destino** que corresponde al perfil secundario que desea usar y especifique el **recuento mínimo de puntos de conexión secundarios**.

1. En la sección *Asignación geográfica*, use la lista desplegable para agregar las regiones desde las que desea enviar el tráfico a este punto de conexión. Se debe agregar al menos una región. Puede tener varias regiones asignadas.

1. Repita el último paso para todos los puntos de conexión que desea agregar en este perfil y, a continuación, seleccione **Guardar**.

    :::image type="content" source="./media/traffic-manager-geographic-routing-method/traffic-manager-add-endpoint.png" alt-text="Incorporación de un punto de conexión de Traffic Manager":::

## <a name="use-the-traffic-manager-profile"></a>Uso del perfil de Traffic Manager

1.  En la barra de búsqueda del portal, busque el nombre del **perfil de Traffic Manager** que creó en la sección anterior y seleccione el perfil de Traffic Manager en los resultados que aparecen.
    
    :::image type="content" source="./media/traffic-manager-geographic-routing-method/search-traffic-manager-profile.png" alt-text="Búsqueda del perfil de Traffic Manager":::

1. La hoja **Perfil de Traffic Manager** muestra el nombre DNS del perfil de Traffic Manager que acaba de crear. Cualquier cliente puede usar este nombre (por ejemplo, mediante un explorador web) para enrutar el tráfico al punto de conexión correcto según el tipo de enrutamiento. Con el enrutamiento geográfico, Traffic Manager busca la dirección IP de origen de la solicitud entrante y determina la región desde la que se origina. Si esa región está asignada a un punto de conexión, el tráfico se enruta ahí. Si esta región no está asignada a un punto de conexión, Traffic Manager devuelve una respuesta de consulta NODATA.

    :::image type="content" source="./media/traffic-manager-geographic-routing-method/traffic-manager-geographic-overview.png" alt-text="Información general del enrutamiento geográfico de Traffic Manager":::

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ya no necesita el perfil de Traffic Manager, busque el perfil y seleccione **Eliminar perfil**.

:::image type="content" source="./media/traffic-manager-geographic-routing-method/delete-traffic-manager-profile.png" alt-text="Eliminar el perfil de Traffic Manager":::

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca del método de enrutamiento geográfico, consulte:

> [!div class="nextstepaction"]
> [Método de enrutamiento del tráfico geográfico](traffic-manager-routing-methods.md#geographic)
