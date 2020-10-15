---
title: 'Preguntas frecuentes: Azure DNS'
description: En este artículo se tratan las preguntas frecuentes sobre Azure DNS.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 6/15/2019
ms.author: rohink
ms.openlocfilehash: 76b19cfb3c00a26d81eab81f67d8e156a520f377
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "77121723"
---
# <a name="azure-dns-faq"></a>Preguntas más frecuentes sobre DNS de Azure

## <a name="about-azure-dns"></a>Más información sobre DNS de Azure

### <a name="what-is-azure-dns"></a>¿Qué es Azure DNS?

El Sistema de nombres de dominio (DNS) traduce, o resuelve, el nombre de un sitio web o servicio a su dirección IP. Azure DNS es un servicio de hospedaje para dominios DNS. Proporciona resolución de nombres mediante el uso de la infraestructura de Microsoft Azure. Al hospedar dominios en Azure, puede administrar los registros de DNS con las mismas credenciales, API, herramientas y facturación que con los demás servicios de Azure.

Los dominios DNS de Azure DNS se hospedan en la red global de servidores de nombres DNS de Azure. Este sistema usa redes de difusión por proximidad, con el fin de que cada consulta de DNS la responda el servidor DNS disponible más cercano disponible. Azure DNS ofrece un rendimiento rápido y alta disponibilidad para su dominio.

Azure DNS se basa en Azure Resource Manager. Azure DNS se beneficia de características de Resource Manager, como control de acceso basado en rol, registros de auditoría y bloqueo de recursos. Puede administrar los dominios y registros a través de Azure Portal, cmdlets de Azure PowerShell y la CLI de Azure multiplataforma. Las aplicaciones que requieren la administración automática de DNS pueden integrarse con el servicio a través de los SDK y API REST.

### <a name="how-much-does-azure-dns-cost"></a>¿Cuánto cuesta DNS de Azure?

El modelo de facturación de Azure DNS se basa en el número de zonas DNS hospedadas en Azure DNS. También se basa en el número de consultas de DNS que se reciben. Los descuentos se proporcionan en función del uso.

Para más información, vea la [página de precios de DNS de Azure](https://azure.microsoft.com/pricing/details/dns/).

### <a name="what-is-the-sla-for-azure-dns"></a>¿Qué es el Acuerdo de Nivel de Servicio de DNS de Azure?

Azure garantiza que las solicitudes de DNS válidas reciben respuesta de al menos un servidor de nombres de Azure DNS el 100 % del tiempo.

Para más información, vea la [página del SLA de DNS de Azure](https://azure.microsoft.com/support/legal/sla/dns).

### <a name="what-is-a-dns-zone-is-it-the-same-as-a-dns-domain"></a>¿Qué es una zona DNS? ¿Es lo mismo que un dominio DNS? 

Un dominio es un nombre único en el sistema de nombres de dominio. Un ejemplo es contoso.com.

Una zona DNS se usa para hospedar los registros DNS de un dominio concreto. Por ejemplo, el dominio contoso.com puede contener varios registros de DNS. Los registros pueden incluir mail.contoso.com para un servidor de correo y www\.contoso.com para un sitio web. Estos registros se hospedan en la zona DNS contoso.com.

Un nombre de dominio es *solo un nombre*. Una zona DNS es un recurso de datos que contiene los registros de DNS para un nombre de dominio. Puede usar Azure DNS para hospedar una zona DNS y administrar los registros DNS de un dominio en Azure. También proporciona servidores de nombres DNS para responder a consultas de DNS de Internet.

### <a name="do-i-need-to-buy-a-dns-domain-name-to-use-azure-dns"></a>¿Tengo que adquirir un nombre de dominio DNS para usar Azure DNS? 

No necesariamente.

No es necesario comprar un dominio para hospedar una zona DNS en Azure DNS. Se puede crear una zona DNS en cualquier momento sin necesidad de poseer el nombre del dominio. Las consultas de DNS de esta zona solo se resuelven si se dirigen a los servidores de nombres de Azure DNS asignados a la zona.

Para vincular la zona DNS en la jerarquía DNS global, debe comprar el nombre de dominio. Luego, las consultas de DNS desde cualquier lugar del mundo encuentran su zona DNS y responden con sus registros de DNS.

## <a name="azure-dns-features"></a>Características de Azure DNS

### <a name="are-there-any-restrictions-when-using-alias-records-for-a-domain-name-apex-with-traffic-manager"></a>¿Hay alguna restricción en el uso de registros de alias para un vértice de nombre de dominio con Traffic Manager?

Sí. Debe usar direcciones IP públicas estáticas con Azure Traffic Manager. Configure el **Punto de conexión externo** de destino mediante una dirección IP estática. 

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>¿DNS de Azure admite la conmutación por error de puntos de conexión o el enrutamiento basados en DNS?

Traffic Manager proporciona las características de conmutación por error de puntos de conexión y el enrutamiento de tráfico basados en DNS. Traffic Manager es un servicio de Azure independiente que puede utilizarse junto con Azure DNS. Para más información, vea la [Introducción a Traffic Manager](../traffic-manager/traffic-manager-overview.md).

Azure DNS solo admite hospedar dominios DNS estáticos, donde cada consulta de DNS de un registro de DNS determinado siempre recibe la misma respuesta DNS.

### <a name="does-azure-dns-support-domain-name-registration"></a>¿DNS de Azure admite el registro de nombres de dominio?

No. Azure DNS no admite actualmente la opción de comprar nombres de dominio. Para comprar dominios, debe usar un registrador de nombres de dominio de terceros. El registrador suele cobrar una tarifa anual reducida. Los dominios se pueden hospedar en Azure DNS para la administración de registros de DNS. Para más información, vea [Delegación de un dominio en DNS de Azure](dns-domain-delegation.md).

Se hace seguimiento de la característica para comprar nombres de dominio en el trabajo pendiente de Azure. Use el sitio de comentarios para [registrar su compatibilidad con esta característica](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar).

### <a name="does-azure-dns-support-dnssec"></a>¿DNS de Azure admite DNSSEC?

No. Azure DNS no admite actualmente las extensiones de seguridad del Sistema de nombres de dominio (DNSSEC).

Se hace seguimiento de la característica DNSSEC en el trabajo pendiente de Azure DNS. Use el sitio de comentarios para [registrar su compatibilidad con esta característica](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support).

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>¿DNS de Azure admite las transferencias de zona (AXFR/IXFR)?

No. Azure DNS no admite actualmente las transferencias de zona. Las zonas DNS se pueden [importar a Azure DNS mediante la CLI de Azure](dns-import-export.md). Los registros DNS se administran a través del [Portal de administración de Azure DNS](dns-operations-recordsets-portal.md), la [API REST](https://docs.microsoft.com/powershell/module/az.dns), el [SDK](dns-sdk.md), los [cmdlets de PowerShell](dns-operations-recordsets.md) o la [herramienta CLI](dns-operations-recordsets-cli.md).

Se hace seguimiento de la característica de transferencia de zona en el trabajo pendiente de Azure DNS. Use el sitio de comentarios para [registrar su compatibilidad con esta característica](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c).

### <a name="does-azure-dns-support-url-redirects"></a>¿DNS de Azure admite los redireccionamientos de dirección URL?

No. Los servicios de redireccionamiento de dirección URL no son un servicio DNS. Funcionan en el nivel HTTP en lugar del nivel DNS. Algunos proveedores DNS incluyen un servicio de redireccionamiento de dirección URL como parte de su oferta general. Este servicio actualmente no es compatible con Azure DNS.

Se hace seguimiento de la característica de redireccionamiento de dirección URL en el trabajo pendiente de Azure DNS. Use el sitio de comentarios para [registrar su compatibilidad con esta característica](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape).

### <a name="does-azure-dns-support-the-extended-ascii-encoding-8-bit-set-for-txt-record-sets"></a>¿Azure DNS admite el conjunto de codificación ASCII extendido (8 bits) para conjuntos de registros TXT?

Sí. Azure DNS admite el conjunto de codificación ASCII extendido para conjuntos de registros TXT, pero se debe usar la versión más reciente de las API REST de Azure, los SDK, PowerShell y la CLI. Las versiones anteriores al 1 de octubre de 2017 o al SDK 2.1 no admiten el conjunto de ASCII extendido. 

Por ejemplo, podría proporcionar una cadena como el valor de un registro TXT con el carácter ASCII extendido \128. Un ejemplo es "abcd\128efgh." Azure DNS usa el valor de bytes de este carácter, que es 128, en la representación interna. En el momento de la resolución DNS; este valor de bytes se devolverá en la respuesta. Debe tener en cuenta también que "abc" y "\097\098\099" son intercambiables en lo que respecta a la resolución. 

Seguimos las reglas de secuencias de escape de caracteres maestros de archivos de zona [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt) para los registros TXT. Por ejemplo, según la RFC, `\` actúa ahora como secuencia de escape. Si especifica `A\B` como valor del registro TXT, se representa y resuelve simplemente como `AB`. Si realmente desea que el registro TXT sea `A\B` en la resolución, tendrá que volver a utilizar una secuencia de escape con `\`. Por ejemplo, especifique `A\\B`.

Esta compatibilidad no está disponible actualmente para los registros TXT creados en Azure Portal.

## <a name="alias-records"></a>Registros de alias

### <a name="what-are-some-scenarios-where-alias-records-are-useful"></a>¿En qué escenarios son útiles los registros de alias?

Consulte la sección de escenarios en [Introducción a los registros de alias de Azure DNS](dns-alias.md).

### <a name="what-record-types-are-supported-for-alias-record-sets"></a>¿Qué tipos de registros se admiten para los conjuntos de registros de alias?

Los conjuntos de registros de alias se admiten para los siguientes tipos de registros en una zona de Azure DNS:
 
- Un 
- AAAA
- CNAME 

### <a name="what-resources-are-supported-as-targets-for-alias-record-sets"></a>¿Qué recursos se admiten como destinos para los conjuntos de registros de alias?

- **Apuntar a un recurso de dirección IP pública desde un conjunto de registros A/AAAA de DNS**. Puede crear un conjunto de registros D/AAAA y hacer que sea un conjunto de registros de alias que apunte a un recurso de dirección IP pública.
- **Apuntar a un perfil de Traffic Manager desde un conjunto de registros D/AAAA/CNAME de DNS.** Puede apuntar al CNAME de un perfil de Traffic Manager desde un conjunto de registros CNAME de DNS. Un ejemplo es contoso.trafficmanager.net. Ahora, también puede apuntar a un perfil de Traffic Manager que tenga puntos de conexión externos desde un registro D o AAAA establecido en su zona DNS.
- **Apunte a un punto de conexión de Azure Content Delivery Network (CDN)** . Esto es útil al crear los sitios web estáticos mediante Azure Storage y Azure CDN.
- **Apuntar a otro conjunto de registros de DNS dentro de la misma zona**. Los registros de alias pueden hacer referencia a otros conjuntos de registros del mismo tipo. Por ejemplo, puede hacer que un conjunto de registros CNAME de DNS sea un alias de otro conjunto de registros CNAME del mismo tipo. Esta organización resulta útil si desea que algunos conjuntos de registros sean alias y otros no alias.

### <a name="can-i-create-and-update-alias-records-from-the-azure-portal"></a>¿Puedo crear y actualizar registros de alias desde Azure Portal?

Sí. Puede crear o administrar los registros de alias en Azure Portal junto con las API REST de Azure, PowerShell, la CLI y los SDK.

### <a name="will-alias-records-help-to-make-sure-my-dns-record-set-is-deleted-when-the-underlying-public-ip-is-deleted"></a>¿Los registros de alias ayudarán a garantizar que mi conjunto de registros de DNS se eliminará cuando se elimine la dirección IP pública subyacente?

Sí. Esta característica es una de las funcionalidades principales de los registros de alias. Ayudan a evitar posibles interrupciones para los usuarios de la aplicación.

### <a name="will-alias-records-help-to-make-sure-my-dns-record-set-is-updated-to-the-correct-ip-address-when-the-underlying-public-ip-address-changes"></a>¿Los registros de alias ayudarán a garantizar que mi conjunto de registros de DNS se actualizará a la dirección IP correcta cuando cambie la dirección IP pública subyacente?

Sí. Esta característica es una de las funcionalidades principales de los registros de alias. Ayudan a evitar posibles interrupciones o riesgos de seguridad para las aplicaciones.

### <a name="are-there-any-restrictions-when-using-alias-record-sets-for-a-or-aaaa-records-to-point-to-traffic-manager"></a>¿Hay alguna restricción a la hora de usar conjuntos de registros de alias para que registros A o AAAA apunten a Traffic Manager?

Sí. Para apuntar a un perfil de Traffic Manager como un alias desde un conjunto de registros A o AAAA, el perfil de Traffic Manager solo debe usar puntos de conexión externos. Al crear los puntos de conexión externos en Traffic Manager, proporcione las direcciones IP reales de los puntos de conexión.

### <a name="is-there-an-additional-charge-to-use-alias-records"></a>¿Hay algún cargo adicional por utilizar los registros de alias?

Los registros de alias son una calificación de un conjunto de registros de DNS válido. No hay ninguna facturación adicional por los registros de alias.

## <a name="use-azure-dns"></a>Uso de Azure DNS

### <a name="can-i-co-host-a-domain-by-using-azure-dns-and-another-dns-provider"></a>¿Puedo hospedar de manera conjunta un dominio mediante Azure DNS y otro proveedor de DNS?

Sí. DNS de Azure es compatible con dominios de hospedaje conjunto con otros servicios DNS.

Para configurar el hospedaje conjunto, modifique los registros de NS para que el dominio apunte a los servidores de nombres de ambos proveedores. Los registros de servidor de nombres (NS) controlan qué proveedores reciben consultas de DNS para el dominio. Puede modificar estos registros de NS en Azure DNS, en el otro proveedor y en la zona primaria. Habitualmente, la zona primera se configura mediante el registrador de nombres de dominio. Para más información sobre la delegación DNS, vea [Delegación de dominios DNS](dns-domain-delegation.md).

Además, asegúrese de que los registros de DNS del dominio estén sincronizados entre ambos proveedores de DNS. Azure DNS actualmente no admite las transferencias de zona DNS. Los registros DNS se deben sincronizar mediante el [Portal de administración de Azure DNS](dns-operations-recordsets-portal.md), [la API REST](https://docs.microsoft.com/rest/api/dns/), el [SDK](dns-sdk.md), los [cmdlets de PowerShell](dns-operations-recordsets.md) o la [herramienta CLI](dns-operations-recordsets-cli.md).

### <a name="do-i-have-to-delegate-my-domain-to-all-four-azure-dns-name-servers"></a>¿Tengo que delegar mi dominio en los cuatro servidores de nombres de Azure DNS?

Sí. Azure DNS asigna cuatro servidores de nombres a cada zona DNS. Esta disposición se usa para aislar los errores y aumentar la resistencia. Para poder optar al Acuerdo de Nivel de Servicio de Azure DNS, delegue el dominio a los cuatro servidores de nombres.

### <a name="what-are-the-usage-limits-for-azure-dns"></a>¿Cuáles son los límites de uso de Azure DNS?

Cuando se usa Azure DNS, se aplican estos límites predeterminados.

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

### <a name="can-i-move-an-azure-dns-zone-between-resource-groups-or-between-subscriptions"></a>¿Puedo mover una zona de Azure DNS entre los grupos de recursos o entre las suscripciones?

Sí. Las zonas DNS se pueden mover entre grupos de recursos o entre suscripciones.

Si mueve una zona DNS, no hay ningún efecto en las consultas de DNS. Los servidores de nombres asignados a la zona siguen siendo los mismos. Las consultas de DNS se procesan de la manera habitual.

Para obtener más información e instrucciones sobre cómo mover zonas DNS, vea [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](../azure-resource-manager/management/move-resource-group-and-subscription.md).

### <a name="how-long-does-it-take-for-dns-changes-to-take-effect"></a>¿Cuánto tardan en surtir efecto los cambios de DNS?

Las nuevas zonas DNS y registros de DNS suelen aparecen rápidamente en los servidores de nombres de Azure DNS. No tarda más allá de unos segundos.

Los cambios en los registros de DNS existentes pueden tardar un poco más. Por lo general, aparecen en los servidores de nombres de Azure DNS en 60 segundos. El almacenamiento en caché de DNS por parte de los clientes de DNS y las resoluciones recursivas de DNS fuera de Azure DNS también pueden afectar cuánto se tarda. Para controlar esta duración de memoria caché, use la propiedad de período de vida (TTL) de cada conjunto de registros.

### <a name="how-can-i-protect-my-dns-zones-against-accidental-deletion"></a>¿Cómo puedo proteger mis zonas DNS contra una eliminación accidental?

Azure DNS se administra mediante Azure Resource Manager. Azure DNS se beneficia de las características de control de acceso que proporciona Azure Resource Manager. El control de acceso basado en rol controla qué usuarios tienen acceso de lectura o escritura a conjuntos de registros y zonas DNS. Los bloqueos de recursos evitan la modificación o eliminación accidental de conjuntos de registros y zonas DNS.

Para más información, consulte [Protección de zonas y registros DNS](dns-protect-zones-recordsets.md).

### <a name="how-do-i-set-up-spf-records-in-azure-dns"></a>¿Cómo puedo configurar registros de SPF en Azure DNS?

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="do-azure-dns-name-servers-resolve-over-ipv6"></a>¿Se resuelven los servidores de nombres de Azure DNS en IPv6? 

Sí. Los servidores de nombres de Azure DNS son de pila doble. Esto significa que tienen direcciones IPv4 e IPv6. Para encontrar la dirección IPv6 de los servidores de nombres de Azure DNS asignados a la zona DNS, use una herramienta como nslookup. Un ejemplo es `nslookup -q=aaaa <Azure DNS Nameserver>`.

### <a name="how-do-i-set-up-an-idn-in-azure-dns"></a>¿Cómo configuro un IDN en Azure DNS?

Los nombres de dominio internacionalizados (IDN) codifican cada nombre DNS mediante [punycode](https://en.wikipedia.org/wiki/Punycode). Las consultas de DNS se realizan con estos nombres codificados mediante punycode.

Para configurar IDN en Azure DNS, convierta el nombre de la zona o el nombre del conjunto de registros a punycode. Azure DNS no admite actualmente la conversión integrada hacia o desde punycode.

## <a name="next-steps"></a>Pasos siguientes

- [Más información sobre Azure DNS](dns-overview.md).

- [Más información sobre cómo usar Azure DNS para dominios privados](private-dns-overview.md).

- [Más información sobre registros y zonas DNS](dns-zones-records.md).

- [Introducción a Azure DNS](dns-getstarted-portal.md).
