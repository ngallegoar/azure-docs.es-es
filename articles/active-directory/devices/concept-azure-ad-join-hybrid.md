---
title: ¿Qué es un dispositivo híbrido unido a Azure AD?
description: Conozca cómo la administración de identidades de dispositivos puede ayudarle a administrar los dispositivos que acceden a los recursos de su entorno.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 15cdaba7d63d72aab25757e7ba6f5eadc48e026a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76512256"
---
# <a name="hybrid-azure-ad-joined-devices"></a>Dispositivos híbridos unidos a Azure AD

Durante más de una década, muchas organizaciones han usado la unión a un dominio en su instancia de Active Directory local para permitir:

- A los departamentos de TI administrar los dispositivos de empresa desde una ubicación central.
- A los usuarios iniciar sesión en sus dispositivos con sus cuentas profesionales o educativas de Active Directory.

Normalmente, las organizaciones con un uso local confían en los métodos de creación de imágenes para aprovisionar los dispositivos y suelen usar **Configuration Manager** o la **directiva de grupo** para administrarlos.

Si su entorno tiene un uso local de AD y también desea aprovechar las funcionalidades proporcionadas por Azure Active Directory, puede implementar dispositivos híbridos unidos a Azure AD. Estos dispositivos se han unido al entorno local de Active Directory y se han registrado en Azure Active Directory.

|   | Unión a Azure AD híbrido |
| --- | --- |
| **Definición** | Dispositivos unidos a AD local y a Azure AD que requieren una cuenta de la organización para iniciar sesión en el dispositivo |
| **Público principal** | Adecuados para organizaciones híbridas con infraestructura de AD local existente |
|   | Se aplica a todos los usuarios de una organización. |
| **Propiedad del dispositivo** | Organización |
| **Sistemas operativos** | Windows 10, 8.1 y 7 |
|   | Windows Server 2008/R2, 2012/R2, 2016 y 2019 |
| **Aprovisionamiento** | Windows 10, Windows Server 2016/2019 |
|   | Unión a un dominio por TI y la unión automática a través de Azure AD Connect o de la configuración de ADFS |
|   | Unión a un dominio por Windows Autopilot y la unión automática a través de Azure AD Connect o de la configuración de ADFS |
|   | Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2. Requieren MSI |
| **Opciones de inicio de sesión en el dispositivo** | Cuentas organizativas que usan: |
|   | Contraseña |
|   | Windows Hello para empresas para Win10 |
| **Administración de dispositivos** | Directiva de grupo |
|   | Administración independiente o conjunta de Configuration Manager con Microsoft Intune |
| **Principales funcionalidades** | SSO tanto a los recursos en la nube como a los recursos locales |
|   | Acceso condicional a través de la unión a un dominio o mediante Intune, si se trata de una coadministración |
|   | Autoservicio de restablecimiento de contraseña y restablecimiento de PIN de Windows Hello |
|   | Enterprise State Roaming entre dispositivos |

![Dispositivos híbridos unidos a Azure AD](./media/concept-azure-ad-join-hybrid/azure-ad-hybrid-joined-device.png)

## <a name="scenarios"></a>Escenarios

Use dispositivos híbridos unidos a Azure AD si:

- Tiene aplicaciones Win32 implementadas en estos dispositivos que se basan en la autenticación de máquina de Active Directory.
- Quiere seguir usando la directiva de grupo para administrar la configuración del dispositivo.
- Quiere seguir usando las soluciones existentes de creación de imágenes para implementar y configurar dispositivos.
- Además de Windows 10, debe admitir dispositivos de Windows 7 y 8.1 de nivel inferior.

## <a name="next-steps"></a>Pasos siguientes

- [Planeación de la implementación de la unión a Azure AD híbrido](hybrid-azuread-join-plan.md)
- [Administración de identidades de dispositivos con Azure Portal](device-management-azure-portal.md)
- [Administración de dispositivos obsoletos en Azure AD](manage-stale-devices.md)
