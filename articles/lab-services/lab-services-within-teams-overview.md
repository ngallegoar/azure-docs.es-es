---
title: Azure Lab Services en Microsoft Teams
description: Proporciona información general sobre el uso de Azure Lab Services en Microsoft Teams.
ms.topic: conceptual
ms.date: 10/07/2020
ms.openlocfilehash: f39613427ed174dfca80d4a48be9473ab7025e79
ms.sourcegitcommit: 5e5a0abe60803704cf8afd407784a1c9469e545f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96433924"
---
# <a name="azure-lab-services-within-microsoft-teams"></a>Azure Lab Services en Microsoft Teams

Azure Lab Services puede usarse en Microsoft Teams con la aplicación Teams para **Azure Lab Services**. Cualquier propietario de equipo con acceso de propietario, colaborador o creador a las cuentas de laboratorio podrá crear laboratorios y aprovisionar máquinas virtuales para todos los miembros del equipo.

En este artículo se describen las ventajas de usar Azure Lab Services dentro de Teams y se proporcionan vínculos a otros artículos con instrucciones sobre cómo crear y administrar laboratorios en Teams. 

> [!NOTE]
>La aplicación Teams para **Azure Lab Services** solo se puede agregar a un equipo; no se puede agregar a chats individuales o chats de grupo.

## <a name="benefits"></a>Ventajas

La integración de Azure Lab Services con Microsoft Teams ayudará a los educadores a configurar un entorno educativo y a proporcionar entornos de laboratorio virtuales dentro del equipo (clase): 

* Los educadores pueden configurar laboratorios para que los alumnos puedan acceder a sus máquinas virtuales desde Teams, sin salir de ahí y sin tener que ir al [sitio web de Azure Lab Services](https://labs.azure.com).
* Inicio de sesión único (SSO) desde Teams en Azure Lab Services.
* Los propietarios de equipos y laboratorios no necesitan mantener las listas de clases en dos sistemas diferentes; la lista de usuarios de laboratorio se rellena automáticamente a partir de la pertenencia a un equipo y se realiza una sincronización cada 24 horas de forma automática. 
* Después de la publicación inicial de la máquina virtual de plantilla, la capacidad del laboratorio (es decir, el número de máquinas virtuales en él) se ajustará automáticamente en función de si se agregan usuarios a la pertenencia al equipo o se eliminan de esta. 
* Los propietarios de equipos y laboratorios solo verán los laboratorios relacionados con el equipo y los alumnos solo verán las máquinas virtuales que se aprovisionan para el equipo concreto. 
* Los usuarios se registrarán automáticamente en el laboratorio y las máquinas virtuales se asignarán automáticamente en el primer inicio de sesión, una vez publicado el laboratorio. No es necesario que los educadores envíen invitaciones ni que los alumnos se registren en el laboratorio por separado.  

## <a name="next-steps"></a>Pasos siguientes

Vea los artículos siguientes:

- [Introducción y creación de un laboratorio en Teams](how-to-get-started-create-lab-within-teams.md)
- [Administración de listas de usuarios de laboratorio dentro de Teams](how-to-manage-user-lists-within-teams.md)
- [Administración del grupo de máquinas virtuales del laboratorio en Teams](how-to-manage-vm-pool-within-teams.md)
- [Creación y administración de programaciones de laboratorio dentro de Teams](how-to-create-schedules-within-teams.md)
- [Acceso a una máquina virtual en Teams: vista del alumno](how-to-access-vm-for-students-within-teams.md)
