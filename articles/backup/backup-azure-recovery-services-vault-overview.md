---
title: Introducción a los almacenes de Recovery Services
description: Información general y comparación entre los almacenes de Recovery Services y los de Azure Backup.
ms.topic: conceptual
ms.date: 08/10/2018
ms.openlocfilehash: 94a3e5a0865bcc8c0a9ecb866ca013f20a558e1a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78673237"
---
# <a name="recovery-services-vaults-overview"></a>Introducción a los almacenes de Recovery Services

En este artículo se describen las características de un almacén de Recovery Services. Un almacén de Recovery Services es una entidad de almacenamiento de Azure que aloja datos. Normalmente, los datos son copias de datos o información de configuración de máquinas virtuales (VM), cargas de trabajo, servidores o estaciones de trabajo. Puede usar almacenes de Recovery Services para almacenar datos de copia de seguridad de varios servicios de Azure como máquinas virtuales de IaaS (Linux o Windows) y bases de datos de Azure SQL. Los almacenes de Recovery Services son compatibles con System Center DPM, Windows Server, Azure Backup Server y muchos más. Los almacenes de Recovery Services facilitan la tarea de organizar los datos de copia de seguridad, al mismo tiempo que reduce al mínimo su sobrecarga administrativa.

Dentro de una suscripción de Azure, puede crear hasta 500 almacenes de Recovery Services por suscripción y región.

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Comparación de almacenes de Recovery Services y de Backup

Si sigue teniendo almacenes de Backup, estos se actualizarán automáticamente a almacenes de Recovery Services. En noviembre de 2017, todos los almacenes de Backup se habrán actualizado a almacenes de Recovery Services.

Los almacenes de Recovery Services se basan en el modelo de Azure Resource Manager, mientras que los almacenes de Backup se basan en el modelo de Azure Service Manager. Al actualizar un almacén de Backup a un almacén de Recovery Services, los datos de copia de seguridad permanecen intactos durante el proceso de actualización y después del mismo. Los almacenes de Recovery Services proporcionan características que no están disponibles para los almacenes de Backup, como:

- **Funcionalidades mejoradas para ayudar a proteger datos de copia de seguridad**: con los almacenes de Recovery Services, Azure Backup proporciona funcionalidades de seguridad para proteger las copias de seguridad en la nube. Estas características de seguridad garantizan que puede proteger las copias de seguridad y recuperar datos de forma segura, incluso si los servidores de producción y copia de seguridad están en peligro. [Más información](backup-azure-security-feature.md)

- **Supervisión central para el entorno de TI híbrido**: con los almacenes de Recovery Services, puede supervisar no solo sus [máquinas virtuales de IaaS de Azure](backup-azure-manage-vms.md), sino también sus [recursos locales](backup-azure-manage-windows-server.md#manage-backup-items) desde un portal central. [Más información](https://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **Control de acceso basado en rol (RBAC)** : RBAC permite un control muy detallado de la administración de acceso en Azure. [Azure proporciona diversas funciones integradas](../role-based-access-control/built-in-roles.md) y Azure Backup tiene tres [roles integrados para administrar puntos de recuperación](backup-rbac-rs-vault.md). Los almacenes de Recovery Services son compatibles con RBAC, que restringe el acceso de copia de seguridad y restauración al conjunto definido de roles de usuario. [Más información](backup-rbac-rs-vault.md)

- **Protección de todas las configuraciones de Azure Virtual Machines**: los almacenes de Recovery Services protegen las máquinas virtuales basadas en Resource Manager, incluidos discos Premium, Managed Disks y máquinas virtuales cifradas. La actualización de un almacén de Backup a un almacén de Recovery Services proporciona la oportunidad de actualizar máquinas virtuales basadas en Service Manager a máquinas virtuales basadas en Resource Manager. Al actualizar el almacén, puede conservar los puntos de recuperación de máquinas virtuales basadas en Service Manager y configurar la protección de las máquinas virtuales actualizadas (habilitadas para Resource Manager). [Más información](https://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **Restauración instantánea para máquinas virtuales de IaaS**: con los almacenes de Recovery Services, puede restaurar archivos y carpetas desde una máquina virtual de IaaS sin tener que restaurar toda la máquina virtual, lo que permite unos tiempos de restauración más rápidos. La restauración instantánea para máquinas virtuales de IaaS está disponible tanto para máquinas virtuales Windows como Linux. [Más información](backup-instant-restore-capability.md)

## <a name="managing-your-recovery-services-vaults-in-the-portal"></a>Administración de almacenes de Recovery Services en el portal

La creación y administración de almacenes de Recovery Services en Azure Portal es fácil porque el servicio Backup está integrado en otros servicios de Azure. Esta integración significa que se puede crear o administrar un almacén de Recovery Services *en el contexto del servicio de destino*. Por ejemplo, para ver los puntos de recuperación de una máquina virtual, selecciónela y haga clic en **Copia de seguridad** en el menú Operaciones.

![Máquinas virtuales de detalles de almacén de Recovery Services](./media/backup-azure-recovery-services-vault-overview/rs-vault-in-context-vm.png)

Si la máquina virtual no tiene una copia de seguridad configurada, se le pedirá que la configure. Si la copia de seguridad se ha configurado, verá información sobre la copia de seguridad de la máquina virtual, incluida una lista de puntos de restauración.  

![Máquinas virtuales de detalles de almacén de Recovery Services](./media/backup-azure-recovery-services-vault-overview/vm-recovery-point-list.png)

En el ejemplo anterior, **ContosoVM** es el nombre de la máquina virtual. **ContosoVM-demovault** es el nombre del almacén de Recovery Services. No es necesario que recuerde el nombre del almacén de Recovery Services que almacena los puntos de recuperación, ya que puede tener acceso a esta información en la máquina virtual.  

Si un almacén de Recovery Services protege varios servidores, puede ser más lógico mirar en el almacén de Recovery Services. Puede buscar todos los almacenes de Recovery Services de la suscripción y elegir uno de la lista.

Las secciones siguientes contienen vínculos a artículos que explican cómo usar un almacén de Recovery Services en cada tipo de actividad.

> [!NOTE]
> El almacén de Recovery Services no se puede crear con el mismo nombre si se ha eliminado en las últimas 24 horas. Utilice un nombre de recurso diferente, elija un grupo de recursos distinto o inténtelo de nuevo después de 24 horas.

### <a name="back-up-data"></a>Copia de seguridad de datos

- [Copia de seguridad de máquinas virtuales de Azure](backup-azure-vms-first-look-arm.md)
- [Copia de seguridad de Windows Server o Windows Workstation](backup-try-azure-backup-in-10-mins.md)
- [Preparación para la copia de seguridad de cargas de trabajo de DPM en Azure](backup-azure-dpm-introduction.md)
- [Preparación para la copia de seguridad de cargas de trabajo mediante Azure Backup Server](backup-azure-microsoft-azure-backup.md)

### <a name="manage-recovery-points"></a>Administración de puntos de recuperación

- [Administrar copias de seguridad de máquina virtual de Azure](backup-azure-manage-vms.md)
- [Administración de archivos y carpetas](backup-azure-manage-windows-server.md)

### <a name="restore-data-from-the-vault"></a>Restauración de datos desde el almacén

- [Recuperación de archivos de máquinas virtuales de Azure](backup-azure-restore-files-from-vm.md)
- [Restauración de máquinas virtuales de Azure](backup-azure-arm-restore-vms.md)

### <a name="secure-the-vault"></a>Seguridad del almacén

- [Seguridad de los datos de copia de seguridad en la nube en los almacenes de Recovery Services](backup-azure-security-feature.md)

## <a name="next-steps"></a>Pasos siguientes

Use los artículos siguientes para realizar estos pasos:</br>
[Copia de seguridad de una máquina virtual de IaaS](backup-azure-arm-vms-prepare.md)</br>
[Copia de seguridad de Azure Backup Server](backup-azure-microsoft-azure-backup.md)</br>
[Copia de seguridad de Windows Server](backup-windows-with-mars-agent.md)
