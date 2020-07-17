---
title: Funcionamiento del inicio de sesión único en recursos locales de dispositivos unidos a Azure AD | Microsoft Docs
description: Aprenda a configurar dispositivos híbridos unidos a Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: ravenn
ms.collection: M365-identity-device-management
ms.openlocfilehash: 149b01401cd6feb7610510efeb1ad9a3c69f3ecf
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86024050"
---
# <a name="how-sso-to-on-premises-resources-works-on-azure-ad-joined-devices"></a>Funcionamiento del inicio de sesión único en recursos locales de dispositivos unidos a Azure AD

Probablemente no sea una sorpresa le sorprenda que un dispositivo unido a Azure Active Directory (Azure AD) le ofrezca una experiencia de inicio de sesión único (SSO) para aplicaciones de nube de su inquilino. Si su entorno tiene una Active Directory local (AD), puede ampliar la experiencia de SSO en estos dispositivos a los recursos y las aplicaciones que se basan también en AD local. 

En este artículo se explica cómo funciona.

## <a name="prerequisites"></a>Requisitos previos

 Si las máquinas unidas a Azure AD no están conectadas a la red de su organización, se requiere una red privada virtual u otra infraestructura de red. El inicio de sesión único local requiere una comunicación de campo visual con los controladores de dominio de AD DS local.

## <a name="how-it-works"></a>Funcionamiento 

Con un dispositivo unido a Azure AD, los usuarios ya tienen una experiencia de inicio de sesión único para las aplicaciones de nube en el entorno. Si su entorno tiene una Azure AD y una instancia de AD local, es posible que desee ampliar el ámbito de su experiencia de SSO a sus aplicaciones locales de línea de negocio (LOB), recursos compartidos de archivos e impresoras.

Los dispositivos unidos a Azure AD no tienen ningún conocimiento acerca del entorno de AD porque no están unidos a él. Sin embargo, puede proporcionar información adicional acerca de su instancia de AD local a estos dispositivos con Azure AD Connect.

Un entorno con ambos, Azure AD y AD local, también se conoce como entorno híbrido. Si tiene un entorno híbrido, es probable que tenga ya implementado Azure AD Connect para sincronizar la información de identidad local con la de la nube. Como parte del proceso de sincronización, Azure AD Connect sincroniza la información de usuario local con Azure AD. Cuando un usuario inicia sesión en un dispositivo de Azure AD en un entorno híbrido:

1. Azure AD envía los detalles del dominio local del usuario de nuevo al dispositivo, junto con el [token de actualización principal](concept-primary-refresh-token.md)
1. El servicio de autoridad de seguridad local (LSA) permite la autenticación Kerberos y NTLM en el dispositivo.

Durante un intento de acceso a un recurso que solicita Kerberos o NTLM en el entorno local del usuario, el dispositivo:

1. Envía las credenciales de usuario y la información de dominio local al controlador de dominio encontrado para autenticar al usuario.
1. Recibe un [ vale de concesión de vales (TGT) de Kerberos](/windows/desktop/secauthn/ticket-granting-tickets) o un token de NTLM basado en el protocolo que admite la aplicación o el recurso local. Si se produce un error al intentar obtener el identificador TGT de Kerberos o el token de NTLM para el dominio (el tiempo de espera de DCLocator puede producir un retraso), se intentan las entradas del Administrador de credenciales o el usuario puede recibir una ventana emergente de autenticación que solicita credenciales para el recurso de destino.

Todas las aplicaciones que están configuradas para la **autenticación integrada de Windows** tienen un inicio de sesión único perfecto al intentar el usuario acceder.

Windows Hello para empresas requiere configuración adicional para habilitar el inicio de sesión único de forma local desde un dispositivo unido a Azure AD. Para más información, consulte [Configure Azure AD joined devices for On-premises Single-Sign On using Windows Hello for Business](/windows/security/identity-protection/hello-for-business/hello-hybrid-aadj-sso-base) (Configuración de los dispositivos unidos a Azure AD para el inicio de sesión único local con Windows Hello para empresas). 

## <a name="what-you-get"></a>Lo que obtiene

Con el inicio de sesión único, en un dispositivo unido a Azure AD puede: 

- Acceder a una ruta de acceso UNC de un servidor miembro de AD
- Acceder a un servidor web miembro de AD configurado para la seguridad integrada de Windows 

Si desea administrar su AD local desde un dispositivo Windows, instale las [Herramientas de administración remota del servidor para Windows 10](https://www.microsoft.com/download/details.aspx?id=45520).

Puede usar:

- El complemento Usuarios y equipos de Active Directory (ADUC) para administrar todos los objetos de AD. Sin embargo, deberá especificar el dominio al que desea conectarse manualmente.
- El complemento DHCP para administrar un servidor DHCP unido a AD. Sin embargo, deberá especificar el nombre del servidor DHCP o la dirección.
 
## <a name="what-you-should-know"></a>Qué debería saber

Es posible que tenga que ajustar el [filtrado basado en dominios](../hybrid/how-to-connect-sync-configure-filtering.md#domain-based-filtering) en Azure AD Connect para asegurarse de que se sincronizan los datos sobre los dominios necesarios.

Las aplicaciones y los recursos que dependen de la autenticación de la máquina en Active Directory no funcionan, ya que los dispositivos unidos a Azure AD no tienen un objeto de equipo en AD. 

No se pueden compartir archivos con otros usuarios en un dispositivo unido a Azure AD.

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte [¿Qué es la administración de dispositivos en Azure Active Directory?](overview.md) 
