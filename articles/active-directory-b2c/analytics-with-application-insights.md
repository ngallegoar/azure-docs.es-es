---
title: Seguimiento del comportamiento del usuario con Application Insights
titleSuffix: Azure AD B2C
description: Aprenda a habilitar registros de eventos en Application Insights a partir de recorridos del usuario de Azure AD B2C mediante directivas personalizadas.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 04/05/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 25e62e7c6865f91daa242a33a0f491f8015be41a
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80672534"
---
# <a name="track-user-behavior-in-azure-active-directory-b2c-using-application-insights"></a>Seguimiento del comportamiento del usuario en Azure Active Directory B2C mediante Application Insights

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure Active Directory B2C (Azure AD B2C) envía los datos de eventos directamente a [Application Insights](../azure-monitor/app/app-insights-overview.md) mediante la clave de instrumentación proporcionada a Azure AD B2C.  Con un perfil técnico de Application Insights, puede obtener registros de eventos detallados y personalizados para los recorridos del usuario para:

* Obtener información detallada sobre el comportamiento del usuario.
* Solucionar problemas con sus propias directivas durante la fase de desarrollo o producción.
* Medir el rendimiento.
* Crear notificaciones de Application Insights.

## <a name="how-it-works"></a>Funcionamiento

En el perfil técnico de [Application Insights](application-insights-technical-profile.md) se define un evento de Azure AD B2C. El perfil especifica el nombre del evento, las notificaciones que se registran y la clave de instrumentación. Para publicar un evento, el perfil técnico se agrega entonces como un paso de orquestación en un [recorrido del usuario](userjourneys.md).

Application Insights puede unificar los eventos por medio de un identificador de correlación para registrar una sesión de usuario. Application Insights permite que el evento y la sesión estén disponibles en cuestión de segundos y presenta muchas herramientas de visualización, exportación y análisis.

## <a name="prerequisites"></a>Prerrequisitos

Realice los pasos del artículo [Introducción a las directivas personalizadas](custom-policy-get-started.md). Debe tener una directiva personalizada activa para registrar e iniciar sesión de cuentas locales.

## <a name="create-an-application-insights-resource"></a>Creación de recursos en Application Insights

Cuando usa Application Insights con Azure AD B2C, todo lo que necesita hacer es crear un recurso y obtener la clave de instrumentación. Para obtener más información, vea [Creación de recursos en Application Insights](../azure-monitor/app/create-new-resource.md)

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Asegúrese de usar el directorio que contiene la suscripción de Azure. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y elija el directorio que contiene la suscripción. Este inquilino no es el inquilino de Azure AD B2C.
3. Elija **Crear un recurso** en la esquina superior izquierda de Azure Portal y, a continuación, busque y seleccione **Application Insights**.
4. Haga clic en **Crear**.
5. En **Nombre**, escriba un nombre para el recurso.
6. En **Tipo de aplicación**, seleccione **Aplicación web ASP.NET**.
7. En **Grupo de recursos**, seleccione un grupo existente o escriba un nombre para un nuevo grupo.
8. Haga clic en **Crear**.
4. Después de crear el recurso de Application Insights, ábralo, expanda **Información esencial** y copie la clave de instrumentación.

![Introducción a Application Insights y la clave de instrumentación](./media/analytics-with-application-insights/app-insights.png)

## <a name="define-claims"></a>Definición de notificaciones

Una notificación proporciona un almacenamiento temporal de datos durante la ejecución de una directiva de Azure AD B2C. El [esquema de notificaciones](claimsschema.md) es el lugar en el que se declaran las notificaciones.

1. Abra el archivo de extensiones de la directiva. Por ejemplo, <em>`SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`**</em>.
1. Busque el elemento [BuildingBlocks](buildingblocks.md). Si el elemento no existe, agréguelo.
1. Busque el elemento [ClaimsSchema](claimsschema.md). Si el elemento no existe, agréguelo.
1. Agregue las notificaciones siguientes al elemento **ClaimsSchema**. 

```xml
<ClaimType Id="EventType">
  <DisplayName>Event type</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="EventTimestamp">
  <DisplayName>Event timestamp</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="PolicyId">
  <DisplayName>Policy Id</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="Culture">
  <DisplayName>Culture ID</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="CorrelationId">
  <DisplayName>Correlation Id</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="federatedUser">
  <DisplayName>Federated user</DisplayName>
  <DataType>boolean</DataType>
</ClaimType>
<ClaimType Id="parsedDomain">
  <DisplayName>Domain name</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>The domain portion of the email address.</UserHelpText>
</ClaimType>
<ClaimType Id="userInLocalDirectory">
  <DisplayName>userInLocalDirectory</DisplayName>
  <DataType>boolean</DataType>
</ClaimType>
```

## <a name="add-new-technical-profiles"></a>Incorporación de nuevos perfiles técnicos

Los perfiles técnicos pueden considerarse funciones en el Marco de experiencia de identidad de Azure AD B2C. En esta tabla se definen los perfiles técnicos que se usan para abrir una sesión y publicar eventos.

| Perfil técnico | Tarea |
| ----------------- | -----|
| AppInsights-Common | El conjunto común de parámetros que se incluirán en todos los perfiles técnicos de Azure Insights. |
| AppInsights-SignInRequest | Registra un `SignInRequest` evento con un conjunto de notificaciones cuando se ha recibido una solicitud de inicio de sesión. |
| AppInsights-UserSignUp | Registra un evento `UserSignUp` cuando el usuario activa la opción de inicio de sesión en un recorrido de inicio de sesión y registro. |
| AppInsights-SignInComplete | Registra un `SignInComplete` evento de finalización correcta de una autenticación cuando se ha enviado un token a la aplicación de usuario de confianza. |

Agregue los perfiles al archivo *TrustFrameworkExtensions.xml* desde el módulo de inicio. Agregue estos elementos al elemento **ClaimsProviders**:

```xml
<ClaimsProvider>
  <DisplayName>Application Insights</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="AppInsights-Common">
      <DisplayName>Application Insights</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.Insights.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <!-- The ApplicationInsights instrumentation key which will be used for logging the events -->
        <Item Key="InstrumentationKey">xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</Item>
        <Item Key="DeveloperMode">false</Item>
        <Item Key="DisableTelemetry ">false</Item>
      </Metadata>
      <InputClaims>
        <!-- Properties of an event are added through the syntax {property:NAME}, where NAME is property being added to the event. DefaultValue can be either a static value or a value that's resolved by one of the supported DefaultClaimResolvers. -->
        <InputClaim ClaimTypeReferenceId="EventTimestamp" PartnerClaimType="{property:EventTimestamp}" DefaultValue="{Context:DateTimeInUtc}" />
        <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
        <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:CorrelationId}" DefaultValue="{Context:CorrelationId}" />
        <InputClaim ClaimTypeReferenceId="Culture" PartnerClaimType="{property:Culture}" DefaultValue="{Culture:RFC5646}" />
      </InputClaims>
    </TechnicalProfile>

    <TechnicalProfile Id="AppInsights-SignInRequest">
      <InputClaims>
        <!-- An input claim with a PartnerClaimType="eventName" is required. This is used by the AzureApplicationInsightsProvider to create an event with the specified value. -->
        <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="SignInRequest" />
      </InputClaims>
      <IncludeTechnicalProfile ReferenceId="AppInsights-Common" />
    </TechnicalProfile>

    <TechnicalProfile Id="AppInsights-UserSignUp">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="UserSignUp" />
      </InputClaims>
      <IncludeTechnicalProfile ReferenceId="AppInsights-Common" />
    </TechnicalProfile>
    
    <TechnicalProfile Id="AppInsights-SignInComplete">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="SignInComplete" />
        <InputClaim ClaimTypeReferenceId="federatedUser" PartnerClaimType="{property:FederatedUser}" DefaultValue="false" />
        <InputClaim ClaimTypeReferenceId="parsedDomain" PartnerClaimType="{property:FederationPartner}" DefaultValue="Not Applicable" />
      </InputClaims>
      <IncludeTechnicalProfile ReferenceId="AppInsights-Common" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

> [!IMPORTANT]
> Cambie la clave de instrumentación del perfil técnico de `AppInsights-Common` por el GUID que su recurso de Application Insights proporciona.

## <a name="add-the-technical-profiles-as-orchestration-steps"></a>Incorporación de los perfiles técnicos como pasos de orquestación

Llame a `AppInsights-SignInRequest` como paso de orquestación 2 para realizar el seguimiento de que se ha recibido una solicitud de inicio de sesión o suscripción:

```xml
<!-- Track that we have received a sign in request -->
<OrchestrationStep Order="1" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInRequest" TechnicalProfileReferenceId="AppInsights-SignInRequest" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Inmediatamente *antes* del paso de orquestación `SendClaims`, agregue un nuevo paso que llame a `AppInsights-UserSignup`. Se desencadena cuando el usuario selecciona el botón de suscripción en un recorrido de inicio de sesión o suscripción.

```xml
<!-- Handles the user clicking the sign up link in the local account sign in page -->
<OrchestrationStep Order="8" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
      <Value>newUser</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
    <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
      <Value>newUser</Value>
      <Value>false</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackUserSignUp" TechnicalProfileReferenceId="AppInsights-UserSignup" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Inmediatamente después del paso de orquestación `SendClaims`, llame a `AppInsights-SignInComplete`. Este paso muestra un recorrido completado correctamente.

```xml
<!-- Track that we have successfully sent a token -->
<OrchestrationStep Order="10" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInComplete" TechnicalProfileReferenceId="AppInsights-SignInComplete" />
  </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> Después de agregar los nuevos pasos de orquestación, vuelva a enumerar los pasos de forma secuencial sin omitir los enteros de 1 a N.


## <a name="upload-your-file-run-the-policy-and-view-events"></a>Carga del archivo, ejecución de la directiva y visualización de eventos

Cargue el archivo *TrustFrameworkExtensions.xml*. Luego, llame a la directiva de usuario de confianza desde su aplicación o use **Ejecutar ahora** en Azure Portal. En segundos, los eventos están disponibles en Application Insights.

1. Abra el recurso de **Application Insights** en su inquilino de Azure Active Directory.
2. Seleccione **Uso** > **Eventos**.
3. Establezca **During** (Durante) en **Last hour** (Última hora) y **By** (por) en **3 minutes** (3 minutos).  Puede que tenga que seleccionar **Refresh** (Actualizar) para ver los resultados.

![Hoja de eventos de uso de Application Insights](./media/analytics-with-application-insights/app-ins-graphic.png)

## <a name="optional-collect-more-data"></a>[Opcional] Recopilar más datos

Agregue tipos de notificación y eventos a su recorrido del usuario según sus necesidades. Puede usar [solucionadores de notificaciones](claim-resolver-overview.md) o cualquier tipo de notificación de cadena y agregar las notificaciones mediante la incorporación de un elemento de **Notificación de entrada** al evento de Application Insights o al perfil técnico AzureInsights-Common.

- **ClaimTypeReferenceId** es la referencia a un tipo de notificación.
- **PartnerClaimType** es el nombre de la propiedad que aparece en Azure Insights. Use la sintaxis de `{property:NAME}`, donde `NAME` es la propiedad que se agrega al evento.
- **DefaultValue** usa cualquier valor de cadena o el solucionador de notificaciones.

```XML
<InputClaim ClaimTypeReferenceId="app_session" PartnerClaimType="{property:app_session}" DefaultValue="{OAUTH-KV:app_session}" />
<InputClaim ClaimTypeReferenceId="loyalty_number" PartnerClaimType="{property:loyalty_number}" DefaultValue="{OAUTH-KV:loyalty_number}" />
<InputClaim ClaimTypeReferenceId="language" PartnerClaimType="{property:language}" DefaultValue="{Culture:RFC5646}" />
```

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre el perfil técnico de [Application Insights](application-insights-technical-profile.md) en la referencia de IEF. 
