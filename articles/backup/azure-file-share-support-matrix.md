---
title: Matriz de compatibilidad de copia de seguridad de recursos compartidos de archivos de Azure
description: Proporciona un resumen de opciones y limitaciones de compatibilidad a la hora de realizar copias de seguridad de recursos compartidos de archivos de Azure.
ms.topic: conceptual
ms.date: 5/07/2020
ms.custom: references_regions
ms.openlocfilehash: 8198c06c1151dd86d99bc64f825764d0a4988287
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84193865"
---
# <a name="support-matrix-for-azure-file-share-backup"></a>Matriz de compatibilidad de copia de seguridad de recursos compartidos de archivos de Azure

Puede usar el [servicio Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview) para realizar copias de seguridad de recursos compartidos de archivos de Azure. En este artículo se resumen las opciones de compatibilidad a la hora de realizar copias de seguridad de recursos compartidos de archivos de Azure con Azure Backup.

## <a name="supported-regions"></a>Regiones admitidas

### <a name="ga-regions-for-azure-file-shares-backup"></a>Regiones con disponibilidad general para las copias de seguridad de recursos compartidos de archivos de Azure

La copia de seguridad de recursos compartidos de archivos de Azure está disponible en todas las regiones **salvo** las siguientes: Centro de Alemania (soberana), Nordeste de Alemania (soberana), Este de China, Este de China 2, Norte de China, Norte de China 2, US Gov Iowa

### <a name="supported-regions-for-accidental-delete-protection"></a>Regiones admitidas para la protección contra eliminación accidental

Centro-Oeste de EE. UU.

## <a name="supported-storage-accounts"></a>Cuentas de almacenamiento admitidas

| Detalles de la cuenta de almacenamiento | Soporte técnico                                                      |
| ------------------------ | ------------------------------------------------------------ |
| Tipo de cuenta            | Azure Backup admite recursos compartidos de archivos de Azure presentes en las cuentas de almacenamiento de tipo de uso general v1, v2 y de almacenamiento de archivos |
| Rendimiento              | Azure Backup admite recursos compartidos de archivos tanto de cuentas estándar como Premium Storage |
| Replicación              | Se admiten recursos compartidos de archivos de Azure de cuentas de almacenamiento con cualquier tipo de replicación |

## <a name="supported-file-shares"></a>Recursos compartidos de archivos admitidos

| Tipo de recurso compartido de archivos                                   | Soporte técnico   |
| -------------------------------------------------- | --------- |
| Estándar                                           | Compatible |
| grande                                              | Compatible |
| Premium                                            | Compatible |
| Recursos compartidos de archivos conectados con el servicio Azure File Sync | Compatible |

## <a name="protection-limits"></a>Límites de protección

| Configuración                                                      | Límite |
| ------------------------------------------------------------ | ----- |
| Número máximo de recursos compartidos de archivos que se pueden proteger al día por almacén | 200   |
| Número máximo de cuentas de almacenamiento que se pueden registrar por almacén al día | 50    |

## <a name="backup-limits"></a>Límites de Backup

| Configuración                                      | Límite |
| -------------------------------------------- | ----- |
| Número máximo de copias de seguridad a petición al día | 4     |
| Número máximo de copias de seguridad programadas al día | 1     |

## <a name="restore-limits"></a>Límites de restauración

| Configuración                                                      | Límite   |
| ------------------------------------------------------------ | ------- |
| Número máximo de restauraciones al día                           | 10      |
| Número máximo de archivos por restauración                         | 10      |
| Tamaño de restauración máximo recomendado por restauración para recursos compartidos de archivos de gran tamaño | 15 TiB |

## <a name="retention-limits"></a>Límites de retención

| Configuración                                                      | Límite    |
| ------------------------------------------------------------ | -------- |
| Puntos de recuperación totales máximos por recurso compartido de archivos en cualquier momento dado | 200      |
| Retención máxima de punto de recuperación creado por una copia de seguridad a petición | 10 años |
| Retención máxima de puntos de recuperación diarios (instantáneas) por recurso compartido de archivos| 200 días |
| Retención máxima de puntos de recuperación semanales (instantáneas) por recurso compartido de archivos | 200 semanas |
| Retención máxima de puntos de recuperación mensuales (instantáneas) por recurso compartido de archivos | 120 meses |
| Retención máxima de puntos de recuperación anuales (instantáneas) por recurso compartido de archivos | 10 años |

## <a name="supported-restore-methods"></a>Métodos de restauración admitidos

| Método de restauración     | Detalles                                                      |
| ------------------ | ------------------------------------------------------------ |
| Recuperación del recurso compartido completo | Puede restaurar un recurso compartido de archivos completo en la ubicación original o en una alternativa |
| Restauración en el nivel de elemento | Puede restaurar archivos y carpetas individuales en la ubicación original o en una alternativa |

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre cómo [realizar copias de seguridad de recursos compartidos de archivos de Azure](backup-afs.md).
* Más información sobre cómo [restaurar recursos compartidos de archivos de Azure](restore-afs.md)
* Más información sobre cómo [administrar copias de seguridad de recursos compartidos de archivos de Azure](manage-afs-backup.md)
