---
title: Azure AD acceso híbrido seguro | Microsoft Docs
description: En este artículo se describen las soluciones de asociados para integrar las aplicaciones locales, de nube pública o de nube privada heredadas con Azure AD. Proteja sus aplicaciones heredadas conectando controladores de entrega de aplicaciones o redes a Azure AD.
services: active-directory
author: gargi-sinha
manager: martinco
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: how-to
ms.workload: identity
ms.date: 9/10/2020
ms.author: gasinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 521439da044fb4fc3c2f578f28658215266138d9
ms.sourcegitcommit: e5f9126c1b04ffe55a2e0eb04b043e2c9e895e48
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96317102"
---
# <a name="secure-hybrid-access-secure-legacy-apps-with-azure-active-directory"></a>Acceso híbrido seguro: Protección de aplicaciones heredadas con Azure Active Directory

Ahora puede proteger sus aplicaciones de autenticación heredadas locales y en la nube conectándolas a Azure Active Directory (AD) con:

- [Azure AD Application Proxy](#secure-hybrid-access-sha-through-azure-ad-application-proxy)

- [Redes y controladores de entrega de aplicaciones existentes](#sha-through-networking-and-delivery-controllers)

- [Aplicaciones de red privada virtual (VPN) y perímetro definido por software (SDP)](#sha-through-vpn-and-sdp-applications)

Puede salvar la brecha y fortalecer su postura de seguridad en todas las aplicaciones con funcionalidades de Azure AD, como [Acceso condicional](../conditional-access/overview.md) en Azure AD y Azure AD [Identity Protection](../identity-protection/overview-identity-protection.md).

## <a name="secure-hybrid-access-sha-through-azure-ad-application-proxy"></a>Acceso híbrido seguro (SHA) a través de Azure AD Application Proxy
  
Con [Application Proxy](./what-is-application-proxy.md), puede proporcionar [acceso remoto seguro](./application-proxy.md) a sus aplicaciones web locales. Los usuarios no necesitan usar una VPN. Los usuarios se benefician al conectarse fácilmente a sus aplicaciones desde cualquier dispositivo después de un [inicio de sesión único](./add-application-portal-setup-sso.md). Application Proxy proporciona acceso remoto como servicio y permite [publicar fácilmente sus aplicaciones locales](./application-proxy-add-on-premises-application.md) a usuarios ajenos a la red corporativa. Ayuda a escalar la administración del acceso a la nube sin necesidad de modificar las aplicaciones locales. [Planee una implementación de Azure AD Application Proxy](./application-proxy-deployment-plan.md) como el siguiente paso.

## <a name="azure-ad-partner-integrations"></a>Integraciones de asociados de Azure AD

### <a name="sha-through-networking-and-delivery-controllers"></a>SHA mediante los controladores de red y entrega

Además de [Azure AD Application Proxy](./what-is-application-proxy.md), para permitirle usar el [marco de confianza cero](https://www.microsoft.com/security/blog/2020/04/02/announcing-microsoft-zero-trust-assessment-tool/), Microsoft se asocia con proveedores externos. Puede usar las redes y los controladores de entrega existentes, así como proteger fácilmente las aplicaciones heredadas que son críticas para los procesos empresariales pero que no se pudieron proteger antes de Azure AD. Es probable que ya tenga todo lo que necesita para empezar a proteger estas aplicaciones.

![La imagen muestra el acceso híbrido seguro con los asociados de red y el proxy de aplicación](./media/secure-hybrid-access/secure-hybrid-access.png)

Los siguientes proveedores de red ofrecen soluciones predefinidas e instrucciones detalladas para la integración con Azure AD.

- [Acceso de aplicaciones empresariales (EAA) de Akamai](../saas-apps/akamai-tutorial.md)

- [Controlador de entrega de aplicaciones (ADC) de Citrix](../saas-apps/citrix-netscaler-tutorial.md)

- [APM BIG-IP de F5](f5-aad-integration.md)

- [Kemp](../saas-apps/kemp-tutorial.md)

### <a name="sha-through-vpn-and-sdp-applications"></a>SHA a través de aplicaciones de VPN y SDP

Con las soluciones de VPN y SDP, puede proporcionar acceso seguro a la red empresarial desde cualquier dispositivo y en cualquier momento y lugar, al mismo tiempo que protege los datos de su organización. Al tener Azure AD como proveedor de identidades (IDP), puede usar métodos de autenticación y autorización modernos, como el [inicio de sesión único](./what-is-single-sign-on.md) y la [autenticación multifactor](../authentication/concept-mfa-howitworks.md) de Azure AD para proteger las aplicaciones heredadas locales.  

![La imagen muestra el acceso híbrido seguro con los asociados de VPN y el proxy de aplicación ](./media/secure-hybrid-access/app-proxy-vpn.png)

Los siguientes proveedores de VPN y SDP ofrecen soluciones predefinidas e instrucciones detalladas para la integración con Azure AD.

• [Cisco AnyConnect](../saas-apps/cisco-anyconnect.md)

• [Fortinet](../saas-apps/fortigate-ssl-vpn-tutorial.md)

• [Big-IP APM de F5](f5-aad-password-less-vpn.md)

• [Palo Alto Networks Global Protect](../saas-apps/paloaltoadmin-tutorial.md)

• [Zscaler Private Access (ZPA)](../saas-apps/zscalerprivateaccess-tutorial.md)
