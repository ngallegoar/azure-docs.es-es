---
title: Habilitación de Azure Disk Encryption para conjuntos de escalado de máquinas virtuales
description: En este artículo se proporcionan instrucciones sobre cómo habilitar Microsoft Azure Disk Encryption para conjuntos de escalado de máquinas virtuales.
author: msmbaldwin
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.author: mbaldwin
ms.date: 10/10/2019
ms.custom: seodec18
ms.openlocfilehash: 36eb83d99af299260ebbbd74fe56d69921ba389b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "76278971"
---
# <a name="azure-disk-encryption-for-virtual-machine-scale-sets"></a>Azure Disk Encryption para conjuntos de escalado de máquinas virtuales.

Azure Disk Encryption proporciona cifrado de volumen para los discos de datos y del sistema operativo de las máquinas virtuales, lo que ayuda a proteger y salvaguardar los datos para satisfacer los compromisos de cumplimiento y seguridad de la organización. Para más información, consulte [Azure Disk Encryption: máquinas virtuales Linux](../virtual-machines/linux/disk-encryption-overview.md) y [Azure Disk Encryption: máquinas virtuales Windows](../virtual-machines/windows/disk-encryption-overview.md)  

Azure Disk Encryption también se pueden aplicar a los conjuntos de escalado de máquinas virtuales Windows y Linux, en estos casos:
- Conjuntos de escalado creados con discos administrados. Azure Disk Encryption no es compatible con los conjuntos de escalado con disco nativo (o no administrado).
- Volúmenes de datos y del sistema operativo en conjuntos de escalado de Windows.
- Volúmenes de datos en conjuntos de escalado de Linux. El cifrado de discos del sistema operativo NO se admite actualmente en los conjuntos de escalado de Linux.

Puede obtener información sobre los aspectos básicos de Azure Disk Encryption para conjuntos de escalado de máquinas virtuales en tan solo unos minutos con los tutoriales [Cifrado de conjuntos de escalado de máquinas virtuales mediante la CLI de Azure](disk-encryption-cli.md) o [Cifrado de conjuntos de escalado de máquinas virtuales mediante Azure PowerShellAzure](disk-encryption-powershell.md).

## <a name="next-steps"></a>Pasos siguientes

- [Cifrado de conjuntos de escalado de máquinas virtuales con Resource Manager](disk-encryption-azure-resource-manager.md)
- [Creación y configuración de un almacén de claves para Azure Disk Encryption](disk-encryption-key-vault.md)
- [Uso de Azure Disk Encryption con la secuenciación de extensiones de un conjunto de escalado de máquinas virtuales](disk-encryption-extension-sequencing.md)
