---
title: 'Control de seguridad de Azure: recuperación de datos'
description: Recuperación de datos del control de seguridad
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: c585ebd903d4070f6247456e06efffbc6ec45270
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934506"
---
# <a name="security-control-data-recovery"></a>Control de seguridad: Recuperación de datos

Asegúrese de que se realiza una copia de seguridad de todos los datos, configuraciones y secretos del sistema de forma periódica.

## <a name="91-ensure-regular-automated-back-ups"></a>9.1: Garantía de copias de seguridad automáticas periódicas

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 9.1 | 10.1 | Customer |

Habilite Azure Backup y configure el origen de la copia de seguridad (máquinas virtuales de Azure, SQL Server o recursos compartidos de archivos), así como la frecuencia y el período de retención deseados.

Habilitación de Azure Backup:

https://docs.microsoft.com/azure/backup/

## <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Realización de copias de seguridad completas del sistema y copia de seguridad de las claves administradas por el cliente

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 9.2 | 10.2 | Customer |

Habilite Azure Backup y las máquinas virtuales de destino, así como la frecuencia y los períodos de retención deseados. Realice una copia de seguridad de las claves administradas del cliente con Azure Key Vault.

Habilitación de Azure Backup:

https://docs.microsoft.com/azure/backup/

Realización de una copia de seguridad de las claves del almacén de claves en Azure:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0

## <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Validación de todas las copias de seguridad, incluidas las claves administradas por el cliente

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 9.3 | 10,3 | Customer |

Garantice la capacidad de realizar la restauración de datos de contenido en Azure Backup de forma periódica. Si es necesario, pruebe la restauración en una VLAN aislada. Pruebe la restauración de las claves administradas por el cliente de la copia de seguridad.

Recuperación de archivos desde una copia de seguridad de máquina virtual de Azure:

https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm

Restauración de las claves del almacén de claves en Azure:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0

## <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Garantía de la protección de las copias de seguridad y las claves administradas del cliente

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 9,4 | 10,4 | Customer |

Para la copia de seguridad local, el cifrado en reposo se proporciona con la frase de contraseña que proporcione durante la copia de seguridad en Azure. Para máquinas virtuales de Azure, los datos están cifrados en reposo con Storage Service Encryption (SSE). Puede habilitar la eliminación temporal en Key Vault para proteger las claves contra la eliminación accidental o malintencionada.

Habilitación de la eliminación temporal en Key Vault:

https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal

## <a name="next-steps"></a>Pasos siguientes

Vea el siguiente control de seguridad: [Respuesta a los incidentes](security-control-incident-response.md)
