---
title: Automatización de las migraciones de VMware sin agente en Azure Migrate
description: Describe cómo usar scripts para migrar un gran número de máquinas virtuales de VMware en Azure Migrate
author: rahulg1190
ms.author: rahugup
ms.manager: bsiva
ms.topic: how-to
ms.date: 10/30/2020
ms.openlocfilehash: cdae1fe13f8e08cb6b817f8ec6431c77013020d7
ms.sourcegitcommit: ea551dad8d870ddcc0fee4423026f51bf4532e19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2020
ms.locfileid: "96754273"
---
# <a name="scale-migration-of-vmware-vms"></a>Migración de máquinas virtuales de VMware a gran escala 

Este artículo lo ayuda a comprender cómo usar scripts para migrar un gran número de máquinas virtuales (VM) de VMware con el método sin agente. Para escalar las migraciones, use el [módulo de PowerShell de Azure Migrate](./tutorial-migrate-vmware-powershell.md). 

Los scripts de automatización de la migración de VMware de Azure Migrate están disponibles para descargarlos en el repositorio de [ejemplos de Azure PowerShell](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/migrate-at-scale-vmware-agentles) en GitHub. Los scripts se pueden usar para migrar máquinas virtuales de VMware a Azure a través del método de migración sin agente. Los comandos de PowerShell para Azure Migrate que se usan en estos scripts están documentados [aquí](./tutorial-migrate-vmware-powershell.md).

## <a name="current-limitations"></a>Limitaciones actuales
- Estos scripts admiten la migración de máquinas virtuales de VMware con todos los discos. Puede actualizar los scripts si quiere replicar de manera selectiva los discos conectados a una máquina virtual de VMware. 
- Los scripts admiten el uso de recomendaciones de evaluación. Si no se usan recomendaciones de evaluación, todos los discos conectados a la máquina virtual de VMware se migran al mismo tipo de disco administrado (Estándar o Premium). Puede actualizar los scripts si quiere usar varios tipos de discos administrados con la misma máquina virtual.

## <a name="prerequisites"></a>Requisitos previos

- [Complete el tutorial de detección](tutorial-discover-vmware.md) para preparar Azure y VMware para la migración.
- Se recomienda completar el segundo tutorial para [evaluar las máquinas virtuales de VMware](./tutorial-assess-vmware-azure-vm.md) antes de migrarlas a Azure.
- Tiene el módulo `Az` de Azure PowerShell. Si necesita instalar o actualizar Azure PowerShell, siga esta [guía para instalar y configurar Azure PowerShell](/powershell/azure/install-az-ps).

## <a name="install-azure-migrate-powershell-module"></a>Instalación del módulo de PowerShell de Azure Migrate

El módulo de PowerShell de Azure Migrate está disponible en versión preliminar. Deberá instalar el módulo de PowerShell mediante el siguiente comando. 

```azurepowershell
Install-Module -Name Az.Migrate 
```

## <a name="csv-input-file"></a>Archivo de entrada CSV
Una vez que haya completado todos los requisitos previos, deberá crear un archivo .csv que contenga datos para cada máquina virtual de origen que quiere migrar. Todos los scripts están diseñados para funcionar en el mismo archivo CSV. Hay una plantilla de CSV de ejemplo disponible en la carpeta de scripts para su referencia. El archivo .csv se puede configurar para usar las recomendaciones de evaluación e, incluso, especificar si no se deben desencadenar determinadas operaciones para una máquina virtual específica. 

> [!NOTE]
> El mismo archivo .csv se puede usar para migrar máquinas virtuales en varios proyectos de Azure Migrate.

### <a name="csv-file-schema"></a>Esquema del archivo .csv

**Encabezado de columna** | **Descripción**
--- | ---
AZMIGRATEPROJECT_SUBSCRIPTION_ID | Indique el id. de suscripción del proyecto de Azure Migrate.
AZMIGRATEPROJECT_RESOURCE_GROUP_NAME | Indique el nombre del grupo de recursos de Azure Migrate.
AZMIGRATEPROJECT_NAME | Indique el nombre del proyecto de Azure Migrate en el que quiere migrar los servidores. 
SOURCE_MACHINE_NAME | Indique el nombre descriptivo (nombre para mostrar) de la máquina virtual detectada en el proyecto de Azure Migrate.
AZMIGRATEASSESSMENT_NAME | Indique el nombre de la evaluación que se debe aprovechar para la migración.
AZMIGRATEGROUP_NAME | Indique el nombre del grupo que se usó para la evaluación de Azure Migrate.
TARGET_RESOURCE_GROUP_NAME | Indique el nombre del grupo de recursos de Azure al que se debe migrar la máquina virtual.
TARGET_VNET_NAME| Indique el nombre de la red virtual de Azure que la máquina virtual migrada debe usar.
TARGET_SUBNET_NAME | Indique el nombre de la subred de la red virtual de destino que la máquina virtual migrada debe usar. Si se deja en blanco, se usará la subred "predeterminada".
TARGET_MACHINE_NAME | Indique el nombre que la máquina virtual migrada debe usar en Azure. Si se deja en blanco, se usará el nombre de la máquina de origen.  
TARGET_MACHINE_SIZE | Proporcione la SKU que la máquina virtual debe usar en Azure. Para migrar una máquina virtual a una máquina virtual D2_v2 de Azure, especifique el valor de este campo como "Standard_D2_v2". Si usa una evaluación, este valor se derivará en función de la recomendación de evaluación.
LICENSE_TYPE | Especifique si quiere usar Ventaja híbrida de Azure para máquinas virtuales de Windows Server. Use el valor "WindowsServer" para aprovechar la Ventaja híbrida de Azure. De lo contrario, déjelo en blanco o use "NoLicenseType".
OS_DISK_ID | Proporcione el identificador de disco del sistema operativo para la máquina virtual que se va a migrar. El identificador de disco que se va a usar es la propiedad de identificador único (UUID) del disco recuperado mediante el cmdlet Get-AzMigrateServer. El script usará el primer disco de la máquina virtual como disco del sistema operativo en caso de que no se proporcione ningún valor.
TARGET_DISKTYPE | Proporcione el tipo de disco que se va a usar para todos los discos de la máquina virtual en Azure. Use "Premium_LRS" para los discos administrados premium, "StandardSSD_LRS" para los discos SSD estándar y "Standard_LRS" para usar discos HDD estándar. Si decide usar una evaluación, el script dará prioridad al uso de los tipos de disco recomendados para cada disco de la máquina virtual. Si no usa la evaluación ni especifica cualquier valor, el script usará los discos HDD estándar de manera predeterminada.    
AVAILABILITYZONE_NUMBER | Especifique el número de zonas de disponibilidad que se usarán para la máquina virtual migrada. Puede dejarlo en blanco si no quiere usar zonas de disponibilidad. 
AVAILABILITYSET_NAME | Especifique el nombre del conjunto de disponibilidad que se va a usar para la máquina virtual migrada. Puede dejarlo en blanco si no desea usar el conjunto de disponibilidad.
TURNOFF_SOURCESERVER | Indice "Y" si quiere apagar la máquina virtual de origen en el momento de la migración. De lo contrario, use "N". Si se deja en blanco, el script supone que el valor es "N".
TESTMIGRATE_VNET_NAME | Especifique el nombre de la red virtual que se va a usar para la migración de prueba.
UPDATED_TARGET_RESOURCE_GROUP_NAME | Si quiere actualizar el grupo de recursos que va a usar la máquina virtual migrada en Azure, especifique el nombre del grupo de recursos de Azure; de lo contrario, déjelo en blanco. 
UPDATED_TARGET_VNET_NAME | Si quiere actualizar la red virtual que va a usar la máquina virtual migrada en Azure, especifique el nombre de la red virtual de Azure; de lo contrario, déjelo en blanco.
UPDATED_TARGET_MACHINE_NAME | Si quiere actualizar el nombre que va a usar la máquina virtual migrada en Azure, especifique el nombre nuevo que se va a usar; de lo contrario, déjelo en blanco.
UPDATED_TARGET_MACHINE_SIZE | Si quiere actualizar la SKU que va a usar la máquina virtual migrada en Azure, especifique la SKU nueva que se va a usar; de lo contrario, déjelo en blanco.
UPDATED_AVAILABILITYZONE_NUMBER | Si quiere actualizar la zona de disponibilidad que va a usar la máquina virtual migrada en Azure, especifique la zona de disponibilidad nueva que se va a usar; de lo contrario, déjelo en blanco.
UPDATED_AVAILABILITYSET_NAME | Si quiere actualizar el conjunto de disponibilidad que va a usar la máquina virtual migrada en Azure, especifique el conjunto de disponibilidad nuevo que se va a usar; de lo contrario, déjelo en blanco.
UPDATE_NIC1_ID | Especifique el id. de la NIC que se va a actualizar. Si se deja en blanco, el script supone que el valor es la primera NIC de la máquina virtual detectada. Si no quiere actualizar la NIC de la máquina virtual, deje en blanco todos los campos que contienen el nombre de la NIC. 
UPDATED_TARGET_NIC1_SELECTIONTYPE | Especifique el valor que se va a usar para esta NIC. Use "Primary","Secondary" o "DoNotCreate" para especificar si esta NIC debe ser la NIC principal, secundaria o si no se debe crear en la máquina virtual migrada. Solo se puede especificar una NIC como principal de la máquina virtual. Déjelo en blanco si no quiere actualizar.
UPDATED_TARGET_NIC1_SUBNET_NAME | Especifique el nombre de la subred que se va a usar para la NIC en la máquina virtual migrada. Déjelo en blanco si no quiere actualizar.
UPDATED_TARGET_NIC1_IP | Especifique la dirección IPv4 que se va a usar en la NIC de la máquina virtual migrada si quiere usar una IP estática. Use "auto" si quiere asignar automáticamente la IP. Déjelo en blanco si no quiere actualizar.
UPDATE_NIC2_ID | Especifique el id. de la NIC que se va a actualizar. Si se deja en blanco, el script supone que el valor es la segunda NIC de la máquina virtual detectada. Si no quiere actualizar la NIC de la máquina virtual, deje en blanco todos los campos que contienen el nombre de la NIC.
UPDATED_TARGET_NIC2_SELECTIONTYPE | Especifique el valor que se va a usar para esta NIC. Use "Primary","Secondary" o "DoNotCreate" para especificar si esta NIC debe ser la NIC principal, secundaria o si no se debe crear en la máquina virtual migrada. Solo se puede especificar una NIC como principal de la máquina virtual. Déjelo en blanco si no quiere actualizar.
UPDATED_TARGET_NIC2_SUBNET_NAME | Especifique el nombre de la subred que se va a usar para la NIC en la máquina virtual migrada. Déjelo en blanco si no quiere actualizar.
UPDATED_TARGET_NIC2_IP | Especifique la dirección IPv4 que se va a usar en la NIC de la máquina virtual migrada si quiere usar una IP estática. Use "auto" si quiere asignar automáticamente la IP. Déjelo en blanco si no quiere actualizar.
OK_TO_UPDATE | Use "Y" para indicar si las propiedades de la máquina virtual se deben actualizar al ejecutar el script AzMigrate_UpdateMachineProperties. De lo contrario, use "N" o déjelo en blanco.
OK_TO_MIGRATE | Use "Y" para indicar si la máquina virtual se debe migrar al ejecutar el script AzMigrate_StartMigration. Use "N" o déjelo en blanco si no quiere migrar la máquina virtual. 
OK_TO_USE_ASSESSMENT | Use "Y" para indicar si la máquina virtual debe iniciar la replicación mediante el uso de recomendaciones de evaluación al ejecutar el script AzMigrate_StartReplication. Esto invalidará los valores de TARGET_MACHINE_SIZE y TARGET_DISKTYPE del archivo .csv. Use "N" o déjelo en blanco si no quiere usar recomendaciones de evaluación.
OK_TO_TESTMIGRATE | Use "Y" para indicar si la máquina virtual se debe migrar al ejecutar el script AzMigrate_StartTestMigration. Use "N" o déjelo en blanco si no quiere hacer una migración de prueba de la máquina virtual. 
OK_TO_RETRIEVE_REPLICATIONSTATUS | Use "Y" para indicar si el estado de la replicación de la máquina virtual se debe actualizar al ejecutar el script AzMigrate_ReplicationStatus. Use "N" o déjelo en blanco si no quiere actualizar el estado de la replicación.
OK_TO_CLEANUP | Use "Y" para indicar si la replicación de la máquina virtual se debe limpiar al ejecutar el script AzMigrate_StopReplication. De lo contrario, use "N" o déjelo en blanco.
OK_TO_TESTMIGRATE_CLEANUP | Use "Y" para indicar si la migración de prueba de la máquina virtual se debe limpiar al ejecutar el script AzMigrate_CleanUpTestMigration. De lo contrario, use "N" o déjelo en blanco.


## <a name="script-execution"></a>Ejecución de script

Una vez que el archivo .csv esté listo, se pueden llevar a cabo estos pasos para migrar las máquinas virtuales de VMware locales.

**Núm. de paso** | **Nombre del script** | **Descripción**
--- | --- | ---
1 | AzMigrate_StartReplication.ps1 | Habilite la replicación para todas las máquinas virtuales que aparecen en el archivo .csv. El script crea una salida .csv y un archivo de registro para solucionar problemas.
2 | AzMigrate_ReplicationStatus.ps1 | Compruebe el estado de la replicación. El archivo crea una salida .csv con el estado de cada máquina virtual y un archivo de registro para solucionar problemas.
3 | AzMigrate_UpdateMachineProperties.ps1 | Una vez completada la replicación inicial de las máquinas virtuales, use este script para actualizar las propiedades de destino de la máquina virtual (propiedades de proceso y red). El script crea una salida .csv con los detalles del trabajo para cada máquina virtual.
4 | AzMigrate_StartTestMigration.ps1 |  Inicie la conmutación por error de prueba de todas las máquinas virtuales que aparecen en el archivo .csv y que están configuradas para la migración de prueba. El script crea una salida .csv con los detalles del trabajo para cada máquina virtual.
5 | AzMigrate_CleanUpTestMigration.ps1 | Una vez que valide manualmente las VM en las que se realizó la conmutación por error de prueba, use este script para limpiar la conmutación por error de prueba de todas las VM que aparecen en el archivo .csv y que están configuradas para la limpieza después de la migración de prueba. El script crea una salida .csv con los detalles del trabajo para cada máquina virtual.
6 | AzMigrate_StartMigration.ps1 | Inicie la migración de todas las máquinas virtuales que aparecen en el archivo .csv y que están configuradas para la migración. El script crea una salida .csv con los detalles del trabajo para cada máquina virtual.
7 | AzMigrate_StopReplication.ps1 | Detiene la replicación de la máquina virtual después de que se haya migrado correctamente o si quiere cancelar la replicación por otras razones. El script crea una salida .csv con los detalles del trabajo para cada máquina virtual.


Otros scripts invocan a los siguientes scripts para todas las operaciones de Azure Migrate, como la habilitación de la replicación, el inicio de la migración de prueba, la actualización de las propiedades de la máquina virtual, entre otras. Asegúrese de que todos los scripts se encuentren en la misma carpeta o ruta de acceso. 

**Núm. de paso** | **Nombre del script** | **Descripción**
--- | --- | ---
1 | AzMigrate_Shared.ps1 | Script común que contiene funciones para recuperar propiedades de evaluación (a través de la API), máquinas virtuales detectadas y máquinas virtuales de replicación. 
2 | AzMigrate_CSV_Processor.ps1 | Script común que contiene funciones que se usan para las operaciones del archivo .csv, como la carga, lectura e impresión de registros. 
3 | AzMigrate_Logger.ps1 | Script común que se invoca para generar el archivo de registro para las operaciones de automatización de Azure Migrate. El archivo de registro tendrá el formato log.NombreDeScript.FechaYHora.txt.

Además de lo anterior, la carpeta también incluye AzMigrate_Template.ps1, que contiene el marco básico para crear scripts personalizados para las distintas operaciones de Azure Migrate. 

### <a name="script-execution-syntax"></a>Sintaxis de ejecución de script

Una vez descargados los scripts, se pueden ejecutar como se indica a continuación.

Si quiere ejecutar el script para iniciar la replicación de máquinas virtuales mediante el uso del archivo Input.csv, use la sintaxis siguiente. 

```azurepowershell
".\AzMigrate_StartReplication.ps1" .\Input.csv 
```

Para más información sobre cómo usar Azure PowerShell para migrar máquinas virtuales de VMware con Azure Migrate, siga el [tutorial](./tutorial-migrate-vmware-powershell.md).