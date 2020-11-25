---
title: 'Azure Security Benchmark V2: copia de seguridad y recuperación'
description: Copia de seguridad y recuperación de Azure Security Benchmark V2
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/20/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 1ed477c6cf069dbb402e5bda9c1f48f7bc208f18
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94698808"
---
# <a name="security-control-v2-backup-and-recovery"></a>Control de seguridad V2: Copia de seguridad y recuperación

La copia de seguridad y la recuperación conllevan controles para garantizar que las copias de seguridad de los datos y la configuración de los distintos niveles de servicio se realizan, se validan y se protegen.

## <a name="br-1-ensure-regular-automated-backups"></a>BR-1: Garantía de copias de seguridad automáticas periódicas

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| BR-1 | 10.1 | CP-2, CP4, CP-6, CP-9 |

Asegúrese de realizar copias de seguridad de sistemas y datos para mantener la continuidad empresarial después de un evento inesperado. Debería definirse por los objetivos del objetivo de punto de recuperación (RPO) y el objetivo de tiempo de recuperación (RTO).

Habilite Azure Backup y configure el origen de las copias de seguridad (por ejemplo, máquinas virtuales de Azure, SQL Server, bases de datos de HANA o recursos compartidos de archivos), así como la frecuencia y el período de retención deseados.  

Para un nivel más alto de protección, puede habilitar la opción de almacenamiento con redundancia geográfica para replicar los datos de copia de seguridad en una región secundaria y recuperarlos mediante la restauración entre regiones.

- [Continuidad empresarial y recuperación ante desastres](/azure/cloud-adoption-framework/ready/enterprise-scale/business-continuity-and-disaster-recovery)

- [Habilitación de Azure Backup](../../backup/index.yml)

- [Habilitación de la restauración entre regiones](../../backup/backup-azure-arm-restore-vms.md#cross-region-restore)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Directivas y estándares](/azure/cloud-adoption-framework/organize/cloud-security-policy-standards)

- [Arquitectura de seguridad](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Preparación de incidentes](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

## <a name="br-2-encrypt-backup-data"></a>BR-2: Cifrado de los datos de copia de seguridad

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| BR-2 | 10,2 | CP-9 |

Asegúrese de que las copias de seguridad están protegidas frente a ataques. Esto debe incluir el cifrado de las copias de seguridad para proteger frente a la pérdida de confidencialidad.   

En el caso de las copias de seguridad locales mediante Azure Backup, se proporciona cifrado en reposo mediante la frase de contraseña que proporcione. En el caso de las copias de seguridad periódicas de servicios de Azure, los datos de copia de seguridad se cifran automáticamente mediante claves administradas por la plataforma de Azure. Puede optar por cifrar las copias de seguridad mediante claves administradas por el cliente. En este caso, asegúrese de que esta clave administrada por el cliente del almacén de claves también esté en el ámbito de la copia de seguridad. 

Use el control de acceso basado en rol de Azure en Azure Backup, Azure Key Vault u otros recursos para proteger las copias de seguridad y las claves administradas por el cliente. Además, puede habilitar características de seguridad avanzadas para requerir MFA antes de modificar o eliminar copias de seguridad.

- [Introducción a las características de seguridad de Azure Backup](../../backup/security-overview.md)

- [Cifrado de datos de copia de seguridad mediante claves administradas por el cliente](../../backup/encryption-at-rest-with-cmk.md) 

- [Copias de seguridad de las claves de Key Vault en Azure](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

- [Características de seguridad que ayudan a proteger las copias de seguridad híbridas frente a ataques](../../backup/backup-azure-security-feature.md#prevent-attacks)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Arquitectura de seguridad](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Preparación de incidentes](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

## <a name="br-3-validate-all-backups-including-customer-managed-keys"></a>BR-3: Validación de todas las copias de seguridad, incluidas las claves administradas por el cliente

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| BR-3 | 10,3 | CP-4, CP-9 |

Realice periódicamente la restauración de los datos de la copia de seguridad. Asegúrese de que puede restaurar las claves administradas por el cliente de las que se hizo una copia de seguridad.

- [Recuperación de archivos desde una copia de seguridad de máquina virtual de Azure](../../backup/backup-azure-restore-files-from-vm.md)

- [Restauración de las claves de Key Vault en Azure](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Preparación de incidentes](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Administración del cumplimiento de la seguridad](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="br-4-mitigate-risk-of-lost-keys"></a>BR-4: Mitigación del riesgo de pérdida de claves

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| BR-4 | 10,4 | CP-9 |

Asegúrese de aplicar medidas para evitar la pérdida de claves y para recuperarse de ella. Habilite la eliminación temporal y la protección de purga de Azure Key Vault para proteger las claves frente a una eliminación accidental o malintencionada.  

- [Procedimiento para habilitar la eliminación temporal y la protección de purga en Key Vault](../../storage/blobs/soft-delete-blob-overview.md?tabs=azure-portal)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Arquitectura de seguridad](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Preparación de incidentes](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Seguridad de los datos](/azure/cloud-adoption-framework/organize/cloud-security-data-security)