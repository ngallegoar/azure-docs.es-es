---
title: Creación de una máquina virtual con una dirección IP pública estática (Azure Portal) | Microsoft Docs
description: Obtenga información sobre cómo crear una máquina virtual con una dirección IP pública estática mediante Azure Portal.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
ms.service: virtual-network
ms.subservice: ip-services
ms.devlang: ''
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/08/2018
ms.author: allensu
ms.openlocfilehash: 9e06e4079a5118e0aa9dedb1fca719f0b28e5716
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91448626"
---
# <a name="create-a-virtual-machine-with-a-static-public-ip-address-using-the-azure-portal"></a>Creación de una máquina virtual con una dirección IP pública estática mediante Azure Portal

Puede crear una máquina virtual con una dirección IP pública estática. Una dirección IP pública le permite comunicarse con una máquina virtual desde Internet. Asigne una dirección IP pública estática, en lugar de una dirección dinámica, para garantizar que la dirección no cambie nunca. Más información sobre [direcciones IP públicas estáticas](virtual-network-ip-addresses-overview-arm.md#allocation-method). Para cambiar una dirección IP pública asignada a una máquina virtual existente de dinámica a estática, o para trabajar con direcciones IP privadas, consulte [Incorporación, cambio o eliminación de direcciones IP](virtual-network-network-interface-addresses.md). Las direcciones IP públicas tienen un [costo nominal](https://azure.microsoft.com/pricing/details/ip-addresses) y hay un [límite](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) en el número de direcciones IP públicas que se pueden usar por suscripción.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="create-a-virtual-machine"></a>Creación de una máquina virtual

1. Seleccione **+ Crear un recurso** en la esquina superior izquierda de Azure Portal.
2. Seleccione **Proceso** y, luego, **Máquina virtual de Windows Server 2016** u otro sistema operativo de su elección.
3. Escriba o seleccione la siguiente información, acepte los valores predeterminados para el resto de la configuración y luego seleccione **Aceptar**:

    |Configuración|Value|
    |---|---|
    |Nombre|myVM|
    |Nombre de usuario| Escriba un nombre de usuario de su elección.|
    |Contraseña| Escriba una contraseña de su elección. La contraseña debe tener al menos 12 caracteres de largo y cumplir con los [requisitos de complejidad definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Subscription| Seleccione su suscripción.|
    |Resource group| Seleccione **Usar existente** y, a continuación, **myResourceGroup**.|
    |Location| Seleccione **Este de EE. UU**.|

4. Seleccione un tamaño para la máquina virtual y luego **Seleccionar**.
5. En **Configuración**, seleccione **Dirección IP pública**.
6. Escriba *myPublicIpAddress*, seleccione **Estática** y luego seleccione **Aceptar**, tal y como se muestra en la siguiente imagen:

   ![Selección de estática](./media/virtual-network-deploy-static-pip-arm-portal/select-static.png)

   Si la dirección IP pública debe ser una SKU estándar, seleccione **Estándar** en **SKU**. Más información sobre las [SKU de dirección IP pública](virtual-network-ip-addresses-overview-arm.md#sku). Si la máquina virtual se va a agregar al grupo back-end de una instancia pública de Azure Load Balancer, la SKU de la dirección IP pública de la máquina virtual debe coincidir con la SKU de la dirección IP del equilibrador de carga. Para más información, consulte [Azure Load Balancer](../load-balancer/skus.md).

6. Seleccione un puerto o ningún puerto en **Seleccionar puertos de entrada públicos**. Se selecciona el puerto 3389 para permitir el acceso remoto a la máquina virtual Windows Server desde Internet. No se recomienda abrir el puerto 3389 desde Internet con cargas de trabajo de producción.

   ![Selección de un puerto](./media/virtual-network-deploy-static-pip-arm-portal/select-port.png)

7. Acepte los valores predeterminados restantes y seleccione **Aceptar**.
8. En la página **Resumen**, seleccione **Crear**. La máquina virtual tarda minutos en crearse.
9. Una vez implementada la máquina virtual, escriba *myPublicIpAddress* en el cuadro de búsqueda de la parte superior del portal. Cuando **myPublicIpAddress** aparezca en los resultados de búsqueda, selecciónela.
10. Puede ver la dirección IP pública asignada, y que la dirección se asigna a la máquina virtual **myVM**, como se muestra en la siguiente imagen:

    ![Captura de pantalla que muestra el panel Dirección I P pública con la dirección I P y el nombre resaltados. ](./media/virtual-network-deploy-static-pip-arm-portal/public-ip-overview.png)

    Azure asigna una dirección IP pública de las direcciones usadas en la región en la que creó la máquina virtual. Puede descargar la lista de intervalos (prefijos) para las nubes de Azure [Pública](https://www.microsoft.com/download/details.aspx?id=56519), [Gobierno de Estados Unidos](https://www.microsoft.com/download/details.aspx?id=57063), [China](https://www.microsoft.com/download/details.aspx?id=57062) y [Alemania](https://www.microsoft.com/download/details.aspx?id=57064).

11. Seleccione **Configuración** para confirmar que la asignación es **Estática**.

    ![Captura de pantalla que muestra el panel Dirección I P pública con el elemento Configuración seleccionado.](./media/virtual-network-deploy-static-pip-arm-portal/public-ip-configuration.png)

> [!WARNING]
> No modifique la configuración de direcciones IP dentro del sistema operativo de la máquina virtual. El sistema operativo no conoce las direcciones IP públicas de Azure. Aunque puede agregar la configuración de dirección IP privada al sistema operativo, se recomienda no hacerlo a menos que sea necesario y no hasta después de haber leído [Incorporación o eliminación de direcciones IP privadas a un sistema operativo](virtual-network-network-interface-addresses.md#private).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no sea necesario, elimine el grupo de recursos y todos los recursos que contiene:

1. Escriba *myResourceGroup* en el cuadro **Buscar** que se encuentra en la parte superior del portal. Seleccione **myResourceGroup** cuando lo vea en los resultados de la búsqueda.
2. Seleccione **Eliminar grupo de recursos**.
3. Escriba *myResourceGroup* para **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS:** y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

- Más información acerca de [direcciones IP públicas](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) en Azure
- Más información acerca de toda la [configuración de dirección IP pública](virtual-network-public-ip-address.md#create-a-public-ip-address)
- Más información acerca de [direcciones IP privadas](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) y la asignación de una [dirección IP privada estática](virtual-network-network-interface-addresses.md#add-ip-addresses) a una máquina virtual de Azure
- Más información acerca de cómo crear máquinas virtuales [Linux](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) y [Windows](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)