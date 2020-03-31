---
title: Migración de máquinas virtuales de VMware con la migración de servidores basada en agentes de Azure Migrate
description: Aprenda a ejecutar una migración de máquinas virtuales de VMware basada en agentes con Azure Migrate.
ms.topic: tutorial
ms.date: 03/09/2020
ms.custom: MVC
ms.openlocfilehash: 64873c5185660c58cd4d07d60df3d086364d6288
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "79222032"
---
# <a name="migrate-vmware-vms-to-azure-agent-based"></a>Migración de máquinas virtuales de VMware a Azure (basada en agente)

En este artículo se muestra cómo migrar máquinas virtuales de VMware locales a Azure mediante la migración basada en agente con la herramienta Azure Migrate Server Migration.

[Azure Migrate](migrate-services-overview.md) proporciona un centro de conectividad para realizar el seguimiento de la detección, evaluación y migración a Azure de las aplicaciones y cargas de trabajo locales, así como de las instancias de máquinas virtuales de AWS o GCP. El centro proporciona herramientas de Azure Migrate para la evaluación y la migración, así como ofertas de proveedores de software independientes (ISV).


En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Configurar el entorno de origen e implementar un dispositivo de replicación de Azure Migrate para la migración basada en agente.
> * Configurar el entorno de destino para la migración.
> * Configurar una directiva de replicación.
> * Habilite la replicación.
> * Ejecute una migración de prueba para asegurarse de que todo funciona de la forma esperada.
> * Ejecutar una migración completa a Azure.

> [!NOTE]
> En los tutoriales se muestra la ruta de implementación más sencilla para un escenario, de modo que pueda configurar rápidamente una prueba de concepto. En ellos se usan las opciones predeterminadas siempre que es posible y no muestran todos los valores y rutas de acceso posibles. Para obtener instrucciones detalladas, revise los procedimientos para la evaluación y la migración de VMware.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.

## <a name="before-you-begin"></a>Antes de empezar

Se recomienda probar la evaluación de máquinas virtuales de VMware con Azure Migrate Server Assessment antes de migrar las máquinas virtuales a Azure. Configure una evaluación como se indica a continuación:

1. Siga el tutorial para [preparar Azure y VMware](tutorial-prepare-vmware.md) para su evaluación.
2. A continuación, siga [este tutorial](tutorial-assess-vmware.md) para configurar un dispositivo de Azure Migrate para su evaluación y para la detección y evaluación de máquinas virtuales.


Aunque se recomienda probar una evaluación, no es necesario ejecutarla antes de migrar las máquinas virtuales.

## <a name="migration-methods"></a>Métodos de migración

Puede migrar las máquinas virtuales de VMware a Azure mediante la herramienta Azure Migrate Server Migration. Esta herramienta ofrece un par de opciones para la migración de máquinas virtuales de VMware:

- Replicación sin agente. Las máquinas virtuales se migran sin necesidad de instalar nada en ellas.
- Migración o replicación basada en agente. Para la replicación se instala un agente (el agente de Mobility Service) en la máquina virtual.

Para decidir si desea utilizar la migración sin agente o basada en agente, consulte estos artículos:

- [Más información](server-migrate-overview.md) sobre las opciones de migración de VMware.
- [Comparación de métodos de migración](server-migrate-overview.md#compare-migration-methods).
- [Siga este artículo](tutorial-migrate-vmware.md) para probar la migración sin agente.



## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar este tutorial, debe:

1. [Revisar](migrate-architecture.md) la arquitectura de migración de VMware.
2. Asegurarse de que la cuenta de Azure tiene asignado el rol Colaborador de máquina virtual, con el fin de que tenga permisos para:

    - Crear una máquina virtual en el grupo de recursos seleccionado.
    - Crear una máquina virtual en la red virtual seleccionada.
    - Escribir en un disco administrado de Azure. 

3. [Configure una red de Azure](../virtual-network/manage-virtual-network.md#create-a-virtual-network). Las máquinas locales se replican en los discos administrados de Azure. Al conmutar por error a Azure para la migración, las máquinas virtuales de Azure se crean a partir de estos discos administrados y se unen a una red de Azure que se especifica cuando se configura la migración.


## <a name="prepare-azure"></a>Preparación de Azure

Si ya ha ejecutado una evaluación con la herramienta de evaluación de servidores de Azure Migrate, puede ignorar las instrucciones de esta sección, puesto que ya ha completado estos pasos. 

Si no ha ejecutado una evaluación, debe configurar los permisos de Azure para poder migrar con la herramienta Azure Migrate Server Migration.

- **Crear un proyecto**: la cuenta de Azure necesita permisos para crear un proyecto de Azure Migrate. 
- **Registrar el dispositivo de replicación de Azure Migrate**: el dispositivo de replicación crea y registra una aplicación de Azure Active Directory en la cuenta de Azure. Delegue permisos para ello.
- **Crear un almacén de claves**: Cuando se migran máquinas virtuales de VMware con Azure Migrate Server Migration, Azure Migrate crea un almacén de claves en el grupo de recursos para administrar las claves de acceso a la cuenta de almacenamiento de replicación de la suscripción. Para crear el almacén, necesita permisos de asignación de roles en el grupo de recursos en el que reside el proyecto de Azure Migrate. 


### <a name="assign-permissions-to-create-project"></a>Asignación de permisos para crear un proyecto

1. En Azure Portal, abra la suscripción y seleccione **Control de acceso (IAM)** .
2. En **Comprobar acceso**, busque la cuenta correspondiente y haga clic en ella para ver los permisos.
3. Debe tener permisos de **Colaborador** o **Propietario**.
    - Si acaba de crear una cuenta de Azure gratuita, es el propietario de la suscripción.
    - Si no es el propietario, trabaje con él para asignar el rol.

### <a name="assign-permissions-to-register-the-replication-appliance"></a>Asignación de permisos para registrar el dispositivo de replicación

En el caso de la migración basada en agente, debe delegar permisos para que la herramienta Azure Migrate Server Migration pueda crear y registrar una aplicación de Azure AD en su cuenta. Puede asignar permisos mediante uno de los métodos siguientes:

- Un administrador de inquilinos o administrador global puede conceder permisos a los usuarios del inquilino para crear y registrar aplicaciones de Azure AD.
- Un administrador de inquilinos o administrador global puede asignar el rol de desarrollador de aplicaciones (que tiene los permisos) a la cuenta.

Merece la pena mencionar que:

- Las aplicaciones no tienen otros permisos de acceso en la suscripción distintos de los descritos anteriormente.
- Solo necesita estos permisos al registrar un nuevo dispositivo de replicación. Puede quitar los permisos una vez configurado el dispositivo de replicación. 


#### <a name="grant-account-permissions"></a>Concesión de permisos de cuenta

El administrador de inquilinos o administrador global puede conceder permisos como se indica:

1. En Azure AD, el administrador de inquilinos o global debe ir a **Azure Active Directory** > **Usuarios** > **Configuración de usuario**.
2. El administrador debe establecer **Registros de aplicaciones** en **Sí**.

    ![Permisos de Azure AD](./media/tutorial-prepare-vmware/aad.png)

> [!NOTE]
> Se trata de una configuración predeterminada que no es confidencial. [Más información](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).

#### <a name="assign-application-developer-role"></a>Asignación del rol de desarrollador de aplicaciones 

El administrador de inquilinos o administrador global puede asignar el rol de desarrollador de aplicaciones a una cuenta. [Más información](../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md).

## <a name="assign-permissions-to-create-key-vault"></a>Asignación de permisos para crear el almacén de claves

Asigne permisos de asignación de roles en el grupo de recursos en el que reside el proyecto de Azure Migrate, como se indica a continuación:

1. En el grupo de recursos de Azure Portal, seleccione **Control de acceso (IAM**).
2. En **Comprobar acceso**, busque la cuenta correspondiente y haga clic en ella para ver los permisos. Necesita permisos de **Propietario** (o **Colaborador** y **Administrador de acceso de usuario**).
3. Si no tiene los permisos necesarios, pídaselos al propietario del grupo de recursos. 


## <a name="prepare-on-premises-vmware"></a>Preparación de instancias locales de VMware

### <a name="prepare-an-account-for-automatic-discovery"></a>Preparación de una cuenta de detección automática

La herramienta Azure Migrate Server Migration necesita acceso a los servidores de VMware para:

- Detectar automáticamente las máquinas virtuales. Se requiere al menos una cuenta de solo lectura.
- Coordinar la replicación, la conmutación por error y la conmutación por recuperación. Necesita una cuenta que pueda ejecutar operaciones como la creación y la eliminación de discos, así como la activación de máquinas virtuales.

Cree la cuenta como se indica a continuación:

1. Para usar una cuenta dedicada, cree un rol en el nivel de vCenter. Asigne al rol un nombre como **Azure_Site_Recovery**.
2. Asigne al rol los permisos que se resumen en la tabla siguiente.
3. Cree un usuario en el servidor vCenter o el host de vSphere. Asigne el rol al usuario.

#### <a name="vmware-account-permissions"></a>Permisos de cuenta de VMware

**Task** | **Rol o permisos** | **Detalles**
--- | --- | ---
**Detección de máquina virtual** | Al menos un usuario de solo lectura<br/><br/> Objeto de centro de datos  –> Propagar al objeto secundario, rol = solo lectura | El usuario se asigna en el nivel de centro de datos y tiene acceso a todos los objetos de este.<br/><br/> Para restringir el acceso, asigne el rol **Sin acceso** con **Propagate to child object** (Propagar a objeto secundario) a los objetos secundarios (hosts de vSphere, almacenes de datos, máquinas virtuales y redes).
**Replicación completa, conmutación por error, conmutación por recuperación** |  Cree un rol (Azure_Site_Recovery) con los permisos necesarios y, después, asígnelo a un grupo o usuario de VMware.<br/><br/> Objeto de centro de datos –> Propagar a objeto secundario, rol = Azure_Site_Recovery<br/><br/> Almacén de datos -> Asignar espacio, examinar almacén de datos, operaciones de archivo de bajo nivel, quitar archivo, actualizar archivos de máquina virtual<br/><br/> Red -> Asignación de red<br/><br/> Recursos-> Asignar máquina virtual al grupo de recursos, migrar apagado de máquina virtual, migrar máquinas virtuales encendidas<br/><br/> Tareas -> Crear tarea, actualizar tarea<br/><br/> Máquina virtual -> Configuración<br/><br/> Máquina virtual -> Interactuar -> Responder a pregunta, conexión de dispositivos, configurar soporte de CD, Configurar soporte de disquete, apagar, encender, instalación de herramientas de VMware<br/><br/> Máquina virtual -> Inventario -> Crear, registrar, anular registro<br/><br/> Máquina virtual -> Aprovisionamiento -> Permitir descarga de máquina virtual, permitir carga de archivos de máquina virtual<br/><br/> Máquina virtual -> Instantáneas -> Quitar instantáneas | El usuario se asigna en el nivel de centro de datos y tiene acceso a todos los objetos de este.<br/><br/> Para restringir el acceso, asigne el rol **Sin acceso** con **Propagate to child object** (Propagar a objeto secundario) a los objetos secundarios (hosts de vSphere, almacenes de datos, máquinas virtuales y redes).

### <a name="prepare-an-account-for-mobility-service-installation"></a>Preparación de una cuenta para la instalación de Mobility Service

Mobility Service debe estar instalado en las máquinas que desee replicar.

- El dispositivo de replicación de Azure Migrate puede realizar una instalación de inserción de este servicio cuando se habilita la replicación de una máquina, o bien puede instalarlo manualmente o mediante herramientas de instalación.
- En este tutorial, se va a instalar Mobility Service con la instalación de inserción.
- Para esta instalación de inserción, debe preparar una cuenta que la herramienta Azure Migrate Server Migration pueda usar para acceder a la máquina virtual. Esta cuenta se usa solo para la instalación de inserción si no instala Mobility Service de forma manual.

Prepare la cuenta como se indica a continuación:

1. Prepare un dominio o cuenta local con permisos para instalar en la máquina virtual.
2. En máquinas virtuales Windows, si no va a usar una cuenta de dominio, deshabilite el control de acceso de usuario remoto en la máquina local: agregue la entrada DWORD **LocalAccountTokenFilterPolicy**, con un valor de 1, en el Registro, bajo **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**.
3. En máquinas virtuales Linux, prepare una cuenta raíz en el servidor Linux de origen.


### <a name="check-vmware-requirements"></a>Comprobación de los requisitos de VMware

Asegúrese de que los servidores y las máquinas virtuales de VMware cumplen los requisitos de migración a Azure. 


> [!NOTE]
> La migración basada en agente con la herramienta Azure Migrate Server Migration se basa en las características del servicio Azure Site Recovery. Es posible que algunos requisitos lleven a la documentación de Site Recovery.

1. [Compruebe](migrate-support-matrix-vmware-migration.md#agent-based-vmware-servers) los requisitos del servidor de VMware.
2. [Compruebe](migrate-support-matrix-vmware-migration.md#agent-based-vmware-vms) los requisitos de compatibilidad de la máquina virtual para la migración.
3. Compruebe la configuración de la máquina virtual. Las máquinas virtuales locales que replique en Azure tienen que cumplir los [requisitos de máquina virtual de Azure](migrate-support-matrix-vmware-migration.md#azure-vm-requirements).



## <a name="add-the-azure-migrate-server-migration-tool"></a>Incorporación de la herramienta Azure Migrate Server Migration

Si no ha seguido el tutorial para evaluar las máquinas virtuales de VMware, configure un proyecto de Azure Migrate y, a continuación, agregue la herramienta Azure Migrate Server Migration:

1. En Azure Portal > **Todos los servicios**, busque **Azure Migrate**.
2. En **Servicios**, seleccione **Azure Migrate**.

    ![Configuración de Azure Migrate](./media/tutorial-migrate-vmware-agent/azure-migrate-search.png)

3. En **Información general**, haga clic en **Evaluar y migrar servidores**.
4. En **Detectar, evaluar y migrar servidores**, haga clic en **Evaluar y migrar servidores**.

    ![Detección y evaluación de servidores](./media/tutorial-migrate-vmware-agent/assess-migrate.png)

1. En **Detectar, evaluar y migrar servidores**, haga clic en **Agregar herramientas**.
2. En **Migrar proyecto**, seleccione la suscripción a Azure y cree un grupo de recursos, en caso de que no lo tenga.
3. En **Detalles del proyecto**, especifique el nombre del proyecto y la zona geográfica en la que desea crearlo; después, haga clic en **Siguiente**.

    ![Crear un proyecto de Azure Migrate](./media/tutorial-migrate-vmware-agent/migrate-project.png)

    Puede crear un proyecto de Azure Migrate en cualquiera de estas zonas geográficas.

    **Geografía** | **Región**
    --- | ---
    Asia | Sudeste de Asia
    Europa | Norte de Europa y Oeste de Europa
    Estados Unidos | Centro-oeste de EE. UU. o Este de EE. UU.

    La ubicación geográfica especificada para el proyecto solo se utiliza para almacenar los metadatos que se recopilan a partir de máquinas virtuales locales. Puede seleccionar cualquier región de destino para la migración real.
4. En **Seleccione una herramienta de evaluación**, seleccione **Omitir por ahora la adición de una herramienta de evaluación** > **Siguiente**.
5. En **Seleccione una herramienta de migración**, seleccione **Azure Migrate: Migración del servidor** > **Siguiente**.
6. En **Revisar y agregar herramientas**, revise la configuración y haga clic en **Agregar herramientas**.
7. Después de agregar la herramienta, aparece en el proyecto de Azure Migrate > **Servidores** > **Herramientas de migración**.

## <a name="set-up-the-replication-appliance"></a>Configuración del dispositivo de replicación

El primer paso de la migración consiste en configurar el dispositivo de replicación. El dispositivo de replicación es una máquina virtual local de VMware única, de alta disponibilidad, que hospeda estos componentes:

- **Servidor de configuración**: El servidor de configuración coordina la comunicación entre el entorno local y Azure, además de administrar la replicación de datos.
- **Servidor de proceso**: El servidor de procesos actúa como puerta de enlace de replicación. Recibe los datos de la replicación; los optimiza mediante el almacenamiento en la caché, la compresión y el cifrado, y los envía a una cuenta de almacenamiento en Azure. El servidor de procesos también instala el agente de Mobility Service en las máquinas virtuales que se desean replicar, además de realizar la detección automática de las máquinas virtuales de VMware locales.


Para configurar el dispositivo de replicación, debe descargar una plantilla de Open Virtualization Application (OVA) preparada. Después debe importar la plantilla en VMware y crear la máquina virtual del dispositivo de replicación. 

### <a name="download-the-replication-appliance-template"></a>Descarga de la plantilla del dispositivo de replicación

Descargue la plantilla de la siguiente manera:

1. En el proyecto de Azure Migrate, haga clic en **Servidores**, en **Objetivos de migración**.
2. En la página **Azure Migrate - Servidores** > **Azure Migrate: Server Migration**, haga clic en **Detectar**.

    ![Detectar máquinas virtuales](./media/tutorial-migrate-vmware-agent/migrate-discover.png)

3. En **Detectar máquinas** >  **¿Las máquinas están virtualizadas?** , haga clic en **Sí, con VMware vSphere Hypervisor**.
4. En **¿Cómo quiere realizar la migración?** , seleccione **Mediante la replicación basada en agente**.
5. En **Región de destino**, seleccione la región de Azure a la que desea migrar las máquinas.
6. Seleccione **Confirme que la región de destino de la migración es nombreDeRegión**.
7. Haga clic en **Crear recursos**. Esto crea un almacén de Azure Site Recovery en segundo plano.
    - Después de hacer clic en este botón ya no se puede cambiar la región de destino de este proyecto.
    - Todas las migraciones posteriores se realizan a esta región.

    ![Crear almacén de Recovery Services](./media/tutorial-migrate-vmware-agent/create-resources.png)

8. En **¿Quiere instalar un nuevo dispositivo de replicación?** , seleccione **Instalar un dispositivo de replicación**.
9. Haga clic en **Descargar**, para descargar el dispositivo de replicación. Se descarga una plantilla OVF que se usa para crear una nueva máquina virtual de VMware que ejecuta el dispositivo.
    ![Descarga de OVA](./media/tutorial-migrate-vmware-agent/download-ova.png)
10. Anote el nombre del grupo de recursos y el almacén de Recovery Services. Los necesitará durante la implementación del dispositivo.


### <a name="import-the-template-in-vmware"></a>Importación de la plantilla en VMware

Después de descargar la plantilla OVF, puede importarla en VMware para crear la aplicación de replicación en una máquina virtual de VMware que ejecute Windows Server 2016.

1. Inicie sesión en el servidor VMware vCenter o el host vSphere ESXi con el cliente de VMWare vSphere.
2. En el menú **File** (Archivo), seleccione **Deploy OVF Template** (Implementar plantilla OVF) para iniciar el **Asistente para implementar plantillas OVF**. 
3. En **Select source** (Seleccionar origen), especifique la ubicación de la plantilla OVF descargada.
4. En **Review details** (Revisar detalles), seleccione **Next** (Siguiente).
5. En **Select name and folder** (Seleccionar nombre y la carpeta) y **Select configuration** (Seleccionar configuración), acepte la configuración predeterminada.
6. En **Select storage** > **Select virtual disk format** (Seleccionar almacenamiento > Seleccionar formato de disco virtual), elija **Thick Provision Eager Zeroed** (Aprovisionamiento grueso diligente con ceros) para obtener un rendimiento mejor.
7. En el resto de las páginas del asistente, acepte la configuración predeterminada.
8. En **Ready to Complete** (Listo para completarse), para configurar la máquina virtual con la configuración predeterminada, seleccione **Power on after deployment** > **Finish** (Encender después de la implementación > Finalizar).

   > [!TIP]
   > Si desea agregar una NIC adicional, desactive **Power on after deployment** > **Finish** (Encender después de la implementación > Finalizar). De forma predeterminada, la plantilla contiene una sola NIC. Puede agregar NIC adicionales después de la implementación.

### <a name="kick-off-replication-appliance-setup"></a>Inicio de la configuración del dispositivo de replicación

1. Desde la consola de cliente de VMWare vSphere, encienda la máquina virtual.
2. La máquina virtual se inicia en una experiencia de instalación de Windows Server 2016. Acepte el contrato de licencia y especifique una contraseña de administrador.
3. Una vez finalizada la instalación, inicie sesión en la máquina virtual como administrador, con la contraseña admin.
4. La primera vez que inicia sesión, tras unos segundos se inicia la herramienta de configuración del dispositivo de replicación (la herramienta de configuración de Azure Site Recovery).
5. Escriba el nombre que usará para registrar el dispositivo con la herramienta Azure Migrate Server Migration. A continuación, haga clic en **Siguiente**.
6. La herramienta comprueba que la máquina virtual pueda conectarse a Azure. Una vez establecida la conexión, haga clic en **Iniciar sesión** para iniciar sesión en la suscripción de Azure.
7. Espere a que la herramienta termine de registrar una aplicación de Azure AD para identificar el dispositivo. El dispositivo se reinicia.
1. Inicie sesión de nuevo en la máquina. En pocos segundos, el Asistente para administración del servidor de configuración se inicia automáticamente.

### <a name="register-the-replication-appliance"></a>Registro del dispositivo de replicación

Termine de configurar y registrar el dispositivo de replicación.

1. En el Asistente para administración del servidor de configuración, seleccione **Configuración de la conectividad**.
2. Seleccione la NIC (de forma predeterminada solo hay una NIC) que el dispositivo de replicación usa para la detección de máquinas virtuales y para realizar una instalación de inserción de Mobility Service en las máquinas de origen.
3. Seleccione la NIC que el dispositivo de replicación usa para la conectividad con Azure. Después, seleccione **Guardar**. Una vez configurada, esta opción no se puede cambiar.
4. Si el dispositivo se encuentra detrás de un servidor proxy, debe especificar la configuración de proxy.
    - Especifique el nombre del proxy como **http://ip-address** o **http://FQDN** . No se admiten servidores proxy HTTPS.
5. Cuando se le pidan los detalles de la suscripción, los grupos de recursos y el almacén, agregue los datos que anotó cuando descargó la plantilla del dispositivo.
6. En **Instalar software de terceros**, acepte el contrato de licencia. Seleccione **Descargar e instalar** para instalar MySQL Server.
7. Seleccione **Install VMware PowerCLI** (Instalar VMware PowerCLI). Asegúrese de que todas las ventanas del explorador están cerradas antes. Después, seleccione **Continuar**.
8. En **Validate appliance configuration** (Comprobar configuración del dispositivo), se comprueban los requisitos previos antes de continuar.
9. En **Configure vCenter Server/vSphere ESXi server** (Configurar vCenter Server/vSphere ESXi Server), especifique la dirección IP o FQDN del servidor vCenter o host de vSphere en los que se encuentran las máquinas virtuales donde desea realizar la replicación. Especifique el puerto en el que escucha el servidor. Escriba un nombre descriptivo que se usará para el servidor de VMware en el almacén.
10. Escriba las credenciales de la cuenta que [creó](#prepare-an-account-for-automatic-discovery) para la detección de VMware. Seleccione **Add** > **Continue** (Agregar > Continuar).
11. En **Configuración de credenciales de máquina virtual**, escriba las credenciales que [creó](#prepare-an-account-for-mobility-service-installation) para la instalación de inserción de Mobility Service, al habilitar la replicación para las máquinas virtuales.  
    - Para las máquinas Windows, la cuenta necesita privilegios de administrador local en las máquinas que se vayan a replicar.
    - Para Linux, proporcione los detalles de la cuenta raíz.
12. Seleccione **Finalize configuration** (Terminar configuración) para completar el registro.


Una vez registrado el dispositivo de replicación, Azure Migrate Server Assessment se conecta a los servidores de VMware con la configuración especificada y detecta las máquinas virtuales. Puede ver las máquinas virtuales detectadas en **Administrar** > **Elementos detectados**, en la pestaña **Otros**.


## <a name="replicate-vms"></a>Replicación de máquinas virtuales

Ahora, seleccione las máquinas virtuales para la migración.

> [!NOTE]
> Puede replicar hasta 10 máquinas juntas. Si necesita replicar más, replíquelas simultáneamente en lotes de 10.

1. En el proyecto de Azure Migrate > **Servidores**, **Azure Migrate: Migración del servidor**, haga clic en **Replicar**.

    ![Replicación de máquinas virtuales](./media/tutorial-migrate-vmware-agent/select-replicate.png)

2. En **Replicar** > **Configuración de origen** >  **¿Las máquinas están virtualizadas?** , seleccione **Sí, con VMware vSphere Hypervisor**.
3. En **Dispositivo local**, seleccione el nombre del dispositivo de Azure Migrate que configuró.
4. En **vCenter Server**, especifique el nombre de la instancia de vCenter Server que administra las máquinas virtuales o la instancia de vSphere Server en el que se hospedan.
5. En **Servidor de procesos**, seleccione el nombre del dispositivo de replicación.
6. En **Credenciales de invitado**, especifique la cuenta de administrador de máquinas virtuales que se usará para la instalación de inserción de Mobility Service. A continuación, haga clic en **Siguiente: Máquinas virtuales**.

    ![Replicación de máquinas virtuales](./media/tutorial-migrate-vmware-agent/source-settings.png)

7. En **Máquinas virtuales**, seleccione las máquinas que quiere replicar.

    - Si ha ejecutado una evaluación para las máquinas virtuales, puede aplicar las recomendaciones de tamaño y tipo de disco (Premium/estándar) de máquina virtual que sugieren los resultados de dicha evaluación. Para ello, en **¿Quiere importar la configuración de migración de evaluación de Azure Migrate?** , seleccione la opción **Sí**.
    - Si no ha ejecutado una evaluación o no desea usar la configuración de evaluación, seleccione la opción **No**.
    - Si ha decidido usar la evaluación, seleccione el grupo de máquinas virtuales y el nombre de la evaluación.

8. Compruebe todas las máquinas virtuales que desea migrar. A continuación, haga clic en **Siguiente: Configuración de destino**.
9. En **Configuración de destino**, seleccione la suscripción y la región de destino a la que va a migrar, y especifique el grupo de recursos en el que residirán las máquinas virtuales de Azure después de la migración.
10. En **Red virtual**, seleccione la red virtual o la subred de Azure a la que se unirán las máquinas virtuales de Azure después de la migración.
11. En **Ventaja híbrida de Azure**:

    - Seleccione **No** si no desea aplicar la Ventaja híbrida de Azure. A continuación, haga clic en **Siguiente**.
    - Seleccione **Sí** si tiene equipos con Windows Server que están incluidos en suscripciones activas de Software Assurance o Windows Server y desea aplicar el beneficio a las máquinas que va a migrar. A continuación, haga clic en **Siguiente**.

12. En **Proceso**, revise el nombre de la máquina virtual, su tamaño, el tipo de disco del sistema operativo y el conjunto de disponibilidad. Las máquinas virtuales deben cumplir los [requisitos de Azure](migrate-support-matrix-vmware-migration.md#agent-based-vmware-vms).

    - **Tamaño de VM**: si usa las recomendaciones de la evaluación, el menú desplegable de tamaño de VM contendrá el tamaño recomendado. De lo contrario, Azure Migrate elige un tamaño en función de la coincidencia más cercana en la suscripción de Azure. También puede elegir un tamaño de manera manual en **Tamaño de la máquina virtual de Azure**. 
    - **Disco del sistema operativo**: especifique el disco del sistema operativo (arranque) de la máquina virtual. Este es el disco que tiene el cargador de arranque y el instalador del sistema operativo. 
    - **Conjunto de disponibilidad**: si la máquina virtual debe estar incluida en un conjunto de disponibilidad de Azure después de la migración, especifique el conjunto. El conjunto debe estar en el grupo de recursos de destino que especifique para la migración.

13. En **Discos**, especifique si los discos de máquina virtual se deben replicar en Azure y seleccione el tipo de disco (discos SSD o HDD estándar o bien discos administrados premium) en Azure. A continuación, haga clic en **Siguiente**.
    - Puede excluir discos de la replicación.
    - Si excluye discos, no estarán presentes en la máquina virtual de Azure después de la migración. 

14. En **Revisar e iniciar la replicación**, revise la configuración y haga clic en **Replicar** para iniciar la replicación inicial de los servidores.

> [!NOTE]
> Puede actualizar la configuración de replicación en cualquier momento antes de que esta comience; para ello, vaya a **Administrar** > **Replicación de máquinas**. Una vez iniciada la replicación, su configuración no se puede cambiar.




## <a name="track-and-monitor"></a>Seguimiento y supervisión

- Al hacer clic en **Replicar**, comienza el trabajo de inicio de replicación. 
- Cuando el trabajo de inicio de replicación finaliza correctamente, las máquinas comienzan su replicación inicial en Azure.
- Cuando finaliza esta replicación inicial, comienza la replicación diferencial. Los cambios incrementales de los discos locales se replican periódicamente en los discos de réplica de Azure.


Puede realizar un seguimiento del estado del trabajo en las notificaciones del portal.

![Seguimiento del trabajo](./media/tutorial-migrate-vmware-agent/jobs.png)

Para supervisar el estado de la replicación, haga clic en **Replicando servidores** en **Azure Migrate: Server Migration**.
![Supervisión de la replicación](./media/tutorial-migrate-vmware-agent/replicate-servers.png)

## <a name="run-a-test-migration"></a>Ejecutar una migración de prueba


Cuando comienza la replicación diferencial, puede ejecutar una migración de prueba para las máquinas virtuales antes de ejecutar una migración completa a Azure. Le recomendamos encarecidamente que lo haga al menos una vez en cada máquina, antes de migrarla.

- La ejecución de una migración de prueba comprueba que la migración funcionará según lo previsto, sin afectar a las máquinas locales, que seguirán estando operativas y continuarán realizando la replicación. 
- Para simular la migración, la migración de prueba crea una máquina virtual de Azure usando datos replicados (normalmente, con una migración a una red virtual que no es de producción en la suscripción a Azure).
- Puede usar la máquina virtual de Azure de prueba replicada para validar la migración, realizar pruebas de aplicaciones y resolver los problemas antes de la migración completa.

Realice una migración de prueba como se indica a continuación:


1. En **Objetivos de migración** > **Servidores** > **Azure Migrate: Server Migration**, haga clic en **Probar servidores migrados**.

     ![Probar servidores migrados](./media/tutorial-migrate-vmware-agent/test-migrated-servers.png)

2. Haga clic con el botón derecho en la máquina virtual que va a probar y haga clic en **Migración de prueba**.

    ![Migración de prueba](./media/tutorial-migrate-vmware-agent/test-migrate.png)

3. En **Migración de prueba**, seleccione la red virtual de Azure en la que se ubicará la máquina virtual de Azure después de la migración. Se recomienda usar una red virtual que no sea de producción.
4. Comienza el trabajo de **Migración de prueba**. Supervise el trabajo en las notificaciones del portal.
5. Una vez finalizada la migración, la máquina virtual de Azure migrada se puede ver en **Máquinas virtuales** en Azure Portal. El nombre de la máquina tiene el sufijo **-Test**.
6. Una vez finalizada la prueba, haga clic con el botón derecho en la máquina virtual de Azure, en **Replicación de máquinas**, y haga clic en **Limpiar la migración de prueba**.

    ![Limpiar la migración](./media/tutorial-migrate-vmware-agent/clean-up.png)


## <a name="migrate-vms"></a>Migración de máquinas virtuales

Después de comprobar que la migración de prueba funciona según lo previsto, puede migrar las máquinas locales.

1. En el proyecto de Azure Migrate > **Servidores** > **Azure Migrate: Server Migration**, haga clic en **Replicando servidores**.

    ![Replicando servidores](./media/tutorial-migrate-vmware-agent/replicate-servers.png)

2. En **Replicación de máquinas**, haga clic con el botón derecho en la máquina virtual > **Migrar**.
3. En **Migrar** >  **¿Quiere apagar las máquinas virtuales y realizar una migración planificada sin perder datos?** , seleccione **Sí** > **Aceptar**.
    - De forma predeterminada, Azure Migrate apaga la máquina virtual local para garantizar una pérdida de datos mínima. 
    - Si no desea apagar la máquina virtual, seleccione **No**
4. Se inicia un trabajo de migración de la máquina virtual. Realice un seguimiento del trabajo en las notificaciones de Azure.
5. Una vez finalizado el trabajo, la máquina virtual puede ver y administrar desde la página **Máquinas virtuales**.

## <a name="complete-the-migration"></a>Completar la migración

1. Una vez finalizada la migración, haga clic con el botón derecho en la máquina virtual > **Detener migración**. Esto hace lo siguiente:
    - Detiene la replicación en la máquina local.
    - Quita la máquina del recuento de **Servidores en replicación** en Azure Migrate: Server Migration.
    - Limpia la información de estado de replicación de la máquina virtual.
2. Instale el agente de [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) o [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) de la máquina virtual de Azure en las máquinas migradas.
3. Realice los ajustes de la aplicación posteriores a la migración, como actualizar las cadenas de conexión de la base de datos y las configuraciones del servidor web.
4. Realice las pruebas finales de la aplicación y la aceptación de la migración en la aplicación migrada que ahora se ejecuta en Azure.
5. Pase el tráfico a la instancia de máquina virtual de Azure migrada.
6. Quite las máquinas virtuales locales del inventario de máquinas virtuales local.
7. Quite las máquinas virtuales locales de las copias de seguridad locales.
8. Actualice la documentación interna para mostrar la nueva ubicación y la dirección IP las máquinas virtuales de Azure. 

## <a name="post-migration-best-practices"></a>Procedimientos recomendados después de la migración

- Local
    - Mueva el tráfico de la aplicación hasta la aplicación que se ejecuta en la instancia de máquina virtual de Azure migrada.
    - Quite las máquinas virtuales locales del inventario de máquinas virtuales local.
    - Quite las máquinas virtuales locales de las copias de seguridad locales.
    - Actualice la documentación interna para mostrar la nueva ubicación y la dirección IP las máquinas virtuales de Azure.
- Ajustar la configuración de las VM de Azure después de la migración:
    - El [agente de máquina virtual de Azure](../virtual-machines/extensions/agent-windows.md) administra la interacción de una máquina virtual con el controlador de tejido de Azure. Se requiere para algunos servicios de Azure, como Azure Backup, Site Recovery y Azure Security. Al migrar VM de VMware con la migración basada en agentes, el instalador de Mobility Service instala el agente de VM de Azure en máquinas Windows. En VM Linux, se recomienda instalar al agente después de la migración.
    - Desinstale manualmente Mobility Service de la VM de Azure después de la migración.
    - Desinstale manualmente las herramientas de VMware después de la migración.
- En Azure:
    - Realice los ajustes de la aplicación posteriores a la migración, como actualizar las cadenas de conexión de la base de datos y las configuraciones del servidor web.
    - Realice las pruebas finales de la aplicación y la aceptación de la migración en la aplicación migrada que ahora se ejecuta en Azure.
- Continuidad empresarial/recuperación ante desastres
    - Proteja los datos mediante la copia de seguridad de máquinas virtuales de Azure mediante el servicio Azure Backup. [Más información](../backup/quick-backup-vm-portal.md).
    - Mantenga las cargas de trabajo en ejecución y disponibles continuamente mediante la replicación de máquinas virtuales de Azure en una región secundaria con Site Recovery. [Más información](../site-recovery/azure-to-azure-tutorial-enable-replication.md).
- Para aumentar la seguridad:
    - Bloquee y limite el acceso de tráfico de entrada con la [administración Just-In-Time de Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).
    - Restrinja el tráfico de red a los puntos de conexión de administración con [grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/security-overview).
    - Implemente [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview) para ayudar a proteger discos y datos frente al robo y acceso no autorizado.
    - Obtenga más información sobre la [protección de recursos IaaS](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/) y visite [Azure Security Center](https://azure.microsoft.com/services/security-center/).
- Para supervisión y administración:
    - Considere la posibilidad de implementar [Azure Cost Management](https://docs.microsoft.com/azure/cost-management/overview) para supervisar el gasto y el uso de recursos.




 ## <a name="next-steps"></a>Pasos siguientes

Investigue el [proceso de la migración en la nube](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/migrate) en el marco de Cloud Adoption Framework para Azure.
