---
title: Copia de seguridad a petición en Azure Service Fabric
description: Use la característica de copia de seguridad y restauración de Service Fabric para realizar la copia de seguridad de sus datos de aplicación cuando lo necesite.
author: aagup
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: aagup
ms.openlocfilehash: 04d8bb4a9f8157a229751d073e8d351f5448fa68
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86247904"
---
# <a name="on-demand-backup-in-azure-service-fabric"></a>Copia de seguridad a petición en Azure Service Fabric

Puede realizar una copia de seguridad de los servicios de confianza sin estado y de Reliable Actors para abordar escenarios de desastres o pérdida de datos.

Azure Service Fabric incluye características para la [copia de seguridad periódica de los datos](service-fabric-backuprestoreservice-quickstart-azurecluster.md) y la copia de seguridad de los datos cuando sea necesaria. La copia de seguridad a petición es útil porque protege frente a la _pérdida de datos_/_los daños en los datos_ debidos a los cambios planeados en el servicio subyacente o su entorno.

Las características de copia de seguridad a petición son útiles para capturar el estado de los servicios antes de cualquier operación desencadenada manualmente relacionada con el servicio o el entorno de este. Por ejemplo, si realiza un cambio en los binarios del servicio al actualizarlo o degradarlo. En tal caso, la copia de seguridad a petición puede ayudar a proteger los datos de daños causados por errores de código de las aplicaciones.
## <a name="prerequisites"></a>Prerrequisitos

- Instale el módulo Microsoft.ServiceFabric.Powershell.Http [en versión preliminar] para realizar llamadas de configuración.

```powershell
    Install-Module -Name Microsoft.ServiceFabric.Powershell.Http -AllowPrerelease
```

- Asegúrese de que el clúster esté conectado mediante el comando `Connect-SFCluster` antes de realizar una solicitud de configuración con el módulo Microsoft.ServiceFabric.Powershell.Http.

```powershell

    Connect-SFCluster -ConnectionEndpoint 'https://mysfcluster.southcentralus.cloudapp.azure.com:19080'   -X509Credential -FindType FindByThumbprint -FindValue '1b7ebe2174649c45474a4819dafae956712c31d3' -StoreLocation 'CurrentUser' -StoreName 'My' -ServerCertThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'  

```


## <a name="triggering-on-demand-backup"></a>Desencadenamiento de la copia de seguridad a petición

La copia de seguridad a petición requiere detalles de almacenamiento para cargar los archivos de copia de seguridad. Especifique la ubicación de la copia de seguridad a petición, en la directiva de copia de seguridad periódica o en una solicitud de copia de seguridad a petición.

### <a name="on-demand-backup-to-storage-specified-by-a-periodic-backup-policy"></a>Copia de seguridad a petición en el almacenamiento especificado por una directiva de copia de seguridad periódica

Puede configurar la directiva de copia de seguridad periódica para usar una partición de un servicio de confianza con estado o de Reliable Actors para una copia de seguridad adicional a petición en el almacenamiento.

El siguiente caso es la continuación del escenario de [Habilitación de la copia de seguridad periódica del servicio de confianza con estado y Reliable Actors](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors). En este caso, se habilita una directiva de copia de seguridad para utilizar una partición y se produce una copia de seguridad con una frecuencia definida en Azure Storage.

#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>PowerShell con el módulo Microsoft.ServiceFabric.Powershell.Http

```powershell

Backup-SFPartition -PartitionId '974bd92a-b395-4631-8a7f-53bd4ae9cf22' 

```

#### <a name="rest-call-using-powershell"></a>Llamada a REST mediante PowerShell

Use [BackupPartition](/rest/api/servicefabric/sfclient-api-backuppartition) API para configurar el desencadenamiento de la copia de seguridad a petición para el identificador de partición `974bd92a-b395-4631-8a7f-53bd4ae9cf22`.

```powershell
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Backup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

Use [GetBackupProgress](/rest/api/servicefabric/sfclient-api-getpartitionbackupprogress) API para habilitar el seguimiento del [progreso de la copia de seguridad a petición](service-fabric-backup-restore-service-ondemand-backup.md#tracking-on-demand-backup-progress).

### <a name="on-demand-backup-to-specified-storage"></a>Copia de seguridad a petición en el almacenamiento especificado

Puede solicitar una copia de seguridad a petición para una partición de un servicio de confianza con estado o de Reliable Actors. Proporcione la información de almacenamiento como parte de la solicitud de copia de seguridad a petición.


#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>PowerShell con el módulo Microsoft.ServiceFabric.Powershell.Http

```powershell

Backup-SFPartition -PartitionId '974bd92a-b395-4631-8a7f-53bd4ae9cf22' -AzureBlobStore -ConnectionString  'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net' -ContainerName 'backup-container'

```

#### <a name="rest-call-using-powershell"></a>Llamada a REST mediante PowerShell

Use [BackupPartition](/rest/api/servicefabric/sfclient-api-backuppartition) API para configurar el desencadenamiento de la copia de seguridad a petición para el identificador de partición `974bd92a-b395-4631-8a7f-53bd4ae9cf22`. Incluya la siguiente información de Azure Storage:

```powershell
$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$OnDemandBackupRequest = @{
    BackupStorage = $StorageInfo
}

$body = (ConvertTo-Json $OnDemandBackupRequest)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Backup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

Puede usar [GetBackupProgress](/rest/api/servicefabric/sfclient-api-getpartitionbackupprogress) API para configurar el seguimiento del [progreso de la copia de seguridad a petición](service-fabric-backup-restore-service-ondemand-backup.md#tracking-on-demand-backup-progress).

### <a name="using-service-fabric-explorer"></a>Uso de Service Fabric Explorer
Asegúrese de que se ha habilitado el modo avanzado en la configuración de Service Fabric Explorer.
1. Seleccione las particiones deseadas y haga clic en Acciones. 
2. Seleccione Trigger Partition Backup (Desencadenar copia de seguridad de particiones) y rellene la información de Azure:

    ![Desencadenar copia de seguridad de particiones][0]

    o recurso compartido de archivos:

    ![Recurso compartido de archivos de Desencadenar copia de seguridad de particiones][1]

## <a name="tracking-on-demand-backup-progress"></a>Seguimiento del progreso de copia de seguridad a petición

Una partición de un servicio de confianza con estado o de Reliable Actors acepta solo una solicitud de copia de seguridad a petición a la vez. Solo puede aceptarse otra solicitud cuando se haya completado la solicitud de copia de seguridad a petición actual.

Particiones distintas pueden desencadenar solicitudes de copia de seguridad a petición al mismo tiempo.


#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>PowerShell con el módulo Microsoft.ServiceFabric.Powershell.Http

```powershell

Get-SFPartitionBackupProgress -PartitionId '974bd92a-b395-4631-8a7f-53bd4ae9cf22'

```
#### <a name="rest-call-using-powershell"></a>Llamada a REST mediante PowerShell

```powershell
$url = "https://mysfcluster-backup.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/GetBackupProgress?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3' 
$backupResponse = (ConvertFrom-Json $response.Content) 
$backupResponse
```

Las solicitudes de copia de seguridad a petición pueden tener los siguientes estados:

- **Accepted** (Aceptado): la copia de seguridad se ha iniciado en la partición y está en curso.
  ```
  BackupState             : Accepted
  TimeStampUtc            : 0001-01-01T00:00:00Z
  BackupId                : 00000000-0000-0000-0000-000000000000
  BackupLocation          :
  EpochOfLastBackupRecord :
  LsnOfLastBackupRecord   : 0
  FailureError            :
  ```
- **Success**, **Failure** o **Timeout**: una copia de seguridad a petición solicitada se puede completar en cualquiera de los siguientes estados:
  - **Correcto**: el estado de copia de seguridad _Success_ indica que el estado de la partición tiene una copia de seguridad correcta. La respuesta proporciona _BackupEpoch_ y _BackupLSN_ para la partición junto con la hora en UTC.
    ```
    BackupState             : Success
    TimeStampUtc            : 2018-11-21T20:00:01Z
    BackupId                : 5d64b697-6acd-45a4-adbd-3d75e0078081
    BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-11-21 20.00.01.zip
    EpochOfLastBackupRecord : @{DataLossNumber=131873018908156893; ConfigurationNumber=8589934592}
    LsnOfLastBackupRecord   : 36
    FailureError            :
    ```
  - **Error**: el estado de copia de seguridad _Failure_ indica que se ha producido un error durante la copia de seguridad del estado de la partición. La causa del error se indica en la respuesta.
    ```
    BackupState             : Failure
    TimeStampUtc            : 0001-01-01T00:00:00Z
    BackupId                : 00000000-0000-0000-0000-000000000000
    BackupLocation          :
    EpochOfLastBackupRecord :
    LsnOfLastBackupRecord   : 0
    FailureError            : @{Code=FABRIC_E_BACKUPCOPIER_UNEXPECTED_ERROR; Message=An error occurred during this operation.  Please check the trace logs for more details.}
    ```
  - **Timeout**: el estado de copia de seguridad _Timeout_ indica que la copia de seguridad del estado de la partición no se pudo crear en un tiempo determinado. El tiempo de espera predeterminado es de 10 minutos. En este escenario, inicie una nueva solicitud de copia de seguridad con un valor de [BackupTimeout](/rest/api/servicefabric/sfclient-api-backuppartition#backuptimeout) mayor en la solicitud de copia de seguridad a petición.
    ```
    BackupState             : Timeout
    TimeStampUtc            : 0001-01-01T00:00:00Z
    BackupId                : 00000000-0000-0000-0000-000000000000
    BackupLocation          :
    EpochOfLastBackupRecord :
    LsnOfLastBackupRecord   : 0
    FailureError            : @{Code=FABRIC_E_TIMEOUT; Message=The request of backup has timed out.}
    ```

## <a name="next-steps"></a>Pasos siguientes

- [Información sobre la configuración de la copia de seguridad periódica](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [Referencia de las API REST BackupRestore](/rest/api/servicefabric/sfclient-index-backuprestore)

[0]: ./media/service-fabric-backuprestoreservice/trigger-partition-backup.png
[1]: ./media/service-fabric-backuprestoreservice/trigger-backup-fileshare.png
