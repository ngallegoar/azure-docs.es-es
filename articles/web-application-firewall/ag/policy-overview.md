---
title: Introducción a las directivas de firewall de aplicaciones web (WAF) de Azure
description: En este artículo se describen las directivas globales, por sitio y por URI del firewall de aplicaciones web (WAF).
services: web-application-firewall
ms.topic: article
author: winthrop28
ms.service: web-application-firewall
ms.date: 10/14/2020
ms.author: victorh
ms.openlocfilehash: 789a60c80d14301c107824d178a3566d358d287f
ms.sourcegitcommit: 1b47921ae4298e7992c856b82cb8263470e9e6f9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92056402"
---
# <a name="azure-web-application-firewall-waf-policy-overview"></a>Introducción a las directivas de firewall de aplicaciones web (WAF) de Azure

Las directivas de firewall de aplicaciones web contienen todas las configuraciones y opciones de WAF. Esto incluye exclusiones, reglas personalizadas o reglas administradas, entre otras. Después, estas directivas se asocian a una puerta de enlace de aplicaciones (global), un cliente de escucha (por sitio) o una regla basada en ruta de acceso (por URI) para que surtan efecto.

> [!NOTE]
> Las directivas de firewall de aplicaciones web (WAF) de Azure por URI se encuentran en versión preliminar pública.
> 
> Esta versión preliminar pública se proporciona sin un acuerdo de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

No hay límite en el número de directivas que puede crear. Cuando se crea una directiva, se debe asociar a una puerta de enlace de aplicaciones para que surta efecto. Se puede asociar a cualquier combinación de puertas de enlace de aplicaciones, clientes de escucha y reglas basadas en rutas de acceso.

## <a name="global-waf-policy"></a>Directiva WAF global

Al asociar una directiva de WAF globalmente, cada sitio detrás del WAF de Application Gateway se protege con las mismas reglas administradas, reglas personalizadas, exclusiones y cualquier otro valor configurado.

Si desea que se aplique una única directiva a todos los sitios, puede asociar la directiva a la puerta de enlace de aplicaciones. Para más información, consulte este artículo para saber cómo [crear directivas de firewall de aplicaciones web para Application Gateway](create-waf-policy-ag.md). 

## <a name="per-site-waf-policy"></a>Directiva de WAF por sitio

Con las directivas de WAF por sitio, puede proteger varios sitios con necesidades de seguridad diferentes en un solo WAF mediante el uso de directivas por sitio. Por ejemplo, si hay cinco sitios detrás del WAF, puede tener cinco directivas de WAF independientes (una para cada cliente de escucha) para personalizar las exclusiones, las reglas personalizadas, los conjuntos de reglas administradas y todas las demás configuraciones de WAF para cada sitio.

Supongamos que la puerta de enlace de aplicaciones tiene una directiva global aplicada. Aplique entonces una directiva diferente a un cliente de escucha en esa puerta de enlace de aplicaciones. La directiva del cliente de escucha ahora surte efecto solo para ese cliente de escucha. La directiva global de la puerta de enlace de aplicaciones se sigue aplicando a todos los demás clientes de escucha y reglas basadas en rutas de acceso que no tienen una directiva específica asignada.

## <a name="per-uri-policy"></a>Directiva por URI

Para personalizar aún más al nivel de URI, puede asociar una directiva de WAF a una regla basada en ruta de acceso. Si hay determinadas páginas en un solo sitio que requieren directivas diferentes, puede realizar cambios en la directiva de WAF que solo afecten a un URI determinado. Esto puede aplicarse a una página de pago o de inicio de sesión, o a cualquier otro URI que necesite una directiva de WAF aún más específica que los demás sitios de WAF.

Al igual que con las directivas de WAF por sitio, las directivas más específicas reemplazan a las menos específicas. Esto significa que una directiva por URI en una asignación de ruta de dirección URL invalida cualquier directiva de WAF global o por sitio anterior.

## <a name="example"></a>Ejemplo

Supongamos que tiene tres sitios: contoso.com, fabrikam.com y adatum.com, todos ellos detrás de la misma puerta de enlace de aplicaciones. Quiere que WAF se aplique a los tres sitios, pero necesita mayor seguridad con adatum.com porque es donde los clientes visitan, examinan y compran productos.

Puede aplicar una directiva global a WAF, con algunas opciones básicas, exclusiones o reglas personalizadas, si es necesario, para evitar que algunos falsos positivos bloqueen el tráfico. En este caso, no es necesario que se ejecuten reglas globales de inyección de SQL, ya que fabrikam.com y contoso.com son páginas estáticas sin back-end de SQL. Por lo tanto, puede deshabilitar esas reglas en la directiva global.

Esta directiva global es adecuada para contoso.com y fabrikam.com, pero debe tener más cuidado con adatum.com, donde se administran los pagos y la información de inicio de sesión. Puede aplicar una directiva por sitio al cliente de escucha de adatum y dejar las reglas de SQL en ejecución. Supongamos también que hay una cookie que bloquea el tráfico, por lo que puede crear una exclusión para esa cookie para detener el falso positivo. 

El URI de adatum.com/payments es donde hay que tener cuidado. Por tanto, aplique otra directiva en ese URI y deje todas las reglas habilitadas, y también quite todas las exclusiones.

En este ejemplo, tiene una directiva global que se aplica a dos sitios. Tiene una directiva por sitio que se aplica a un sitio y, a continuación, una directiva por URI que se aplica a una regla específica basada en la ruta de acceso. Consulte (Insertar vínculo aquí cuando lo haya) cómo crear directivas por sitio y por URI para el PowerShell correspondiente en este ejemplo.

## <a name="existing-waf-configurations"></a>Configuraciones WAF existentes

Toda la nueva configuración de WAF del firewall de aplicaciones web (reglas personalizadas, configuraciones de conjunto de reglas administradas, exclusiones, etc.) se encuentra en una directiva de WAF. Si tiene una instancia de WAF existente, es posible que esta configuración siga existiendo en la configuración de WAF. Para obtener más información sobre cómo cambiar a la nueva directiva de WAF, consulte[Migración de la configuración de WAF a una directiva de WAF](https://docs.microsoft.com/azure/web-application-firewall/ag/migrate-policy). 


## <a name="next-steps"></a>Pasos siguientes

- [Creación de directivas por sitio y por URI mediante Azure PowerShell](per-site-policies.md).
