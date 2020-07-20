---
title: Authentication
description: Explica el funcionamiento de la autenticación
author: florianborn71
ms.author: flborn
ms.date: 06/15/2020
ms.topic: how-to
ms.openlocfilehash: 8f3b144a7790c3122d59d27183b3037998ddadd1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85565848"
---
# <a name="configure-authentication"></a>Configurar la autenticación

Azure Remote Rendering usa el mismo mecanismo de autenticación que [Azure Spatial Anchors (ASA)](https://docs.microsoft.com/azure/spatial-anchors/concepts/authentication?tabs=csharp). Los clientes deben establecer *una* de las siguientes para llamar a las API de REST correctamente:

* **AccountKey**: se puede obtener en la pestaña "Claves" de la cuenta de Remote Rendering de Azure Portal. Las claves de cuenta solo se recomiendan para el desarrollo o la creación de prototipos.
    ![Id. de cuenta](./media/azure-account-primary-key.png)

* **AuthenticationToken** es un token de Azure AD, que se puede obtener mediante la [biblioteca MSAL](https://docs.microsoft.com/azure/active-directory/develop/msal-overview). Hay varios flujos diferentes disponibles para aceptar las credenciales de usuario y usar esas credenciales para obtener un token de acceso.

* **MRAccessToken** es un token de MR, que se puede obtener del servicio de token de seguridad (STS) de Azure Mixed Reality. Recuperado del punto de conexión `https://sts.mixedreality.azure.com` mediante una llamada REST similar a la llamada siguiente:

    ```rest
    GET https://sts.mixedreality.azure.com/Accounts/35d830cb-f062-4062-9792-d6316039df56/token HTTP/1.1
    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni<truncated>FL8Hq5aaOqZQnJr1koaQ
    Host: sts.mixedreality.azure.com
    Connection: Keep-Alive

    HTTP/1.1 200 OK
    Date: Tue, 24 Mar 2020 09:09:00 GMT
    Content-Type: application/json; charset=utf-8
    Content-Length: 1153
    Accept: application/json
    MS-CV: 05JLqWeKFkWpbdY944yl7A.0
    {"AccessToken":"eyJhbGciOiJSUzI1<truncated>uLkO2FvA"}
    ```

    El encabezado de autorización tiene el formato siguiente: `Bearer <Azure_AD_token>` o `Bearer <accoundId>:<accountKey>`. Es preferible el primero por seguridad. El token que devuelve esta llamada REST es el token de acceso de MR.

## <a name="authentication-for-deployed-applications"></a>Autenticación para aplicaciones implementadas

Se recomiendan las claves de cuenta para la creación rápida de prototipos solo durante la fase de desarrollo. Se recomienda no enviar su aplicación a producción con una clave de cuenta insertada en ella. El enfoque recomendado es usar un enfoque de autenticación de Azure AD basado en el usuario o en el servicio.

 La autenticación de Azure AD se describe en la sección [Autenticación de usuario de Azure AD](https://docs.microsoft.com/azure/spatial-anchors/concepts/authentication?tabs=csharp#azure-ad-user-authentication) del servicio [Azure Spatial Anchors (ASA)](https://docs.microsoft.com/azure/spatial-anchors/).

 Para más información, consulte [Tutorial: Protección de Azure Remote Rendering y el almacenamiento de modelos: Autenticación con Azure Active Directory](../tutorials/unity/security/security.md#azure-active-directory-azure-ad-authentication)

## <a name="role-based-access-control"></a>Control de acceso basado en rol

Para ayudar a controlar el nivel de acceso concedido al servicio, use los siguientes roles al conceder el acceso basado en roles:

* **Administrador de Remote Rendering**: proporciona al usuario funcionalidades de conversión, administración de sesiones, representación y diagnóstico para Azure Remote Rendering.
* **Cliente de Remote Rendering**: proporciona al usuario funcionalidades de administración de sesiones, representación y diagnóstico para Azure Remote Rendering.

## <a name="next-steps"></a>Pasos siguientes

* [Crear una cuenta](create-an-account.md)
* [Uso de las API de front-end de Azure para la autenticación](frontend-apis.md)
* [Scripts de PowerShell de ejemplo](../samples/powershell-example-scripts.md)
