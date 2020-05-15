---
title: Authentication
description: Explica el funcionamiento de la autenticación
author: florianborn71
ms.author: flborn
ms.date: 02/12/2019
ms.topic: how-to
ms.custom: has-adal-ref
ms.openlocfilehash: 34874e01b526a4d88ff4402edb0c3771c75a3726
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83195300"
---
# <a name="configure-authentication"></a>Configurar la autenticación

Azure Remote Rendering usa el mismo mecanismo de autenticación que [Azure Spatial Anchors (ASA)](https://docs.microsoft.com/azure/spatial-anchors/concepts/authentication?tabs=csharp). Los clientes deben establecer *AccountKey*, *AuthenticationToken* o *AccessToken* para llamar a las API REST correctamente. *AccountKey* se puede obtener en la pestaña "Claves" de la cuenta de Remote Rendering de Azure Portal. *AuthenticationToken* es un token de Azure AD, que se puede obtener mediante la [biblioteca ADAL](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries). *AccessToken* es un token de MR, que se puede obtener del servicio de token de seguridad (STS) de Azure Mixed Reality.

## <a name="authentication-for-deployed-applications"></a>Autenticación para aplicaciones implementadas

 Se recomienda el uso de claves de cuenta para una incorporación rápida, pero solo durante el desarrollo y la creación de prototipos. Se recomienda encarecidamente no enviar la aplicación a producción mediante una clave de cuenta incrustada en ella; en su lugar, use los enfoques de autenticación de Azure AD basados en el usuario o en el servicio.

 La autenticación de Azure AD se describe en la sección [Autenticación de usuario de Azure AD](https://docs.microsoft.com/azure/spatial-anchors/concepts/authentication?tabs=csharp#azure-ad-user-authentication) del servicio [Azure Spatial Anchors (ASA)](https://docs.microsoft.com/azure/spatial-anchors/).

## <a name="role-based-access-control"></a>Control de acceso basado en rol

Para ayudar a controlar el nivel de acceso concedido a las aplicaciones, los servicios o los usuarios de Azure AD de su servicio, se crearon los siguientes roles que puede asignar como considere necesario en las cuentas de Azure Remote Rendering:

* **Administrador de Remote Rendering**: proporciona al usuario funcionalidades de conversión, administración de sesiones, representación y diagnóstico para Azure Remote Rendering.
* **Cliente de Remote Rendering**: proporciona al usuario funcionalidades de administración de sesiones, representación y diagnóstico para Azure Remote Rendering.

## <a name="next-steps"></a>Pasos siguientes

* [Crear una cuenta](create-an-account.md)
* [Uso de las API de front-end de Azure para la autenticación](frontend-apis.md)
* [Scripts de PowerShell de ejemplo](../samples/powershell-example-scripts.md)
