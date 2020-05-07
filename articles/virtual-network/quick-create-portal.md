---
title: 'Inicio rápido: Creación de una red virtual mediante Azure Portal'
titleSuffix: Azure Virtual Network
description: 'Inicio rápido: Cree una red virtual en Azure Portal. Estas redes permiten que los recursos de Azure, como máquinas virtuales, se comuniquen de forma segura entre ellos y con Internet.'
author: KumudD
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/05/2020
ms.author: kumud
ms.openlocfilehash: 1a1593566b8bdb72f322d64c1ee99c7018f49329
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "79214793"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-portal"></a>Inicio rápido: Creación de una red virtual mediante el Portal de Azure

En esta guía de inicio rápido aprenderá a crear una red virtual mediante Azure Portal. Va a implementar dos máquinas virtuales. Después, debe comunicarse de forma segura entre las máquinas virtuales y conectarse a las máquinas virtuales desde Internet. Una red virtual es el bloque de creación básico de una red privada en Azure. Permite que los recursos de Azure, como las máquinas virtuales, se comuniquen de manera segura entre sí y con Internet.

## <a name="prerequisites"></a>Prerrequisitos

* Una cuenta de Azure con una suscripción activa. [cree una de forma gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="create-a-virtual-network"></a>Creación de una red virtual

1. En el menú de Azure Portal, seleccione **Crear un recurso**. En Azure Marketplace, seleccione **Redes** > **Red virtual**.

1. En **Creación de una red virtual**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | Subscription | Seleccione su suscripción.|
    | Resource group | Seleccione **Crear nuevo**, escriba *myResourceGroup* y, después, seleccione **Aceptar**. |
    | Nombre | Escriba *myVirtualNetwork*. |
    | Location | Seleccione **Este de EE. UU**.|

1. Seleccione **Siguiente: Direcciones IP** y, para el **espacio de direcciones IPv4**, escriba *10.1.0.0/16*.

1. Seleccione **Agregar subred** y, a continuación, escriba *myVirtualSubnet* para el **nombre de subred** y *10.1.0.0/24*para**Intervalo de direcciones de subred**.

1. Seleccione **Agregar** y, después, **Revisar y crear**. Deje el resto tal como está y seleccione **Crear**.

1. En **Crear red virtual**, seleccione **Crear**.

## <a name="create-virtual-machines"></a>Creación de máquinas virtuales

Cree dos máquinas virtuales en la red virtual:

### <a name="create-the-first-vm"></a>Creación de la primera máquina virtual

1. En el menú de Azure Portal, seleccione **Crear un recurso**.

1. En Azure Marketplace, seleccione **Proceso** > **Windows Server 2019 Datacenter**. Seleccione **Crear**.

1. En **Creación de una máquina virtual: conceptos básicos**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | **Detalles del proyecto** | |
    | Subscription | Seleccione su suscripción. |
    | Resource group | Seleccione **myResourceGroup**. Ha creado este grupo de recursos en la sección anterior. |
    | **Detalles de instancia** |  |
    | Nombre de la máquina virtual | Escriba *myVm1*. |
    | Region | Seleccione **Este de EE. UU**. |
    | Opciones de disponibilidad | El valor predeterminado es **No se requiere redundancia de la infraestructura**. |
    | Imagen | El valor predeterminado es **Windows Server 2019 Datacenter**. |
    | Size | El valor predeterminado es **Estándar DS1 v2**. |
    | **Cuenta de administrador** |  |
    | Nombre de usuario | Escriba un nombre de usuario de su elección. |
    | Contraseña | Escriba una contraseña de su elección. La contraseña debe tener al menos 12 caracteres de largo y cumplir con los [requisitos de complejidad definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    | Confirm Password | Vuelva a escribir la contraseña. |
    | **Reglas de puerto de entrada** |  |
    | Puertos de entrada públicos | Seleccione **Permitir los puertos seleccionados**. |
    | Selección de puertos de entrada | Escriba *HTTP (80)* y *RDP (3389)* . |
    | **Ahorre dinero** |  |
    | ¿Ya tiene una licencia de Windows? | El valor predeterminado es **No**. |

1. Seleccione **Siguiente: Discos**.

1. En **Creación de una máquina virtual: Discos**, deje los valores predeterminados y seleccione **Siguiente: Redes**.

1. En **Creación de una máquina virtual: Redes**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | Virtual network | El valor predeterminado es **myVirtualNetwork**. |
    | Subnet | El valor predeterminado es **myVirtualSubnet (10.1.0.0/24)** . |
    | Dirección IP pública | El valor predeterminado es **(nuevo) myVm-IP**. |
    | Grupo de seguridad de red de NIC | El valor predeterminado es **Básico**. |
    | Puertos de entrada públicos | El valor predeterminado es **Permitir los puertos seleccionados**. |
    | Selección de puertos de entrada | El valor predeterminado es **HTTP** y **RDP**.

1. Seleccione **Siguiente: Administración**.

1. En **Creación de una máquina virtual: Administración**, para **Cuenta de almacenamiento de diagnóstico**, seleccione **Crear nuevo**.

1. En **Crear cuenta de almacenamiento**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | Nombre | Escriba *myvmstorageaccount*. Si el nombre ya existe, cree uno único.|
    | Tipo de cuenta | El valor predeterminado es **Storage (Uso general v1)** . |
    | Rendimiento | El valor predeterminado es **Estándar**. |
    | Replicación | El valor predeterminado es **Almacenamiento con redundancia local (LRS)** . |

1. Seleccione **Aceptar** y, después, **Revisar y crear**. Se le remitirá a la página **Revisar y crear**, donde Azure validará la configuración.

1. Cuando reciba el mensaje **Validación superada**, seleccione **Crear**.

### <a name="create-the-second-vm"></a>Creación de la segunda máquina virtual

Repita el procedimiento de la sección anterior para crear otra máquina virtual.

> [!IMPORTANT]
> En el **nombre de la máquina virtual**, escriba *myVm2*.
>
> En **Cuenta de almacenamiento de diagnóstico**, asegúrese de seleccionar **myvmstorageaccount**, en lugar de crear una.

## <a name="connect-to-a-vm-from-the-internet"></a>Conexión a una máquina virtual desde Internet

Después de crear *myVm1*, conéctese a Internet.

1. En Azure Portal, busque y seleccione *myVm1*.

1. Seleccione **Conectar** y, después, **RDP**.

    ![Conexión a una máquina virtual](./media/quick-create-portal/connect-to-virtual-machine.png)

    Se abre la página **Conectar**.

1. Seleccione **Descargar archivo RDP**. Azure crea un archivo de Protocolo de Escritorio remoto ( *.rdp*) y lo descarga en su equipo.

1. Abra el archivo RDP. Cuando se le pida, seleccione **Conectar**.

1. Escriba el nombre de usuario y la contraseña que especificó al crear la VM.

    > [!NOTE]
    > Es posible que tenga que seleccionar **Más opciones** > **Usar otra cuenta** para especificar las credenciales que escribió al crear la máquina virtual.

1. Seleccione **Aceptar**.

1. Puede que reciba una advertencia de certificado cuando inicie sesión. Si recibe una advertencia de certificado, seleccione **Sí** o **Continuar**.

1. Una vez que aparezca el escritorio de la máquina virtual, minimícelo para volver a su escritorio local.

## <a name="communicate-between-vms"></a>Comunicarse entre máquinas virtuales

1. En el Escritorio remoto de *myVm1*, abra PowerShell.

1. Escriba `ping myVm2`.

    Recibirá un mensaje similar a esta salida:

    ```output
    Pinging myVm2.0v0zze1s0uiedpvtxz5z0r0cxg.bx.internal.clouda
    Request timed out.
    Request timed out.
    Request timed out.
    Request timed out.

    Ping statistics for 10.1.0.5:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
    ```

    Se produce un error de `ping`, porque `ping` usa el Protocolo de mensajes de control de Internet (ICMP). De manera predeterminada, el protocolo ICMP no puede atravesar el Firewall de Windows.

1. Para permitir que *myVm2* haga ping a *myVm1* en un paso posterior, escriba este comando:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

    Ese comando permite una conexión entrante de ICMP a través del Firewall de Windows:

1. Cierre la conexión de Escritorio remoto a *myVm1*.

1. Vuelva a realizar los pasos de [Conexión a una máquina virtual desde Internet](#connect-to-a-vm-from-the-internet), pero conéctese a *myVm2*.

1. Desde un símbolo del sistema, escriba `ping myvm1`.

    Obtendrá un mensaje similar al siguiente:

    ```output
    Pinging myVm1.0v0zze1s0uiedpvtxz5z0r0cxg.bx.internal.cloudapp.net [10.1.0.4] with 32 bytes of data:
    Reply from 10.1.0.4: bytes=32 time=1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128

    Ping statistics for 10.1.0.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 1ms, Average = 0ms
    ```

    Recibe respuestas de *myVm1* porque permitió ICMP a través del Firewall de Windows en la VM *myVm1* en el paso 3.

1. Cierre la conexión de Escritorio remoto a *myVm2*.

## <a name="clean-up-resources"></a>Limpieza de recursos

En esta guía de inicio rápido, ha creado una red virtual predeterminada y dos máquinas virtuales. Se ha conectado a una VM desde Internet y se ha comunicado de forma segura entre las dos VM.

Cuando haya terminado de usar la red virtual y las VM, elimine el grupo de recursos y todos los recursos que contiene:

1. Busque y seleccione *myResourceGroup*.

1. Seleccione **Eliminar grupo de recursos**.

1. Escriba *myResourceGroup* para **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS** y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la configuración de red virtual, consulte [Crear, cambiar o eliminar una red virtual](manage-virtual-network.md).

De forma predeterminada, Azure permite una comunicación segura entre las máquinas virtuales. Azure solo permite conexiones de Escritorio remoto entrantes a las máquinas virtuales Windows desde Internet. Para más información sobre los tipos de comunicaciones de red de máquinas virtuales, consulte el artículo sobre el [filtrado del tráfico de red](tutorial-filter-network-traffic.md).
