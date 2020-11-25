---
title: Información general sobre App Service Environment
description: Información general sobre App Service Environment
author: ccompy
ms.assetid: 3d37f007-d6f2-4e47-8e26-b844e47ee919
ms.topic: article
ms.date: 11/16/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: fbc498fcd654d16936c2548528e2600be68a2ad9
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94663376"
---
# <a name="app-service-environment-overview"></a>Información general sobre App Service Environment 

> [!NOTE]
> Este artículo trata sobre App Service Environment v3 (versión preliminar).
> 

Azure App Service Environment es una característica de Azure App Service que proporciona un entorno completamente aislado y dedicado para ejecutar de forma segura las aplicaciones de App Service a gran escala. Esta funcionalidad puede hospedar sus:

- Aplicaciones web de Windows
- Aplicaciones web de Linux
- Contenedores de Docker
- Functions

Las instancias de App Service Environment (ASE) son adecuadas para cargas de trabajo de aplicaciones que necesitan:

- Gran escala.
- Aislamiento y acceso a redes seguro
- Alta utilización de memoria
- Muchas solicitudes por segundo (RPS). Puede crear varias instancias de ASE en una o varias regiones de Azure. Esta flexibilidad hace que las instancias de ASE sean perfectas para aplicaciones sin estado de escalado horizontal con un requisito exigente de RPS.

Los ASE hospedan aplicaciones de un solo cliente y lo hacen en una de sus redes virtuales. Los clientes tienen un mayor control sobre el tráfico de red entrante y saliente de la aplicación. Las aplicaciones pueden establecer conexiones seguras a alta velocidad por redes virtuales a los recursos corporativos locales.

ASEv3 incluye su propio plan de tarifa, la versión 2 aislada.
Las instancias de App Service Environment v3 proporcionan un entorno que protege las aplicaciones de una subred de la red y proporciona su propia implementación privada de Azure App Service.
Varias instancias de ASE pueden utilizarse para escalar horizontalmente. Las aplicaciones que se ejecutan en ASE pueden tener su acceso validado por dispositivos de subida como firewalls de aplicación web (WAF). Para más información, consulte Firewall de aplicaciones web (WAF).

## <a name="usage-scenarios"></a>Escenarios de uso

App Service Environment tiene muchos casos de uso, incluidos:

- Aplicaciones de línea de negocio internas
- Aplicaciones que necesitan más de 30 instancias de ASP
- Sistema de un solo inquilino para satisfacer los requisitos internos de seguridad o cumplimiento
- Hospedaje de aplicaciones aisladas de red
- Aplicaciones de múltiples niveles

Hay una serie de características de red que permiten que las aplicaciones de App Service multiinquilino lleguen a recursos aislados de red o que se aíslen de la red por sí mismos. Estas características se habilitan en el nivel de aplicación.  Con un ASE, no hay ninguna configuración adicional en las aplicaciones para que estén en la red virtual. Las aplicaciones se implementan en un entorno con aislamiento de red que ya está en una red virtual. En la parte superior del ASE que hospeda las aplicaciones con aislamiento de red, también es un sistema de un solo inquilino. No hay otros clientes que usen el ASE. Si realmente necesita un caso de aislamiento completo, también puede implementar el ASE en hardware dedicado. Entre el hospedaje de aplicaciones con aislamiento de red, el inquilino único y la capacidad 

## <a name="dedicated-environment"></a>Entorno dedicado
Una instancia de ASE está dedicada exclusivamente a una sola suscripción y puede hospedar 200 instancias de un plan de App Service. El rango puede abarcar 100 instancias en un único plan de App Service, 100 planes de App Service de una sola instancia o cualquier opción intermedia.

Una instancia de ASE se compone de front-end y trabajos. Los servidores front-end son responsables de la terminación HTTP/HTTPS y del equilibrio de carga automático de la solicitudes de aplicación en una instancia de ASE. Los servidores front-end se agregan automáticamente ya que los planes de App Service en la instancia de ASE se escalan horizontalmente.

Los trabajos son roles que hospedan las aplicaciones del cliente. Los trabajos están disponibles en tres tamaños fijos:

- Dos unidades vCPU/8 GB de RAM
- Cuatro unidades vCPU/16 GB de RAM
- Ocho unidades vCPU/32 GB de RAM

Los clientes no necesitan administrar los servidores front-end ni los roles de trabajo. Toda la infraestructura se administra automáticamente. Cuando se crean o reducen horizontalmente planes de App Service en un ASE, la infraestructura necesaria se agrega o elimina según corresponda.

Hay un cargo por las instancias del plan de App Service de la versión 2 aislada. Si no tiene ningún plan de App Service en el ASE, se le cobrará como si tuviera un plan de App Service con una instancia de los dos roles de trabajo principales.

## <a name="virtual-network-support"></a>Compatibilidad con redes virtuales
La característica de ASE es una implementación de Azure App Service directamente en la red virtual de Azure Resource Manager de un cliente. Un ASE siempre existe en una subred de una red virtual. Puede usar las características de seguridad de las redes virtuales para controlar las comunicaciones de red entrantes y salientes de las aplicaciones.

Los grupos de seguridad de red restringen las comunicaciones de red entrantes a la subred donde reside una instancia de ASE. Puede usar grupos de seguridad de red para ejecutar aplicaciones tras dispositivos y servicios ascendentes, como WAF y proveedores de SaaS de red.

Las aplicaciones, además, suelen requerir acceso a recursos corporativos, como bases de datos internas y servicios web. Si implementa la instancia de ASE en una red virtual que tiene una conexión VPN a la red local, las aplicaciones de la instancia de ASE podrán tener acceso a los recursos locales. La capacidad es cierta independientemente de si la red privada virtual es una VPN de sitio a sitio o de Azure ExpressRoute.

## <a name="preview"></a>Versión preliminar
App Service Environment v3 se encuentra en versión preliminar pública.  Se están agregando algunas características durante la progresión de la versión preliminar. Las limitaciones actuales de ASEv3 incluyen:

- Incapacidad de escalar un plan de App Service más allá de cinco instancias
- Incapacidad de obtener un contenedor de un registro privado
- Incapacidad para que las características de App Service no admitidas actualmente pasen a través de la red virtual del cliente
- Ningún modelo de implementación externo con un punto de conexión accesible por Internet
- Incompatibilidad con la línea de comandos (CLI de AZ y PowerShell)
- Incapacidad de actualizar de ASEv2 a ASEv3
- Incompatibilidad con FTP
- Incapacidad para que algunas características de App Service pasen a través de la red virtual del cliente. La copia de seguridad o restauración, las referencias de Key Vault en la configuración de la aplicación, el uso de un registro de contenedor privado y el registro de diagnóstico en Storage no funcionarán con puntos de conexión de servicio o puntos de conexión privados
    
### <a name="asev3-preview-architecture"></a>Arquitectura de la versión preliminar de ASEv3
En la versión preliminar de ASEv3, el ASE usará puntos de conexión privados para admitir el tráfico entrante. El punto de conexión privado se reemplazará por los equilibradores de carga en GA. En la versión preliminar, el ASE no integrará compatibilidad con un punto de conexión accesible por Internet. Puede agregar una instancia de Application Gateway para ese propósito. El ASE necesita recursos en dos subredes.  El tráfico entrante fluirá a través de un punto de conexión privado. El punto de conexión privado se puede colocar en cualquier subred, siempre que tenga una dirección disponible que puedan usar los puntos de conexión privados.  La subred de salida debe estar vacía y delegada en Microsoft.Web/hostingEnvironments. Mientras el ASE usa la subred de salida, esta no se puede usar para nada más.

Con ASEv3, no hay ningún requisito de red entrante o saliente en la subred de ASE. Puede controlar el tráfico con grupos de seguridad de red y tablas de enrutamiento, y solo afectará al tráfico de la aplicación. No elimine el punto de conexión privado asociado a su ASE, ya que esta acción no se puede deshacer. El punto de conexión privado utilizado en el ASE también se utilizará en todas las aplicaciones del ASE. 
