---
title: Apertura de puertos en una VM desde Azure Portal
description: Aprenda a abrir un puerto o crear un punto de conexión a la máquina virtual Windows con el modelo de implementación de Resource Manager en Azure Portal
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
ms.assetid: f7cf0319-5ee7-435e-8f94-c484bf5ee6f1
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/27/2018
ms.author: cynthn
ms.openlocfilehash: 130d3315b5a9a6f175bd3d67ed33a034ab5f8dda
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75371418"
---
# <a name="how-to-open-ports-to-a-virtual-machine-with-the-azure-portal"></a>Apertura de puertos en una máquina virtual con Azure Portal
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]


## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure
Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="create-a-network-security-group"></a>Crear un grupo de seguridad de red

1. Busque y seleccione un grupo de recursos para la máquina virtual, elija **Agregar** y busque y seleccione **Grupo de seguridad de red**.

2. Seleccione **Crear**.

    Se abre la ventana **Crear un grupo de seguridad de red**.

    ![Crear un grupo de seguridad de red](./media/nsg-quickstart-portal/create-nsg.png)

2. Escriba un nombre para el grupo de seguridad de red. 

3. Seleccione o cree un grupo de recursos y luego seleccione una ubicación.

4. Seleccione **Crear** para crear el grupo de seguridad de red.

## <a name="create-an-inbound-security-rule"></a>Creación de una regla de seguridad de entrada

1. Seleccione el nuevo grupo de seguridad de red. 

2. Seleccione **Reglas de seguridad de entrada** y luego **Agregar**.

    ![Agregar una regla de entrada](./media/nsg-quickstart-portal/add-inbound-rule.png)

3. Seleccione **Advanced** (Avanzadas). 

4. Elija un **servicio** común en el menú desplegable, como **HTTP**. También puede seleccionar **Personalizado** si quiere proporcionar un puerto específico para su uso. 

5. Si lo desea, cambie la **Prioridad** o el **Nombre**. La prioridad afecta al orden en que se aplican las reglas (cuanto más bajo es el valor numérico, antes se aplica la regla).

6. Para crear la regla, seleccione **Agregar**.

## <a name="associate-your-network-security-group-with-a-subnet"></a>Asociación del grupo de seguridad de red con una subred

El paso final es asociar el grupo de seguridad de red a una subred o una interfaz de red específica. Para este ejemplo, vamos a asociar el grupo de seguridad de red con una subred. 

1. Seleccione **Subredes** y elija **Asociar**.

    ![Asociar un grupo de seguridad de red con una subred](./media/nsg-quickstart-portal/associate-subnet.png)

2. Seleccione la red virtual y luego seleccione la subred adecuada.

    ![Asociar un grupo de seguridad de red con una red virtual](./media/nsg-quickstart-portal/select-vnet-subnet.png)

    A través del puerto 80 se accede a las máquinas virtuales que se conectan a esa subred.

## <a name="additional-information"></a>Información adicional

También puede [llevar a cabo los pasos en este artículo utilizando Azure PowerShell](nsg-quickstart-powershell.md).

Los comandos descritos en este artículo le permiten hacer que el tráfico fluya rápidamente a la máquina virtual. Los grupos de seguridad de red proporcionan muchas características excelentes y granularidad para controlar el acceso a sus recursos. Para más información, consulte [Filtrado del tráfico de red con un grupo de seguridad de red](../../virtual-network/tutorial-filter-network-traffic.md).

Para las aplicaciones web de alta disponibilidad, considere la posibilidad de colocar las máquinas virtuales detrás de un equilibrador de carga de Azure. El equilibrador de carga distribuye el tráfico a las máquinas virtuales, con un grupo de seguridad de red que proporciona el filtrado del tráfico. Para más información, consulte [Equilibrio de carga de máquinas virtuales Windows en Azure para crear una aplicación de alta disponibilidad](tutorial-load-balancer.md).

## <a name="next-steps"></a>Pasos siguientes
En este artículo ha creado un grupo de seguridad de red, creó una regla de entrada que permite el tráfico HTTP en el puerto 80 y luego ha asociado esta regla con una subred. 

Puede encontrar información sobre la creación de entornos más detallados en los siguientes artículos:
- [Información general sobre Azure Resource Manager](../../azure-resource-manager/management/overview.md)
- [Grupos de seguridad](../../virtual-network/security-overview.md)