---
title: Carga de archivos desde dispositivos a Azure IoT Hub con Python | Microsoft Docs
description: Cómo cargar archivos de un dispositivo a la nube mediante el SDK de dispositivo IoT de Azure para Python. Los archivos cargados se almacenan en un contenedor de blobs de Azure Storage.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: robinsh
ms.openlocfilehash: f1c0c046c40ff8edbc33c5e93e4207d9fe2fc67a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77110751"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-python"></a>Carga de archivos de un dispositivo a la nube con IoT Hub (Python)

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

En este tutorial, se describe cómo usar las [funcionalidades de carga de archivos de IoT Hub](iot-hub-devguide-file-upload.md) para cargar un archivo en [Azure Blob Storage](../storage/index.yml). En este tutorial se muestra cómo realizar las siguientes acciones:

* Proporcionar de forma segura un contenedor de almacenamiento para cargar un archivo.

* Usar el cliente de Python para cargar un archivo mediante IoT Hub.

En el inicio rápido sobre el [Envío de telemetría desde un dispositivo a un centro de IoT](quickstart-send-telemetry-python.md) se explica la funcionalidad básica de mensajería del dispositivo a la nube de IoT Hub. Sin embargo, en algunos casos no se pueden asignar fácilmente los datos de que los dispositivos envían en los mensajes de dispositivo a nube con un tamaño relativamente reducido que acepta Azure IoT Hub. Cuando necesite cargar archivos desde un dispositivo, todavía puede usar la seguridad y confiabilidad de IoT Hub.

> [!NOTE]
> El SDK de Python de IoT Hub solo admite actualmente la carga de archivos basados en caracteres como, por ejemplo, archivos **.txt**.

Al final de este tutorial, ejecutará la aplicación de consola de Python:

* **FileUpload.py**, que carga un archivo en el almacenamiento con el SDK de dispositivo de Python.

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

> [!NOTE]
> En esta guía se usa la versión 1 del SDK de Python (en desuso), ya que la característica de carga de archivos aún no se ha implementado en la nueva versión 2.

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE [iot-hub-include-python-installation-notes](../../includes/iot-hub-include-python-installation-notes.md)]

* Asegúrese de que está abierto el puerto 8883 del firewall. En el ejemplo de dispositivo de este artículo se usa el protocolo MQTT, que se comunica mediante el puerto 8883. Este puerto puede estar bloqueado en algunos entornos de red corporativos y educativos. Para más información y saber cómo solucionar este problema, consulte [Conexión a IoT Hub (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub).

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Carga de un archivo desde una aplicación de dispositivo

En esta sección, creará la aplicación para dispositivos para cargar un archivo en IoT Hub.

1. En el símbolo del sistema, ejecute el siguiente comando para instalar el paquete **azure-iothub-device-client**:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

2. Con un editor de texto, cree un archivo de prueba para cargarlo en Blob Storage.

    > [!NOTE]
    > El SDK de Python de IoT Hub solo admite actualmente la carga de archivos basados en caracteres como, por ejemplo, archivos **.txt**.

3. Con un editor de texto, cree un archivo **FileUpload.py** en la carpeta de trabajo.

4. Agregue las siguientes instrucciones y variables `import` al principio del archivo **FileUpload.py**. 

    ```python
    import time
    import sys
    import iothub_client
    import os
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError

    CONNECTION_STRING = "[Device Connection String]"
    PROTOCOL = IoTHubTransportProvider.HTTP

    PATHTOFILE = "[Full path to file]"
    FILENAME = "[File name for storage]"
    ```

5. En el archivo, sustituya `[Device Connection String]` por la cadena de conexión del dispositivo de IoT Hub. Sustituya `[Full path to file]` por la ruta de acceso del archivo de prueba que creó o de cualquier archivo del dispositivo que desee cargar. Sustituya `[File name for storage]` por el nombre que desee proporcionar al archivo una vez cargado en Blob Storage. 

6. Cree una devolución de llamada para la función **upload_blob**:

    ```python
    def blob_upload_conf_callback(result, user_context):
        if str(result) == 'OK':
            print ( "...file uploaded successfully." )
        else:
            print ( "...file upload callback returned: " + str(result) )
    ```

7. Agregue el siguiente código para conectar el cliente y cargar el archivo. Incluya también la rutina `main`:

    ```python
    def iothub_file_upload_sample_run():
        try:
            print ( "IoT Hub file upload sample, press Ctrl-C to exit" )

            client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

            f = open(PATHTOFILE, "r")
            content = f.read()

            client.upload_blob_async(FILENAME, content, len(content), blob_upload_conf_callback, 0)

            print ( "" )
            print ( "File upload initiated..." )

            while True:
                time.sleep(30)

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
        except:
            print ( "generic error" )

    if __name__ == '__main__':
        print ( "Simulating a file upload using the Azure IoT Hub Device SDK for Python" )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_file_upload_sample_run()
    ```

8. Guarde y cierre el archivo **UploadFile.py**.

## <a name="run-the-application"></a>Ejecución de la aplicación

Ahora está preparado para ejecutar la aplicación.

1. En un símbolo del sistema de la carpeta de trabajo, ejecute el siguiente comando:

    ```cmd/sh
    python FileUpload.py
    ```

2. En la captura de pantalla siguiente se muestra la salida de la aplicación **FileUpload**:

    ![Salida de la aplicación simulated-device](./media/iot-hub-python-python-file-upload/1.png)

3. Puede usar el portal para ver el archivo cargado en el contenedor de almacenamiento que configuró:

    ![Archivo cargado](./media/iot-hub-python-python-file-upload/2.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a usar la funcionalidad de carga de archivos de IoT Hub para simplificar la carga de archivos desde los dispositivos. Puede continuar explorando las características y los escenarios del centro de IoT con los siguientes artículos:

* [Creación de un centro de IoT mediante programación](iot-hub-rm-template-powershell.md)

* [Introducción a SDK para C](iot-hub-device-sdk-c-intro.md)

* [SDK de IoT de Azure](iot-hub-devguide-sdks.md)
