---
title: Actualización de una máquina virtual de Azure con SUSE Linux Enterprise Server a SUSE 15 SP1 | Microsoft Docs
description: En este artículo se exponen los pasos generales para usar el sistema de migración de distribución de SUSE con el fin de actualizar SUSE Linux Enterprise Server a SUSE 15 SP1 para una máquina virtual de Azure.
services: virtual-machines-linux
documentationcenter: ''
author: amkarmak
manager: dcscontentpm
editor: ''
tags: virtual-machines
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 05/21/2020
ms.author: arremana
ms.openlocfilehash: 7ed355f82b88f460ff4b372484a690f166a15550
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91359518"
---
# <a name="upgrade-azure-vm-with-sles-12-to-sles-15-sp1"></a>Actualización de una máquina virtual de Azure con SLES 12 a SLES 15 SP1

En este artículo se exponen los pasos generales para actualizar SUSE Linux Enterprise Server (SLES) 12 a SLES 15 SP1 para una máquina virtual de Azure. Para obtener más información, vea [Uso del sistema de migración de distribución de SUSE](https://documentation.suse.com/suse-distribution-migration-system/1.0/single-html/distribution-migration-system/index.html) y [Guía de actualización de SUSE Linux Enterprise Server 15 SP1](https://documentation.suse.com/sles/15-SP1/single-html/SLES-upgrade/index.html#sec-update-preparation-update).

## <a name="supported-upgrade-paths"></a>Rutas de actualización compatibles
La versión SLES actual debe ser SLES 12 SP4 o 12 SP5 para poder realizar la actualización a SLES 15 SP1.

![Captura de pantalla con la ruta de actualización admitida](./media/linux-upgrade-suse-15sp1/upgrade-path.png)

## <a name="prerequisites"></a>Requisitos previos

- Planifique la actividad de migración según el período de tiempo de inactividad aprobado. Esto es importante porque la máquina virtual se reinicia durante la migración.
- Antes de empezar la actividad de migración, realice una copia de seguridad completa de la máquina virtual.
- Si la copia de seguridad no está configurada, realice una copia de seguridad de instantánea del disco del sistema operativo.
- [Compruebe si la máquina virtual es de generación V1 o V2](#check-the-generation-version-for-a-vm).

## <a name="upgrade-from-suse-12-sp4-or-sp5-to-suse-15-sp1"></a>Actualización de SUSE 12 SP4 o SP5 a SUSE 15 SP1

1. Instale el paquete más reciente de la máquina virtual:

    ```
    zypper clean --all
    zypper refresh
    zypper update
    ```

2. Después de que la instalación se haya completado, reinicie la máquina virtual.

3. Compruebe la versión del kernel y del sistema operativo. Asegúrese de que la versión sea SUSE 12 SP4 o SUSE 12 SP5.

    ```
    uname -a
    cat /etc/os-release
    ```

4. Instale el paquete **suse-migration-sle15-activation**. Cuando se haya instalado el paquete **suse-migration-sle15-activation**, se instalará automáticamente otro paquete **SLES15-Migration** como un paquete de dependencia. 

   ```
   zypper install suse-migration-sle15-activation
   ```

5. Después de que la instalación se haya completado, ejecute el comando `reboot` para reiniciar la máquina virtual.

6. Vaya a [Azure Portal](https://portal.azure.com), seleccione la máquina virtual y elija **Consola de serie**. Verá que el sistema se detiene en "reboot: Restarting system" (Reinicio: el sistema se está reiniciando). Este proceso tardará alrededor de 15-45 minutos. En el caso de una máquina virtual de generación 2, podría quedarse bloqueada en la pantalla "reboot: Restarting system". En este caso, espere 45 minutos. Si sigue sin progresar, vaya a la página **Información general** de la máquina virtual en Azure Portal, deténgala y reiníciela.

     ![Captura de pantalla con los mensajes de la consola serie.](./media/linux-upgrade-suse-15sp1/reboot-message.png)

8. Una vez que el sistema se ha reiniciado con el nuevo kernel, verá el mensaje siguiente.

     ![Captura de pantalla con los mensajes de la consola serie después de reiniciar el sistema con el nuevo kernel.](./media/linux-upgrade-suse-15sp1/output-message.png)
9. Compruebe la versión del kernel y del sistema operativo para asegurarse de que el sistema se ha actualizado correctamente.

    ```
    uname -a
    cat /etc/os-release
    ```

## <a name="check-the-generation-version-for-a-vm"></a>Comprobación de la versión de generación de una máquina virtual

Puede usar uno de los métodos siguientes para configurar la versión de generación:

- En el terminal de SLES, ejecute el comando `dmidecode | grep -i hyper`. Si se trata de una máquina virtual de generación V1, no devolverá ningún resultado. En el caso de las máquinas virtuales de generación V2, verá la salida siguiente:

     ![Captura de pantalla con la salida de la máquina virtual de generación 2](./media/linux-upgrade-suse-15sp1/output-gen2.png)
- En [Azure Portal](https://portal.azure.com), vaya a **Propiedades** de la máquina virtual y compruebe el campo **Generación de VM**.
