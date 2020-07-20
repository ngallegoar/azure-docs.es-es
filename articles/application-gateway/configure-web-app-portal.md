---
title: Administración del tráfico a aplicaciones multiinquilino mediante el portal
titleSuffix: Azure Application Gateway
description: En este artículo se proporciona una guía sobre cómo configurar aplicaciones web de servicio de App de Azure como miembros de un grupo de back-end en una puerta de enlace de aplicaciones nueva o existente.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: how-to
ms.date: 06/09/2020
ms.author: absha
ms.openlocfilehash: 1109dae90790c9667b3c60afb6416c20061a95fe
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84808108"
---
# <a name="configure-app-service-with-application-gateway"></a>Configuración de App Service con Application Gateway

Dado que el servicio de aplicaciones es multiinquilino, en lugar de una implementación dedicada, usa el encabezado del host de la solicitud entrante para resolver la solicitud en el punto de conexión del servicio de aplicaciones correcto. Normalmente, el nombre DNS de la aplicación, que a su vez es el nombre DNS asociado con la puerta de enlace de aplicaciones principal del servicio de aplicaciones, no es el mismo que el nombre de dominio del servicio de aplicaciones back-end. Por lo tanto, el encabezado de host de la solicitud original recibido por la puerta de enlace de la aplicación no es el mismo que el nombre de host del servicio back-end. Por este motivo, a menos que el encabezado de host de la solicitud de la puerta de enlace de aplicaciones back-end, se cambia al nombre de host del servicio back-end, los back-end multiinquilino no pueden resolver la solicitud en el punto de conexión correcto.

Application Gateway proporciona un modificador llamado `Pick host name from backend address` que reemplaza el encabezado del host de la solicitud por el nombre de host del back-end cuando la solicitud se enruta de Application Gateway al back-end. Esta funcionalidad permite la compatibilidad con servidores back-end multiinquilino, como Azure App Service y API Management. 

En este artículo aprenderá a:

> [!div class="checklist"]
>
> - Crear un grupo de back-end y agregarle un App Service.
> - Crear una configuración HTTP y un sondeo personalizado con los modificadores "Seleccionar nombre de host" habilitados.

## <a name="prerequisites"></a>Requisitos previos

- Puerta de enlace de aplicaciones: Si no se dispone de una puerta de enlace de aplicaciones, consulte cómo [crear una puerta de enlace de aplicaciones](https://docs.microsoft.com/azure/application-gateway/quick-create-portal).
- Servicio de aplicaciones: Si no se tiene un servicio de aplicaciones existente, consulte la [documentación de servicio de aplicaciones](https://docs.microsoft.com/azure/app-service/).

## <a name="add-app-service-as-backend-pool"></a>Agregación de servicio de aplicaciones como grupo de back-end

1. En Azure Portal, abra la vista de configuración de la puerta de enlace de aplicaciones.

2. En **Grupos de back-end**, haga clic en **Agregar** para crear un nuevo grupo de back-end.

3. Proporcione un nombre adecuado para el grupo de back-end. 

4. En **Destinos**, haga clic en la lista desplegable y elija **App Services** como opción.

5. Aparecerá una lista desplegable con los App Services justo debajo de la lista desplegable **Destinos**. En esta lista desplegable, elija el App Service que quiera agregar como miembro del grupo de back-end y haga clic en Agregar.

   ![Back-end del servicio de aplicaciones](./media/configure-web-app-portal/backendpool.png)
   
   > [!NOTE]
   > La lista desplegable solo rellenará los servicios de aplicaciones que se encuentren en la misma suscripción que Application Gateway. Si quiere usar un servicio de aplicaciones que está en una suscripción diferente de aquella en la que se encuentra Application Gateway, en lugar de seleccionar **App Services** en la lista desplegable **Destinos**, elija la opción **Nombre de host o dirección IP** y escriba el nombre de host (example. azurewebsites.net) del servicio de aplicaciones.

## <a name="create-http-settings-for-app-service"></a>Creación de una configuración HTTP para el servicio de aplicaciones

1. En **Configuración HTTP**, haga clic en **Agregar** para crear una nueva configuración HTTP.

2. Especifique un nombre para la configuración HTTP y podrá habilitar o deshabilitar la Afinidad basada en cookies según sus requisitos.

3. Elija el protocolo como HTTP o HTTPS según el caso de uso. 

   > [!NOTE]
   > Si selecciona HTTPS, no es necesario cargar ningún certificado de autenticación ni certificado raíz de confianza para permitir el back-end de App Service, ya que App Service es un servicio de Azure de confianza.

4. Active la casilla **Uso para App Service**. Tenga en cuenta que los modificadores `Create a probe with pick host name from backend address` y `Pick host name from backend address` se habilitarán automáticamente. `Pick host name from backend address` reemplazará el encabezado del host de la solicitud por el nombre de host del back-end cuando la solicitud se enrute de Application Gateway al back-end.  

   `Create a probe with pick host name from backend address` creará automáticamente un sondeo de mantenimiento y lo asociará a esta configuración de HTTP. No es necesario crear ningún otro sondeo de mantenimiento para esta configuración de HTTP. Puede comprobar que se ha agregado un nuevo sondeo con el nombre <HTTP Setting name><Unique GUID> en la lista de sondeos de mantenimiento y que ya tiene el modificador `Pick host name from backend http settings enabled`.

   Si ya tiene una o más configuraciones de HTTP que se usan para App Service y si dichas configuraciones emplean el mismo protocolo que el que usted usa en la que está creando, en lugar del modificador `Create a probe with pick host name from backend address`, obtendrá una lista desplegable para seleccionar uno de los sondeos personalizados. Esto se debe a que, como ya existe una configuración HTTP con el servicio de aplicaciones, también existirá un sondeo de mantenimiento que tenga el modificador `Pick host name from backend http settings enabled`. Seleccione ese sondeo personalizado en la lista desplegable.

5. Haga clic en **Aceptar** para crear la configuración HTTP.

   ![HTTP-setting1](./media/configure-web-app-portal/http-setting1.png)

   ![HTTP-setting2](./media/configure-web-app-portal/http-setting2.png)



## <a name="create-rule-to-tie-the-listener-backend-pool-and-http-setting"></a>Creación de reglas para enlazar el cliente de escucha, el grupo de back-end y la configuración HTTP

1. En **Reglas**, haga clic en **Básicas** para crear una regla básica nueva.

2. Proporcione un nombre adecuado y seleccione el cliente de escucha que va a aceptar las solicitudes entrantes para el servicio de aplicaciones.

3. En la lista desplegable **Grupo de back-end**, elija el grupo de back-end que se ha creado anteriormente.

4. En la lista desplegable **Configuración HTTP**, elija la configuración HTTP que se ha creado anteriormente.

5. Haga clic en **Aceptar** para guardar esta regla.

   ![Regla](./media/configure-web-app-portal/rule.png)

## <a name="additional-configuration-in-case-of-redirection-to-app-services-relative-path"></a>Configuración adicional en caso de redireccionamiento a la ruta de acceso relativa del servicio de aplicaciones

Cuando el servicio de aplicaciones envía una respuesta de redireccionamiento al cliente para redirigir a su ruta de acceso relativa (por ejemplo, un redireccionamiento de contoso.azurewebsites.net/path1 a contoso.azurewebsites.net/path2), usa el mismo nombre de host en el encabezado de ubicación de su respuesta que el de la solicitud que recibió de la puerta de enlace de aplicaciones. Así pues, el cliente hará la solicitud directamente a contoso.azurewebsites.net/path2, en lugar de pasar por la puerta de enlace de aplicaciones (contoso.com/path2). No es conveniente omitir la puerta de enlace de aplicaciones.

Si en su caso de uso se encuentra con algunos escenarios en los que App Service tendrá que enviar una respuesta de redireccionamiento al cliente, realice los [pasos adicionales para reescribir el encabezado de ubicación](https://docs.microsoft.com/azure/application-gateway/troubleshoot-app-service-redirection-app-service-url#sample-configuration).

## <a name="restrict-access"></a>Restricción del acceso

Las aplicaciones web implementadas en estos ejemplos usan direcciones IP públicas a las que se puede acceder directamente desde Internet. Esto ayuda a solucionar los problemas cuando se está aprendiendo una nueva característica y se prueban cosas nuevas. Pero si va a implementar una característica en un entorno de producción, tendrá que agregar más restricciones.

Una manera de restringir el acceso a las aplicaciones web es usar [Restricciones de IP estáticas de Azure App Service](../app-service/app-service-ip-restrictions.md). Por ejemplo, puede restringir la aplicación web para que solo recibe tráfico desde la puerta de enlace de aplicaciones. Use la característica de restricción de IP de servicio de aplicación para mostrar la dirección IP virtual de la puerta de enlace de aplicaciones como la única dirección con el acceso.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el servicio de aplicaciones y otras compatibilidades multiinquilino con la puerta de enlace de aplicaciones, consulte la [compatibilidad del servicio multiinquilino con la puerta de enlace de aplicaciones](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview).
