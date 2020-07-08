---
title: 'Datos de ejemplo de Azure Blob Storage: proceso de ciencia de datos en equipos'
description: Para muestrear datos de ejemplo en Azure Blob Storage, deben descargarse mediante programación y luego realizar un muestreo de los mismos con procedimientos escritos en Python.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 04528d28e9f54710cd0a63372e32b099c2e07fb5
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86026175"
---
# <a name="sample-data-in-azure-blob-storage"></a><a name="heading"></a>Muestra de datos en el almacenamiento de blobs de Azure

En este artículo se tratan los datos de muestreo almacenados en el almacenamiento de blobs de Azure; para ello, se descargan mediante programación y luego se realiza un muestreo de los mismos con procedimientos escritos en Python.

**¿Por qué realizar un muestreo de los datos?**
Si el conjunto de datos que pretende analizar es grande, es recomendable reducirlo a un tamaño más pequeño, pero representativo, que sea más manejable. El muestreo facilita el reconocimiento y la exploración de los datos, así como el diseño de características. Su rol en el proceso de análisis de Cortana es permitir la rápida creación de prototipos de las funciones de procesamiento de datos y de los modelos de aprendizaje automático.

Esta tarea de muestreo es un paso en el [proceso de ciencia de datos en equipos (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/).

## <a name="download-and-down-sample-data"></a>Descarga y muestreado de datos
1. Descargue los datos del almacenamiento de blobs de Azure con Blob Service con el código de Python de ejemplo siguiente: 

    ```python
    from azure.storage.blob import BlobService
    import tables

    STORAGEACCOUNTNAME= <storage_account_name>
    STORAGEACCOUNTKEY= <storage_account_key>
    LOCALFILENAME= <local_file_name>        
    CONTAINERNAME= <container_name>
    BLOBNAME= <blob_name>

    #download from blob
    t1=time.time()
    blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
    t2=time.time()
    print(("It takes %s seconds to download "+blobname) % (t2 - t1))
    ```

2. Leer los datos en una trama de datos de Pandas desde el archivo descargado anteriormente.

    ```python
    import pandas as pd

    #directly ready from file on disk
    dataframe_blobdata = pd.read_csv(LOCALFILE)
    ```

3. Muestreo de los datos mediante `random.choice` de `numpy` como se indica a continuación:

    ```python
    # A 1 percent sample
    sample_ratio = 0.01 
    sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
    sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
    dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]
    ```

Ahora se puede trabajar con el marco de datos anterior, con el ejemplo del uno por ciento, para la generación y exploración de características más a fondo.

## <a name="upload-data-and-read-it-into-azure-machine-learning"></a><a name="heading"></a>Carga de datos y lectura en Azure Machine Learning
Puede usar el ejemplo de código siguiente para muestrear los datos y usarlos directamente en Azure Machine Learning:

1. Escribir la trama de datos en un archivo local

    ```python
    dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
    ```

2. Cargar el archivo local en un blob de Azure mediante el código de ejemplo siguiente:

    ```python
    from azure.storage.blob import BlobService
    import tables

    STORAGEACCOUNTNAME= <storage_account_name>
    LOCALFILENAME= <local_file_name>
    STORAGEACCOUNTKEY= <storage_account_key>
    CONTAINERNAME= <container_name>
    BLOBNAME= <blob_name>

    output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
    localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory

    try:

    #perform upload
    output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)

    except:            
        print ("Something went wrong with uploading to the blob:"+ BLOBNAME)
    ```

3. Lea los datos del blob de Azure con el módulo [Importar datos](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) de Azure Machine Learning, como se muestra en la imagen siguiente:

![lector de blobs](./media/sample-data-blob/reader_blob.png)

