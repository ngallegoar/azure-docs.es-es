---
title: 'Inicio rápido: Lectura de datos capturados de la aplicación de Python: Azure Event Hubs'
description: 'Inicio rápido: Scripts que usan el SDK de Azure para Python a fin de demostrar la característica Event Hubs Capture.'
ms.topic: quickstart
ms.date: 06/23/2020
ms.openlocfilehash: 364ca789f560dc8fdae099b09c77946bc4ad5005
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86537231"
---
# <a name="quickstart-event-hubs-capture-walkthrough-python-azure-eventhub-version-1"></a>Inicio rápido: Tutorial de Event Hubs Capture: Python (azure-eventhub, versión 1)

Capture es una característica de Azure Event Hubs. Puede usar Capture para enviar automáticamente los datos de streaming que hay en un centro de eventos a una cuenta de Azure Blob Storage que prefiera. Esto facilita el procesamiento por lotes en datos de streaming en tiempo real. En este artículo se describe cómo utilizar Event Hubs Capture con Python. Para más información acerca de Capture de Event Hubs, consulte [Capture de Event Hubs mediante Azure Event Hubs][Overview of Event Hubs Capture].

En esta guía se usa el [SDK de Azure para Python](https://azure.microsoft.com/develop/python/) a fin de demostrar la característica Capture. El programa *sender.py* envía datos telemétricos del entorno simulados a Event Hubs en formato JSON. El centro de eventos usa la característica Capture para escribir estos datos en Blob Storage en lotes. La aplicación *capturereader.py* lee estos blobs, crea un archivo de anexos para cada uno de los dispositivos y escribe los datos en los archivos *.csv* de cada dispositivo.

> [!WARNING]
> Este inicio rápido es para la versión 1 del SDK de Python para Azure Event Hubs. Se recomienda [migrar](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/migration_guide.md) el código a la [versión 5 del SDK de Python](get-started-capture-python-v2.md).

En este tutorial realizará lo siguiente: 

> [!div class="checklist"]
> * Crear una cuenta de Azure Blob Storage y un contenedor en Azure Portal.
> * Habilitar Capture de Event Hubs y dirigirlo a su cuenta de almacenamiento.
> * Enviar datos al centro de eventos con un script de Python.
> * Leer y procesar los archivos de Capture de Event Hubs con otro script de Python.

## <a name="prerequisites"></a>Requisitos previos

- Python 3.4 o posterior, con `pip` instalado y actualizado.
  
- Suscripción a Azure. Si no tiene una, [cree una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
  
- Un espacio de nombres de Event Hubs activo y un centro de eventos, creados siguiendo las instrucciones que se indican en [Inicio rápido: Creación de un centro de eventos mediante Azure Portal](event-hubs-create.md). Tome nota del espacio de nombres y los nombres del centro de eventos para usarlos más adelante en este tutorial. 
  
  > [!NOTE]
  > Si ya tiene un contenedor de almacenamiento para usar, puede habilitar Capture y seleccionar el contenedor de almacenamiento al crear el centro de eventos. 
  > 
  
- El nombre de la clave de acceso compartido y el valor de clave principal de Event Hubs. Busque o cree estos valores en **Directivas de acceso compartido** en la página de Event Hubs. El nombre de la clave de acceso predeterminada es **RootManageSharedAccessKey**. Copie el nombre de clave de acceso y el valor de clave principal para usarlos más adelante en este tutorial. 

## <a name="create-an-azure-blob-storage-account-and-container"></a>Creación de una cuenta y un contenedor de Azure Blob Storage

Cree una cuenta de almacenamiento y un contenedor que se usarán para la captura. 

1. Inicie sesión en [Azure Portal][Azure portal].
2. En el panel de navegación izquierdo, seleccione **Cuentas de almacenamiento** y, en la pantalla **Cuentas de almacenamiento**, seleccione **Agregar**.
3. En la pantalla de creación de cuentas de almacenamiento, seleccione una suscripción y un grupo de recursos, y asigne un nombre a la cuenta de almacenamiento. Puede dejar las demás selecciones con sus valores predeterminados. Seleccione **Revisar y crear**, revise la configuración y, después, seleccione **Crear**. 
   
   ![Crear cuenta de almacenamiento][1]
   
4. Una vez finalizada la implementación, seleccione **Ir al recurso** y, en la pantalla **Información general** de la cuenta de almacenamiento, seleccione **Contenedores**.
5. En la pantalla **Contenedores**, seleccione **+ Contenedor**. 
6. En la pantalla **Nuevo contenedor**, asigne un nombre al contenedor y, a continuación, seleccione **Aceptar**. Tome nota del nombre del contenedor para usarlo más adelante en el tutorial. 
7. En el panel de navegación izquierdo de la pantalla **Contenedores**, seleccione **Claves de acceso**. Copie el **Nombre de cuenta de almacenamiento** y el valor de **Clave** en **key1** (clave1), para usarlo más adelante en el tutorial.
 
## <a name="enable-event-hubs-capture"></a>Habilitación de Capture de Event Hubs

1. En Azure Portal, vaya al centro de eventos; para ello, seleccione su espacio de nombres de Event Hubs en **Todos los recursos**, seleccione **Event Hubs** en el panel de navegación izquierdo y, a continuación, seleccione el centro de eventos. 
2. En la pantalla **Información general** del centro de eventos, seleccione **Capturar eventos**.
3. En la pantalla **Capture**, seleccione **Activar**. A continuación, en **Contenedor de Azure Storage**, seleccione **Seleccionar el contenedor**. 
4. En la pantalla **Contenedores**, seleccione el contenedor de almacenamiento que quiera utilizar y, a continuación, seleccione **Seleccionar**. 
5. En la pantalla **Capture**, seleccione **Guardar cambios**. 

## <a name="create-a-python-script-to-send-events-to-event-hub"></a>Creación de un script de Python para enviar eventos a un centro de eventos
Este script le envía 200 eventos a un centro de eventos. Los eventos son lecturas simples del entorno enviadas en JSON.

1. Abra el editor de Python que prefiera, como [Visual Studio Code][Visual Studio Code].
2. Cree un nuevo archivo denominado *sender.py*. 
3. Pegue el código siguiente en *sender.py*. Sustituya sus propios valores por los valores \<namespace>, \<AccessKeyName>, \<primary key value> y \<eventhub> de Event Hubs.
   
   ```python
   import uuid
   import datetime
   import random
   import json
   from azure.servicebus.control_client import ServiceBusService
   
   sbs = ServiceBusService(service_namespace='<namespace>', shared_access_key_name='<AccessKeyName>', shared_access_key_value='<primary key value>')
   devices = []
   for x in range(0, 10):
       devices.append(str(uuid.uuid4()))
   
   for y in range(0,20):
       for dev in devices:
           reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
           s = json.dumps(reading)
           sbs.send_event('<eventhub>', s)
       print(y)
   ```
4. Guarde el archivo.

## <a name="create-a-python-script-to-read-capture-files"></a>Creación de un script de Python que lea archivos de Capture

Este script lee los archivos capturados y crea un archivo para cada dispositivo a fin de escribir los datos solo para dicho dispositivo.

1. En el editor de Python, cree un nuevo archivo denominado *capturereader.py*. 
2. Pegue el código siguiente en *capturereader.py*. Sustituya los valores guardados por sus valores \<storageaccount>, \<storage account access key> y \<storagecontainer>.
   
   ```python
   import os
   import string
   import json
   import avro.schema
   from avro.datafile import DataFileReader, DataFileWriter
   from avro.io import DatumReader, DatumWriter
   from azure.storage.blob import BlockBlobService
   
   def processBlob(filename):
       reader = DataFileReader(open(filename, 'rb'), DatumReader())
       dict = {}
       for reading in reader:
           parsed_json = json.loads(reading["Body"])
           if not 'id' in parsed_json:
               return
           if not parsed_json['id'] in dict:
               list = []
               dict[parsed_json['id']] = list
           else:
               list = dict[parsed_json['id']]
               list.append(parsed_json)
       reader.close()
       for device in dict.keys():
           deviceFile = open(device + '.csv', "a")
           for r in dict[device]:
               deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\n')
   
   def startProcessing(accountName, key, container):
       print('Processor started using path: ' + os.getcwd())
       block_blob_service = BlockBlobService(account_name=accountName, account_key=key)
       generator = block_blob_service.list_blobs(container)
       for blob in generator:
           #content_length == 508 is an empty file, so only process content_length > 508 (skip empty files)
           if blob.properties.content_length > 508:
               print('Downloaded a non empty blob: ' + blob.name)
               cleanName = str.replace(blob.name, '/', '_')
               block_blob_service.get_blob_to_path(container, blob.name, cleanName)
               processBlob(cleanName)
               os.remove(cleanName)
           block_blob_service.delete_blob(container, blob.name)
   startProcessing('<storageaccount>', '<storage account access key>', '<storagecontainer>')
   ```

## <a name="run-the-python-scripts"></a>Ejecución de los scripts de Python

1. Abra un símbolo del sistema que tenga Python en su ruta de acceso y ejecute dichos comandos para instalar los paquetes de requisitos previos de Python:
   
   ```cmd
   pip install azure-storage
   pip install azure-servicebus
   pip install avro-python3
   ```
   
   Si tiene una versión anterior de `azure-storage` o `azure`, es posible que tenga que utilizar la opción `--upgrade`.
   
   También puede que tenga que ejecutar el comando siguiente. La ejecución de este comando no es necesaria en la mayoría de los sistemas. 
   
   ```cmd
   pip install cryptography
   ```
   
2. Desde el directorio en el que guardó *sender.py* y *capturereader.py*, ejecute este comando:
   
   ```cmd
   start python sender.py
   ```
   
   Este comando inicia un nuevo proceso de Python para ejecutar el remitente.
   
3. Cuando la captura termine de ejecutarse, ejecute este comando:
   
   ```cmd
   python capturereader.py
   ```

   El procesador de captura descarga todos los blobs que no estén vacíos del contenedor de la cuenta de almacenamiento y escribe los resultados como archivos *.csv* en el directorio local. 

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Event Hubs, consulte: 

* [Información general de Event Hubs Capture][Overview of Event Hubs Capture]
* [Aplicaciones de ejemplo que usan Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)
* [Información general de Event Hubs][Event Hubs overview]

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-capture-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: ./event-hubs-about.md
