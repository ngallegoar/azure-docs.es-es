---
title: Automatización de la migración de máquinas en Azure Migrate
description: Describe cómo usar scripts para migrar un gran número de máquinas en Azure Migrate
author: anvar-ms
ms.author: anvar
ms.manager: bsiva
ms.topic: how-to
ms.date: 04/01/2019
ms.openlocfilehash: 01196b23ce5bf9ce842da89d0773a77e6a1d5107
ms.sourcegitcommit: ea551dad8d870ddcc0fee4423026f51bf4532e19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2020
ms.locfileid: "96754239"
---
# <a name="scale-migration-of-vms"></a>Migración de VM a escala 

Este artículo le ayuda a comprender cómo usar scripts para migrar un gran número de máquinas virtuales (VM). Para la migración a escala, use [Azure Site Recovery](../site-recovery/site-recovery-overview.md). 

Los scripts de Site Recovery están disponibles para su descarga en el repositorio de [ejemplos de Azure PowerShell](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/migrate-at-scale-with-site-recovery) en GitHub. Los scripts pueden usarse para migrar VMware, AWS, máquinas virtuales de GCP y servidores físicos a discos administrados en Azure. También puede usar estos scripts para migrar máquinas virtuales de Hyper-V si va a migrar las máquinas virtuales como servidores físicos. Los scripts que aprovechan PowerShell en Azure Site Recovery, se documentan [aquí](../site-recovery/vmware-azure-disaster-recovery-powershell.md).

## <a name="current-limitations"></a>Limitaciones actuales
- Compatibilidad de la especificación de la dirección IP estática solo para la NIC principal de la máquina virtual de destino
- Los scripts no toman las entradas relacionadas con la Ventaja híbrida de Azure, tendrá que actualizar manualmente las propiedades de la VM replicada en el portal

## <a name="how-does-it-work"></a>¿Cómo funciona?

### <a name="prerequisites"></a>Prerequisites
Antes de comenzar, siga estos pasos:
- Asegúrese de que se ha creado el almacén de Site Recovery en su suscripción de Azure
- Asegúrese de que el servidor de configuración y el servidor de procesos están instalados en el entorno de origen y el almacén es capaz de detectar el entorno
- Asegúrese de que se crea una directiva de replicación y de que se asocia al servidor de configuración
- Asegúrese de que ha agregado la cuenta de administrador de la máquina virtual al servidor de configuración (que se usará para replicar las máquinas virtuales de un entorno local)
- Asegúrese de que se crean los artefactos de destino en Azure
    - Grupo de recursos de destino
    - Cuenta de almacenamiento de destino (y su grupo de recursos): cree una cuenta de almacenamiento prémium si pretende migrar a discos administrados prémium
    - Cuenta de almacenamiento en caché (y su grupo de recursos): cree una cuenta de almacenamiento estándar en la misma región que el almacén
    - Red virtual de destino para la conmutación por error (y su grupo de recursos)
    - Subred de destino
    - Red virtual de destino para la conmutación por error de prueba (y su grupo de recursos)
    - Conjunto de disponibilidad (si se necesita)
    - Grupo de seguridad de red de destino y su grupo de recursos
- Asegúrese de que ha decidido las propiedades de la máquina virtual de destino
    - Nombre de máquina virtual de destino
    - Tamaño de la máquina virtual de destino en Azure (se puede decidir mediante la evaluación de Azure Migrate)
    - Dirección de IP privada de la NIC principal en la máquina virtual
- Descarga de los scripts de los [ejemplos de Azure PowerShell](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/migrate-at-scale-with-site-recovery) en GitHub

### <a name="csv-input-file"></a>Archivo de entrada CSV
Una vez que haya completado todos los requisitos previos, deberá crear un archivo CSV que contenga datos para cada máquina de origen que desea migrar. El archivo CSV de entrada debe tener una línea de encabezado con los detalles de entrada y una fila con los detalles de cada máquina que se debe migrar. Todos los scripts están diseñados para funcionar en el mismo archivo CSV. Hay una plantilla de CSV de ejemplo disponible en la carpeta de scripts para su referencia.

### <a name="script-execution"></a>Ejecución de script
Una vez que el archivo CSV está listo, puede ejecutar los pasos siguientes para realizar la migración de las máquinas virtuales locales:

**Núm. de paso** | **Nombre del script** | **Descripción**
--- | --- | ---
1 | asr_startmigration.ps1 | Habilite la replicación para todas las máquinas virtuales que se muestran en el archivo .csv, el script crea una salida .csv con los detalles del trabajo para cada máquina virtual.
2 | asr_replicationstatus.ps1 | Compruebe el estado de replicación, el script crea un archivo .csv con el estado de cada máquina virtual.
3 | asr_updateproperties.ps1 | Una vez que las máquinas virtuales se replican/protegen, use este script para actualizar las propiedades de destino de la máquina virtual (propiedades de proceso y red).
4 | asr_propertiescheck.ps1 | Compruebe si las propiedades se actualizan correctamente.
5 | asr_testmigration.ps1 |  Inicie la conmutación por error de prueba para las máquinas virtuales que se muestran en el archivo .csv, el script crea una salida .csv con los detalles del trabajo para cada máquina virtual.
6 | asr_cleanuptestmigration.ps1 | Una vez que valide manualmente las máquinas virtuales que realizaron la conmutación por error de prueba, puede usar este script para limpiar las máquinas virtuales de conmutación por error de prueba.
7 | asr_migration.ps1 | Realice una conmutación por error no planeada para las máquinas virtuales que se muestran en el archivo .csv, el script crea una salida .csv con los detalles del trabajo para cada máquina virtual. El script no apaga las máquinas virtuales de un entorno local antes de desencadenar la conmutación por error; por motivos de coherencia de la aplicación, se recomienda que apague manualmente las máquinas virtuales antes de ejecutar el script.
8 | asr_completemigration.ps1 | Realice la operación de confirmación en las máquinas virtuales y elimine las entidades de Azure Site Recovery.
9 | asr_postmigration.ps1 | Si tiene previsto asignar grupos de seguridad de red a las NIC después de la conmutación por error, puede usar este script para hacerlo. Asigna un NSG a una NIC en la máquina virtual de destino.

## <a name="how-to-migrate-to-managed-disks"></a>¿Cómo migrar discos administrado?
De forma predeterminada, el script migra las máquinas virtuales a los discos administrados en Azure. Si la cuenta de almacenamiento de destino proporcionada es una cuenta de almacenamiento prémium, los discos administrados prémium se crean después de la migración. La cuenta de almacenamiento en caché aún puede ser una cuenta estándar. Si la cuenta de almacenamiento de destino es una cuenta de almacenamiento estándar, los discos estándar se crean después de la migración. 

## <a name="next-steps"></a>Pasos siguientes

[Obtenga más información](../site-recovery/migrate-tutorial-on-premises-azure.md) sobre cómo migrar servidores a Azure mediante Azure Site Recovery.
