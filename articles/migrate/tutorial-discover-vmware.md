---
title: Detección de las máquinas virtuales de VMware con Azure Migrate Server Assessment
description: Obtenga información acerca de cómo detectar las máquinas virtuales locales de VMware con la herramienta Azure Migrate Server Assessment
ms.topic: tutorial
ms.date: 09/14/2020
ms.custom: mvc
ms.openlocfilehash: 8a09f4583bd5cdae977b927be9649897a2d24ee6
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94832660"
---
# <a name="tutorial-discover-vmware-vms-with-server-assessment"></a>Tutorial: Detección de máquinas virtuales de VMware con Server Assessment

Como parte del recorrido de la migración a Azure, puede detectar el inventario y las cargas de trabajo locales. 

En este tutorial se muestra cómo detectar las máquinas virtuales locales de VMware con la herramienta Azure Migrate: Server Assessment, con un dispositivo ligero de Azure Migrate. El dispositivo se implementa como una máquina virtual de VMware para detectar continuamente los metadatos de las máquinas virtuales y del rendimiento, las aplicaciones que se ejecutan en máquinas virtuales y las dependencias de las máquinas virtuales.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configurar una cuenta de Azure.
> * Preparar el entorno de VMware para la detección.
> * Cree un proyecto de Azure Migrate.
> * Configurar el dispositivo de Azure Migrate.
> * Iniciar la detección continua.

> [!NOTE]
> Los tutoriales muestran la manera más rápida de probar un escenario y utilizar las opciones predeterminadas siempre que sea posible.  

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.


## <a name="prerequisites"></a>Requisitos previos

Antes de empezar este tutorial, compruebe que dispone de estos requisitos previos.


**Requisito** | **Detalles**
--- | ---
**Host vCenter Server/ESXi** | Necesita una instancia de vCenter Server que ejecute las versiones 5.5, 6.0, 6.5 o 6.7.<br/><br/> Las máquinas virtuales deben estar hospedadas en un host ESXi que ejecute la versión 5.5 o posterior.<br/><br/> En vCenter Server, permita las conexiones entrantes en el puerto TCP 443 para que el dispositivo pueda recopilar los datos de evaluación.<br/><br/> De forma predeterminada, el dispositivo se conecta a vCenter en el puerto 443. Si el servidor vCenter escucha en un puerto diferente, podrá modificarlo cuando se conecte desde el dispositivo al servidor para iniciar la detección.<br/><br/> En el servidor de EXSi que hospeda las máquinas virtuales, asegúrese de que se permita el acceso de entrada en el puerto TCP 443 para la detección de aplicaciones.
**Dispositivo** | vCenter Server necesita recursos para asignar una máquina virtual al dispositivo de Azure Migrate:<br/><br/> - Windows Server 2016<br/><br/> - 32 GB de RAM, ocho vCPU y alrededor de 80 GB de almacenamiento en disco.<br/><br/> - Un conmutador virtual externo y acceso a Internet en la máquina virtual, directamente o a través de un proxy.
**Máquinas virtuales** | Para usar este tutorial, las máquinas virtuales de Windows deben ejecutar Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2.<br/><br/> Las máquinas virtuales Linux deben ejecutar Red Hat Enterprise Linux 7/6/5, Ubuntu Linux 14.04/16.04, Debian 7/8, Oracle Linux 6/7 o CentOS 5/6/7.<br/><br/> Las máquinas virtuales necesitan que las herramientas de VMware (una versión posterior a la 10.2.0) estén instaladas y en ejecución.<br/><br/> En las máquinas virtuales Windows, debe estar instalado Windows PowerShell 2.0 o posterior.


## <a name="prepare-an-azure-user-account"></a>Preparación de una cuenta de usuario de Azure

Para crear un proyecto de Azure Migrate y registrar el dispositivo Azure Migrate, necesita una cuenta con:
- Permisos de nivel de colaborador o propietario en una suscripción de Azure.
- Permisos para registrar aplicaciones de Azure Active Directory.

Si acaba de crear una cuenta de Azure gratuita, es el propietario de la suscripción. Si no es el propietario, trabaje con él para asignar los permisos, como se indica a continuación:

1. En Azure Portal, busque "suscripciones" y, en **Servicios**, seleccione **Suscripciones**.

    ![Cuadro para buscar la suscripción de Azure](./media/tutorial-discover-vmware/search-subscription.png)

2. En la página **Suscripciones**, seleccione aquella en la que desee crear un proyecto de Azure Migrate. 
3. En la suscripción, seleccione **Access control (IAM)**  > **Comprobar acceso**.
4. En **Comprobar acceso**, busque la cuenta de usuario correspondiente.
5. En **Agregar una asignación de roles**, haga clic en **Agregar**.

    ![Búsqueda de una cuenta de usuario para comprobar el acceso y asignar un rol](./media/tutorial-discover-vmware/azure-account-access.png)

6. En **Agregar asignación de roles**, seleccione el rol Colaborador o Propietario, y seleccione la cuenta (azmigrateuser en nuestro ejemplo). A continuación, haga clic en **Save**(Guardar).

    ![Se abre la página Agregar asignación de roles para asignar un rol a la cuenta.](./media/tutorial-discover-vmware/assign-role.png)

7. En el portal, busque los usuarios y, en **Servicios**, seleccione **Usuarios**.
8. En **Configuración de usuario**, compruebe que los usuarios de Azure AD puedan registrar aplicaciones (establecido en **Sí** de forma predeterminada).

    ![Comprobación en la configuración de usuario de que los usuarios puedan registrar aplicaciones de Active Directory](./media/tutorial-discover-vmware/register-apps.png)

9. Como alternativa, el administrador de inquilinos o administrador global puede asignar el rol de **desarrollador de aplicaciones** a una cuenta para permitir el registro de aplicaciones de AAD. [Más información](../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md).

## <a name="prepare-vmware"></a>Preparación de VMware

En vCenter Server, cree una cuenta que el dispositivo pueda usar para acceder al servidor de vCenter Server y compruebe que los puertos necesarios estén abiertos. También necesita una cuenta que el dispositivo pueda usar para acceder a las máquinas virtuales. 

### <a name="create-an-account-to-access-vcenter"></a>Creación de una cuenta para acceder a vCenter

En el cliente web de vSphere, configure una cuenta de la siguiente manera:

1. Con una cuenta con privilegios de administración, en el cliente web de vSphere > seleccione **Administration** (Administración).
2. En **Access** (Acceso), seleccione **SSO Users and Groups** (Usuarios y grupos de SSO).
3. En **Users** (Usuarios), agregue un nuevo usuario.
4. En **New User** (Nuevo usuario), escriba los detalles de la cuenta. A continuación, haga clic en **Aceptar**.
5. En **Global Permissions** (Permisos globales), seleccione la cuenta de usuario y asígnele el rol **Read-only** (Solo lectura). A continuación, haga clic en **Aceptar**.
6. En **Roles** > seleccione el rol **Read-only** (Solo lectura) y, en **Privileges** (Privilegios), seleccione **Guest Operations** (operaciones de invitado). Estos privilegios son necesarios para detectar las aplicaciones que se ejecutan en las máquinas virtuales y analizar las dependencias de la máquina virtual.
 
    ![Casilla para permitir las operaciones de invitado en el rol de solo lectura](./media/tutorial-discover-vmware/guest-operations.png)


### <a name="create-an-account-to-access-vms"></a>Creación de una cuenta para acceder a las máquinas virtuales

El dispositivo accede a las máquinas virtuales para detectar las aplicaciones y analizar las dependencias de la máquina virtual. El dispositivo no instala ningún agente en las máquinas virtuales.

1. Cree una cuenta de administrador local que el dispositivo pueda usar para detectar las aplicaciones y las dependencias en las máquinas virtuales Windows.
2. En el caso de las máquinas Linux, cree una cuenta de usuario con privilegios raíz o, como alternativa, una cuenta de usuario con estos permisos en los archivos /bin/netstat y /bin/ls: CAP_DAC_READ_SEARCH y CAP_SYS_PTRACE.

> [!NOTE]
> Azure Migrate admite una credencial para la detección de aplicaciones en todos los servidores de Windows y una credencial para la detección de aplicaciones en todas las máquinas Linux.


## <a name="set-up-a-project"></a>Configuración de un proyecto

Configure un proyecto nuevo de Azure Migrate.

1. En Azure Portal > **Todos los servicios**, busque **Azure Migrate**.
2. En **Servicios**, seleccione **Azure Migrate**.
3. En **Información general**, seleccione **Crear proyecto**.
5. En **Crear proyecto**, seleccione su suscripción y grupo de recursos de Azure. Cree un grupo de recursos si no tiene ninguno.
6. En **Detalles del proyecto**, especifique el nombre del proyecto y la región geográfica en la que quiere crearlo. Revise las zonas geográficas admitidas para nubes [públicas](migrate-support-matrix.md#supported-geographies-public-cloud) y [nubes gubernamentales](migrate-support-matrix.md#supported-geographies-azure-government).

   ![Cuadros de nombre de proyecto y región](./media/tutorial-discover-vmware/new-project.png)

7. Seleccione **Crear**.
8. Espere unos minutos para que se implemente el proyecto de Azure Migrate.

La herramienta **Azure Migrate: Server Assessment** se agrega de forma predeterminada al nuevo proyecto.

![Página que muestra la herramienta Server Assessment agregada de forma predeterminada](./media/tutorial-discover-vmware/added-tool.png)


## <a name="set-up-the-appliance"></a>Configuración del dispositivo

Para configurar el dispositivo mediante una plantilla de OVA:
- Proporcione un nombre de dispositivo y genere una clave del proyecto de Azure Migrate en el portal.
- Descargue una plantilla OVA e impórtela en vCenter Server.
- Crear el dispositivo y comprobar que se puede conectar a Azure Migrate Server Assessment.
- Configure el dispositivo por primera vez y regístrelo en el proyecto de Azure Migrate mediante la clave del proyecto de Azure Migrate.

> [!NOTE]
> Si, por alguna razón, no puede configurar el dispositivo mediante la plantilla, puede usar un script de PowerShell. [Más información](deploy-appliance-script.md#set-up-the-appliance-for-vmware).


### <a name="deploy-with-ova"></a>Implementación con OVA

Para configurar el dispositivo mediante una plantilla de OVA:
- Proporcione un nombre de dispositivo y genere una clave del proyecto de Azure Migrate en el portal.
- Descargue una plantilla OVA e impórtela en vCenter Server.
- Crear el dispositivo y comprobar que se puede conectar a Azure Migrate Server Assessment.
- Configure el dispositivo por primera vez y regístrelo en el proyecto de Azure Migrate mediante la clave del proyecto de Azure Migrate.

### <a name="generate-the-azure-migrate-project-key"></a>Generación de la clave del proyecto de Azure Migrate

1. En **Objetivos de migración** > **Servidores** > **Azure Migrate: Server Assessment**, seleccione **Detectar**.
2. En **Detectar máquinas** >  **¿Las máquinas están virtualizadas?** , haga clic en **Sí, con VMware vSphere Hypervisor**.
3. En **1: Generar la clave del proyecto de Azure Migrate**, proporcione un nombre para el dispositivo de Azure Migrate que configurará para la detección de máquinas virtuales de VMware. El nombre debe ser alfanumérico con 14 caracteres como máximo.
1. Haga clic en **Generar clave** para iniciar la creación de los recursos de Azure necesarios. No cierre la página Detectar máquinas durante la creación de recursos.
1. Después de la creación correcta de los recursos de Azure, se genera una **clave del proyecto Azure Migrate**.
1. Copie la clave, ya que la necesitará para completar el registro del dispositivo durante su configuración.

### <a name="download-the-ova-template"></a>Descarga de la plantilla OVA

En **2: Descargar dispositivo de Azure Migrate**, seleccione el archivo .OVA y haga clic en **Descargar**. 


### <a name="verify-security"></a>Comprobación de la seguridad

Compruebe que el archivo OVA es seguro antes de implementarlo:

1. En la máquina en la que descargó el archivo, abra una ventana de comandos de administrador.
2. Ejecute el siguiente comando para generar el código hash del archivo OVA:
  
   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
   
   Ejemplo de uso: ```C:\>CertUtil -HashFile C:\Users\Administrator\Desktop\MicrosoftAzureMigration.ova SHA256```

3. Compruebe las versiones más recientes del dispositivo y los valores hash:

    - Para la nube pública de Azure:
    
        **Algoritmo** | **Descargar** | **SHA256**
        --- | --- | ---
        VMware (11,9 GB) | [La versión más reciente](https://go.microsoft.com/fwlink/?linkid=2140333) | e9c9a1fe4f3ebae81008328e8f3a7933d78ff835ecd871d1b17f367621ce3c74

    - Para Azure Government:
    
        **Algoritmo** | **Descargar** | **SHA256**
        --- | --- | ---
        VMware (85,8 MB) | [La versión más reciente](https://go.microsoft.com/fwlink/?linkid=2140337) | 2daaa2a59302bf911e8ef195f8add7d7c8352de77a9af0b860e2a627979085ca




### <a name="create-the-appliance-vm"></a>Creación de la máquina virtual del dispositivo

Importe el archivo descargado y cree una máquina virtual.

1. En la consola de cliente de vSphere, haga clic en **File** (Archivo) > **Deploy OVF Template** (Implementar plantilla de OVF).
2. En el Deploy OVF Template Wizard (Asistente para implementar la plantilla de OVF) > **Source** (Origen), especifique la ubicación del archivo OVA.
3. En **Name** (Nombre) y **Location** (Ubicación), especifique un nombre descriptivo para la máquina virtual. Seleccione el objeto de inventario en el que se hospedará la máquina virtual.
5. En **Host/Cluster** (Host o clúster), especifique el host o clúster en que se ejecutará la máquina virtual.
6. En **Storage**, especifique el destino de almacenamiento de la máquina virtual.
7. En **Disk Format** (Formato de disco), especifique el tamaño y el tipo de disco.
8. En **Network Mapping** (Asignación de red), especifique la red a la que se conectará la máquina virtual. La red necesita conectividad a Internet para enviar metadatos a Azure Migrate Server Assessment.
9. Revise y confirme la configuración y haga clic en **Finish** (Finalizar).


### <a name="verify-appliance-access-to-azure"></a>Comprobación de que el dispositivo puede acceder a Azure

Asegúrese de que la VM del dispositivo pueda conectarse a las direcciones URL de Azure para las nubes [públicas](migrate-appliance.md#public-cloud-urls) y [gubernamentales](migrate-appliance.md#government-cloud-urls).


### <a name="configure-the-appliance"></a>Configuración del dispositivo

Configure el dispositivo por primera vez.

> [!NOTE]
> Si configura la aplicación mediante un [script de PowerShell](deploy-appliance-script.md) en lugar de la OVA descargada, los dos primeros pasos de este procedimiento no son pertinentes.

1. En la consola de cliente de vSphere, haga clic con el botón derecho en la máquina virtual y, luego, seleccione **Open Console** (Abrir consola).
2. Especifique el idioma, la zona horaria y la contraseña del dispositivo.
3. Abra un explorador en cualquier equipo que pueda conectarse a la máquina virtual y abra la dirección URL de la aplicación web del dispositivo: **https://*nombre o dirección IP del dispositivo*: 44368**.

   Como alternativa, puede abrir la aplicación desde el escritorio del dispositivo, para lo que debe seleccionar el acceso directo de la aplicación.
1. Acepte los **términos de licencia** y lea la información de terceros.
1. En la aplicación web > **Set up prerequisites** (Configurar los requisitos previos ), realice las siguientes operaciones:
   - **Connectivity** (Conectividad): la aplicación comprueba que la máquina virtual tiene acceso a Internet. Si la máquina virtual usa un proxy:
     - Haga clic en **Configurar el proxy** y especifique la dirección del proxy (con los formatos http://ProxyIPAddress o http://ProxyFQDN) ) y el puerto de escucha.
     - Especifique las credenciales si el proxy requiere autenticación.
     - Solo se admite un proxy HTTP.
     - Si ha agregado detalles del proxy o ha deshabilitado el proxy o la autenticación, haga clic en **Guardar** para desencadenar la comprobación de conectividad.
   - **Time sync** (Sincronización de hora): Para que la detección funcione correctamente, la hora del dispositivo debe estar sincronizada con la hora de Internet.
   - **Instalación de actualizaciones**: el dispositivo garantiza que se instalan las actualizaciones más recientes. Una vez finalizada la comprobación, puede hacer clic en **Ver servicios del dispositivo** para ver el estado y las versiones de los componentes que se ejecutan en el dispositivo.
   - **Install VDDK** (Instalar VDDK): El dispositivo comprueba si VMware vSphere Virtual Disk Development Kit (VDDK) está instalado. Si no está instalado, descargue VDDK 6.7 de VMware y extraiga el contenido del archivo ZIP descargado en la ubicación especificada del dispositivo, tal como se indica en las **instrucciones de instalación**.

     Server Migration de Azure Migrate usa VDDK para replicar las máquinas durante la migración a Azure. 
1. Si quiere, puede **volver a ejecutar los requisitos previos** en cualquier momento durante la configuración del dispositivo para comprobar si el dispositivo cumple todos los requisitos previos.

### <a name="register-the-appliance-with-azure-migrate"></a>Registro del dispositivo en Azure Migrate

1. Pegue la **clave del proyecto de Azure Migrate** copiada desde el portal. Si no tiene la clave, vaya a **Server Assessment > Detectar > Administrar los dispositivos existentes**, seleccione el nombre del dispositivo que proporcionó en el momento de la generación de la clave y copie la clave correspondiente.
1. Haga clic en **Iniciar sesión**. Se abrirá un mensaje de inicio de sesión de Azure en una nueva pestaña del explorador. Si no aparece, asegúrese de que ha deshabilitado el bloqueador de elementos emergentes en el explorador.
1. En la nueva pestaña, inicie sesión con su nombre de usuario y contraseña de Azure.
   
   No se admite el inicio de sesión con un PIN.
3. Después de iniciar sesión correctamente, vuelva a la aplicación web. 
4. Si la cuenta de usuario de Azure que se usa para el registro tiene los permisos adecuados en los recursos de Azure creados durante la generación de la clave, se iniciará el registro del dispositivo.
1. Una vez que el dispositivo se ha registrado correctamente, puede ver los detalles de registro haciendo clic en **Ver detalles**.



## <a name="start-continuous-discovery"></a>Inicio de detección continua

El dispositivo necesita conectarse a vCenter Server para detectar los datos de configuración y rendimiento de las máquinas virtuales.

1. En **Paso 1: Proporcionar las credenciales de vCenter Server**, haga clic en **Agregar credenciales** para especificar un nombre descriptivo para las credenciales, agregue un **nombre de usuario** y una **contraseña** para la cuenta de vCenter Server que utilizará el dispositivo para detectar máquinas virtuales en la instancia de vCenter Server.
    - Debe haber configurado una cuenta con los permisos necesarios en el tutorial anterior.
    - Si desea limitar el ámbito de la detección a objetos específicos de VMware (centros de datos de vCenter Server, clústeres, una carpeta de clústeres, hosts, una carpeta de hosts o máquinas virtuales individuales), consulte las instrucciones de [este artículo](set-discovery-scope.md) para restringir la cuenta usada por Azure Migrate.
1. En el **Paso 2: Proporcionar los detalles de vCenter Server**, haga clic en **Agregar origen de detección** para seleccionar el nombre descriptivo de las credenciales en la lista desplegable, especifique la **dirección IP o el FQDN** de la instancia de vCenter Server. Puede dejar el **puerto** en el valor predeterminado (443) o especificar un puerto personalizado en el que vCenter Server escuche y hacer clic en **Guardar**.
1. Al hacer clic en Guardar, el dispositivo intentará validar la conexión a vCenter Server con las credenciales proporcionadas y mostrar el **estado de validación** en la tabla con la dirección IP o el FQDN de vCenter Server.
1. Puede **volver a validar** la conectividad a vCenter Server en cualquier momento antes de iniciar la detección.
1. En **Paso 3: Proporcionar las credenciales de la máquina virtual para detectar las aplicaciones instaladas y para realizar la asignación de dependencias sin agente**, haga clic en **Agregar credenciales** y especifique el sistema operativo para el que se proporcionan las credenciales, el nombre descriptivo de las credenciales y el **nombre de usuario** y la **contraseña**. A continuación, haga clic en **Guardar**.

    - Si lo desea, agregue las credenciales aquí si ha creado una cuenta para utilizarla en la [característica de detección de aplicaciones](how-to-discover-applications.md) o [en la característica de análisis de dependencias sin agente](how-to-create-group-machine-dependencies-agentless.md).
    - Si no quiere usar estas características, puede hacer clic en el control deslizante para omitir el paso. Puede invertir la intención en cualquier momento más tarde.
    - Consulte cuáles son las credenciales necesarias para la [detección de aplicaciones](migrate-support-matrix-vmware.md#application-discovery-requirements) o para el [análisis de dependencias sin agente](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless).

5. Haga clic en **Iniciar detección** para iniciar la detección de máquinas virtuales. Una vez que la detección se ha iniciado correctamente, puede comprobar el estado de detección de la dirección IP o el FQDN de vCenter en la tabla.

La detección funciona como se indica a continuación:
- Los metadatos de las máquinas virtuales detectadas tardan unos 15 minutos en aparecer en el portal.
- La detección de las aplicaciones, los roles y las características instaladas tarda un rato. Todo depende del número de máquinas virtuales que se detectan. En el caso de 500 máquinas virtuales, el inventario de la aplicación tarda aproximadamente una hora en aparecer en el portal de Azure Migrate.


## <a name="next-steps"></a>Pasos siguientes

- [Evalúe las máquinas virtuales de VMware](./tutorial-assess-vmware-azure-vm.md) para la migración a máquinas virtuales de Azure.
- [Revise los datos](migrate-appliance.md#collected-data---vmware) que el dispositivo recopila durante la detección.