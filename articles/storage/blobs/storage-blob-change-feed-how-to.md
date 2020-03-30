---
title: Procesamiento de la fuente de cambios en Azure Blob Storage (versión preliminar) | Microsoft Docs
description: Obtenga información sobre cómo procesar los registros de fuente de cambios en una aplicación cliente de .NET.
author: normesta
ms.author: normesta
ms.date: 11/04/2019
ms.topic: article
ms.service: storage
ms.subservice: blobs
ms.reviewer: sadodd
ms.openlocfilehash: 75995eeb3f8255cb4c60d5be267f9c343edfea89
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74111854"
---
# <a name="process-change-feed-in-azure-blob-storage-preview"></a>Procesamiento de la fuente de cambios en Azure Blob Storage (versión preliminar)

La fuente de cambios proporciona registros de transacciones de todos los cambios que se producen en los blobs y en los metadatos de blobs de la cuenta de almacenamiento. En este artículo se muestra cómo leer los registros de la fuente de cambios con la biblioteca de procesadores de la fuente de cambios de blob.

Para más información sobre la fuente de cambios, consulte [Fuente de cambios en Azure Blob Storage (versión preliminar)](storage-blob-change-feed.md).

> [!NOTE]
> La fuente de cambios está en versión preliminar pública y se encuentra disponible en las regiones **westcentralus** y **westus2**. Para más información sobre esta característica junto con limitaciones y problemas conocidos, consulte [Compatibilidad con la fuente de cambios en Azure Blob Storage](storage-blob-change-feed.md). La biblioteca de procesadores de la fuente de cambios está sujeta a cambios entre este momento y cuando esta biblioteca esté disponible con carácter general.

## <a name="get-the-blob-change-feed-processor-library"></a>Obtención de la biblioteca de procesadores de la fuente de cambios de blob

1. En Visual Studio, agregue la dirección URL `https://azuresdkartifacts.blob.core.windows.net/azuresdkpartnerdrops/index.json` a los orígenes de paquetes de NuGet. 

   Para saber cómo hacerlo, consulte los [orígenes de paquetes](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio#package-sources).

2. En el administrador de paquetes de NuGet, busque el paquete **Microsoft.Azure.Storage.Changefeed** e instálelo en el proyecto. 

   Para saber cómo hacerlo, consulte [Búsqueda e instalación de un paquete](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio#find-and-install-a-package).

## <a name="connect-to-the-storage-account"></a>Conexión con la cuenta de almacenamiento

Para analizar la cadena de conexión, llame al método [CloudStorageAccount.TryParse](/dotnet/api/microsoft.azure.storage.cloudstorageaccount.tryparse). 

A continuación, cree un objeto que represente a Blob Storage en la cuenta de almacenamiento. Para ello, llame al método [CloudStorageAccount.CreateCloudBlobClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.blobaccountextensions.createcloudblobclient).

```cs
public bool GetBlobClient(ref CloudBlobClient cloudBlobClient, string storageConnectionString)
{
    if (CloudStorageAccount.TryParse
        (storageConnectionString, out CloudStorageAccount storageAccount))
        {
            cloudBlobClient = storageAccount.CreateCloudBlobClient();

            return true;
        }
        else
        {
            return false;
        }
    }
}
```

## <a name="initialize-the-change-feed"></a>Inicialización de la fuente de cambios

Agregue las instrucciones USING siguientes al inicio del archivo de código. 

```csharp
using Avro.Generic;
using ChangeFeedClient;
```

Luego, para crear una instancia de la clase **ChangeFeed**, llame al método **GetContainerReference**. Pase el nombre del contenedor de la fuente de cambios.

```csharp
public async Task<ChangeFeed> GetChangeFeed(CloudBlobClient cloudBlobClient)
{
    CloudBlobContainer changeFeedContainer =
        cloudBlobClient.GetContainerReference("$blobchangefeed");

    ChangeFeed changeFeed = new ChangeFeed(changeFeedContainer);
    await changeFeed.InitializeAsync();

    return changeFeed;
}
```

## <a name="reading-records"></a>Lectura de registros

> [!NOTE]
> La fuente de cambios es una entidad inmutable y de solo lectura en la cuenta de almacenamiento. Cualquier número de aplicaciones puede leer y procesar la fuente de cambios de manera simultáneamente y de manera independiente a su comodidad. Los registros no se quitan de la fuente de cambios cuando una aplicación los lee. El estado de lectura o de iteración de cada lector de consumo es independiente y solo lo mantiene la aplicación.

La manera más sencilla de leer registros es crear una instancia de la clase **ChangeFeedReader**. 

Este ejemplo recorre en iteración todos los registros de la fuente de cambios y, a continuación, imprime en la consola unos pocos valores de cada registro. 
 
```csharp
public async Task ProcessRecords(ChangeFeed changeFeed)
{
    ChangeFeedReader processor = await changeFeed.CreateChangeFeedReaderAsync();

    ChangeFeedRecord currentRecord = null;
    do
    {
        currentRecord = await processor.GetNextItemAsync();

        if (currentRecord != null)
        {
            string subject = currentRecord.record["subject"].ToString();
            string eventType = ((GenericEnum)currentRecord.record["eventType"]).Value;
            string api = ((GenericEnum)((GenericRecord)currentRecord.record["data"])["api"]).Value;

            Console.WriteLine("Subject: " + subject + "\n" +
                "Event Type: " + eventType + "\n" +
                "Api: " + api);
        }

    } while (currentRecord != null);
}
```

## <a name="resuming-reading-records-from-a-saved-position"></a>Reanudación de la lectura de registros a partir de una posición guardada

Puede optar por guardar la posición de lectura en la fuente de cambios y reanudar la iteración de los registros en el futuro. Puede guardar el estado de la iteración de la fuente de cambios en cualquier momento con el método **ChangeFeedReader.SerializeState()** . El estado es una **cadena** y la aplicación puede guardar ese estado en función del diseño de la aplicación (por ejemplo, en una base de datos o un archivo).

```csharp
    string currentReadState = processor.SerializeState();
```

Puede continuar recorriendo en iteración los registros a partir del último estado mediante la creación de **ChangeFeedReader** con el método **CreateChangeFeedReaderFromPointerAsync**.

```csharp
public async Task ProcessRecordsFromLastPosition(ChangeFeed changeFeed, string lastReadState)
{
    ChangeFeedReader processor = await changeFeed.CreateChangeFeedReaderFromPointerAsync(lastReadState);

    ChangeFeedRecord currentRecord = null;
    do
    {
        currentRecord = await processor.GetNextItemAsync();

        if (currentRecord != null)
        {
            string subject = currentRecord.record["subject"].ToString();
            string eventType = ((GenericEnum)currentRecord.record["eventType"]).Value;
            string api = ((GenericEnum)((GenericRecord)currentRecord.record["data"])["api"]).Value;

            Console.WriteLine("Subject: " + subject + "\n" +
                "Event Type: " + eventType + "\n" +
                "Api: " + api);
        }

    } while (currentRecord != null);
}

```

## <a name="stream-processing-of-records"></a>Flujo del procesamiento de los registros

Puede elegir procesar los registros de la fuente de cambios a medida que llegan. Consulte las [especificaciones](storage-blob-change-feed.md#specifications).

```csharp
public async Task ProcessRecordsStream(ChangeFeed changeFeed, int waitTimeMs)
{
    ChangeFeedReader processor = await changeFeed.CreateChangeFeedReaderAsync();

    ChangeFeedRecord currentRecord = null;
    while (true)
    {
        do
        {
            currentRecord = await processor.GetNextItemAsync();

            if (currentRecord != null)
            {
                string subject = currentRecord.record["subject"].ToString();
                string eventType = ((GenericEnum)currentRecord.record["eventType"]).Value;
                string api = ((GenericEnum)((GenericRecord)currentRecord.record["data"])["api"]).Value;

                Console.WriteLine("Subject: " + subject + "\n" +
                    "Event Type: " + eventType + "\n" +
                    "Api: " + api);
            }

        } while (currentRecord != null);

        await Task.Delay(waitTimeMs);
    }
}
```

## <a name="reading-records-within-a-time-range"></a>Lectura de registros dentro de un intervalo de tiempo

La fuente de cambios se organiza en segmentos por hora según la hora del evento de cambio. Consulte las [especificaciones](storage-blob-change-feed.md#specifications). Puede leer los registros de los segmentos de la fuente de cambios que se encuentran dentro de un intervalo de tiempo específico.

En este ejemplo se obtienen las horas de inicio de todos los segmentos. A continuación, recorre en iteración esa lista hasta que la hora de inicio sea posterior a la hora del último segmento consumible o posterior a la hora de finalización del intervalo deseado. 

### <a name="selecting-segments-for-a-time-range"></a>Selección de segmentos para un intervalo de tiempo

```csharp
public async Task<List<DateTimeOffset>> GetChangeFeedSegmentRefsForTimeRange
    (ChangeFeed changeFeed, DateTimeOffset startTime, DateTimeOffset endTime)
{
    List<DateTimeOffset> result = new List<DateTimeOffset>();

    DateTimeOffset stAdj = startTime.AddMinutes(-15);
    DateTimeOffset enAdj = endTime.AddMinutes(15);

    DateTimeOffset lastConsumable = (DateTimeOffset)changeFeed.LastConsumable;

    List<DateTimeOffset> segments = 
        (await changeFeed.ListAvailableSegmentTimesAsync()).ToList();

    foreach (var segmentStart in segments)
    {
        if (lastConsumable.CompareTo(segmentStart) < 0)
        {
            break;
        }

        if (enAdj.CompareTo(segmentStart) < 0)
        {
            break;
        }

        DateTimeOffset segmentEnd = segmentStart.AddMinutes(60);

        bool overlaps = stAdj.CompareTo(segmentEnd) < 0 && 
            segmentStart.CompareTo(enAdj) < 0;

        if (overlaps)
        {
            result.Add(segmentStart);
        }
    }

    return result;
}
```

### <a name="reading-records-in-a-segment"></a>Lectura de registros de un segmento

Puede leer registros de segmentos individuales o intervalos de segmentos.

```csharp
public async Task ProcessRecordsInSegment(ChangeFeed changeFeed, DateTimeOffset segmentOffset)
{
    ChangeFeedSegment segment = new ChangeFeedSegment(segmentOffset, changeFeed);
    await segment.InitializeAsync();

    ChangeFeedSegmentReader processor = await segment.CreateChangeFeedSegmentReaderAsync();

    ChangeFeedRecord currentRecord = null;
    do
    {
        currentRecord = await processor.GetNextItemAsync();

        if (currentRecord != null)
        {
            string subject = currentRecord.record["subject"].ToString();
            string eventType = ((GenericEnum)currentRecord.record["eventType"]).Value;
            string api = ((GenericEnum)((GenericRecord)currentRecord.record["data"])["api"]).Value;

            Console.WriteLine("Subject: " + subject + "\n" +
                "Event Type: " + eventType + "\n" +
                "Api: " + api);
        }

    } while (currentRecord != null);
}
```

## <a name="read-records-starting-from-a-time"></a>Lectura de registros a partir de una hora

Puede leer los registros de la fuente de cambios desde un segmento inicial hasta el final. De manera similar a la lectura de registros dentro de un intervalo de tiempo, puede enumerar los segmentos y elegir un segmento desde el cual iniciar la iteración.

En este ejemplo se obtiene el valor [DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset?view=netframework-4.8) del primer segmento que se va a procesar.

```csharp
public async Task<DateTimeOffset> GetChangeFeedSegmentRefAfterTime
    (ChangeFeed changeFeed, DateTimeOffset timestamp)
{
    DateTimeOffset result = new DateTimeOffset();

    DateTimeOffset lastConsumable = (DateTimeOffset)changeFeed.LastConsumable;
    DateTimeOffset lastConsumableEnd = lastConsumable.AddMinutes(60);

    DateTimeOffset timestampAdj = timestamp.AddMinutes(-15);

    if (lastConsumableEnd.CompareTo(timestampAdj) < 0)
    {
        return result;
    }

    List<DateTimeOffset> segments = (await changeFeed.ListAvailableSegmentTimesAsync()).ToList();
    foreach (var segmentStart in segments)
    {
        DateTimeOffset segmentEnd = segmentStart.AddMinutes(60);
        if (timestampAdj.CompareTo(segmentEnd) <= 0)
        {
            result = segmentStart;
            break;
        }
    }

    return result;
}
```

En este ejemplo se procesan los registros de la fuente de cambios a partir del valor [DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset?view=netframework-4.8) de un segmento de inicio.

```csharp
public async Task ProcessRecordsStartingFromSegment(ChangeFeed changeFeed, DateTimeOffset segmentStart)
{
    TimeSpan waitTime = new TimeSpan(60 * 1000);

    ChangeFeedSegment segment = new ChangeFeedSegment(segmentStart, changeFeed);

    await segment.InitializeAsync();

    while (true)
    {
        while (!await IsSegmentConsumableAsync(changeFeed, segment))
        {
            await Task.Delay(waitTime);
        }

        ChangeFeedSegmentReader reader = await segment.CreateChangeFeedSegmentReaderAsync();

        do
        {
            await reader.CheckForFinalizationAsync();

            ChangeFeedRecord currentItem = null;
            do
            {
                currentItem = await reader.GetNextItemAsync();
                if (currentItem != null)
                {
                    string subject = currentItem.record["subject"].ToString();
                    string eventType = ((GenericEnum)currentItem.record["eventType"]).Value;
                    string api = ((GenericEnum)((GenericRecord)currentItem.record["data"])["api"]).Value;

                    Console.WriteLine("Subject: " + subject + "\n" +
                        "Event Type: " + eventType + "\n" +
                        "Api: " + api);
                }
            } while (currentItem != null);

            if (segment.timeWindowStatus != ChangefeedSegmentStatus.Finalized)
            {
                await Task.Delay(waitTime);
            }
        } while (segment.timeWindowStatus != ChangefeedSegmentStatus.Finalized);

        segment = await segment.GetNextSegmentAsync(); // TODO: What if next window doesn't yet exist?
        await segment.InitializeAsync(); // Should update status, shard list.
    }
}

private async Task<bool> IsSegmentConsumableAsync(ChangeFeed changeFeed, ChangeFeedSegment segment)
{
    if (changeFeed.LastConsumable >= segment.startTime)
    {
        return true;
    }
    await changeFeed.InitializeAsync();
    return changeFeed.LastConsumable >= segment.startTime;
}
```

>[!TIP]
> Un segmento puede tener registros de la fuente de cambios en una o más *chunkFilePath*. En caso de varias *chunkFilePath*, el sistema ha dividido de manera interna los registros en varias particiones para administrar el rendimiento de la publicación. Se garantiza que cada partición del segmento contendrá cambios para los blobs mutuamente excluyentes y que se podrá procesar de manera independiente sin infringir el orden. Puede usar la clase **ChangeFeedSegmentShardReader** para recorrer en iteración los registros en el nivel de partición si eso es más eficaz para su escenario.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre los registros de la fuente de cambios. Consulte [Fuente de cambios en Azure Blob Storage (versión preliminar)](storage-blob-change-feed.md)
