---
title: Conexión a un conjunto de escalado de máquinas virtuales Windows con Azure Bastion | Microsoft Docs
description: En este artículo aprenderá a conectarse a un conjunto de escalado de máquinas virtuales de Azure mediante Azure Bastion.
services: bastion
author: charwen
ms.service: bastion
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: charwen
ms.openlocfilehash: 290a20fcd827841c24983f3bdd54b6db8e154462
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "80619337"
---
# <a name="connect-to-a-virtual-machine-scale-set-using-azure-bastion"></a>Conexión a un conjunto de escalado de máquinas virtuales mediante Azure Bastion

En este artículo se muestra cómo establecer una conexión segura y sin problemas mediante RDP con un escalado de máquinas virtuales Windows de una red virtual de Azure con Azure Bastion. Puede conectarse a la instancia de un conjunto de escalado de máquinas virtuales directamente desde Azure Portal. Cuando se utiliza Azure Bastion, las máquinas virtuales no requieren un cliente, un agente ni software adicional. Para más información sobre Azure Bastion, consulte la página de [información general](bastion-overview.md).

## <a name="before-you-begin"></a>Antes de empezar

Asegúrese de haber configurado un host de Azure Bastion para la red virtual donde reside el conjunto de escalado de máquinas virtuales. Para más información, consulte [Create an Azure Bastion host](bastion-create-host-portal.md) (Creación de un host de Azure Bastion). Cuando el servicio Bastion se aprovisiona e implementa en la red virtual, puede usarlo para conectarse a cualquier instancia de conjunto de escalado de máquinas virtuales de esta red virtual. Bastion presupone que usa RDP para conectarse a un conjunto de escalado de máquinas virtuales Windows y SSH para conectarse al conjunto de escalado de máquinas virtuales Linux. Para información sobre la conexión a una máquina virtual Linux, consulte el artículo sobre cómo [conectarse a una máquina virtual Linux](bastion-connect-vm-ssh.md).

## <a name="connect-using-rdp"></a><a name="rdp"></a>Conexión mediante RDP

1. Abra [Azure Portal](https://portal.azure.com). Vaya al conjunto de escalado de máquinas virtuales al que quiere conectarse.

   ![navegar](./media/bastion-connect-vm-scale-set/1.png)
2. Vaya a la instancia del conjunto de escalado de máquinas virtuales al que quiere conectarse y, luego, seleccione **Conectar**. Cuando se usa una conexión RDP, el conjunto de escalado de máquinas virtuales debe ser un conjunto de escalado de máquinas virtuales Windows.

   ![conjunto de escalado de máquinas virtuales](./media/bastion-connect-vm-scale-set/2.png)
3. Después de seleccionar **Conectar**, aparecerá una barra lateral con tres pestañas: RDP, SSH y Bastion. Seleccione la pestaña **Bastion** de la barra lateral. Si no aprovisionó Bastion para la red virtual, puede seleccionar el vínculo para configurarlo. Para instrucciones sobre la configuración, consulte el artículo sobre la [configuración de Bastion](bastion-create-host-portal.md).

   ![Pestaña Bastion](./media/bastion-connect-vm-scale-set/3.png)
4. En la pestaña Bastion, escriba el nombre de usuario y la contraseña del conjunto de escalado de máquinas virtuales y, luego, seleccione **Conectar**.

   ![conectar](./media/bastion-connect-vm-scale-set/4.png)
5. La conexión RDP con esta máquina virtual a través de Bastion se abrirá directamente en Azure Portal (a través de HTML5) mediante el puerto 443 y el servicio Bastion.

## <a name="next-steps"></a>Pasos siguientes

Lea el artículo sobre [preguntas frecuentes de Bastion](bastion-faq.md).