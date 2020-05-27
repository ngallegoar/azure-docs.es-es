---
title: Habilitación de Escritorio remoto para Linux en Azure Lab Services | Microsoft Docs
description: Obtenga información sobre cómo habilitar Escritorio remoto para máquinas virtuales Linux en un laboratorio en Azure Lab Services.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2020
ms.author: spelluru
ms.openlocfilehash: a40575340fc5b1c202be6b001807085954439f03
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588163"
---
# <a name="enable-remote-desktop-for-linux-virtual-machines-in-a-lab-in-azure-lab-services"></a>Habilitación del Escritorio remoto para máquinas virtuales Linux en un laboratorio de Azure Lab Services
En este artículo se muestra cómo realizar las tareas siguientes:

- Habilitar Escritorio remoto para una VM Linux
- Cómo los formadores pueden conectarse a la VM de plantillas a través de una Conexión a Escritorio remoto (RDP).

## <a name="enable-remote-desktop-for-linux-vm"></a>Habilitar Escritorio remoto para una VM Linux
Durante la creación del laboratorio, los formadores pueden habilitar la **Conexión a Escritorio remoto** para imágenes **Linux**. La opción **Habilitar Conexión a Escritorio remoto** aparece cuando se selecciona una imagen de Linux para la plantilla. Cuando se habilita esta opción, los formadores pueden conectarse a la VM de plantillas y las VM de alumnos a través de RDP (Escritorio remoto). 

![Habilitación de la conexión a Escritorio remoto para una imagen de Linux](../media/how-to-enable-remote-desktop-linux/enable-rdp-option.png)

En el cuadro de mensaje **Enabling Remote Desktop Connection** (Habilitar conexión a Escritorio remoto), seleccione **Continue with Remote Desktop** (Continuar con Escritorio remoto). 

![Habilitación de la conexión a Escritorio remoto para una imagen de Linux](../media/how-to-enable-remote-desktop-linux/enabling-remote-desktop-connection-dialog.png)

> [!IMPORTANT] 
> Al habilitar una **conexión a Escritorio remoto**, solo se abre el puerto **RDP** en máquinas Linux. Si RDP ya está instalado y configurado en la imagen de máquina virtual, los alumnos pueden conectarse a las máquinas virtuales a través de RDP sin seguir ningún paso adicional.
> 
> Si la imagen de máquina virtual no tiene RDP instalado y configurado, debe conectarse a la máquina Linux mediante SSH por primera vez e instalar paquetes RDP y GUI para que sea posible conectarse a la máquina Linux mediante RDP más tarde. Para más información, consulte [Instalación y configuración del escritorio remoto para conectarse a una máquina virtual Linux en Azure](../../virtual-machines/linux/use-remote-desktop.md). A continuación, publicará la imagen para que los alumnos puedan usar RDP para conectarse a las máquinas virtuales Linux de alumnos. 

## <a name="supported-operating-systems"></a>Sistemas operativos admitidos
Actualmente, la conexión a Escritorio remoto se admite para los sistemas operativos siguientes:

- openSUSE Leap 42.3
- Basado en CentOS 7.5
- Debian 9 "Stretch"
- Ubuntu Server 16.04 LTS

## <a name="connect-to-the-template-vm"></a>Conexión a la plantilla de máquina virtual 
Los formadores primero deben conectarse a la VM de plantillas mediante SSH e instalar los paquetes de RDP y GUI en ella. A continuación, los formadores pueden usar RDP para conectarse a la VM de plantilla: 

1. Si ve **Customize template** (Personalizar plantilla) en la barra de herramientas, selecciónelo. A continuación, seleccione **Continue** (Continuar) en el cuadro de diálogo **Customize template** (Personalizar plantilla). Esta acción inicia la máquina virtual de la plantilla.  

    ![Customize template (Personalizar plantilla)](../media/how-to-enable-remote-desktop-linux/customize-template.png)
2. Una vez iniciada la máquina virtual de plantilla, puede seleccionar **Connect template** (Conectar plantilla) y **Connect via SSH** (Conectar mediante SSH) en la barra de herramientas. 

    ![Conexión a una plantilla a través de RDP una vez creado el laboratorio](../media/how-to-enable-remote-desktop-linux/rdp-after-lab-creation.png) 
3. Verá el cuadro de diálogo **Conectarse a su máquina virtual** siguiente. Seleccione el botón **Copiar** situado junto al cuadro de texto para copiarlo en el Portapapeles. Guarde la cadena de conexión de SSH. Utilice esta cadena de conexión desde un terminal de SSH (como [Putty](https://www.putty.org/)) para conectarse a la máquina virtual.
 
    ![Cadena de conexión de SSH](../media/how-to-enable-remote-desktop-linux/ssh-connection-string.png)
4. Instale los paquetes de RDP y de la interfaz gráfica de usuario para que los alumnos puedan conectarse a la máquina Linux mediante RDP más adelante. Para más información, consulte [Instalación y configuración del escritorio remoto para conectarse a una máquina virtual Linux en Azure](../../virtual-machines/linux/use-remote-desktop.md). A continuación, publicará la imagen para que los alumnos puedan usar RDP para conectarse a las máquinas virtuales Linux de alumnos.
5. Una vez instalados estos paquetes, puede usar **Connect to template** (Conectarse a la plantilla) y luego **Connect via RDP** (Conectar a través de RDP) para conectarse a la máquina virtual de plantilla mediante RDP. Guarde el archivo RDP y úselo para conectarse a la máquina virtual de plantilla mediante RDP. 

## <a name="next-steps"></a>Pasos siguientes
Después de que un instructor habilita la característica de Conexión a Escritorio remoto, los alumnos pueden conectarse a sus máquinas virtuales a través de RDP/SSH. Para obtener más información, consulte [Uso de Escritorio remoto para máquinas virtuales Linux en un laboratorio de clase](how-to-use-remote-desktop-linux-student.md). 