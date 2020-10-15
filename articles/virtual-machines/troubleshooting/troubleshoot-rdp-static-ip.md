---
title: No se puede conectar mediante escritorio remoto a Azure Virtual Machines debido a una dirección IP estática | Microsoft Docs
description: Obtenga información sobre cómo solucionar problemas de RDP provocados por una dirección IP estática en Microsoft Azure | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/08/2018
ms.author: genli
ms.openlocfilehash: 49f3f44c7de8c700d0093c5eb6f166a1dffb34a4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86087255"
---
#  <a name="cannot-remote-desktop-to-azure-virtual-machines-because-of-static-ip"></a>No se puede conectar mediante escritorio remoto a Azure Virtual Machines debido a una dirección IP estática

En este artículo se describe un problema en el que no se puede conectar mediante escritorio remoto a Azure Windows Virtual Machines (máquinas virtuales) después de configurar una dirección IP estática en la máquina virtual.


## <a name="symptoms"></a>Síntomas

Al realizar una conexión RDP a una máquina virtual en Azure, recibe el siguiente mensaje de error:

**Escritorio remoto no se puede conectar al equipo remoto por una de estas razones:**

1. **No está habilitado el acceso remoto al servidor.**

2. **El equipo remoto está desactivado.**

3. **El equipo remoto no está disponible en la red.**

**Asegúrese de que el equipo remoto está encendido y conectado a la red, y de que está habilitado el acceso remoto.**

Al comprobar la captura de pantalla de los [Diagnósticos de arranque](../troubleshooting/boot-diagnostics.md) en Azure portal, puede ver que la máquina virtual se inicia de manera normal y espera las credenciales en la pantalla de inicio de sesión.

## <a name="cause"></a>Causa

La máquina virtual tiene una dirección IP estática que se define en la interfaz de red en Windows. Esta dirección IP es diferente de la dirección que se ha definido en Azure Portal.

## <a name="solution"></a>Solución

Antes de seguir estos pasos, tome una instantánea del disco del sistema operativo de la máquina virtual afectada como copia de seguridad. Para obtener más información, consulte [Instantánea de un disco](../windows/snapshot-copy-managed-disk.md).

Para resolver este problema, use el control serie para habilitar DHCP o realice un [restablecimiento de la interfaz de red](reset-network-interface.md) de la máquina virtual.

### <a name="use-serial-control"></a>Uso del control serie

1. Conéctese a una [consola serie y abra la instancia de CMD](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console
). Si la consola serie no está habilitada en la máquina virtual, consulte [Restablecimiento de la interfaz de red](reset-network-interface.md).
2. Compruebe si el protocolo DHCP está deshabilitado en la interfaz de red:

    ```console
    netsh interface ip show config
    ```

3. Si el protocolo DHCP está deshabilitado, puede revertir la configuración de la interfaz de red para usar DHCP:

    ```console
    netsh interface ip set address name="<NIC Name>" source=dhc
    ```

    Por ejemplo, si la interfaz de red se llama "Ethernet 2", ejecute el siguiente comando:

    ```console
    netsh interface ip set address name="Ethernet 2" source=dhc
    ```

4. Consulte de nuevo la configuración IP para asegurarse de que la interfaz de red está ahora configurada correctamente. La nueva dirección IP debe coincidir con la proporcionada por Azure.

    ```console
    netsh interface ip show config
    ```

    No tiene que reiniciar la máquina virtual en este momento. La máquina virtual volverá a ser accesible.

Después de eso, si desea configurar la dirección IP estática para la máquina virtual, consulte [Configuración de direcciones IP estáticas para una máquina virtual](../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md).