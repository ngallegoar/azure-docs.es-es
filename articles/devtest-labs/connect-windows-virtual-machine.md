---
title: Conexión a las máquinas virtuales Windows en Azure DevTest Labs
description: Obtenga información sobre cómo conectarse a las máquinas virtuales Windows en un laboratorio (Azure DevTest Labs)
ms.topic: how-to
ms.date: 07/17/2020
ms.openlocfilehash: e1e786daa396548030976159d1b150caa4b24396
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86540625"
---
# <a name="connect-to-a-windows-vm-in-your-lab-azure-devtest-labs"></a>Conexión a una máquina virtual Windows en el laboratorio (Azure DevTest Labs)
En este artículo se muestra cómo conectarse a máquina virtual Windows en el laboratorio. 

## <a name="connect-to-a-windows-vm"></a>Conexión a una máquina virtual Windows
1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. En la barra de búsqueda, busque y seleccione **DevTest Labs** . 

    :::image type="content" source="./media/connect-windows-virtual-machine/search-select.png" alt-text="Búsqueda y selección de DevTest Labs":::    
1. En la lista de laboratorios, seleccione el **suyo** .

    :::image type="content" source="./media/connect-windows-virtual-machine/select-lab.png" alt-text="Búsqueda y selección de DevTest Labs":::            
1. En la página principal del laboratorio, seleccione la máquina virtual Windows en la lista **Mis máquinas virtuales** . 

    :::image type="content" source="./media/connect-windows-virtual-machine/select-windows-vm.png" alt-text="Búsqueda y selección de DevTest Labs":::                
1. En la página **Máquina virtual** de la VM, seleccione **Conectar** en la barra de herramientas. Para iniciar la máquina virtual en caso de estar detenida, seleccione **Iniciar** .

    :::image type="content" source="./media/connect-windows-virtual-machine/select-connect.png" alt-text="Búsqueda y selección de DevTest Labs":::                    
1. Si usa el explorador Edge, verá el vínculo al **archivo RDP descargado** en la parte inferior del mismo. 

    :::image type="content" source="./media/connect-windows-virtual-machine/rdp-download.png" alt-text="Búsqueda y selección de DevTest Labs":::                        
1. Abra el archivo RDP y escriba las credenciales de máquina virtual que escribió al crearla. Ahora debe estar conectado a la máquina virtual Windows. 

## <a name="next-steps"></a>Pasos siguientes
[Procedimiento: Conexión a una máquina virtual Linux](connect-linux-virtual-machine.md)
