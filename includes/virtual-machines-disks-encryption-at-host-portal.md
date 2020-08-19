---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/07/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: f0a706cc51027bc503da9ee3bc637216e9e52853
ms.sourcegitcommit: d7bd8f23ff51244636e31240dc7e689f138c31f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2020
ms.locfileid: "87171372"
---
## <a name="restrictions"></a>Restricciones

[!INCLUDE [virtual-machines-disks-encryption-at-host-restrictions](virtual-machines-disks-encryption-at-host-restrictions.md)]

### <a name="supported-regions"></a>Regiones admitidas

[!INCLUDE [virtual-machines-disks-encryption-at-host-regions](virtual-machines-disks-encryption-at-host-regions.md)]

### <a name="supported-vm-sizes"></a>Tamaños de máquinas virtuales que se admiten

[!INCLUDE [virtual-machines-disks-encryption-at-host-suported-sizes](virtual-machines-disks-encryption-at-host-suported-sizes.md)]

## <a name="prerequisites"></a>Requisitos previos

Para poder usar el cifrado en el host para las máquinas virtuales o los conjuntos de escalado de máquinas virtuales, debe habilitar la característica en la suscripción. Envíe un correo electrónico a encryptionAtHost@microsoft. con los identificadores de suscripción para que la característica se habilite para sus suscripciones.

Inicie sesión en Azure Portal con el [vínculo proporcionado](https://aka.ms/diskencryptionupdates).

> [!IMPORTANT]
> Debe usar el [vínculo proporcionado](https://aka.ms/diskencryptionupdates) para tener acceso a Azure Portal. El cifrado en el host no está visible actualmente en Azure Portal público sin usar el vínculo.

### <a name="create-an-azure-key-vault-and-disk-encryption-set"></a>Creación de un almacén de Azure Key Vault y conjunto de cifrado de disco

Una vez habilitada la característica, deberá configurar un almacén de Azure Key Vault y un conjunto de cifrado de disco, si no lo ha hecho aún.

[!INCLUDE [virtual-machines-disks-encryption-create-key-vault-portal](virtual-machines-disks-encryption-create-key-vault-portal.md)]

## <a name="deploy-a-vm"></a>Implementación de una máquina virtual

Debe implementar una máquina virtual nueva para habilitar el cifrado en el host, no se puede habilitar en las máquinas virtuales existentes.

1. Busque **Máquinas virtuales** y seleccione **+Agregar** para crear una VM.
1. Cree una máquina virtual nueva, seleccione una región adecuada y un tamaño de máquina virtual compatible.
1. Rellene los otros valores de la hoja **Básico** como desee y, luego, vaya a la hoja **Discos**.

    :::image type="content" source="media/virtual-machines-disks-encryption-at-host-portal/disks-encryption-at-host-basic-blade.png" alt-text="Captura de pantalla de la hoja Básico de la creación de máquinas virtuales, se resaltan la región y el tamaño de la máquina virtual.":::

1. En la hoja **Discos**, seleccione **Sí** para **Cifrado en el host**.
1. Realice las selecciones restantes como desee.

    :::image type="content" source="media/virtual-machines-disks-encryption-at-host-portal/disks-encryption-at-host-disk-blade.png" alt-text="Captura de pantalla de la hoja Discos de la creación de máquinas virtuales, se resalta el cifrado en el host.":::

1. Finalice el proceso de implementación de la máquina virtual y realice las selecciones que se adapten a su entorno.

Ahora que ha implementado una máquina virtual con cifrado en el host habilitado, todos sus discos asociados se cifrarán con el cifrado en el host.