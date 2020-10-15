---
title: 'Enrutamiento del tráfico de red (tutorial): Azure Portal'
titlesuffix: Azure Virtual Network
description: En este tutorial, aprenderá a enrutar el tráfico de red con una tabla de rutas mediante Azure Portal.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2020
ms.author: kumud
ms.openlocfilehash: d630a41f9b83a852605ffad2a85ad6dd14bbac73
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86079656"
---
# <a name="tutorial-route-network-traffic-with-a-route-table-using-the-azure-portal"></a>Tutorial: Enrutamiento del tráfico de red con una tabla de rutas mediante Azure Portal

De forma predeterminada, Azure enruta el tráfico entre todas las subredes de una red virtual. Sin embargo, puede crear sus propias rutas para invalidar las predeterminadas de Azure. Las rutas personalizadas resultan de utilidad si, por ejemplo, quiere enrutar el tráfico entre subredes por medio de una aplicación virtual de red (NVA). En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una aplicación virtual de red para enrutar el tráfico
> * Creación de una tabla de rutas
> * Creación de una ruta
> * Asociación de una tabla de rutas a una subred
> * Implementación de máquinas virtuales (VM) en subredes diferentes
> * Enrutamiento del tráfico desde una subred a otra a través de una aplicación virtual de red

Este tutorial usa [Azure Portal](https://portal.azure.com). También puede usar la [CLI de Azure](tutorial-create-route-table-cli.md) o [Azure PowerShell](tutorial-create-route-table-powershell.md).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-an-nva"></a>Creación de una aplicación virtual de red

Las aplicaciones virtuales de red (NVA) son máquinas virtuales que ayudan con las funciones de red, como la optimización del enrutamiento y del firewall. En este tutorial se supone que está utilizando **Windows Server 2016 Datacenter**. Si lo desea puede seleccionar un sistema operativo diferente.

1. En el menú [Azure Portal](https://portal.azure.com) o en la página **Inicio**, seleccione **Crear un recurso**.

1. Elija **Seguridad** > **Windows Server 2016 Datacenter**.

    ![Windows Server 2016 Datacenter, Creación de una máquina virtual, Azure Portal](./media/tutorial-create-route-table-portal/vm-ws2016-datacenter.png)

1. En la página **Creación de una máquina virtual**, en **Conceptos básicos**, escriba o seleccione esta información:

    | Sección | Configuración | Acción |
    | ------- | ------- | ----- |
    | **Detalles del proyecto** | Suscripción | Elija su suscripción. |
    | | Resource group | Seleccione **Crear nuevo**, escriba *myResourceGroup* y seleccione **Aceptar**. |
    | **Detalles de instancia** | Nombre de la máquina virtual | Escriba *myVmNva*. |
    | | Region | Elija **(EE. UU.) Este de EE. UU.** . |
    | | Opciones de disponibilidad | Elija **No se requiere redundancia de la infraestructura**. |
    | | Imagen | Elija **Windows Server 2016 Datacenter**. |
    | | Size | Deje el valor predeterminado **Estándar DS1 v2**. |
    | **Cuenta de administrador** | Nombre de usuario | Escriba un nombre de usuario de su elección. |
    | | Contraseña | Introduzca una contraseña de su elección, que debe tener al menos 12 caracteres y cumplir con los [requisitos de complejidad definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm). |
    | | Confirm Password | Vuelva a escribir la contraseña. |
    | **Reglas de puerto de entrada** | Puertos de entrada públicos | Elija **Ninguno**. |
    | **Ahorrar dinero** | ¿Ya tiene una licencia de Windows Server? | Elija **No**. |

    ![Aspectos básicos, Creación de una máquina virtual Azure Portal](./media/tutorial-create-route-table-portal/basics-create-virtual-machine.png)

    Después, seleccione **Siguiente: Discos >** .

1. En **Discos**, seleccione la configuración adecuada para sus necesidades y, después, seleccione **Siguiente: Redes >** .

1. En **Redes**:

    1. En **Red virtual**, seleccione **Crear nuevo**.
    
    1. En el cuadro de diálogo **Crear red virtual**, en **Nombre**, introduzca *myVirtualNetwork*.

    1. En **Espacio de direcciones**, reemplace el intervalo de direcciones existente por *10.0.0.0/16*.

    1. En **Subredes**, seleccione el icono **Eliminar** para eliminar la subred existente y, a continuación, especifique las siguientes combinaciones de **nombre de subred** e **intervalo de direcciones**. Cuando se especifica un nombre y un intervalo válidos, aparece una nueva fila vacía debajo de él.

        | Nombre de subred | Intervalo de direcciones |
        | ----------- | ------------- |
        | *Pública* | *10.0.0.0/24* |
        | *Privada* | *10.0.1.0/24* |
        | *DMZ* | *10.0.2.0/24* |

    1. Seleccione **Aceptar** para salir del cuadro de diálogo.

    1. En **Subred**, seleccione **DMZ (10.0.2.0/24)** .

    1. En **IP pública**, elija **Ninguno**, ya que esta máquina virtual no se conectará a través de Internet.

    1. Seleccione **Siguiente: Administración >** .

1. En **Administración**:

    1. En **Cuenta de almacenamiento de diagnóstico**, seleccione **Crear nuevo**.
    
    1. En el cuadro de diálogo **Crear cuenta de almacenamiento**, escriba o seleccione esta información:

        | Configuración | Value |
        | ------- | ----- |
        | Nombre | *mynvastorageaccount* |
        | Tipo de cuenta | **Storage (de uso general v1)** |
        | Rendimiento | **Estándar** |
        | Replicación | **Almacenamiento con redundancia local (LRS).** |
    
    1. Seleccione **Aceptar** para salir del cuadro de diálogo.

    1. Seleccione **Revisar + crear**. Se le remite a la página **Revisar y crear** y Azure valida la configuración.

1. Cuando reciba el mensaje **Validación superada**, seleccione **Crear**.

    La máquina virtual tarda en crearse unos minutos. Espere hasta que Azure termine de crear la máquina virtual. La página **La implementación está en curso** le mostrará los detalles de la implementación.

1. Cuando la máquina virtual esté lista, seleccione **Ir al recurso**.

## <a name="create-a-route-table"></a>Creación de una tabla de rutas

1. En el menú [Azure Portal](https://portal.azure.com) o en la página **Inicio**, seleccione **Crear un recurso**.

2. En el cuadro de búsqueda, escriba *Tabla de enrutamiento*. Cuando **Tabla de enrutamiento** aparezca en los resultados de la búsqueda, selecciónelo.

3. En la página **Tabla de enrutamiento**, seleccione **Crear**.

4. En **Crear tabla de rutas**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | Nombre | *myRouteTablePublic* |
    | Suscripción | Su suscripción |
    | Resource group | **myResourceGroup** |
    | Location | **(EE. UU.) Este de EE. UU.** |
    | Propagación de rutas de puerta de enlace de red virtual | **Enabled** |

    ![Crear tabla de rutas, Azure Portal](./media/tutorial-create-route-table-portal/create-route-table.png)

5. Seleccione **Crear**.

## <a name="create-a-route"></a>Creación de una ruta

1. Vaya a [Azure Portal](https://portal.azure.com) para administrar la tabla de rutas. Busque y seleccione **Tablas de rutas**.

1. Seleccione el nombre de la tabla de rutas (**myRouteTablePublic**).

1. Elija **Rutas** > **Agregar**.

    ![Agregar ruta, tabla de rutas, Azure Portal](./media/tutorial-create-route-table-portal/add-route.png)

1. En **Agregar ruta**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | Nombre de ruta | *ToPrivateSubnet* |
    | Prefijo de dirección | *10.0.1.0/24* (el intervalo de direcciones de la subred *Privada* creada anteriormente) |
    | Tipo de próximo salto | **Aplicación virtual** |
    | Siguiente dirección de salto | *10.0.2.4* (una dirección dentro del intervalo de direcciones de la subred *perimetral*) |

1. Seleccione **Aceptar**.

## <a name="associate-a-route-table-to-a-subnet"></a>Asociación de una tabla de rutas a una subred

1. Vaya a [Azure Portal](https://portal.azure.com) para administrar la red virtual. Busque y seleccione **Redes virtuales**.

1. Elija el nombre de la red virtual (**myVirtualNetwork**).

1. En la barra de menús de la red virtual, elija **Subredes**.

1. En la lista de subredes de la red virtual, elija **Público**.

1. En **Tabla de rutas**, elija la tabla de rutas que creó (**myRouteTablePublic**) y, a continuación, seleccione **Guardar** para asociar la tabla de rutas a la subred *Pública*.

    ![Asociar tabla de rutas, lista de subredes, red virtual, Azure Portal](./media/tutorial-create-route-table-portal/associate-route-table.png)

## <a name="turn-on-ip-forwarding"></a>Habilitación del reenvío IP

A continuación, active el reenvío IP para la nueva máquina virtual NVA, *myVmNva*. Cuando Azure envía tráfico de red a *myVmNva*, si el tráfico se destina a una dirección IP diferente, el reenvío IP envía el tráfico a la ubicación correcta.

1. Vaya a [Azure Portal](https://portal.azure.com) para administrar la máquina virtual. Busque y seleccione **Máquinas virtuales**.

1. Elija el nombre de la máquina virtual (**myVmNva**).

1. En la barra de menús de la máquina virtual NVA, seleccione **Redes**.

1. Seleccione **myvmnva123**. Esta es la interfaz de red que Azure creó para la máquina virtual. Azure agrega números para asegurar un nombre único.

    ![Redes, aplicación virtual de red (NVA), máquina virtual, Azure Portal](./media/tutorial-create-route-table-portal/virtual-machine-networking.png)

1. En la barra de menús de la interfaz de red, seleccione **Configuraciones de IP**.

1. En la página **Configuraciones de IP**, establezca **Reenvío IP**en **Habilitado** y seleccione **Guardar**.

    ![Habilitar reenvío IP, configuraciones de IP, interfaz de red, dispositivo virtual de red (NVA), máquina virtual, Azure Portal](./media/tutorial-create-route-table-portal/enable-ip-forwarding.png)

## <a name="create-public-and-private-virtual-machines"></a>Creación de máquinas virtuales públicas y privadas

Cree una máquina virtual pública y una máquina virtual privada en la red virtual. Más adelante, las usará para ver que Azure enruta el tráfico de subred *Público* a la subred *Privada* mediante la aplicación virtual de red.

Para crear la máquina virtual pública y la máquina virtual privada, siga los pasos de [Creación de una aplicación virtual de red](#create-an-nva) anteriormente. No es necesario esperar a que finalice la implementación o ir al recurso de la máquina virtual. Usará la mayoría de los valores, excepto como se describe a continuación.

Antes de seleccionar **Crear** para crear la máquina virtual pública o privada, vaya a las dos subsecciones siguientes ([Máquina virtual pública](#public-vm) y [Máquina virtual privada](#private-vm)), que muestran los valores que deben ser diferentes. Puede continuar con la siguiente sección ([Enrutamiento del tráfico desde una subred a otra a través de una aplicación virtual de red](#route-traffic-through-an-nva)) después de que Azure termine de implementar ambas máquinas virtuales.

### <a name="public-vm"></a>Máquina virtual pública

| Pestaña | Configuración | Value |
| --- | ------- | ----- |
| Aspectos básicos | Resource group | **myResourceGroup** |
| | Nombre de la máquina virtual | *myVmPublic* |
| | Puertos de entrada públicos | **Permitir los puertos seleccionados** |
| | Selección de puertos de entrada | **RDP** |
| Redes | Virtual network | **myVirtualNetwork** |
| | Subnet | **Público (10.0.0.0/24)** |
| | Dirección IP pública | El valor predeterminado |
| Administración | Cuenta de almacenamiento de diagnóstico | **mynvastorageaccount** |

### <a name="private-vm"></a>Máquina virtual privada

| Pestaña | Configuración | Value |
| --- | ------- | ----- |
| Aspectos básicos | Resource group | **myResourceGroup** |
| | Nombre de la máquina virtual | *myVmPrivate* |
| | Puertos de entrada públicos | **Permitir los puertos seleccionados** |
| | Selección de puertos de entrada | **RDP** |
| Redes | Virtual network | **myVirtualNetwork** |
| | Subnet | **Privado (10.0.1.0/24)** |
| | Dirección IP pública | El valor predeterminado |
| Administración | Cuenta de almacenamiento de diagnóstico | **mynvastorageaccount** |

## <a name="route-traffic-through-an-nva"></a>Enrutamiento del tráfico a través de una aplicación virtual de red

### <a name="sign-in-to-myvmprivate-over-remote-desktop"></a>Inicio de sesión en myVmPrivate a través de Escritorio remoto

1. Vaya a [Azure Portal](https://portal.azure.com) para administrar la máquina virtual privada. Busque y seleccione **Máquinas virtuales**.

1. Seleccione el nombre de la máquina virtual privada (**myVmPrivate**).

1. En la barra de menú de la máquina virtual, seleccione **Conectar** para crear una conexión al Escritorio remoto para la máquina virtual privada.

1. En la página **Conectar con RDP**, seleccione **Descargar archivo RDP**. Azure crea un archivo de Protocolo de Escritorio remoto ( *.rdp*) y lo descarga en su equipo.

1. Abra el archivo *.rdp* descargado. Cuando se le pida, seleccione **Conectar**. Seleccione **Más opciones** > **Usar otra cuenta** y, después, escriba el nombre de usuario y la contraseña que especificó al crear la máquina virtual privada.

1. Seleccione **Aceptar**.

1. Si recibe una advertencia de certificado durante el proceso de inicio de sesión, seleccione **Sí** para conectar la máquina virtual.

### <a name="enable-icmp-through-the-windows-firewall"></a>Habilite ICMP mediante el firewall de Windows

En un paso posterior, usará la herramienta trace route para probar el enrutamiento. Trace route usa el Protocolo de mensajes de control de Internet (ICMP), que el Firewall de Windows deniega de manera predeterminada. Habilite ICMP a través del Firewall de Windows.

1. En el escritorio remoto de *myVmPrivate*, abra PowerShell.

1. Escriba este comando:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

    Estará usando el comando trace route para probar el enrutamiento en este tutorial. Para entornos de producción, no se recomienda permitir ICMP a través del Firewall de Windows.

### <a name="turn-on-ip-forwarding-within-myvmnva"></a>Activación del reenvío IP en myVmNva

Ha [activado el reenvío IP](#turn-on-ip-forwarding) para la interfaz de red de la máquina virtual con Azure. El sistema operativo de la máquina virtual también tiene que reenviar el tráfico de red. Active el reenvío IP para el sistema operativo de máquina virtual *myVmNva* con estos comandos.

1. Desde un símbolo del sistema en la máquina virtual *myVmPrivate*, abra un Escritorio remoto en la máquina virtual *myVmNva*:

    ```cmd
    mstsc /v:myvmnva
    ```

1. En PowerShell, en la máquina virtual *myVmNva*, escriba este comando para activar el reenvío IP:

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```

1. Reinicie la máquina virtual *myVmNva*. En la barra de tareas, seleccione **Inicio** > **Encendido**, **Otros (planeados)**  > **Continuar**.

    Esto también desconecta la sesión de Escritorio remoto.

1. Una vez que se reinicie la máquina virtual *myVmNva*, cree una sesión de Escritorio remoto en la máquina virtual *myVmPublic*. Mientras sigue conectado a la máquina virtual *myVmPrivate*, abra un símbolo del sistema y ejecute este comando:

    ```cmd
    mstsc /v:myVmPublic
    ```
1. En el Escritorio remoto de *myVmPublic*, abra PowerShell.

1. Habilite ICMP mediante el Firewall de Windows con el comando siguiente:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

## <a name="test-the-routing-of-network-traffic"></a>Prueba del enrutamiento del tráfico de red

En primer lugar, vamos a probar el enrutamiento del tráfico de red desde la máquina virtual *myVmPublic* a la máquina virtual *myVmPrivate*.

1. En PowerShell, en la máquina virtual *myVmPublic*, escriba este comando:

    ```powershell
    tracert myVmPrivate
    ```

    La respuesta es similar a este ejemplo:

    ```powershell
    Tracing route to myVmPrivate.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
    over a maximum of 30 hops:

    1    <1 ms     *        1 ms  10.0.2.4
    2     1 ms     1 ms     1 ms  10.0.1.4

    Trace complete.
    ```

    Puede ver que el primer salto es 10.0.2.4, que es la dirección IP privada de la aplicación virtual de red. El segundo salto es a la dirección IP privada de la máquina virtual *myVmPrivate*: 10.0.1.4. Anteriormente, agregó la ruta a la tabla de rutas *myRouteTablePublic* y la asoció a la subred *Pública*. Como resultado, Azure envió el tráfico a través de la aplicación virtual de red y no directamente a la subred *Privada*.

1. Cierre la sesión de Escritorio remoto a la máquina virtual *myVmPublic*, que aún le deja conectado a la máquina virtual *myVmPrivate*.

1. Desde un símbolo del sistema de la máquina virtual *myVmPrivate*, escriba este comando:

    ```cmd
    tracert myVmPublic
    ```

    Este comando prueba el enrutamiento del tráfico de red desde la máquina virtual *myVmPrivate* a la máquina virtual *myVmPublic*. La respuesta es similar a este ejemplo:

    ```cmd
    Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
    over a maximum of 30 hops:

    1     1 ms     1 ms     1 ms  10.0.0.4

    Trace complete.
    ```

    Puede ver que Azure enruta el tráfico directamente desde la máquina virtual *myVmPrivate* a la máquina virtual *myVmPublic*. De forma predeterminada, Azure enruta el tráfico directamente entre subredes.

1. Cierre la sesión de Escritorio remoto a la máquina virtual *myVmPrivate*.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando el grupo de recursos ya no sea necesario, elimine *myResourceGroup* y todos los recursos que contiene:

1. Vaya a [Azure Portal](https://portal.azure.com) para administrar el grupo de recursos. Busque y seleccione **Grupos de recursos**.

1. Asigne un nombre al grupo de recursos (**myResourceGroup**).

1. Seleccione **Eliminar grupo de recursos**.

1. En el cuadro de diálogo de confirmación, escriba *myResourceGroup* en **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS** y, después, seleccione **Eliminar**. Azure elimina el grupo de recursos *myResourceGroup* y todos los recursos vinculados a ese grupo de recursos, incluidas las tablas de rutas, las cuentas de almacenamiento, las redes virtuales, las interfaces de red y las direcciones IP públicas.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha creado una tabla de rutas y la ha asociado a una subred. Creó una aplicación virtual de red sencilla que enrutó el tráfico desde una subred pública hasta una subred privada. Ahora puede implementar diferentes aplicaciones virtuales de red preconfiguradas desde [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking), que proporcionan muchas funciones de red útiles. Para más información acerca del enrutamiento, consulte [Introducción al enrutamiento](virtual-networks-udr-overview.md) y [Administración de una tabla de rutas](manage-route-table.md).

Aunque puede implementar muchos recursos de Azure en una red virtual, Azure no puede implementar recursos para algunos servicios de PaaS en una red virtual. Es posible restringir el acceso a los recursos de algunos servicios de PaaS de Azure, aunque la restricción solo debe ser el tráfico de una subred de red virtual. Para aprender a restringir el acceso de red a los recursos de PaaS de Azure, consulte el siguiente tutorial.

> [!div class="nextstepaction"]
> [Restringir el acceso de red a los recursos de PaaS](tutorial-restrict-network-access-to-resources.md)

> [!NOTE] 
> Los servicios de Azure cuestan dinero. Azure Cost Management le ayuda a establecer presupuestos y a configurar alertas para mantener el gasto bajo control. Analice, administre y optimice sus costos de Azure con Cost Management. Para obtener más información, consulte el [inicio rápido sobre el análisis de los costos](https://docs.microsoft.com/azure/cost-management-billing/costs/quick-acm-cost-analysis?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).