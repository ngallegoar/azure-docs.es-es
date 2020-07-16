---
title: Descripción de la configuración de la copia de seguridad periódica
description: Use la característica de copia de seguridad periódica y restauración de Service Fabric para habilitar la copia de seguridad periódica de los datos de su aplicación.
author: hrushib
ms.topic: article
ms.date: 2/01/2019
ms.author: hrushib
ms.openlocfilehash: c77f069d93e368652c30cd100b0f99ca55341882
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86261231"
---
# <a name="understanding-periodic-backup-configuration-in-azure-service-fabric"></a>Información sobre la configuración de la copia de seguridad periódica en Azure Service Fabric

La configuración de la copia de seguridad periódica de los servicios con estado de confianza o Reliable Actors consta de los pasos siguientes:

1. **Creación de directivas de copia de seguridad**: en este paso, se crean una o varias directivas de copia de seguridad según los requisitos.

2. **Habilitación de la copia de seguridad**: en este paso, se asocian las directivas de copia de seguridad creadas en el **paso 1** a las entidades necesarias, _aplicación_, _servicio_ o  _partición_.

## <a name="create-backup-policy"></a>Crear directiva de copia de seguridad

Una directiva de copia de seguridad consta de las siguientes configuraciones:

* **Restauración automática en caso de pérdida de datos**: especifica si se debe desencadenar automáticamente la restauración con la última copia de seguridad disponible en caso de que la partición experimente un evento de pérdida de datos.

* **Número máximo de copias de seguridad incrementales**: define el número máximo de copias de seguridad incrementales que se deben realizar entre dos copias de seguridad completas. El número máximo de copias de seguridad incrementales especifica el límite superior. Se puede realizar una copia de seguridad completa antes de que se complete el número especificado de copias de seguridad incrementales cuando se da una de las condiciones siguientes

    1. La réplica nunca ha realizado una copia de seguridad completa desde que se convirtió en la primaria.

    2. Algunas de las entradas del registro se han truncado desde la última copia de seguridad.

    3. La réplica superó el límite MaxAccumulatedBackupLogSizeInMB.

* **Programación de copia de seguridad**: las horas a las que se van a realizar las copias de seguridad periódicas o la frecuencia. Se puede programar que las copias de seguridad sean periódicas con el intervalo especificado o en un momento fijo, diaria o semanalmente.

    1. **Programación de copia de seguridad basada en frecuencia**: este tipo de programación debe usarse si es necesario realizar copias de seguridad de datos a intervalos fijos. El intervalo de tiempo deseado entre dos copias de seguridad consecutivas se define con el formato ISO8601. La programación de copia de seguridad basada en frecuencia admite la resolución de intervalos de un minuto.
        ```json
        {
            "ScheduleKind": "FrequencyBased",
            "Interval": "PT10M"
        }
        ```

    2. **Programación de copia de seguridad basada en tiempo**: este tipo de programación debe usarse si es necesario realizar copias de seguridad de datos a horas específicas del día o la semana. El tipo de frecuencia de la programación puede ser diaria o semanal.
        1. **Programación de copia de seguridad basada en tiempo _diaria_** : este tipo de programación debe usarse si es necesario realizar copias de seguridad de datos a horas específicas del día. Para especificarla, establezca `ScheduleFrequencyType` en _Cada día_; y establezca `RunTimes` en la lista de horas del día deseadas con el formato ISO8601. Se omitirá la fecha especificada junto con la hora. Por ejemplo, `0001-01-01T18:00:00` representa _6:00 P.M._ cada día y se omite la parte de fecha _0001-01-01_. En el ejemplo siguiente se ilustra la configuración para desencadenar la copia de seguridad diaria a las _9:00 AM_ y _6:00 P.M._ todos los días.

            ```json
            {
                "ScheduleKind": "TimeBased",
                "ScheduleFrequencyType": "Daily",
                "RunTimes": [
                  "0001-01-01T09:00:00Z",
                  "0001-01-01T18:00:00Z"
                ]
            }
            ```

        2. **Programación de copia de seguridad basada en tiempo _semanal_** : este tipo de programación debe usarse si es necesario realizar copias de seguridad de datos a horas específicas del día. Para especificarla, establezca `ScheduleFrequencyType` en _Cada semana_; establezca `RunDays` en la lista de días de la semana en las que se debe desencadenar la copia de seguridad y establezca `RunTimes` en la lista de horas del día deseadas con el formato ISO8601, se omitirá la fecha especificada junto con la hora. Lista de días de la semana en las que se debe desencadenar la copia de seguridad periódica. En el ejemplo siguiente se ilustra la configuración para desencadenar la copia de seguridad diaria a las _9:00 AM_ y _6:00 P.M._ de lunes a viernes.

            ```json
            {
                "ScheduleKind": "TimeBased",
                "ScheduleFrequencyType": "Weekly",
                "RunDays": [
                   "Monday",
                   "Tuesday",
                   "Wednesday",
                   "Thursday",
                   "Friday"
                ],
                "RunTimes": [
                  "0001-01-01T09:00:00Z",
                  "0001-01-01T18:00:00Z"
                ]
            }
            ```

* **Almacenamiento de copia de seguridad**: especifica la ubicación en la que se deben cargar las copias de seguridad. El almacenamiento puede ser el almacén de blobs de Azure o un recurso compartido de archivos.
    1. **Azure Blob Storage**: este tipo de almacenamiento debe seleccionarse cuando se deben almacenar copias de seguridad generadas en Azure. Tanto los clústeres _independientes_ como los clústeres _basados en Azure_ pueden usar este tipo de almacenamiento. La descripción de este tipo de almacenamiento requiere la cadena de conexión y el nombre del contenedor donde se deben cargar las copias de seguridad. Si el contenedor con el nombre especificado no está disponible, se crea durante la carga de una copia de seguridad.
        ```json
        {
            "StorageKind": "AzureBlobStore",
            "FriendlyName": "Azure_storagesample",
            "ConnectionString": "<Put your Azure blob store connection string here>",
            "ContainerName": "BackupContainer"
        }
        ```

    2. **Recurso compartido de archivos**: se debe seleccionar este tipo de almacenamiento para clústeres _independientes_ cuando se debe almacenar la copia de seguridad de datos localmente. La descripción de este tipo de almacenamiento requiere la ruta de acceso del recurso compartido de archivos donde se deben cargar las copias de seguridad. Se puede configurar el acceso al recurso compartido de archivos mediante una de las opciones siguientes
        1. _Autenticación de Windows integrada_, donde el acceso al recurso compartido de archivos se proporciona a todos los equipos que pertenecen al clúster de Service Fabric. En este caso, establezca los siguientes campos para configurar el almacenamiento de copia de seguridad basado en _recurso compartido de archivos_.

            ```json
            {
                "StorageKind": "FileShare",
                "FriendlyName": "Sample_FileShare",
                "Path": "\\\\StorageServer\\BackupStore"
            }
            ```

        2. _Protección del recurso compartido de archivos mediante nombre de usuario y contraseña_, donde se proporciona el acceso al recurso compartido de archivos a usuarios específicos. La especificación del almacenamiento de recurso compartido de archivos también proporciona la capacidad de especificar un nombre de usuario y contraseña secundarios para ofrecer credenciales de reserva en caso de que se produzca un error de autenticación con el nombre de usuario y contraseña principales. En este caso, establezca los siguientes campos para configurar el almacenamiento de copia de seguridad basado en _recurso compartido de archivos_.

            ```json
            {
                "StorageKind": "FileShare",
                "FriendlyName": "Sample_FileShare",
                "Path": "\\\\StorageServer\\BackupStore",
                "PrimaryUserName": "backupaccount",
                "PrimaryPassword": "<Password for backupaccount>",
                "SecondaryUserName": "backupaccount2",
                "SecondaryPassword": "<Password for backupaccount2>"
            }
            ```

> [!NOTE]
> Asegúrese de que la confiabilidad del almacenamiento cumpla o supere los requisitos de confiabilidad de los datos de copia de seguridad.
>

* **Directiva de retención**: especifica la directiva para conservar copias de seguridad en el almacenamiento configurado. Solo se admite la directiva de retención básica.
    1. **Directiva de retención básica**: esta directiva de retención permite garantizar un uso óptimo del almacenamiento eliminando los archivos de copia de seguridad que ya no son necesarios. `RetentionDuration` puede especificarse para establecer el intervalo de tiempo durante el que se deben conservar las copias de seguridad en el almacenamiento. `MinimumNumberOfBackups` es un parámetro opcional que se puede especificar para asegurarse de que el número especificado de copias de seguridad se conserva siempre con independencia de `RetentionDuration`. En el ejemplo siguiente se muestra la configuración para conservar las copias de seguridad durante _10_ días y no permite que el número de copias de seguridad sea inferior a _20_.

        ```json
        {
            "RetentionPolicyType": "Basic",
            "RetentionDuration" : "P10D",
            "MinimumNumberOfBackups": 20
        }
        ```

## <a name="enable-periodic-backup"></a>Habilitación de la copia de seguridad periódica
Después de definir la directiva de copia de seguridad para satisfacer los requisitos de copia de seguridad de datos, la directiva de copia de seguridad debe asociarse correctamente con una _aplicación_, _servicio_ o _partición_.

### <a name="hierarchical-propagation-of-backup-policy"></a>Propagación jerárquica de directiva de copia de seguridad
En Service Fabric, la relación entre la aplicación, el servicio y las particiones es jerárquica, como se explica en [Modelo de aplicación](./service-fabric-application-model.md). La directiva de copia de seguridad puede asociarse a una _aplicación_, un _servicio_ o una _partición_ en la jerarquía. La directiva de copia de seguridad se propaga de forma jerárquica al siguiente nivel. Por ejemplo, si solo se ha creado una única directiva de copia de seguridad y se ha asociado con una _aplicación_, se hará copia de seguridad de todas las particiones con estado que pertenezcan a todos los _servicios con estado de confianza_ y a _Reliable Actors_ de la _aplicación_ mediante la directiva de copia de seguridad. O si la directiva de copia de seguridad está asociada con un _servicio con estado de confianza_, se hará copia de seguridad de todas sus particiones mediante la directiva de copia de seguridad.

### <a name="overriding-backup-policy"></a>Reemplazar directiva de copia de seguridad
Puede haber un escenario en el que se requiera una copia de seguridad de datos con la misma programación de copia de seguridad para todos los servicios de la aplicación, excepto para servicios específicos en los que se deba realizar la copia de seguridad de datos con una programación de frecuencia mayor o en una cuenta de almacenamiento diferente o recurso compartido de archivos. Para abordar estos escenarios, el servicio de restauración de copia de seguridad proporciona un recurso para reemplazar la directiva propagada en el ámbito del servicio y la partición. Cuando la directiva de copia de seguridad se asocia al _servicio_ o _partición_, reemplaza a la directiva de copia de seguridad propagada, si la hubiera.

### <a name="example"></a>Ejemplo

En este ejemplo se usa la configuración con dos aplicaciones _MyApp_A_ y _MyApp_B_. La aplicación _MyApp_A_ contiene dos servicios con estado de confianza, _SvcA1_ & _SvcA3_, y un servicio de Reliable Actor, _ActorA2_. _SvcA1_ contiene tres particiones, mientras que _ActorA2_ y _SvcA3_ contienen dos particiones cada uno.  La aplicación _MyApp_B_ contiene tres servicios con estado de confianza, _SvcB1_, _SvcB2_ y _SvcB3_. _SvcB1_ y _SvcB2_ contienen dos particiones cada uno, mientras que _SvcB3_ contiene tres particiones.

Se supone que los requisitos de copia de seguridad de datos de estas aplicaciones son los siguientes

1. MyApp_A
    1. Crear una copia de seguridad de datos diaria para todas las particiones de todos los _servicios de confianza con estado_ y _Reliable Actors_ que pertenezcan a la aplicación. Cargar los datos de copia de seguridad en la ubicación _BackupStore1_.

    2. Uno de los servicios, _SvcA3_, requiere una copia de seguridad de datos cada hora.

    3. El tamaño de los datos de la partición _SvcA1_P2_ es mayor de lo esperado y la copia de seguridad de datos se debe almacenar en una ubicación de almacenamiento diferente, _BackupStore2_.

2. MyApp_B
    1. Crear una copia de seguridad de datos todos los domingos a las 8:00 A.M. para todas las particiones del servicio _SvcB1_. Cargar los datos de copia de seguridad en la ubicación _BackupStore1_.

    2. Crear una copia de seguridad de datos todos los días a las 8:00 A.M. para la partición _SvcB2_P1_. Cargar los datos de copia de seguridad en la ubicación _BackupStore1_.

Para afrontar estos requisitos de copia de seguridad de datos, se crean las directivas de copia de seguridad BP_1 a BP_5 y la copia de seguridad se habilita de la siguiente manera.
1. MyApp_A
    1. Cree una directiva de copia de seguridad, _BP_1_, con la programación de copia de seguridad basada en frecuencia, donde esta se establezca en 24 horas y el almacenamiento de copia de seguridad se configure para usar la ubicación de almacenamiento _BackupStore1_. Habilite esta directiva para la aplicación _MyApp_A_ con la API para [habilitar copia de seguridad de aplicación](/rest/api/servicefabric/sfclient-api-enableapplicationbackup). Esta acción habilita la copia de seguridad de datos mediante la directiva de copia de seguridad _BP_1_ para todas las particiones de _servicios con estado de confianza_ y _Reliable Actors_ que pertenezcan a la aplicación _MyApp_A_.

    2. Cree una directiva de copia de seguridad, _BP_2_, con la programación de copia de seguridad basada en frecuencia, donde esta se establezca en 1 hora y el almacenamiento de copia de seguridad se configure para usar la ubicación de almacenamiento _BackupStore1_. Habilite esta directiva para el servicio _SvcA3_ mediante la API para [habilitar copia de seguridad de servicio](/rest/api/servicefabric/sfclient-api-enableservicebackup). Esta acción reemplaza la directiva propagada _BP_1_ por la directiva de copia de seguridad habilitada explícitamente _BP_2_ para todas las particiones del servicio _SvcA3_ lo que da lugar a la copia de seguridad de datos con la directiva de copia de seguridad _BP_2_ para estas particiones.

    3. Cree una directiva de copia de seguridad, _BP_3_, con la programación de copia de seguridad basada en frecuencia, donde esta se establezca en 24 horas y el almacenamiento de copia de seguridad se configure para usar la ubicación de almacenamiento _BackupStore2_. Habilite esta directiva para la partición _SvcA1_P2_ mediante la API para [habilitar copia de seguridad de partición](/rest/api/servicefabric/sfclient-api-enablepartitionbackup). Esta acción reemplaza la directiva propagada _BP_1_ por la directiva de copia de seguridad habilitada explícitamente _BP_3_ para la partición _SvcA1_P2_.

2. MyApp_B
    1. Cree una directiva de copia de seguridad, _BP_4_, con programación copia de seguridad basada en tiempo donde el tipo de frecuencia de programación se establezca en semanal, los días de ejecución en domingo y las horas de ejecución en 8:00 AM. El almacenamiento de copia de seguridad se configura para usar la ubicación de almacenamiento _BackupStore1_. Habilite esta directiva para el servicio _SvcB1_ mediante la API para [habilitar copia de seguridad de servicio](/rest/api/servicefabric/sfclient-api-enableservicebackup). Esta acción habilita la copia de seguridad de datos con la directiva de copia de seguridad _BP_4_ para todas las particiones del servicio _SvcB1_.

    2. Cree una directiva de copia de seguridad, _BP_5_, con programación de copia de seguridad basada en tiempo donde el tipo de frecuencia de programación se establezca en diaria y las horas de ejecución en 8:00 AM. El almacenamiento de copia de seguridad se configura para usar la ubicación de almacenamiento _BackupStore1_. Habilite esta directiva para la partición _SvcB2_P1_ mediante la API para [habilitar copia de seguridad de partición](/rest/api/servicefabric/sfclient-api-enablepartitionbackup). Esta acción habilita la copia de seguridad de datos con la directiva de copia de seguridad _BP_5_ para la partición _SvcB2_P1_.

En el diagrama siguiente se representan las directivas de copia de seguridad habilitadas explícitamente y las directivas de copia de seguridad propagadas.

![Jerarquía de aplicación de Service Fabric][0]

## <a name="disable-backup"></a>Deshabilitar copias de seguridad
Cuando no es necesario realizar copias de seguridad, se pueden deshabilitar las directivas de copia de seguridad. La directiva de copia de seguridad habilitada en una _aplicación_ solo se puede deshabilitar en la misma _aplicación_ con la API para [deshabilitar copia de seguridad de aplicación](/rest/api/servicefabric/sfclient-api-disableapplicationbackup), la directiva de copia de seguridad habilitada en un _servicio_ se puede deshabilitar en el mismo _servicio_ con la API para [deshabilitar copia de seguridad de servicio](/rest/api/servicefabric/sfclient-api-disableservicebackup) y la directiva de copia de seguridad habilitada en una _partición_ se puede deshabilitar en la misma _partición_ con la API para [deshabilitar copia de seguridad de partición](/rest/api/servicefabric/sfclient-api-disablepartitionbackup).

* Cuando se deshabilita la directiva de copia de seguridad para una _aplicación_, se detienen todas las copias de seguridad de datos periódicas en ejecución como resultado de la propagación de la directiva de copia de seguridad a las particiones de servicio con estado de confianza o las particiones de Reliable Actor.

* Cuando se deshabilita la directiva de copia de seguridad para un _servicio_, se detienen todas las copias de seguridad de datos periódicas en ejecución como resultado de la propagación de la directiva de copia de seguridad a las particiones del _servicio_.

* Cuando se deshabilita la directiva de copia de seguridad para una _partición_, se detienen todas las copias de seguridad de datos periódicas en ejecución debido a la directiva de copia de seguridad de la partición.

* Al deshabilitar la copia de seguridad de una entidad (aplicación, servicio o partición), `CleanBackup` se puede establecer en _verdadero_ para eliminar todas las copias de seguridad del almacenamiento configurado.
    ```json
    {
        "CleanBackup": true 
    }
    ```

## <a name="suspend--resume-backup"></a>Suspender y reanudar la copia de seguridad
Determinadas situaciones pueden requerir la suspensión temporal de la copia de seguridad de datos periódica. En esta situación, en función de los requisitos, se puede usar la API para suspender la copia de seguridad en una _aplicación_, _servicio_ o _partición_. La suspensión de la copia de seguridad periódica es transitiva a través del subárbol de la jerarquía de la aplicación desde el punto en el que se aplica. 

* Cuando se aplica la suspensión en una _aplicación_ mediante la API para [suspender copia de seguridad de aplicación](/rest/api/servicefabric/sfclient-api-suspendapplicationbackup), se suspende la copia de seguridad de datos periódica en todos los servicios y las particiones de esta aplicación.

* Cuando se aplica la suspensión en un _servicio_ mediante la API para [suspender copia de seguridad de servicio](/rest/api/servicefabric/sfclient-api-suspendservicebackup), se suspende la copia de seguridad de datos periódica en todas las particiones de este servicio.

* Cuando se aplica la suspensión en una _partición_ mediante la API para [suspender copia de seguridad de partición](/rest/api/servicefabric/sfclient-api-suspendpartitionbackup), se suspende la copia de seguridad de datos periódica en las particiones de este servicio.

Cuando ya no sea necesaria la suspensión, la copia de seguridad de datos periódica se puede restaurar mediante la respectiva API para reanudar la copia de seguridad. Las copias de seguridad periódicas se deben reanudar en la misma _aplicación_, _servicio_ o _partición_ donde se suspendió.

* Si se aplicó la suspensión en una _aplicación_, se debe reanudar mediante la API para [reanudar copia de seguridad de aplicación](/rest/api/servicefabric/sfclient-api-resumeapplicationbackup). 

* Si se aplicó la suspensión en un _servicio_, se debe reanudar mediante la API para [reanudar copia de seguridad de servicio](/rest/api/servicefabric/sfclient-api-resumeservicebackup).

* Si se aplicó la suspensión en una _partición_, se debe reanudar mediante la API para [reanudar copia de seguridad de partición](/rest/api/servicefabric/sfclient-api-resumepartitionbackup).

### <a name="difference-between-suspend-and-disable-backups"></a>Diferencia entre suspender y deshabilitar copias de seguridad
Deshabilitar copias de seguridad debe usarse cuando las copias de seguridad ya no sean necesarias para una determinada aplicación, servicio o partición. Se puede invocar la solicitud de deshabilitar copias de seguridad junto con el parámetro de limpiar copias de seguridad establecido en verdadero, lo que significaría que también se eliminarían todas las copias de seguridad existentes. Sin embargo, suspender debe usarse en escenarios donde se desea desactivar las copias de seguridad temporalmente, como cuando se llena el disco local o se producen errores al cargar la copia de seguridad debido a un problema de red conocido. 

Mientras la deshabilitación solo se puede invocar en el nivel en el que la copia de seguridad se había habilitado explícitamente; sin embargo, la suspensión se puede aplicar en cualquier nivel en el que esté habilitada la copia de seguridad ya sea directamente o a través de herencia o jerarquía. Por ejemplo, si está habilitada la copia de seguridad en nivel de aplicación, la deshabilitación se puede invocar solo en el nivel de la aplicación; sin embargo, la suspensión se puede invocar en la aplicación, cualquier servicio o partición de esa aplicación. 

## <a name="auto-restore-on-data-loss"></a>Restaurar automáticamente en caso de pérdida de datos
La partición de servicio puede perder datos debido a errores inesperados. Por ejemplo, el disco de dos de las tres réplicas de una partición (incluida la réplica principal) sufre daños o se borra.

Cuando Service Fabric detecta que la partición está perdiendo datos, invoca al método de interfaz `OnDataLossAsync` en la partición y espera que la partición realice la acción necesaria para dejar de perder datos. En esta situación, si la directiva de copia de seguridad efectiva en la partición tiene la marca `AutoRestoreOnDataLoss` establecida en `true`, la restauración se desencadena automáticamente con la copia de seguridad más reciente disponible para esta partición.

## <a name="get-backup-configuration"></a>Obtener la configuración de copia de seguridad
Están disponibles API independientes para obtener la información de configuración de copia de seguridad en el ámbito de una _aplicación_, un _servicio_ y una _partición_. Estas son las API para [obtener información de configuración de copia de seguridad de aplicación](/rest/api/servicefabric/sfclient-api-getapplicationbackupconfigurationinfo), [obtener información de configuración de copia de seguridad de servicio](/rest/api/servicefabric/sfclient-api-getservicebackupconfigurationinfo) y [obtener información de configuración de copia de seguridad de partición](/rest/api/servicefabric/sfclient-api-getpartitionbackupconfigurationinfo), respectivamente. Principalmente, estas API devuelven la directiva de copia de seguridad aplicable, el ámbito en la que la directiva de copia de seguridad se aplica y los detalles de la suspensión de la copia de seguridad. La siguiente es una breve descripción sobre los resultados devueltos de estas API.

- Información de configuración de copia de seguridad de aplicación: proporciona los detalles de la directiva de copia de seguridad aplicada en la aplicación y todas las directivas reemplazadas en servicios y particiones que pertenecen a la aplicación. También incluye la información de la suspensión de la aplicación, los servicios y las particiones.

- Información de configuración de copia de seguridad de servicio: proporciona los detalles de la directiva de copia de seguridad efectiva en el servicio y el ámbito en que se aplicó esta directiva y todas las directivas reemplazadas en sus particiones. También incluye la información de la suspensión del servicio y las particiones.

- Información de configuración de copia de seguridad de partición: proporciona los detalles de la directiva de copia de seguridad efectiva en la partición y el ámbito en que se aplicó esta directiva. También incluye la información de la suspensión para las particiones.

## <a name="list-available-backups"></a>Listar copias de seguridad disponibles

Las copias de seguridad disponibles se pueden enumerar mediante la API para obtener la lista de copias de seguridad. El resultado de la llamada de API incluye elementos de información de copia de seguridad relacionados con todas las copias de seguridad disponibles en el almacenamiento de copia de seguridad, que se configura en la directiva de copia de seguridad aplicable. Se proporcionan distintas variantes de esta API para enumerar las copias de seguridad disponibles que pertenecen a una aplicación, servicio o partición. Estas API admiten la obtención de la copia de seguridad _más reciente_ disponible de todas las particiones aplicables o el filtrado de copias de seguridad en función de la _fecha de inicio_ y _fecha de finalización_.

Estas API también admiten la paginación de los resultados, cuando el parámetro _MaxResults_ se establece en un número entero positivo distinto de cero, la API devuelve los elementos de información de copia de seguridad _MaxResults_ máximos. En el caso de que haya más elementos de información de copia de seguridad que el valor de _MaxResults_, se devuelve un token de continuación. El parámetro de token de continuación válido se puede utilizar para obtener el siguiente conjunto de resultados. Cuando el valor de token de continuación válido se pasa a la siguiente llamada de la API, esta devuelve el siguiente conjunto de resultados. No se incluye ningún token de continuación en la respuesta cuando se devuelven todos los resultados disponibles.

La información resumida de las variantes admitidas es la siguiente.

- [Obtener lista de copia de seguridad de aplicación](/rest/api/servicefabric/sfclient-api-getapplicationbackuplist): devuelve una lista de copias de seguridad disponibles para todas las particiones que pertenecen a la aplicación de Service Fabric específica.

- [Obtener lista de copia de seguridad de servicio](/rest/api/servicefabric/sfclient-api-getservicebackuplist): devuelve una lista de copias de seguridad disponibles para todas las particiones que pertenecen al servicio de Service Fabric específico.
 
- [Obtener lista de copia de seguridad de partición](/rest/api/servicefabric/sfclient-api-getpartitionbackuplist): devuelve una lista de copias de seguridad disponibles para la partición especificada.

## <a name="next-steps"></a>Pasos siguientes
- [Referencia de la API REST de restauración de copias de seguridad](/rest/api/servicefabric/sfclient-index-backuprestore)

[0]: ./media/service-fabric-backuprestoreservice/backup-policy-association-example.png
