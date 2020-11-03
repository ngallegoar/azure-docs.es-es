---
title: Conceptos sobre identidad y acceso
description: Obtenga información sobre los conceptos de identidad y acceso de Azure VMware Solution
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 7127109801d92d2177f6edac3efcaf76ddf217e6
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92674651"
---
# <a name="azure-vmware-solution-identity-concepts"></a>Conceptos de identidad en Azure VMware Solution

Cuando se implementa una nube privada, se aprovisionan un servidor vCenter y NSX-T Manager. vCenter se usa para administrar cargas de trabajo de máquina virtual (VM). NSX-T Manager se usa para ampliar la red definida por software de la nube privada.

Administración de acceso e identidad usa los privilegios del grupo CloudAdmin para vCenter y derechos de administrador restringidos para NSX-T Manager. Garantiza que la plataforma de nube privada se actualiza automáticamente con las características y revisiones más recientes.  Para obtener más información, vea el [artículo sobre conceptos de actualización de nube privada][concepts-upgrades].

## <a name="vcenter-access-and-identity"></a>Acceso e identidad de vCenter

Los privilegios en vCenter se proporcionan a través del grupo CloudAdmin. Ese grupo se puede administrar localmente en vCenter, o a través de la integración del inicio de sesión único de vCenter LDAP con Azure Active Directory. Se le proporciona la capacidad de habilitar esa integración después de implementar una nube privada.

Los privilegios de CloudAdmin y CloudGlobalAdmin se muestran en la tabla siguiente.

|  Conjunto de privilegios           | CloudAdmin | CloudGlobalAdmin | Comentario |
| :---                     |    :---:   |       :---:      |   :--:  |
|  Alarmas                  | Un usuario de CloudAdmin tiene todos los privilegios de Alarmas para las alarmas en las VM y Compute-ResourcePool.     |          --        |  -- |
|  Implementación automática             |  --  |        --        |  Microsoft se encarga de la administración de hosts.  |
|  Certificados            |  --  |        --       |  Microsoft se encarga de la administración de certificados.  |
|  Biblioteca de contenido         | Un usuario de CloudAdmin tiene privilegios para crear y usar archivos en una biblioteca de contenido.    |         Habilitado con SSO.         |  Microsoft distribuirá los archivos de la biblioteca de contenido a los hosts ESXi.  |
|  Centro de datos              |  --  |        --          |  Microsoft realiza todas las operaciones de centros de datos.  |
|  Almacén de datos               | Datastore.AllocateSpace, Datastore.Browse, Datastore.Config, Datastore.DeleteFile, Datastore.FileManagement, Datastore.UpdateVirtualMachineMetadata     |    --    |   -- |
|  ESX Agent Manager       |  --  |         --       |  Microsoft realiza todas las operaciones.  |
|  Carpeta                  |  Un usuario de CloudAdmin tiene todos los privilegios de Carpeta.     |  --  |  --  |
|  Global                  |  Global.CancelTask, Global.GlobalTag, Global.Health, Global.LogEvent, Global.ManageCustomFields, Global.ServiceManagers, Global.SetCustomField, Global.SystemTag         |                  |    |
|  Host                    |  Host.Hbr.HbrManagement      |        --          |  Microsoft realiza todas las demás operaciones de Host.  |
|  InventoryService        |  InventoryService.Tagging      |        --          |  --  |
|  Red                 |  Network.Assign    |                  |  Microsoft realiza todas las demás operaciones de Red.  |
|  Permisos             |  --  |        --       |  Microsoft realiza todas las operaciones de Permisos.  |
|  Almacenamiento controlado por perfiles  |  --  |        --       |  Microsoft realiza todas las operaciones de Perfil.  |
|  Resource                |  Un usuario de CloudAdmin tiene todos los privilegios de Recurso.        |      --       | --   |
|  Tarea programada          |  Un usuario de CloudAdmin tiene todos los privilegios de Tarea programada.   |   --   | -- |
|  Sesiones                |  Sessions.GlobalMessage, Sessions.ValidateSession      |   --   |  Microsoft realiza todas las demás operaciones de Sesiones.  |
|  Vistas de almacenamiento           |  StorageViews.View   |        --          |  Microsoft realiza todas las demás operaciones de Vista de almacenamiento (configurar servicio).  |
|  Tareas                   |  --  |  --   |  Microsoft administra extensiones que administran Tareas.  |
|  vApp                    |  Un usuario de CloudAdmin tiene todos los privilegios de vApp.  |  --  |  --  |
|  Máquina virtual         |  Un usuario de CloudAdmin tiene todos los privilegios de Máquina virtual.  |  --  |  --  |
|  vService                |  Un usuario de CloudAdmin tiene todos los privilegios de vService.  |  --  |  --  |

## <a name="nsx-t-manager-access-and-identity"></a>Acceso e identidad de NSX-T Manager

Use la cuenta "administrador" para acceder a NSX-T Manager. Tiene privilegios completos y le permite crear y administrar enrutadores T1, conmutadores lógicos y todos los servicios. Los privilegios proporcionan acceso al enrutador NSX-T T0. Un cambio en el enrutador T0 podría provocar una degradación en el rendimiento de la red o una pérdida de acceso a la nube privada. Abra una solicitud de soporte técnico en Azure Portal para solicitar cambios en el enrutador NSX-T T0.
  
## <a name="next-steps"></a>Pasos siguientes

El siguiente paso es obtener información sobre los [conceptos de actualización de nubes privadas][concepts-upgrades].

<!-- LINKS - external -->

<!-- LINKS - internal -->
[concepts-upgrades]: ./concepts-upgrades.md