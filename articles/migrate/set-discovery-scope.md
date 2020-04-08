---
title: Establezca el ámbito para la detección de máquinas virtuales de VMware con Azure Migrate
description: Describe cómo establecer el ámbito de detección para la evaluación y la migración de máquinas virtuales de VMware con Azure Migrate.
ms.topic: how-to
ms.date: 03/23/2020
ms.openlocfilehash: 29b3077ead168cef2790468d6ac62d1c59c24c11
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80337354"
---
# <a name="set-discovery-scope-for-vmware-vms"></a>Establecimiento del ámbito de detección para las máquinas virtuales de VMware

En este artículo se describe cómo limitar el ámbito de detección de las máquinas virtuales de VMware detectadas por el [dispositivo de Azure Migrate](migrate-appliance-architecture.md).

El dispositivo de Azure Migrate detecta máquinas virtuales de VMware locales en estos casos: 

- Al usar la herramienta [Azure Migrate Server Assessment](migrate-services-overview.md#azure-migrate-server-assessment-tool) para evaluar las máquinas virtuales de VMware para la migración a Azure.
- Al usar la herramienta [Azure Migrate Server Migration](migrate-services-overview.md#azure-migrate-server-migration-tool) para la [migración sin agente](server-migrate-overview.md) de máquinas virtuales de VMware a Azure.

## <a name="set-discovery-scope"></a>Establecimiento del ámbito de detección


Después de configurar el dispositivo de Azure Migrate para la evaluación o la migración de máquinas virtuales de VMware, el dispositivo comienza a detectar máquinas virtuales y a enviar metadatos de máquinas virtuales a Azure. Antes de conectar el dispositivo a vCenter Server para la detección, puede establecer el ámbito de detección para limitar la detección a centros de datos, clústeres, una carpeta de clústeres, hosts, una carpeta de hosts o máquinas virtuales concretas de vCenter Server.

Para establecer el ámbito, debe asignar permisos en la cuenta que usa Azure Migrate para obtener acceso a vCenter Server.

## <a name="create-a-vcenter-user-account"></a>Creación de una cuenta de usuario de vCenter

Si aún no ha configurado una cuenta de usuario de vCenter que el dispositivo de Azure Migrate use para detectar, evaluar y migrar máquinas virtuales de VMware, hágalo en primer lugar.

1.    Inicie sesión en el cliente web de vSphere como administrador de vCenter Server.
2.    Seleccione **Administration** > **SSO users and Groups** (Administración > Usuarios y grupos de inicio de sesión único) y haga clic en la pestaña **Users** (Usuarios).
3.    Seleccione el icono **New User** (Nuevo usuario).
4.    Rellene la información del usuario nuevo > **OK** (Aceptar).

Los permisos de cuenta dependen de lo que se vaya a implementar.

**Característica** | **Permisos de cuenta**
--- | ---
[Evaluar](tutorial-assess-vmware.md)| La cuenta necesita acceso de solo lectura.
[Detección de aplicaciones/roles/características](how-to-discover-applications.md) | La cuenta necesita acceso de solo lectura, con privilegios habilitados para Máquinas virtuales > Operaciones de invitado.
[Análisis de las dependencias (sin agente)](how-to-create-group-machine-dependencies-agentless.md) | La cuenta necesita acceso de solo lectura, con privilegios habilitados para Máquinas virtuales > Operaciones de invitado.
[Migración (sin agente)](tutorial-migrate-vmware.md) | Necesita un rol que tenga asignados los permisos adecuados.<br/><br/> Para crear un rol, inicie sesión en el cliente web de vSphere como administrador de vCenter Server. Seleccione la instancia de vCenter Server > **Crear rol**. Especifique un nombre de rol (por ejemplo, <em>Azure_Migrate</em>) y asigne los [permisos necesarios](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) al rol.


## <a name="assign-permissions-on-vcenter-objects"></a>Asignación de permisos de los objetos de vCenter

Puede asignar permisos en objetos de inventario mediante uno de estos dos métodos:

- Asigne un rol con los permisos necesarios a la cuenta que está usando para todos los objetos primarios que hospedan máquinas virtuales que quiera detectar o migrar.
- Como alternativa, puede asignar el rol y la cuenta de usuario a nivel de centro de datos y propagarlos a los objetos secundarios. A continuación, conceda a la cuenta el rol **Sin acceso** para cada objeto que no quiera detectar o migrar. No se recomienda este enfoque porque resulta complicado y podría exponer los controles de acceso, ya que a cada objeto secundario nuevo se le concede automáticamente acceso heredado del objeto primario.

Haga lo siguiente para asignar un rol a la cuenta que está usando para todos los objetos pertinentes:

- **Para la evaluación**: En cuanto a la evaluación de máquinas virtuales, aplique el rol **Solo lectura** a la cuenta de usuario de vCenter para todos los objetos primarios que hospeden máquinas virtuales que quiera detectar. Objetos primarios incluidos: host, carpeta de hosts, clúster y carpeta de clústeres en la jerarquía hasta el centro de datos. Propague estos permisos a los objetos secundarios de la jerarquía.

    ![Asignación de permisos](./media/tutorial-assess-vmware/assign-perms.png)

- **Para la migración sin agente**: En cuanto a la migración sin agente, aplique un rol definido por el usuario con [permisos necesarios](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) a la cuenta de usuario para todos los objetos primarios que hospeden máquinas virtuales que quiera detectar. Puede asignar al rol el nombre <em>Azure_Migrate</em>.

### <a name="scope-support"></a>Compatibilidad de los ámbitos

En la actualidad, la herramienta Server Assessment no puede detectar máquinas virtuales si a la cuenta de vCenter se le ha concedido acceso a nivel de carpeta de máquina virtual de vCenter. Se admiten carpetas de hosts y clústeres.

Si quiere limitar el ámbito de detección por carpetas de máquina virtual, lleve a cabo el siguiente procedimiento para asegurarse de que la cuenta de vCenter tiene asignado acceso de solo lectura a nivel de máquina virtual.

1. Asigne permisos de solo lectura en todas las máquinas virtuales de las carpetas de las máquinas virtuales para las que quiere limitar el ámbito de la detección.
2. Conceda acceso de solo lectura a todos los objetos primarios que hospeden máquinas virtuales.
    - Se incluyen todos los objetos primarios (host, carpeta de hosts, clúster, carpeta de clústeres) de la jerarquía hasta el centro de datos.
    - No es necesario propagar los permisos a todos los objetos secundarios.
3. Use las credenciales para la detección mediante la selección del centro de datos como **Ámbito del grupo**. El control de acceso basado en rol configurado garantiza que la cuenta de usuario correspondiente de vCenter solo tenga acceso a las máquinas virtuales específicas del inquilino.


## <a name="next-steps"></a>Pasos siguientes

[Configuración del dispositivo](how-to-set-up-appliance-vmware.md) e [inicio de la detección continua](how-to-set-up-appliance-vmware.md#start-continuous-discovery-by-providing-vcenter-server-and-vm-credential).
