---
title: Creación y cifrado de un conjunto de escalado de máquinas virtuales con plantillas de Azure Resource Manager
description: En este inicio rápido, aprenderá a usar las plantillas de Azure Resource Manager para crear y cifrar un conjunto de escalado de máquinas virtuales
author: msmbaldwin
ms.author: mbaldwin
ms.service: virtual-machine-scale-sets
ms.topic: quickstart
ms.date: 10/10/2019
ms.openlocfilehash: aeba9aee8dd10b1245fb0a1e1cc98a1951382c72
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "72530943"
---
# <a name="encrypt-virtual-machine-scale-sets-with-azure-resource-manager"></a>Cifrado de conjuntos de escalado de máquinas virtuales con Azure Resource Manager

Mediante las plantillas de Azure Resource Manager es posible cifrar o descifrar conjunto de escalado de máquinas virtuales de Linux.

## <a name="deploying-templates"></a>Implementación de plantillas

En primer lugar, seleccione la plantilla que se ajuste a su escenario.

- [Habilitar el cifrado de disco en un conjunto de escalado de máquinas virtuales Linux en ejecución](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-linux)

- [Habilitar el cifrado de disco en un conjunto de escalado de máquinas virtuales Windows en ejecución](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-windows)

  - [Implementación de un conjunto de escalado de máquinas virtuales Linux con un JumpBox y habilitación del cifrado en los conjuntos de escalado de máquinas virtuales Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox)

  - [Implementación de un conjunto de escalado de máquinas virtuales Windows con un JumpBox y habilitación del cifrado en los conjuntos de escalado de máquinas virtuales Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox)

- [Deshabilitar el cifrado de disco en un conjunto de escalado de máquinas virtuales Linux en ejecución](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux)

- [Deshabilitar el cifrado de disco en un conjunto de escalado de máquinas virtuales Windows en ejecución](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-windows)

A continuación, siga estos pasos:

     1. Click **Deploy to Azure**.
     2. Fill in the required fields then agree to the terms and conditions.
     3. Click **Purchase** to deploy the template.

## <a name="next-steps"></a>Pasos siguientes

- [Azure Disk Encryption para conjuntos de escalado de máquinas virtuales](disk-encryption-overview.md)
- [Cifrado de conjuntos de escalado de máquinas virtuales mediante la CLI de Azure](disk-encryption-cli.md)
- [Cifrado de conjuntos de escalado de máquinas virtuales mediante Azure PowerShell](disk-encryption-powershell.md)
- [Creación y configuración de un almacén de claves para Azure Disk Encryption](disk-encryption-key-vault.md)
- [Uso de Azure Disk Encryption con la secuenciación de extensiones de un conjunto de escalado de máquinas virtuales](disk-encryption-extension-sequencing.md)