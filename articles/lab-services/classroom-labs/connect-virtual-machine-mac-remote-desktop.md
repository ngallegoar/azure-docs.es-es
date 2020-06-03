---
title: Procedimientos para conectar a una máquina virtual Azure Lab Services desde Mac | Microsoft Docs
description: Aprenda a conectarse desde un equipo Mac a una máquina virtual en Azure Lab Services.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2020
ms.author: spelluru
ms.openlocfilehash: 9087e4fb7989f89c19a70afd9ee8f061f9061166
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83704370"
---
# <a name="connect-to-a-vm-using-remote-desktop-protocol-on-a-mac"></a>Conexión a una máquina virtual mediante el Protocolo de escritorio remoto en un equipo Mac
En esta sección se muestra cómo un alumno puede conectarse a una máquina virtual de un laboratorio educativo desde un equipo Mac mediante RDP.

## <a name="install-microsoft-remote-desktop-on-a-mac"></a>Instalar Escritorio remoto de Microsoft en un equipo Mac
1. Abra App Store en el equipo Mac y busque **Escritorio remoto de Microsoft**.

    ![Escritorio remoto de Microsoft](../media/how-to-use-classroom-lab/install-ms-remote-desktop.png)
1. Instale la versión más reciente de Escritorio remoto de Microsoft. 

## <a name="access-the-vm-from-your-mac-using-rdp"></a>Acceder a la máquina virtual desde el equipo Mac con RDP
1. Abra el archivo **RDP** que se descarga en el equipo con **Escritorio remoto de Microsoft** instalado. Debe empezar a conectarse a la máquina virtual. 

    ![Conexión con una máquina virtual](../media/how-to-use-classroom-lab/connect-linux-vm.png)
1. Seleccione **Continuar** si recibe la advertencia siguiente. 

    ![Advertencia de certificado](../media/how-to-use-classroom-lab/certificate-error.png)
1. Debería ver la máquina virtual. 

    > [!NOTE]
    > El ejemplo siguiente es para una máquina virtual Linux de CentOS. 

    ![máquina virtual](../media/how-to-use-classroom-lab/vm-ui.png)


## <a name="next-steps"></a>Pasos siguientes
Para obtener información sobre cómo conectarse a máquinas virtuales Linux con RDP, consulte [Uso de Escritorio remoto para máquinas virtuales Linux](how-to-use-remote-desktop-linux-student.md)


