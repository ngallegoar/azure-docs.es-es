---
title: Use Microsoft Graph API para configurar el inicio de sesión único basado en SAML
titleSuffix: Azure Active Directory
description: ¿Necesita configurar el inicio de sesión único basado en SAML para múltiples instancias de una aplicación? Aprenda a ahorrar tiempo mediante el uso de Microsoft Graph API para automatizar la configuración del inicio de sesión único basado en SAML.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: how-to
ms.date: 05/19/2020
ms.author: kenwith
ms.reviewer: luleon
ms.openlocfilehash: 50ee9e3c22c885931e2586f65ba2fa3353fccfeb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85355852"
---
# <a name="automate-saml-based-sso-app-configuration-with-microsoft-graph-api"></a>Automatización de la configuración de aplicaciones SSO basadas en SAML con Microsoft Graph API

En este artículo, aprenderá a crear y configurar una aplicación desde la galería de Azure Active Directory (Azure AD). En este artículo se usa AWS como ejemplo, pero puede usar los pasos de este artículo para cualquier aplicación basada en SAML en la galería de Azure AD.

**Pasos para usar Microsoft Graph API con el fin de automatizar la configuración del inicio de sesión único basado en SAML**

| Paso  | Detalles  |
|---------|---------|
| [1. Crear la aplicación de galería](#step-1-create-the-gallery-application) | Iniciar sesión en el cliente API <br> Recuperar la aplicación de galería <br> Crear la aplicación de galería|
| [2. Configurar inicio de sesión único](#step-2-configure-single-sign-on) | Recuperar id. de objeto de aplicación y id. de objeto de entidad de servicio <br> Modo de inicio de sesión único <br> Establecer direcciones URL de SAML básicas como el identificador, la dirección URL de respuesta y la dirección URL de inicio de sesión <br> Agregar roles de aplicación (opcional)|
| [3. Configurar asignación de notificaciones](#step-3-configure-claims-mapping) | Crear una directiva de asignación de notificaciones <br> Asignar la directiva de asignación de notificaciones a la entidad de servicio|
| [4. Creación de certificado de firma](#step-4-configure-signing-certificate) | Crear un certificado <BR> Agregar una clave de firma de personalizada <br> Activar la clave de firma personalizada|
| [5. Asignación de usuarios](#step-5-assign-users) | Asignación de usuarios y grupos a la aplicación
| [6. Configurar el lado de la aplicación](#step-6-configure-the-application-side)| Obtención de los metadatos de SAML de Azure AD

**Lista de todas las API que se usan en la documentación**

Asegúrese de que tiene los permisos correspondientes para llamar a las siguientes API.

|Tipo de recurso |Método |
|---------|---------|
|[applicationTemplate](https://docs.microsoft.com/graph/api/resources/applicationtemplate?view=graph-rest-beta)|[Enumerar applicationTemplate](https://docs.microsoft.com/graph/api/applicationtemplate-list?view=graph-rest-beta&tabs=http) <br>[Crear instancias de applicationTemplate](https://docs.microsoft.com/graph/api/applicationtemplate-instantiate?view=graph-rest-beta&tabs=http)|
|[servicePrincipals](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-1.0)|[Actualizar servicePrincipal](https://docs.microsoft.com/graph/api/serviceprincipal-update?view=graph-rest-1.0&tabs=http) <br> [Crear appRoleAssignments](https://docs.microsoft.com/graph/api/serviceprincipal-post-approleassignments?view=graph-rest-1.0&tabs=http) <br> [Asignar claimsMappingPolicies](https://docs.microsoft.com/graph/api/serviceprincipal-post-claimsmappingpolicies?view=graph-rest-beta&tabs=http)|
|[applications](https://docs.microsoft.com/graph/api/resources/application?view=graph-rest-1.0)|[Actualizar aplicación](https://docs.microsoft.com/graph/api/application-update?view=graph-rest-1.0&tabs=http)|
|[claimsMappingPolicy](https://docs.microsoft.com/graph/api/resources/claimsmappingpolicy?view=graph-rest-beta)| [Crear claimsMappingPolicy](https://docs.microsoft.com/graph/api/claimsmappingpolicy-post-claimsmappingpolicies?view=graph-rest-beta&tabs=http)

>[!NOTE]
>Los objetos de respuesta que se muestran en este artículo pueden estar abreviados para mejorar la legibilidad. Todas las propiedades se devolverán desde una llamada real.

## <a name="step-1-create-the-gallery-application"></a>Paso 1: Crear la aplicación de galería

### <a name="sign-in-to-microsoft-graph-explorer-recommended-postman-or-any-other-api-client-you-use"></a>Iniciar sesión en el Explorador de Microsoft Graph (recomendado), Postman o cualquier otro cliente de API que use

1. Inicie el [Explorador de Microsoft Graph](https://developer.microsoft.com/graph/graph-explorer).
2. Seleccione **Iniciar sesión con Microsoft** e inicie sesión con las credenciales de administrador de la aplicación o de administrador global de Azure AD.
3. Una vez haya iniciado sesión correctamente, verá los detalles de la cuenta de usuario en el panel izquierdo.

### <a name="retrieve-the-gallery-application-template-identifier"></a>Recuperar el identificador de plantilla de la aplicación de galería

Las aplicaciones de la galería de aplicaciones de Azure AD tienen una [plantilla de aplicación](https://docs.microsoft.com/graph/api/applicationtemplate-list?view=graph-rest-beta&tabs=http) cada una, que describe los metadatos de esa aplicación. Con esta plantilla, puede crear una instancia de la aplicación y la entidad de servicio en el inquilino para la administración.

#### <a name="request"></a>Solicitud

<!-- {
  "blockType": "request",
  "name": "get_applicationtemplates"
}-->

```msgraph-interactive
GET https://graph.microsoft.com/beta/applicationTemplates
```

#### <a name="response"></a>Response

<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.applicationTemplate",
  "isCollection": true
} -->

```http
HTTP/1.1 200 OK
Content-type: application/json

{
  "value": [
  {
    "id": "8b1025e4-1dd2-430b-a150-2ef79cd700f5",
        "displayName": "Amazon Web Services (AWS)",
        "homePageUrl": "http://aws.amazon.com/",
        "supportedSingleSignOnModes": [
             "password",
             "saml",
             "external"
         ],
         "supportedProvisioningTypes": [
             "sync"
         ],
         "logoUrl": "https://az495088.vo.msecnd.net/app-logo/aws_215.png",
         "categories": [
             "developerServices"
         ],
         "publisher": "Amazon",
         "description": null    
  
}
```

### <a name="create-the-gallery-application"></a>Crear la aplicación de galería

Use el id. de plantilla recuperado para la aplicación en el último paso, [crear una instancia](https://docs.microsoft.com/graph/api/applicationtemplate-instantiate?view=graph-rest-beta&tabs=http) de la aplicación y la entidad de servicio en el inquilino.

> [!NOTE] 
> Puede usar la API de applicationTemplate para crear instancias de [aplicaciones situadas fuera de la galería](add-non-gallery-app.md). Use el applicationTemplateId `8adf8e6e-67b2-4cf2-a259-e3dc5476c621`.
#### <a name="request"></a>Solicitud

<!-- {
  "blockType": "request",
  "name": "applicationtemplate_instantiate"
}-->

```msgraph-interactive
POST https://graph.microsoft.com/beta/applicationTemplates/8b1025e4-1dd2-430b-a150-2ef79cd700f5/instantiate
Content-type: application/json

{
  "displayName": "AWS Contoso"
}
```

#### <a name="response"></a>Response


<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.applicationServicePrincipal"
} -->

```http
HTTP/1.1 201 OK
Content-type: application/json


{
    "application": {
        "objectId": "cbc071a6-0fa5-4859-8g55-e983ef63df63",
        "appId": "92653dd4-aa3a-3323-80cf-e8cfefcc8d5d",
        "applicationTemplateId": "8b1025e4-1dd2-430b-a150-2ef79cd700f5",
        "displayName": "AWS Contoso",
        "homepage": "https://signin.aws.amazon.com/saml?metadata=aws|ISV9.1|primary|z",
        "replyUrls": [
            "https://signin.aws.amazon.com/saml"
        ],
        "logoutUrl": null,
        "samlMetadataUrl": null,
    },
    "servicePrincipal": {
        "objectId": "f47a6776-bca7-4f2e-bc6c-eec59d058e3e",
        "appDisplayName": "AWS Contoso",
        "applicationTemplateId": "8b1025e4-1dd2-430b-a150-2ef79cd700f5",
        "appRoleAssignmentRequired": true,
        "displayName": "My custom name",
        "homepage": "https://signin.aws.amazon.com/saml?metadata=aws|ISV9.1|primary|z",
        "replyUrls": [
            "https://signin.aws.amazon.com/saml"
        ],
        "servicePrincipalNames": [
            "93653dd4-aa3a-4323-80cf-e8cfefcc8d7d"
        ],
        "tags": [
            "WindowsAzureActiveDirectoryIntegratedApp"
        ],
    }
}
```

## <a name="step-2-configure-single-sign-on"></a>Paso 2: Configurar inicio de sesión único

### <a name="retrieve-app-object-id-and-service-principal-object-id"></a>Recuperar id. de objeto de aplicación y id. de objeto de entidad de servicio

Use la respuesta de la llamada anterior para recuperar y guardar el id. de objeto de la aplicación y el ide. de objeto de la entidad de servicio.

```
"application": {
        "objectId": "cbc071a6-0fa5-4859-8g55-e983ef63df63"
}
"servicePrincipal": {
        "objectId": "f47a6776-bca7-4f2e-bc6c-eec59d058e3e"
}
```
### <a name="set-single-sign-on-mode"></a>Modo de inicio de sesión único

En este ejemplo, establecerá `saml` como el modo de inicio de sesión único en el [tipo de recurso servicePrincipal](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-1.0). Otras propiedades de SSO de SAML que se pueden configurar son: `notificationEmailAddresses`, `loginUrl`y `samlSingleSignOnSettings.relayState`

#### <a name="request"></a>Solicitud

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->

```msgraph-interactive
PATCH https://graph.microsoft.com/v1.0/servicePrincipals/f47a6776-bca7-4f2e-bc6c-eec59d058e3e
Content-type: servicePrincipal/json

{
    "preferredSingleSignOnMode": "saml"
}
```

#### <a name="response"></a>Response

<!-- {
  "blockType": "response",
  "truncated": true,
} -->

```http
HTTP/1.1 204
```

### <a name="set-basic-saml-urls-such-as-identifier-reply-url-sign-on-url"></a>Establecer direcciones URL de SAML básicas como el identificador, la dirección URL de respuesta y la dirección URL de inicio de sesión

Establezca el identificador y las direcciones URL de respuesta para AWS en el objeto de aplicación.

#### <a name="request"></a>Solicitud

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->

```msgraph-interactive
PATCH https://graph.microsoft.com/v1.0/applications/cbc071a6-0fa5-4859-8g55-e983ef63df63
Content-type: applications/json

{
    "web": {
        "redirectUris": [
            "https://signin.aws.amazon.com/saml"
        ] 
    },
    "identifierUris": [
        "https://signin.aws.amazon.com/saml"
    ]    
}
```
#### <a name="response"></a>Response

<!-- {
  "blockType": "response",
  "truncated": true,
} -->

```http
HTTP/1.1 204
```
### <a name="add-app-roles-optional"></a>Agregar roles de aplicación (opcional)

Si la aplicación requiere la información del rol en el token, agregue la definición de los roles en el objeto de aplicación. Para AWS, puede [habilitar el aprovisionamiento de usuarios](../app-provisioning/application-provisioning-configure-api.md) para capturar todos los roles de esa cuenta de AWS. 

Para obtener más información, lea [Configuración de la notificación de rol emitida en el token SAML](../develop/active-directory-enterprise-app-role-management.md)

> [!NOTE] 
> Al agregar roles de aplicación, no modifique los roles de aplicación predeterminados msiam_access. 

#### <a name="request"></a>Solicitud

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->

```msgraph-interactive
PATCH https://graph.microsoft.com/v1.0/serviceprincipals/f47a6776-bca7-4f2e-bc6c-eec59d058e3e
Content-type: serviceprincipals/json

{
"appRoles": [
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "msiam_access",
            "displayName": "msiam_access",
            "id": "7dfd756e-8c27-4472-b2b7-38c17fc5de5e",
            "isEnabled": true,
            "origin": "Application",
            "value": null
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Admin,WAAD",
            "displayName": "Admin,WAAD",
            "id": "454dc4c2-8176-498e-99df-8c4efcde41ef",
            "isEnabled": true,
            "value": "arn:aws:iam::212743507312:role/accountname-aws-admin,arn:aws:iam::212743507312:saml-provider/WAAD"
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Finance,WAAD",
            "displayName": "Finance,WAAD",
            "id": "8642d5fa-18a3-4245-ab8c-a96000c1a217",
            "isEnabled": true,
            "value": "arn:aws:iam::212743507312:role/accountname-aws-finance,arn:aws:iam::212743507312:saml-provider/WAAD"
        }
    ]

}
```

#### <a name="response"></a>Response

<!-- {
  "blockType": "response",
  "truncated": true,
} -->
```http
HTTP/1.1 204
```

## <a name="step-3-configure-claims-mapping"></a>Paso 3: Configurar asignación de notificaciones

### <a name="create-claims-mapping-policy"></a>Crear una directiva de asignación de notificaciones

Además de las notificaciones básicas, configure las siguientes notificaciones para que Azure AD emita en el token SAML:

| Nombre de notificación | Source  |
|---------|---------|
| `https://aws.amazon.com/SAML/Attributes/Role` | assignedroles| 
| `https://aws.amazon.com/SAML/Attributes/RoleSessionName` | userprincipalname |
| `https://aws.amazon.com/SAML/Attributes/SessionDuration` | "900" |
| roles | assignedroles |
| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` | userprincipalname |

Para más información, vea [Personalizar las notificaciones emitidas en tokens](https://docs.microsoft.com/azure/active-directory/develop/active-directory-claims-mapping).

#### <a name="request"></a>Solicitud

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->

```msgraph-interactive
POST https://graph.microsoft.com/v1.0/policies/claimsMappingPolicies
Content-type: claimsMappingPolicies/json

{
    "definition":[
            "{\"ClaimsMappingPolicy\": {
                \"Version\": 1,
                \"IncludeBasicClaimSet\": \"true\",
                \"ClaimsSchema\": [
                    {
                        \"Source\": \"user\",
                        \"ID\": \"assignedroles\",
                        \"SamlClaimType\": \"https://aws.amazon.com/SAML/Attributes/Role\"
                    },
                    {
                        \"Source\": \"user\",
                        \"ID\": \"userprincipalname\",
                        \"SamlClaimType\": \"https://aws.amazon.com/SAML/Attributes/RoleSessionName\"
                    },
                    {
                        \"Source\": \"user\",
                        \"ID\": \"900\",
                        \"SamlClaimType\": \"https://aws.amazon.com/SAML/Attributes/SessionDuration\"
                    },
                    {
                        \"Source\": \"user\",
                        \"ID\": \"assignedroles\",
                        \"SamlClaimType\": \"appRoles\"
                    },
                    {
                        \"Source\": \"user\",
                        \"ID\": \"userprincipalname\",
                        \"SamlClaimType\": \"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier\"
                    }
                ]
            }
        }"

    ],
    "displayName": "AWS Claims policy",
    "isOrganizationDefault": false
}
```

#### <a name="response"></a>Response

<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.claimsMappingPolicies",
  "isCollection": true
} -->

```http
HTTP/1.1 200 OK
Content-type: claimsMappingPolicies/json

{
    "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#policies/claimsMappingPolicies/$entity",
    "id": "6b33aa8e-51f3-41a6-a0fd-d660d276197a",
    "definition": [
        "{\"ClaimsMappingPolicy\": {\"Version\": 1,\"IncludeBasicClaimSet\": \"true\",\"ClaimsSchema\": [{\"Source\": \"user\",\"ID\": \"assignedroles\",\"SamlClaimType\":\"https://aws.amazon.com/SAML/Attributes/Role\"\r\n                    },{\"Source\": \"user\",\"ID\": \"userprincipalname\",\"SamlClaimType\": \"https://aws.amazon.com/SAML/Attributes/RoleSessionName\"},{\"Source\": \"user\",\"ID\": \"900\",\"SamlClaimType\": \"https://aws.amazon.com/SAML/Attributes/SessionDuration\"},{\"Source\": \"user\",\"ID\": \"assignedroles\",\"SamlClaimType\":\"appRoles\"},{\"Source\": \"user\",\"ID\": \"userprincipalname\",\"SamlClaimType\": \"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier\"}]}}"
    ],
    "displayName": "AWS Claims policy",
    "isOrganizationDefault": false
}
```

### <a name="assign-claims-mapping-policy-to-service-principal"></a>Asignar la directiva de asignación de notificaciones a la entidad de servicio

#### <a name="request"></a>Solicitud

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->
```msgraph-interactive
POST https://graph.microsoft.com/beta/servicePrincipals/f47a6776-bca7-4f2e-bc6c-eec59d058e3e/claimsMappingPolicies/$ref

Content-type: claimsMappingPolicies/json

{
  "@odata.id":"https://graph.microsoft.com/v1.0/policies/claimsMappingPolicies/6b33aa8e-51f3-41a6-a0fd-d660d276197a"
}
```

#### <a name="response"></a>Response

<!-- {
  "blockType": "response",
  "truncated": true,
} -->
```http
HTTP/1.1 204
```

## <a name="step-4-configure-signing-certificate"></a>Paso 4: Configuración de certificado de firma

El uso de la API de [applicationTemplate](https://docs.microsoft.com/graph/api/resources/applicationtemplate?view=graph-rest-beta) no crea un certificado de firma de forma predeterminada. Cree el certificado de firma personalizado y asígnelo a la aplicación. 

### <a name="create-a-custom-signing-certificate"></a>Crear un certificado de firma personalizado

Para probar, puede usar el siguiente comando de PowerShell para obtener un certificado autofirmado. Use la mejor práctica de seguridad de su empresa para crear un certificado de firma para producción.

```powershell
Param(
    [Parameter(Mandatory=$true)]
    [string]$fqdn,
    [Parameter(Mandatory=$true)]
    [string]$pwd,
    [Parameter(Mandatory=$true)]
    [string]$location
) 

if (!$PSBoundParameters.ContainsKey('location'))
{
    $location = "."
} 

$cert = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -DnsName $fqdn
$pwdSecure = ConvertTo-SecureString -String $pwd -Force -AsPlainText
$path = 'cert:\currentuser\my\' + $cert.Thumbprint
$cerFile = $location + "\\" + $fqdn + ".cer"
$pfxFile = $location + "\\" + $fqdn + ".pfx" 

Export-PfxCertificate -cert $path -FilePath $pfxFile -Password $pwdSecure
Export-Certificate -cert $path -FilePath $cerFile
```

### <a name="add-a-custom-signing-key"></a>Agregar una clave de firma de personalizada

Agregue la siguiente información a la entidad de servicio:

* Clave privada
* Contraseña
* Clave pública 

Extraiga la clave privada y pública codificada en Base64 del archivo PFX. Para obtener más información sobre las propiedades, lea [Tipo de recurso keyCredential](https://docs.microsoft.com/graph/api/resources/keycredential?view=graph-rest-1.0).

Asegúrese de que el id. de la clave que se usa para "Iniciar sesión" en keyCredential coincide con el id. de la clave de passwordCredential.

Puede generar el `customkeyIdentifier` obteniendo el hash de la huella digital del certificado.

```csharp
  public string GetSha256FromThumbprint(string thumbprint)
  {
      var message = Encoding.ASCII.GetBytes(thumbprint);
      SHA256Managed hashString = new SHA256Managed();
      return Convert.ToBase64String(hashString.ComputeHash(message));
  }
```

#### <a name="request"></a>Solicitud

> [!NOTE]
> El valor "clave" de la propiedad keyCredentials se acorta para mejorar la legibilidad. El valor está codificado en base 64. En la clave privada, la propiedad `usage` es "Sign". En la clave pública, la propiedad `usage` es "Verify".

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->
```msgraph-interactive
PATCH https://graph.microsoft.com/v1.0/servicePrincipals/f47a6776-bca7-4f2e-bc6c-eec59d058e3e

Content-type: servicePrincipals/json

{
    "keyCredentials":[
        {
            "customKeyIdentifier": "lY85bR8r6yWTW6jnciNEONwlVhDyiQjdVLgPDnkI5mA=",
            "endDateTime": "2021-04-22T22:10:13Z",
            "keyId": "4c266507-3e74-4b91-aeba-18a25b450f6e",
            "startDateTime": "2020-04-22T21:50:13Z",
            "type": "AsymmetricX509Cert",
            "usage": "Sign",
            "key":"MIIKIAIBAz.....HBgUrDgMCERE20nuTptI9MEFCh2Ih2jaaLZBZGeZBRFVNXeZmAAgIH0A==",
            "displayName": "CN=awsAPI"
        },
        {
            "customKeyIdentifier": "lY85bR8r6yWTW6jnciNEONwlVhDyiQjdVLgPDnkI5mA=",
            "endDateTime": "2021-04-22T22:10:13Z",
            "keyId": "e35a7d11-fef0-49ad-9f3e-aacbe0a42c42",
            "startDateTime": "2020-04-22T21:50:13Z",
            "type": "AsymmetricX509Cert",
            "usage": "Verify",
            "key": "MIIDJzCCAg+gAw......CTxQvJ/zN3bafeesMSueR83hlCSyg==",
            "displayName": "CN=awsAPI"
        }

    ],
    "passwordCredentials": [
        {
            "customKeyIdentifier": "lY85bR8r6yWTW6jnciNEONwlVhDyiQjdVLgPDnkI5mA=",
            "keyId": "4c266507-3e74-4b91-aeba-18a25b450f6e",
            "endDateTime": "2022-01-27T19:40:33Z",
            "startDateTime": "2020-04-20T19:40:33Z",
            "secretText": "61891f4ee44d"
        }
    ]
}
```

#### <a name="response"></a>Response

<!-- {
  "blockType": "response",
  "truncated": true,
} -->
```http
HTTP/1.1 204
```

### <a name="activate-the-custom-signing-key"></a>Activar la clave de firma personalizada

Debe establecer la propiedad `preferredTokenSigningKeyThumbprint` en la huella digital del certificado que desea que Azure AD use para firmar la respuesta SAML. 

#### <a name="request"></a>Solicitud

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->
```msgraph-interactive
PATCH https://graph.microsoft.com/v1.0/servicePrincipals/f47a6776-bca7-4f2e-bc6c-eec59d058e3e

Content-type: servicePrincipals/json

{
    "preferredTokenSigningKeyThumbprint": "AC09FEF18DDE6983EE2A164FBA3C4DD7518BD787"
}
```

#### <a name="response"></a>Response

<!-- {
  "blockType": "response",
  "truncated": true,
} -->
```http
HTTP/1.1 204
```
## <a name="step-5-assign-users"></a>Paso 5: Asignar usuarios

### <a name="assign-users-and-groups-to-the-application"></a>Asignación de usuarios y grupos a la aplicación

Asigne el siguiente usuario a la entidad de servicio y asigne el AWS_Role1. 

| Nombre  | ID  |
|---------|---------|
| Id. de usuario (id. principal) | 6cad4079-4e79-4a3f-9efb-ea30a14bdb26 |
| Tipo (principalType) | Usuario |
| Id. del rol de aplicación (id. de appRole) | 454dc4c2-8176-498e-99df-8c4efcde41ef |
| Id. de ServicePrincipalId (id. de recurso) | 515f62cb-d18a-4dca-bec3-bb0bf31deeea |

#### <a name="request"></a>Solicitud

<!-- {
  "blockType": "request",
  "name": "servicePrincipals"
}-->
```msgraph-interactive
POST https://graph.microsoft.com/v1.0/servicePrincipals/f47a6776-bca7-4f2e-bc6c-eec59d058e3e/appRoleAssignments

Content-type: appRoleAssignments/json

{
  "principalId": "6cad4079-4e79-4a3f-9efb-ea30a14bdb26",
  "principalType": "User",
  "appRoleId":"454dc4c2-8176-498e-99df-8c4efcde41ef",
  "resourceId":"515f62cb-d18a-4dca-bec3-bb0bf31deeea"
}
```
#### <a name="response"></a>Response

<!-- {
  "blockType": "response",
  "truncated": true,
} -->
```http
HTTP/1.1 201 
Content-type: appRoleAssignments/json

{
    "id": "rq7hyzl4yECaNZleMrTpDV-OCe5TEl5Ao_o76XMrRFU",
    "creationTimestamp": "2020-04-23T17:38:13.2508567Z",
    "appRoleId": "454dc4c2-8176-498e-99df-8c4efcde41ef",
    "principalDisplayName": "User 1",
    "principalId": "6cad4079-4e79-4a3f-9efb-ea30a14bdb26",
    "principalType": "User",
    "resourceDisplayName": "AWS API Created",
    "resourceId": "515f62cb-d18a-4dca-bec3-bb0bf31deeea"
}
```

Para obtener más información, vea el tipo de recurso [appRoleAssignment](https://docs.microsoft.com/graph/api/resources/approleassignment?view=graph-rest-1.0).

## <a name="step-6-configure-the-application-side"></a>Paso 6: Configurar el lado de la aplicación

### <a name="get-azure-ad-saml-metadata"></a>Obtención de los metadatos de SAML de Azure AD

Use la siguiente dirección URL para obtener los metadatos de Azure AD de SAML para la aplicación configurada concreta. Los metadatos contienen información como el certificado de firma, id. de entidad de Azure AD y SingleSignOnService de Azure AD, entre otros.

https://login.microsoftonline.com/{tenant-id}/federationmetadata/2007-06/federationmetadata.xml?appid={app-id}

## <a name="next-steps"></a>Pasos siguientes
- [Use Microsoft Graph Api para configurar el aprovisionamiento del usuario](../app-provisioning/application-provisioning-configure-api.md)
- [Use el informe de actividades de aplicaciones de AD FS para migrar aplicaciones a Azure AD](migrate-adfs-application-activity.md)
