---
title: Configuración del control de acceso de red
titleSuffix: Azure SignalR Service
description: Configure el control de acceso de red para su instancia de Azure SignalR Service.
services: signalr
author: ArchangelSDY
ms.service: signalr
ms.topic: conceptual
ms.date: 05/06/2020
ms.author: dayshen
ms.openlocfilehash: 78b9859adbcf61ae3a9fc0d72a63d5973f90a487
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/02/2020
ms.locfileid: "84302167"
---
# <a name="configure-network-access-control"></a>Configuración del control de acceso de red

Azure SignalR Service le permite proteger y controlar el nivel de acceso al punto de conexión de su servicio en función del tipo de solicitud y del subconjunto de redes usadas. Cuando se configuran las reglas de red, solo las aplicaciones que solicitan datos del conjunto especificado de redes pueden acceder a su instancia de Azure SignalR Service.

Azure SignalR Service tiene un punto de conexión público accesible a través de Internet. También puede crear [puntos de conexión privados para Azure SignalR Service](howto-private-endpoints.md). El punto de conexión privado asigna una dirección IP privada de su red virtual a Azure SignalR Service y protege todo el tráfico entre la red virtual y Azure SignalR Service a través de un vínculo privado. El control de acceso de red de Azure SignalR Service permite controlar el acceso para los puntos de conexión públicos y privados.

Opcionalmente, puede permitir o denegar determinados tipos de solicitudes para los puntos de conexión públicos y para cada punto de conexión privado. Por ejemplo, puede bloquear todas las [conexiones con el servidor](signalr-concept-internals.md#server-connections) desde un punto de conexión público y asegurarse de que solo se originan desde una red virtual específica.

Una aplicación que accede a una instancia de Azure SignalR Service cuando las reglas de control de acceso de red están en vigor todavía requiere una autorización adecuada para la solicitud.

## <a name="scenario-a---no-public-traffic"></a>Escenario A: sin tráfico público

Para denegar completamente todo el tráfico público, primero debe configurar la regla de red pública para que no permita ningún tipo de solicitud. A continuación, debe configurar las reglas que conceden acceso al tráfico desde redes virtuales específicas. Esta configuración le permite crear un límite de red seguro para las aplicaciones.

## <a name="scenario-b---only-client-connections-from-public-network"></a>Escenario B: solo conexiones de cliente desde la red pública

En este escenario, puede configurar la regla de red pública para permitir solo [conexiones de cliente](signalr-concept-internals.md#client-connections) desde esta red. Después, puede configurar reglas de red privada para permitir otros tipos de solicitudes originadas en una red virtual específica. Esta configuración oculta sus servidores de aplicaciones de la red pública y establece conexiones seguras entre los servidores de aplicaciones y Azure SignalR Service.

## <a name="managing-network-access-control"></a>Administración del control de acceso de red

Puede administrar el control de acceso de red para Azure SignalR Service a través de Azure Portal.

### <a name="azure-portal"></a>Azure portal

1. Vaya a la instancia de Azure SignalR Service que quiere proteger.

1. Haga clic en el menú de configuración denominado **Control de acceso de red**.

    ![Lista de control de acceso de red en el portal](media/howto-network-access-control/portal.png)

1. Para editar la acción predeterminada, alterne el botón **Permitir/Denegar**.

    > [!TIP]
    > La acción predeterminada es la acción que se realiza cuando no hay ninguna coincidencia de regla de ACL. Por ejemplo, si la acción predeterminada es **Denegar**, se denegarán los tipos de solicitud que no se incluyan explícitamente en la lista blanca que aparece.

1. Para editar la regla de red pública, seleccione los tipos de solicitudes permitidos en **Red pública**.

    ![Edición de la lista de control de acceso de redes públicas en el portal ](media/howto-network-access-control/portal-public-network.png)

1. Para editar reglas de red de puntos de conexión privados, seleccione los tipos de solicitudes permitidos en cada una de las filas que aparecen en **Conexiones de punto de conexión privado**.

    ![Edición de la lista de control de acceso de puntos de conexión privados en el portal ](media/howto-network-access-control/portal-private-endpoint.png)

1. Haga clic en **Guardar** para aplicar los cambios.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre [Azure Private Link](/azure/private-link/private-link-overview).
