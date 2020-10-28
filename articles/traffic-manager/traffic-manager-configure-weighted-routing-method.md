---
title: 'Tutorial: Configuración del enrutamiento de tráfico round robin ponderado con Azure Traffic Manager'
description: En este tutorial se explica cómo equilibrar la carga del tráfico mediante un método round robin en Traffic Manager
services: traffic-manager
documentationcenter: ''
author: duongau
ms.service: traffic-manager
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/19/2020
ms.author: duau
ms.openlocfilehash: abcfce43b90c7371d5b38aa5b7a6d478e9d6a0dd
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92207846"
---
# <a name="tutorial-configure-the-weighted-traffic-routing-method-in-traffic-manager"></a>Tutorial: Configuración del método de enrutamiento de tráfico ponderado en Traffic Manager

Un patrón del método de enrutamiento del tráfico es proporcionar un conjunto de extremos idénticos, que incluye servicios en la nube y sitios web, y enviar tráfico a cada uno de ellos de forma equitativa. Los siguientes pasos describen cómo configurar este tipo de método de enrutamiento de tráfico.

> [!NOTE]
> Azure Web App ya le proporciona la funcionalidad de equilibrio de carga Round Robin para sitios web en una región de Azure (que puede incluir varios centros de datos). Traffic Manager permite distribuir el tráfico entre los sitios web en distintos centros de datos.

En este tutorial, obtendrá información sobre cómo:
> [!div class="checklist"]
> - Crear un perfil de Traffic Manager con enrutamiento ponderado.
> - Usar el perfil de Traffic Manager.
> - Eliminar el perfil de Traffic Manager.

## <a name="prerequisites"></a>Requisitos previos

* Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/).

## <a name="configure-the-weighted-traffic-routing-method"></a>Configuración del método de enrutamiento de tráfico ponderado

1. En un explorador, inicie sesión en [Azure Portal](https://portal.azure.com).

1. En la barra de búsqueda del portal, busque el nombre del **perfil de Traffic Manager** que creó en la sección anterior y seleccione el perfil de Traffic Manager en los resultados que aparecen.

    :::image type="content" source="./media/traffic-manager-weighted-routing-method/search-traffic-manager-weighted-profile.png" alt-text="Búsqueda del perfil de Traffic Manager&quot;:::

1. Seleccione **Configuración** y seleccione o escriba los siguientes valores:

    | Configuración         | Valor                                              |
    | ---             | ---                                                |
    | Método de enrutamiento            | Seleccione **Ponderado** . |    
    | Período de vida de DNS (TTL) | Este valor controla la frecuencia con la que el servidor de nombres de almacenamiento en caché local del cliente consultará en el sistema de Traffic Manager las entradas DNS actualizadas. Cualquier cambio que se produzca con Traffic Manager, como cambios en el método de enrutamiento del tráfico o en la disponibilidad de los puntos de conexión agregados, tardará este tiempo en actualizarse en todo el sistema global de servidores DNS. |
    | Protocolo    | Seleccione un protocolo para la supervisión del punto de conexión. *Opciones: HTTP, HTTPS y TCP* |
    | Port | Especifique el número de puerto. |
    | Ruta de acceso | Para supervisar los puntos de conexión, debe especificar una ruta de acceso y un nombre de archivo. Una barra diagonal &quot;/" es una entrada válida para la ruta de acceso relativa e implica que el archivo se encuentra en el directorio raíz (valor predeterminado). |
    | Configuración de encabezado personalizado | Configure los encabezados personalizados en formato de host: contoso.com,nuevoEncabezado:nuevoValor. Se admite un máximo de 8 pares. Aplicable para los protocolos HTTP y HTTPS. Aplicable para todos los puntos de conexión del perfil. |
    | Intervalos de códigos de estado esperados (valor predeterminado: 200) | Configure los intervalos de código de estado con el formato 200-299,301-301. Se admite un máximo de 8 intervalos. Aplicable para los protocolos HTTP y HTTPS. Aplicable para todos los puntos de conexión del perfil. |
    | Intervalo de sondeo | Configure el intervalo de tiempo entre los sondeos de estado del punto de conexión. Puede elegir 10 o 30 segundos. |
    | Número tolerado de errores | Configure el número de errores de sondeo de estado tolerados antes de que se desencadene un error de punto de conexión. Puede escribir un número entre 0 y 9. | 
    | Tiempo de expiración del sondeo | Configure el tiempo necesario antes de que se agote el tiempo de espera de un sondeo de estado de punto de conexión. Este valor debe ser al menos 5 y menor que el valor del intervalo de sondeo. |

1. Seleccione **Guardar** para completar la configuración.

    :::image type="content" source="./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-configuration.png" alt-text="Búsqueda del perfil de Traffic Manager&quot;:::

1. Seleccione **Configuración** y seleccione o escriba los siguientes valores:

    | Configuración         | Valor                                              |
    | ---             | ---                                                |
    | Método de enrutamiento            | Seleccione **Ponderado** . |    
    | Período de vida de DNS (TTL) | Este valor controla la frecuencia con la que el servidor de nombres de almacenamiento en caché local del cliente consultará en el sistema de Traffic Manager las entradas DNS actualizadas. Cualquier cambio que se produzca con Traffic Manager, como cambios en el método de enrutamiento del tráfico o en la disponibilidad de los puntos de conexión agregados, tardará este tiempo en actualizarse en todo el sistema global de servidores DNS. |
    | Protocolo    | Seleccione un protocolo para la supervisión del punto de conexión. *Opciones: HTTP, HTTPS y TCP* |
    | Port | Especifique el número de puerto. |
    | Ruta de acceso | Para supervisar los puntos de conexión, debe especificar una ruta de acceso y un nombre de archivo. Una barra diagonal &quot;/"::: 

1. Seleccione **Punto de conexión** y configure la ponderación de cada punto de conexión. La ponderación puede estar entre 1 y 1000. Cuanto mayor sea la ponderación, mayor será la prioridad.  

    :::image type="content" source="./media/traffic-manager-weighted-routing-method/traffic-manager-configure-endpoints-weighted.png" alt-text="Búsqueda del perfil de Traffic Manager&quot;:::

1. Seleccione **Configuración** y seleccione o escriba los siguientes valores:

    | Configuración         | Valor                                              |
    | ---             | ---                                                |
    | Método de enrutamiento            | Seleccione **Ponderado** . |    
    | Período de vida de DNS (TTL) | Este valor controla la frecuencia con la que el servidor de nombres de almacenamiento en caché local del cliente consultará en el sistema de Traffic Manager las entradas DNS actualizadas. Cualquier cambio que se produzca con Traffic Manager, como cambios en el método de enrutamiento del tráfico o en la disponibilidad de los puntos de conexión agregados, tardará este tiempo en actualizarse en todo el sistema global de servidores DNS. |
    | Protocolo    | Seleccione un protocolo para la supervisión del punto de conexión. *Opciones: HTTP, HTTPS y TCP* |
    | Port | Especifique el número de puerto. |
    | Ruta de acceso | Para supervisar los puntos de conexión, debe especificar una ruta de acceso y un nombre de archivo. Una barra diagonal &quot;/"::: 

## <a name="use-the-traffic-manager-profile"></a>Uso del perfil de Traffic Manager

La hoja **Perfil de Traffic Manager** muestra el nombre DNS del perfil de Traffic Manager que acaba de crear. Cualquier cliente puede usar este nombre (por ejemplo, mediante un explorador web) para enrutar el tráfico al punto de conexión correcto según el tipo de enrutamiento. En este caso, todas las solicitudes se enrutan a cada punto de conexión según el método round-robin.

:::image type="content" source="./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-overview.png" alt-text="Búsqueda del perfil de Traffic Manager&quot;:::

1. Seleccione **Configuración** y seleccione o escriba los siguientes valores:

    | Configuración         | Valor                                              |
    | ---             | ---                                                |
    | Método de enrutamiento            | Seleccione **Ponderado** . |    
    | Período de vida de DNS (TTL) | Este valor controla la frecuencia con la que el servidor de nombres de almacenamiento en caché local del cliente consultará en el sistema de Traffic Manager las entradas DNS actualizadas. Cualquier cambio que se produzca con Traffic Manager, como cambios en el método de enrutamiento del tráfico o en la disponibilidad de los puntos de conexión agregados, tardará este tiempo en actualizarse en todo el sistema global de servidores DNS. |
    | Protocolo    | Seleccione un protocolo para la supervisión del punto de conexión. *Opciones: HTTP, HTTPS y TCP* |
    | Port | Especifique el número de puerto. |
    | Ruta de acceso | Para supervisar los puntos de conexión, debe especificar una ruta de acceso y un nombre de archivo. Una barra diagonal &quot;/"::: 

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ya no necesita el perfil de Traffic Manager, busque el perfil y seleccione **Eliminar perfil** .

:::image type="content" source="./media/traffic-manager-weighted-routing-method/delete-traffic-manager-weighted-profile.png" alt-text="Búsqueda del perfil de Traffic Manager&quot;:::

1. Seleccione **Configuración** y seleccione o escriba los siguientes valores:

    | Configuración         | Valor                                              |
    | ---             | ---                                                |
    | Método de enrutamiento            | Seleccione **Ponderado** . |    
    | Período de vida de DNS (TTL) | Este valor controla la frecuencia con la que el servidor de nombres de almacenamiento en caché local del cliente consultará en el sistema de Traffic Manager las entradas DNS actualizadas. Cualquier cambio que se produzca con Traffic Manager, como cambios en el método de enrutamiento del tráfico o en la disponibilidad de los puntos de conexión agregados, tardará este tiempo en actualizarse en todo el sistema global de servidores DNS. |
    | Protocolo    | Seleccione un protocolo para la supervisión del punto de conexión. *Opciones: HTTP, HTTPS y TCP* |
    | Port | Especifique el número de puerto. |
    | Ruta de acceso | Para supervisar los puntos de conexión, debe especificar una ruta de acceso y un nombre de archivo. Una barra diagonal &quot;/":::

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el método de enrutamiento ponderado, consulte:

> [!div class="nextstepaction"]
> [Método ponderado de enrutamiento de tráfico](traffic-manager-routing-methods.md#weighted)