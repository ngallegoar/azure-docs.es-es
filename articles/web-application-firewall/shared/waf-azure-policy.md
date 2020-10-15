---
title: Azure Policy y firewall de aplicaciones web de Azure
description: El firewall de aplicaciones web (WAF) de Azure combinado con Azure Policy puede ayudar a aplicar los estándares de la organización y evaluar el cumplimiento a escala para los recursos de WAF.
author: tremansdoerfer
ms.service: web-application-firewall
services: web-application-firewall
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: rimansdo
ms.openlocfilehash: 937f220980d602b755b6329da4d93df0e4b372ab
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86224067"
---
# <a name="azure-web-application-firewall-and-azure-policy"></a>Azure Policy y firewall de aplicaciones web de Azure

El firewall de aplicaciones web (WAF) de Azure combinado con Azure Policy puede ayudar a aplicar los estándares de la organización y evaluar el cumplimiento a escala para los recursos de WAF. La directiva de Azure es una herramienta de gobernanza que proporciona una vista agregada para evaluar el estado general del entorno, con la posibilidad de explorar en profundidad hasta el nivel de recurso y directiva. La directiva de Azure también ayuda al cumplimiento de los recursos gracias a la corrección masiva de los recursos existentes y la corrección automática de nuevos recursos.

## <a name="azure-policies-for-web-application-firewall"></a>Directivas de Azure para firewall de aplicaciones web

Hay varias directivas de Azure integradas para administrar los recursos de WAF. A continuación se muestra un desglose de las directivas y sus funcionalidades:

1. **El firewall de aplicaciones web (WAF) debe estar habilitado para Azure Front Door Service**: Los servicios de Azure Front Door Service se evalúan en si hay un WAF presente en la creación de recursos. La directiva tiene tres efectos: Auditar, denegar y deshabilitar. Auditoría realiza un seguimiento cuando Azure Front Door Service no tiene un WAF y permite a los usuarios ver qué es lo que no cumple Azure Front Door Service. La denegación impide que se cree un Azure Front Door Service en caso de que no se adjunte un WAF. Si está deshabilitada, la directiva se desactiva.

2. **El firewall de aplicaciones web (WAF) debe estar habilitado para Application Gateway**: App Gateways se evalúan en si hay un WAF presente en la creación de recursos. La directiva tiene tres efectos: Auditar, denegar y deshabilitar. Auditar realiza un seguimiento cuando una Application Gateway no tiene un WAF y permite a los usuarios ver lo que Application Gateway no cumple. La denegación impide que se cree una Application Gateway en caso de que no se adjunte un WAF. Si está deshabilitada, la directiva se desactiva.

3. **El firewall de aplicaciones web (WAF) debe usar el modo especificado para Azure Front Door Service**: Exige que el modo de detección o prevención esté activo en todas las directivas de firewall de aplicaciones web para Azure Front Door Service. La directiva tiene tres efectos: Auditar, denegar y deshabilitar. Auditar realiza un seguimiento cuando un WAF no se ajusta al modo especificado. La denegación impide que se cree WAF si no está en el modo correcto. Si está deshabilitada, la directiva se desactiva.

4. **El firewall de aplicaciones web (WAF) debe usar el modo especificado para Application Gateway**: Exige que el modo de detección o prevención esté activo en todas las directivas de firewall de aplicaciones web para Application Gateway. La directiva tiene tres efectos: Auditar, denegar y deshabilitar. Auditar realiza un seguimiento cuando un WAF no se ajusta al modo especificado. La denegación impide que se cree WAF si no está en el modo correcto. Si está deshabilitada, la directiva se desactiva.


## <a name="launch-an-azure-policy"></a>Inicio de Azure Policy


1.  En la página principal de Azure, escriba Directiva en la barra de búsqueda y haga clic en el icono de Azure Policy.

2.  En el servicio de directivas de Azure, en **Creación**, seleccione **Asignaciones**.

[!div class="mx-imgBorder"]
![Firewall de aplicaciones web de Azure](../media/waf-azure-policy/policy-home.png)

3.  En la página asignaciones, seleccione el icono **Directivas de asignación** en la parte superior.

[!div class="mx-imgBorder"]
![Firewall de aplicaciones web de Azure](../media/waf-azure-policy/assign-policy.png)

4.  En la pestaña de conceptos básicos de la página de Directivas de asignación, actualice los campos siguientes:
    1.  **Ámbito**: Seleccione las suscripciones y los grupos de recursos de Azure que se deben ver afectados por el Azure Policy.
    2.  **Exclusiones**: Seleccionar los recursos del ámbito que se van a excluir de la directiva. 
    3.  **Definición de directiva**: Seleccione la Azure Policy que se va a aplicar al ámbito con exclusiones. Escriba "Firewall de aplicaciones web" en la barra de búsqueda para elegir el Azure Policy de seguridad de aplicaciones web pertinente.

[!div class="mx-imgBorder"]
![Firewall de aplicaciones web de Azure](../media/waf-azure-policy/policy-listing.png)


5.  Seleccione la pestaña **Parámetros** y actualice los parámetros de las directivas. Para aclarar mejor lo que hace el parámetro, mantenga el mouse sobre el icono de información junto al nombre del parámetro para una mayor aclaración.

6.  Seleccione **Revisar + crear** para finalizar la directiva de Azure. La directiva de Azure tardará aproximadamente 15 minutos hasta que esté activa para los nuevos recursos.
