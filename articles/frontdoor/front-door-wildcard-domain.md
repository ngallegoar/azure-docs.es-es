---
title: 'Azure Front Door: Compatibilidad con dominios con caracteres comodín'
description: Este artículo le ayudará a comprender cómo Azure Front Door admite la asignación y administración de dominios con caracteres comodín en la lista de dominios personalizados.
services: frontdoor
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2020
ms.author: sharadag
ms.openlocfilehash: c568c9cc5c57098385cc7399459ec656cdbfc305
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79540964"
---
# <a name="wildcard-domains"></a>Dominios con caracteres comodín

Aparte de los dominios de vértice y los subdominios, también puede asignar un nombre de dominio con comodín a la lista de hosts de front-end o a los dominios personalizados del perfil de Front Door. La presencia de dominios con caracteres comodín en la configuración de Front Door simplifica el comportamiento de enrutamiento del tráfico para varios subdominios de una API, aplicación o sitio web a partir de la misma regla de enrutamiento sin tener que modificar la configuración para agregar o especificar cada subdominio por separado. Por ejemplo, puede definir el enrutamiento para `customer1.contoso.com`, `customer2.contoso.com` y `customerN.contoso.com` con la misma regla de enrutamiento agregando un dominio con caracteres comodín `*.contoso.com`.

Algunos de los escenarios clave que se resuelven con la compatibilidad con los dominios con caracteres comodín incluyen:

- Ya no es necesario incorporar cada subdominio en Front Door y, después, habilitar HTTPS para enlazar un certificado para cada subdominio.
- Si una aplicación agrega un nuevo subdominio, ya no es necesario cambiar la configuración de Front Door. Por el contrario, antes se necesitaba agregar el subdominio, enlazar un certificado a él, asociar una directiva de firewall de aplicaciones web (WAF) y agregar el dominio a diferentes reglas de enrutamiento.

> [!NOTE]
> Actualmente, los dominios con caracteres comodín solo se admiten a través de la API, PowerShell y la CLI. La posibilidad de agregar dominios con caracteres comodín mediante Azure Portal no está disponible.

## <a name="adding-wildcard-domains"></a>Incorporación de dominios con caracteres comodín

Puede incorporar un dominio con caracteres comodín en la sección Hosts de front-end o Dominios. Al igual que con los subdominios, Front Door comprueba que haya también una asignación de CNAME para el dominio con caracteres comodín. Esta asignación de DNS puede ser una asignación directa de CNAME como `*.contoso.com` que se asigna a `contoso.azurefd.net`, o a través de una asignación temporal de afdverify como `afdverify.contoso.com` que se asigna a `afdverify.contoso.azurefd.net` y que comprueba la asignación de CNAME en busca de caracteres comodín (Azure DNS admite registros de caracteres comodín).

También puede agregar todos los subdominios de nivel único del dominio con caracteres comodín que desee en los hosts de front-end siempre y cuando no superen el límite máximo establecido en estos hosts. Esta funcionalidad puede ser necesaria para definir una ruta diferente para un subdominio que para el resto de los dominios (del dominio con caracteres comodín) o tener una directiva de firewall de aplicaciones web diferente para un subdominio concreto. Por lo tanto, `*.contoso.com` permitirá agregar `foo.contoso.com` sin tener que volver a demostrar la propiedad del dominio, pero no podrá agregar `foo.bar.contoso.com` ya que no es un subdominio de nivel único de `*.contoso.com`. Para agregar `foo.bar.contoso.com` sin la validación adicional de la propiedad del dominio, deberá agregar `*.bar.contosonews.com`.

### <a name="limitations"></a>Limitaciones

1. Si se agrega un dominio con caracteres comodín a un perfil de Front Door determinado, no se podrá agregar a ningún otro perfil. 
2. Si se agrega un dominio con caracteres comodín en un perfil de Front Door determinado, los subdominios de ese dominio con caracteres comodín no se podrán agregar a otro perfil de Front Door ni de Azure CDN de Microsoft.
3. Si se agrega un subdominio de un dominio con caracteres comodín a un perfil de Front Door o de Azure CDN de Microsoft, el dominio con caracteres comodín no se podrá agregar a ningún otro perfil de Front Door. 
4. Si dos perfiles (de Front Door o de Azure CDN de Microsoft) tienen varios subdominios de un dominio raíz, no se podrán agregar dominios con caracteres comodín en ninguno de los perfiles.

## <a name="certificate-binding-for-wildcard-domains-and-its-subdomains"></a>Enlace de certificados para dominios con caracteres comodín y sus subdominios

Para aceptar el tráfico HTTPS en el dominio con caracteres comodín, debe habilitar HTTPS en ese dominio. El enlace de certificados para el dominio con caracteres comodín requiere un certificado comodín, es decir, el nombre del asunto del certificado debe también contener el dominio con caracteres comodín.

> [!NOTE]
> Actualmente, solo está disponible la opción de usar su propio certificado SSL personalizado para habilitar HTTPS para dominios con caracteres comodín. Los certificados administrados de Front Door no se pueden usar para dominios con caracteres comodín. 

También se puede usar el mismo certificado comodín de Key Vault para los subdominios o usar certificados administrados de Front Door para los subdominios.
Si se agrega un subdominio para un dominio con caracteres comodín y este ya tiene un certificado asociado, no se puede deshabilitar HTTPS para este subdominio. El subdominio usará de forma predeterminada el enlace de certificados del dominio con caracteres comodín, a menos que se reemplace este por un certificado de Key Vault diferente o un certificado administrado de Front Door.

## <a name="web-application-firewall-for-wildcard-domains-and-its-subdomains"></a>Firewall de aplicaciones web (WAF) para dominios con caracteres comodín y sus subdominios

Las directivas de WAF se pueden adjuntar a un dominio con caracteres comodín al igual que a otros dominios. Se puede aplicar una directiva de WAF diferente a un subdominio de un dominio con caracteres comodín. En el caso de los subdominios, debe especificar explícitamente la directiva de WAF que se va a usar incluso si se trata de la misma directiva que para el dominio con caracteres comodín. Los subdominios **no** heredarán automáticamente la directiva de WAF del dominio con caracteres comodín.

Si tiene un escenario en el que no desea que el WAF se ejecute para un subdominio, puede crear una directiva de WAF en blanco sin conjuntos de reglas personalizados o administrados.

## <a name="routing-rules-for-wildcard-domains-and-its-subdomains"></a>Reglas de enrutamiento para dominios con caracteres comodín y sus subdominios

Al configurar una regla de enrutamiento, puede seleccionar un dominio con caracteres comodín como host de front-end. También puede disponer de un comportamiento de enrutamiento diferente para el dominio con caracteres comodín y para sus subdominios. Como se indica en [Coincidencia de rutas con Front Door](front-door-route-matching.md), se elegirá la coincidencia más específica para el dominio en diferentes reglas de enrutamiento del entorno de ejecución.

> [!WARNING]
> Si tiene dos reglas de enrutamiento como, por ejemplo, **Ruta 1**: `*.foo.com/*` que se asigna al grupo de back-end A, y **Ruta 2**: `bar.foo.com/somePath/*` que se asigna al grupo de back-end B, si llega una solicitud a `bar.foo.com/anotherPath/*`, los clientes sufrirán errores ya que Front Door no encontrará ninguna coincidencia entre ambas rutas. Esto se debe a que, según nuestro [algoritmo de coincidencia de rutas](front-door-route-matching.md), Front Door seleccionará la ruta 2 en función de una coincidencia de dominio más específica, pero solo para descubrir que no hay patrones de ruta de acceso coincidentes. 


## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [crear una instancia de Front Door](quickstart-create-front-door.md).
- Aprenda a [agregar un dominio personalizado en Front Door](front-door-custom-domain.md).
- Aprenda a [habilitar HTTPS en un dominio personalizado](front-door-custom-domain-https.md).
