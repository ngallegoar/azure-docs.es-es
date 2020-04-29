---
title: Conectarse a la máquina virtual basada en Microsoft Azure | Azure Marketplace
description: Se explica cómo conectarse a la nueva máquina virtual creada en Azure.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: dsindona
ms.openlocfilehash: 4aea624c2127c9b0a61d72b8d14929ce6f47df24
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82142486"
---
# <a name="connect-to-your-azure-based-virtual-machine"></a>Conectarse a la máquina virtual basada en Azure

> [!IMPORTANT]
> A partir del 13 de abril de 2020, comenzaremos a trasladar la administración de las ofertas de máquinas virtuales de Azure al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Creación de recursos técnicos de Azure Virtual Machine](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-create-offer) para administrar las ofertas migradas.

En este artículo se explica cómo conectarse a las máquinas virtuales (VM) creadas en Azure y cómo iniciar sesión en ellas.  Cuando se haya conectado correctamente, puede trabajar con la máquina virtual como si hubiera iniciado sesión localmente en su servidor host.

## <a name="connect-to-a-windows-based-vm"></a>Conectarse a una máquina virtual basada en Windows

Para conectarse a la máquina virtual basada en Windows hospedada en Azure, se usa el cliente de escritorio remoto.  La mayoría de las versiones de Windows incluyen compatibilidad de forma nativa con el Protocolo de escritorio remoto (RDP).  En el caso de las demás máquinas, puede encontrar más información sobre los clientes en [Clientes de escritorio remoto](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients).  

En el siguiente artículo se detalla cómo usar la compatibilidad integrada de Windows RDP para conectarse a una VM: [Conexión a una máquina virtual de Azure donde se ejecuta Windows e inicio de sesión en ella](../../../virtual-machines/windows/connect-logon.md).  

>[!TIP]
> Es posible que obtenga advertencias de seguridad durante el proceso, por ejemplo que el archivo .rdp procede de un publicador desconocido o que no se pueden verificar sus credenciales de usuario.  Es seguro hacer caso omiso de estas advertencias.


## <a name="connect-to-a-linux-based-vm"></a>Conectarse a una máquina virtual basada en Linux

Para conectarse a la máquina virtual basada en Linux, necesita un cliente de protocolo Secure Shell (SSH).  Esta discusión usa el terminal SSH [PuTTY](https://www.ssh.com/ssh/putty/) libre.

1. Vaya a [Azure Portal](https://ms.portal.azure.com). Busque y seleccione **Máquinas virtuales**. 
2. Seleccione el nombre de la máquina virtual a la que quiere conectarse.  
3. **Inicie** la máquina virtual si aún no se está ejecutando.
4. Haga clic en el nombre de la máquina virtual para abrir su página **Información general**.
5. Anote la dirección IP pública y el nombre DNS de la máquina virtual.  (Si estos valores no están establecidos, debe [Crear una interfaz de red](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface#create-a-network-interface))

   ![Configuración de Información general de máquina virtual](./media/publishvm_019.png)
 
6. Abra la aplicación PuTTY.  
7. En el cuadro de diálogo Configuración de PuTTY, escriba la dirección IP o el nombre DNS de la máquina virtual. 

   ![Configuración de terminal PuTTY](./media/publishvm_020.png)
 
8. Haga clic en **Abrir** para abrir un terminal PuTTY.  
9. Cuando se le pida, escriba el nombre y la contraseña de la cuenta de máquina virtual Linux. 

Si tiene problemas de conexión, vea la documentación del cliente SSH, por ejemplo el [Capítulo 10: Mensajes de error habituales](https://www.ssh.com/ssh/putty/putty-manuals).

Para obtener más información, por ejemplo cómo agregar un escritorio a una máquina virtual Linux aprovisionada, vea [Instalación y configuración del escritorio remoto para conectarse a una máquina virtual Linux en Azure](../../../virtual-machines/linux/use-remote-desktop.md).


## <a name="stop-unused-vms"></a>Detener máquinas virtuales sin usar
Azure factura por el hospedaje de máquinas virtuales cuando hay una en ejecución *o inactiva*.  Por lo tanto, es recomendable detener las máquinas virtuales que no se están usando.  Por ejemplo, las máquinas virtuales de prueba, de copia de seguridad o retiradas son candidatas para la detención. Para detener una máquina virtual, complete los pasos siguientes:

1. En la hoja **Máquinas virtuales**, seleccione la máquina virtual que quiere detener. 
2. En la barra de herramientas de la parte superior de la página, haga clic en el botón **Detener**.

   ![Detención de una máquina virtual](./media/publishvm_018.png)

Azure detiene rápidamente la máquina virtual en un proceso denominado *desasignación*, que no solo apaga el sistema operativo en la máquina virtual, sino que también libera los recursos de hardware y de red aprovisionados previamente para ella.

Si quiere volver a activar más adelante una máquina virtual detenida, selecciónela y haga clic en el botón **Iniciar**.


## <a name="next-steps"></a>Pasos siguientes

Cuando se haya conectado de forma remota, estará listo para [configurar la máquina virtual](./cpp-configure-vm.md).
