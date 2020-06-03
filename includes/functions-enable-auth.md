---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/21/2020
ms.author: glenga
ms.openlocfilehash: 380878fedaa2f7ea6160c3c4801c8dece6e1a9ff
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83648973"
---
#### <a name="enable-app-service-authenticationauthorization"></a>Habilitación de la autenticación o la autorización de App Service

La plataforma App Service le permite usar Azure Active Directory (AAD) y varios proveedores de identidades de terceros para autenticar a los clientes. Se puede usar esta estrategia para implementar reglas de autorización personalizadas para las funciones y permite trabajar con información de usuario desde el código de función. Para obtener más información, vea [Autenticación y autorización en Azure App Service](../articles/app-service/overview-authentication-authorization.md) y [Uso de identidades de cliente](../articles/azure-functions/functions-bindings-http-webhook-trigger.md#working-with-client-identities).

#### <a name="use-azure-api-management-apim-to-authenticate-requests"></a>Uso de Azure API Management (APIM) para autenticar solicitudes

APIM proporciona una serie de opciones de seguridad de API para las solicitudes entrantes. Para obtener más información, vea [Directivas de autenticación de Azure API Management](../articles/api-management/api-management-authentication-policies.md). Con APIM, puede configurar la aplicación de función de modo que solo acepte solicitudes de la dirección IP de la instancia de APIM. Para obtener más información, vea [Restricciones de las direcciones IP](../articles/azure-functions/ip-addresses.md#ip-address-restrictions).
