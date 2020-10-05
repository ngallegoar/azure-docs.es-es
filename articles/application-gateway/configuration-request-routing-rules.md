---
title: Configuración de reglas de enrutamiento de solicitudes de Azure Application Gateway
description: En este artículo se describe cómo configurar las reglas de enrutamiento de solicitudes de Azure Application Gateway.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 09/09/2020
ms.author: surmb
ms.openlocfilehash: 20a665eefbb73f062f1f036e17b16da891a43eef
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89652738"
---
# <a name="application-gateway-request-routing-rules"></a>Reglas de enrutamiento de solicitudes de Application Gateway

Si crea una puerta de enlace de aplicaciones mediante Azure Portal, creará una regla predeterminada (*rule1*). Esta regla enlaza el cliente de escucha predeterminado (*appGatewayHttpListener*) con el grupo de servidores back-end predeterminado (*appGatewayBackendPool*) y la configuración de HTTP de back-end predeterminada (*appGatewayBackendHttpSettings*). Después de crear la puerta de enlace, puede modificar la configuración de la regla predeterminada o crear nuevas reglas.

## <a name="rule-type"></a>Tipo de regla

Cuando se crea una regla, puede elegir entre [*básica* y *basada en ruta de acceso*](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#request-routing-rules).

- Elija básica si desea reenviar todas las solicitudes del cliente de escucha asociado (por ejemplo, *blog<i></i>.contoso.com/\*)* a un único grupo de servidores back-end.
- Elija basada en ruta de acceso si desea enrutar las solicitudes desde determinadas rutas de acceso URL a grupos de servidores back-end específicos. El patrón basado en rutas de acceso se aplica solo a la ruta de acceso de la dirección URL, no a sus parámetros de consulta.

### <a name="order-of-processing-rules"></a>Orden de procesamiento de las reglas

Para las SKU v1 y v2, la coincidencia de patrones de las solicitudes entrantes se procesa en el orden en que se enumeran las rutas en la asignación de rutas de acceso de la dirección URL de la regla basada en ruta de acceso. Si una solicitud coincide con el patrón en dos o más rutas de la asignación de rutas, se selecciona la primera ruta de la lista. Y la solicitud se reenvía al back-end que está asociado con esa ruta de acceso.

## <a name="associated-listener"></a>Cliente de escucha asociado

Asocie un cliente de escucha a la regla para que la *regla de enrutamiento de solicitudes* que está asociada con él realice una evaluación para determinar el grupo de servidores back-end al que enrutar la solicitud.

## <a name="associated-back-end-pool"></a>Grupo de servidores back-end asociado

Asocie la regla al grupo de servidores back-end que contiene los back-end de destino que se encargan de las solicitudes que recibe el cliente de escucha.

 - En una regla básica, se permite un único grupo de servidores back-end. Todas las solicitudes del cliente de escucha asociado se reenviarán a ese grupo de servidores back-end.

 - En el caso de una regla basada en ruta de acceso, agregue varios grupos de servidores back-end que se correspondan con cada ruta de acceso URL. Aquellas solicitudes que coincidan con la ruta de acceso especificada se reenviarán al grupo de servidores back-end correspondiente. Agregue también un grupo de servidores back-end predeterminado. Las solicitudes que no coincidan con ninguna ruta de acceso URL de la regla se reenviarán a ese grupo.

## <a name="associated-back-end-http-setting"></a>Configuración de HTTP del back-end asociado

Agregue una configuración de HTTP de back-end para cada regla. Las solicitudes se enrutarán desde la puerta de enlace de aplicaciones a los back-end de destino mediante el número de puerto, el protocolo y cualquier otra información que se especifique en esta configuración.

En una regla básica, se permite solo una configuración de HTTP de back-end. Todas las solicitudes del cliente de escucha asociado se reenviarán a los back-end de destino correspondientes mediante esta configuración de HTTP.

En el caso de una regla basada en ruta de acceso, agregue varias configuraciones HTTP de back-end que se correspondan con cada ruta de acceso URL. Las solicitudes que coincidan con la ruta de acceso URL de esta configuración se reenviarán a los back-end de destino correspondientes mediante la configuración de HTTP que se corresponda con cada ruta de acceso URL. Agregue también una configuración de HTTP predeterminada. Las solicitudes que no coincidan con ninguna ruta de acceso URL de esta regla se reenviarán al grupo de servidores back-end predeterminado mediante la configuración de HTTP predeterminada.

## <a name="redirection-setting"></a>Configuración de redireccionamiento

Si se configura el redireccionamiento para una regla básica, todas las solicitudes del cliente de escucha asociado se reenviarán al destino. Esto es un redireccionamiento *global*. Si se configura el redireccionamiento para una regla basada en ruta de acceso, solo se redirigirán aquellas solicitudes de una determinada área del sitio. Un ejemplo es un área de carro de la compra que se indica mediante */cart/\** . Esto es un redireccionamiento *basado en ruta de acceso*.

Para más información acerca de los redireccionamientos, consulte [Introducción a la redirección de Application Gateway](redirect-overview.md).

### <a name="redirection-type"></a>Tipo de redireccionamiento

Elija el tipo de redireccionamiento necesario: *Permanente (301)* , *Temporal (307)* , *Encontrado (302)* o *Ver otro (303)* .

### <a name="redirection-target"></a>Destino del redireccionamiento

Elija otro cliente de escucha o un sitio externo como destino del redireccionamiento.

#### <a name="listener"></a>Agente de escucha

Elige el cliente de escucha como destino de redireccionamiento para redirigir el tráfico desde un cliente de escucha a otro en la puerta de enlace. Esta configuración es necesaria si se desea habilitar el redireccionamiento de HTTP a HTTPS. Redirige el tráfico desde el cliente de escucha de origen que comprueba las solicitudes HTTP entrantes al cliente de escucha de destino que comprueba las solicitudes HTTPS entrantes. También puede decidir incluir la cadena de consulta y la ruta de acceso de la solicitud original en la solicitud que se reenviará al destino de redireccionamiento.

![Cuadro de diálogo de componentes de Application Gateway](./media/configuration-overview/configure-redirection.png)

Para más información sobre el redireccionamiento de HTTP a HTTPS, consulte:
- [HTTP-to-HTTPS redirection by using the Azure portal](redirect-http-to-https-portal.md) (Redireccionamiento de HTTP a HTTPS mediante Azure Portal)
- [HTTP-to-HTTPS redirection by using PowerShell](redirect-http-to-https-powershell.md) (Redireccionamiento de HTTP a HTTPS mediante PowerShell)
- [HTTP-to-HTTPS redirection by using the Azure CLI](redirect-http-to-https-cli.md) (Redireccionamiento de HTTP a HTTPS mediante la CLI de Azure)

#### <a name="external-site"></a>Sitio externo

Elija un sitio externo cuando desee redirigir el tráfico del cliente de escucha asociado con esta regla a un sitio externo. Puede decidir incluir la cadena de consulta de la solicitud original en la solicitud que se reenviará al destino de redireccionamiento. No se puede reenviar la ruta de acceso al sitio externo que se encontraba en la solicitud original.

Para más información sobre el redireccionamiento, consulte:
- [Redirect traffic to an external site by using PowerShell](redirect-external-site-powershell.md) (Redireccionamiento del tráfico a un sitio externo mediante PowerShell)
- [Redirect traffic to an external site by using the CLI](redirect-external-site-cli.md) (Redireccionamiento del tráfico a un sitio externo mediante la CLI)

## <a name="rewrite-http-headers-and-url"></a>Reescritura de encabezados HTTP y URL

Mediante las reglas de reescritura, puede agregar, quitar o actualizar encabezados de solicitud y respuesta HTTP(S), así como parámetros de ruta de acceso URL y cadena de consulta, dado que los paquetes de solicitud y respuesta se mueven entre el cliente y los grupos back-end a través de la puerta de enlace de aplicaciones.

Los parámetros de URL y encabezado se pueden establecer en valores estáticos o en otros encabezados y variables de servidor. Como consecuencia, sirve de ayuda en casos de uso importantes, como la extracción de direcciones IP de cliente, la eliminación de información confidencial sobre el back-end, la adición de más seguridad, etc.
Para más información, consulte:

 - [Información general sobre la reescritura de encabezados HTTP y URL](rewrite-http-headers-url.md)
 - [Configuración de la reescritura de encabezados HTTP](rewrite-http-headers-portal.md)
 - [Configuración de la reescritura de URL](rewrite-url-portal.md)

## <a name="next-steps"></a>Pasos siguientes

- [Más información sobre la configuración de HTTP](configuration-http-settings.md)