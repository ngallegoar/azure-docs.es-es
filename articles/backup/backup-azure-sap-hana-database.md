---
title: Copia de seguridad de una base de datos de SAP HANA a Azure con Azure Backup
description: En este artículo, aprenderá a realizar copias de seguridad de una base de datos de SAP HANA en máquinas virtuales de Azure con el servicio Azure Backup.
ms.topic: conceptual
ms.date: 11/12/2019
ms.openlocfilehash: deedd4d2553b3b06f76f698fdb2425a8d3878d23
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79226112"
---
# <a name="back-up-sap-hana-databases-in-azure-vms"></a>Copia de seguridad de bases de datos de SAP HANA en máquinas virtuales de Azure

Las bases de datos de SAP HANA son cargas de trabajo críticas que requieren un objetivo de punto de recuperación (RPO) bajo y retención a largo plazo. Puede hacer una copia de seguridad de las bases de datos de SAP HANA que se ejecutan en máquinas virtuales de Azure mediante [Azure Backup](backup-overview.md).

Este artículo muestra cómo realizar una copia de seguridad de las bases de datos de SAP HANA que se ejecutan en máquinas virtuales de Azure en un almacén de Azure Backup Recovery Services.

En este artículo, aprenderá a:
> [!div class="checklist"]
>
> * Crear y configurar un almacén.
> * Detectar bases de datos.
> * Configuración de copias de seguridad
> * Ejecutar un trabajo de copia de seguridad a petición.

>[!NOTE]
>**La eliminación temporal de SQL Server en máquinas virtuales de Azure y la eliminación temporal de SAP HANA en cargas de trabajo de máquinas virtuales de Azure** están ahora disponible en versión preliminar.<br>
>Para suscribirse a la versión preliminar, escriba a AskAzureBackupTeam@microsoft.com.

## <a name="prerequisites"></a>Prerrequisitos

Consulte las secciones sobre [requisitos previos](tutorial-backup-sap-hana-db.md#prerequisites) y [Qué hace el script de registro previo](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does) para configurar la base de datos para la copia de seguridad.

### <a name="set-up-network-connectivity"></a>Configurar la conectividad de red

Para todas las operaciones, la máquina virtual de SAP HANA necesita conectividad a las direcciones IP públicas de Azure. Las operaciones de máquinas virtuales (detección de bases de datos, configuración de copias de seguridad o restauración de puntos de recuperación, entre otras) producirán errores en las direcciones IP públicas de Azure.

Establezca la conectividad con una de las siguientes opciones:

#### <a name="allow-the-azure-datacenter-ip-ranges"></a>Allow the Azure datacenter IP ranges (Permitir los intervalos IP del centro de datos de Azure)

Esta opción permite los [intervalos IP](https://www.microsoft.com/download/details.aspx?id=41653) en el archivo descargado. Para acceder a un grupo de seguridad de red (NSG), use el cmdlet Set-AzureNetworkSecurityRule. Si su lista de destinatarios seguros incluye direcciones IP específicas de una región, también tendrá que actualizar la lista de destinatarios seguros con la etiqueta de servicio de Azure Active Directory (Azure AD) para habilitar la autenticación.

#### <a name="allow-access-using-nsg-tags"></a>Allow access using NSG tags (Permitir el acceso mediante etiquetas de NSG)

Si usa NSG para restringir la conectividad, debe usar la etiqueta de servicio AzureBackup para permitir el acceso saliente a Azure Backup. Además, debe permitir la conectividad para la autenticación y la transferencia de datos mediante [reglas](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) de Azure AD y Azure Storage. Esto puede realizarse desde Azure Portal o a través de PowerShell.

Para crear una regla mediante el portal:

  1. En **Todos los servicios**, vaya a **Grupos de seguridad de red** y seleccione el grupo de seguridad de red.
  2. En **Configuración**, seleccione **Reglas de seguridad de salida**.
  3. Seleccione **Agregar**. Escriba todos los detalles necesarios para crear una nueva regla, como se explica en [Configuración de reglas de seguridad](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group#security-rule-settings). Asegúrese de que la opción **Destino** esté establecida en **Etiqueta de servicio** y de que **Etiqueta de servicio de destino** esté establecida en **AzureBackup**.
  4. Haga clic en **Agregar** para guardar la regla de seguridad de salida recién creada.

Para crear una regla mediante PowerShell:

 1. Agregue las credenciales de la cuenta de Azure y actualice las nubes nacionales.<br/>
      `Add-AzureRmAccount`<br/>

 2. Seleccione la suscripción a NSG.<br/>
      `Select-AzureRmSubscription "<Subscription Id>"`

 3. Seleccione el NSG.<br/>
    `$nsg = Get-AzureRmNetworkSecurityGroup -Name "<NSG name>" -ResourceGroupName "<NSG resource group name>"`

 4. Agregue el permiso de la regla de salida para la etiqueta de servicio de Azure Backup.<br/>
    `Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name "AzureBackupAllowOutbound" -Access Allow -Protocol * -Direction Outbound -Priority <priority> -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix "AzureBackup" -DestinationPortRange 443 -Description "Allow outbound traffic to Azure Backup service"`

 5. Agregue el permiso de la regla de salida para la etiqueta de servicio de Storage.<br/>
    `Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name "StorageAllowOutbound" -Access Allow -Protocol * -Direction Outbound -Priority <priority> -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix "Storage" -DestinationPortRange 443 -Description "Allow outbound traffic to Azure Backup service"`

 6. Agregue el permiso de la regla de salida para la etiqueta de servicio de Azure Active Directory.<br/>
    `Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name "AzureActiveDirectoryAllowOutbound" -Access Allow -Protocol * -Direction Outbound -Priority <priority> -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix "AzureActiveDirectory" -DestinationPortRange 443 -Description "Allow outbound traffic to AzureActiveDirectory service"`

 7. Guarde el NSG.<br/>
    `Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg`

**Allow access by using Azure Firewall tags** (Permitir el acceso mediante el uso de etiquetas de Azure Firewall). Si usa Azure Firewall, cree una regla de aplicación mediante la [etiqueta de nombre de dominio completo](https://docs.microsoft.com/azure/firewall/fqdn-tags) AzureBackup. Esto permite el acceso de salida a Azure Backup.

**Deploy an HTTP proxy server to route traffic** (Implementar un servidor proxy HTTP para enrutar el tráfico). Al hacer una copia de seguridad de una base de datos de SAP HANA en una máquina virtual de Azure, la extensión de copia de seguridad de la máquina virtual usa las API HTTPS para enviar comandos de administración a Azure Backup y datos a Azure Storage. La extensión de copia de seguridad también usa Azure AD para la autenticación. Enrute el tráfico de extensión de copia de seguridad de estos tres servicios a través del proxy HTTP. Las extensiones son el único componente configurado para el acceso a la red pública de Internet.

Las opciones de conectividad incluyen las siguientes ventajas y desventajas:

**Opción** | **Ventajas** | **Desventajas**
--- | --- | ---
Permitir intervalos IP | Sin costos adicionales. | Este escenario es complejo de administrar, ya que los intervalos de direcciones IP cambian con el tiempo. <br/><br/> Proporciona acceso a la totalidad de Azure, no solo a Azure Storage.
Uso de las etiquetas de servicio de NSG | Más fácil de administrar ya que los cambios de intervalo se combinan automáticamente <br/><br/> Sin costos adicionales. <br/><br/> | Solo se puede usar con grupos de seguridad de red. <br/><br/> Proporciona acceso a todo el servicio.
Uso de las etiquetas de nombre de dominio completo de Azure Firewall | Más fácil de administrar ya que los nombres de dominio completo se administran automáticamente. | Solo se puede usar con Azure Firewall.
Usar un servidor proxy HTTP | Se permite un control detallado en el proxy sobre las direcciones URL de almacenamiento. <br/><br/> Un único punto de acceso a Internet para las máquinas virtuales. <br/><br/> No están sujetas a cambios de direcciones IP de Azure. | Costos adicionales de ejecutar una máquina virtual con el software de proxy.

#### <a name="private-endpoints"></a>Puntos de conexión privados

[!INCLUDE [Private Endpoints](../../includes/backup-private-endpoints.md)]

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-the-databases"></a>Detección de bases de datos

1. En el almacén, en **Introducción**, haga clic en **Copia de seguridad**. En **¿Dónde se ejecuta su carga de trabajo?** , seleccione **SAP HANA en máquina virtual de Azure**.
2. Haga clic en **Iniciar detección**. Se iniciará la detección de máquinas virtuales de Linux no protegidas en la región del almacén.

   * Tras la detección, las máquinas virtuales no protegidas aparecen en el portal, enumeradas por nombre y grupo de recursos.
   * Si una máquina virtual no aparece en la lista como cabría esperar, compruebe que no se haya copiado ya en un almacén.
   * Varias máquinas virtuales pueden tener el mismo nombre pero pertenecer a distintos grupos de recursos.

3. En **Seleccionar máquinas virtuales**, haga clic en el vínculo para descargar el script que proporciona permisos para que el servicio Azure Backup obtenga acceso a las máquinas virtuales de SAP HANA para la detección de bases de datos
4. Ejecute el script en cada máquina virtual que hospede bases de datos de SAP HANA de las que desee hacer una copia de seguridad.
5. Tras ejecutar el script en las máquinas virtuales, seleccione las máquinas virtuales en **Seleccionar máquinas virtuales**. A continuación, haga clic en **Detectar bases de datos**.
6. Azure Backup detecta todas las bases de datos de SAP HANA en la máquina virtual. Durante la detección, Azure Backup registra la máquina virtual con el almacén e instala una extensión en la máquina virtual. No se instala ningún agente en la base de datos.

    ![Detectar bases de datos de SAP HANA](./media/backup-azure-sap-hana-database/hana-discover.png)

## <a name="configure-backup"></a>Configuración de la copia de seguridad  

Ahora habilite la copia de seguridad.

1. En el paso 2, haga clic en **Configurar copia de seguridad**.

    ![Configurar la copia de seguridad](./media/backup-azure-sap-hana-database/configure-backup.png)
2. En **Seleccione los elementos de los que desea hacer una copia de seguridad**, seleccione todas las bases de datos que desee proteger y haga clic en **Aceptar**.

    ![Seleccionar elementos de los que se va a hacer una copia de seguridad](./media/backup-azure-sap-hana-database/select-items.png)
3. En **Directiva de copia de seguridad** > **Elegir directiva de copia de seguridad**, cree una directiva de copia de seguridad para las bases de datos, según las instrucciones siguientes.

    ![Elegir directiva de copia de seguridad](./media/backup-azure-sap-hana-database/backup-policy.png)
4. Tras crear la directiva, en el menú **Copia de seguridad**, haga clic en **Habilitar la copia de seguridad**.

    ![Habilitación de la copia de seguridad](./media/backup-azure-sap-hana-database/enable-backup.png)
5. Realice el seguimiento del progreso de la configuración de copia de seguridad en el área **Notificaciones** del portal.

### <a name="create-a-backup-policy"></a>Crear una directiva de copia de seguridad

Una directiva de copia de seguridad define cuándo se realizan las copias de seguridad y cuánto tiempo se conservan.

* Una directiva se crea en el nivel de almacén.
* Varios almacenes pueden usar la misma directiva de copia de seguridad, pero debe aplicar la directiva de copia de seguridad a cada almacén.

Especifique la configuración de la directiva como se muestra a continuación:

1. En **Nombre de la directiva**, escriba un nombre para la nueva directiva.

   ![Escribir nombre de la directiva](./media/backup-azure-sap-hana-database/policy-name.png)
2. En el menú **Full Backup policy** (Directiva de copia de seguridad completa), seleccione un valor para **Backup Frequency** (Frecuencia de copia de seguridad) entre **Daily** (Diaria) o **Weekly** (Semanal).
   * **Diaria**: Seleccione la zona horaria y la hora a la que comienza el trabajo de copia de seguridad.
       * Debe ejecutar una copia de seguridad completa. No se puede desactivar esta opción.
       * Haga clic en **Full Backup** (Copia de seguridad completa) para ver la directiva.
       * No puede crear copias de seguridad diferenciales para copias de seguridad completas diarias.
   * **Semanal**: seleccione el día de la semana, la hora y la zona horaria en la que se ejecuta el trabajo de copia de seguridad.

   ![Seleccionar una frecuencia de copia de seguridad](./media/backup-azure-sap-hana-database/backup-frequency.png)

3. En **Duración de retención**, configure las opciones de retención de la copia de seguridad completa.
    * De forma predeterminada, se seleccionan todas las opciones. Borre los límites de duración de retención que no desee usar y establezca los que sí quiera utilizar.
    * El período de retención mínimo para cualquier tipo de copia de seguridad (completa, diferencial o de registro) es de siete días.
    * Los puntos de recuperación se etiquetan para la retención en función de su duración de retención. Por ejemplo, si selecciona la frecuencia diaria, solo se desencadena una copia de seguridad completa cada día.
    * La copia de seguridad de un día específico se etiqueta y se retiene según la duración de retención semanal y la configuración.
    * La duración de retención mensual y anual se comporta de forma similar.

4. En el menú **Directiva de copia de seguridad completa**, haga clic en **Aceptar** para aceptar la configuración.
5. Para agregar una directiva de copia de seguridad diferencial, seleccione **Copia de seguridad diferencial**.
6. En **Differential Backup policy** (Directiva de copia de seguridad diferencial), seleccione **Enable** (Habilitar) para abrir los controles de retención y frecuencia.
    * A lo sumo, puede desencadenar una copia de seguridad diferencial al día.
    * Como máximo, las copias de seguridad diferenciales se pueden retener durante 180 días. Si necesita más tiempo de retención, debe usar copias de seguridad completas.

    ![Directiva de copia de seguridad diferencial](./media/backup-azure-sap-hana-database/differential-backup-policy.png)

    > [!NOTE]
    > Las copias de seguridad incrementales no se admiten en la actualidad.

7. Haga clic en **Aceptar** para guardar la directiva y volver al menú principal **Directiva de copia de seguridad**.
8. Para agregar una directiva de copia de seguridad del registro de transacciones, seleccione **Copia de seguridad de registros**.
    * En **Copia de seguridad de registros**, seleccione **Habilitar**.  No se puede deshabilitar, ya que SAP HANA administra todas las copias de seguridad de registros.
    * Establezca los controles de frecuencia y retención.

    > [!NOTE]
    > Las copias de seguridad de registros comienzan el flujo solo después de que se haya completado correctamente una copia de seguridad completa.

9. Haga clic en **Aceptar** para guardar la directiva y volver al menú principal **Directiva de copia de seguridad**.
10. Cuando haya terminado de definir la directiva de copia de seguridad, haga clic en **Aceptar**.

> [!NOTE]
> Cada copia de seguridad de registros está encadenada a la copia de seguridad completa anterior para formar una cadena de recuperación. Esta copia de seguridad completa se conservará hasta que expire la retención de la última copia de seguridad de registros. Esto puede significar que la copia de seguridad completa se conservará durante un período adicional para asegurarse de que se pueden recuperar todos los registros. Supongamos que el usuario tiene una copia de seguridad completa semanal, una copia diferencial diaria y registros de 2 horas. Todos ellos se conservan 30 días. Sin embargo, la copia completa semanal puede limpiarse o eliminarse en realidad solo después de que esté disponible la siguiente copia de seguridad completa, es decir, después de 30+7 días. Por ejemplo, una copia de seguridad completa semanal se produce el 16 de noviembre. Según la directiva de retención, debe conservarse hasta el 16 de diciembre. La última copia de seguridad de registros de esta copia completa se produce antes de la siguiente copia completa programada, el 22 de noviembre. Hasta que este registro esté disponible el 22 de diciembre, no se puede eliminar la copia completa del 16 de noviembre. Por lo tanto, la copia completa del 16 de noviembre se conserva hasta el 22 de diciembre.

## <a name="run-an-on-demand-backup"></a>Ejecución de una copia de seguridad a petición

Las copias de seguridad se ejecutan según la programación de la directiva. Puede ejecutar una copia de seguridad a petición siguiendo estos pasos:

1. En el menú Almacén, haga clic en **Elementos de copia de seguridad**.
2. En **Elementos de copia de seguridad**, seleccione la máquina virtual que ejecuta la base de datos de SAP HANA y, a continuación, haga clic en **Hacer copia de seguridad ahora**.
3. En **Realizar copia de seguridad ahora**, use el control del calendario para seleccionar el último día que debería retenerse el punto de recuperación. A continuación, haga clic en **Aceptar**.
4. Supervise las notificaciones del portal. Puede supervisar el progreso del trabajo en el panel del almacén > **Trabajos de copia de seguridad** > **En curso**. Según el tamaño de la base de datos, la creación de la copia de seguridad inicial puede tardar un tiempo.

## <a name="run-sap-hana-studio-backup-on-a-database-with-azure-backup-enabled"></a>Ejecutar una copia de seguridad de SAP HANA Studio en una base de datos con Azure Backup habilitado

Si desea hacer una copia de seguridad local (mediante HANA Studio) de una base de datos de la que se está haciendo una copia de seguridad con Azure Backup, haga lo siguiente:

1. Espere a que finalicen las copias de seguridad completas o de registro de la base de datos. Compruebe el estado en SAP HANA Studio o Cockpit.
2. Inhabilite las copias de seguridad de registros y establezca el catálogo de copias de seguridad al sistema de archivos de la base de datos pertinente.
3. Para ello, haga doble clic en **systemdb** > **Configuración** > **Seleccionar base de datos** > **Filtro (registro)** .
4. Establezca **enable_auto_log_backup** en **No**.
5. Establezca **log_backup_using_backint** en **False**.
6. A continuación, haga una copia de seguridad completa a petición de la base de datos.
7. Espere a que finalicen la copia de seguridad completa y la copia de seguridad de catálogos.
8. Revierta la configuración anterior a las opciones de Azure:
    * Establezca **enable_auto_log_backup** en **Sí**.
    * Establezca **log_backup_using_backint** en **True**.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [restaurar bases de datos de SAP HANA que se ejecutan en máquinas virtuales de Azure](https://docs.microsoft.com/azure/backup/sap-hana-db-restore).
* Obtenga información acerca de cómo [administrar bases de datos de SAP HANA de las que se realizan copias de seguridad mediante Azure Backup](https://docs.microsoft.com/azure/backup/sap-hana-db-manage).
