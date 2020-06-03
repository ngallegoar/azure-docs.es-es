---
title: 'Instancia de clúster de conmutación por error (FCI) de SQL Server con recurso compartido de archivos premium: Azure Virtual Machines'
description: En este artículo se explica cómo crear una instancia de clúster de conmutación por error de SQL Server con un recurso compartido de archivos Premium en Azure Virtual Machines.
services: virtual-machines
documentationCenter: na
author: MashaMSFT
editor: monicar
tags: azure-service-management
ms.assetid: 9fc761b1-21ad-4d79-bebc-a2f094ec214d
ms.service: virtual-machines-sql
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/09/2019
ms.author: mathoma
ms.openlocfilehash: 60526dbeb3e221e6a2e4c6b900ff3a109d4cdf8f
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84032426"
---
# <a name="configure-a-sql-server-failover-cluster-instance-with-premium-file-share-on-azure-virtual-machines"></a>Configuración de una instancia de clúster de conmutación por error de SQL Server con un recurso compartido de archivos Premium en Azure Virtual Machines
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

En este artículo se explica cómo crear una instancia de clúster de conmutación por error de SQL Server en Azure Virtual Machines con un [recurso compartido de archivos Premium](../../../storage/files/storage-how-to-create-premium-fileshare.md).

Los recursos compartidos de archivos Premium son recursos compartidos de archivos de baja latencia constante con respaldo de SSD que son totalmente compatibles para utilizarlos con la instancia del clúster de conmutación por error en SQL Server 2012 y versiones posteriores en Windows Server 2012 y versiones posteriores. Los recursos compartidos de archivos Premium ofrecen mayor flexibilidad, lo que le permite cambiar el tamaño y escalar el recurso compartido de archivos sin tiempo de inactividad.


## <a name="before-you-begin"></a>Antes de empezar

Hay algunas cosas que debe conocer y tener en cuenta antes de empezar.

Debe estar familiarizado con el funcionamiento de estas tecnologías:

- [Tecnologías de clúster de Windows](/windows-server/failover-clustering/failover-clustering-overview)
- [Instancias del clúster de conmutación por error de SQL Server](/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server)

Hay que tener en cuenta, en un clúster de conmutación por error de VM de IaaS de Azure, se recomienda una sola NIC por servidor (nodo de clúster) y una sola subred. La red de Azure tiene redundancia física, que hace que las NIC y subredes adicionales sean innecesarias en un clúster invitado de VM de IaaS de Azure. El informe de validación de clúster le avisará de que solo se puede tener acceso a los nodos en una sola red. Puede omitir esta advertencia en los clústeres de conmutación por error de máquinas virtuales de IaaS de Azure.

También debe tener conocimientos generales de estas tecnologías:

- [Recurso compartido de archivos premium de Azure](../../../storage/files/storage-how-to-create-premium-fileshare.md)
- [Grupos de recursos de Azure](../../../azure-resource-manager/management/manage-resource-groups-portal.md)

> [!IMPORTANT]
> Actualmente, las instancias del clúster de conmutación por error de SQL Server de las máquinas virtuales de Azure solo se admiten con el [modo de administración ligera](sql-vm-resource-provider-register.md#management-modes) de la [extensión del agente de IaaS de SQL Server](sql-server-iaas-agent-extension-automate-management.md). Para cambiar del modo de extensión total a extensión ligera, elimine el recurso **Máquina virtual SQL** de las máquinas virtuales correspondientes y regístrelas con el proveedor de recursos de VM con SQL en el modo ligero. Al eliminar el recurso **Máquina virtual SQL** desde Azure Portal, **desactive la casilla de la máquina virtual correcta**. La extensión completa admite características como la copia de seguridad automatizada, y la aplicación de revisiones y la administración avanzada del portal. Estas características no funcionarán para las máquinas virtuales de SQL si se vuelve a instalar el agente en modo de administración lightweight.

Los recursos compartidos de archivos Premium proporcionan funcionalidades de IOPS y rendimiento para responder a las necesidades de varias cargas de trabajo. Para cargas de trabajo con un uso intensivo de E/S, considere la [instancia de clúster de conmutación por error de SQL Server con Espacios de almacenamiento directo](failover-cluster-instance-storage-spaces-direct-manually-configure.md) basada en discos o ultradiscos Premium administrados.  

Compruebe la actividad de IOPS de su entorno y compruebe que los recursos compartidos de archivos Premium proporcionarán la capacidad de IOPS que necesita antes de iniciar una implementación o migración. Use los contadores de disco del Monitor de rendimiento de Windows para supervisar el total de IOPS (transferencias de disco/segundo) y el rendimiento (bytes de disco/segundo) necesarios para archivos de datos de SQL Server, registro y bases de datos temporales.

Muchas cargas de trabajo tienen ráfagas de E/S, por lo que es una buena idea realizar comprobaciones durante períodos de uso intensivo y anotar tanto el valor IOPS máximo como el valor IOPS medio. Los recursos compartidos de archivos Premium proporcionan IOPS en función del tamaño del recurso compartido. Los recursos compartidos de archivos Premium también proporcionan ráfagas gratuitas que le permiten aumentar la capacidad de E/S para triplicar la cantidad de base de referencia durante un máximo de una hora.

Para obtener más información sobre el rendimiento del recurso compartido de archivos premium, consulte [Niveles de rendimiento de un recurso compartido de archivos](https://docs.microsoft.com/azure/storage/files/storage-files-planning#storage-tiers).

### <a name="licensing-and-pricing"></a>Licencias y precios

En Azure Virtual Machines, puede obtener licencias de SQL Server mediante imágenes de VM de pago por uso (PAYG) o traiga su propia licencia (BYOL). El tipo de imagen que elija afecta a cómo se le cobra.

Con las licencias de pago por uso, una instancia de clúster de conmutación por error (FCI) de SQL Server en Azure Virtual Machines incurre en cargos para todos los nodos de FCI, incluidos los nodos pasivos. Para obtener más información, consulte [Precios de máquinas virtuales SQL Server Enterprise](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/).

Si cuenta con un Contrato Enterprise con Software Assurance puede utilizar un nodo FCI pasivo gratuito por cada nodo activo. Para aprovechar esta ventaja en Azure, use imágenes de VM BYOL y, use la misma licencia en los nodos activos y pasivos de FCI. Para obtener más información, consulte [Contrato Enterprise](https://www.microsoft.com/Licensing/licensing-programs/enterprise.aspx).

Para comparar las licencias de pago por uso y BYOL para SQL Server en Azure Virtual Machines, consulte [Introducción a máquinas virtuales con SQL](sql-server-on-azure-vm-iaas-what-is-overview.md#get-started-with-sql-vms).

Para obtener información completa acerca de las licencias de SQL Server, consulte los [precios](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

### <a name="filestream"></a>Secuencia de archivos

La secuencia de archivos no se admite en los clústeres de conmutación por error con un recurso compartido de archivos Premium. Para usar la secuencia de archivos, implemente el clúster con [Espacios de almacenamiento directo](failover-cluster-instance-storage-spaces-direct-manually-configure.md).

## <a name="prerequisites"></a>Requisitos previos

Antes de completar los pasos de este artículo, ya debe tener:

- Una suscripción de Microsoft Azure.
- Un dominio de Windows en máquinas virtuales de Azure.
- Una cuenta de usuario de dominio que tenga permisos para crear objetos en máquinas virtuales de Azure y en Active Directory.
- Una cuenta de usuario de dominio para ejecutar el servicio de SQL Server y que pueda iniciar sesión en la máquina virtual al montar el recurso compartido de archivos.  
- Una red virtual de Azure y una subred con espacio de direcciones IP suficiente para estos componentes:
   - Dos máquinas virtuales.
   - La dirección IP del clúster de conmutación por error.
   - Una dirección IP para cada FCI.
- DNS configurado en la red de Azure que señala a los controladores de dominio.
- Un [recurso compartido de archivos premium](../../../storage/files/storage-how-to-create-premium-fileshare.md) que se usará como unidad en clúster, en función de la cuota de almacenamiento de la base de datos de los archivos de datos.
- Si usa Windows Server 2012 R2 y versiones anteriores, necesitará otro recurso compartido de archivos para usarlo como testigo de recurso compartido de archivos, ya que se admiten testigos en la nube para Windows 2016 y versiones más recientes. Puede usar otro recurso compartido de archivos de Azure o puede usar un recurso compartido de archivos de una máquina virtual distinta. Si va a usar otro recurso compartido de archivos de Azure, puede montarlo con el mismo proceso que para el recurso compartido de archivos premium que se usa para la unidad en clúster. 

Una vez que cumpla los requisitos previos, puede comenzar la creación de un clúster de conmutación por error. El primer paso es crear las máquinas virtuales.

## <a name="step-1-create-the-virtual-machines"></a>Paso 1: Creación de las máquinas virtuales

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con su suscripción.

1. [Cree un conjunto de disponibilidad de Azure](../../../virtual-machines/linux/tutorial-availability-sets.md).

   El conjunto de disponibilidad agrupa las máquinas virtuales en dominios de error y dominios de actualización. Sirve para garantizar que la aplicación no resulta afectada por puntos únicos de error, como el conmutador de red o la fuente de alimentación de un conjunto de servidores.

   Si no ha creado el grupo de recursos para las máquinas virtuales, hágalo al crear un conjunto de disponibilidad de Azure. Si va a usar Azure Portal para crear el conjunto de disponibilidad, siga estos pasos:

   1. En Azure Portal, seleccione **Crear un recurso** para abrir Azure Marketplace. Busque **Conjunto de disponibilidad**.
   1. Seleccione **Conjunto de disponibilidad**.
   1. Seleccione **Crear**.
   1. En **Crear conjunto de disponibilidad**, proporcione estos valores:
      - **Name**: nombre del conjunto de disponibilidad.
      - **Suscripción**: Su suscripción de Azure.
      - **Grupo de recursos**: si quiere utilizar un grupo existente, haga clic en **Seleccionar existente** y, luego, seleccione el grupo de la lista. De lo contrario, seleccione **Crear nuevo** y escriba el nombre del grupo.
      - **Ubicación**: establezca la ubicación en la que planea crear las máquinas virtuales.
      - **Dominios de error**: use el valor predeterminado (**3**).
      - **Dominios de actualización**: use el valor predeterminado (**5**).
   1. Seleccione **Crear** para crear el conjunto de disponibilidad.

1. Cree las máquinas virtuales en el conjunto de disponibilidad.

   Aprovisionar dos máquinas virtuales de SQL Server en el conjunto de disponibilidad de Azure. Para conocer las instrucciones, consulte [Aprovisionamiento de una máquina virtual de SQL Server en Azure Portal](create-sql-vm-portal.md).

   Coloque ambas máquinas virtuales:

   - En el mismo grupo de recursos de Azure en el que se encuentra su conjunto de disponibilidad.
   - En la misma red que el controlador de dominio.
   - En una subred que tenga suficiente espacio de direcciones IP para ambas máquinas virtuales y todas las FCI que finalmente puede utilizar en este clúster.
   - En el conjunto de disponibilidad de Azure.

      >[!IMPORTANT]
      >Una vez creada una máquina virtual el conjunto de disponibilidad no se puede establecer o cambiar.

   Elija una imagen en Azure Marketplace. Puede usar una imagen de Azure Marketplace que incluya Windows Server y SQL Server, o use una que solo incluya Windows Server. Para más información, consulte [Información general de SQL Server en Azure Virtual Machines](sql-server-on-azure-vm-iaas-what-is-overview.md).

   Las imágenes oficiales de SQL Server de la galería de Azure incluyen una instancia de SQL Server instalada, el software de instalación de SQL Server y la clave necesaria.

   >[!IMPORTANT]
   > Después de crear la máquina virtual, quite la instancia de SQL Server independiente preinstalada. Usará usar los medios de SQL Server preinstalado para crear la FCI de SQL Server tras configurar al clúster de conmutación por error y el recurso compartido de archivos Premium como almacenamiento.

   Como alternativa, puede utilizar imágenes de Azure Marketplace que solo tienen el sistema operativo. Elija una imagen de **Windows Server 2016 Datacenter** e instale la FCI de SQL Server después de configurar el clúster de conmutación por error y el recurso compartido de archivos Premium como almacenamiento. Esta imagen no contiene los medios de instalación de SQL Server. Coloque los medios de instalación de SQL Server en una ubicación en la que pueda ejecutarlos para cada servidor.

1. Una vez que Azure cree las máquinas virtuales, conéctese a cada una de ellas con RDP.

   La primera vez que se conecte a una máquina virtual con RDP, un mensaje le pregunta si desea permitir que el equipo se pueda detectar en la red. Seleccione **Sí**.

1. Si utiliza una de las imágenes de máquina virtual basada en SQL Server, quite la instancia de SQL Server.

   1. En **Programas y características**, haga clic con el botón derecho en **Microsoft SQL Server 201_ (64 bits)** y seleccione **Desinstalar o cambiar**.
   1. Seleccione **Quitar**.
   1. Seleccione la instancia predeterminada.
   1. Quite todas las características en **Servicios de motor de base de datos**. No quite **Características compartidas**. Verá algo parecido a la siguiente captura de pantalla:

        ![Selección de características](./media/failover-cluster-instance-premium-file-share-manually-configure/03-remove-features.png)

   1. Seleccione **Siguiente** y después **Quitar**.

1. <span id="ports"> </span>Abra los puertos del firewall.  

   En cada máquina virtual, abra estos puertos de Firewall de Windows:

   | Propósito | Puerto TCP | Notas
   | ------ | ------ | ------
   | SQL Server | 1433 | Puerto normal para las instancias predeterminadas de SQL Server. Si usó una imagen de la galería, este puerto se abre automáticamente.
   | Sondeo de mantenimiento | 59999 | Cualquier puerto TCP abierto. En un paso posterior, configure el [sondeo de mantenimiento](#probe) del equilibrador de carga y el clúster para usar este puerto.
   | Recurso compartido de archivos | 445 | Puerto usado por el servicio del recurso compartido de archivos.

1. [Agregue las máquinas virtuales a un dominio preexistente](availability-group-manually-configure-prerequisites-tutorial.md#joinDomain).

Después de crear y configurar las máquinas virtuales, puede configurar el recurso compartido de archivos Premium.

## <a name="step-2-mount-the-premium-file-share"></a>Paso 2: Montaje del recurso compartido de archivos Premium

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a la cuenta de almacenamiento.
1. Vaya a **Recursos compartidos de archivos** en **Servicio Archivo** y seleccione el recurso compartido de archivos Premium que quiere usar para el almacenamiento de SQL.
1. Seleccione **Conectar** para mostrar la cadena de conexión del recurso compartido de archivos.
1. Seleccione la letra de la unidad que desea usar en la lista desplegable y, a continuación, copie ambos bloques de código en un bloc de notas.


   :::image type="content" source="media/manually-configure-failover-cluster-instance-premium-file-share/premium-file-storage-commands.png" alt-text="Copia de ambos comandos de PowerShell desde el portal de conexión del recurso compartido de archivos":::

1. Aplique el protocolo de escritorio remoto para conectarse a la VM con SQL Server con la cuenta que la FCI de SQL Server usará para la cuenta de servicio.
1. Abra una consola de comandos de PowerShell de administración.
1. Ejecute los comandos que guardó anteriormente cuando trabajaba en el portal.
1. Vaya hasta el recurso compartido con el explorador de archivos o el cuadro de diálogo **Ejecutar** (tecla Windows + r). Use la ruta de acceso a la red `\\storageaccountname.file.core.windows.net\filesharename`. Por ejemplo: `\\sqlvmstorageaccount.file.core.windows.net\sqlpremiumfileshare`

1. Cree al menos una carpeta en el recurso compartido de archivos recién conectado para colocar los archivos de datos de SQL.
1. Repita estos pasos en cada VM con SQL Server que participará en el clúster.

  > [!IMPORTANT]
  > - Considere la posibilidad de usar un recurso compartido de archivos independiente para los archivos de copia de seguridad a fin de ahorrar la capacidad de IOPS y espacio de este recurso compartido para usarla para el archivo de registro y de datos. Puede usar un recurso compartido de archivos Premium o Estándar para los archivos de copia de seguridad.
  > - Si tiene Windows 2012 R2 y versiones anteriores, siga estos mismos pasos para montar el recurso compartido de archivos que va a usar como testigo de recurso compartido de archivos. 

## <a name="step-3-configure-the-failover-cluster"></a>Paso 3: Configuración del clúster de conmutación por error

El siguiente paso es configurar el clúster de conmutación por error. En este paso, completará los siguientes pasos secundarios:

1. Adición de la característica de clústeres de conmutación por error de Windows Server.
1. Validación del clúster.
1. Creación del clúster de conmutación por error.
1. Cree el testigo en la nube (para Windows Server 2016 y versiones más recientes) o el testigo de recurso compartido de archivos (para Windows Server 2012 R2 y versiones anteriores).


### <a name="add-windows-server-failover-clustering"></a>Agregar Clústeres de conmutación por error de Windows Server

1. Conéctese a la primera máquina virtual con RDP mediante una cuenta de dominio que sea miembro de los administradores locales y tenga permisos para crear objetos en Active Directory. Utilice esta cuenta para el resto de la configuración.

1. [Agregue Clústeres de conmutación por error a todas las máquinas virtuales](availability-group-manually-configure-prerequisites-tutorial.md#add-failover-clustering-features-to-both-sql-server-vms).

   Para instalar la característica Clústeres de conmutación por error desde la interfaz de usuario, realice estos pasos en las dos máquinas virtuales:
   1. En el **Administrador del servidor**, seleccione **Administrar** y, a continuación, seleccione **Agregar roles y características**.
   1. En el **Asistente para agregar roles y características**, seleccione **Siguiente** hasta llegar a **Seleccionar características**.
   1. En **Seleccionar características**, seleccione **Clúster de conmutación por error**. Incluya todas las características y herramientas de administración requeridas. Seleccione **Agregar características**.
   1. Seleccione **Siguiente** y, después, **Finalizar** para instalar las características.

   Para instalar Clústeres de conmutación por error con PowerShell, ejecute el siguiente script en una sesión de PowerShell de administrador de una de las máquinas virtuales:

   ```powershell
   $nodes = ("<node1>","<node2>")
   Invoke-Command  $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools}
   ```

### <a name="validate-the-cluster"></a>Validación del clúster

Valide el clúster en la interfaz de usuario o con PowerShell.

Para validar el clúster con la interfaz de usuario, realice los pasos siguientes en una de las máquinas virtuales:

1. En **Administrador del servidor**, seleccione **Herramientas** y, después, seleccione **Administrador de clústeres de conmutación por error**.
1. En **Administrador de clústeres de conmutación por error**, seleccione **Acción**y, a continuación, seleccione **Validar configuración**.
1. Seleccione **Next** (Siguiente).
1. En **Seleccionar servidores o un clúster**, escriba el nombre de ambas máquinas virtuales.
1. En **Opciones de pruebas**, seleccione **Ejecutar solo las pruebas que seleccione**. Seleccione **Next** (Siguiente).
1. En **Selección de pruebas**, seleccione todas las pruebas excepto **Almacenamiento** y **Espacios de almacenamiento directo** como se muestra aquí:

   :::image type="content" source="media/manually-configure-failover-cluster-instance-premium-file-share/cluster-validation.png" alt-text="Seleccionar pruebas de validación de clústeres":::

1. Seleccione **Next** (Siguiente).
1. En **Confirmación**, seleccione **Siguiente**.

El **Asistente para validar una configuración** ejecuta las pruebas de validación.

Para validar el clúster con PowerShell, ejecute el siguiente script en una sesión de PowerShell de administrador de una de las máquinas virtuales:

   ```powershell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Inventory", "Network", "System Configuration"
   ```

Después de validar el clúster, cree el clúster de conmutación por error.

### <a name="create-the-failover-cluster"></a>Creación del clúster de conmutación por error

Para crear el clúster de conmutación por error, necesita:
- Los nombres de las máquinas virtuales que se convertirán en nodos del clúster.
- Un nombre para el clúster de conmutación por error
- Una dirección IP para el clúster de conmutación por error. Puede usar una dirección IP que no se utilice en la misma red virtual de Azure y subred como nodos del clúster.

#### <a name="windows-server-2012-through-windows-server-2016"></a>Windows Server 2012 a Windows Server 2016

El siguiente script de PowerShell crea un clúster de conmutación por error para Windows Server 2012 a través de Windows Server 2016. Actualice el script con los nombres de los nodos (los nombres de las máquinas virtuales) y una dirección IP disponible desde la red virtual de Azure.

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

#### <a name="windows-server-2019"></a>Windows Server 2019

El siguiente script de PowerShell crea un clúster de conmutación por error para Windows Server 2019. Para más información, consulte [Clúster de conmutación por error: objeto de red en clúster](https://blogs.windows.com/windowsexperience/2018/08/14/announcing-windows-server-2019-insider-preview-build-17733/#W0YAxO8BfwBRbkzG.97). Actualice el script con los nombres de los nodos (los nombres de las máquinas virtuales) y una dirección IP disponible desde la red virtual de Azure.

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage -ManagementPointNetworkType Singleton 
```


### <a name="create-a-cloud-witness-win-2016-"></a>Creación de un testigo en la nube (Win 2016+)

Si usa Windows Server 2016 y versiones posteriores, deberá crear un testigo en la nube. Testigo en la nube es un nuevo tipo de testigo de quórum de clúster que está almacenado en una instancia de Azure Storage Blob. Esto elimina la necesidad de una máquina virtual independiente que hospede un recurso compartido de testigo o que use un recurso compartido de archivos diferente.

1. [Cree un testigo en la nube para el clúster de conmutación por error](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).

1. Cree un contenedor de blobs.

1. Guarde las claves de acceso y la dirección URL del contenedor.

### <a name="configure-quorum"></a>Configuración de un cuórum 

En Windows Server 2016 y versiones posteriores, configure el clúster para usar el testigo en la nube que acaba de crear. Siga todos los pasos que se indican en [Configuración del testigo de cuórum en la interfaz de usuario](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness).

En Windows Server 2012 R2 y versiones anteriores, siga los mismos pasos que se indican en [Configuración del testigo de cuórum en la interfaz de usuario](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness) pero, en la página **Seleccionar testigo de quórum**, seleccione la opción **Configurar un testigo de recurso compartido de archivos**. Especifique el recurso compartido de archivos que asignó como testigo de recurso compartido de archivos, tanto si lo configuró en una máquina virtual independiente o lo montó desde Azure. 


## <a name="step-4-test-cluster-failover"></a>Paso 4: Conmutación por error del clúster de prueba

Pruebe la conmutación por error del clúster. En **Administrador de clústeres de conmutación por error**, haga clic con el botón derecho en el clúster y seleccione **Más acciones** > **Move Core Cluster Resource (Mover recurso del clúster del recurso principal)**  > **Seleccionar nodo**, y después, seleccione el otro nodo del clúster. Mueva el recurso de clúster principal a cada nodo del clúster y, después, devuélvalo al nodo principal. Si puede mover correctamente el clúster a cada nodo, está listo para instalar SQL Server.  

:::image type="content" source="media/manually-configure-failover-cluster-instance-premium-file-share/test-cluster-failover.png" alt-text="Prueba de la conmutación por error del clúster moviendo el recurso principal a los demás nodos":::

## <a name="step-5-create-the-sql-server-fci"></a>Paso 5: Crear la FCI de SQL Server

Después de haber configurado el clúster de conmutación por error, puede crear la FCI de SQL Server.

1. Conéctese a la primera máquina virtual con RDP.

1. En **Administrador de clústeres de conmutación por error**, asegúrese de que todos los recursos principales de clúster estén en la primera máquina virtual. Si es necesario, mueva todos los recursos a esta máquina virtual.

1. Localice los medios de instalación. Si la máquina virtual usa una de las imágenes de Azure Marketplace, los medios se encuentran en `C:\SQLServer_<version number>_Full`. Seleccione **Setup** (Configuración).

1. En el **Centro de instalación de SQL Server**, seleccione **Instalación**.

1. Seleccione **Nueva instalación de clúster de conmutación por error de SQL Server**. Siga las instrucciones del asistente para instalar la FCI de SQL Server.

   Es preciso que los directorios de datos de FCI estén en el recurso compartido de archivos premium. Escriba la ruta de acceso completa del recurso compartido,con este formato `\\storageaccountname.file.core.windows.net\filesharename\foldername`. Aparecerá una advertencia que le notificará que ha especificado un servidor de archivos como directorio de datos. Se espera esta advertencia. Para evitar posibles errores, asegúrese de que la cuenta de usuario con la que se conectó a la máquina virtual mediante RDP cuando conservó el recurso compartido de archivos sea la misma que usa el servicio SQL Server.

   :::image type="content" source="media/manually-configure-failover-cluster-instance-premium-file-share/use-file-share-as-data-directories.png" alt-text="Uso del recurso compartido de archivos como directorios de datos SQL":::

1. Después de completar los pasos del asistente, el programa de instalación instalará una FCI de SQL Server en el primer nodo.

1. Una vez que el programa de instalación instale la FCI en el primer nodo, conéctese al segundo nodo con RDP.

1. Abra **Centro de instalación de SQL Server**. Seleccione **Instalación**.

1. Seleccione **Agregar nodo a clúster de conmutación por error de SQL Server**. Siga las instrucciones del asistente para instalar el servidor de SQL Server y agregarlo a la FCI.

   >[!NOTE]
   >Si usó una imagen de la galería de Azure Marketplace con SQL Server, las herramientas de SQL Server estaban incluidas en la imagen. Si no usó alguna de estas imágenes, instale las herramientas de SQL Server por separado. Consulte [Descarga de SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="step-6-create-the-azure-load-balancer"></a>Paso 6: Creación de una instancia de Azure Load Balancer

En máquinas virtuales de Azure, los clústeres usan un equilibrador de carga para mantener una dirección IP que es preciso que esté en los nodos de clúster de uno en uno. En esta solución, el equilibrador de carga que retiene la dirección IP para la FCI de SQL Server.

Para más información, consulte [Creación y configuración de un equilibrador de carga de Azure](availability-group-manually-configure-tutorial.md#configure-internal-load-balancer).

### <a name="create-the-load-balancer-in-the-azure-portal"></a>Creación del equilibrador de carga en Azure Portal

Para crear el equilibrador de carga:

1. En Azure Portal, vaya al grupo de recursos que contiene las máquinas virtuales.

1. Seleccione **Agregar**. Busque **Equilibrador de carga** en Azure Marketplace. Seleccione **Equilibrador de carga**.

1. Seleccione **Crear**.

1. Configure el equilibrador de carga con los siguientes valores:

   - **Suscripción**: Su suscripción de Azure.
   - **Grupo de recursos**: El grupo de recursos que contiene las máquinas virtuales.
   - **Name**: un nombre que identifica el equilibrador de carga.
   - **Región**: La ubicación de Azure que contiene las máquinas virtuales.
   - **Tipo**: Pública o privada. A los equilibradores de carga privados se puede acceder desde la red virtual. La mayoría de las aplicaciones de Azure pueden usar un equilibrador de carga privado. Si la aplicación necesita acceder a SQL Server directamente a través de Internet, utilice un equilibrador de carga público.
   - **SKU**: Estándar.
   - **Red virtual**: la misma red que la de las máquinas virtuales.
   - **Asignación de dirección IP**: Estática. 
   - **Dirección IP privada**: la dirección IP que asignó al recurso de red de clúster de la FCI de SQL Server.

   En la imagen siguiente se muestra la interfaz de usuario **Crear un equilibrador de carga**:

   ![Configuración del equilibrador de carga](./media/failover-cluster-instance-premium-file-share-manually-configure/30-load-balancer-create.png)
   

### <a name="configure-the-load-balancer-backend-pool"></a>Configuración del grupo backend del equilibrador de carga

1. Vuelva al grupo de recursos de Azure que contiene las máquinas virtuales y busque el equilibrador de carga nuevo. Es posible que tenga que actualizar la vista en el grupo de recursos. Seleccione el equilibrador de carga.

1. Seleccione **Grupos de back-end** y, a continuación, seleccione **Agregar**.

1. Asocie el grupo de back-end con el conjunto de disponibilidad que contiene las máquinas virtuales.

1. En **Configuraciones IP de red de destino**, active **MÁQUINA VIRTUAL** y seleccione las máquinas virtuales que participarán como nodos de clúster. No olvide incluir todas las máquinas virtuales que hospedarán el FCI.

1. Seleccione **Aceptar** para crear el grupo de back-end.

### <a name="configure-a-load-balancer-health-probe"></a>Configuración de un sondeo de mantenimiento de un equilibrador de carga

1. En la hoja del equilibrador de carga, seleccione **Health probes** (Sondeos de mantenimiento).

1. Seleccione **Agregar**.

1. En la hoja **Add health probe** (Agregar sonda de mantenimiento), <span id="probe"> </span>establezca los parámetros del sondeo de mantenimiento siguientes.

   - **Name**: nombre del sondeo de estado.
   - **Protocolo**: TCP.
   - **Puerto**: el puerto que creó en el firewall para el sondeo de estado en [este paso](#ports). En este artículo, el ejemplo usa el puerto TCP `59999`.
   - **Intervalo**: 5 segundos.
   - **Umbral incorrecto**: 2 errores consecutivos.

1. Seleccione **Aceptar**.

### <a name="set-load-balancing-rules"></a>Establecimiento de reglas de equilibrio de carga

1. En la hoja del equilibrador de carga, seleccione **Reglas de equilibrio de carga**.

1. Seleccione **Agregar**.

1. Establezca los parámetros de la regla de equilibrio de carga:

   - **Name**: nombre de las reglas de equilibrio de carga.
   - **Dirección IP de front-end**: la dirección IP del recurso de red del clúster de la FCI de SQL Server.
   - **Puerto**: el puerto TCP de la FCI de SQL Server. El puerto de la instancia predeterminado es 1433.
   - **Puerto back-end**: utiliza el mismo puerto que el valor **Puerto** cuando se habilita **IP flotante (Direct Server Return)** .
   - **Grupo de back-end**: el nombre del grupo de back-end que configuró anteriormente.
   - **Sondeo de mantenimiento**: el sondeo de estado que configuró anteriormente.
   - **Persistencia de la sesión**: Ninguno.
   - **Tiempo de espera de inactividad (minutos)** : 4.
   - **IP flotante (Direct Server Return)** : Habilitado.

1. Seleccione **Aceptar**.

## <a name="step-7-configure-the-cluster-for-the-probe"></a>Paso 7: Configuración del clúster para el sondeo

Establezca el parámetro del puerto de sondeo de clúster en PowerShell.

Para establecer el parámetro del puerto de sondeo de clúster, actualice las variables del siguiente script con los valores del entorno. Quite los corchetes angulares (`<` y `>`) del script.

   ```powershell
   $ClusterNetworkName = "<Cluster Network Name>"
   $IPResourceName = "<SQL Server FCI IP Address Resource Name>" 
   $ILBIP = "<n.n.n.n>" 
   [int]$ProbePort = <nnnnn>

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

En la lista siguiente se describen los valores que debe actualizar:

   - `<Cluster Network Name>`: el nombre del clúster de conmutación por error de Windows Server para la red. En **Administrador de clústeres de conmutación por error** > **Redes**, haga clic con el botón derecho en la red y después seleccione **Propiedades**. El valor correcto está debajo del campo **Nombre** en la pestaña **General**.

   - `<SQL Server FCI IP Address Resource Name>`: nombre de recurso de la dirección IP de FCI de SQL Server. En **Administrador de clústeres de conmutación por error** > **Roles**, en el rol FCI de SQL Server, en **Nombre del servidor**, haga clic con el botón derecho en el recurso de la dirección IP y después seleccione **Propiedades**. El valor correcto está debajo del campo **Nombre** en la pestaña **General**.

   - `<ILBIP>`: dirección IP del ILB. Esta dirección se configura en Azure Portal como la dirección front-end de ILB. También es la dirección IP de FCI de SQL Server. Puede encontrarla en **Administrador de clústeres de conmutación por error** en la misma página de propiedades donde encuentra `<SQL Server FCI IP Address Resource Name>`.  

   - `<nnnnn>`: el puerto de sondeo configurado en el sondeo de estado del equilibrador de carga. Cualquier puerto TCP no utilizado es válido.

>[!IMPORTANT]
>La máscara de subred para los parámetros del clúster debe ser la dirección de difusión TCP/IP: `255.255.255.255`.

Después de establecer el sondeo de clúster puede ver todos los parámetros del clúster en PowerShell. Ejecute este script:

   ```powershell
   Get-ClusterResource $IPResourceName | Get-ClusterParameter
  ```

## <a name="step-8-test-fci-failover"></a>Paso 8: Probar la conmutación por error de la FCI

Probar la conmutación por error de la FCI para validar la funcionalidad del clúster. Siga estos pasos.

1. Conéctese a uno de los nodos de clúster de la FCI de SQL Server con RDP.

1. Abra el **Administrador de clústeres de conmutación por error**. Seleccione **Roles**. Observe qué nodo posee el rol de FCI de SQL Server.

1. Haga clic con el botón derecho en el rol de FCI de SQL Server.

1. Seleccione **Mover** y, después, seleccione **Mejor nodo posible**.

El **Administrador de clústeres de conmutación por error** muestra el rol y sus recursos pierden su conexión. Después, los recursos se mueven y vuelven a conectarse en el otro nodo.

### <a name="test-connectivity"></a>Comprobación de la conectividad

Para probar la conectividad, inicie sesión en otra máquina virtual de la misma red virtual. Abra **SQL Server Management Studio** y conéctese al nombre de la FCI de SQL Server.

>[!NOTE]
>Si es necesario, puede [descargar SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="limitations"></a>Limitaciones

Azure Virtual Machines admiten el Coordinador de transacciones distribuidas de Microsoft (MSDTC) en Windows Server 2019 con almacenamiento en volúmenes compartidos en clúster (CSV) y un [equilibrador de carga estándar](../../../load-balancer/load-balancer-standard-overview.md).

En máquinas virtuales de Azure, MSDTC no se admite en Windows Server 2016 y versiones anteriores porque:

- El recurso MSDTC en clúster no puede configurarse para usar almacenamiento compartido. En Windows Server 2016, si crea un recurso MSDTC, no mostrará ningún almacenamiento compartido disponible para su uso, incluso si el almacenamiento está disponible. Este problema se ha corregido en Windows Server 2019.
- El equilibrador de carga básico no controla los puertos RPC.

## <a name="see-also"></a>Consulte también

- [Tecnologías de clúster de Windows](/windows-server/failover-clustering/failover-clustering-overview)
- [Instancias del clúster de conmutación por error de SQL Server](/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server)
