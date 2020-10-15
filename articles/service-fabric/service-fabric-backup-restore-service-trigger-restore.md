---
title: Restauración de una copia de seguridad en Azure Service Fabric
description: Use la característica de copia de seguridad y restauración periódicas de Service Fabric para restaurar datos de una copia de seguridad de los datos de aplicación.
author: aagup
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: aagup
ms.openlocfilehash: f98bf4f4518abd5f1b1a826e355c851acc055852
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86246697"
---
# <a name="restoring-backup-in-azure-service-fabric"></a>Restauración de una copia de seguridad en Azure Service Fabric

En Azure Service Fabric, los servicios de confianza con estado y Reliable Actors pueden mantener un estado mutable y autoritativo después de que se ha completado una transacción de solicitud y respuesta. Un servicio con estado podría dejan de funcionar durante mucho tiempo o perder información debido a un desastre. Si esto sucede, el servicio debe restaurarse desde la última copia de seguridad aceptable, para que puede seguir funcionando.

Por ejemplo, puede configurar un servicio para realizar una copia de seguridad de sus datos a fin de protegerse frente a los siguientes casos:

- **Caso de recuperación ante desastres**: En caso de que se produzca una pérdida permanente de todo un clúster de Service Fabric.
- **Caso de pérdida de datos**: Pérdida permanente de la mayoría de las réplicas de una partición del servicio.
- **Caso de pérdida de datos**: eliminación accidental o daños del servicio. Por ejemplo, un administrador elimina por error el servicio.
- **Caso de datos dañados**: Los errores en el servicio provocan daños en los datos. Por ejemplo, se pueden producir daños en los datos cuando una actualización del código de servicio escribe datos defectuosos en una colección de confianza. En tal caso, puede que tenga que restaurar el código y los datos a un estado anterior.

## <a name="prerequisites"></a>Prerrequisitos

- Para desencadenar la restauración, el _servicio de análisis de errores (FAS)_ debe estar habilitado para el clúster.
- El _servicio de restauración de copia de seguridad (BRS)_ creó la copia de seguridad.
- La restauración solo se puede desencadenar en una partición.
- Instale el módulo Microsoft.ServiceFabric.Powershell.Http [en versión preliminar] para realizar llamadas de configuración.

```powershell
    Install-Module -Name Microsoft.ServiceFabric.Powershell.Http -AllowPrerelease
```

- Asegúrese de que el clúster esté conectado mediante el comando `Connect-SFCluster` antes de realizar una solicitud de configuración con el módulo Microsoft.ServiceFabric.Powershell.Http.

```powershell

    Connect-SFCluster -ConnectionEndpoint 'https://mysfcluster.southcentralus.cloudapp.azure.com:19080'   -X509Credential -FindType FindByThumbprint -FindValue '1b7ebe2174649c45474a4819dafae956712c31d3' -StoreLocation 'CurrentUser' -StoreName 'My' -ServerCertThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'  

```


## <a name="triggered-restore"></a>Restauración desencadenada

Una restauración se puede desencadenar en cualquiera de los siguientes casos:

- Restauración de datos en una _recuperación ante desastres_.
- Restauración de datos ante _pérdida de datos o daños en los datos_.

### <a name="data-restore-in-the-case-of-disaster-recovery"></a>Restauración de datos en caso de recuperación ante desastres

Si se pierde un clúster entero de Service Fabric, puede recuperar los datos de las particiones del servicio de confianza con estado y Reliable Actors. La copia de seguridad deseada se puede seleccionar de la lista cuando se usan los [detalles de GetBackupAPI con almacenamiento de copia de seguridad](/rest/api/servicefabric/sfclient-api-getbackupsfrombackuplocation). La enumeración de copia de seguridad puede ser para una aplicación, un servicio o una partición.

En el ejemplo siguiente, suponga que el clúster perdido es el mismo clúster al que se hace referencia en [Habilitación de la copia de seguridad periódica del servicio de confianza con estado y Reliable Actors](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors). En este caso, `SampleApp` se implementa con la directiva de copia de seguridad habilitada, y las copias de seguridad se configuran para Azure Storage.

#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>PowerShell con el módulo Microsoft.ServiceFabric.Powershell.Http

```powershell
Get-SFBackupsFromBackupLocation -Application -ApplicationName 'fabric:/SampleApp' -AzureBlobStore -ConnectionString 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net' -ContainerName 'backup-container'

```

#### <a name="rest-call-using-powershell"></a>Llamada a REST mediante PowerShell

Ejecute un script de PowerShell para usar la API REST para devolver una lista de copias de seguridad creadas para todas las particiones dentro de la aplicación `SampleApp`. La API requiere la información de almacenamiento de copia de seguridad para enumerar las copias de seguridad disponibles.

```powershell
$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$BackupEntity = @{
    EntityKind = 'Application'
    ApplicationName='fabric:/SampleApp'
}

$BackupLocationAndEntityInfo = @{
    Storage = $StorageInfo
    BackupEntity = $BackupEntity
}

$body = (ConvertTo-Json $BackupLocationAndEntityInfo)
$url = "https://myalternatesfcluster.southcentralus.cloudapp.azure.com:19080/BackupRestore/$/GetBackups?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
$BackupPoints = (ConvertFrom-Json $response.Content)
$BackupPoints.Items
```

Salida de ejemplo de la ejecución anterior:

```
BackupId                : b9577400-1131-4f88-b309-2bb1e943322c
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 20.55.16.zip
BackupType              : Full
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3334
CreationTimeUtc         : 2018-04-06T20:55:16Z
FailureError            : 
*
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            :
*
BackupId                : 69436834-c810-4163-9386-a7a800f78359
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.25.36.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3764
CreationTimeUtc         : 2018-04-06T21:25:36Z
FailureError            :
```

Para desencadenar la restauración, elija una de las copias de seguridad. Por ejemplo, la copia de seguridad actual para la recuperación ante desastres podría ser la copia de seguridad siguiente:

```
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            :
```

Para la API de restauración, debe proporcionar los detalles _BackupId_ y _BackupLocation_.

También debe elegir una partición de destino en el clúster alternativo, como se detalla en el [esquema de partición](service-fabric-concepts-partitioning.md#get-started-with-partitioning). La copia de seguridad del clúster alternativo se restaura en la partición especificada en el esquema de partición del clúster perdido original.

Si el identificador de partición del clúster alternativo es `1c42c47f-439e-4e09-98b9-88b8f60800c6`, puede asignarlo al identificador de partición del clúster original `974bd92a-b395-4631-8a7f-53bd4ae9cf22`; simplemente compare la clave superior e inferior para la _creación de particiones por rango (UniformInt64Partition)_ .

Para la _creación de particiones con nombre_, el valor de nombre se compara para identificar la partición de destino en el clúster alternativo.

#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>PowerShell con el módulo Microsoft.ServiceFabric.Powershell.Http

```powershell

Restore-SFPartition  -PartitionId '1c42c47f-439e-4e09-98b9-88b8f60800c6' -BackupId 'b0035075-b327-41a5-a58f-3ea94b68faa4' -BackupLocation 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip' -AzureBlobStore -ConnectionString 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net' -ContainerName 'backup-container'

```

#### <a name="rest-call-using-powershell"></a>Llamada a REST mediante PowerShell

La restauración se solicita en la partición del clúster de copia de seguridad mediante la siguiente [API de restauración](/rest/api/servicefabric/sfclient-api-restorepartition):

```powershell

$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$RestorePartitionReference = @{
    BackupId = 'b0035075-b327-41a5-a58f-3ea94b68faa4'
    BackupLocation = 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip'
    BackupStorage  = $StorageInfo
}

$body = (ConvertTo-Json $RestorePartitionReference) 
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/1c42c47f-439e-4e09-98b9-88b8f60800c6/$/Restore?api-version=6.4" 

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

Puede realizar un seguimiento del progreso de una restauración con TrackRestoreProgress.

### <a name="using-service-fabric-explorer"></a>Uso de Service Fabric Explorer
Puede desencadenar una restauración desde Service Fabric Explorer. Asegúrese de que se ha habilitado el modo avanzado en la configuración de Service Fabric Explorer.
1. Seleccione las particiones deseadas y haga clic en Acciones. 
2. Seleccione Trigger Partition Restore (Desencadenar restauración de particiones) y rellene la información para Azure:

    ![Trigger Partition Restore (Desencadenar restauración de particiones)][2]

    o recurso compartido de archivos:

    ![Recurso compartido de archivos Trigger Partition Restore (Desencadenar restauración de particiones)][3]

### <a name="data-restore-for-_data-corruption__data-loss_"></a>Restauración de datos ante _daños dañados_/_pérdida de datos_

En el caso de _pérdida de datos_ o _datos dañados_, las particiones en copia de seguridad de las particiones del servicio de confianza con estado y Reliable Actors se pueden restaurar a cualquiera de las copias de seguridad elegidas.

El siguiente ejemplo es una continuación del escenario de [Habilitación de la copia de seguridad periódica del servicio de confianza con estado y Reliable Actors](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors). En este ejemplo, una directiva de copia de seguridad está habilitada para la partición y el servicio está realizando copias de seguridad con una frecuencia deseada en Azure Storage.

Seleccione una copia de seguridad de la salida de [GetBackupAPI](service-fabric-backuprestoreservice-quickstart-azurecluster.md#list-backups). En este escenario, la copia de seguridad se genera del mismo clúster que antes.

Para desencadenar la restauración, elija una copia de seguridad de la lista. Para la _pérdida de datos_/_datos dañados_ actuales, seleccione la siguiente copia de seguridad:

```
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            :
```

Para la API de restauración, proporcione los detalles _BackupId_ y _BackupLocation_. Dado que el clúster tiene la copia de seguridad habilitada, el _servicio de restauración de copia de seguridad (BRS)_ de Service Fabric identifica la ubicación de almacenamiento correcta a partir de la directiva de copia de seguridad asociada.


#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>PowerShell con el módulo Microsoft.ServiceFabric.Powershell.Http

```powershell
Restore-SFPartition  -PartitionId '974bd92a-b395-4631-8a7f-53bd4ae9cf22' -BackupId 'b0035075-b327-41a5-a58f-3ea94b68faa4' -BackupLocation 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip'

```

#### <a name="rest-call-using-powershell"></a>Llamada a REST mediante PowerShell

```powershell
$RestorePartitionReference = @{
    BackupId = 'b0035075-b327-41a5-a58f-3ea94b68faa4',
    BackupLocation = 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip'
}

$body = (ConvertTo-Json $RestorePartitionReference)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Restore?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

Puede realizar un seguimiento del progreso de la restauración mediante TrackRestoreProgress.

## <a name="track-restore-progress"></a>Seguimiento del progreso de la restauración

Una partición de un servicio de confianza con estado o de Reliable Actors acepta solo una solicitud de restauración a la vez. Una partición solo acepta otra solicitud una vez completada la solicitud de restauración actual. Se pueden desencadenar varias solicitudes de restauración en distintas particiones al mismo tiempo.

#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>PowerShell con el módulo Microsoft.ServiceFabric.Powershell.Http

```powershell
    Get-SFPartitionRestoreProgress -PartitionId '974bd92a-b395-4631-8a7f-53bd4ae9cf22'
```

#### <a name="rest-call-using-powershell"></a>Llamada a REST mediante PowerShell

```powershell
$url = "https://mysfcluster-backup.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/GetRestoreProgress?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'

$restoreResponse = (ConvertFrom-Json $response.Content)
$restoreResponse | Format-List
```

La solicitud de restauración progresa en el orden siguiente:

1. **Accepted** (Aceptado): el estado de restauración _Accepted_ indica que la partición solicitada se ha desencadenado con los parámetros de solicitud correctos.
    ```
    RestoreState  : Accepted
    TimeStampUtc  : 0001-01-01T00:00:00Z
    RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
    RestoredLsn   : 3552
    ```
2. **InProgress**: el estado de restauración _InProgress_ indica que se está produciendo una restauración en la partición con la copia de seguridad mencionado en la solicitud. La partición notifica el estado _dataloss_.
    ```
    RestoreState  : RestoreInProgress
    TimeStampUtc  : 0001-01-01T00:00:00Z
    RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
    RestoredLsn   : 3552
    ```
    
3. **Success**, **Failure** o **Timeout**: la restauración solicitada se puede realizar en cualquiera de los siguientes estados. Cada estado tiene los siguientes detalles de importancia y respuesta:
    - **Correcto**: el estado de restauración _Success_ indica un estado de partición recuperado. La partición notifica los estados _RestoredEpoch_ y _RestoredLSN_ junto con la hora en UTC.

        ```
        RestoreState  : Success
        TimeStampUtc  : 2018-11-22T11:22:33Z
        RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
        RestoredLsn   : 3552
        ```        
    - **Error**: el estado de restauración _Failure_ indica el error de la solicitud de restauración. Se notifica la causa del error.

        ```
        RestoreState  : Failure
        TimeStampUtc  : 0001-01-01T00:00:00Z
        RestoredEpoch : 
        RestoredLsn   : 0
        ```
    - **Timeout**: el estado de restauración _Timeout_ indica que la solicitud tiene tiempo de espera. Cree una solicitud de restauración con un valor mayor de [RestoreTimeout](/rest/api/servicefabric/sfclient-api-backuppartition#backuptimeout). El tiempo de espera predeterminado es de 10 minutos. Antes de volver a solicitar la restauración, asegúrese de que la partición esté fuera del estado de pérdida de datos.
     
        ```
        RestoreState  : Timeout
        TimeStampUtc  : 0001-01-01T00:00:00Z
        RestoredEpoch : 
        RestoredLsn   : 0
        ```

## <a name="automatic-restore"></a>Restauración automática

Puede configurar las particiones del servicio de confianza con estado y Reliable Actors en el clúster de Service Fabric para la _restauración automática_. En la directiva de copia de seguridad, establezca `AutoRestore` en _true_. Ah habilitar la _restauración automática_ se restauran automáticamente los datos de la copia de seguridad más reciente de la partición cuando se notifica la pérdida de datos. Para más información, consulte:

- [Habilitación de la restauración automática en la directiva de copia de seguridad](service-fabric-backuprestoreservice-configure-periodic-backup.md#auto-restore-on-data-loss)
- [Referencia de API RestorePartition](/rest/api/servicefabric/sfclient-api-restorepartition)
- [Referencia de API GetPartitionRestoreProgress](/rest/api/servicefabric/sfclient-api-getpartitionrestoreprogress)

## <a name="next-steps"></a>Pasos siguientes
- [Información sobre la configuración de la copia de seguridad periódica](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [Referencia de la API REST de restauración de copias de seguridad](/rest/api/servicefabric/sfclient-index-backuprestore)

[2]: ./media/service-fabric-backuprestoreservice/restore-partition-backup.png
[3]: ./media/service-fabric-backuprestoreservice/restore-partition-fileshare.png
