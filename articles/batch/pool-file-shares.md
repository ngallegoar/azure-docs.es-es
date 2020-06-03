---
title: Recurso compartido de archivos de Azure para grupos de Azure Batch
description: Cómo montar un recurso compartido de archivos de Azure a partir de nodos de proceso en un grupo de Linux o Windows en Azure Batch.
ms.topic: how-to
ms.date: 05/24/2018
ms.openlocfilehash: cb7e6f158e246319e851ee2edd5b21bae33c3723
ms.sourcegitcommit: a9784a3fd208f19c8814fe22da9e70fcf1da9c93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2020
ms.locfileid: "83780285"
---
# <a name="use-an-azure-file-share-with-a-batch-pool"></a>Uso de un recurso compartido de archivos con un grupo de Batch

[Azure Files](../storage/files/storage-files-introduction.md) ofrece recursos compartidos de archivos completamente administrados en la nube, a los que se puede acceder mediante el protocolo de Bloque de mensajes del servidor (SMB). En este artículo se proporciona información y ejemplos de código para montar y usar un recurso compartido de archivos de Azure en un grupo de nodos de proceso. En los ejemplos de código se usan los SDK de .NET y Python para Batch, pero estas mismas operaciones se pueden realizar con otros SDK y herramientas de Batch.

Batch ofrece compatibilidad nativa con API para el uso de blobs de Azure Storage para la lectura y escritura de los datos. Sin embargo, puede que en algunos casos quiera acceder a un recurso compartido de archivos de Azure desde el grupo de nodos de proceso. Por ejemplo, tiene una carga de trabajo heredada que depende de un recurso compartido de archivos de SMB, o sus tareas necesitan acceso a datos compartidos o generan salidas compartidas. 

## <a name="considerations-for-use-with-batch"></a>Consideraciones sobre el uso con Batch

* Podría usar un recurso compartido de archivos de Azure cuando tiene grupos que ejecutan un número de tareas en paralelo relativamente bajo. Revise los [objetivos de rendimiento y escala](../storage/files/storage-files-scale-targets.md) para determinar si se debe usar Azure Files (que usa una cuenta de Azure Storage), dado el tamaño esperado del grupo y el número de archivos de recursos. 

* Los recursos compartidos de archivos de Azure son [rentables](https://azure.microsoft.com/pricing/details/storage/files/) y se pueden configurar con la replicación de datos en otra región, así que son redundantes de forma global. 

* Se puede montar al mismo tiempo un recurso compartido de archivos de Azure desde un equipo local.

* Consulte también las [consideraciones de planeación](../storage/files/storage-files-planning.md) sobre los recursos compartidos de archivos de Azure.


## <a name="create-a-file-share"></a>Creación de un recurso compartido de archivos

[Cree un recurso compartido de archivos](../storage/files/storage-how-to-create-file-share.md) en una cuenta de almacenamiento que esté vinculada a la cuenta Batch o en una cuenta de almacenamiento independiente.

## <a name="mount-a-share-on-a-windows-pool"></a>Montaje de un recurso compartido en un grupo de Windows

En esta sección se proporcionan pasos y ejemplos de código para montar y usar un recurso compartido de archivos de Azure en un grupo de nodos de Windows. Para información adicional, consulte la [documentación](../storage/files/storage-how-to-use-files-windows.md) relacionada. 

En Batch, deberá montar el recurso compartido cada vez que se ejecute una tarea en un nodo de Windows. Actualmente, no es posible conservar la conexión de red entre las tareas en los nodos de Windows.

Por ejemplo, incluya un comando `net use` para montar el recurso compartido de archivos como parte de cada línea de comandos de la tarea. Para montar el recurso compartido de archivos, se necesitan las siguientes credenciales:

* **Nombre de usuario**: AZURE\\\<storageaccountname\>, por ejemplo, AZURE\\*mystorageaccountname*
* **Contraseña**: \<StorageAccountKeyWhichEnds in==>, por ejemplo, *XXXXXXXXXXXXXXXXXXXXX==*

El siguiente comando monta un recurso compartido de archivos *myfileshare* en la cuenta de almacenamiento *mystorageaccountname* como la unidad *S:* :

```
net use S: \\mystorageaccountname.file.core.windows.net\myfileshare /user:AZURE\mystorageaccountname XXXXXXXXXXXXXXXXXXXXX==
```

Por razones de simplicidad, los ejemplos aquí mostrados pasan las credenciales directamente en el texto. En la práctica, se recomienda firmemente administrar las credenciales mediante variables de entorno, certificados o una solución como Azure Key Vault.

Para simplificar la operación de montaje, puede conservar las credenciales en los nodos. Luego, puede montar el recurso compartido sin credenciales. Lleve a cabo los dos siguientes pasos:

1. Ejecute la utilidad de línea de comandos `cmdkey` mediante una tarea de inicio en la configuración del grupo. Esta utilidad conserva las credenciales en cada nodo de Windows. La línea de comandos de tarea de inicio es similar a:

   ```
   cmd /c "cmdkey /add:mystorageaccountname.file.core.windows.net /user:AZURE\mystorageaccountname /pass:XXXXXXXXXXXXXXXXXXXXX=="

   ```

2. Montar el recurso compartido en cada nodo como parte de cada tarea mediante `net use`. Por ejemplo, la siguiente línea de comandos de tarea monta el recurso compartido de archivos como la unidad *S:* . Esto iría seguido de un comando o script que hace referencia el recurso compartido. Las credenciales almacenadas en caché se usan para llamar a `net use`. En este paso se da por hecho que en estas tareas usa la misma identidad de usuario que usó en la tarea de inicio en el grupo, que no resulta apropiada para todos los escenarios.

   ```
   cmd /c "net use S: \\mystorageaccountname.file.core.windows.net\myfileshare" 
   ```

### <a name="c-example"></a>Ejemplo de C#
En el ejemplo siguiente de C# se muestra cómo conservar las credenciales en un grupo de Windows mediante una tarea de inicio. El nombre del servicio de archivo de almacenamiento y las credenciales de almacenamiento se pasan como constantes definidas. Aquí, la tarea de inicio se ejecuta con una cuenta de usuario estándar (no administrador) automática con ámbito de grupo.

```csharp
...
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: PoolId,
    targetDedicatedComputeNodes: PoolNodeCount,
    virtualMachineSize: PoolVMSize,
    virtualMachineConfiguration: virtualMachineConfiguration);

// Start task to store credentials to mount file share
string startTaskCommandLine = String.Format("cmd /c \"cmdkey /add:{0} /user:AZURE\\{1} /pass:{2}\"", StorageFileService, StorageAccountName, StorageAccountKey);

pool.StartTask = new StartTask
{
    CommandLine = startTaskCommandLine,
    UserIdentity = new UserIdentity(new AutoUserSpecification(
        elevationLevel: ElevationLevel.NonAdmin, 
        scope: AutoUserScope.Pool))
};

pool.Commit();
```

Después de almacenar las credenciales, use las líneas de comando de tarea para montar el recurso compartido y hacer referencia a él en las operaciones de lectura o escritura. Como ejemplo básico, la línea de comandos de tarea del siguiente fragmento de código usa el comando `dir` para enumerar los archivos del recurso compartido de archivos. Asegúrese de ejecutar cada tarea de trabajo mediante la misma [identidad de usuario](batch-user-accounts.md) que usó para ejecutar la tarea de inicio en el grupo. 

```csharp
...
string taskId = "myTask";
string taskCommandLine = String.Format("cmd /c \"net use {0} {1} & dir {2}\"", ShareMountPoint, StorageFileShare, ShareMountPoint);

CloudTask task = new CloudTask(taskId, taskCommandLine);
task.UserIdentity = new UserIdentity(new AutoUserSpecification(
    elevationLevel: ElevationLevel.NonAdmin,
    scope: AutoUserScope.Pool));
tasks.Add(task);
```

## <a name="mount-a-share-on-a-linux-pool"></a>Montaje de un recurso compartido en un grupo de Linux

Los recursos compartidos de archivos de Azure se pueden montar en distribuciones de Linux mediante el [cliente kernel de CIFS](https://wiki.samba.org/index.php/LinuxCIFS). En el ejemplo siguiente se muestra cómo montar un recurso compartido de archivos en un grupo de nodos de proceso de Ubuntu 16.04 LTS. Si usa una distribución de Linux diferente, los pasos generales son similares, pero debe usar el administrador de paquetes correspondiente a su distribución. Para más información y ejemplos adicionales, consulte [Uso de Azure Files con Linux](../storage/files/storage-how-to-use-files-linux.md).

En primer lugar, use una identidad de usuario administrador para instalar el paquete `cifs-utils`, y cree el punto de montaje (por ejemplo, */mnt/MyAzureFileShare*) en el sistema de archivos local. La carpeta para el punto de montaje se puede crear en cualquier parte del sistema de archivos, pero es habitual crearla en la carpeta `/mnt`. Asegúrese de no crear un punto de montaje directamente en `/mnt` (en Ubuntu) o en `/mnt/resource` (en el caso de otras distribuciones).

```
apt-get update && apt-get install cifs-utils && sudo mkdir -p /mnt/MyAzureFileShare
```

A continuación, ejecute el comando `mount` para montar el recurso compartido de archivos mediante estas credenciales:

* **Nombre de usuario**: \<storageaccountname\>, por ejemplo, *mystorageaccountname*
* **Contraseña**: \<StorageAccountKeyWhichEnds in==>, por ejemplo, *XXXXXXXXXXXXXXXXXXXXX==*

El siguiente comando monta un recurso compartido de archivos *myfileshare* en la cuenta de almacenamiento *mystorageaccountname* en */mnt/MyAzureFileShare*: 

```
mount -t cifs //mystorageaccountname.file.core.windows.net/myfileshare /mnt/MyAzureFileShare -o vers=3.0,username=mystorageaccountname,password=XXXXXXXXXXXXXXXXXXXXX==,dir_mode=0777,file_mode=0777,serverino && ls /mnt/MyAzureFileShare
```

Por razones de simplicidad, los ejemplos aquí mostrados pasan las credenciales directamente en el texto. En la práctica, se recomienda firmemente administrar las credenciales mediante variables de entorno, certificados o una solución como Azure Key Vault.

En el grupo de Linux, puede combinar todos estos pasos en una única tarea de inicio o ejecutarlos en un script. Ejecute la tarea de inicio como un usuario administrador en el grupo. Establezca la tarea de inicio para esperar a que finalice correctamente antes de ejecutar tareas adicionales en el grupo que hagan referencia al recurso compartido.

### <a name="python-example"></a>Ejemplo de Python

En el ejemplo de Python siguiente se muestra cómo configurar un grupo de Ubuntu para montar el recurso compartido en una tarea de inicio. El punto de montaje, el punto de conexión del recurso compartido de archivos y las credenciales de almacenamiento se pasan como constantes definidas. La tarea de inicio se ejecuta con una cuenta de usuario administrador automática con ámbito de grupo.

```python
pool = batch.models.PoolAddParameter(
    id=pool_id,
    virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
        image_reference=batchmodels.ImageReference(
            publisher="Canonical",
            offer="UbuntuServer",
            sku="16.04.0-LTS",
            version="latest"),
        node_agent_sku_id="batch.node.ubuntu 16.04"),
    vm_size=_POOL_VM_SIZE,
    target_dedicated_nodes=_POOL_NODE_COUNT,
    start_task=batchmodels.StartTask(
        command_line="/bin/bash -c \"apt-get update && apt-get install cifs-utils && mkdir -p {} && mount -t cifs {} {} -o vers=3.0,username={},password={},dir_mode=0777,file_mode=0777,serverino\"".format(
            _COMPUTE_NODE_MOUNT_POINT, _STORAGE_ACCOUNT_SHARE_ENDPOINT, _COMPUTE_NODE_MOUNT_POINT, _STORAGE_ACCOUNT_NAME, _STORAGE_ACCOUNT_KEY),
        wait_for_success=True,
        user_identity=batchmodels.UserIdentity(
            auto_user=batchmodels.AutoUserSpecification(
                scope=batchmodels.AutoUserScope.pool,
                elevation_level=batchmodels.ElevationLevel.admin)),
    )
)
batch_service_client.pool.add(pool)
```

Después de montar el recurso compartido y definir el trabajo, use el recurso compartido en las líneas de comando de tarea. Por ejemplo, el siguiente comando básico usa `ls` para enumerar los archivos del recurso compartido de archivos.

```python
...
task = batch.models.TaskAddParameter(
    id='mytask',
    command_line="/bin/bash -c \"ls {}\"".format(_COMPUTE_NODE_MOUNT_POINT))

batch_service_client.task.add(job_id, task)
```


## <a name="next-steps"></a>Pasos siguientes

* Para obtener información sobre otras opciones de lectura y escritura de datos en Batch, consulte [Trabajo persistente y resultado de la tarea](batch-task-output.md).
* Consulte también el kit de herramientas [Batch Shipyard](https://github.com/Azure/batch-shipyard), que incluye [recetas de Shipyard](https://github.com/Azure/batch-shipyard/tree/master/recipes) para implementar sistemas de archivos para cargas de trabajo de contenedor de Batch.
