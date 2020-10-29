---
title: archivo de inclusión
description: archivo de inclusión
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 10/21/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 083ab61d5a20bfb8e38747ae0694b1176c0a0fd1
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92521547"
---
1. Abra [Azure Portal](https://portal.azure.com). Vaya a la máquina virtual a la que quiere conectarse y, luego, haga clic en **Conectar** . En el menú desplegable, seleccione **Bastion** .

   :::image type="content" source="./media/bastion-vm-rdp/connect-vm.png" alt-text="Seleccione Bastion":::

1. Después de seleccionar Bastion en el menú desplegable, aparecerá una barra lateral con tres pestañas: RDP, SSH y Bastion. Debido a que aprovisionó Bastion para la red virtual, la pestaña Bastion aparece activa de manera predeterminada. Seleccione **Usar Bastion** .

   :::image type="content" source="./media/bastion-vm-rdp/select-use-bastion.png" alt-text="Seleccione Bastion":::

1. En la página **Conectar mediante Azure Bastion** , escriba el nombre de usuario y la contraseña de las máquinas virtuales y, luego, seleccione **Conectar** .

   :::image type="content" source="./media/bastion-vm-rdp/connect-vm-host.png" alt-text="Seleccione Bastion":::

1. La conexión RDP con esta máquina virtual a través de Bastion se abrirá directamente en Azure Portal (a través de HTML5) mediante el puerto 443 y el servicio Bastion.

   :::image type="content" source="./media/bastion-vm-rdp/connection.png" alt-text="Seleccione Bastion":::
