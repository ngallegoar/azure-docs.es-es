---
title: Envío o recepción de eventos desde Azure Event Hubs mediante Python (anteriores)
description: En este tutorial se muestra cómo crear y ejecutar scripts de Python que envíen eventos a Azure Event Hubs, o los reciban de él, mediante el anterior paquete azure-eventhub versión 1.
services: event-hubs
author: spelluru
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 01/15/2020
ms.author: spelluru
ms.custom: tracking-python
ms.openlocfilehash: 96c95efdc8f2154e0586fec59d1af66496acb101
ms.sourcegitcommit: 964af22b530263bb17fff94fd859321d37745d13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/09/2020
ms.locfileid: "84558961"
---
# <a name="quickstart-send-and-receive-events-with-event-hubs-using-python-azure-eventhub-version-1"></a>Inicio rápido: Envío y recepción de eventos con Azure Event Hubs mediante Python (azure-eventhub versión 1)
En este inicio rápido se muestra cómo enviar y recibir eventos desde un centro de eventos mediante el paquete de Python **azure-eventhub, versión 1**. 

> [!WARNING]
> En este inicio rápido se usa el antiguo paquete azure-eventhub, versión 1. Para ver un inicio rápido que use la **versión 5** más reciente del paquete, consulte [Envío y recepción de eventos mediante la versión 5 de azure-eventhub](get-started-python-send-v2.md). Para que la aplicación pase de usar el paquete antiguo a uno nuevo, consulte la [guía para migrar de la versión 1 a la versión 5 de azure-eventhub](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/migration_guide.md).
 

## <a name="prerequisites"></a>Requisitos previos
Si es la primera vez que usa Azure Event Hubs, consulte la [información general de Event Hubs](event-hubs-about.md) antes de continuar con este inicio rápido. 

Para completar este tutorial de inicio rápido, debe cumplir los siguientes requisitos previos:

- Una **suscripción a Microsoft Azure**. Para usar los servicios de Azure, entre los que se incluye Azure Event Hubs, se necesita una suscripción.  Si no se dispone de una cuenta de Azure, es posible registrarse para obtener una [evaluación gratuita](https://azure.microsoft.com/free/), o bien usar las ventajas que disfrutan los suscriptores MSDN al [crear una cuenta](https://azure.microsoft.com).
- Python 3.4 o posterior, con `pip` instalado y actualizado.
- El paquete de Python para Event Hubs. Para instalar el paquete, ejecute este comando en un símbolo del sistema que tenga Python en su ruta de acceso: 
  
  ```cmd
  pip install azure-eventhub==1.3.*
  ```
- **Creación de un espacio de nombres de Event Hubs y un centro de eventos**. El primer paso consiste en usar [Azure Portal](https://portal.azure.com) para crear un espacio de nombres de tipo Event Hubs y obtener las credenciales de administración que la aplicación necesita para comunicarse con el centro de eventos. Para crear un espacio de nombres y un centro de eventos, siga el procedimiento que se indica en [este artículo](event-hubs-create.md). Luego, para obtener el valor de la clave de acceso del centro de eventos, siga las instrucciones del artículo: [Obtenga la cadena de conexión](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Utilice la clave de acceso en el código que escribirá más adelante en este inicio rápido. El nombre de la clave predeterminada es: **RootManageSharedAccessKey**. 


## <a name="send-events"></a>Envío de eventos

Para crear una aplicación de Python que envíe eventos a un centro de eventos:

> [!NOTE]
> En lugar de trabajar con el inicio rápido, puede descargar y ejecutar las [aplicaciones de ejemplo](https://github.com/Azure/azure-event-hubs-python/tree/master/examples) de GitHub. Reemplace las cadenas `EventHubConnectionString` y `EventHubName` por los valores del centro de eventos.

1. Abra el editor de Python que prefiera, como [Visual Studio Code](https://code.visualstudio.com/).
2. Cree un nuevo archivo denominado *send.py*. Este script envía 100 eventos a un centro de eventos.
3. Pegue el código siguiente en *send.py*, y reemplace los valores de \<namespace>, \<eventhub>, \<AccessKeyName> y \<primary key value> de Event Hubs por sus valores: 
   
   ```python
   import sys
   import logging
   import datetime
   import time
   import os
   
   from azure.eventhub import EventHubClient, Sender, EventData
   
   logger = logging.getLogger("azure")
   
   # Address can be in either of these formats:
   # "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<namespace>.servicebus.windows.net/eventhub"
   # "amqps://<namespace>.servicebus.windows.net/<eventhub>"
   # SAS policy and key are not required if they are encoded in the URL
   
   ADDRESS = "amqps://<namespace>.servicebus.windows.net/<eventhub>"
   USER = "<AccessKeyName>"
   KEY = "<primary key value>"
   
   try:
       if not ADDRESS:
           raise ValueError("No EventHubs URL supplied.")
   
       # Create Event Hubs client
       client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
       sender = client.add_sender(partition="0")
       client.run()
       try:
           start_time = time.time()
           for i in range(100):
               print("Sending message: {}".format(i))
               message = "Message {}".format(i)
               sender.send(EventData(message))
       except:
           raise
       finally:
           end_time = time.time()
           client.stop()
           run_time = end_time - start_time
           logger.info("Runtime: {} seconds".format(run_time))
   
   except KeyboardInterrupt:
       pass
   ```
   
4. Guarde el archivo. 

Para ejecutar el script, desde el directorio en el que guardó *send.py*, ejecute este comando:

```cmd
start python send.py
```

Felicidades. Ha enviado mensajes a un centro de eventos.

## <a name="receive-events"></a>Recepción de eventos

Para crear una aplicación de Python que reciba eventos de un centro de eventos:

1. En el editor de Python, cree un archivo llamado *recv.py*.
2. Pegue el código siguiente en *recv.py*, y reemplace los valores de \<namespace>, \<eventhub>, \<AccessKeyName> y \<primary key value> de Event Hubs por sus valores: 
   
   ```python
   import os
   import sys
   import logging
   import time
   from azure.eventhub import EventHubClient, Receiver, Offset
   
   logger = logging.getLogger("azure")
   
   # Address can be in either of these formats:
   # "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.windows.net/myeventhub"
   # "amqps://<namespace>.servicebus.windows.net/<eventhub>"
   # SAS policy and key are not required if they are encoded in the URL
   
   ADDRESS = "amqps://<namespace>.servicebus.windows.net/<eventhub>"
   USER = "<AccessKeyName>"
   KEY = "<primary key value>"
   
   
   CONSUMER_GROUP = "$default"
   OFFSET = Offset("-1")
   PARTITION = "0"
   
   total = 0
   last_sn = -1
   last_offset = "-1"
   client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
   try:
       receiver = client.add_receiver(
           CONSUMER_GROUP, PARTITION, prefetch=5000, offset=OFFSET)
       client.run()
       start_time = time.time()
       for event_data in receiver.receive(timeout=100):
           print("Received: {}".format(event_data.body_as_str(encoding='UTF-8')))
           total += 1
   
       end_time = time.time()
       client.stop()
       run_time = end_time - start_time
       print("Received {} messages in {} seconds".format(total, run_time))
   
   except KeyboardInterrupt:
       pass
   finally:
       client.stop()
   ```
   
4. Guarde el archivo.

Para ejecutar el script, desde el directorio en el que guardó *recv.py*, ejecute este comando:

```cmd
start python recv.py
```

## <a name="next-steps"></a>Pasos siguientes
Para más información acerca de Event Hubs, consulte los siguientes artículos:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Características y terminología de Azure Event Hubs](event-hubs-features.md)
- [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md)

