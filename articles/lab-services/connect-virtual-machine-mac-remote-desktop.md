---
title: Procedimientos para conectar a una máquina virtual Azure Lab Services desde Mac | Microsoft Docs
description: Aprenda a conectarse desde un equipo Mac a una máquina virtual en Azure Lab Services.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 674bea13093e185ae991148c2cf96599a273d0bc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85444631"
---
# <a name="connect-to-a-vm-using-remote-desktop-protocol-on-a-mac"></a>Conexión a una máquina virtual mediante el Protocolo de escritorio remoto en un equipo Mac
En esta sección se muestra cómo un alumno puede conectarse a una máquina virtual de un laboratorio educativo desde un equipo Mac mediante RDP.

## <a name="install-microsoft-remote-desktop-on-a-mac"></a>Instalar Escritorio remoto de Microsoft en un equipo Mac
1. Abra App Store en el equipo Mac y busque **Escritorio remoto de Microsoft**.

    ![Escritorio remoto de Microsoft](./media/how-to-use-classroom-lab/install-ms-remote-desktop.png)
1. Instale la versión más reciente de Escritorio remoto de Microsoft. 

## <a name="access-the-vm-from-your-mac-using-rdp"></a>Acceder a la máquina virtual desde el equipo Mac con RDP
1. Abra el archivo **RDP** que se descarga en el equipo con **Escritorio remoto de Microsoft** instalado. Debe empezar a conectarse a la máquina virtual. 

    ![Conexión con una máquina virtual](./media/how-to-use-classroom-lab/connect-linux-vm.png)
1. Seleccione **Continuar** si recibe la advertencia siguiente. 

    ![Advertencia de certificado](./media/how-to-use-classroom-lab/certificate-error.png)
1. Debería ver la máquina virtual. 

    > [!NOTE]
    > El ejemplo siguiente es para una máquina virtual Linux de CentOS. 

    ![máquina virtual](./media/how-to-use-classroom-lab/vm-ui.png)


## <a name="next-steps"></a>Pasos siguientes
Para obtener información sobre cómo conectarse a máquinas virtuales Linux con RDP, consulte [Uso de Escritorio remoto para máquinas virtuales Linux](how-to-use-remote-desktop-linux-student.md)


