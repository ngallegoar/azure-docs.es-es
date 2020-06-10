---
title: Preparación de máquinas virtuales de Hyper-V para evaluación y migración con Azure Migrate
description: Aprenda a prepararse para la evaluación y migración de máquinas virtuales de Hyper-V con Azure Migrate.
ms.topic: tutorial
ms.date: 04/15/2020
ms.custom: mvc
ms.openlocfilehash: 22fd5bc87494eb2fc162828363e7ca70afe1bbf0
ms.sourcegitcommit: 58ff2addf1ffa32d529ee9661bbef8fbae3cddec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84322172"
---
# <a name="prepare-for-assessment-and-migration-of-hyper-v-vms-to-azure"></a>Preparación de la evaluación y migración de máquinas virtuales de Hyper-V a Azure

En este artículo se describe cómo preparar la evaluación de máquinas virtuales de Hyper-V locales con [Azure Migrate:Server Assessment](migrate-services-overview.md#azure-migrate-server-assessment-tool), y la migración de máquinas virtuales de Hyper-V con [Azure Migrate:Server Migration](migrate-services-overview.md#azure-migrate-server-migration-tool).


Este tutorial es el primero de una serie que muestra cómo evaluar máquinas virtuales de Hyper-V y migrarlas a Azure. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Prepare Azure. Configure los permisos de la cuenta y los recursos de Azure para poder usar Azure Migrate.
> * Prepare los hosts y las máquinas virtuales de Hyper-V locales para la evaluación de los servidores. Puede prepararse con un script de configuración o de forma manual.
> * Prepárese para la implementación del dispositivo de Azure Migrate. El dispositivo se usa para detectar y evaluar máquinas virtuales locales.
> * Prepare los hosts y las máquinas virtuales de Hyper-V locales para la migración de los servidores.


> [!NOTE]
> En los tutoriales se muestra la ruta de implementación más sencilla para un escenario, de modo que pueda configurar rápidamente una prueba de concepto. En ellos se usan las opciones predeterminadas siempre que es posible y no muestran todos los valores y rutas de acceso posibles. Para instrucciones detalladas, revise los procedimientos para la evaluación y la migración de Hyper-V.


Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.


## <a name="prepare-azure"></a>Preparación de Azure

### <a name="azure-permissions"></a>Permisos de Azure

Necesita configurar permisos para la implementación de Azure Migrate.

**Task** | **Detalles** 
--- | --- 
**Crear un proyecto de Azure Migrate** | La cuenta de Azure necesita permisos de colaborador o propietario para crear un proyecto. | 
**Registrar proveedores de recursos** | Azure Migrate usa un dispositivo ligero para detectar y evaluar máquinas virtuales de Hyper-V con Azure Migrate Server Assessment.<br/><br/> Durante el registro de la aplicación, los proveedores de recursos se registran con la suscripción elegida en la aplicación. [Más información](migrate-appliance-architecture.md#appliance-registration).<br/><br/> Para registrar los proveedores de recursos debe tener el rol colaborador o propietario de la suscripción.
**Creación de una aplicación de Azure AD** | Al registrar la aplicación, Azure Migrate crea una aplicación de Azure Active Directory (Azure AD) que se usa para la comunicación entre los agentes que se ejecutan en la aplicación con sus respectivos servicios que se ejecutan en Azure. [Más información](migrate-appliance-architecture.md#appliance-registration).<br/><br/> Para crear aplicaciones de Azure AD, se necesitan permisos (disponibles en el Desarrollador de aplicaciones).



### <a name="assign-permissions-to-create-project"></a>Asignación de permisos para crear un proyecto

Compruebe que tenga permiso para crear un proyecto de Azure Migrate.

1. En Azure Portal, abra la suscripción y seleccione **Control de acceso (IAM)** .
2. En **Comprobar acceso**, busque la cuenta correspondiente y haga clic en ella para ver los permisos.
3. Debe tener permisos de **Colaborador** o **Propietario**.
    - Si acaba de crear una cuenta de Azure gratuita, es el propietario de la suscripción.
    - Si no es el propietario, trabaje con él para asignar el rol.


### <a name="assign-permissions-to-register-the-appliance"></a>Asignación de permisos para registrar el dispositivo

Puede asignar permisos para Azure Migrate con el fin de crear las aplicaciones de Azure AD durante el registro del dispositivo mediante uno de los métodos siguientes:

- Un administrador de inquilinos o administrador global puede conceder permisos a los usuarios del inquilino para crear y registrar aplicaciones de Azure AD.
- Un administrador de inquilinos o administrador global puede asignar el rol de desarrollador de aplicaciones (que tiene los permisos) a la cuenta.

> [!NOTE]
> - Las aplicaciones no tienen otros permisos de acceso en la suscripción distintos de los descritos anteriormente.
> - Solo necesita estos permisos al registrar un nuevo dispositivo. Puede quitar los permisos una vez configurado el dispositivo.


#### <a name="grant-account-permissions"></a>Concesión de permisos de cuenta

El administrador de inquilinos o global puede conceder permisos como se indica:

1. En Azure AD, el administrador de inquilinos o global debe ir a **Azure Active Directory** > **Usuarios** > **Configuración de usuario**.
2. El administrador debe establecer **Registros de aplicaciones** en **Sí**.

    ![Permisos de Azure AD](./media/tutorial-prepare-hyper-v/aad.png)

> [!NOTE]
> Se trata de una configuración predeterminada que no es confidencial. [Más información](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).



#### <a name="assign-application-developer-role"></a>Asignación del rol de desarrollador de aplicaciones

El administrador de inquilinos o administrador global puede asignar el rol de desarrollador de aplicaciones a una cuenta. [Más información](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).


## <a name="prepare-hyper-v-for-assessment"></a>Preparación de Hyper-V para la evaluación

Puede preparar Hyper-V para la evaluación de máquinas virtuales manualmente o mediante un script de configuración. Estos son los pasos de preparación:
- [Compruebe](migrate-support-matrix-hyper-v.md#hyper-v-host-requirements) la configuración de los hosts de Hyper-V y asegúrese de que los [puertos necesarios](migrate-support-matrix-hyper-v.md#port-access) estén abiertos en ellos.
- Configure la comunicación remota de PowerShell en cada host de modo que el dispositivo de Azure Migrate pueda ejecutar comandos de PowerShell en el host mediante una conexión de WinRM.
- Delegue credenciales si los discos de máquina virtual se encuentran en recursos compartidos SMB remotos.
- Configure una cuenta que el dispositivo usará para detectar máquinas virtuales en hosts de Hyper-V.
- Configure Hyper-V Integration Services en las máquinas virtuales que desee detectar y evaluar. Los valores predeterminados que se utilizan al habilitar Integration Services son suficientes para Azure Migrate.

    ![Habilitación de Integration Services](./media/tutorial-prepare-hyper-v/integrated-services.png)


## <a name="prepare-with-a-script"></a>Preparación con un script

El script hace lo siguiente:

- Comprueba que se está ejecutando el script en una versión compatible de PowerShell.
- Comprueba que usted (el usuario que ejecuta el script) tiene privilegios administrativos en el host de Hyper-V.
- Permite crear una cuenta de usuario local (no administrador) que se usa para que el servicio Azure Migrate se comunique con el host de Hyper-V. Esta cuenta de usuario se agrega a estos grupos en el host:
    - Usuarios de Administración remota
    - Administradores de Hyper-V
    - Usuarios de Monitor de rendimiento
- Comprueba que el host está ejecutando una versión compatible de Hyper-V y el rol de Hyper-V.
- Habilita el servicio WinRM y abre los puertos 5985 (HTTP) y 5986 (HTTPS) en el host (necesario para la recopilación de metadatos).
- Habilita la comunicación remota con PowerShell en el host.
- Comprueba que los servicios de integración de Hyper-V está habilitado en todas las máquinas virtuales administradas por el host.
- Habilita CredSSP en el host si es necesario.

Ejecute el script como se indica a continuación:

1. Asegúrese de que tiene instalada la versión de PowerShell 4.0 o posterior en el host de Hyper-V.
2. Descargue el script del [Centro de descarga de Microsoft](https://aka.ms/migrate/script/hyperv). Microsoft firma criptográficamente el script.
3. Valide la integridad del script mediante archivos hash MD5 o SHA256. A continuación se muestran los valores de hashtag. Ejecute este comando para generar el código hash para el script:
    ```
    C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]
    ```
    Ejemplo de uso:
    ```
    C:\>CertUtil -HashFile C:\Users\Administrators\Desktop\ MicrosoftAzureMigrate-Hyper-V.ps1 SHA256
    ```

4. Después de validar la integridad del script, ejecútelo en cada host de Hyper-V con este comando de PowerShell:
    ```
    PS C:\Users\Administrators\Desktop> MicrosoftAzureMigrate-Hyper-V.ps1
    ```

### <a name="hashtag-values"></a>Valores de hashtag

Los valores de hash son:

| **Hash** | **Valor** |
| --- | --- |
| **MD5** | 0ef418f31915d01f896ac42a80dc414e |
| **SHA256** | 0ad60e7299925eff4d1ae9f1c7db485dc9316ef45b0964148a3c07c80761ade2 |


## <a name="prepare-manually"></a>Preparación manual

Siga los procedimientos de esta sección para preparar Hyper-V manualmente, en lugar de usar el script.

### <a name="verify-powershell-version"></a>Verificación de la versión de PowerShell

Asegúrese de que tiene instalada la versión de PowerShell 4.0 o posterior en el host de Hyper-V.



### <a name="set-up-an-account-for-vm-discovery"></a>Configuración de una cuenta para la detección de máquinas virtuales

Azure Migrate necesita permisos para detectar máquinas virtuales locales.

- Configure una cuenta de usuario local o de dominio con permisos de administrador en el clúster o los hosts de Hyper-V.

    - Necesita una sola cuenta para todos los hosts y clústeres que desee incluir en la detección.
    - La cuenta puede ser una cuenta local o de dominio. Se recomienda que tenga permisos de administrador en los hosts o clústeres de Hyper-V.
    - O bien, si no desea asignar permisos de administrador, se necesitan los siguientes permisos:
        - Usuarios de Administración remota
        - Administradores de Hyper-V
        - Usuarios de Monitor de rendimiento

### <a name="verify-hyper-v-host-settings"></a>Comprobación de la configuración del host de Hyper-V

1. Compruebe los [requisitos del host de Hyper-V](migrate-support-matrix-hyper-v.md#hyper-v-host-requirements) para la evaluación de los servidores.
2. Asegúrese de que los [puertos necesarios](migrate-support-matrix-hyper-v.md#port-access) están abiertos en los hosts de Hyper-V.

### <a name="enable-powershell-remoting-on-hosts"></a>Habilitación de la comunicación remota con PowerShell en los hosts

Configure la comunicación remota de PowerShell en cada host, como se indica a continuación:

1. En cada uno de los hosts, abra una consola de PowerShell como administrador.
2. Ejecute este comando:

    ```
    Enable-PSRemoting -force
    ```
### <a name="enable-integration-services-on-vms"></a>Habilitación de Integration Services en máquinas virtuales

Integration Services debe estar habilitado en todas las máquinas virtuales para que Azure Migrate pueda capturar la información del sistema operativo de la máquina virtual.

En las máquinas virtuales que quiera detectar y evaluar, habilite [Hyper-V Integration Services](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) en cada máquina virtual.


### <a name="enable-credssp-on-hosts"></a>Habilitación de CredSSP en los hosts

Si el host tiene discos de las máquinas virtuales en recursos compartidos SMB, complete este paso en cada host.

- Puede ejecutar este comando de forma remota en todos los hosts de Hyper-V.
- Si agrega nuevos nodos de host en un clúster, se agregan automáticamente para la detección, pero debe habilitar CredSSP manualmente en los nuevos nodos si es necesario.

Habilite de la siguiente manera:

1. Identifique los hosts de Hyper-V que ejecutan máquinas virtuales de Hyper-V con discos en recursos compartidos SMB.
2. Ejecute el siguiente comando en cada host de Hyper-V identificado:

    ```
    Enable-WSManCredSSP -Role Server -Force
    ```

Al configurar el dispositivo, termine de configurar CredSSP mediante su [habilitación en el dispositivo](tutorial-assess-hyper-v.md#delegate-credentials-for-smb-vhds). Esto se describe en el siguiente tutorial de la serie.


## <a name="prepare-for-appliance-deployment"></a>Preparación para la implementación del dispositivo

Antes de configurar el dispositivo de Azure Migrate y comenzar la evaluación en el siguiente tutorial, prepare la implementación del dispositivo.

1. [Compruebe](migrate-appliance.md#appliance---hyper-v) los requisitos del dispositivo.
2. Revise las direcciones URL de Azure a las que el dispositivo necesitará acceder en las nubes [públicas](migrate-appliance.md#public-cloud-urls) y [gubernamentales](migrate-appliance.md#government-cloud-urls). Si usa un proxy de firewall basado en direcciones URL, asegúrese de que permiten el acceso a las URL necesarias.
3. Revise los datos que el dispositivo va a recopilar durante la detección y la evaluación.
4. [Revise](migrate-appliance.md#collected-data---hyper-v) los requisitos de acceso a los puertos del dispositivo.




## <a name="prepare-for-hyper-v-migration"></a>Preparación de la migración de Hyper-V

1. [Revise](migrate-support-matrix-hyper-v-migration.md#hyper-v-hosts) los requisitos de los hosts de Hyper-V para la migración y las direcciones URL de Azure a las que los hosts y clústeres de Hyper-V necesitan acceder para la migración de las máquinas virtuales.
2. [Consulte](migrate-support-matrix-hyper-v-migration.md#hyper-v-vms) los requisitos de las máquinas virtuales de Hyper-V que desea migrar a Azure.


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, hizo lo siguiente:

> [!div class="checklist"]
> * Configurar los permisos de la cuenta de Azure.
> * Preparar las máquinas virtuales y los hosts de Hyper-V para la evaluación y migración.
> * Se preparó para la implementación del dispositivo de Azure Migrate.

Continúe con el siguiente tutorial para crear un proyecto de Azure Migrate, implementar el dispositivo y detectar y evaluar las máquinas virtuales de Hyper-V para la migración a Azure.

> [!div class="nextstepaction"]
> [Evaluación de máquinas virtuales de Hyper-V](./tutorial-assess-hyper-v.md)
