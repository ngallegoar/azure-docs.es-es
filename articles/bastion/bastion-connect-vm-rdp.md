---
title: Conexión a una máquina virtual Windows con Azure Bastion
description: En este artículo, descubra cómo usar Azure Bastion para conectarse a una máquina virtual de Azure que ejecuta Windows.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 02/24/2020
ms.author: cherylmc
ms.openlocfilehash: 6ec60dc313c8a4374637adf38ea0e5a7d4ed964b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77597358"
---
# <a name="connect-to-a-windows-virtual-machine-using-azure-bastion"></a>Conexión a una máquina virtual Windows con Azure Bastion

Con Azure Bastion, puede conectarse de forma segura y sin problemas a las máquinas virtuales en SSL directamente en Azure Portal. Cuando se usa Azure Bastion, las máquinas virtuales no necesitan un cliente, un agente o software adicional. En este artículo se muestra cómo conectarse a las máquinas virtuales Windows. Para información sobre cómo conectarse a una máquina virtual Linux, consulte [Conexión a una máquina virtual mediante Azure Bastion: Linux](bastion-connect-vm-ssh.md).

Azure Bastion proporciona conectividad segura a todas las máquinas virtuales de la red virtual en la que se aprovisiona. El uso de Azure Bastion protege las máquinas virtuales frente a la exposición de los puertos de RDP/SSH al mundo exterior, al tiempo que ofrece acceso seguro mediante RDP/SSH. Para más información, consulte la [introducción](bastion-overview.md).

## <a name="before-you-begin"></a>Antes de empezar

Asegúrese de haber configurado un host de Azure Bastion para la red virtual donde está ubicada la máquina virtual. Cuando el servicio Bastion se aprovisiona e implementa en la red virtual, puede usarlo para conectarse a cualquier máquina virtual de la red virtual. Para configurar un host de Azure Bastion, consulte [Creación de un host de Azure Bastion](bastion-create-host-portal.md).

### <a name="required-roles"></a>Roles necesarios

Para crear una conexión, se requieren los siguientes roles:

* Rol de lector en la máquina virtual
* Rol de lector en la tarjeta de interfaz de red con la dirección IP privada de la máquina virtual
* Rol de lector en el recurso de Azure Bastion

### <a name="ports"></a>Puertos

Para conectarse a la máquina virtual Windows, debe tener abiertos los siguientes puertos en ella:

* Puertos de entrada: RDP (3389)

## <a name="connect"></a><a name="rdp"></a>Conexión

1. Abra [Azure Portal](https://portal.azure.com). Vaya a la máquina virtual a la que quiere conectarse, luego haga clic en **Conectar** y seleccione **Bastion** en la lista desplegable.

   ![Conexión de una máquina virtual](./media/bastion-connect-vm-rdp/connect.png)
1. Después de hacer clic en Bastion, aparece una barra lateral con tres pestañas: RDP, SSH y Bastion. Si se aprovisionó Bastion para la red virtual, la pestaña Bastion aparece activa de manera predeterminada. Si no aprovisionó Bastion para la red virtual, puede hacer clic en el vínculo para configurarlo. Para instrucciones sobre la configuración, consulte el artículo sobre la [configuración de Bastion](bastion-create-host-portal.md).

   ![Pestaña Bastion](./media/bastion-connect-vm-rdp/bastion.png)
1. En la pestaña Bastion, escriba el nombre de usuario y la contraseña de la máquina virtual y, luego, haga clic en **Connect** (Conectar). La conexión RDP con esta máquina virtual a través de Bastion se abrirá directamente en Azure Portal (a través de HTML5) mediante el puerto 443 y el servicio Bastion.

   ![Conexión RDP](./media/bastion-connect-vm-rdp/443rdp.png)
 
## <a name="next-steps"></a>Pasos siguientes

Lea el artículo sobre [preguntas frecuentes de Bastion](bastion-faq.md).
