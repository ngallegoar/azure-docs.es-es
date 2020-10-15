---
title: Proteger los servicios de back-end con la autenticación de certificados de cliente
titleSuffix: Azure API Management
description: Averigüe cómo asegurar servicios back-end con la autenticación de certificados de cliente en Azure API Management
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/08/2020
ms.author: apimpm
ms.openlocfilehash: 419de27ad87b113de62dacb0dc384702420afbd6
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92071122"
---
# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Cómo asegurar servicios back-end con la autenticación de certificados de cliente en Azure API Management

API Management permite acceder de forma segura al servicio back-end de una API con certificados de cliente. Esta guía muestra cómo administrar certificados del servicio Azure API Management en Azure Portal. También se explica cómo configurar una API para que use un certificado para acceder a un servicio back-end.

Para obtener más información sobre cómo administrar certificados con la API de REST de API Management, consulte <a href="/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-certificate-entity">Entidad de certificado de la API REST de Azure API Management</a>.

## <a name="prerequisites"></a><a name="prerequisites"> </a>Requisitos previos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Esta guía muestra cómo configurar la instancia de servicio de API Management para acceder al servicio back-end de una API con la autenticación de certificados de cliente. Antes de seguir los pasos de este artículo, debe tener el servicio back-end configurado para la autenticación de certificados de cliente ([para configurar la autenticación de certificados en Azure App Service, consulte este artículo][to configure certificate authentication in Azure WebSites refer to this article]). Debe acceder al certificado y a la contraseña para cargarlos al servicio API Management.

## <a name="upload-a-certificate"></a><a name="step1"> </a>Carga de un certificado

> [!NOTE]
> En lugar de un certificado cargado, puede usar un certificado almacenado en el servicio [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) como se muestra en este [ejemplo](https://github.com/galiniliev/api-management-policy-snippets/blob/galin/AkvCert/examples/Look%20up%20Key%20Vault%20certificate%20using%20Managed%20Service%20Identity%20and%20call%20backend.policy.xml).

![Incorporación de certificados de cliente](media/api-management-howto-mutual-certificates/apim-client-cert-new.png)

Siga los pasos que se describen a continuación para cargar un nuevo certificado de cliente. Si todavía no ha creado una instancia del servicio API Management, consulte el tutorial [Creación de una instancia del servicio API Management][Create an API Management service instance].

1. Vaya a la instancia del servicio Azure API Management en Azure Portal.
2. Seleccione **Certificados** en el menú.
3. Haga clic en el botón **+ Agregar**.
    ![Incorporación de certificados de cliente](media/api-management-howto-mutual-certificates/apim-client-cert-add.png)
4. Busque el certificado y proporcione su identificador y la contraseña.
5. Haga clic en **Crear**.

> [!NOTE]
> El certificado debe estar en formato **.pfx** . Se admiten los certificados autofirmados.

Una vez cargado el certificado, se muestra en **Certificados**.  Si tiene muchos certificados, tome nota de la huella digital del certificado deseado con el fin de [configurar una API para realizar la autenticación de puerta de enlace con un certificado de cliente][Configure an API to use a client certificate for gateway authentication].

> [!NOTE]
> Para desactivar la validación de la cadena de certificados cuando se utiliza, por ejemplo, un certificado autofirmado, siga los pasos descritos en esta [sección](api-management-faq.md#can-i-use-a-self-signed-tlsssl-certificate-for-a-back-end) de preguntas más frecuentes.

## <a name="delete-a-client-certificate"></a><a name="step1a"> </a>Eliminar un certificado de cliente

Para eliminar un certificado, haga clic en el menú contextual **...** y seleccione **Eliminar** junto a este.

![Eliminación de certificados de cliente](media/api-management-howto-mutual-certificates/apim-client-cert-delete-new.png)

Si alguna API está usando el certificado, aparecerá una pantalla de advertencia. Para eliminar el certificado, primero quítelo de todas las API que se hayan configurado para usarlo.

![Error al eliminar certificados de cliente](media/api-management-howto-mutual-certificates/apim-client-cert-delete-failure.png)

## <a name="configure-an-api-to-use-a-client-certificate-for-gateway-authentication"></a><a name="step2"> </a>Configurar una API para realizar la autenticación de puerta de enlace con un certificado de cliente

1. Haga clic en **API** en el menú **API Management** de la izquierda y vaya a la API.
    ![Habilitar certificados de cliente](media/api-management-howto-mutual-certificates/apim-client-cert-enable.png)

2. En la pestaña **Diseño**, haga clic en un icono de lápiz de la sección **Backend**.
3. Cambie las **credenciales de puerta de enlace** a **certificado de cliente** y seleccione el certificado en la lista desplegable.
    ![Habilitar certificados de cliente](media/api-management-howto-mutual-certificates/apim-client-cert-enable-select.png)

4. Haga clic en **Save**(Guardar).

> [!WARNING]
> Este cambio se hace efectivo de forma inmediata y llama a las operaciones de la API que realizarán la autenticación en el servidor back-end con el certificado.


> [!TIP]
> Cuando se especifica un certificado para la autenticación de puerta de enlace del servicio back-end de una API, el certificado se integra en la directiva de dicha API y puede verse en el editor de directivas.

## <a name="self-signed-certificates"></a>Certificados autofirmados

Si utiliza certificados autofirmados, tendrá que deshabilitar la validación de la cadena de certificados para que API Management pueda comunicarse con el sistema back-end. De lo contrario, se producirá un error con el código 500. Para establecer esta configuración, puede utilizar los cmdlets de PowerShell [`New-AzApiManagementBackend`](/powershell/module/az.apimanagement/new-azapimanagementbackend) (con el nuevo back-end) o [`Set-AzApiManagementBackend`](/powershell/module/az.apimanagement/set-azapimanagementbackend) (con el back-end existente) y establecer el parámetro `-SkipCertificateChainValidation` en `True`.

```powershell
$context = New-AzApiManagementContext -resourcegroup 'ContosoResourceGroup' -servicename 'ContosoAPIMService'
New-AzApiManagementBackend -Context  $context -Url 'https://contoso.com/myapi' -Protocol http -SkipCertificateChainValidation $true
```

[How to add operations to an API]: ./mock-api-responses.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: get-started-create-service-instance.md
[API Management policy reference]: ./api-management-policies.md
[Caching policies]: ./api-management-policies.md#caching-policies
[Create an API Management service instance]: get-started-create-service-instance.md

[Azure API Management REST API Certificate entity]: ./api-management-caching-policies.md
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[to configure certificate authentication in Azure WebSites refer to this article]: ../app-service/app-service-web-configure-tls-mutual-auth.md

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Configure an API to use a client certificate for gateway authentication]: #step2
[Test the configuration by calling an operation in the Developer Portal]: #step3
[Next steps]: #next-steps