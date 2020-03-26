---
title: 'Tutorial: restauración de SAP HANA DB en Azure mediante la CLI'
description: En este tutorial aprenderá a restaurar las bases de datos de SAP HANA que se ejecutan en una máquina virtual de Azure de un almacén de Recovery Services de Azure Backup mediante la CLI de Azure.
ms.topic: tutorial
ms.date: 12/4/2019
ms.openlocfilehash: 6dbe0c4382b648506d853feb281c70a8e8401595
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "75470410"
---
# <a name="tutorial-restore-sap-hana-databases-in-an-azure-vm-using-azure-cli"></a>Tutorial: Restauración de las bases de datos de SAP HANA en una máquina virtual de Azure con la CLI de Azure

La CLI de Azure se usa para crear y administrar los recursos de Azure desde la línea de comandos o mediante scripts. En esta documentación se detalla cómo restaurar una base de datos de SAP HANA con copia de seguridad en una máquina virtual de Azure con la CLI de Azure. También puede llevar a cabo estos pasos con [Azure Portal](https://docs.microsoft.com/azure/backup/sap-hana-db-restore).

Use [Azure Cloud Shell](tutorial-sap-hana-backup-cli.md) para ejecutar comandos de la CLI.

Al final de este tutorial podrá realizar lo siguiente:

> [!div class="checklist"]
>
> * Ver los puntos de restauración de una base de datos con copia de seguridad
> * Restaurar una base de datos

En este tutorial se presupone que tiene una base de datos de SAP HANA que se ejecuta en una máquina virtual de Azure con copia de seguridad realizada mediante Azure Backup. Si ha usado [Realización de una copia de seguridad de una base de datos de SAP HANA en Azure mediante la CLI](tutorial-sap-hana-backup-cli.md) para realizar la copia de seguridad de la base de datos de SAP HANA, estará usando los siguientes recursos:

* un grupo de recursos denominado *saphanaResourceGroup*,
* un almacén denominado *saphanaVault*,
* un contenedor protegido denominado *VMAppContainer;Compute;saphanaResourceGroup;saphanaVM*,
* el elemento o la base de datos con copia de seguridad denominado *saphanadatabase;hxe;hxe* y
* los recursos de la región *westus2*.

## <a name="view-restore-points-for-a-backed-up-database"></a>Ver los puntos de restauración de una base de datos con copia de seguridad

Para ver la lista de todos los puntos de recuperación de una base de datos, use el cmdlet [az backup recoverpoint list](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az-backup-recoverypoint-show-log-chain) como se indica a continuación:

```azurecli-interactive
az backup recoverypoint list --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --item-name saphanadatabase;hxe;hxe \
   --output table
```

La lista de puntos de recuperación tendrá el siguiente aspecto:

```output
Name                      Time                               BackupManagementType   Item Name               RecoveryPointType
-------------------       ---------------------------------  ---------------------  ----------------------  ------------------
7660777527047692711       2019-12-10T04:00:32.346000+00:00   AzureWorkload          SAPHanaDtabase;hxe;hxe  Full
7896624824685666836       2019-12-15T10:33:32.346000+00:00   AzureWorkload          SAPHanaDtabase;hxe;hxe  Differential
DefaultRangeRecoveryPoint                                    AzureWorkload          SAPHanaDtabase;hxe;hxe  Log
```

Como puede ver, la lista anterior contiene tres puntos de recuperación: para las copias de seguridad completas, las diferenciales y de registro.

>[!NOTE]
>También puede ver los puntos inicial y final de cada cadena de copia de seguridad de registro íntegra mediante el cmdlet [az backup recoverypoin show-log-chain](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az-backup-recoverypoint-show-log-chain).

## <a name="prerequisites-to-restore-a-database"></a>Requisitos previos para restaurar una base de datos

Antes de restaurar una base de datos, asegúrese de que se cumplen los requisitos previos siguientes:

* Solo puede restaurar la base de datos en una instancia de SAP HANA que se encuentre en la misma región.
* La instancia de destino debe estar registrada en el mismo almacén que la de origen.
* Azure Backup no puede identificar dos instancias de SAP HANA diferentes en la misma máquina virtual. Por lo tanto, no es posible restaurar los datos de una instancia en otra en la misma máquina virtual.

## <a name="restore-a-database"></a>Restaurar una base de datos

Azure Backup puede restaurar bases de datos de SAP HANA que se ejecutan en máquinas virtuales de Azure como se indica a continuación:

* Restaurar a una fecha u hora específicas (con precisión de segundos) mediante copias de seguridad de registros. Azure Backup determina automáticamente la copia de seguridad diferencial o completa apropiada, y la cadena de copias de seguridad de registros necesarias para restaurar los datos en función del tiempo seleccionado.
* Restaurar una copia de seguridad completa o diferencial específica para restaurar a un punto de recuperación específico.

Para restaurar una base de datos, use el cmdlet [az restore-azurewl](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurewl), que requiere un objeto de configuración de recuperación como una de las entradas. Este objeto se puede generar mediante el cmdlet [az backup recoveryconfig show](https://docs.microsoft.com/cli/azure/backup/recoveryconfig?view=azure-cli-latest#az-backup-recoveryconfig-show). El objeto de configuración de recuperación contiene todos los detalles para realizar la restauración. Uno de ellos es el modo de restauración: **OriginalWorkloadRestore** o **AlternateWorkloadRestore**.

>[!NOTE]
> **OriginalWorkloadRestore**: restaura los datos en la misma instancia de SAP HANA de origen. Esta opción sobrescribe la base de datos original. <br>
> **AlternateWorkloadRestore**: restaura la base de datos en una ubicación alternativa y mantiene la base de datos de origen original.

## <a name="restore-to-alternate-location"></a>Restauración a una ubicación alternativa

Para restaurar una base de datos en una ubicación alternativa, use **AlternateWorkloadRestore** como modo de restauración. A continuación debe elegir el punto de restauración, que puede ser un momento anterior o cualquiera de los puntos de restauración anteriores.

En este tutorial se realizará la restauración a un punto de restauración anterior. [Consulte la lista de puntos de restauración](#view-restore-points-for-a-backed-up-database) para la base de datos y elija el momento al que desea realizar la restauración. En este tutorial se usará el punto de restauración con el nombre *7660777527047692711*.

Con el nombre de punto de restauración anterior y el modo de restauración, vamos a crear el objeto de configuración de recuperación mediante el cmdlet [az backup recoveryconfig show](https://docs.microsoft.com/cli/azure/backup/recoveryconfig?view=azure-cli-latest#az-backup-recoveryconfig-show). Echemos un vistazo a lo que significa cada uno de los parámetros restantes de este cmdlet:

* **--target-item-name** es el nombre que utilizará la base de datos restaurada. En este caso, *restored_database*.
* **--target-server-name** es el nombre de un servidor SAP HANA que se ha registrado correctamente en un almacén de Recovery Services y se encuentra en la misma región que la base de datos que se va a restaurar. En este tutorial se va a restaurar la base de datos en el mismo servidor SAP HANA que se ha protegido, denominado *hxehost*.
* **--target-server-type**; para la restauración de las bases de datos de SAP HANA, se debe usar **SapHanaDatabase**.

```azurecli-interactive

az backup recoveryconfig show --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --item-name saphanadatabase;hxe;hxe \
    --restore-mode AlternateWorkloadRestore \
    --rp-name 7660777527047692711 \
    --target-item-name restored_database \
    --target-server-name hxehost \
    --target-server-type HANAInstance \
    --workload-type SAPHANA \
    --output json
```

La respuesta a la consulta anterior será un objeto de configuración de recuperación con un aspecto similar al siguiente:

```output
"{\"restore_mode\": \"OriginalLocation\", \"container_uri\": \" VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \", \"item_uri\": \"SAPHanaDatabase;hxe;hxe\", \"recovery_point_id\": \"DefaultRangeRecoveryPoint\", \"log_point_in_time\": \"28-11-2019-09:53:00\", \"item_type\": \"SAPHana\", \"source_resource_id\": \"/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/saphanaResourceGroup/providers/Microsoft.Compute/virtualMachines/saphanavm\", \"database_name\": null, \"container_id\": null, \"alternate_directory_paths\": null}"
```

Ahora, para restaurar la base de datos, ejecute el cmdlet [az restore-azurewl](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurewl). Para usar este comando, se especificará la salida JSON anterior, guardada en un archivo denominado *recoveryconfig.json*.

```azurecli-interactive
az backup restore restore-azurewl --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --restore-config recoveryconfig.json \
    --output table
```

El resultado tendrá un aspecto similar al siguiente:

```output
Name                                  Resource
------------------------------------  -------
5b198508-9712-43df-844b-977e5dfc30ea  SAPHANA
```

La respuesta le proporcionará el nombre del trabajo. Este nombre de trabajo se puede usar para realizar el seguimiento del estado del trabajo mediante el cmdlet [az backup job show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show).

## <a name="restore-and-overwrite"></a>Restauración y sobrescritura

Para la restauración en la ubicación original, usaremos **OrignialWorkloadRestore** como modo de restauración. A continuación debe elegir el punto de restauración, que puede ser un momento anterior o cualquiera de los puntos de restauración anteriores.

En este tutorial elegiremos el momento anterior "28-11-2019-09:53:00" para la restauración. Puede proporcionar este punto de restauración en los siguientes formatos: dd-mm-aaaa, dd-mm-aaaa-hh:mm:ss. Para elegir un momento dado para la restauración, use el cmdlet [az backup recoverypoint show-log-chain](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az-backup-recoverypoint-show-log-chain), que muestra los intervalos de copias de seguridad de la cadena de registro íntegras.

```azurecli-interactive
az backup recoveryconfig show --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --item-name saphanadatabase;hxe;hxe \
    --restore-mode OriginalWorkloadRestore \
    --log-point-in-time 28-11-2019-09:53:00 \
    --output json
```

La respuesta a la consulta anterior será un objeto de configuración de recuperación con el aspecto siguiente:

```output
"{\"restore_mode\": \"OriginalLocation\", \"container_uri\": \" VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \", \"item_uri\": \"SAPHanaDatabase;hxe;hxe\", \"recovery_point_id\": \"DefaultRangeRecoveryPoint\", \"log_point_in_time\": \"28-11-2019-09:53:00\", \"item_type\": \"SAPHana\", \"source_resource_id\": \"/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/saphanaResourceGroup/providers/Microsoft.Compute/virtualMachines/saphanavm\", \"database_name\": null, \"container_id\": null, \"alternate_directory_paths\": null}"
```

Ahora, para restaurar la base de datos, ejecute el cmdlet [az restore-azurewl](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurewl). Para usar este comando, se especificará la salida JSON anterior, guardada en un archivo denominado *recoveryconfig.json*.

```azurecli-interactive
az backup restore restore-azurewl --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --restore-config recoveryconfig.json \
    --output table
```

El resultado tendrá un aspecto similar al siguiente:

```output
Name                                  Resource
------------------------------------  --------
5b198508-9712-43df-844b-977e5dfc30ea  SAPHANA
```

La respuesta le proporcionará el nombre del trabajo. Este nombre de trabajo se puede usar para realizar el seguimiento del estado del trabajo mediante el cmdlet [az backup job show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show).

## <a name="next-steps"></a>Pasos siguientes

* Para aprender a administrar las bases de datos de SAP HANA con copia de seguridad realizada mediante la CLI de Azure, continúe con el tutorial: [Administración de una base de datos de SAP HANA en una máquina virtual de Azure con la CLI](tutorial-sap-hana-backup-cli.md)

* Para aprender a restaurar una base de datos de SAP HANA que se ejecuta en una máquina virtual de Azure mediante Azure Portal, consulte [Restauración de bases de datos de SAP HANA en máquinas virtuales de Azure](https://docs.microsoft.com/azure/backup/sap-hana-db-restore)
