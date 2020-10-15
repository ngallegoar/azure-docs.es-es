---
title: 'Tutorial: Creación de una puerta de enlace de NAT: Azure Portal'
titlesuffix: Azure Virtual Network NAT
description: En este inicio rápido se muestra cómo crear una puerta de enlace de NAT mediante Azure Portal.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
Customer intent: I want to create a NAT gateway for outbound connectivity for my virtual network.
ms.service: virtual-network
ms.subservice: nat
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2020
ms.author: allensu
ms.openlocfilehash: 5010dc08b695a0376ace5dde935c63caf0c39633
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84711415"
---
# <a name="tutorial-create-a-nat-gateway-using-the-azure-portal"></a>Tutorial: Creación de una puerta de enlace de NAT mediante Azure Portal

En este tutorial se muestra cómo usar el servicio Azure Virtual Network NAT. Creará una puerta de enlace de NAT para proporcionar conectividad saliente para una máquina virtual en Azure. 

Si lo prefiere, puede seguir estos pasos con la [CLI de Azure](quickstart-create-nat-gateway-cli.md), [Azure PowerShell](quickstart-create-nat-gateway-powershell.md) o implemente una [plantilla ARM](quickstart-create-nat-gateway-powershell.md) en lugar del portal.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="virtual-network-and-parameters"></a>Red virtual y parámetros

Antes de implementar una máquina virtual y usar la puerta de enlace de NAT, es necesario crear el grupo de recursos y la red virtual.

En los pasos de esta sección, tendrá que reemplazar los siguientes parámetros por la siguiente información:

| Parámetro                   | Value                |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroupNAT |
| **\<virtual-network-name>** | myVNet          |
| **\<region-name>**          | Este de EE. UU. 2      |
| **\<IPv4-address-space>**   | 192.168.0.0\16          |
| **\<subnet-name>**          | mySubnet        |
| **\<subnet-address-range>** | 192.168.0.0\24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

## <a name="create-a-vm-to-use-the-nat-gateway"></a>Creación de una máquina virtual para usar la puerta de enlace de NAT

Ahora se va a crear una máquina virtual para usar el servicio NAT. Esta máquina virtual tiene una dirección IP pública en el nivel de instancia que le permite el acceso a la máquina virtual. El servicio NAT tiene en cuenta la dirección del flujo y reemplazará el destino predeterminado de Internet en la subred. La dirección IP pública de la máquina virtual no se usará para las conexiones salientes.

1. En la parte superior izquierda del portal, seleccione **Crear un recurso** > **Proceso** > **Ubuntu Server 18.04 LTS**, o busque **Ubuntu Server 18.04 LTS** en el cuadro de búsqueda de Marketplace.

2. En **Crear una máquina virtual**, escriba o seleccione los valores siguientes en la pestaña **Básico**:
   - **Suscripción** > **Grupo de recursos**: Seleccione **myResourceGroupNAT**.
   - **Detalles de instancia** > **Nombre de máquina virtual**: Escriba **myVM**.
   - **Detalles de instancia** > **Región** > seleccione **Este de EE. UU. 2**.
   - **Cuenta de administrador** > **Tipo de autenticación**: Seleccione **Contraseña**.
   - **Cuenta de administrador**> escriba la información de **Nombre de usuario**, **Contraseña** y **Confirmar contraseña**.
   - **Reglas de puerto de entrada** > **Puertos de entrada públicos**: Seleccione **Permitir los puertos seleccionados**.
   - **Reglas de puerto de entrada** > **Seleccionar puertos de entrada**: Seleccione **SSH (22)** .
   - Seleccione la pestaña **Redes** o seleccione **Siguiente: Discos** y, después, **Siguiente: Redes**.

3. En la pestaña **Redes**, asegúrese de que está seleccionado lo siguiente:
   - **Red virtual**: **myVnet**
   - **Subred**: **mySubnet**
   - **Dirección IP pública** > seleccione **Crear nueva**.  En la ventana **Crear dirección IP pública**, escriba **myPublicIPVM** en el campo **Nombre** y elija **Estándar** para la **SKU**.  Haga clic en **OK**.
   - **Grupo de seguridad de red de NIC**: Seleccione **Básica**.
   - **Puertos de entrada públicos**: Seleccione **Permitir los puertos seleccionados**.
   - **Seleccionar puertos de entrada**: Confirme que **SSH** está seleccionado.

4. En la pestaña **Administración**, en **Supervisión**, establezca **Diagnósticos de arranque** en **Desactivado**.

5. Seleccione **Revisar + crear**. 

6. Revise la configuración y haga clic en **Crear**.

## <a name="create-the-nat-gateway"></a>Creación de la puerta de enlace de NAT

Puede usar uno o varios recursos de dirección IP pública, prefijos de dirección IP pública, o ambos. Se agregará un recurso de dirección IP pública, un prefijo de dirección IP pública y un recurso de puerta de enlace de NAT.

En esta sección se detalla cómo crear y configurar los componentes siguientes del servicio NAT mediante el recurso de puerta de enlace de NAT:
  - Un grupo de direcciones IP públicas y un prefijo de dirección IP pública que se usarán en los flujos salientes traducidos por el recurso de puerta de enlace de NAT.
  - Cambie el tiempo de espera de inactividad de 4 (el valor predeterminado) a 10 minutos.

### <a name="create-a-public-ip-address"></a>Crear una dirección IP pública

1. En la parte superior izquierda del portal, seleccione **Crear un recurso** > **Redes** > **Dirección IP pública**, o busque **Dirección IP pública** en el cuadro de búsqueda de Marketplace.

2. En **Crear dirección IP pública**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | Versión de la dirección IP | Seleccione **IPv4**.
    | SKU | Seleccione **Estándar**.
    | Nombre | Escriba **myPublicIP**. |
    | Suscripción | Seleccione su suscripción.|
    | Resource group | Seleccione **myResourceGroupNAT**. |
    | Location | Seleccione **Este de EE. UU. 2**.|

3. Deje el resto de valores predeterminados y seleccione **Crear**.

### <a name="create-a-public-ip-prefix"></a>Creación de un prefijo de dirección IP pública

1. En la parte superior izquierda del portal, seleccione **Crear un recurso** > **Redes** > **Prefijo de dirección IP pública**, o busque **Prefijo de dirección IP pública** en el cuadro de búsqueda de Marketplace. 

2. En **Crear un prefijo de dirección IP pública**, escriba o seleccione los valores siguientes en la pestaña **Aspectos básicos**:
   - **Suscripción** > **Grupo de recursos**: Selección de **myResourceGroupNAT**>
   - **Detalles de instancia** > **Nombre**: Escriba **myPublicIPprefix**.
   - **Detalles de instancia** > **Región**: Seleccione **Este de EE. UU. 2**.
   - **Detalles de instancia** > **Tamaño del prefijo**: seleccione **/31 (2 direcciones)** .

3. Deje el resto de valores predeterminados y seleccione **Revisar y crear**.

4. Revise la configuración y, a continuación, seleccione **Crear**.
   

### <a name="create-a-nat-gateway-resource"></a>Creación de un recurso de puerta de enlace de NAT

1. En la parte superior izquierda del portal, seleccione **Crear un recurso** > **Redes** > **puerta de enlace de NAT**, o busque **puerta de enlace de NAT** en el cuadro de búsqueda de Marketplace.

2. En **Crear puerta de enlace de traducción de direcciones de red (NAT)** , escriba o seleccione los valores siguientes en la pestaña **Aspectos básicos**:
   - **Suscripción** > **Grupo de recursos**: Seleccione **myResourceGroupNAT**.
   - **Detalles de instancia** > **Nombre de puerta de enlace de NAT**: Escriba **myNATgateway**.
   - **Detalles de instancia** > **Región**: Seleccione **Este de EE. UU. 2**.
   - **Detalles de instancia** > **Tiempo de espera de inactividad (minutos)** : escriba **10**.
   - Seleccione la pestaña **IP pública** o seleccione **Siguiente: IP pública**.

3. En la pestaña **IP pública**, escriba o seleccione los valores siguientes:
   - **Direcciones IP públicas**: seleccione **myPublicIP**.
   - **Prefijos de direcciones IP públicas**: seleccione **myPublicIPprefix**.
   - Seleccione la pestaña **Subred** o seleccione **Siguiente: Subred**.

4. En la pestaña **Subred**, escriba o seleccione los siguientes valores:
   - **Red virtual**: seleccione **myResourceGroupNAT** > **myVnet**.
   - **Nombre de subred**: active la casilla junto a **mySubnet**.

5. Seleccione **Revisar + crear**.

6. Revise la configuración y, a continuación, seleccione **Crear**.

## <a name="discover-the-ip-address-of-the-vm"></a>Detección de la dirección IP de la máquina virtual

1. En el lado izquierdo del portal, seleccione **Grupos de recursos**.
2. Seleccione **myResourceGroupNAT**.
3. Seleccione **myVM**.
4. En **Información general**, copie el valor de **Dirección IP pública** y péguelo en el Bloc de notas para que pueda usarlo para acceder a la máquina virtual.

>[!IMPORTANT]
>Copie la dirección IP pública y, luego, péguela en un bloc de notas para que pueda usarla para acceder a la máquina virtual.

## <a name="sign-in-to-vm"></a>Inicio de sesión en la máquina virtual

Abra [Azure Cloud Shell](https://shell.azure.com) en el explorador. Use la dirección IP recuperada en el paso anterior para conectarse a la máquina virtual mediante SSH.

```azurecli-interactive
ssh <username>@<ip-address-destination>
```

Ahora está listo para usar el servicio NAT.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine el grupo de recursos, la puerta de enlace de NAT y todos los recursos relacionados. Seleccione el grupo de recursos **myResourceGroupNAT** que contiene la puerta de enlace de NAT y, luego, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado una puerta de enlace de NAT y una máquina virtual para usarla. 

Revise las métricas de Azure Monitor para ver el funcionamiento del servicio NAT. Diagnostique problemas como el agotamiento de recursos de los puertos SNAT disponibles.  El agotamiento de recursos de los puertos SNAT se soluciona agregando recursos de dirección IP pública o recursos de prefijo de dirección IP pública adicionales (o ambos).


- Obtenga más información sobre [Azure Virtual Network NAT](./nat-overview.md).
- Más información sobre [recursos de puerta de enlace de NAT](./nat-gateway-resource.md).
- Inicio rápido para la implementación de [recursos de puerta de enlace de NAT con la CLI de Azure](./quickstart-create-nat-gateway-cli.md).
- Inicio rápido para la implementación de [recursos de puerta de enlace de NAT con PowerShell](./quickstart-create-nat-gateway-powershell.md).
- Inicio rápido para la implementación de [recursos de puerta de enlace de NAT con Azure Portal](./quickstart-create-nat-gateway-portal.md).
> [!div class="nextstepaction"]

