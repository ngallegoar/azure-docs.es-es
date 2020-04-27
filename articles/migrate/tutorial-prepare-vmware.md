---
title: Preparación de máquinas virtuales de VMware para la evaluación y migración con Azure Migrate
description: Aprenda a prepararse para la evaluación y migración de máquinas virtuales de VMware con Azure Migrate.
ms.topic: tutorial
ms.date: 04/19/2020
ms.custom: mvc
ms.openlocfilehash: a3f9716d6302c41f139d9a2a8b1f994b79afd199
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81677298"
---
# <a name="prepare-vmware-vms-for-assessment-and-migration-to-azure"></a>Preparación de máquinas virtuales de VMware para la evaluación y migración a Azure

Este artículo ayuda a preparar la valoración y migración de máquinas virtuales de VMware locales a Azure mediante [Azure Migrate](migrate-services-overview.md).



Este tutorial es el primero de una serie que muestra cómo evaluar y migrar máquinas virtuales de VMware. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Preparar Azure para trabajar con Azure Migrate.
> * Preparar VMware para la valoración de la máquina virtual con la herramienta Azure Migrate:Server Assessment.
> * Preparar VMware para la migración de la máquina virtual con la herramienta Azure Migrate:Server Migration. 

> [!NOTE]
> Los tutoriales muestran la ruta de implementación más sencilla para un escenario. Resultan útiles cuando aprende a configurar una implementación y como una prueba de concepto rápida. En ellos se usan las opciones predeterminadas siempre que es posible y no muestran todos los valores y rutas de acceso posibles. 

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.


## <a name="prepare-azure"></a>Preparación de Azure

Estos permisos son necesarios para poder realizar estas tareas en Azure antes de poder evaluar o migrar máquinas virtuales de VMware.

**Task** | **Detalles** 
--- | --- 
**Crear un proyecto de Azure Migrate** | La cuenta de Azure necesita permisos de colaborador o propietario para crear un proyecto. 
**Registrar proveedores de recursos** | Azure Migrate usa un dispositivo ligero para detectar y evaluar máquinas virtuales de VMware y migrarlas a Azure con Azure Migrate Server Assessment.<br/><br/> Durante el registro de la aplicación, los proveedores de recursos se registran con la suscripción elegida en la aplicación. [Más información](migrate-appliance-architecture.md#appliance-registration).<br/><br/> Para registrar los proveedores de recursos debe tener el rol colaborador o propietario de la suscripción.
**Crear aplicaciones de Azure AD** | Al registrar el dispositivo, Azure Migrate crea aplicaciones de Azure Active Directory (Azure AD). <br/><br/> - La primera aplicación se usa para la comunicación entre los agentes que se ejecutan en la aplicación y sus servicios respectivos que se ejecutan en Azure.<br/><br/> - La segunda aplicación se usa exclusivamente para acceder a la instancia de KeyVault creada en la suscripción del usuario para la migración de la máquina virtual de VMware sin agente. [Más información](migrate-appliance-architecture.md#appliance-registration).<br/><br/> Para crear aplicaciones de Azure AD, se necesitan permisos (disponibles en Desarrollador de aplicaciones).
**Crear un almacén de claves** | Para migrar máquinas virtuales de VMware mediante la migración sin agente, Azure Migrate crea un almacén de claves para administrar las claves de acceso a la cuenta de almacenamiento de replicación de la suscripción.<br/><br/> Para crear el almacén, necesita permisos de asignación de roles en el grupo de recursos en el que reside el proyecto de Azure Migrate.




### <a name="assign-permissions-to-create-project"></a>Asignación de permisos para crear un proyecto

1. En Azure Portal, abra la suscripción y seleccione **Control de acceso (IAM)** .
2. En **Comprobar acceso**, busque la cuenta correspondiente y haga clic en ella para ver los permisos.
3. Debe tener permisos de **Colaborador** o **Propietario**.
    - Si acaba de crear una cuenta de Azure gratuita, es el propietario de la suscripción.
    - Si no es el propietario, trabaje con él para asignar el rol.

### <a name="assign-permissions-to-register-the-appliance"></a>Asignación de permisos para registrar el dispositivo

Para registrar el dispositivo, asigne los permisos para Azure Migrate de modo que se creen las aplicaciones de Azure AD durante el registro del dispositivo. Los permisos se pueden asignar mediante uno de los métodos siguientes:

- **Conceder permisos**: Un administrador de inquilinos o administrador global puede conceder permisos a los usuarios del inquilino para crear y registrar aplicaciones de Azure AD.
- **Asignación del rol de desarrollador de aplicaciones**: Un administrador de inquilinos o administrador global puede asignar el rol de desarrollador de aplicaciones (que tiene los permisos) a la cuenta.

> [!NOTE]
> - Las aplicaciones no tienen otros permisos de acceso en la suscripción distintos de los descritos anteriormente.
> - Solo necesita estos permisos al registrar un nuevo dispositivo. Puede quitar los permisos una vez configurado el dispositivo.


#### <a name="grant-account-permissions"></a>Concesión de permisos de cuenta

Si desea que el administrador de inquilinos o global conceda permisos, haga lo siguiente:

1. En Azure AD, el administrador de inquilinos o global debe ir a **Azure Active Directory** > **Usuarios** > **Configuración de usuario**.
2. El administrador debe establecer **Registros de aplicaciones** en **Sí**. Se trata de una configuración predeterminada que no es confidencial. [Más información](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).

    ![Permisos de Azure AD](./media/tutorial-prepare-vmware/aad.png)



#### <a name="assign-application-developer-role"></a>Asignación del rol de desarrollador de aplicaciones

Como alternativa, el administrador de inquilinos o administrador global puede asignar el rol de desarrollador de aplicaciones a una cuenta. [Más información](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal) sobre la asignación de roles.

### <a name="assign-permissions-to-create-a-key-vault"></a>Asignación de permisos para crear un almacén de claves

Para habilitar Azure Migrate para crear un almacén de claves, asigne los permisos de la siguiente manera:

1. En el grupo de recursos de Azure Portal, seleccione **Control de acceso (IAM**).
2. En **Comprobar acceso**, busque la cuenta correspondiente y haga clic en ella para ver los permisos.

    - Para ejecutar Server Assessment, son suficientes los permisos de **colaborador**.
    - Para ejecutar la migración de un servidor sin agente, debe disponer de permisos de **propietario** (o **colaborador** y **administrador de acceso de usuario**).

3. Si no tiene los permisos necesarios, pídaselos al propietario del grupo de recursos.



## <a name="prepare-for-vmware-vm-assessment"></a>Preparación para la evaluación de máquinas virtuales de VMware

Para preparar la valoración de máquinas virtuales de VMware, tiene que:

- **Comprobar la configuración de VMware**. Asegúrese de que las máquinas virtuales y vCenter Server que desea migrar cumplan los requisitos.
- **Configuración de una cuenta para la valoración**. Azure Migrate usa esta cuenta para acceder a vCenter Server, con el fin de detectar máquinas virtuales para la valoración.
- **Comprobar los requisitos de los dispositivos**. Compruebe los requisitos de implementación de la aplicación de Azure Migrate antes de implementarlo.

### <a name="verify-vmware-settings"></a>Comprobación de la configuración de VMware

1. [Compruebe](migrate-support-matrix-vmware.md#vmware-requirements) los requisitos del servidor de VMware para la valoración.
2. [Asegúrese](migrate-support-matrix-vmware.md#port-access) de que los puertos necesarios están abiertos en vCenter Server.
3. En vCenter Server, asegúrese de que la cuenta tiene permisos para crear una máquina virtual mediante un archivo OVA. Puede implementar el dispositivo de Azure Migrate como una máquina virtual de VMware mediante un archivo OVA.


### <a name="set-up-an-account-for-assessment"></a>Configuración de una cuenta para la evaluación

Azure Migrate necesita acceder a vCenter Server para detectar máquinas virtuales para su evaluación y migración sin agente.

- Si tiene previsto detectar aplicaciones o visualizar dependencias sin agente, cree una cuenta de vCenter Server con acceso de solo lectura junto con los privilegios habilitados para **Virtual machines** > **Guest Operations** (Máquinas virtuales > Operaciones de invitado).

  ![Privilegios de cuenta de vCenter Server](./media/tutorial-prepare-vmware/vcenter-server-permissions.png)

- Si no tiene previsto realizar la detección de aplicaciones y la visualización de dependencias sin agente, configure una cuenta de solo lectura para vCenter Server.

### <a name="verify-appliance-settings-for-assessment"></a>Comprobación de la configuración del dispositivo para su evaluación

Antes de configurar el dispositivo de Azure Migrate y comenzar la evaluación en el siguiente tutorial, prepare la implementación del dispositivo.

1. [Compruebe](migrate-appliance.md#appliance---vmware) los requisitos de la aplicación de Azure Migrate.
2. Revise las direcciones URL de Azure a las que el dispositivo necesitará acceder en las nubes [públicas](migrate-appliance.md#public-cloud-urls) y [gubernamentales](migrate-appliance.md#government-cloud-urls).
3. [Revise los datos](migrate-appliance.md#collected-data---vmware) que la aplicación recopila durante la detección y valoración.
4. [Tenga en cuenta](migrate-support-matrix-vmware.md#port-access) los requisitos de acceso a puertos del dispositivo.




## <a name="prepare-for-agentless-vmware-migration"></a>Preparación para la migración de VMware sin agente

Revise los requisitos para la [migración sin agente](server-migrate-overview.md) de máquinas virtuales de VMware.

1. [Revise](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) los requisitos del servidor de VMware.
2. [Revise los permisos](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) que necesita Azure Migrate para acceder a vCenter Server.
3. [Revise](migrate-support-matrix-vmware-migration.md#agentless-vmware-vms) los requisitos de las máquinas virtuales de VMware.
4. [Revise](migrate-support-matrix-vmware-migration.md#agentless-azure-migrate-appliance) los requisitos de la aplicación de Azure Migrate.
5. Tenga en cuenta que es necesario acceder a la dirección URL para las nubes [públicas](migrate-appliance.md#public-cloud-urls) y [gubernamentales](migrate-appliance.md#government-cloud-urls).
6. Revise los requisitos de [acceso a los puertos](migrate-support-matrix-vmware-migration.md#agentless-ports).

## <a name="prepare-for-agent-based-vmware-migration"></a>Preparación para la migración de VMware basada en agente

Revise los requisitos para la [migración basada en agente](server-migrate-overview.md) de máquinas virtuales de VMware.

1. [Revise](migrate-support-matrix-vmware-migration.md#agent-based-vmware-servers) los requisitos del servidor de VMware.
2. [Revise los permisos](migrate-support-matrix-vmware-migration.md#agent-based-vmware-servers) que necesita Azure Migrate para acceder a vCenter Server.
2. [Revise](migrate-support-matrix-vmware-migration.md#agent-based-vmware-vms) los requisitos de las máquinas virtuales de VMware, entre los que se incluyen la instalación de Mobility Service en todas las máquinas virtuales que desee migrar.
3. La migración basada en agente usa una aplicación de replicación:
    - [Revise](migrate-replication-appliance.md#appliance-requirements) los requisitos de implementación de la aplicación de replicación.
    - [Revise las opciones](migrate-replication-appliance.md#mysql-installation) para instalar MySQL en la aplicación.
    - Tenga en cuenta que es necesario acceder a la dirección URL para las nubes [públicas](migrate-replication-appliance.md#url-access) y [gubernamentales](migrate-replication-appliance.md#azure-government-url-access).
    - Revise los requisitos de [acceso a los puertos](migrate-replication-appliance.md#port-access) del dispositivo de replicación.
    
## <a name="next-steps"></a>Pasos siguientes

En este tutorial, hizo lo siguiente:

> [!div class="checklist"]
> * Configuró permisos de Azure.
> * Preparó VMware para la migración y evaluación.


Continúe con el segundo tutorial para configurar un proyecto de Azure Migrate y evaluar las máquinas virtuales de VMware para la migración a Azure.

> [!div class="nextstepaction"]
> [Evaluación de máquinas virtuales de VMware](./tutorial-assess-vmware.md)
