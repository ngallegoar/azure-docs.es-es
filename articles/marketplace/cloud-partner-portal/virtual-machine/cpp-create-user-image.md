---
title: Creación de una imagen de VM de usuario para Azure Marketplace
description: Se enumeran los pasos y las referencias necesarios para crear una imagen de VM de usuario.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: dsindona
ms.openlocfilehash: 49db8c6717cd26886c3b49f8c99fdd2b08e8713d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80278014"
---
# <a name="create-a-user-vm-image"></a>Crear una imagen de máquina virtual de usuario

En este artículo se explican los dos pasos generales necesarios para crear una imagen no administrada desde un VHD generalizado.  Se proporcionan referencias para guiarle a través de cada paso: capturar la imagen y generalizar la imagen.


## <a name="capture-the-vm-image"></a>Capturar la imagen de máquina virtual

Siga las instrucciones del artículo siguiente sobre cómo capturar la máquina virtual que correspondan con el método de acceso:

-  PowerShell: [Captura de una imagen administrada de una máquina virtual generalizada en Azure](../../../virtual-machines/windows/capture-image-resource.md)
-  CLI de Azure: [Creación de una imagen de una máquina virtual o un disco duro virtual](../../../virtual-machines/linux/capture-image.md)
-  API: [Virtual Machines - Capture](https://docs.microsoft.com/rest/api/compute/virtualmachines/capture) (Máquinas virtuales: Capture)


## <a name="generalize-the-vm-image"></a>Generalizar la imagen de máquina virtual

Dado que ha generado la imagen de usuario a partir de un VHD generalizado anteriormente, también debería estar generalizada.  Una vez más, seleccione el siguiente artículo que corresponda con el mecanismo de acceso.  (Es posible que ya hubiera generalizado el disco cuando lo capturó).

-  PowerShell: [Generalización de la máquina virtual](https://docs.microsoft.com/azure/virtual-machines/windows/sa-copy-generalized#generalize-the-vm)
-  CLI de Azure: [Paso 2: Crear la imagen de máquina virtual](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image#step-2-create-vm-image)
-  API: [Virtual Machines - Generalize](https://docs.microsoft.com/rest/api/compute/virtualmachines/generalize) (Máquinas virtuales: Generalize)


## <a name="next-steps"></a>Pasos siguientes

A continuación podrá [crear un certificado](cpp-create-key-vault-cert.md) y almacenarlo en un nuevo Azure Key Vault.  Este certificado es necesario para establecer una conexión WinRM segura a la VM.
