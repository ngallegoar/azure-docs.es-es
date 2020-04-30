---
title: 'Configuración de direcciones IP privadas para máquinas virtuales: Azure Portal'
description: Obtenga información sobre cómo configurar direcciones IP privadas para máquinas virtuales mediante Azure Portal.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
tags: azure-resource-manager
ms.assetid: 11245645-357d-4358-9a14-dd78e367b494
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/07/2020
ms.author: kumud
ms.openlocfilehash: 946926a8a805ec3c53ea3c57dc3eded2462f7673
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81461555"
---
# <a name="configure-a-private-ip-address-for-a-vm-using-the-azure-portal"></a>Configuración de una dirección IP privada para una VM mediante Azure Portal

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

En los siguientes pasos de ejemplo se presupone que ya se ha creado un entorno simple. Si desea ejecutar los pasos que se indican en este documento, [cree primero una red virtual](quick-create-portal.md#create-a-virtual-network). Sin embargo, en el paso 3, use estos valores en su lugar:

| Configuración | Value |
| ------- | ----- |
| Nombre | *TestVNet* |
| Espacio de direcciones | *192.168.0.0/16* |
| Resource group | **TestRG** (si es necesario, seleccione **Crear nuevo** para crearlo) |
| Subred: nombre | *FrontEnd* |
| Subred: intervalo de direcciones | *192.168.1.0/24* |

## <a name="create-a-vm-for-testing-static-private-ip-addresses"></a>Creación de una VM para probar direcciones IP privadas estáticas
Si crea una VM en el modo de implementación de Resource Manager, no puede establecer una dirección IP privada estática mediante Azure Portal. En su lugar, primero debe crear la VM. A continuación, puede establecer la dirección IP privada como estática.

Para crear una VM denominada *DNS01* en la subred *FrontEnd* de una red virtual denominada *TestVNet*, siga estos pasos:

1. En el menú de [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso**.

    ![Crear un recurso, Azure Portal](./media/virtual-networks-static-ip-arm-pportal/create-a-resource.png)
2. Seleccione **Proceso** > **Máquina virtual**.

    ![Crear una VM, Azure Portal](./media/virtual-networks-static-ip-arm-pportal/compute-virtual-machine.png)
3. En **Aspectos básicos**, especifique los valores de los elementos como se describe en la tabla siguiente. A continuación, seleccione **Siguiente&nbsp;:&nbsp;Discos** y **Siguiente&nbsp;:&nbsp;Redes**.

    | Elemento | Value |
    | --- | --- |
    | **Suscripción** | Su suscripción actual |
    | **Grupos de recursos** | **TestRG** (seleccione la opción en la lista desplegable) |
    | **Nombre de la máquina virtual** | *DNS01* |
    | **Región** | **(EE. UU.) Este de EE. UU.** |
    | **Imagen** | **Windows Server 2019 Datacenter** |
    | **Tamaño** | **Tamaño de VM** **B1ls**, **Oferta** **Estándar** |
    | **Nombre de usuario** | El nombre de usuario de la cuenta de administrador |
    | **Contraseña** | La contraseña del nombre de usuario de la cuenta de administrador |
    | **Confirmar contraseña** | La contraseña otra vez |

    ![Pestaña Aspectos básicos, Creación de una máquina virtual, Azure Portal](./media/virtual-networks-static-ip-arm-pportal/create-a-virtual-machine-basics.png)
4. En **Redes**, especifique los valores de los elementos como se describe en la tabla siguiente y, a continuación, seleccione **Siguiente**.

    | Elemento | Value |
    | --- | --- |
    | **Red virtual** | **TestVNet** |
    | **Subred** | **FrontEnd** |

    ![Pestaña Redes, Creación de una máquina virtual, Azure Portal](./media/virtual-networks-static-ip-arm-pportal/create-a-virtual-machine-networking.png)
5. En **Administración**, en **Cuenta de almacenamiento de diagnóstico**, elija **vnetstorage**. Si esa cuenta de almacenamiento no aparece en la lista, seleccione **Crear nueva**, en **Nombre** especifique *vnetstorage* y seleccione **Aceptar**. Por último, seleccione **Revisar y crear**.

    ![Pestaña Administración, Creación de una máquina virtual, Azure Portal](./media/virtual-networks-static-ip-arm-pportal/create-a-virtual-machine-management.png)
6. En **Revisar y crear**, revise la información general y, después, seleccione **Crear**.

    ![Pestaña Revisar y crear, Creación de una máquina virtual, Azure Portal](./media/virtual-networks-static-ip-arm-pportal/create-a-virtual-machine-review-create.png)

Cuando se crea la VM, aparece el siguiente mensaje.

![Mensaje de finalización de la implementación, Creación de una máquina virtual, Azure Portal](./media/virtual-networks-static-ip-arm-pportal/deployment-is-complete.png)

## <a name="retrieve-private-ip-address-information-for-a-vm"></a>Recuperación de la información de la dirección IP privada de una VM
Para ver la información de la dirección IP privada de la nueva VM:

1. Vaya a [Azure Portal](https://portal.azure.com) para encontrar la VM. Busque y seleccione **Máquinas virtuales**.

    ![Máquinas virtuales, cuadro de búsqueda, Azure Portal](./media/virtual-networks-static-ip-arm-pportal/search-box-virtual-machines.png)

2. Seleccione el nombre de la nueva VM (**DNS01**).

    ![Lista de máquinas virtuales, Azure Portal](./media/virtual-networks-static-ip-arm-pportal/virtual-machine-list.png)

3. Elija **Redes** y seleccione la única interfaz de red que se muestra.

    ![Interfaz de red, redes, máquina virtual, Azure Portal](./media/virtual-networks-static-ip-arm-pportal/networking-network-interface.png)

4. Elija **Configuraciones de IP** y seleccione la configuración de IP que se muestra en la tabla.

    ![Configuración de IP, interfaz de red, redes, máquina virtual, Azure Portal](./media/virtual-networks-static-ip-arm-pportal/network-interface-ip-configurations.png)

5. En **Configuración de dirección IP privada**, en la red virtual o la subred **TestVNet/FrontEnd**, anote el valor de **Asignación** (**Dinámica** o **Estática**) y la **Dirección IP**.

    ![Asignación dinámica o estática, configuración de dirección IP privada antigua, configuración de IP, interfaz de red, redes, máquina virtual, Azure Portal](./media/virtual-networks-static-ip-arm-pportal/private-ip-address-settings-old.png)

## <a name="add-a-static-private-ip-address-to-an-existing-vm"></a>Adición de una dirección IP privada estática a una VM existente
Para agregar una dirección IP privada estática para la nueva VM:

1. En la página Configuración de IP, establezca la asignación de la dirección IP privada en **Estática**.
2. Cambie el valor de **Dirección IP** privada a *192.168.1.101* y, a continuación, seleccione **Guardar**.
   
    ![Asignación dinámica o estática, configuración de nueva dirección IP privada, configuración de IP, interfaz de red, redes, máquina virtual, Azure Portal](./media/virtual-networks-static-ip-arm-pportal/private-ip-address-settings-new.png)

> [!NOTE]
> Si después de seleccionar **Guardar** se da cuenta de que la asignación sigue establecida en **Dinámica**, significa que la dirección IP que ha escrito sigue estando en uso. Pruebe con otra dirección IP.

## <a name="remove-a-static-private-ip-address-from-a-vm"></a>Eliminación de una dirección IP privada estática de una VM
Para eliminar una dirección IP privada estática de la VM:

En la página Configuración de IP, establezca la asignación de la dirección IP privada en **Dinámica** y, a continuación, seleccione **Guardar**.

## <a name="set-ip-addresses-within-the-operating-system"></a>Configuración de direcciones IP en el sistema operativo

Desde dentro del sistema operativo de una VM, no debe asignar estáticamente la IP *privada* asignada a la VM de Azure. Realice la asignación estática de una dirección IP privada solo cuando sea necesario, por ejemplo al [asignar muchas direcciones IP a las VM](virtual-network-multiple-ip-addresses-portal.md). Si establece manualmente la dirección IP privada en el sistema operativo, asegúrese de que coincide con la dirección IP privada asignada a la [interfaz de red](virtual-network-network-interface-addresses.md#change-ip-address-settings) de Azure. De lo contrario, puede perder la conectividad a la VM. Más información sobre la configuración de la [dirección IP privada](virtual-network-network-interface-addresses.md#private).

Asimismo, nunca debe asignar manualmente la dirección IP *pública* asignada a una máquina virtual de Azure en el sistema operativo de la máquina virtual.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre la administración de la [configuración de la dirección IP privada](virtual-network-network-interface-addresses.md).
