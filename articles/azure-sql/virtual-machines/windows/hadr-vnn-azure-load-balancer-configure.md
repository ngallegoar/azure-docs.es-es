---
title: Configuración del VNN con Azure Load Balancer para HADR
description: Aprenda a configurar un equilibrador de carga de Azure para enrutar el tráfico al nombre de red virtual (VNN) para el grupo de disponibilidad o la instancia de clúster de conmutación por error (FCI) con SQL Server en máquinas virtuales de Azure para lograr una alta disponibilidad y recuperación ante desastres (HADR).
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/02/2020
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 2f36e568603ded5a89f88cf11627a09a5a240fac
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91316994"
---
# <a name="configure-vnn-with-azure-load-balancer-sql-server-on-azure-vms"></a>Configuración del VNN con Azure Load Balancer (SQL Server en máquinas virtuales de Azure)
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

En máquinas virtuales de Azure, los clústeres usan un equilibrador de carga para mantener una dirección IP que es preciso que esté en un único nodo de clúster a la vez. En esta solución, el equilibrador de carga contiene la dirección IP del nombre de red virtual (VNN) que utiliza el recurso en clúster en Azure. 

En este artículo aprende a configurar un equilibrador de carga mediante el servicio Azure Load Balancer. El equilibrador de carga enrutará el tráfico al [cliente de escucha del grupo de disponibilidad (AG)](availability-group-overview.md) o a las [instancias de clúster de conmutación por error (FCI)](failover-cluster-instance-overview.md) con SQL Server en máquinas virtuales de Azure para lograr una alta disponibilidad y recuperación ante desastres (HADR). 



## <a name="prerequisites"></a>Requisitos previos

Antes de completar los pasos de este artículo, ya debe tener:

- Decidido que Azure Load Balancer es la [opción de conectividad adecuada para su solución HADR](hadr-cluster-best-practices.md#connectivity).
- Configurado el [cliente de escucha del grupo de disponibilidad](availability-group-overview.md) o las [instancias de clúster de conmutación por error](failover-cluster-instance-overview.md). 
- Instalada la versión más reciente de [PowerShell](/powershell/azure/install-az-ps?view=azps-4.2.0). 


## <a name="create-load-balancer"></a>Creación de un equilibrador de carga

Utilice [Azure Portal](https://portal.azure.com) para crear del equilibrador de carga:

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
   - **Dirección IP privada**: la dirección IP que asignó al recurso de red en clúster.

   En la imagen siguiente se muestra la interfaz de usuario **Crear un equilibrador de carga**:

   ![Configuración del equilibrador de carga](./media/failover-cluster-instance-premium-file-share-manually-configure/30-load-balancer-create.png)
   

## <a name="configure-backend-pool"></a>Configuración del grupo back-end

1. Vuelva al grupo de recursos de Azure que contiene las máquinas virtuales y busque el equilibrador de carga nuevo. Es posible que tenga que actualizar la vista en el grupo de recursos. Seleccione el equilibrador de carga.

1. Seleccione **Grupos de back-end** y, a continuación, seleccione **Agregar**.

1. Asocie el grupo de back-end con el conjunto de disponibilidad que contiene las máquinas virtuales.

1. En **Configuraciones IP de red de destino**, active **MÁQUINA VIRTUAL** y seleccione las máquinas virtuales que participarán como nodos de clúster. No olvide incluir todas las máquinas virtuales que hospedarán la FCI o el grupo de disponibilidad.

1. Seleccione **Aceptar** para crear el grupo de back-end.

## <a name="configure-health-probe"></a>Configuración de sondeo de estado

1. En el panel del equilibrador de carga, seleccione **Sondeos de estado**.

1. Seleccione **Agregar**.

1. En el panel **Agregar sondeo de estado**, <span id="probe"> </span>establezca los parámetros del sondeo de estado siguientes:

   - **Name**: nombre del sondeo de estado.
   - **Protocolo**: TCP.
   - **Puerto**: el puerto que creó en el firewall para el sondeo de estado al [preparar la máquina virtual](failover-cluster-instance-prepare-vm.md#uninstall-sql-server-1). En este artículo, el ejemplo usa el puerto TCP `59999`.
   - **Intervalo**: 5 segundos.
   - **Umbral incorrecto**: 2 errores consecutivos.

1. Seleccione **Aceptar**.

## <a name="set-load-balancing-rules"></a>Establecimiento de reglas de equilibrio de carga

1. En el panel del equilibrador de carga, seleccione **Reglas de equilibrio de carga**.

1. Seleccione **Agregar**.

1. Establezca los parámetros de la regla de equilibrio de carga:

   - **Name**: nombre de las reglas de equilibrio de carga.
   - **Dirección IP de front-end**: La dirección IP del recurso de red en clúster del cliente de escucha del grupo de disponibilidad o de la FCI de SQL Server.
   - **Puerto**: el puerto TCP de SQL Server. El puerto de la instancia predeterminado es 1433.
   - **Puerto back-end**: el mismo puerto que el valor **Puerto** cuando se habilita **IP flotante (Direct Server Return)** .
   - **Grupo de back-end**: el nombre del grupo de back-end que configuró anteriormente.
   - **Sondeo de mantenimiento**: el sondeo de estado que configuró anteriormente.
   - **Persistencia de la sesión**: Ninguno.
   - **Tiempo de espera de inactividad (minutos)** : 4.
   - **IP flotante (Direct Server Return)** : Habilitado.

1. Seleccione **Aceptar**.

## <a name="configure-cluster-probe"></a>Configuración del clúster de sondeo

Establezca el parámetro del puerto de sondeo de clúster en PowerShell.

Para establecer el parámetro del puerto de sondeo de clúster, actualice las variables del siguiente script con los valores del entorno. Quite los corchetes angulares (`<` y `>`) del script.

```powershell
$ClusterNetworkName = "<Cluster Network Name>"
$IPResourceName = "<SQL Server FCI / AG Listener IP Address Resource Name>" 
$ILBIP = "<n.n.n.n>" 
[int]$ProbePort = <nnnnn>

Import-Module FailoverClusters

Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
```

En la tabla siguiente se describen los valores que debe actualizar:


|**Valor**|**Descripción**|
|---------|---------|
|`Cluster Network Name`| el nombre del clúster de conmutación por error de Windows Server para la red. En **Administrador de clústeres de conmutación por error** > **Redes**, haga clic con el botón derecho en la red y después seleccione **Propiedades**. El valor correcto está debajo del campo **Nombre** en la pestaña **General**.|
|`SQL Server FCI/AG listener IP Address Resource Name`|El nombre del recurso para la dirección IP del cliente de escucha del grupo de disponibilidad o de la FCI de SQL Server. En **Administrador de clústeres de conmutación por error** > **Roles**, en el rol FCI de SQL Server, en **Nombre del servidor**, haga clic con el botón derecho en el recurso de la dirección IP y después seleccione **Propiedades**. El valor correcto está debajo del campo **Nombre** en la pestaña **General**.|
|`ILBIP`|La dirección IP del equilibrador de carga interno (ILB). Esta dirección se configura en Azure Portal como la dirección front-end de ILB. También es la dirección IP de FCI de SQL Server. Puede encontrarla en **Administrador de clústeres de conmutación por error** en la misma página de propiedades donde encuentra `<SQL Server FCI/AG listener IP Address Resource Name>`.|
|`nnnnn`|El puerto de sondeo configurado en el sondeo de estado del equilibrador de carga. Cualquier puerto TCP no utilizado es válido.|
|"SubnetMask"| Máscara de subred para el parámetro de clúster. Debe ser la dirección de difusión TCP IP: `255.255.255.255`.| 


Después de establecer el sondeo de clúster puede ver todos los parámetros del clúster en PowerShell. Ejecute este script:

```powershell
Get-ClusterResource $IPResourceName | Get-ClusterParameter
```


## <a name="test-failover"></a>Conmutación por error de prueba


Pruebe la conmutación por error del recurso en clúster para asegurarse de la funcionalidad del clúster. 

# <a name="failover-cluster-instance"></a>[Instancia de clúster de conmutación por error](#tab/fci)

Siga estos pasos.

1. Conéctese a uno de los nodos de clúster de SQL Server con RDP.
1. Abra el **Administrador de clústeres de conmutación por error**. Seleccione **Roles**. Observe qué nodo posee el rol de FCI de SQL Server.
1. Haga clic con el botón derecho en el rol de FCI de SQL Server. 
1. Seleccione **Mover** y, después, seleccione **Mejor nodo posible**.

El **Administrador de clústeres de conmutación por error** muestra el rol y sus recursos pierden su conexión. Después, los recursos se mueven y vuelven a conectarse en el otro nodo.



# <a name="ag-listener"></a>[Cliente de escucha del grupo de disponibilidad](#tab/ag)

Siga estos pasos.

1. Abra [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) y conéctese al cliente de escucha del grupo de disponibilidad. 

1. Expanda **Always On Availability Group** (Grupo de disponibilidad Always On) en el **Explorador de objetos**. 
1. Haga clic con el botón derecho en el grupo de disponibilidad y seleccione **Conmutación por error**. 
1. Siga las indicaciones del asistente para conmutar por error el grupo de disponibilidad a una réplica secundaria. 

La conmutación por error se realiza correctamente cuando las réplicas cambian de roles y se sincronizan. 

---

## <a name="test-connectivity"></a>Comprobación de la conectividad

Para probar la conectividad, inicie sesión en otra máquina virtual de la misma red virtual. Abra **SQL Server Management Studio** y conéctese al nombre de la FCI de SQL Server o al cliente de escucha del grupo de disponibilidad.

>[!NOTE]
>Si es necesario, puede [descargar SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms).



## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las características de HADR de SQL Server en Azure, consulte [Grupos de disponibilidad](availability-group-overview.md) e [Instancia de clúster de conmutación por error](failover-cluster-instance-overview.md). También puede consultar los [procedimientos recomendados](hadr-cluster-best-practices.md) para configurar el entorno para una alta disponibilidad y recuperación ante desastres. 



