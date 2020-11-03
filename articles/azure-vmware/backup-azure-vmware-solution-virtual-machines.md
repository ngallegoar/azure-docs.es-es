---
title: Copia de seguridad de máquinas virtuales de Azure VMware Solution con Azure Backup Server
description: Configure el entorno de Azure VMware Solution para realizar copias de seguridad de máquinas virtuales mediante Azure Backup Server.
ms.topic: how-to
ms.date: 06/09/2020
ms.openlocfilehash: d4273980a134fbdaabe64215aaf0b66a53253788
ms.sourcegitcommit: d6a739ff99b2ba9f7705993cf23d4c668235719f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92495696"
---
# <a name="back-up-azure-vmware-solution-vms-with-azure-backup-server"></a>Copia de seguridad de máquinas virtuales de Azure VMware Solution con Azure Backup Server

En este artículo, se realizará una copia de seguridad de las máquinas virtuales (VM) de VMware que se ejecutan en Azure VMware Solution con Azure Backup Server. Primero, consulte detenidamente [Configuración de Microsoft Azure Backup Server para Azure VMware Solution](set-up-backup-server-for-azure-vmware-solution.md).

A continuación, examinaremos todos los procedimientos necesarios para:

> [!div class="checklist"] 
> * Configurar un canal seguro para que Azure Backup Server pueda comunicarse con los servidores de VMware a través de HTTPS. 
> * Agregar las credenciales de la cuenta a Azure Backup Server. 
> * Agregar vCenter a Azure Backup Server. 
> * Configurar un grupo de protección que contenga las máquinas virtuales de VMware de las que desea realizar copias de seguridad, y especificar la configuración de copia de seguridad y programarla.

## <a name="create-a-secure-connection-to-the-vcenter-server"></a>Creación de una conexión segura con vCenter Server

De forma predeterminada, Azure Backup Server se comunica con los servidores de VMware a través de HTTPS. Para configurar la conexión HTTPS, descargue el certificado de la entidad de certificación (CA) de VMware e impórtelo en Azure Backup Server.

### <a name="set-up-the-certificate"></a>Configuración del certificado

1. En el explorador de la máquina de Azure Backup Server, escriba la dirección URL del cliente web de vSphere.

   > [!NOTE] 
   > Si no aparece la página **Introducción** de VMware, compruebe la configuración del proxy del explorador y la conexión y vuelva a intentarlo.

1. En la página **Introducción** de VMware, seleccione **Descargar certificados de CA raíz de confianza**.

   :::image type="content" source="../backup/media/backup-azure-backup-server-vmware/vsphere-web-client.png" alt-text="Cliente web de vSphere":::

1. Guarde el archivo **download.zip** en la máquina de Azure Backup Server y, a continuación, extraiga su contenido en la carpeta **certs** , que contiene:

   - El archivo del certificado raíz con una extensión que comienza con una secuencia numerada como .0 y .1.
   - El archivo CRL con una extensión que comienza con una secuencia numerada como .r0 y .r1.

1. En la carpeta **certs** , haga clic con el botón derecho en el archivo de certificado raíz y, después, seleccione **Cambiar nombre** para cambiar la extensión a **.crt**.

   El icono del archivo cambia a uno que representa un certificado raíz.

1. Haga clic con el botón derecho en el certificado raíz y seleccione **Instalar certificado**.

1. En el **Asistente para importar certificados** , seleccione **Máquina local** como destino del certificado y seleccione en **Siguiente**.

   ![Página principal del asistente](../backup/media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)

   > [!NOTE] 
   > Si se le pregunta, confirme que quiere permitir cambios en el equipo.

1. Seleccione **Colocar todos los certificados en el siguiente almacén** y, después, seleccione **Examinar** para elegir el almacén de certificados.

   ![Almacenamiento de certificados](../backup/media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

1. Seleccione **Entidades de certificación raíz de confianza** como carpeta de destino para los certificados y seleccione **Aceptar**.

1. Revise la configuración y seleccione **Finalizar** para empezar a importar el certificado.

   ![Comprobar que el certificado se encuentra en la carpeta correcta](../backup/media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)

1. Una vez confirmada la importación del certificado, inicie sesión en vCenter Server para comprobar que la conexión es segura.

### <a name="enable-tls-12-on-azure-backup-server"></a>Habilitación de TLS 1.2 en Azure Backup Server

En VMware 6.7 y versiones posteriores se ha habilitado TLS como protocolo de comunicación. 

1. Copie los siguientes valores del registro y péguelos en el Bloc de notas. A continuación, guarde el archivo como TLS.REG sin la extensión .txt.

   ```text
   
   Windows Registry Editor Version 5.00
   
   [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v2.0.50727]
   
      "SystemDefaultTlsVersions"=dword:00000001
   
      "SchUseStrongCrypto"=dword:00000001
   
   [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319]
   
      "SystemDefaultTlsVersions"=dword:00000001
   
      "SchUseStrongCrypto"=dword:00000001
   
   [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v2.0.50727]
   
      "SystemDefaultTlsVersions"=dword:00000001
   
      "SchUseStrongCrypto"=dword:00000001
   
   [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
   
      "SystemDefaultTlsVersions"=dword:00000001
   
      "SchUseStrongCrypto"=dword:00000001
   
   ```

1. Haga clic con el botón derecho en TLS.REG y seleccione **Combinar** o **Abrir** para agregar la configuración al registro.


## <a name="add-the-account-on-azure-backup-server"></a>Incorporación de la cuenta a Azure Backup Server

1. Abra Azure Backup Server y, en la consola, seleccione **Administración** > **Servidores de producción** > **Administrar VMware**.

   ![Consola de Azure Backup Server](../backup/media/backup-azure-backup-server-vmware/add-vmware-credentials.png)

1. En el cuadro de diálogo **Administrar credenciales** , seleccione **Agregar**.

   ![En el cuadro de diálogo Administrar credenciales, seleccione Agregar.](../backup/media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

1. En el cuadro de diálogo **Agregar credenciales** , introduzca un nombre y una descripción para la nueva credencial. Especifique el nombre de usuario y la contraseña que ha definido en el servidor de VMware.

   > [!NOTE] 
   > Si el servidor de VMware y Azure Backup Server no están en el mismo dominio, especifique el dominio en el cuadro **Nombre de usuario**.

   ![Cuadro de diálogo Agregar credenciales de Azure Backup Server](../backup/media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

1. Haga clic en **Agregar** para agregar la nueva credencial.

   ![Captura de pantalla que muestra el cuadro de diálogo Administrar credenciales de Azure Backup Server con nuevas credenciales.](../backup/media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)

## <a name="add-the-vcenter-server-to-azure-backup-server"></a>Incorporación de vCenter Server a Azure Backup Server

1. En la consola de Azure Backup Server, seleccione **Administración** > **Servidores de producción** > **Agregar**.

   ![Abrir el Asistente para adición del servidor de producción](../backup/media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)

1. Seleccione **Servidores de VMware** y **Siguiente**.

   ![Asistente para adición del servidor de producción](../backup/media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

1. Especifique la dirección IP de vCenter.

   ![Especificación del servidor de VMware](../backup/media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

1. En el cuadro **Puerto SSL** , escriba el puerto usado para comunicarse con vCenter.

   > [!TIP] 
   > El puerto 443 es el puerto predeterminado, pero puede cambiarlo si vCenter escucha en un puerto diferente.

1. En el cuadro **Especificar credencial** , seleccione la credencial que creó en la sección anterior.

1. Seleccione **Agregar** para agregar vCenter a la lista de servidores y seleccione **Siguiente**.

   ![Agregar credencial y el servidor de VMware](../backup/media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

1. En la pantalla **Resumen** , seleccione **Agregar** para agregar vCenter a Azure Backup Server.

   El nuevo servidor se agrega inmediatamente. vCenter no necesita un agente.

   ![Agregar el servidor de VMware a Azure Backup Server](../backup/media/backup-azure-backup-server-vmware/tasks-screen.png)

1. En la página **Finalizar** , revise la configuración y seleccione **Cerrar**.

   ![Página Finalizar](../backup/media/backup-azure-backup-server-vmware/summary-screen.png)

   Verá el servidor vCenter en el listado de **Servidor de producción** con las siguientes características:
   - Tipo: **Servidor de VMware** 
   - Estado del agente: **Correcto** 
   
      Si ve **Estado del agente** como **Desconocido** , seleccione **Actualizar**.

## <a name="configure-a-protection-group"></a>Configuración de un grupo de protección

Los grupos de protección recopilan varias máquinas virtuales y aplican la misma configuración de copia de seguridad y de retención de datos para todas las máquinas virtuales del grupo.

1. En la consola de Azure Backup Server, seleccione **Protección** > **Nuevo**.

   ![Abrir el asistente Crear nuevo grupo de protección.](../backup/media/backup-azure-backup-server-vmware/open-protection-wizard.png)

1. En la página principal del asistente **Crear grupo de protección** , seleccione **Siguiente**.

   ![Cuadro de diálogo del asistente Crear nuevo grupo de protección](../backup/media/backup-azure-backup-server-vmware/protection-wizard.png)

1. En la página **Seleccionar tipo de grupo de protección** , seleccione **Servidores** y haga clic en **Siguiente**. Se muestra la página **Seleccionar miembros del grupo**.

1. En la página **Seleccionar miembros del grupo** , seleccione las máquinas virtuales (o carpetas de la máquina virtual) de las que quiere realizar copias de seguridad y, después, seleccione **Siguiente**.

   > [!NOTE]
   > Al seleccionar una carpeta o máquinas virtuales, las carpetas dentro de esa carpeta también se seleccionan para la copia de seguridad. Puede desactivar las carpetas o máquinas virtuales de las que no desee copia de seguridad. Si ya se está realizando una copia de seguridad de una máquina virtual o una carpeta, no puede seleccionarla, lo que garantiza que no se creen puntos de recuperación duplicados para una máquina virtual.

   ![Seleccionar a miembros del grupo](../backup/media/backup-azure-backup-server-vmware/server-add-selected-members.png)

1. En la página **Seleccionar método de protección de datos** , introduzca un nombre para el grupo de protección y la configuración. 

1. Configure la protección a corto plazo en **Disco** , habilite la protección en línea y, después, seleccione **Siguiente**.

   ![Seleccionar método de protección de datos](../backup/media/backup-azure-backup-server-vmware/name-protection-group.png)

1. Especifique cuánto tiempo quiere mantener la copia de seguridad de los datos en el disco.

   - **Intervalo de retención** : número de días que se conservan los puntos de recuperación en disco.
   - **Copia de seguridad completa rápida** : frecuencia con la que se toman los puntos de recuperación en disco. Para cambiar las fechas u horas en las que se producen las copias de seguridad a corto plazo, seleccione **Modificar**.

   :::image type="content" source="media/azure-vmware-solution-backup/new-protection-group-specify-short-term-goals.png" alt-text="Especificar los objetivos a corto plazo para la protección en disco":::

1. En **Revisar asignación de Disk Storage** , revise el espacio en disco para las copias de seguridad de máquinas virtuales.

   - Las asignaciones de disco recomendadas se basan en la duración de retención especificada, el tipo de carga de trabajo y el tamaño de los datos protegidos. Realice los cambios necesarios y seleccione **Siguiente**.
   - **Tamaño de los datos:** tamaño de los datos del grupo de protección.
   - **Espacio en disco:** espacio en disco recomendado para el grupo de protección. Si desea modificar esta configuración, seleccione un espacio ligeramente mayor que la cantidad que calcula que va a crecer cada origen de datos.
   - **Detalles del grupo de almacenamiento:** muestra el estado actual del grupo de almacenamiento, que incluye el tamaño total y restante del disco.

   :::image type="content" source="media/azure-vmware-solution-backup/review-disk-allocation.png" alt-text="Revisar el espacio en disco proporcionado en el bloque de almacenamiento":::

   > [!NOTE]
   > En algunos escenarios, el tamaño de datos indicado es mayor que el tamaño real de la máquina virtual. Somos conscientes del problema y lo estamos investigando actualmente.

1. En la página **Seleccionar método de creación de réplicas** , indique cómo quiere realizar la copia de seguridad inicial y seleccione **Siguiente**.

   - El valor predeterminado es **Automáticamente a través de la red** y **Ahora**. Si usa el valor predeterminado, especifique una hora de poco tráfico. Si elige **Más tarde** , especifique un día y una hora.
   - Para grandes cantidades de datos o condiciones de red no del todo óptimas, considere la posibilidad de replicar los datos sin conexión con medios extraíbles.

   ![Elegir método de creación de réplica](../backup/media/backup-azure-backup-server-vmware/replica-creation.png)

1. En **Opciones de comprobación de coherencia** , seleccione cómo y cuándo automatizar las comprobaciones de coherencia y, después, seleccione **Siguiente**.

   - Puede ejecutar comprobaciones de coherencia si los datos de réplica no son coherentes o en una programación establecida.
   - Si no quiere configurar las comprobaciones de coherencia automáticas, puede ejecutar una comprobación manual si hace clic con el botón derecho en el grupo de protección **Realizar comprobación de coherencia**.

1. En la página **Especificar datos de protección en línea** , seleccione las máquinas virtuales o las carpetas de máquina virtual de las que quiera realizar copias de seguridad y, después, seleccione **Siguiente**. 

   > [!TIP]
   > Puede seleccionar los miembros individualmente o elegir **Seleccionar todo** para elegir todos los miembros.

   ![Especificar datos de protección en línea](../backup/media/backup-azure-backup-server-vmware/select-data-to-protect.png)

1. En la página **Especificar la programación de copia de seguridad en línea** , especifique con qué frecuencia desea realizar la copia de seguridad de los datos del espacio de almacenamiento local en Azure. 

   - Se generarán puntos de recuperación de los datos en la nube según la programación. 
   - Después de generar el punto de recuperación, se transfiere al almacén de Recovery Services de Azure.

   ![Especificar programación de copia de seguridad en línea](../backup/media/backup-azure-backup-server-vmware/online-backup-schedule.png)

1. En la página **Especificar la directiva de retención en línea** , indique el tiempo de conservación de los puntos de recuperación que se crean de las copias de seguridad diarias, semanales, mensuales y anuales en Azure.

   - Los datos pueden guardarse en Azure sin límite de tiempo.
   - El único límite es que no se pueden tener más de 9999 puntos de recuperación por instancia protegida. En este ejemplo, la instancia protegida es el servidor de VMware.

   ![Especificar directiva de retención en línea](../backup/media/backup-azure-backup-server-vmware/retention-policy.png)

1. En la página **Resumen** , revise la configuración y seleccione **Crear grupo**.

   ![Resumen de configuración y miembros del grupo de protección](../backup/media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="monitor-with-the-azure-backup-server-console"></a>Supervisión con la consola de Azure Backup Server

Después de configurar el grupo de protección para realizar copias de seguridad de las máquinas virtuales de Azure VMware Solution, puede supervisar el estado del trabajo de copia de seguridad y alerta mediante la consola de Azure Backup Server. Esto es lo que se puede supervisar.

- En el área de tareas **Supervisión** :
   - En **Alertas** , puede supervisar los errores, las advertencias y la información general.  Puede ver las alertas activas e inactivas y configurar las notificaciones por correo electrónico.
   - En **Trabajos** , puede ver los trabajos iniciados por Azure Backup Server para un grupo de protección o un origen de datos protegido específico. Puede seguir el progreso del trabajo o comprobar los recursos consumidos por los trabajos.
- En el área de tareas **Protección** , puede comprobar el estado de los volúmenes y recursos compartidos en el grupo de protección. También puede comprobar las opciones de configuración, como la configuración de recuperación, la asignación de disco y la programación de copia de seguridad.
- En el área de tareas **Administración** , puede ver las pestañas **Discos en línea** y **Agentes** para comprobar el estado de los discos en el bloque de almacenamiento, el registro en Azure y el estado del agente DPM implementado.

:::image type="content" source="media/azure-vmware-solution-backup/monitor-backup-jobs.png" alt-text="Supervisar el estado de los trabajos de copia de seguridad en Azure Backup Server":::

## <a name="restore-vmware-virtual-machines"></a>Restauración de máquinas virtuales de VMware

En la Consola de administrador de Azure Backup Server se pueden hallar datos recuperables de dos formas. Puede buscar o examinar. Al recuperar datos, puede que quiera (o no) restaurar los datos o una máquina virtual en la misma ubicación. Por este motivo, Azure Backup Server admite tres opciones de recuperación en las copias de seguridad de máquinas virtuales de VMware:

- **Recuperación de ubicación original (OLR)** : use OLR para restaurar una máquina virtual protegida en la ubicación original correspondiente. Una máquina virtual se puede restaurar en su ubicación original únicamente si no se han agregado o eliminado discos desde que se hizo la copia de seguridad. Si se han agregado o eliminado discos, deberá usar otra forma de recuperación de ubicación.
- **Recuperación de ubicación alternativa (ALR)** : úsela si la máquina virtual original falta o no quiere suponer una molestia en la máquina virtual original. Proporcione la ubicación de un host ESXi, un grupo de recursos de servidor, una carpeta y la ruta de acceso y el almacén de información de almacenamiento. Para poder diferenciar la máquina virtual restaurada de la máquina virtual original, Azure Backup Server anexa *"-Recuperado"* al nombre de la máquina virtual.
- **Recuperación de ubicación de archivo individual (ILR)** : si la máquina virtual protegida es una máquina virtual de Windows Server, se pueden recuperar archivos o carpetas individuales de la máquina virtual con la capacidad ILR de Azure Backup Server. Para recuperar archivos individuales, vea el procedimiento correspondiente más adelante en este artículo. La restauración de un archivo individual desde una VM solo está disponible para los puntos de recuperación de discos y VM Windows.

### <a name="restore-a-recovery-point"></a>Restauración de un punto de recuperación

1. En la consola de administrador de Azure Backup Server, seleccione la vista **Recuperación**. 

1. Mediante el panel **Examinar** , busque o filtre para encontrar la máquina virtual que quiera recuperar. Después de seleccionar una máquina virtual o una carpeta, el panel **Puntos de recuperación para muestra los puntos de recuperación disponibles.

   ![Puntos de recuperación disponibles](../backup/media/restore-azure-backup-server-vmware/recovery-points.png)

1. En el panel **Puntos de recuperación para** , seleccione una fecha en la que se tomó un punto de recuperación. Las fechas del calendario en negrita son las que tienen puntos de recuperación disponibles. Como alternativa, puede hacer clic con el botón derecho en la máquina virtual y seleccionar **Mostrar todos los puntos de recuperación** y, después, seleccionar el punto de recuperación de la lista.

   > [!NOTE] 
   > Para la protección a corto plazo, seleccione un punto de recuperación basado en disco para una recuperación más rápida. Después de que expiren los puntos de recuperación a corto plazo, solo verá los puntos de recuperación **en línea** para recuperar.

1. Antes de realizar la recuperación desde un punto de recuperación en línea, asegúrese de que la ubicación de almacenamiento provisional contiene suficiente espacio libre para alojar el tamaño sin comprimir completo de la máquina virtual que quiere recuperar. La ubicación de almacenamiento provisional se puede ver o cambiar mediante la ejecución del **Asistente para establecer la configuración de la suscripción**.

   :::image type="content" source="media/azure-vmware-solution-backup/mabs-recovery-folder-settings.png" alt-text="Configuración de la carpeta de recuperación de Azure Backup Server":::

1. Seleccione **Recuperar** para abrir el **Asistente de recuperación**.

   ![Asistente de recuperación, página Revisar selección de recuperación](../backup/media/restore-azure-backup-server-vmware/recovery-wizard.png)

1. Seleccione **Siguiente** para ir a la pantalla **Especificar opciones de recuperación**. Vuelva a seleccionar **Siguiente** para ir a la pantalla **Seleccionar tipo de recuperación**. 

   > [!NOTE]
   > Las cargas de trabajo de VMware no admiten la limitación de ancho de banda de red.

1. En la pantalla **Seleccionar tipo de recuperación** , recupere en la instancia original o en una nueva ubicación.

   - Si elige **Recuperar en instancia original** , no será necesario realizar más opciones en el asistente. Se usan los datos de la instancia original.
   - Si elige **Recuperar como máquina virtual en cualquier host** , indique en la pantalla **Especificar destino** la información que sea necesaria relativa al **host ESXi** , al **grupo de recursos** , a la **carpeta** y a la **ruta de acceso**.

   ![Página Seleccionar tipo de recuperación](../backup/media/restore-azure-backup-server-vmware/recovery-type.png)

1. En la página **Resumen** , revise la configuración y seleccione **Recuperar** para iniciar el proceso de recuperación. 

   La pantalla **Estado de la recuperación** muestra el progreso de la operación de recuperación.

### <a name="restore-an-individual-file-from-a-vm"></a>Restauración de un archivo individual desde una máquina virtual

Puede restaurar archivos individuales desde un punto de recuperación de una máquina virtual protegida. Esta característica solo está disponible en máquinas virtuales de Windows Server. La restauración de archivos individuales es similar a la restauración de toda la máquina virtual, excepto por el hecho de que se examina el VMDK y se buscan los archivos que se quieren antes de iniciar el proceso de recuperación en sí. 

> [!NOTE]
> La restauración de un archivo individual desde una VM solo está disponible para los puntos de recuperación de discos y VM Windows.

1. En la consola de administrador de Azure Backup Server, seleccione la vista **Recuperación**.

1. Mediante el panel **Examinar** , busque o filtre para encontrar la máquina virtual que quiera recuperar. Después de seleccionar una máquina virtual o una carpeta, el panel **Puntos de recuperación para muestra los puntos de recuperación disponibles.

   ![Puntos de recuperación disponibles](../backup/media/restore-azure-backup-server-vmware/vmware-rp-disk.png)

1. En el panel **Puntos de recuperación para** , use el calendario para seleccionar la fecha que contenga los puntos de recuperación que quiera. En función de cómo esté configurada la directiva de copia de seguridad, es posible que las fechas tengan más de un punto de recuperación. 

1. Después de seleccionar el día en que se tomó el punto de recuperación, asegúrese de elegir la **Hora de recuperación** adecuada. 

   > [!NOTE]
   > Si la fecha seleccionada tiene varios puntos de recuperación, elija el punto de recuperación seleccionándolo en el menú desplegable **Hora de recuperación**. 

   Después de elegir el punto de recuperación, la lista de elementos recuperables aparecerá en el panel **Ruta de acceso**.

1. Para encontrar los archivos que quiere recuperar, en el panel **Ruta de acceso** , haga doble clic en el elemento en la columna **Elemento recuperable** para abrirlo. Seleccione el archivo o las carpetas que quiera recuperar. Para seleccionar varios elementos, seleccione la tecla **CTRL** mientras realiza la selección. Use el panel **Ruta de acceso** para buscar en la lista de archivos o carpetas que se muestra en la columna **Elemento recuperable**.
    
   > [!NOTE]
   > La opción **Lista de búsqueda a continuación** no busca en las subcarpetas. Para buscar en las subcarpetas, haga doble clic en la carpeta. Use el botón **Arriba** para pasar de una carpeta secundaria a una carpeta principal. Se pueden seleccionar varios elementos (archivos y carpetas), pero deben estar en la misma carpeta principal. No se pueden recuperar elementos de varias carpetas en el mismo trabajo de recuperación.

   ![Revisar selección de recuperación](../backup/media/restore-azure-backup-server-vmware/vmware-rp-disk-ilr-2.png)

1. Una vez seleccionados los elementos que se van a recuperar, en la cinta de opciones de la Consola de administrador, seleccione **Recuperar** para abrir el **Asistente de recuperación**. En el **Asistente de recuperación** , la pantalla **Revisar selección de recuperación** muestra los elementos seleccionados que se van a recuperar.

1. En la pantalla **Especificar opciones de recuperación** , siga uno de estos pasos:

   - Seleccione **Modificar** para habilitar el límite de ancho de banda de red. En el cuadro de diálogo **Limitación** , seleccione **Habilitar el límite de uso del ancho de banda de red**. Una vez habilitada, configure las opciones **Configuración** y **Programación de trabajos**.
   - Para dejar el límite de red deshabilitado, seleccione **Siguiente**.

1. En la pantalla **Seleccionar tipo de recuperación** , seleccione **Siguiente**. Los archivos o las carpetas solo se pueden recuperar en una carpeta de red.

1. En la pantalla **Especificar destino** , seleccione **Examinar** para buscar una ubicación de red para los archivos o carpetas. Azure Backup Server crea una carpeta donde se copian todos los elementos recuperados. El nombre de la carpeta tiene el prefijo MABS_día-mes-año. Al seleccionar una ubicación para los archivos o carpetas recuperados, se facilitan los detalles de esa ubicación.

   ![Especificar la ubicación donde recuperar archivos](../backup/media/restore-azure-backup-server-vmware/specify-destination.png)

1. En la pantalla **Especificar opciones de recuperación** , elija la configuración de seguridad que quiera aplicar. Puede optar por modificar el límite de uso del ancho de banda de red, pero este límite está deshabilitado de forma predeterminada. Las opciones **Recuperación de SAN** y **Notificación** tampoco están habilitadas.

1. En la pantalla **Resumen** , revise la configuración y seleccione **Recuperar** para iniciar el proceso de recuperación. La pantalla **Estado de la recuperación** muestra el progreso de la operación de recuperación.

## <a name="next-steps"></a>Pasos siguientes

Para solucionar problemas de configuración de las copias de seguridad, revise la guía de solución de problemas de Azure Backup Server.

> [!div class="nextstepaction"]
> [Guía de solución de problemas de Azure Backup Server](../backup/backup-azure-mabs-troubleshoot.md)
