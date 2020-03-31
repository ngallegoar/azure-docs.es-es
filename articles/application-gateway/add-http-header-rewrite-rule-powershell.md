---
title: Reescribir encabezados HTTP en Azure Application Gateway
description: En este artículo se proporciona información sobre cómo reescribir los encabezados HTTP en Azure Application Gateway con Azure PowerShell
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 04/12/2019
ms.author: absha
ms.openlocfilehash: 47fe6a5247622e3ad3b3720955068580e0329913
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "64947204"
---
# <a name="rewrite-http-request-and-response-headers-with-azure-application-gateway---azure-powershell"></a>Reescribir los encabezados HTTP de solicitud y respuesta con Azure Application Gateway mediante Azure PowerShell

En este artículo se describe cómo usar Azure PowerShell para configurar una instancia de [SKU de Application Gateway v2](<https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant>) para reescribir los encabezados HTTP en las solicitudes y respuestas.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="before-you-begin"></a>Antes de empezar

- Deberá ejecutar Azure PowerShell de manera local para completar los pasos descritos en este artículo. También debe tener instalado el módulo de Az versión 1.0.0 o posterior. Ejecute `Import-Module Az` y, luego, `Get-Module Az` para conocer la versión que tiene instalada. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps). Después de verificar la versión de PowerShell, ejecute `Login-AzAccount` para crear una conexión con Azure.
- Debe tener una instancia de SKU de Application Gateway v2. No se admite la reescritura de encabezados en la SKU de v1. Si no tiene la SKU de v2, cree una instancia de [SKU de Application Gateway v2](https://docs.microsoft.com/azure/application-gateway/tutorial-autoscale-ps) antes de comenzar.

## <a name="create-required-objects"></a>Crear los objetos necesarios

Para configurar la reescritura de encabezados HTTP, es preciso completar estos pasos.

1. Cree los objetos necesarios para la reescritura de encabezados HTTP:

   - **RequestHeaderConfiguration**: Se usa para especificar los campos de encabezado de solicitud que quiere reescribir y el nuevo valor para los encabezados.

   - **ResponseHeaderConfiguration**: Se usa para especificar los campos de encabezado de respuesta que quiere reescribir y el nuevo valor para los encabezados.

   - **ActionSet**: Contiene las configuraciones de los encabezados de solicitud y respuesta especificados anteriormente.

   - **Condición**: Una configuración opcional. Las condiciones de reescritura evalúan el contenido de las solicitudes y respuestas HTTP(S). La acción de reescritura tendrá lugar si la solicitud o respuesta HTTP(S) coinciden con la condición de reescritura.

     Si asocia más de una condición con una acción, la acción se produce solo cuando se cumplen todas las condiciones. En otras palabras, se trata de una operación AND lógica.

   - **RewriteRule**: Contiene varias combinaciones de acción de reescritura y condición de reescritura.

   - **RuleSequence**: Configuración opcional que ayuda a establecer el orden en que se ejecutan las reglas de reescritura. Esta configuración es útil cuando hay varias reglas de reescritura en un conjunto de reescritura. Una regla de reescritura que tiene un valor de secuencia de reglas más bajo se ejecuta primero. Si asigna el mismo valor de secuencia de reglas a dos reglas de reescritura, el orden de ejecución es no determinista.

     Si no se especifica explícitamente RuleSequence, se establece un valor predeterminado de 100.

   - **RewriteRuleSet**: Contiene varias reglas de reescritura que se asociarán a una regla de enrutamiento de solicitudes.

2. Conecte RewriteRuleSet a una regla de enrutamiento. La configuración de reescritura se conecta al agente de escucha de origen mediante la regla de enrutamiento. Cuando usa una regla de enrutamiento básica, la configuración de reescritura de encabezados se asocia a un agente de escucha de origen y es una reescritura de encabezados global. Cuando usa una regla de enrutamiento basada en rutas, la configuración de reescritura de encabezados se define en la asignación de la ruta de URL. En este caso, solo se aplica al área específica de la ruta de acceso de un sitio.

Puede crear varios conjuntos de reescritura de encabezados HTTP y aplicar cada conjunto de reescritura a varios agentes de escucha. Pero solo puede aplicar un conjunto de reescritura a un agente de escucha específico.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

```azurepowershell
Connect-AzAccount
Select-AzSubscription -Subscription "<sub name>"
```

## <a name="specify-the-http-header-rewrite-rule-configuration"></a>Especificar la configuración de la regla de reescritura de encabezado HTTP

En este ejemplo, modificará una dirección URL de redireccionamiento al reescribir el encabezado de ubicación en la respuesta HTTP siempre que el encabezado de ubicación contenga una referencia a azurewebsites.net. Para ello, agregaremos una condición para evaluar si el encabezado de ubicación en la respuesta contiene azurewebsites.net: Vamos a usar el patrón `(https?):\/\/.*azurewebsites\.net(.*)$`. Y vamos a usar `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` como valor del encabezado. Este valor reemplazará *azurewebsites.net* con *contoso.com* en el encabezado de ubicación.

```azurepowershell
$responseHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Location" -HeaderValue "{http_resp_Location_1}://contoso.com{http_resp_Location_2}"
$actionSet = New-AzApplicationGatewayRewriteRuleActionSet -RequestHeaderConfiguration $requestHeaderConfiguration -ResponseHeaderConfiguration $responseHeaderConfiguration
$condition = New-AzApplicationGatewayRewriteRuleCondition -Variable "http_resp_Location" -Pattern "(https?):\/\/.*azurewebsites\.net(.*)$" -IgnoreCase
$rewriteRule = New-AzApplicationGatewayRewriteRule -Name LocationHeader -ActionSet $actionSet
$rewriteRuleSet = New-AzApplicationGatewayRewriteRuleSet -Name LocationHeaderRewrite -RewriteRule $rewriteRule
```

## <a name="retrieve-the-configuration-of-your-application-gateway"></a>Recuperación de la configuración de la puerta de enlace de aplicación

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
```

## <a name="retrieve-the-configuration-of-your-request-routing-rule"></a>Recuperación de la configuración de la regla de enrutamiento de solicitud

```azurepowershell
$reqRoutingRule = Get-AzApplicationGatewayRequestRoutingRule -Name rule1 -ApplicationGateway $appgw
```

## <a name="update-the-application-gateway-with-the-configuration-for-rewriting-http-headers"></a>Actualización de la puerta de enlace de aplicaciones con la configuración para volver a escribir los encabezados HTTP

```azurepowershell
Add-AzApplicationGatewayRewriteRuleSet -ApplicationGateway $appgw -Name LocationHeaderRewrite -RewriteRule $rewriteRuleSet.RewriteRules
Set-AzApplicationGatewayRequestRoutingRule -ApplicationGateway $appgw -Name rule1 -RuleType $reqRoutingRule.RuleType -BackendHttpSettingsId $reqRoutingRule.BackendHttpSettings.Id -HttpListenerId $reqRoutingRule.HttpListener.Id -BackendAddressPoolId $reqRoutingRule.BackendAddressPool.Id -RewriteRuleSetId $rewriteRuleSet.Id
Set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="delete-a-rewrite-rule"></a>Eliminación de una regla de reescritura

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
Remove-AzApplicationGatewayRewriteRuleSet -Name "LocationHeaderRewrite" -ApplicationGateway $appgw
$requestroutingrule= Get-AzApplicationGatewayRequestRoutingRule -Name "rule1" -ApplicationGateway $appgw
$requestroutingrule.RewriteRuleSet= $null
set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo configurar algunos casos de uso comunes, consulte los [escenarios de reescritura de encabezado común](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).