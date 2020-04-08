---
title: 'Identidad del dispositivo y virtualización del escritorio: Azure Active Directory'
description: Obtenga información sobre cómo se pueden usar en conjunto las identidades de dispositivos de Azure AD y VDI
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7b431cee3b8e5fc168dec2766442d6f6b9869d1e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74900373"
---
# <a name="device-identity-and-desktop-virtualization"></a>Identidad del dispositivo y virtualización del escritorio

Por lo general, los administradores implementan plataformas de infraestructura de escritorio virtual (VDI) que hospedan los sistemas operativos Windows en sus organizaciones. Los administradores implementan VDI para:

- Simplificar la administración.
- Reducir los costos a través de la consolidación y la centralización de los recursos.
- Ofrecer a los usuarios finales movilidad y libertad para acceder a los escritorios virtuales en cualquier momento, desde cualquier lugar y en cualquier dispositivo.

Hay dos tipos principales de escritorios virtuales:

- Persistente
- No persistente

Las versiones persistentes usan una imagen de escritorio única para cada usuario o grupo de usuarios. Estos escritorios únicos se pueden personalizar y guardar para su uso futuro. 

Las versiones no persistentes usan una colección de equipos de escritorio a los que los usuarios pueden tener acceso según sea necesario. Estos escritorios no persistentes se revierten a su estado original una vez que el usuario cierra la sesión.

En este artículo se tratan las guías de Microsoft para los administradores sobre la compatibilidad con la identidad del dispositivo y VDI. Para más información sobre la identidad del dispositivo, consulte el artículo [¿Qué es una identidad de dispositivo?](overview.md)

## <a name="supported-scenarios"></a>Escenarios admitidos

Antes de configurar las identidades de dispositivo en Azure AD para el entorno de VDI, familiarícese con los escenarios admitidos. En la tabla siguiente se muestran los escenarios de aprovisionamiento que se admiten. El aprovisionamiento en este contexto implica que un administrador puede configurar identidades de dispositivo a escala sin requerir ninguna interacción del usuario final.

| Tipo de identidad del dispositivo | Infraestructura de identidad | Dispositivos Windows | Versión de la plataforma de VDI | Compatible |
| --- | --- | --- | --- | --- |
| Híbrido unido a Azure AD | Federada* | Windows actual*** y Windows de nivel inferior**** | Persistente | Sí |
|   |   | Windows actual | No persistente | No |
|   |   | Dispositivos de Windows de nivel inferior | No persistente | Sí |
|   | Administrada** | Windows actual y Windows de nivel inferior | Persistente | Sí |
|   |   | Windows actual | No persistente | No |
|   |   | Dispositivos de Windows de nivel inferior | No persistente | Sí |
| Unido a Azure AD | Federado | Windows actual | Persistente | No |
|   |   |   | No persistente | No |
|   | Administrado | Windows actual | Persistente | No |
|   |   |   | No persistente | No |
| Registrado en Azure AD | Federado | Windows actual | Persistente | No |
|   |   |   | No persistente | No |
|   | Administrado | Windows actual | Persistente | No |
|   |   |   | No persistente | No |

\* Un entorno de infraestructura de identidad **federada** representa un entorno con un proveedor de identidades como AD FS u otro IdP de terceros.

\*\* Un entorno de infraestructura de identidad **administrada** representa un entorno con Azure AD como el proveedor de identidades implementado ya sea con la [sincronización de hash de contraseñas (PHS)](../hybrid/whatis-phs.md) o la [autenticación de paso a través (PTA)](../hybrid/how-to-connect-pta.md) con [inicio de sesión único de conexión directa](../hybrid/how-to-connect-sso.md).

\*\*\* Los dispositivos de **Windows actual** representan a Windows 10, Windows Server 2016 y Windows Server 2019.

\*\*\*\* Los dispositivos de **Windows de nivel inferior** representan a Windows 7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012 y Windows Server 2012 R2. Para obtener información de soporte técnico sobre Windows 7, consulte [El soporte técnico para Windows 7 está llegando a su fin](https://www.microsoft.com/microsoft-365/windows/end-of-windows-7-support). Para información de soporte técnico sobre Windows Server 2008 R2, consulte [Preparación para la finalización del soporte técnico de Windows Server 2008](https://www.microsoft.com/cloud-platform/windows-server-2008).

## <a name="microsofts-guidance"></a>Guías de Microsoft

Los administradores deben consultar los artículos siguientes, en función de su infraestructura de identidad, para aprender a configurar la Unión a Azure AD híbrido.

- [Configuración de la unión a Azure Active Directory híbrido para un entorno federado](hybrid-azuread-join-federated-domains.md)
- [Configuración de la unión a Azure Active Directory híbrido para un entorno administrado](hybrid-azuread-join-managed-domains.md)

Si se basa en la herramienta de preparación del sistema (sysprep.exe) y utiliza para la instalación una imagen anterior a Windows 10 1809, asegúrese de que esa imagen no corresponda a un dispositivo que ya está registrado en Azure AD como unido a Azure AD híbrido.

Si se basa en la instantánea de una máquina virtual (VM) para crear otras VM, asegúrese de que esa instantánea no sea de una VM que ya se haya registrado en Azure AD como unión a Azure AD híbrido.

Al implementar VDI no persistente, los administradores de TI deben prestar mucha atención a la administración de dispositivos obsoletos en Azure AD. Microsoft recomienda que los administradores de TI implementen las instrucciones siguientes. Si no lo hace, el directorio tendrá una gran cantidad de dispositivos unidos a Azure AD híbrido que se han registrado en la plataforma VDI no persistente.

- Cree y use un prefijo para el nombre para mostrar del equipo que indique el escritorio como basado en VDI.
- Implemente el siguiente comando como parte del script de cierre de sesión. Este comando desencadenará una llamada de mejor esfuerzo a Azure AD para eliminar el dispositivo.
   - Para dispositivos de Windows de nivel inferior, autoworkplace.exe /leave
- Defina e implemente el proceso para [administrar dispositivos obsoletos](manage-stale-devices.md).
   - Una vez que tenga una estrategia para identificar los dispositivos unidos a Azure AD híbrido no persistentes, puede ser más agresivo en la limpieza de estos dispositivos para asegurarse de que su directorio no se consuma con muchos dispositivos obsoletos.
 
## <a name="next-steps"></a>Pasos siguientes

[Configuración de la unión a Azure Active Directory híbrido para un entorno federado](hybrid-azuread-join-federated-domains.md)
