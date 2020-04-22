---
title: 'Azure AD Connect: Consideraciones al respecto de la identidad híbrida para Azure Government'
description: Consideraciones especiales para implementar Azure AD Connect con la nube de Azure Government.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/14/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2ad28beb68afb5207e7b36c5d76c4dac808c5114
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2020
ms.locfileid: "81377557"
---
# <a name="hybrid-identity-considerations-for-azure-government"></a>Consideraciones al respecto de la identidad híbrida para Azure Government 
El siguiente documento describe las consideraciones para implementar un entorno híbrido con la nube de Azure Government.  Esta información se proporciona como referencia para los administradores y arquitectos que trabajan con la nube de Azure Government.  
> [!NOTE] 
> Para integrar un entorno de AD local con la nube de Azure Government, debe actualizar a la versión más reciente de [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594). 

> [!NOTE] 
> Para obtener una lista de puntos de conexión de DoD del gobierno de EE. UU., consulte esta [documentación](https://docs.microsoft.com/office365/enterprise/office-365-u-s-government-dod-endpoints). 

## <a name="pass-through-authentication"></a>Autenticación de paso a través 
La siguiente información se proporciona para la implementación de la autenticación de paso a través (PTA) y la nube de Azure Government.

### <a name="allow-access-to-urls"></a>Permiso de acceso a direcciones URL  
Antes de implementar el agente de autenticación de paso a través, compruebe si hay un firewall entre los servidores y Azure AD. Si el firewall o el servidor proxy permiten la inclusión en la lista de permitidos de DNS, agregue las siguientes conexiones: 
> [!NOTE]
> Las siguientes instrucciones también se aplican a la instalación del [conector de Application Proxy](https://aka.ms/whyappproxy) para entornos de Azure Government.

|URL |Cómo se usa|
|-----|-----| 
|*.msappproxy.us *.servicebus.usgovcloudapi.net|Comunicación entre el agente y el servicio en la nube de Azure AD |
|mscrl.microsoft.us:80 crl.microsoft.us:80 </br>ocsp.msocsp.us:80 www.microsoft.us:80| El agente utiliza estas direcciones URL para verificar los certificados.| 
|login.windows.us secure.aadcdn.microsoftonline-p.com *.microsoftonline.us </br>* .microsoftonline-p.us </br>*.msauth.net </br>* .msauthimages.net </br>*.msecnd.net</br>* .msftauth.net </br>*.msftauthimages.net</br>* .phonefactor.net </br>enterpriseregistration.windows.net</br>management.azure.com </br>policykeyservice.dc.ad.msft.net</br>ctdl.windowsupdate.us:80| El agente utiliza estas direcciones URL durante el proceso de registro.| 

### <a name="install-the-agent-for-the-azure-government-cloud"></a>Instalación del agente para la nube de Azure Government 
Para instalar el agente para la nube de Azure Government, debe seguir estos pasos específicos: En el terminal de la línea de comandos, vaya a la carpeta donde se encuentra el archivo ejecutable para instalar el agente. Ejecute el siguiente comando, que especifica que la instalación es para Azure Government. 

Para la autenticación de paso a través: 
```
AADConnectAuthAgentSetup.exe ENVIRONMENTNAME="AzureUSGovernment"
```

Para el proxy de aplicación:
```
AADApplicationProxyConnectorInstaller.exe ENVIRONMENTNAME="AzureUSGovernment" 
```

## <a name="single-sign-on"></a>Inicio de sesión único 
Configuración del servidor de Azure AD Connect: si usa la autenticación de paso a través como método de inicio de sesión, no es necesaria ninguna otra comprobación de requisitos previos. Si va a usar la sincronización de hash de contraseña como método de inicio de sesión y hay un firewall entre Azure AD Connect y Azure AD, asegúrese de que:
- Usa Azure AD Connect 1.1.644.0 o cualquier versión posterior. 
- Si el firewall o el proxy permiten la creación de listas de permitidos para DNS, agregue las conexiones a las direcciones URL *.msapproxy.us en el puerto 443. En caso contrario, permita el acceso a los intervalos de direcciones IP del centro de datos de Azure, que se actualizan cada semana. Este requisito solo es aplicable cuando se habilita la característica. No es necesario para los inicios de sesión de usuarios reales. 

### <a name="rolling-out-seamless-sso"></a>Implementación del inicio de sesión único de conexión directa 
Puede implementar el inicio de sesión único de conexión directa gradualmente a los usuarios con las instrucciones que se proporcionan a continuación. Para empezar, agregue la siguiente dirección URL de Azure AD a la configuración de la zona de la intranet de todos los usuarios o de los seleccionados mediante la directiva de grupo de Active Directory: https://autologon.microsoft.us 

Además, debe habilitar una configuración de directiva de zona de intranet llamada Permitir actualizaciones en la barra de estado a través de script mediante la directiva de grupo. Consideraciones sobre el explorador Mozilla Firefox (todas las plataformas) Mozilla Firefox no utiliza automáticamente la autenticación Kerberos. Cada usuario debe agregar manualmente la dirección URL de Azure AD a su configuración de Firefox mediante estos pasos: 
1. Ejecute Firefox y escriba about:config en la barra de direcciones. Descarte las notificaciones que vea. 
2. Busque la preferencia network.negotiate-auth.trusted-uris. Esta preferencia enumera los sitios de confianza de Firefox para la autenticación Kerberos. 
3. Haga clic con el botón derecho y seleccione Modificar. 
4. Escriba https://autologon.microsoft.us en el campo.
5. Seleccione Aceptar y vuelva a abrir el explorador. 

### <a name="microsoft-edge-based-on-chromium-all-platforms"></a>Microsoft Edge basado en Chromium (todas las plataformas) 
Si ha reemplazado la configuración de las directivas `AuthNegotiateDelegateAllowlist` o `AuthServerAllowlist`  en el entorno, asegúrese de agregar también la dirección URL de Azure AD (https://autologon.microsoft.us) ). 

### <a name="google-chrome-all-platforms"></a>Google Chrome (todas las plataformas) 
Si ha reemplazado la configuración de las directivas `AuthNegotiateDelegateWhitelist` o `AuthServerWhitelist`  en el entorno, asegúrese de agregar también la dirección URL de Azure AD (https://autologon.microsoft.us) ). 

## <a name="next-steps"></a>Pasos siguientes
[Autenticación de paso a través](how-to-connect-pta-quick-start.md#step-1-check-the-prerequisites)
[Inicio de sesión único](how-to-connect-sso-quick-start.md#step-1-check-the-prerequisites) 
