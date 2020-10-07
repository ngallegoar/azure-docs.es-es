---
title: Detección de las máquinas virtuales de Hyper-V con Azure Migrate Server Assessment
description: Obtenga información acerca de cómo detectar las máquinas virtuales locales de Hyper-V con la herramienta Azure Migrate Server Assessment.
ms.topic: tutorial
ms.date: 09/14/2020
ms.custom: mvc
ms.openlocfilehash: e62effc31ab5dbc687e0509617b89561c5f2a3b6
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91442318"
---
# <a name="tutorial-discover-hyper-v-vms-with-server-assessment"></a>Tutorial: Detección de máquinas virtuales de Hyper-V con Server Assessment

Como parte del recorrido de la migración a Azure, puede detectar el inventario y las cargas de trabajo locales. 

En este tutorial se muestra cómo detectar las máquinas virtuales (VM) locales de Hyper-V con la herramienta Azure Migrate: Server Assessment, con un dispositivo ligero de Azure Migrate. El dispositivo se implementa como una máquina virtual de Hyper-V para detectar continuamente los metadatos de rendimiento y la máquina.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configuración de una cuenta de Azure
> * Preparar el entorno de Hyper-V para la detección.
> * Cree un proyecto de Azure Migrate.
> * Configurar el dispositivo de Azure Migrate.
> * Iniciar la detección continua.

> [!NOTE]
> Los tutoriales muestran la manera más rápida de probar un escenario y utilizar las opciones predeterminadas.  

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar este tutorial, compruebe que dispone de estos requisitos previos.


**Requisito** | **Detalles**
--- | ---
**Host de Hyper-V** | Los hosts de Hyper-V en los que se encuentran las máquinas virtuales pueden ser independientes o formar parte de un clúster.<br/><br/> El host debe ejecutar Windows Server 2019, Windows Server 2016 o Windows Server 2012 R2.<br/><br/> Compruebe que las conexiones entrantes están permitidas en el puerto WinRM 5985 (HTTP) para que el dispositivo pueda conectarse para extraer datos de rendimiento y metadatos de máquinas virtuales mediante una sesión del Modelo de información común (CIM).
**Implementación del dispositivo** | El host de Hyper-V necesita recursos para asignar una máquina virtual al dispositivo:<br/><br/> - Windows Server 2016<br/><br/> \- 16 GB de RAM<br/><br/> - Ocho vCPU<br/><br/> - Alrededor de 80 GB de almacenamiento en disco<br/><br/> - Un conmutador virtual externo<br/><br/> - Acceso a Internet en la máquina virtual, directamente o a través de un proxy
**Máquinas virtuales** | Las máquinas virtuales pueden ejecutar cualquier sistema operativo Windows o Linux. 

Antes de empezar, puede [revisar los datos](migrate-appliance.md#collected-data---hyper-v) que recopila el dispositivo durante la detección.

## <a name="prepare-an-azure-user-account"></a>Preparación de una cuenta de usuario de Azure

Para crear un proyecto de Azure Migrate y registrar el dispositivo Azure Migrate, necesita una cuenta con:
- Permisos de nivel de colaborador o propietario en una suscripción de Azure.
- Permisos para registrar aplicaciones de Azure Active Directory.

Si acaba de crear una cuenta de Azure gratuita, es el propietario de la suscripción. Si no es el propietario, trabaje con él para asignar los permisos, como se indica a continuación:


1. En Azure Portal, busque "suscripciones" y, en **Servicios**, seleccione **Suscripciones**.

    ![Cuadro para buscar la suscripción de Azure](./media/tutorial-discover-hyper-v/search-subscription.png)

2. En la página **Suscripciones**, seleccione aquella en la que desee crear un proyecto de Azure Migrate. 
3. En la suscripción, seleccione **Access control (IAM)**  > **Comprobar acceso**.
4. En **Comprobar acceso**, busque la cuenta de usuario correspondiente.
5. En **Agregar una asignación de roles**, haga clic en **Agregar**.

    ![Búsqueda de una cuenta de usuario para comprobar el acceso y asignar un rol](./media/tutorial-discover-hyper-v/azure-account-access.png)

6. En **Agregar asignación de roles**, seleccione el rol Colaborador o Propietario, y seleccione la cuenta (azmigrateuser en nuestro ejemplo). A continuación, haga clic en **Save**(Guardar).

    ![Se abre la página Agregar asignación de roles para asignar un rol a la cuenta.](./media/tutorial-discover-hyper-v/assign-role.png)

7. En el portal, busque los usuarios y, en **Servicios**, seleccione **Usuarios**.
8. En **Configuración de usuario**, compruebe que los usuarios de Azure AD puedan registrar aplicaciones (establecido en **Sí** de forma predeterminada).

    ![Comprobación en la configuración de usuario de que los usuarios puedan registrar aplicaciones de Active Directory](./media/tutorial-discover-hyper-v/register-apps.png)

9. Como alternativa, el administrador de inquilinos o administrador global puede asignar el rol de **desarrollador de aplicaciones** a una cuenta para permitir el registro de aplicaciones de AAD. [Más información](../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md).

## <a name="prepare-hyper-v-hosts"></a>Preparar los hosts de Hyper-V

Configure una cuenta con acceso de administrador en los hosts de Hyper-V. El dispositivo usa esta cuenta para la detección.

- Opción 1: Preparar una cuenta con acceso de administrador a la máquina host de Hyper-V.
- Opción 2: Preparar una cuenta de administrador local o una cuenta de administrador de dominio y agregarla a estos grupos: Usuarios de administración remota, Administradores de Hyper-V y Usuarios del Monitor de rendimiento.


## <a name="set-up-a-project"></a>Configuración de un proyecto

Configure un proyecto nuevo de Azure Migrate.

1. En Azure Portal > **Todos los servicios**, busque **Azure Migrate**.
2. En **Servicios**, seleccione **Azure Migrate**.
3. En **Información general**, seleccione **Crear proyecto**.
5. En **Crear proyecto**, seleccione su suscripción y grupo de recursos de Azure. Cree un grupo de recursos si no tiene ninguno.
6. En **Detalles del proyecto**, especifique el nombre del proyecto y la región geográfica en la que quiere crearlo. Revise las zonas geográficas admitidas para nubes [públicas](migrate-support-matrix.md#supported-geographies-public-cloud) y [nubes gubernamentales](migrate-support-matrix.md#supported-geographies-azure-government).

   ![Cuadros de nombre de proyecto y región](./media/tutorial-discover-hyper-v/new-project.png)

7. Seleccione **Crear**.
8. Espere unos minutos para que se implemente el proyecto de Azure Migrate.

La herramienta **Azure Migrate: Server Assessment** se agrega de forma predeterminada al nuevo proyecto.

![Página que muestra la herramienta Server Assessment agregada de forma predeterminada](./media/tutorial-discover-hyper-v/added-tool.png)


## <a name="set-up-the-appliance"></a>Configuración del dispositivo

En este tutorial se configura el dispositivo en una máquina virtual de Hyper-V, como se indica a continuación:

- Proporcione un nombre de dispositivo y genere una clave del proyecto de Azure Migrate en el portal.
- Descargue un disco duro virtual de Hyper-V comprimido desde Azure Portal.
- Crear el dispositivo y comprobar que se puede conectar a Azure Migrate Server Assessment.
- Configure el dispositivo por primera vez y regístrelo en el proyecto de Azure Migrate mediante la clave del proyecto de Azure Migrate.
> [!NOTE]
> Si, por alguna razón, no puede configurar el dispositivo mediante una plantilla, puede usar un script de PowerShell. [Más información](deploy-appliance-script.md#set-up-the-appliance-for-hyper-v).


### <a name="generate-the-azure-migrate-project-key"></a>Generación de la clave del proyecto de Azure Migrate

1. En **Objetivos de migración** > **Servidores** > **Azure Migrate: Server Assessment**, seleccione **Detectar**.
2. En **Detectar máquinas** >  **¿Las máquinas están virtualizadas?** , seleccione **Sí, con Hyper-V**.
3. En **1: Generar la clave del proyecto de Azure Migrate**, proporcione un nombre para el dispositivo de Azure Migrate que configurará para la detección de máquinas virtuales de Hyper-V. El nombre debe ser alfanumérico con 14 caracteres como máximo.
1. Haga clic en **Generar clave** para iniciar la creación de los recursos de Azure necesarios. No cierre la página Detectar máquinas durante la creación de recursos.
1. Después de la creación correcta de los recursos de Azure, se genera una **clave del proyecto Azure Migrate**.
1. Copie la clave, ya que la necesitará para completar el registro del dispositivo durante su configuración.

### <a name="download-the-vhd"></a>Descarga del disco duro virtual

En **2: Descargar dispositivo de Azure Migrate**, seleccione el archivo .VHD y haga clic en **Descargar**. 


### <a name="verify-security"></a>Comprobación de la seguridad

Compruebe que el archivo comprimido es seguro, antes de implementarlo.

1. En la máquina en la que descargó el archivo, abra una ventana de comandos de administrador.

2. Ejecute el siguiente comando de PowerShell para generar el código hash para el archivo ZIP
    - ```C:\>Get-FileHash -Path <file_location> -Algorithm [Hashing Algorithm]```
    - Ejemplo de uso: ```C:\>Get-FileHash -Path ./AzureMigrateAppliance_v3.20.09.25.zip -Algorithm SHA256```

3.  Compruebe las versiones más recientes del dispositivo y los valores hash:

    - Para la nube pública de Azure:

        **Escenario** | **Descargar** | **SHA256**
        --- | --- | ---
        Hyper-V (8,91 GB) | [La versión más reciente](https://go.microsoft.com/fwlink/?linkid=2140422) |  40aa037987771794428b1c6ebee2614b092e6d69ac56d48a2bbc75eeef86c99a

    - Para Azure Government:

        **Escenario*** | **Descargar** | **SHA256**
        --- | --- | ---
        Hyper-V (85,8 MB) | [La versión más reciente](https://go.microsoft.com/fwlink/?linkid=2140424) |  cfed44bb52c9ab3024a628dc7a5d0df8c624f156ec1ecc3507116bae330b257f

### <a name="create-the-appliance-vm"></a>Creación de la máquina virtual del dispositivo

Importe el archivo descargado y cree la VM.

1. Extraiga el archivo del disco duro virtual comprimido en la carpeta del host de Hyper-V que hospedará la VM del dispositivo. Se extraen tres carpetas.
2. Abra el administrador de Hyper-V. En **Acciones**, haga clic en **Importar máquina virtual**.
2. En el Asistente para importar máquinas virtuales > **Antes de comenzar**, haga clic en **Siguiente**.
3. En **Buscar carpeta**, especifique la carpeta que contiene el disco duro virtual extraído. A continuación, haga clic en **Siguiente**.
1. En **Seleccionar máquina virtual**, haga clic en **Siguiente**.
2. En **Elegir tipo de importación**, haga clic en **Copiar la máquina virtual (crear un identificador único nuevo)** . A continuación, haga clic en **Siguiente**.
3. En **Elegir destino**, deje la configuración predeterminada. Haga clic en **Next**.
4. En **Carpetas de almacenamiento**, deje la configuración predeterminada. Haga clic en **Next**.
5. En **Elegir red**, especifique el conmutador virtual que usará la VM. El conmutador necesita conectividad a Internet para enviar datos a Azure.
6. En **Resumen**, revise los valores de configuración. Haga clic en **Finalizar**.
7. En Administrador de Hyper-V > **Máquinas virtuales**, inicie la VM.


### <a name="verify-appliance-access-to-azure"></a>Comprobación de que el dispositivo puede acceder a Azure

Asegúrese de que la VM del dispositivo pueda conectarse a las direcciones URL de Azure para las nubes [públicas](migrate-appliance.md#public-cloud-urls) y [gubernamentales](migrate-appliance.md#government-cloud-urls).

### <a name="configure-the-appliance"></a>Configuración del dispositivo

Configure el dispositivo por primera vez.

> [!NOTE]
> Si configura la aplicación mediante un [script de PowerShell](deploy-appliance-script.md), en lugar del disco duro virtual descargado, los dos primeros pasos de este procedimiento no son pertinentes.

1. En Administrador de Hyper-V > **Máquinas virtuales**, haga clic con el botón derecho en la VM > **Conectar**.
2. Especifique el idioma, la zona horaria y la contraseña del dispositivo.
3. Abra un explorador en cualquier equipo que pueda conectarse a la máquina virtual y abra la dirección URL de la aplicación web del dispositivo: **https://*nombre o dirección IP del dispositivo*: 44368**.

   Como alternativa, puede abrir la aplicación desde el escritorio del dispositivo, para lo que debe hacer clic en el acceso directo de la aplicación.
1. Acepte los **términos de licencia** y lea la información de terceros.
1. En la aplicación web > **Set up prerequisites** (Configurar los requisitos previos ), realice las siguientes operaciones:
    - **Connectivity** (Conectividad): la aplicación comprueba que la máquina virtual tiene acceso a Internet. Si la máquina virtual usa un proxy:
      - Haga clic en **Configurar el proxy** y especifique la dirección del proxy (con los formatos http://ProxyIPAddress o http://ProxyFQDN) ) y el puerto de escucha.
      - Especifique las credenciales si el proxy requiere autenticación.
      - Solo se admite un proxy HTTP.
      - Si ha agregado detalles del proxy o ha deshabilitado el proxy o la autenticación, haga clic en **Guardar** para desencadenar la comprobación de conectividad.
    - **Time sync** (Sincronización de hora): Se comprueba la hora. Para que la detección funcione correctamente, la hora del dispositivo debe estar sincronizada con la hora de Internet.
    - **Instalación de actualizaciones**: Azure Migrate Server Assessment comprueba que el dispositivo tiene instaladas las actualizaciones más recientes. Una vez finalizada la comprobación, puede hacer clic en **Ver servicios del dispositivo** para ver el estado y las versiones de los componentes que se ejecutan en el dispositivo.

### <a name="register-the-appliance-with-azure-migrate"></a>Registro del dispositivo en Azure Migrate

1. Pegue la **clave del proyecto de Azure Migrate** copiada desde el portal. Si no tiene la clave, vaya a **Server Assessment > Detectar > Administrar los dispositivos existentes**, seleccione el nombre del dispositivo que proporcionó en el momento de la generación de la clave y copie la clave correspondiente.
1. Haga clic en **Iniciar sesión**. Se abrirá un mensaje de inicio de sesión de Azure en una nueva pestaña del explorador. Si no aparece, asegúrese de que ha deshabilitado el bloqueador de elementos emergentes en el explorador.
1. En la nueva pestaña, inicie sesión con su nombre de usuario y contraseña de Azure.
   
   No se admite el inicio de sesión con un PIN.
3. Después de iniciar sesión correctamente, vuelva a la aplicación web. 
4. Si la cuenta de usuario de Azure que se usa para el registro tiene los [permisos](tutorial-prepare-hyper-v.md#prepare-azure) adecuados en los recursos de Azure creados durante la generación de la clave, se iniciará el registro del dispositivo.
1. Una vez que el dispositivo se ha registrado correctamente, puede ver los detalles de registro haciendo clic en **Ver detalles**.



### <a name="delegate-credentials-for-smb-vhds"></a>Delegación de credenciales para discos duros virtuales de SMB

Si va a ejecutar discos duros virtuales en SMB, debe habilitar la delegación de credenciales desde el dispositivo a los hosts de Hyper-V. Para ello, desde el dispositivo:

1. En la VM del dispositivo, ejecute este comando. HyperVHost1 y HyperVHost2 son nombres de host de ejemplo.

    ```
    Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com, HyperVHost2.contoso.com, HyperVHost1, HyperVHost2 -Force
    ```

2. También puede hacerlo en el Editor de directivas de grupo local en el dispositivo:
    - En **Directiva de equipo local** > **Configuración del equipo**, haga clic en **Plantillas administrativas** > **Sistema** > **Delegación de credenciales**.
    - Haga doble clic en **Permitir delegación de credenciales nuevas** y seleccione **Habilitado**.
    - En **Opciones**, haga clic en **Mostrar** y agregue cada host de Hyper-V que desee detectar en la lista, con **wsman/** como prefijo.
    - En **Delegación de credenciales**, haga doble clic en **Permitir la delegación de credenciales nuevas con autenticación solo NTLM de servidor**. De nuevo, agregue cada host de Hyper-V que quiera detectar en la lista, con **wsman/** como prefijo.

## <a name="start-continuous-discovery"></a>Inicio de detección continua

Conéctese desde el dispositivo a los hosts o clústeres de Hyper-V e inicie la detección de VM.

1. En **Paso 1: Proporcionar las credenciales del host de Hyper-V**, haga clic en **Agregar credenciales** para especificar un nombre descriptivo para las credenciales, agregue un **nombre de usuario** y una **contraseña** para el host o el clúster de Hyper-V que utilizará el dispositivo para detectar máquinas virtuales. Haga clic en **Guardar**.
1. Si quiere agregar varias credenciales a la vez, haga clic en **Agregar más** para guardar y agregar más credenciales. Se admiten varias credenciales para la detección de máquinas virtuales de Hyper-V.
1. En el **Paso 2: Proporcionar los detalles del host o el clúster de Hyper-V**, haga clic en **Agregar origen de detección** para especificar la **dirección IP o el FQDN** del host o el clúster de Hyper-V y el nombre descriptivo de las credenciales para conectarse al host o al clúster.
1. Puede **Agregar un solo elemento** cada vez o **Agregar varios elementos** de una sola vez. También hay una opción para proporcionar los detalles del host o el clúster de Hyper-V a través de **Importar CSV**.


    - Si elige **Agregar un solo elemento**, debe especificar el nombre descriptivo de las credenciales y la **dirección IP o el FQDN** del host o el clúster de Hyper-V y hacer clic en **Guardar**.
    - Si elige **Agregar varios elementos** _(opción seleccionada de manera predeterminada)_ , puede agregar varios registros a la vez mediante la especificación de la **dirección IP o el FQDN** del host o el clúster de Hyper-V con el nombre descriptivo de las credenciales en el cuadro de texto. **Compruebe** los registros agregados y haga clic en **Guardar**.
    - Si elige **Importar CSV**, puede descargar un archivo de plantilla CSV, rellenar el archivo con la **dirección IP o el FQDN** del host o el clúster de Hyper-V y el nombre descriptivo de las credenciales. A continuación, importe el archivo en el dispositivo, **compruebe** los registros del archivo y haga clic en **Guardar**.

1. Al hacer clic en Guardar, el dispositivo intentará validar la conexión a los hosts o clústeres agregados de Hyper-V y mostrar el **estado de validación** en la tabla en cada host o clúster.
    - En el caso de los hosts o clústeres validados correctamente, puede ver más detalles si hace clic en su dirección IP o FQDN.
    - Si se produce un error de validación para un host, haga clic en **Error en la validación** en la columna Estado de la tabla para revisar el error. Corrija el problema y vuelva a validar.
    - Para quitar hosts o clústeres, seleccione **Eliminar**.
    - No se puede quitar un host específico de un clúster. Solo puede quitar todo el clúster.
    - Puede agregar un clúster, incluso si hay problemas con hosts específicos del clúster.
1. Puede **volver a validar** la conectividad a los hosts o clústeres en cualquier momento antes de iniciar la detección.
1. Haga clic en **Iniciar detección** para dar comienzo a la detección de máquinas virtuales de los hosts o clústeres validados correctamente. Una vez que la detección se ha iniciado correctamente, puede comprobar el estado de detección en cada host o clúster de la tabla.

De esta forma comienza la detección. Los metadatos de los servidores detectados tardan alrededor de 2 minutos por host en aparecer en Azure Portal.

## <a name="verify-vms-in-the-portal"></a>Comprobación de VM en el portal

Una vez finalizada la detección, puede verificar que las VM aparezcan en el portal.

1. Abra el panel de Azure Migrate.
2. En la página **Azure Migrate - Servidores** > **Azure Migrate: Server Assessment**, haga clic en el icono que muestra el número de **servidores detectados**.

## <a name="next-steps"></a>Pasos siguientes

- [Evaluación de máquinas virtuales de Hyper-V](tutorial-assess-hyper-v.md) para la migración a máquinas virtuales de Azure.
- [Revise los datos](migrate-appliance.md#collected-data---hyper-v) que el dispositivo recopila durante la detección.
