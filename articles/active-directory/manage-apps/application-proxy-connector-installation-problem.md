---
title: Problema al instalar el conector de agente del proxy de aplicación | Microsoft Docs
description: Cómo solucionar los problemas que se produzcan al instalar el conector del agente de proxy de aplicación
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 466e1ce0efbdec3f5475634f3857d02554d93d98
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80049129"
---
# <a name="problem-installing-the-application-proxy-agent-connector"></a>Problema al instalar el conector de agente del proxy de aplicación

El conector del proxy de aplicación de Microsoft AAD es un componente de dominio interno que utiliza conexiones salientes para establecer la conectividad desde el punto de conexión disponible en la nube hacia el dominio interno.

## <a name="general-problem-areas-with-connector-installation"></a>Áreas problemáticas generales con la instalación del conector

Cuando se produce un error en la instalación de un conector, la causa principal suele pertenecer a una de las áreas siguientes:

1.  **Conectividad**: para completar una instalación correcta, el nuevo conector necesita registrarse y establecer propiedades de confianza futuras. Lo hace conectándose al servicio en la nube del proxy de aplicación de AAD.

2.  **Establecimiento de confianza**: el nuevo conector crea un certificado autofirmado y se registra en el servicio en la nube.

3.  **Autenticación del administrador**: durante la instalación, el usuario debe proporcionar credenciales de administrador para completar la instalación del conector.

> [!NOTE]
> Los registros de instalación del conector pueden encontrarse en la carpeta% TEMP%, y pueden ayudar a proporcionar información adicional sobre lo que provoca un error de instalación.

## <a name="verify-connectivity-to-the-cloud-application-proxy-service-and-microsoft-login-page"></a>Comprobación de la conectividad con el servicio de proxy de aplicación en la nube y la página de inicio de sesión de Microsoft

**Objetivo:** compruebe que la máquina del conector se pueda conectar al punto de conexión de registro del proxy de aplicación de AAD, así como a la página de inicio de sesión de Microsoft.

1.  En el servidor del conector, ejecute una prueba de puertos con [telnet](https://docs.microsoft.com/windows-server/administration/windows-commands/telnet) u otra herramienta para este fin y compruebe si los puertos 443 y 80 están abiertos.

2.  Si alguno de estos puertos no es correcto, compruebe que el firewall o el proxy de back-end tengan acceso a los dominios y puertos necesarios; consulte [Preparación del entorno local](application-proxy-add-on-premises-application.md#prepare-your-on-premises-environment).

3.  Abra un explorador (en otra pestaña), vaya a la siguiente página web: `https://login.microsoftonline.com` y asegúrese de que pueda iniciar sesión en esa página.

## <a name="verify-machine-and-backend-components-support-for-application-proxy-trust-cert"></a>Comprobación de que los componentes de máquina y back-end admitan el certificado de confianza del proxy de aplicación

**Objetivo:** compruebe que la máquina del conector, el proxy de back-end y el firewall admitan el certificado creado por el conector para la confianza futura.

>[!NOTE]
>El conector intenta crear un certificado SHA512 que sea compatible con TLS 1.2. Si la máquina o el firewall y el proxy de back-end no admiten TLS1.2, la instalación producirá un error.
>
>

**Para resolver el problema:**

1.  Compruebe que la máquina sea compatible con TLS 1.2: todas las versiones de Windows posteriores a 2012 R2 deberían admitir TLS 1.2. Si la máquina del conector es de la versión 2012 R2 o una anterior, asegúrese de que los siguientes KB estén instalados en la máquina: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2>

2.  Póngase en contacto con el administrador de la red y pídale que compruebe que el proxy y el firewall de back-end no bloqueen SHA512 para el tráfico saliente.

## <a name="verify-admin-is-used-to-install-the-connector"></a>Comprobación de que se use el administrador para instalar el conector

**Objetivo:** compruebe que el usuario que intenta instalar el conector sea un administrador con las credenciales correctas. Actualmente, el usuario debe ser como mínimo administrador de aplicaciones para que la instalación se complete correctamente.

**Para comprobar que las credenciales sean correctas:**

Conéctese a `https://login.microsoftonline.com` y use las mismas credenciales. Asegúrese de que el inicio de sesión se haya realizado correctamente. Para comprobar el rol de usuario, vaya a **Azure Active Directory**  -&gt; **Usuarios y grupos** -&gt; **Todos los usuarios**. 

Seleccione su cuenta de usuario y "Rol del directorio" en el menú resultante. Compruebe que el rol seleccionado sea "Administrador de aplicaciones". Si no puede acceder a alguna de las páginas en estos pasos, significa que no tiene el rol necesario.

## <a name="next-steps"></a>Pasos siguientes
[Descripción de los conectores del Proxy de aplicación de Azure AD](application-proxy-connectors.md)
