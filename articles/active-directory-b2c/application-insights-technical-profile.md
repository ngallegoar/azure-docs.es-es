---
title: Definición de un perfil técnico de Application Insights en una directiva personalizada
titleSuffix: Azure AD B2C
description: Defina un perfil técnico de Application Insights en una directiva personalizada de Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/20/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 77bb53e2605913fcee6999284acb04616efc53af
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85201419"
---
# <a name="define-an-application-insights-technical-profile-in-an-azure-ad-b2c-custom-policy"></a>Definición de un perfil técnico de Application Insights en una directiva personalizada de Azure AD B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) envía los datos de eventos directamente a [Application Insights](../azure-monitor/app/app-insights-overview.md) mediante la clave de instrumentación proporcionada a Azure AD B2C.  Con un perfil técnico de Application Insights, puede obtener registros de eventos detallados y personalizados para los recorridos del usuario para:

* Obtener información detallada sobre el comportamiento del usuario.
* Solucionar problemas con sus propias directivas durante la fase de desarrollo o producción.
* Medir el rendimiento.
* Crear notificaciones de Application Insights.


## <a name="protocol"></a>Protocolo

El atributo **Name** del elemento **Protocol** tiene que establecerse en `Proprietary`. El atributo **handler** debe contener el nombre completo del ensamblado de controlador de protocolo que usa Azure AD B2C para Application Insights: `Web.TPEngine.Providers.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`

En el ejemplo siguiente se muestra el perfil técnico común de Application Insights. Otros perfiles técnicos de Application Insights incluyen AzureInsights-Common para aprovechar su configuración.  

```xml
<TechnicalProfile Id="AzureInsights-Common">
  <DisplayName>Azure Insights Common</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.Insights.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

## <a name="input-claims"></a>Notificaciones de entrada

El elemento **InputClaims** contiene una lista de notificaciones para enviar a Application Insights. También puede asignar el nombre de la notificación a un nombre que prefiera que aparezca en Application Insights. En el ejemplo siguiente se muestra cómo enviar datos de telemetría a Application Insights. Las propiedades de un evento se agregan a través de la sintaxis `{property:NAME}`, donde NAME es la propiedad que se agrega al evento. DefaultValue puede ser un valor estático o un valor resuelto por uno de los [solucionadores de notificaciones](claim-resolver-overview.md) admitidos.

```xml
<InputClaims>
  <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
  <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:JourneyId}" DefaultValue="{Context:CorrelationId}" />
  <InputClaim ClaimTypeReferenceId="Culture" PartnerClaimType="{property:Culture}" DefaultValue="{Culture:RFC5646}" />
  <InputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="{property:objectId}"  />
</InputClaims>
```

El elemento **InputClaimsTransformations** puede contener una colección de elementos **InputClaimsTransformation** que se usan para modificar las notificaciones de entrada o para generar otras nuevas antes del envío a Application Insights.

## <a name="persist-claims"></a>Conservar las notificaciones

El elemento PersistedClaims no se usa.

## <a name="output-claims"></a>Notificaciones de salida

Los elementos OutputClaims y OutputClaimsTransformations no se utilizan.

## <a name="cryptographic-keys"></a>Claves de cifrado

El elemento CryptographicKeys no se utiliza.


## <a name="metadata"></a>Metadatos

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| InstrumentationKey| Sí | [Clave de instrumentación](../azure-monitor/app/create-new-resource.md#copy-the-instrumentation-key) de Application Insights, que se utilizará para registrar los eventos. | 
| DeveloperMode| No | Valor booleano que indica si el modo de desarrollador está habilitado. Valores posibles: `true` o `false` (valor predeterminado). Estos metadatos controlan cómo se almacenan en búfer los eventos. En un entorno de desarrollo con un volumen de eventos mínimo, al habilitar el modo de desarrollador, los eventos se envían inmediatamente a Application Insights.|  
|DisableTelemetry |No |Valor booleano que indica si la telemetría debe habilitarse o no. Valores posibles: `true` o `false` (valor predeterminado).| 


## <a name="next-steps"></a>Pasos siguientes

- [Creación de recursos en Application Insights](../azure-monitor/app/create-new-resource.md)
- Obtener información sobre el [Seguimiento del comportamiento del usuario en Azure Active Directory B2C mediante Application Insights](analytics-with-application-insights.md)
