---
title: Traslado de máquinas virtuales de Azure a una nueva suscripción o grupo de recursos
description: Use Azure Resource Manager para trasladar máquinas virtuales a un nuevo grupo de recursos o a una nueva suscripción.
ms.topic: conceptual
ms.date: 03/31/2020
ms.openlocfilehash: df34268b7741f76621c290e9979cf24d828ddc09
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478671"
---
# <a name="move-guidance-for-virtual-machines"></a>Guía del traslado de máquinas virtuales

En este artículo se describen los escenarios que actualmente no se admiten y los pasos para trasladar máquinas virtuales con copia de seguridad.

## <a name="scenarios-not-supported"></a>Escenarios no admitidos

Todavía no se admiten los siguientes escenarios:

* Las instancias de Managed Disks en Availability Zones no se pueden trasladar a una suscripción diferente.
* No es posible trasladar Virtual Machine Scale Sets con equilibrador de carga o IP pública de SKU estándar.
* Las máquinas virtuales creadas a partir de recursos de Marketplace con planes adjuntos no se pueden mover entre suscripciones o grupos de recursos. Desaprovisione el recurso en la suscripción activa y vuelva a implementarlo en la nueva suscripción.
* Las máquinas virtuales de una red virtual existente no se pueden mover a una suscripción nueva si no se van a mover todos los recursos de la red virtual.
* Las máquinas virtuales de prioridad baja y los conjuntos de escalado de máquinas virtuales de prioridad baja no pueden moverse entre grupos de recursos o suscripciones.
* Las máquinas virtuales de un conjunto de disponibilidad no se pueden mover individualmente.

## <a name="virtual-machines-with-azure-backup"></a>Máquinas virtuales con Azure Backup

Para trasladar las máquinas virtuales configuradas con Azure Backup, debe eliminar los puntos de restauración del almacén.

Si la [eliminación temporal](../../../backup/backup-azure-security-feature-cloud.md) está habilitada para la máquina virtual, no podrá moverla mientras se conserven esos puntos de restauración. [Deshabilite la eliminación temporal](../../../backup/backup-azure-security-feature-cloud.md#disabling-soft-delete) o espere 14 días después de eliminar los puntos de restauración.

### <a name="portal"></a>Portal

1. Seleccione la máquina virtual que está configurada para la copia de seguridad.

1. En el panel izquierdo, seleccione **Copia de seguridad**.

1. Seleccione **Detener copia de seguridad**.

1. Seleccione **Delete back data** (Eliminar datos de copia de seguridad).

1. Después de finalizar la eliminación, puede trasladar el almacén y la máquina virtual a la suscripción de destino. Tras el traslado, puede continuar realizando las copias de seguridad.

### <a name="powershell"></a>PowerShell

* Busque la ubicación de la máquina virtual.
* Busque un grupo de recursos con el patrón de nombres siguiente: `AzureBackupRG_<location of your VM>_1`, por ejemplo, AzureBackupRG_westus2_1
* Si se encuentra en PowerShell, use el cmdlet `Get-AzResource -ResourceGroupName AzureBackupRG_<location of your VM>_1`
* Busque el recurso con el tipo `Microsoft.Compute/restorePointCollections` que tiene el patrón de nombres `AzureBackup_<name of your VM that you're trying to move>_###########`
* Elimine este recurso. Esta operación elimina solo los puntos de recuperación instantáneos, no los datos de copia de seguridad que se encuentran en el almacén.

### <a name="azure-cli"></a>Azure CLI

* Busque la ubicación de la máquina virtual.
* Busque un grupo de recursos con el patrón de nombres siguiente: `AzureBackupRG_<location of your VM>_1`, por ejemplo, AzureBackupRG_westus2_1
* Si está en la CLI, use `az resource list -g AzureBackupRG_<location of your VM>_1`
* Busque el recurso con el tipo `Microsoft.Compute/restorePointCollections` que tiene el patrón de nombres `AzureBackup_<name of your VM that you're trying to move>_###########`
* Elimine este recurso. Esta operación elimina solo los puntos de recuperación instantáneos, no los datos de copia de seguridad que se encuentran en el almacén.

## <a name="next-steps"></a>Pasos siguientes

* Para ver los comandos para trasladar recursos, vea [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](../move-resource-group-and-subscription.md).

* Para más información sobre cómo mover los almacenes de Recovery Services para realizar copias de seguridad, consulte [Limitaciones de Recovery Services](../../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).
