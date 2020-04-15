---
title: Dispositivo con Azure Migrate
description: Proporciona información general sobre el dispositivo de Azure Migrate usado en la evaluación y migración del servidor.
ms.topic: conceptual
ms.date: 03/23/2020
ms.openlocfilehash: bccf4738d46b65f2d149eafc8e69591141d7d073
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437587"
---
# <a name="azure-migrate-appliance"></a>Dispositivo con Azure Migrate

En este artículo se resumen los requisitos previos y los requisitos de compatibilidad del dispositivo de Azure Migrate. 

## <a name="deployment-scenarios"></a>Escenarios de implementación

El dispositivo Azure Migrate se usa en los escenarios siguientes.

**Escenario** | **Herramienta** | **Se usa para** 
--- | --- | ---
**Valoración de máquinas virtuales de VMware** | Server Assessment de Azure Migrate | Detectar máquinas virtuales de VMware<br/><br/> Detectar aplicaciones y dependencias de la máquina<br/><br/> Recopilar metadatos de máquinas y metadatos de rendimiento para las evaluaciones
**Migración sin agentes de máquinas virtuales de VMware** | Server Migration de Azure Migrate | Detectar máquinas virtuales de VMware <br/><br/> Replicar máquinas virtuales de VMware con la migración sin agentes
**Valoración de máquinas virtuales de Hyper-V** | Server Assessment de Azure Migrate | Detectar máquinas virtuales de Hyper-V<br/><br/> Recopilar metadatos de máquinas y metadatos de rendimiento para las evaluaciones
**Valoración de máquina física** |  Server Assessment de Azure Migrate |  Descubra servidores físicos (o máquinas virtuales que se tratan como servidores físicos).<br/><br/> Recopilar metadatos de máquinas y metadatos de rendimiento para las evaluaciones

## <a name="appliance---vmware"></a>Dispositivo: VMware 

En la tabla siguiente se resumen los requisitos del dispositivo de Azure Migrate para VMware.

**Requisito** | **VMware** 
--- | ---
**Componentes del dispositivo** | El dispositivo tiene los componentes siguientes:<br/><br/> - **Aplicación de administración**: una aplicación web para la entrada del usuario durante la implementación del dispositivo. Se usa a la hora de evaluar las máquinas para su migración a Azure.<br/> - **Agente de detección**: un agente que reúne los datos de configuración de la máquina. Se usa a la hora de evaluar las máquinas para su migración a Azure.<br/>- **Agente de valoración**: un agente que recopila datos de rendimiento. Se usa a la hora de evaluar las máquinas para su migración a Azure.<br/>- **Servicio de actualización automática**: actualiza los componentes del dispositivo (se ejecuta cada 24 horas).<br/>- **Agente de recuperación de datos (DRA)** : organiza la replicación de VM y coordina la comunicación entre las máquinas replicadas y Azure. Solo se usa al replicar máquinas virtuales de VMware en Azure mediante la migración sin agentes.<br/>- **Puerta de enlace**: Envía los datos replicados a Azure. Solo se usa al replicar máquinas virtuales de VMware en Azure mediante la migración sin agentes.
**Implementación admitida** | Impleméntelo como máquina virtual de VMware mediante la plantilla de OVA.<br/><br/> Impleméntelo como máquina física o máquina virtual de VMware mediante el script de instalación de PowerShell.
**Compatibilidad con proyectos** |  Un dispositivo solo puede estar asociado a un proyecto. <br/> Se puede asociar cualquier número de dispositivos a un solo proyecto.<br/> 
**Límites de detección** | Un dispositivo puede detectar hasta 10 000 máquinas virtuales de VMware en una instancia de vCenter Server.<br/> Un dispositivo solo puede conectarse a una instancia de vCenter Server.
**Plantilla de OVA** | Descárguela del portal o en https://aka.ms/migrate/appliance/vmware.<br/><br/> El tamaño de la descarga es de 11,2 GB.<br/><br/> La plantilla del dispositivo descargada incluye una licencia de evaluación de Windows Server 2016 que es válida durante 180 días. Si el período de evaluación está a punto de expirar, se recomienda descargar e implementar un nuevo dispositivo, o bien activar la licencia del sistema operativo de la máquina virtual del dispositivo.
**Script de PowerShell** | [Descarga](https://go.microsoft.com/fwlink/?linkid=2105112) del script.<br/><br/> 
**Software/hardware** |  El dispositivo debe ejecutarse en una máquina con Windows Server 2016, 32 GB de RAM, 8 vCPU, alrededor de 80 GB de almacenamiento en disco y un conmutador virtual externo.<br/> El dispositivo requiere acceso a Internet, ya sea directamente o a través de un proxy.<br/><br/> Si ejecuta el dispositivo en una máquina virtual de VMware, necesitará recursos suficientes en vCenter Server para asignar una máquina virtual que cumpla los requisitos.<br/><br/> Si ejecuta el dispositivo en una máquina física, asegúrese de que está ejecutando Windows Server 2016 y que cumple los requisitos de hardware. 
**Requisitos de VMware** | Si el dispositivo se implementa como una máquina virtual de VMware, debe implementarse en un host ESXi que ejecute la versión 5.5 o una posterior.<br/><br/> vCenter Server que ejecute la versión 5.5, 6.0, 6.5 o 6.7.
**VDDK (migración sin agentes)** | Si el dispositivo se implementa como una máquina virtual de VMware y ejecuta una migración sin agentes, es necesario tener instalado el VDDK de VMware vSphere en la máquina virtual del dispositivo.
**Valor del código hash: OVA** | [Compruebe](tutorial-assess-vmware.md#verify-security) los valores hash de la plantilla de OVA.
**Valor del código hash: script de PowerShell** | [Compruebe](deploy-appliance-script.md#verify-file-security) los valores hash del script de PowerShell.




## <a name="appliance---hyper-v"></a>Dispositivo: Hyper-V

**Requisito** | **Hyper-V** 
--- | ---
**Componentes del dispositivo** | El dispositivo tiene los componentes siguientes:<br/><br/>- **Aplicación de administración**: una aplicación web para la entrada del usuario durante la implementación del dispositivo. Se usa a la hora de evaluar las máquinas para su migración a Azure.<br/> - **Agente de detección**: un agente que reúne los datos de configuración de la máquina. Se usa a la hora de evaluar las máquinas para su migración a Azure.<br/>- **Agente de valoración**: un agente que recopila datos de rendimiento. Se usa a la hora de evaluar las máquinas para su migración a Azure.<br/>- **Servicio de actualización automática**: actualiza los componentes del dispositivo (se ejecuta cada 24 horas).
**Implementación admitida** | Impleméntelo como máquina virtual de Hyper-V mediante una plantilla de VHD.<br/><br/> Impleméntelo como máquina física o máquina virtual de Hyper-V mediante el script de instalación de PowerShell.
**Compatibilidad con proyectos** |  Un dispositivo solo puede estar asociado a un proyecto. <br/> Se puede asociar cualquier número de dispositivos a un solo proyecto.<br/> 
**Límites de detección** | Un dispositivo puede detectar hasta 5000 VM de Hyper-V.<br/> Un dispositivo puede conectarse hasta a 300 hosts de Hyper-V.
**Plantilla de VHD** | Carpeta comprimida que incluye el VHD. Descárguela del portal o en https://aka.ms/migrate/appliance/hyperv.<br/><br/> El tamaño de la descarga es de 10 GB.<br/><br/> La plantilla del dispositivo descargada incluye una licencia de evaluación de Windows Server 2016 que es válida durante 180 días. Si el período de evaluación está a punto de expirar, se recomienda descargar e implementar un nuevo dispositivo, o bien activar la licencia del sistema operativo de la máquina virtual del dispositivo.
**Script de PowerShell** | [Descarga](https://go.microsoft.com/fwlink/?linkid=2105112) del script.<br/><br/> 
**Software/hardware***   |  El dispositivo debe ejecutarse en una máquina con Windows Server 2016, 32 GB de RAM, 8 vCPU, alrededor de 80 GB de almacenamiento en disco y un conmutador virtual externo.<br/> El dispositivo necesita una dirección IP estática o dinámica y requiere acceso a Internet, ya sea directamente o a través de un proxy.<br/><br/> Si se ejecuta el dispositivo como una máquina virtual de Hyper-V, se necesitan recursos suficientes en el host de Hyper-V para asignar 16 GB de RAM, 8 vCPU, alrededor de 80 GB de espacio de almacenamiento y un conmutador externo para la máquina virtual del dispositivo.<br/><br/> Si ejecuta el dispositivo en una máquina física, asegúrese de que está ejecutando Windows Server 2016 y que cumple los requisitos de hardware. 
**Requisitos de Hyper-V** | Si se implementa el dispositivo con la plantilla de VHD, la máquina virtual del dispositivo que proporciona Azure Migrate es la versión 5.0 de la máquina virtual de Hyper-V.<br/><br/> El host de Hyper-V debe ejecutarse en Windows Server 2012 R2 o posterior. 
**Valor del código hash: VHD** | [Compruebe](tutorial-assess-hyper-v.md#verify-security) los valores hash de la plantilla de VHD.
**Valor del código hash: script de PowerShell** | [Compruebe](deploy-appliance-script.md#verify-file-security) los valores hash del script de PowerShell.


## <a name="appliance---physical"></a>Dispositivo: físico

**Requisito** | **Físico** 
--- | ---
**Componentes del dispositivo** | El dispositivo tiene los componentes siguientes: <br/><br/> - **Aplicación de administración**: una aplicación web para la entrada del usuario durante la implementación del dispositivo. Se usa a la hora de evaluar las máquinas para su migración a Azure.<br/> - **Agente de detección**: un agente que reúne los datos de configuración de la máquina. Se usa a la hora de evaluar las máquinas para su migración a Azure.<br/>- **Agente de valoración**: un agente que recopila datos de rendimiento. Se usa a la hora de evaluar las máquinas para su migración a Azure.<br/>- **Servicio de actualización automática**: actualiza los componentes del dispositivo (se ejecuta cada 24 horas).
**Implementación admitida** | Impleméntelo como una máquina física dedicada o una máquina virtual mediante el script de instalación de PowerShell.
**Compatibilidad con proyectos** |  Un dispositivo solo puede estar asociado a un proyecto. <br/> Se puede asociar cualquier número de dispositivos a un solo proyecto.<br/> 
**Límites de detección** | Un dispositivo puede detectar hasta 250 servidores físicos.
**Script de PowerShell** | Descargue el script (AzureMigrateInstaller.ps1) del portal en una carpeta comprimida. [Más información](tutorial-assess-physical.md#set-up-the-appliance). Como alternativa, [descárguelo directamente](https://go.microsoft.com/fwlink/?linkid=2105112).<br/><br/> El tamaño de la descarga es de 59.7 MB.
**Software/hardware** |  El dispositivo debe ejecutarse en una máquina con Windows Server 2016, 32 GB de RAM, 8 vCPU, alrededor de 80 GB de almacenamiento en disco y un conmutador virtual externo.<br/> El dispositivo necesita una dirección IP estática o dinámica y requiere acceso a Internet, ya sea directamente o a través de un proxy.<br/><br/> Si ejecuta el dispositivo en una máquina física, asegúrese de que está ejecutando Windows Server 2016 y que cumple los requisitos de hardware. 
**Valor del código hash** | [Compruebe](deploy-appliance-script.md#verify-file-security) los valores hash del script de PowerShell.

## <a name="url-access"></a>acceso URL

El dispositivo de Azure Migrate necesita conectividad a Internet.

- Al implementar el dispositivo, Azure Migrate hace una comprobación de conectividad con las direcciones URL que se resumen en la tabla siguiente.
- Si usa un proxy basado en URL para conectarse a Internet, tiene que permitir el acceso a estas direcciones URL y asegurarse de que el proxy resuelve los registros CNAME recibidos al buscar estas direcciones.

**URL** | **Detalles**  
--- | --- |
*.portal.azure.com  | Acceda a Azure Portal.
\* .windows.net <br/> *.msftauth.net <br/> *.msauth.net <br/> *.microsoft.com <br/> *.live.com | Inicie sesión en la suscripción de Azure.
*.microsoftonline.com <br/> *.microsoftonline-p.com | Cree aplicaciones de Azure Active Directory (AD) para que el dispositivo se comunique con Azure Migrate.
management.azure.com | Cree aplicaciones de Azure AD para que el dispositivo se comunique con el servicio Azure Migrate.
dc.services.visualstudio.com | Cargue los registros de aplicaciones que se usan para la supervisión interna.
*.vault.azure.net | Administre secretos en Azure Key Vault.
aka.ms/* | Permiso de acceso a vínculos aka. Se usa para las actualizaciones del dispositivo de Azure Migrate.
download.microsoft.com/download | Permita descargas de Microsoft.
*.servicebus.windows.net | Comunicación entre el dispositivo y el servicio Azure Migrate.
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com | Conéctese a las direcciones URL del servicio Azure Migrate.
*.hypervrecoverymanager.windowsazure.com | **Se usa para la migración sin agentes de VMware.**<br/><br/> Conéctese a las direcciones URL del servicio Azure Migrate.
*.blob.core.windows.net |  **Se usa para la migración sin agentes de VMware.**<br/><br/>Cargue los datos al almacenamiento para la migración.




## <a name="collected-data---vmware"></a>Datos recopilados: VMware

El dispositivo recopila metadatos, datos de rendimiento y datos de análisis de dependencias (si se usa [análisis de dependencias](concepts-dependency-visualization.md) sin agente).

### <a name="metadata"></a>Metadatos

Los metadatos que descubre el dispositivo de Azure Migrate ayudan a averiguar si las máquinas y las aplicaciones están listas para la migración a Azure, el tamaño correcto de las máquinas y las aplicaciones, los planes de costos y el análisis de las dependencias de la aplicación. Microsoft no usa estos datos en ninguna auditoría de cumplimiento de licencias.

Esta es la lista completa de metadatos de VM de VMware que el dispositivo recopila y envía a Azure.

**DATOS** | **CONTADOR**
--- | --- 
**Detalles de la máquina** | 
Id. de VM | vm.Config.InstanceUuid 
Nombre de la máquina virtual | vm.Config.Name
Id. de vCenter Server | VMwareClient.Instance.Uuid
Descripción de la VM | vm.Summary.Config.Annotation
Nombre del producto de licencia | vm.Client.ServiceContent.About.LicenseProductName
Tipo de sistema operativo | vm.SummaryConfig.GuestFullName
Tipo de arranque | vm.Config.Firmware
Número de núcleos | vm.Config.Hardware.NumCPU
Memoria (MB) | vm.Config.Hardware.MemoryMB
Número de discos | vm.Config.Hardware.Device.ToList().FindAll(x => es VirtualDisk).count
Lista de tamaños de los discos | vm.Config.Hardware.Device.ToList().FindAll(x => es VirtualDisk)
Lista de adaptadores de red | vm.Config.Hardware.Device.ToList().FindAll(x => es VirtualEthernet).count
Uso de CPU | cpu.usage.average
Uso de memoria |mem.usage.average
**Detalles por disco** | 
Valor de clave del disco | disk.Key
Número de unidades de disco | disk.UnitNumber
Valor de clave de controladora de disco | disk.ControllerKey.Value
Gigabytes aprovisionados | virtualDisk.DeviceInfo.Summary
Nombre del disco | Valor generado mediante disk.UnitNumber, disk.Key y disk.ControllerKey.Value
Operaciones de lectura por segundo | virtualDisk.numberReadAveraged.average
Operaciones de escritura por segundo | virtualDisk.numberWriteAveraged.average
Rendimiento de lectura (MB por segundo) | virtualDisk.read.average
Rendimiento de escritura (MB por segundo) | virtualDisk.write.average
**Detalles por NIC** | 
Nombre del adaptador de red | nic.Key
Dirección MAC | ((VirtualEthernetCard)nic).MacAddress
Direcciones IPv4 | vm.Guest.Net
Direcciones IPv6 | vm.Guest.Net
Rendimiento de lectura (MB por segundo) | net.received.average
Rendimiento de escritura (MB por segundo) | net.transmitted.average
**Detalles de la ruta de acceso de inventario** | 
Nombre | container.GetType().Name
Tipo de objeto secundario | container.ChildType
Información de referencia | container.MoRef
Detalles del objeto primario | Container.Parent
Detalles de la carpeta por VM | ((Folder)container).ChildEntity.Type
Detalles del centro de datos por VM | ((Datacenter)container).VmFolder
Detalles del centro de datos por carpeta de host | ((Datacenter)container).HostFolder
Detalles del clúster por host | ((ClusterComputeResource)container).Host
Detalles del host por VM | ((HostSystem)container).VM

### <a name="performance-data"></a>Datos de rendimiento


Estos son los datos de rendimiento de las VM de VMware que el dispositivo recopila y envía a Azure.

**Data** | **Contador** | **Impacto en la evaluación**
--- | --- | ---
Uso de CPU | cpu.usage.average | Tamaño/costo de VM recomendado
Uso de memoria | mem.usage.average | Tamaño/costo de VM recomendado
Rendimiento de lectura de disco (MB por segundo) | virtualDisk.read.average | Cálculo del tamaño del disco, el costo de almacenamiento y el tamaño de VM
Rendimiento de escrituras de discos (MB por segundo) | virtualDisk.write.average | Cálculo del tamaño del disco, el costo de almacenamiento y el tamaño de VM
Operaciones de lectura de disco por segundo | virtualDisk.numberReadAveraged.average | Cálculo del tamaño del disco, el costo de almacenamiento y el tamaño de VM
Operaciones de escritura de discos por segundo | virtualDisk.numberWriteAveraged.average  | Cálculo del tamaño del disco, el costo de almacenamiento y el tamaño de VM
Rendimiento de lectura de NIC (MB por segundo) | net.received.average | Cálculo del tamaño de VM
Rendimiento de escrituras de adaptadores de red (MB por segundo) | net.transmitted.average  |Cálculo del tamaño de VM

### <a name="app-dependencies-metadata"></a>Metadatos de dependencias de la aplicación

El análisis de dependencias sin agente recopila datos de conexión y de proceso.

#### <a name="connection-data"></a>Datos de conexión

Estos son los datos de conexión que el dispositivo recopila de cada máquina virtual habilitada para el análisis de dependencias sin agente. Estos datos se envían a Azure.

**Data** | **Comando utilizado** 
--- | --- 
Puerto local | netstat
Dirección IP local | netstat
Puerto remoto | netstat
Dirección IP remota | netstat
Estado de conexión de TCP | netstat
Identificador del proceso | netstat
No. de conexiones activas | netstat

#### <a name="process-data"></a>Datos de proceso
Estos son los datos de proceso que el dispositivo recopila de cada máquina virtual habilitada para el análisis de dependencias sin agente. Estos datos se envían a Azure.

**Data** | **Clase WMI** | **Propiedad de clase WMI**
--- | --- | ---
Nombre del proceso | Win32_Process | ExecutablePath _s
Argumentos de procesos | Win32_Process | CommandLine
Nombre de la aplicación | Win32_Process | Parámetro VersionInfo.ProductName de la propiedad ExecutablePath

#### <a name="linux-vm-data"></a>Datos de la máquina virtual Linux

Estos son los datos de conexión y de proceso que el dispositivo recopila de cada máquina virtual Linux habilitada para el análisis de dependencias sin agente. Estos datos se envían a Azure.

**Data** | **Comando utilizado** 
--- | ---
Puerto local | netstat 
Dirección IP local | netstat 
Puerto remoto | netstat 
Dirección IP remota | netstat 
Estado de conexión de TCP | netstat 
No. de conexiones activas | netstat
Identificador del proceso  | netstat 
Nombre del proceso | ps
Argumentos de procesos | ps
Nombre de la aplicación | dpkg o rpm



## <a name="collected-data---hyper-v"></a>Datos recopilados: Hyper-V

El dispositivo recopila metadatos, datos de rendimiento y datos de análisis de dependencias (si se usa [análisis de dependencias](concepts-dependency-visualization.md) sin agente).

### <a name="metadata"></a>Metadatos
Los metadatos que descubre el dispositivo de Azure Migrate ayudan a averiguar si las máquinas y las aplicaciones están listas para la migración a Azure, el tamaño correcto de las máquinas y las aplicaciones, los planes de costos y el análisis de las dependencias de la aplicación. Microsoft no usa estos datos en ninguna auditoría de cumplimiento de licencias.

Esta es la lista completa de metadatos de VM de Hyper-V que el dispositivo recopila y envía a Azure.

**DATOS* | **CLASE WMI** | **PROPIEDAD DE CLASE WMI**
--- | --- | ---
**Detalles de la máquina** | 
Número de serie de BIOS _ Msvm_BIOSElement | BIOSSerialNumber
Tipo de VM (gen. 1 o 2) | Msvm_VirtualSystemSettingData | VirtualSystemSubType
Nombre para mostrar de VM | Msvm_VirtualSystemSettingData | ElementName
Versión de VM | Msvm_ProcessorSettingData | VirtualQuantity
Memoria (bytes) | Msvm_MemorySettingData | VirtualQuantity
Memoria máxima que se puede consumir por VM | Msvm_MemorySettingData | Límite
Memoria dinámica habilitada | Msvm_MemorySettingData | DynamicMemoryEnabled
Nombre/versión/FQDN del sistema operativo | Msvm_KvpExchangeComponent | Datos de nombre de GuestIntrinsicExchangeItems
Estado de energía de VM | Msvm_ComputerSystem | EnabledState
**Detalles por disco** | 
Identificador de disco | Msvm_VirtualHardDiskSettingData | VirtualDiskId
Tipo de disco duro virtual | Msvm_VirtualHardDiskSettingData | Tipo
Tamaño de disco duro virtual | Msvm_VirtualHardDiskSettingData | MaxInternalSize
Primario de disco duro virtual | Msvm_VirtualHardDiskSettingData | ParentPath
**Detalles por NIC** | 
Direcciones IP (NIC sintéticas) | Msvm_GuestNetworkAdapterConfiguration | IPAddresses
DHCP habilitado (NIC sintéticas) | Msvm_GuestNetworkAdapterConfiguration | DHCPEnabled
Id. de NIC (NIC sintéticas) | Msvm_SyntheticEthernetPortSettingData | InstanceID
Dirección MAC de NIC (NIC sintéticas) | Msvm_SyntheticEthernetPortSettingData | Dirección
Id. de NIC (NIC heredadas) | Datos de MsvmEmulatedEthernetPortSetting | InstanceID
Id. de MAC de NIC (NIC heredadas) | Datos de MsvmEmulatedEthernetPortSetting | Dirección

### <a name="performance-data"></a>Datos de rendimiento

Estos son los datos de rendimiento de las máquinas virtuales de Hyper-V que el dispositivo recopila y envía a Azure.

**Clase de contador de rendimiento** | **Contador** | **Impacto en la evaluación**
--- | --- | ---
Procesador virtual del hipervisor de Hyper-V | Tiempo de ejecución de invitado (%) | Tamaño/costo de VM recomendado
VM de Memoria dinámica de Hyper-V | Presión actual (%)<br/> Memoria física visible de invitado (MB) | Tamaño/costo de VM recomendado
Dispositivo de almacenamiento virtual de Hyper-V | Bytes de lectura/segundo | Cálculo del tamaño del disco, el costo de almacenamiento y el tamaño de VM
Dispositivo de almacenamiento virtual de Hyper-V | Bytes de escritura/segundo | Cálculo del tamaño del disco, el costo de almacenamiento y el tamaño de VM
Adaptador de red virtual de Hyper-V | Bytes recibidos/segundo | Cálculo del tamaño de VM
Adaptador de red virtual de Hyper-V | Bytes enviados/segundo | Cálculo del tamaño de VM

- El uso de CPU es la suma de todo el uso, para todos los procesadores virtuales conectados a una VM.
- El uso de memoria es (presión actual * memoria física visible del invitado)/100.
- Los valores de uso de disco y red se recopilan de los contadores de rendimiento de Hyper-V enumerados.

## <a name="appliance-upgrades"></a>Actualizaciones del dispositivo

El dispositivo se actualiza a medida que se actualizan los agentes de Azure Migrate que se ejecutan en el dispositivo. Esto sucede automáticamente porque la actualización automática está habilitada en el dispositivo de forma predeterminada. Puede cambiar esta configuración predeterminada para actualizar los agentes manualmente.

- **Desactivación de la actualización automática**: para desactivar la actualización automática en el registro, establezca la clave "AutoUpdate" que se encuentra en HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureAppliance en 0 (DWORD). Si se opta por usar las actualizaciones manuales, es importante actualizar todos los agentes del dispositivo al mismo tiempo, usando el botón **Actualizar** para cada agente no actualizado en el dispositivo.
- **Actualización manual**: en el caso de las actualizaciones manuales, asegúrese de actualizar todos los agentes del dispositivo al mismo tiempo mediante el botón **Actualizar** para cada agente no actualizado en el dispositivo. Puede volver a cambiar la configuración de actualización a actualizaciones automáticas en cualquier momento.

![Actualización automática del dispositivo](./media/migrate-appliance/autoupdate.png)

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga información](how-to-set-up-appliance-vmware.md) sobre cómo configurar el dispositivo para VMware.
- [Obtenga información](how-to-set-up-appliance-hyper-v.md) sobre cómo configurar el dispositivo para Hyper-V.
- [Obtenga información](how-to-set-up-appliance-physical.md) sobre cómo configurar el dispositivo para servidores físicos.

