---
author: rothja
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 10/26/2018
ms.author: jroth
ms.openlocfilehash: fe5daa38c43723c85fb464e191ee4a3e85700e0b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "67186271"
---
1. Una vez creada y en ejecución la máquina virtual de Azure, haga clic en el icono Virtual Machines en Azure Portal para ver las máquinas virtuales.

1. Haga clic en el botón de puntos suspensivos, **...** , de la nueva máquina virtual.

1. Haga clic en **Conectar**.

   ![Conexión a la máquina virtual en el portal](./media/virtual-machines-sql-server-remote-desktop-connect/azure-virtual-machine-connect.png)

1. Abra el archivo **RDP** que ha descargado el explorador para la máquina virtual.

1. La conexión a Escritorio remoto le avisará de que no se puede identificar el publicador de esta conexión remota. Haga clic en **Conectar** para continuar.

1. En el cuadro de diálogo **Seguridad de Windows**, haga clic en **Usar una cuenta diferente**. Es posible que deba hacer clic en **Más opciones** para verlo. Use el nombre de usuario y la contraseña que configuró al crear la máquina virtual. Debe agregar una barra diagonal inversa delante del nombre de usuario.

   ![Autenticación de escritorio remoto](./media/virtual-machines-sql-server-remote-desktop-connect/remote-desktop-connect.png)

1. Haga clic en **Aceptar** para conectar.
