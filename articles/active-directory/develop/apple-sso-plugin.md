---
title: Complemento de Microsoft Enterprise SSO para dispositivos Apple
titleSuffix: Microsoft identity platform | Azure
description: Obtenga información sobre el complemento de SSO de Microsoft Azure Active Directory para dispositivos iOS y macOS.
services: active-directory
author: brandwe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 03/31/2020
ms.author: brandwe
ms.reviewer: brandwe
ms.custom: aaddev
ms.openlocfilehash: a6af1a78582ebf62aed82bad8fa06cac37f511ea
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2020
ms.locfileid: "80982589"
---
# <a name="microsoft-enterprise-sso-plug-in-for-apple-devices-preview"></a>Complemento de Microsoft Enterprise SSO para dispositivos Apple (versión preliminar)

> [!NOTE]
> Esta característica está en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

El *complemento de Microsoft Enterprise SSO para dispositivos de Apple* proporciona cuentas de inicio de sesión único (SSO) para Azure Active Directory (Azure AD) en todas las aplicaciones que admiten la característica [Enterprise Single Sign-On](https://developer.apple.com/documentation/authenticationservices) de Apple. Microsoft ha trabajado estrechamente con Apple para desarrollar este complemento con el fin de aumentar la facilidad de uso de su aplicación y ofrecer la máxima protección de la que disponen Apple y Microsoft.

En esta versión preliminar pública, el complemento Enterprise SSO solo está disponible para dispositivos iOS y se distribuye en determinadas aplicaciones de Microsoft.

Nuestro primer uso del complemento Enterprise SSO es con la nueva característica [Modo de dispositivo compartido](msal-ios-shared-devices.md).

## <a name="features"></a>Características

El complemento Microsoft Enterprise SSO para dispositivos de Apple ofrece las siguientes ventajas:

- Proporciona SSO para las cuentas de Azure AD en todas las aplicaciones que admiten la característica Enterprise Single Sign-On de Apple.
- Se entrega automáticamente en Microsoft Authenticator y se puede habilitar mediante cualquier solución de administración de dispositivos móviles (MDM).

## <a name="requirements"></a>Requisitos

Para usar el complemento de Microsoft Enterprise SSO para dispositivos de Apple:

- iOS 13.0 o posterior debe estar instalado en el dispositivo.
- Debe instalarse una aplicación de Microsoft que proporcione el complemento Microsoft Enterprise SSO para dispositivos de Apple en el dispositivo. En el caso de la versión preliminar pública, estas aplicaciones incluyen la [aplicación Microsoft Authenticator](../user-help/user-help-auth-app-overview.md).
- El dispositivo debe estar inscrito en MDM (por ejemplo, con Microsoft Intune).
- La configuración se debe insertar en el dispositivo para habilitar el complemento Microsoft Enterprise SSO para dispositivos Apple en el dispositivo. Apple requiere esta restricción de seguridad.

## <a name="enable-the-sso-extension-with-mobile-device-management-mdm"></a>Habilitación de la extensión de SSO con administración de dispositivos móviles (MDM)

Para habilitar el complemento de Microsoft Enterprise SSO para dispositivos de Apple, los dispositivos deben enviar una señal a través de un servicio MDM. Puesto que Microsoft incluye el complemento Enterprise SSO en la [aplicación Microsoft Authenticator](..//user-help/user-help-auth-app-overview.md), use su MDM para configurar la aplicación y habilitar el complemento Microsoft Enterprise SSO.

Use los parámetros siguientes para configurar el complemento Microsoft Enterprise SSO para dispositivos de Apple:

- **Tipo**: Redirect
- **Identificador de extensión**: `com.microsoft.azureauthenticator.ssoextension`
- **Identificador del equipo**: `SGGM6D27TK`
- **URL**:
  - `https://login.microsoftonline.com`
  - `https://login.windows.net`
  - `https://login.microsoft.com`
  - `https://sts.windows.net`
  - `https://login.partner.microsoftonline.cn`
  - `https://login.chinacloudapi.cn`
  - `https://login.microsoftonline.de`
  - `https://login.microsoftonline.us`
  - `https://login.usgovcloudapi.net`
  - `https://login-us.microsoftonline.com`

Puede usar Microsoft Intune como su servicio MDM para facilitar la configuración del complemento de Microsoft Enterprise SSO. Para obtener más información, consulte la [documentación de configuración de Intune](https://docs.microsoft.com/intune/configuration/ios-device-features-settings).

## <a name="using-the-sso-extension-in-your-application"></a>Uso de la extensión SSO en su aplicación

La [Biblioteca de autenticación de Microsoft (MSAL) para dispositivos Apple](https://github.com/AzureAD/microsoft-authentication-library-for-objc) de la versión 1.1.0 y versiones posteriores admite el complemento Microsoft Enterprise SSO para dispositivos Apple.

Si quiere admitir el modo de dispositivo compartido que proporciona el complemento Microsoft Enterprise SSO para dispositivos de Apple, asegúrese de que las aplicaciones usen la versión mínima especificada necesaria de MSAL.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el modo de dispositivo compartido en iOS, consulte [Modo de dispositivo compartido para dispositivos iOS](msal-ios-shared-devices.md).
