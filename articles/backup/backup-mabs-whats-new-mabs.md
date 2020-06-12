---
title: Novedades de Microsoft Azure Backup Server
description: Microsoft Azure Backup Server proporciona funcionalidades mejoradas de copia de seguridad para proteger máquinas virtuales, archivos y carpetas, cargas de trabajo, etc. Aprenda a instalar o actualizar Azure Backup Server V3.
ms.topic: conceptual
ms.date: 11/13/2018
ms.openlocfilehash: 358de6f2986036621accde90550e2afcc47f9135
ms.sourcegitcommit: 8017209cc9d8a825cc404df852c8dc02f74d584b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2020
ms.locfileid: "84247553"
---
# <a name="whats-new-in-microsoft-azure-backup-server"></a>Novedades de Microsoft Azure Backup Server

Microsoft Azure Backup Server versión 3 (V3 MABS) es la actualización más reciente e incluye correcciones de errores críticos, compatibilidad con Windows Server 2019, compatibilidad con SQL 2017 y otras características y mejoras. Para ver la lista de errores corregidos y las instrucciones de instalación de MABS V3, consulte el artículo de KB [4457852](https://support.microsoft.com/help/4457852/microsoft-azure-backup-server-v3).

Las siguientes características se incluyen en MABS V3:

## <a name="volume-to-volume-migration"></a>Migración de volumen a volumen

Con Modern Backup Storage (MBS) en MABS V2, anunciábamos almacenamiento con reconocimiento de la carga de trabajo, donde se configuran determinadas cargas de trabajo para la copia de seguridad en almacenamiento específico, según las propiedades de almacenamiento. Sin embargo, después de la configuración, puede que tenga la necesidad de mover copias de seguridad de algunos orígenes de datos a otro almacenamiento para la utilización optimizada de los recursos. MABS V3 le ofrece la funcionalidad de migrar las copias de seguridad y configurarlas para su almacenamiento en un volumen diferente en [tres pasos](https://techcommunity.microsoft.com/t5/system-center-blog/sc-2016-dpm-ur4-migrate-backup-storage-in-3-simple-steps/ba-p/351842).

## <a name="prevent-unexpected-data-loss"></a>Evitar la pérdida inesperada de datos

En las empresas, un equipo de administradores administra MABS. Aunque existen directrices sobre el almacenamiento que debe usarse para las copias de seguridad, el hecho de dar a MABS un volumen incorrecto como almacenamiento de copia de seguridad puede dar lugar a la pérdida de datos críticos. Con MABS V3, puede evitar tales situaciones si configura esos volúmenes como los que no están disponibles para el almacenamiento con [estos cmdlets de PowerShell](https://docs.microsoft.com/azure/backup/backup-mabs-add-storage).

## <a name="custom-size-allocation"></a>Asignación de tamaño personalizada

Modern Backup Storage (MBS) consume almacenamiento fino, como y cuando sea necesario. Para ello, MABS calcula el tamaño de los datos de la copia de seguridad cuando se configura para la protección. Sin embargo, si se realiza la copia de seguridad de muchos archivos y carpetas juntos, como en el caso de un servidor de archivos, el cálculo del tamaño puede llevar mucho tiempo. Con MABS V3, puede configurar MABS para aceptar el tamaño del volumen como valor predeterminado, en lugar de calcular el tamaño de cada archivo, lo que ahorra tiempo.

## <a name="optimized-cc-for-rct-vms"></a>CC optimizado para máquinas virtuales RCT

MABS usa RCT (el seguimiento de cambios nativo de Hyper-V), lo que reduce la necesidad de laboriosas comprobaciones de coherencia en situaciones donde la máquina virtual se bloquea. RCT proporciona una mejor resistencia que el seguimiento de cambios proporcionado por las copias de seguridad basadas en instantáneas de VSS. MABS V3 optimiza el consumo de red y almacenamiento aún más al transferir solo los datos cambiados durante las comprobaciones de coherencia.

## <a name="support-to-tls-12"></a>Compatibilidad con TLS 1.2

TLS 1.2 es el modo seguro de comunicación sugerido por Microsoft con el mejor cifrado de su clase. MABS admite ahora la comunicación TLS 1.2 entre MABS y los servidores protegidos, para la autenticación basada en certificados y las copias de seguridad en la nube.

## <a name="vmware-vm-protection-support"></a>Compatibilidad con la protección de máquinas virtuales de VMware

Ahora, se admite la copia de seguridad de máquinas virtuales de VMware para las implementaciones en producción. MABS V3 ofrece las siguientes características para la protección de máquinas virtuales de VMware:

- Compatibilidad con vCenter y ESXi 6.5, además de compatibilidad con 5.5 y 6.0.
- Protección automática de máquinas virtuales de VMware en la nube. Si se agregan nuevas máquinas virtuales de VMware a carpeta protegida, se protegen automáticamente en el disco y en la nube.
- Mejoras de la eficiencia de recuperación para la recuperación de ubicación alternativa de VMware.

## <a name="sql-2017-support"></a>Compatibilidad con SQL 2017

MABS V3 se puede instalar con SQL 2017 como base de datos de MABS. Puede actualizar el servidor de SQL Server de SQL 2016 a SQL 2017, o instalarlo de nuevo. También puede hacer una copia de seguridad de la carga de trabajo de SQL 2017 en entornos agrupados y no agrupados con MABS V3.

## <a name="windows-server-2019-support"></a>Compatibilidad con Windows Server 2019

MABS V3 se puede instalar en Windows Server 2019. Para usar MABS V3 con WS2019, puede actualizar el sistema operativo a WS2019 con anterioridad o posterioridad a la instalación de MABS V3 en WS2016 o la actualización a esta versión.

MABS V3 es una versión completa y se puede instalar directamente en Windows Server 2016 y Windows Server 2019, o se puede actualizar desde MABS V2. Antes de instalar Backup Server V3 o actualizar a esta versión, lea este artículo sobre los requisitos previos de instalación.
Puede encontrar más información sobre los pasos de instalación o actualización de MABS [aquí](https://docs.microsoft.com/azure/backup/backup-azure-microsoft-azure-backup#software-package).

> [!NOTE]
>
> MABS tiene la misma base de código que System Center Data Protection Manager. MABS v3 es equivalente a Data Protection Manager 1807.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo preparar el servidor o empezar a proteger la carga de trabajo:

- [Problemas conocidos de MABS V3](backup-mabs-release-notes-v3.md)
- [Preparar cargas de trabajo de Backup Server](backup-azure-microsoft-azure-backup.md)
- [Usar Backup Server para hacer una copia de seguridad de un servidor de VMware](backup-azure-backup-server-vmware.md)
- [Usar Backup Server para hacer una copia de seguridad de SQL Server](backup-azure-sql-mabs.md)
- [Usar Modern Backup Storage con Backup Server](backup-mabs-add-storage.md)
