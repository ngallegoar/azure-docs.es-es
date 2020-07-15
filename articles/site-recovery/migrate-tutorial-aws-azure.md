---
title: Migración de máquinas virtuales de AWS a Azure con el servicio Azure Site Recovery | Microsoft Docs
description: En este artículo se describe cómo migrar máquinas virtuales Windows que se ejecutan en Amazon Web Services (AWS) a Azure mediante Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 09/09/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 1070230d968a6239a5a4aab5ac412280393d17da
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86133178"
---
# <a name="migrate-amazon-web-services-aws-vms-to-azure"></a>Migración de máquinas virtuales de Amazon Web Services (AWS) a Azure

En este tutorial se muestra cómo migrar máquinas virtuales (VM) de Amazon Web Services (AWS) a máquinas virtuales de Azure con Azure Site Recovery. Al migrar instancias de AWS EC2 a Azure, se tratan las máquinas virtuales como si fuesen equipos físicos locales. En este tutorial, aprenderá a:


> [!TIP]
> A partir de ahora debería usar el servicio de Azure Migrate para migrar máquinas virtuales de AWS a Azure, en lugar del servicio Azure Site Recovery. [Más información](../migrate/tutorial-migrate-physical-virtual-machines.md).


> [!div class="checklist"]
> * Verificar los requisitos previos
> * Preparar recursos de Azure
> * Preparar las instancias de AWS EC2 para la migración
> * Implementar un servidor de configuración
> * Habilitar la replicación para máquinas virtuales
> * Probar la conmutación por error para asegurarse de que todo funciona
> * Ejecutar una conmutación por error única en Azure

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.


## <a name="prerequisites"></a>Prerrequisitos
- Asegúrese de que las máquinas virtuales que quiere migrar ejecutan una versión del sistema operativo compatible. Las versiones admitidas son: 
  - Windows Server 2016 
  - Windows Server 2012 R2
  - Windows Server 2012 
  - Versión de 64 bits de Windows Server 2008 R2 SP1 o posterior
  - Red Hat Enterprise Linux 6.4 a 6.10, 7.1 a 7.6 (solo instancias virtualizadas de HVM) *(no se admiten instancias que ejecuten controladores RedHat PV).*
  - CentOS 6.4 a 6.10 7.1 a 7.6 (solo instancias virtualizadas de HVM)
 
- Mobility Service debe instalarse en todas las máquinas que se quieran replicar. 

    > [!IMPORTANT]
    > Site Recovery instala este servicio automáticamente cuando se habilita la replicación para la máquina virtual. Para la instalación automática, debe preparar una cuenta en las instancias de EC2, que Site Recovery usará para acceder a la máquina virtual. Puede usar una cuenta local o de dominio. 
    > - En el caso de las máquinas virtuales Linux, debe ser la cuenta raíz en el servidor Linux de origen. 
    > - En el caso de las máquinas virtuales Windows, si no usa una cuenta de dominio, deshabilite el control Acceso de usuarios remotos en la máquina local:
    >
    >      En el Registro, en **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, agregue la entrada DWORD **LocalAccountTokenFilterPolicy** y establezca el valor en **1**.

- Una instancia de EC2 aparte, que puede usar como servidor de configuración de Site Recovery. Esta instancia debe ejecutar Windows Server 2012 R2.

## <a name="prepare-azure-resources"></a>Preparar recursos de Azure

Debe tener algunos recursos preparados en Azure para las instancias de EC2 migradas que se van a usar. Se incluyen una cuenta de almacenamiento, un almacén y una red virtual.

### <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

Las imágenes de máquinas replicadas se conservan en Azure Storage. Las máquinas virtuales de Azure se crean desde el almacenamiento cuando se realiza la conmutación por error desde el entorno local en Azure.

1. En [Azure Portal](https://portal.azure.com), en el menú de la izquierda, seleccione **Crear un recurso** > **Almacenamiento** > **Cuenta de almacenamiento**.
2. Escriba un nombre para la cuenta de almacenamiento. En estos tutoriales, usamos el nombre **awsmigrated2017**. El nombre debe:
    - Ser único en Azure
    - Tener entre 3 y 24 caracteres
    - Contener solo números y letras minúsculas
3. Mantenga los valores predeterminados de **Modelo de implementación**, **Tipo de cuenta**, **Rendimiento** y **Se requiere transferencia segura**.
5. Seleccione el valor predeterminado **RA-GRS** en **Replicación**.
6. Seleccione la suscripción que quiere usar para este tutorial.
7. En **Grupo de recursos**, seleccione **Crear nuevo**. En este ejemplo, utilizamos **migrationRG** para el nombre del grupo de recursos.
8. En **Ubicación**, seleccione **Oeste de Europa**.
9. Seleccione **Crear** para crear la cuenta de almacenamiento.

### <a name="create-a-vault"></a>Creación de un almacén

1. En [Azure Portal](https://portal.azure.com), seleccione **Todos los servicios**. Busque y seleccione **Almacenes de Recovery Services**.
2. En la página Almacenes de Azure Recovery Services, seleccione **Agregar**.
3. En **Nombre**, escriba **myVault**.
4. En **Suscripción**, seleccione la suscripción que quiera usar.
4. En **Grupo de recursos**, seleccione **Usar existente** y, después, **migrationRG**.
5. En **Ubicación**, seleccione **Oeste de Europa**.
5. Para acceder rápidamente al nuevo almacén desde el panel, seleccione **Anclar al panel**.
7. Seleccione **Crear** cuando haya terminado.

Para ver el nuevo almacén, vaya a **Panel** > **Todos los recursos**. El nuevo almacén aparecerá en la página principal de **Almacenes de Recovery Services**.

### <a name="set-up-an-azure-network"></a>Configurar una red de Azure

Cuando se crean máquinas virtuales de Azure después de la migración (conmutación por error), se unen a esta red de Azure.

1. En [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso** > **Redes** >
   **Red virtual**.
3. En **Nombre**, escriba **myMigrationNetwork**.
4. Deje el valor predeterminado de **Espacio de direcciones** (es preciso escribir un valor).
5. En **Suscripción**, seleccione la suscripción que quiera usar.
6. En **Grupo de recursos**, seleccione **Usar existente** y, después, **migrationRG**.
7. En **Ubicación**, seleccione **Oeste de Europa**.
8. En **Subred**, deje los valores predeterminados de **Nombre** e **Intervalo IP** (es preciso especificar un valor).
9. Agregue instrucciones para la configuración de protección contra DDoS.
10. Mantenga la opción **Puntos de conexión de servicio** deshabilitada.
11. Agregue instrucciones para la configuración de firewall.
12. Seleccione **Crear** cuando haya terminado.

## <a name="prepare-the-infrastructure"></a>Preparación de la infraestructura

En la página del almacén de Azure Portal, en la sección **Introducción**, seleccione **Site Recovery** y, después, seleccione **Preparar infraestructura**. Complete los siguientes pasos.

### <a name="1-protection-goal"></a>1: Objetivo de protección

Seleccione los valores siguientes en la página **Objetivo de protección**:

|    |  |
|---------|-----------|
| ¿Dónde se encuentran sus máquinas? |Seleccione **Local**.|
| ¿Dónde quiere replicar las máquinas? |Seleccione **En Azure**.|
| ¿Está realizando una migración? | Seleccione **Sí** y, a continuación, active la casilla junto a **I understand, but I would like to continue with Azure Site Recovery** (Lo entiendo, pero deseo continuar con Azure Site Recovery).
| ¿Las máquinas están virtualizadas? |Seleccione **No virtualizado/Otro**.|

Cuando haya terminado, seleccione **Aceptar** para continuar con la siguiente sección.

### <a name="2-select-deployment-planning"></a>2: Selección de un planeamiento de implementación

En **¿Completó el planeamiento de implementación?** , seleccione **Lo haré más tarde** y, a continuación, seleccione **Aceptar**.

### <a name="3-prepare-source"></a>3: Preparación del origen

En la página **Preparar origen**, seleccione **+Servidor de configuración**.

1. Use una instancia de EC2 que ejecute Windows Server 2012 R2 para crear un servidor de configuración y registrarlo en el almacén de recuperación.
2. Configure el proxy en la máquina virtual de la instancia de EC2 que usa como el servidor de configuración para que pueda acceder a [Direcciones URL del servicio](./vmware-physical-azure-support-matrix.md).
3. Descargue [Instalación unificada de Microsoft Azure Site Recovery](https://aka.ms/unifiedinstaller_wus). Puede descargarlo en la máquina local y, a continuación, copiarlo en la máquina virtual que usa como el servidor de configuración.
4. Seleccione el botón **Descargar** para descargar la clave de registro del almacén. Copie el archivo descargado en la máquina virtual que usa como el servidor de configuración.
5. En la máquina virtual, haga clic con el botón derecho en el instalador que descargó para la Instalación unificada de Microsoft Azure Site Recovery y seleccione **Ejecutar como administrador**.

    1. En **Antes de comenzar**, seleccione **Install the configuration server and process server** (Instalar el servidor de configuración y el servidor de procesos) y luego haga clic en **Siguiente**.
    2. En **Third-Party Software License** (Licencia de software de terceros), haga clic en **I Accept the third party license agreement** (Acepto el contrato de licencia de terceros) y, a continuación, seleccione **Siguiente**.
    3. En **Registro**, seleccione **Examinar** y vaya a donde colocó el archivo de clave de registro del almacén. Seleccione **Next** (Siguiente).
    4. En **Configuración de Internet**, seleccione **Conectar directamente con Azure Site Recovery sin un servidor proxy** y, a continuación, seleccione **Siguiente**.
    5. En la página **Comprobación de requisitos previos**, se ejecutan comprobaciones de varios elementos. Cuando haya finalizado, seleccione **Siguiente**.
    6. En **Configuración de MySQL**, proporcione las contraseñas necesarias y, a continuación, seleccione **Siguiente**.
    7. En **Detalles del entorno**, seleccione **No**. No es necesario proteger las máquinas de VMware. Después, seleccione **Siguiente**.
    8. En **Ubicación de instalación**, seleccione **Siguiente** para aceptar el valor predeterminado.
    9. En **Selección de red**, seleccione **Siguiente** para aceptar el valor predeterminado.
    10. En **Resumen**, seleccione **Instalar**.
    11. **Progreso de la instalación** le muestra información acerca del proceso de instalación. Cuando haya finalizado, seleccione **Finalizar**. Aparece una ventana que muestra un mensaje sobre un reinicio. Seleccione **Aceptar**. A continuación, en una ventana se muestra un mensaje sobre la frase de contraseña de la conexión del servidor de configuración. Copie la frase de contraseña en el Portapapeles y guárdela en algún lugar seguro.
6. En la máquina virtual, ejecute cspsconfigtool.exe para crear una o varias cuentas de administración en el servidor de configuración. Asegúrese de que las cuentas de administración tienen permisos de administrador en las instancias de EC2 que quiere migrar.

Cuando termine de configurar el servidor de configuración, vuelva al portal y seleccione el servidor que creó como **Servidor de configuración**. Seleccione **Aceptar** para ir a 3: Preparación del destino.

### <a name="4-prepare-target"></a>4: Preparación del destino

En esta sección, debe especificar información sobre los recursos que creó en la sección [Preparar recursos de Azure](#prepare-azure-resources) anteriormente en este tutorial.

1. En **Suscripción**, seleccione la suscripción de Azure que usó para el tutorial [Preparación de Azure](tutorial-prepare-azure.md).
2. Seleccione **Resource Manager** como modelo de implementación.
3. Site Recovery comprueba que tiene una o más redes y cuentas de Azure Storage compatibles. Estos deberían ser los recursos que creó en la sección [Preparar recursos de Azure](#prepare-azure-resources) anteriormente en este tutorial.
4. Cuando finalice, seleccione **Aceptar**.

### <a name="5-prepare-replication-settings"></a>5: Preparación de la configuración de replicación

Debe crear una directiva de replicación para poder habilitar la replicación.

1. Seleccione **Crear y asociar**.
2. En **Nombre**, escriba **myReplicationPolicy**.
3. Deje el resto de la configuración en los valores predeterminados y, después, seleccione **Aceptar** para crear la directiva. La nueva directiva se asocia automáticamente al servidor de configuración.

Cuando haya acabado con las cinco secciones de **Preparar infraestructura**, seleccione **Aceptar**.

## <a name="enable-replication"></a>Habilitar replicación

Habilite la replicación para cada una de las máquinas virtuales que quiera migrar. Cuando se habilita la replicación, Site Recovery instala Mobility Service automáticamente.

1. Vaya a [Azure Portal](https://portal.azure.com).
1. En la página del almacén, en **Introducción**, seleccione **Site Recovery**.
2. En **Para máquinas locales y máquinas virtuales de Azure**, seleccione **Paso 1: Replicar la aplicación**. Complete las páginas del asistente con la siguiente información. Seleccione **Aceptar** en cada página cuando haya terminado:
   - 1: Configuración del origen

     |  |  |
     |-----|-----|
     | Origen: | Seleccione **Local**.|
     | Ubicación de origen:| Escriba el nombre de la instancia de EC2 del servidor de configuración.|
     |Tipo de máquina: | Seleccione **Máquinas físicas**.|
     | Servidor de proceso: | seleccione el servidor de configuración en la lista desplegable.|

   - 2: Configuración del destino

     |  |  |
     |-----|-----|
     | Destino: | deje el valor predeterminado.|
     | Suscripción: | Seleccione la suscripción que ha usado.|
     | Grupo de recursos posterior a la conmutación por error:| Use el grupo de recursos que se creó en la sección [Preparar recursos de Azure](#prepare-azure-resources).|
     | Modelo de implementación posterior a la conmutación por error: | Select **Administrador de recursos**.|
     | Cuenta de almacenamiento: | Seleccione la cuenta de almacenamiento que se creó en la sección [Preparar recursos de Azure](#prepare-azure-resources).|
     | Red de Azure: | Seleccione **Configurar ahora para las máquinas seleccionadas**.|
     | Red de Azure posterior a la conmutación por error: | Elija la red que se creó en [Preparar recursos de Azure](#prepare-azure-resources).|
     | Subred: | Seleccione el **valor predeterminado** de la lista desplegable.|

   - 3: Selección de máquinas físicas

     Seleccione **Máquina física** y, a continuación, escriba los valores de **Nombre**, **Dirección IP** y **Tipo de SO** de la instancia de EC2 que quiere migrar. Seleccione **Aceptar**.

   - 4: Propiedades de configuración

     Seleccione la cuenta que creó en el servidor de configuración y seleccione **Aceptar**.

   - 5: Configuración de las opciones de replicación

     Asegúrese de que la directiva de replicación seleccionada en la lista desplegable es **myReplicationPolicy** y, a continuación, seleccione **Aceptar**.

3. Cuando se complete el asistente, seleccione **Habilitar replicación**.

Para realizar un seguimiento del progreso del trabajo **Habilitar la protección**, vaya a **Supervisión e informes** > **Trabajos** > **Trabajos de Site Recovery**. La máquina estará preparada para la conmutación por error después de que finalice el trabajo **Finalizar la protección**.        

Cuando se habilita la replicación para una máquina virtual, los cambios pueden tardar 15 minutos o más en aplicarse y aparecer en el portal.

## <a name="run-a-test-failover"></a>Ejecución de una conmutación por error de prueba

Cuando se ejecuta una conmutación por error de prueba, ocurren los siguientes eventos:

- Se ejecuta una comprobación de requisitos previos para garantizar que se dan todas las condiciones necesarias para la conmutación por error.
- La conmutación por error procesa los datos, de modo que se pueda crear una máquina virtual de Azure. Si se selecciona el último punto de recuperación, se crea un punto de recuperación a partir de los datos.
- Se crea una máquina virtual de Azure mediante los datos procesados en el paso anterior.

En el portal, ejecute la conmutación por error de prueba:

1. En la página de su almacén, vaya a **Elementos protegidos** > **Elementos replicados**. Seleccione la máquina virtual y, a continuación, seleccione **Conmutación por error de prueba**.
2. Seleccione un punto de recuperación para usarlo en la conmutación por error:
    - **Procesado más recientemente**: conmuta por error la máquina virtual en el último punto de recuperación procesado por Site Recovery. Se muestra la marca de tiempo. Con esta opción, no se emplea tiempo en el procesamiento de datos, por lo que se proporciona un objetivo de tiempo de recuperación (RTO) bajo.
    - **Más reciente coherente con la aplicación**: esta opción conmuta por error todas las VM en el punto de recuperación más reciente coherente con la aplicación. Se muestra la marca de tiempo.
    - **Personalizado**: seleccione un punto de recuperación.

3. En **Probar conmutación por error**, seleccione la red de Azure de destino a la que se conectarán las máquinas virtuales de Azure después de la conmutación por error. Debe ser la red que se creó en la sección [Preparar recursos de Azure](#prepare-azure-resources).
4. Seleccione **Aceptar** para iniciar la conmutación por error. Para realizar el seguimiento del progreso, seleccione la máquina virtual para ver sus propiedades. O bien puede seleccionar el trabajo **Conmutación por error de prueba** en la página del almacén. Para ello, seleccione **Supervisión e informes** > **Trabajos** >  **Trabajos de Site Recovery**.
5. Una vez finalizada la conmutación por error, la máquina virtual de Azure de réplica aparece en Azure Portal. Para ver la máquina virtual, seleccione **Máquinas virtuales**. Asegúrese de que la máquina virtual tiene el tamaño adecuado, que está conectada a la red correspondiente y que se está ejecutando.
6. Ahora debería poder conectarse a la VM replicada en Azure.
7. Para eliminar máquinas virtuales de Azure que se crearon durante la conmutación por error de prueba, seleccione **Limpiar conmutación por error de prueba** en el plan de recuperación. En **Notas**, registre y guarde las observaciones asociadas a la conmutación por error de prueba.

En algunos escenarios, la conmutación por error requiere un procesamiento adicional. Este proceso tarda entre 8 y 10 minutos en finalizar.

## <a name="migrate-to-azure"></a>Migración a Azure

Ejecute una conmutación por error real para las instancias de EC2 a fin de migrarlas a máquinas virtuales de Azure:

1. En **Elementos protegidos** > **Elementos replicados**, seleccione las instancias de AWS y, a continuación, seleccione **Conmutación por error**.
2. En **Conmutación por error**, seleccione un **Punto de recuperación** en el que realizar la conmutación por error. Seleccione el punto de recuperación más reciente e inicie la conmutación por error. Puede seguir el progreso de la conmutación por error en la página **Trabajos**.
1. Asegúrese de que la máquina virtual aparece en **Elementos replicados**.
2. Haga clic con el botón derecho en cada máquina virtual y, a continuación, seleccione **Completar la migración**. Esto hace lo siguiente:

   - Con esta acción se completa el proceso de migración, y se detienen la replicación de la máquina virtual de AWS y la facturación de Site Recovery para la VM.
   - Este paso limpia los datos de replicación. No elimina las máquinas virtuales migradas. 

     ![Completar migración](./media/migrate-tutorial-aws-azure/complete-migration.png)

> [!WARNING]
> *No cancele una conmutación por error en curso*. Antes de iniciar la conmutación por error, se detiene la replicación de la máquina virtual. Si se cancela una conmutación por error en curso, la conmutación por error se detiene, pero no se replica la máquina virtual de nuevo.  


## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo migrar instancias de AWS EC2 en máquinas virtuales de Azure. Para más información acerca de las máquinas virtuales de Azure, continúe con los tutoriales de máquinas virtuales Windows.

> [!div class="nextstepaction"]
> [Tutoriales de máquinas virtuales Windows de Azure](../virtual-machines/windows/tutorial-manage-vm.md)
