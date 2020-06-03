---
title: Conexión privada a una aplicación web mediante un punto de conexión privado de Azure
description: Conexión privada a una aplicación web mediante el punto de conexión privado de Azure
author: ericgre
ms.assetid: 2dceac28-1ba6-4904-a15d-9e91d5ee162c
ms.topic: article
ms.date: 05/25/2020
ms.author: ericg
ms.service: app-service
ms.workload: web
ms.custom: fasttrack-edit
ms.openlocfilehash: 4c48a2fad927812cc45543243b48a2df81acf73b
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/26/2020
ms.locfileid: "83846960"
---
# <a name="using-private-endpoints-for-azure-web-app-preview"></a>Uso de puntos de conexión privados para una aplicación web de Azure (versión preliminar)

> [!Note]
> Con la actualización de versión preliminar, publicamos la característica de protección de filtración de datos.
> La versión preliminar está disponible en las regiones del este y oeste 2 de EE. UU. para todas las funciones PremiumV2 de Windows y Linux Web Apps y Elastic Premium. 

Puede usar el punto de conexión privado para la aplicación web de Azure a fin de permitir que los clientes ubicados en la red privada accedan de forma segura a la aplicación a través de un vínculo privado. El punto de conexión privado usa una dirección IP del espacio de direcciones de la red virtual de Azure. El tráfico de red entre un cliente en la red privada y la aplicación web atraviesa la red virtual y un servicio Private Link en la red troncal de Microsoft, lo que elimina la exposición desde la red pública de Internet.

El uso de un punto de conexión privado para la aplicación web le permite:

- Proteger la aplicación web mediante la configuración del punto de conexión privado, lo que elimina la exposición pública.
- Conectarse de forma segura a la aplicación web desde las redes locales que se conectan a la red virtual mediante VPN o el emparejamiento privado de ExpressRoute.
- Evite la filtración de datos desde la red virtual. 

Si solo necesita una conexión segura entre la red virtual y la aplicación web, los puntos de conexión de servicio son la solución más sencilla. Si también necesita acceso a la aplicación web desde el entorno local mediante una puerta de enlace de Azure, una red virtual emparejada de forma regional o globalmente, el punto de conexión privado es la solución.  

Para obtener más información, consulte [Puntos de conexión de servicio][serviceendpoint].

## <a name="conceptual-overview"></a>Información general conceptual

Un punto de conexión privado es una interfaz de red (NIC) especial para la aplicación web de Azure en una subred dentro de la red virtual (Vnet).
Cuando se crea un punto de conexión privado para la aplicación web, se proporciona una conectividad segura entre los clientes de la red privada y la aplicación web. Al punto de conexión privado se le asigna una dirección IP del intervalo de direcciones IP de la red virtual.
La conexión entre el punto de conexión privado y la aplicación web usa una instancia de [Private Link][privatelink] segura. El punto de conexión privado solo se usa para los flujos entrantes en la aplicación web. Los flujos salientes no usarán este punto de conexión privado, pero puede insertar flujos salientes en la red de una subred diferente mediante la [característica de integración de red virtual][vnetintegrationfeature].

La subred en la que se conecta el punto de conexión privado puede tener otros recursos, no necesita una subred vacía dedicada.
También puede implementar el punto de conexión privado en una región distinta a la de la aplicación web. 

> [!Note]
>La característica de integración de red virtual no puede usar la misma subred que el punto de conexión privado; esta es una limitación de la característica de integración de red virtual.

Desde la perspectiva de seguridad:

- Cuando se habilitan puntos de conexión privados a la aplicación web, se deshabilita todo el acceso público.
- Puede habilitar varios puntos de conexión privados en otras redes virtuales y subredes, incluidas las redes virtuales de otras regiones.
- La dirección IP de la NIC de punto de conexión privado debe ser dinámica, pero seguirá siendo la misma hasta que se elimine el punto de conexión privado.
- La NIC del punto de conexión privado no puede tener un grupo de seguridad de red asociado.
- La subred que hospeda el punto de conexión privado puede tener un grupo de seguridad de red asociado, pero debe deshabilitar la aplicación de directivas de red para el punto de conexión privado. Consulte [Deshabilitar las directivas de red de puntos de conexión privados][disablesecuritype]. Como resultado, no puede filtrar por ningún grupo de seguridad de red el acceso al punto de conexión privado.
- Al habilitar un punto de conexión privado en la aplicación web, no se evalúa la configuración de [restricciones de acceso ][accessrestrictions] de la aplicación web.
- Puede eliminar el riesgo de filtración de datos desde la red virtual mediante la eliminación de todas las reglas de grupo de seguridad de red en las que el destino es la etiqueta Internet o servicios de Azure. Al implementar un punto de conexión privado para una aplicación web, solo puede acceder a esta aplicación web específica a través del punto de conexión privado. Si tiene otra aplicación web, tendrá que implementar en ella otro punto de conexión privado dedicado.

En los registros HTTP web de la aplicación web, encontrará la dirección IP de origen del cliente. Se implementa mediante el protocolo proxy TCP, reenviando la propiedad de la dirección IP de cliente hasta la aplicación web. Para obtener más información, consulte [Cómo obtener información de conexión mediante el proxy TCP V2][tcpproxy].


  > [!div class="mx-imgBorder"]
  > ![Información general global del punto de conexión privado de aplicación web](media/private-endpoint/global-schema-web-app.png)

## <a name="dns"></a>DNS

Como esta característica está en la versión preliminar, durante la versión preliminar no se cambia la entrada DNS. Debe administrar la entrada DNS en el servidor DNS privado o en la zona privada de Azure DNS.
Si necesita usar un nombre DNS personalizado, debe agregarlo en la aplicación web. Durante la versión preliminar, el nombre personalizado se debe validar como cualquier otro mediante la resolución DNS pública. Para obtener más información, vea [Validación de un DNS personalizado][dnsvalidation].

Si necesita usar la consola de Kudu o la API REST de Kudu (implementación con agentes autohospedados de Azure DevOps, por ejemplo), tendrá que crear dos registros en la zona privada de Azure DNS o en el servidor DNS personalizado. 
- PrivateEndpointIP yourwebappname.azurewebsites.net 
- PrivateEndpointIP yourwebappname.scm.azurewebsites.net 

## <a name="pricing"></a>Precios

Para más información sobre los precios, consulte [Precios de Azure Private Link][pricing].

## <a name="limitations"></a>Limitaciones

Al usar Azure Functions en el plan Elástico Premium con un punto de conexión privado, para ejecutar la función en el portal web de Azure, debe tener acceso directo a la red o se producirá un error HTTP 403. En otras palabras, el explorador debe ser capaz de contactar con el punto de conexión privado para ejecutar la función desde el portal web de Azure. 

Durante la versión preliminar, la ranura de producción solo se expone detrás del punto de conexión privado; el punto de conexión público debe llegar a otras ranuras.

Estamos mejorando la característica Private Link y el punto de conexión privado periódicamente. Consulte [este artículo][pllimitations] para información actualizada sobre las limitaciones.

## <a name="next-steps"></a>Pasos siguientes

Para implementar un punto de conexión privado para la aplicación web mediante el portal, consulte [Cómo conectarse de forma privada a una aplicación web][howtoguide]




<!--Links-->
[serviceendpoint]: https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview
[privatelink]: https://docs.microsoft.com/azure/private-link/private-link-overview
[vnetintegrationfeature]: https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet
[disablesecuritype]: https://docs.microsoft.com/azure/private-link/disable-private-endpoint-network-policy
[accessrestrictions]: https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions
[tcpproxy]: ../../private-link/private-link-service-overview.md#getting-connection-information-using-tcp-proxy-v2
[dnsvalidation]: https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain
[pllimitations]: https://docs.microsoft.com/azure/private-link/private-endpoint-overview#limitations
[pricing]: https://azure.microsoft.com/pricing/details/private-link/
[howtoguide]: https://docs.microsoft.com/azure/private-link/create-private-endpoint-webapp-portal
