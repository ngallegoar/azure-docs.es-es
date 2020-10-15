---
title: 'Migración manual desde Windows Virtual Desktop (clásico): Azure'
description: Cómo migrar manualmente desde Windows Virtual Desktop (clásico) a Windows Virtual Desktop.
author: Heidilohr
ms.topic: how-to
ms.date: 09/11/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 74527f57340f850b60dd00dcd054992c423a49c7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90038916"
---
# <a name="migrate-manually-from-windows-virtual-desktop-classic"></a>Migración manual desde Windows Virtual Desktop (clásico)

Windows Virtual Desktop (clásico) crea su entorno de servicio con los cmdlets de PowerShell, las API de REST y los objetos de servicio. Un "objeto" en un entorno de servicio de Windows Virtual Desktop es algo que crea Windows Virtual Desktop. Los objetos de servicio incluyen inquilinos, grupos de hosts, grupos de aplicaciones y hosts de sesión.

Sin embargo, Windows Virtual Desktop (clásico) no se integra con Azure. Sin la integración con Azure, los objetos que cree no se administrarán automáticamente mediante Azure Portal, ya que no están conectados a su suscripción de Azure.

La actualización principal reciente de Windows Virtual Desktop marca un cambio en el servicio hacia la integración completa con Azure. Los objetos que se cree en Windows Virtual Desktop se administrarán automáticamente mediante Azure Portal.

En este artículo, explicaremos por qué debería considerar la posibilidad de migrar a la versión más reciente de Windows Virtual Desktop. Después de eso, le indicaremos cómo migrar manualmente desde Windows Virtual Desktop (clásico) a la actualización más reciente de Windows Virtual Desktop.

## <a name="why-migrate"></a>¿Por qué migrar?

Las actualizaciones principales pueden ser inadecuadas, especialmente las que tiene que hacer manualmente. Sin embargo, hay algunos motivos por los que no puede migrar automáticamente:

- Los objetos de servicio existentes creados con la versión clásica no tienen ninguna representación en Azure. Su ámbito no se extiende más allá del servicio Windows Virtual Desktop.
- Con la actualización más reciente, el identificador de la aplicación del servicio se cambió para revocar el consentimiento de las aplicaciones como hizo con Windows Virtual Desktop (clásico). No podrá crear nuevos objetos de Azure con Windows Virtual Desktop a menos que se autentiquen con el nuevo identificador de la aplicación.

A pesar de los inconvenientes, migrar desde la versión clásica sigue siendo importante. Esto es lo que puede hacer después de migrar:

- Administre Windows Virtual Desktop a través de Azure Portal.
- Asigne grupos de usuarios de Azure Active Directory (AD) a grupos de aplicaciones.
- Use la característica Log Analytics mejorada para solucionar problemas de implementación.
- Use controles de acceso basados en rol nativos de Azure para administrar el acceso administrativo.

## <a name="when-should-i-migrate"></a>¿Cuándo debo migrar?

Cuando se pregunte si debe migrar, también debe tener en cuenta la situación actual y futura de la implementación.

Hay algunos escenarios en concreto en los que le recomendamos migrar manualmente:

- Tiene una configuración del grupo de hosts de prueba con un número reducido de usuarios.
- Tiene una configuración del grupo de hosts de producción con un número reducido de usuarios, pero tiene previsto que estos lleguen a ser cientos.
- Tiene una configuración simple que se puede replicar fácilmente. Por ejemplo, si las máquinas virtuales usan una imagen de la galería.

> [!IMPORTANT]
> Si usa una configuración avanzada que tardó mucho tiempo en estabilizarse o tiene muchos usuarios, no recomendamos migrar manualmente.

## <a name="prepare-for-migration"></a>Preparación para la migración

Antes de empezar, tendrá que asegurarse de que el entorno está listo para la migración.

Esto es lo que necesita para iniciar el proceso de migración:

- Una suscripción de Azure en la que creará nuevos objetos de servicio de Azure.
- Asegúrese de que se le han asignado los siguientes roles:
    
    - Colaborador
    - Administrador de acceso de usuario
    
    El rol Colaborador le permite crear objetos de Azure en la suscripción, mientras que el rol Administrador de acceso de usuario le permite asignar usuarios a grupos de aplicaciones.

## <a name="how-to-migrate-manually"></a>Cómo migrar manualmente

Ahora que se ha preparado para el proceso de migración, es el momento de migrar de verdad.

Para migrar manualmente desde Windows Virtual Desktop (clásico) a Windows Virtual Desktop:

1. Siga las instrucciones en [Creación de un grupo de hosts con Azure Portal](create-host-pools-azure-marketplace.md) para crear todos los objetos de alto nivel con Azure Portal.
2. Si desea traer las máquinas virtuales que ya usa, siga las instrucciones en [Registro de las máquinas virtuales en el grupo de hosts de Windows Virtual Desktop](create-host-pools-powershell.md#register-the-virtual-machines-to-the-windows-virtual-desktop-host-pool) para registrarlas manualmente en el nuevo grupo de hosts que creó en el paso 1.
3. Cree nuevos grupos de aplicaciones RemoteApp.
4. Publique los usuarios o grupos de usuarios en los nuevos grupos de aplicaciones RemoteApp y de escritorio.
5. Actualice la directiva de acceso condicional para permitir los nuevos objetos siguiendo las instrucciones en [Configuración de la autenticación multifactor](set-up-mfa.md).

Para evitar el tiempo de inactividad, primero debe registrar los hosts de sesión existentes en los grupos de hosts integrados en Azure Resource Manager en pequeños grupos a la vez. Después, traiga poco a poco a los usuarios a los nuevos grupos de aplicaciones integrados en Azure Resource Manager.

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya migrado, descubra cómo funciona Windows Virtual Desktop consultando [nuestros tutoriales](create-host-pools-azure-marketplace.md). Obtenga información sobre funcionalidades de administración avanzadas en [Expansión de un grupo de hosts existente](expand-existing-host-pool.md) y [Personalización de propiedades de RDP](customize-rdp-properties.md).

Para más información sobre los objetos de servicio, consulte [Entorno de Windows Virtual Desktop](environment-setup.md).
