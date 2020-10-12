---
title: Administración del tráfico a aplicaciones multiinquilino mediante el portal
titleSuffix: Azure Application Gateway
description: En este artículo se proporciona una guía sobre cómo configurar aplicaciones web de servicio de App de Azure como miembros de un grupo de back-end en una puerta de enlace de aplicaciones nueva o existente.
services: application-gateway
author: surajmb
ms.service: application-gateway
ms.topic: how-to
ms.date: 09/23/2020
ms.author: victorh
ms.openlocfilehash: df92e08e91761d77c606ccb5389eee7dc219c101
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91323397"
---
# <a name="configure-app-service-with-application-gateway"></a>Configuración de App Service con Application Gateway

Dado que el servicio de aplicaciones es multiinquilino, en lugar de una implementación dedicada, usa el encabezado del host de la solicitud entrante para resolver la solicitud en el punto de conexión del servicio de aplicaciones correcto. Normalmente, el nombre DNS de la aplicación, que a su vez es el nombre DNS asociado con la puerta de enlace de aplicaciones principal del servicio de aplicaciones, no es el mismo que el nombre de dominio del servicio de aplicaciones back-end. Por lo tanto, el encabezado de host de la solicitud original recibido por la puerta de enlace de la aplicación no es el mismo que el nombre de host del servicio back-end. Por este motivo, a menos que el encabezado de host de la solicitud de la puerta de enlace de aplicaciones back-end, se cambia al nombre de host del servicio back-end, los back-end multiinquilino no pueden resolver la solicitud en el punto de conexión correcto.

Application Gateway proporciona un modificador llamado `Pick host name from backend target` que reemplaza el encabezado del host de la solicitud por el nombre de host del back-end cuando la solicitud se enruta de Application Gateway al back-end. Esta funcionalidad permite la compatibilidad con servidores back-end multiinquilino, como Azure App Service y API Management. 

En este artículo aprenderá a:

- Editar un grupo de back-end y agregarle un App Service
- Editar la configuración de HTTP con el conmutador "Seleccionar el nombre de host" habilitado

## <a name="prerequisites"></a>Requisitos previos

- Puerta de enlace de aplicaciones: cree una puerta de enlace de aplicaciones sin un destino de grupo de back-end. Para más información, consulte [Inicio rápido: Dirección del tráfico web con Azure Application Gateway: Azure Portal](quick-create-portal.md)

- Servicio de aplicaciones: Si no se tiene un servicio de aplicaciones existente, consulte la [documentación de servicio de aplicaciones](https://docs.microsoft.com/azure/app-service/).

## <a name="add-app-service-as-backend-pool"></a>Agregación de servicio de aplicaciones como grupo de back-end

1. En Azure Portal, seleccione el recurso de puerta de enlace de aplicación.

2. En **Grupos de back-end**, seleccione el grupo de back-end.

4. En **Tipo de destino**, seleccione **App Services**.

5. En **Destino** seleccione la instancia de App Service.

   :::image type="content" source="./media/configure-web-app-portal/backend-pool.png" alt-text="Back-end del servicio de aplicaciones":::
   
   > [!NOTE]
   > La lista desplegable solo rellenará los App Services que se encuentren en la misma suscripción que Application Gateway. Si quiere usar un servicio de aplicaciones que está en una suscripción diferente de aquella en la que se encuentra Application Gateway, en lugar de seleccionar **App Services** en la lista desplegable **Destinos**, elija la opción **Nombre de host o dirección IP** y escriba el nombre de host (example. azurewebsites.net) del servicio de aplicaciones.
1. Seleccione **Guardar**.

## <a name="edit-http-settings-for-app-service"></a>Edición de una configuración de HTTP para App Service

1. En **Configuración de HTTP**, seleccione la configuración de HTTP existente.

2. En **Reemplazar por un nuevo nombre de host**, seleccione **Sí**.
3. En **Reemplazo del nombre de host**, seleccione **Seleccionar el nombre de host del destino de back-end**.
4. Seleccione **Guardar**.

   :::image type="content" source="./media/configure-web-app-portal/http-settings.png" alt-text="Back-end del servicio de aplicaciones":::

## <a name="additional-configuration-in-case-of-redirection-to-app-services-relative-path"></a>Configuración adicional en caso de redireccionamiento a la ruta de acceso relativa del servicio de aplicaciones

Cuando App Service envía una respuesta de redireccionamiento al cliente para redirigir a su ruta de acceso relativa (por ejemplo, un redireccionamiento desde `contoso.azurewebsites.net/path1` a `contoso.azurewebsites.net/path2`), usa el mismo nombre de host en el encabezado de ubicación de su respuesta que el de la solicitud que recibió de la puerta de enlace de aplicación. Así pues, el cliente hará la solicitud directamente a `contoso.azurewebsites.net/path2`, en lugar de pasar por la puerta de enlace de aplicación (`contoso.com/path2`). No es conveniente omitir la puerta de enlace de aplicaciones.

Si en su caso de uso se encuentra con algunos escenarios en los que App Service tendrá que enviar una respuesta de redireccionamiento al cliente, realice los [pasos adicionales para reescribir el encabezado de ubicación](https://docs.microsoft.com/azure/application-gateway/troubleshoot-app-service-redirection-app-service-url#sample-configuration).

## <a name="restrict-access"></a>Restricción del acceso

Las aplicaciones web implementadas en estos ejemplos usan direcciones IP públicas a las que se puede acceder directamente desde Internet. Esto ayuda a solucionar los problemas cuando se está aprendiendo una nueva característica y se prueban cosas nuevas. Pero si va a implementar una característica en un entorno de producción, tendrá que agregar más restricciones.

Una manera de restringir el acceso a las aplicaciones web es usar [Restricciones de IP estáticas de Azure App Service](../app-service/app-service-ip-restrictions.md). Por ejemplo, puede restringir la aplicación web para que solo recibe tráfico desde la puerta de enlace de aplicaciones. Use la característica de restricción de IP de servicio de aplicación para mostrar la dirección IP virtual de la puerta de enlace de aplicaciones como la única dirección con el acceso.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el servicio de aplicaciones y otras compatibilidades multiinquilino con la puerta de enlace de aplicaciones, consulte la [compatibilidad del servicio multiinquilino con la puerta de enlace de aplicaciones](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview).
