---
title: Depuración local de funciones de Azure desencadenadas por Event Grid
description: Obtenga información sobre cómo depurar localmente funciones de Azure desencadenadas por un evento de Event Grid
author: craigshoemaker
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: cshoe
ms.openlocfilehash: fdd046a855c4e2114e96e7911a928165f808710c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91530551"
---
# <a name="azure-function-event-grid-trigger-local-debugging"></a>Depuración local de funciones de Azure desencadenadas por Event Grid

En este artículo se muestra cómo depurar una función local que controla un evento de Azure Event Grid generado por una cuenta de almacenamiento. 

## <a name="prerequisites"></a>Requisitos previos

- Cree o use una aplicación de función existente.
- Cree o use una cuenta de almacenamiento existente.
- Descargue [ngrok](https://ngrok.com/) para permitir que Azure llame a su función local.

## <a name="create-a-new-function"></a>Creación de una función

Abra la aplicación de función en Visual Studio, haga clic con el botón derecho en el nombre del proyecto en el Explorador de soluciones y, a continuación, haga clic en **Agregar > Nueva función de Azure**.

En la ventana *Nueva función de Azure*, seleccione **Desencadenador de la cuadrícula de eventos** y haga clic en **Aceptar**.

![Crear una nueva función](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-add-function.png)

Una vez creada la función, abra el archivo de código y copie la dirección URL comentada en la parte superior del archivo. Esta ubicación se utiliza al configurar el desencadenador de Event Grid.

![Ubicación de copia](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-copy-location.png)

A continuación, establezca un punto de interrupción en la línea que comienza por `log.LogInformation`.

![Establecimiento de punto de interrupción](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-set-breakpoint.png)


A continuación, **presione F5** para iniciar una sesión de depuración.

## <a name="allow-azure-to-call-your-local-function"></a>Permitir que Azure llame a su función local

Para interrumpir la depuración de una función en su máquina, debe habilitar una manera de que Azure se comunique con la función local desde la nube.

La utilidad [ngrok](https://ngrok.com/) proporciona una manera de que Azure llame a la función que se ejecuta en la máquina. Inicie *ngrok* con el comando siguiente:

```bash
ngrok http -host-header=localhost 7071
```
Al configurar la utilidad, la ventana de comandos debe ser similar a la que se muestra en la siguiente captura de pantalla:

![Captura de pantalla que muestra el símbolo del sistema después de iniciar la utilidad "ngrok".](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-ngrok.png)

Copie la dirección URL **HTTPS** generada al ejecutar *ngrok*. Este valor se utiliza al configurar el punto de conexión del desencadenador de Event Grid.

## <a name="add-a-storage-event"></a>Agregar un evento de almacenamiento

Abra Azure Portal, vaya a una cuenta de almacenamiento y haga clic en la opción **Eventos**.

![Agregar un evento de cuenta de almacenamiento](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-add-event.png)

En la ventana *Eventos*, haga clic en el botón **Suscripción de eventos**. En la ventana *Suscripción de eventos*, haga clic en el menú desplegable *Tipo de punto de conexión* y seleccione **Webhook**.

![Seleccionar el tipo de suscripción](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-event-subscription-type.png)

Una vez configurado el tipo de punto de conexión, haga clic en **Select an endpoint** (Seleccionar un punto de conexión) para configurar el valor del punto de conexión.

![Seleccionar el tipo de punto de conexión](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-event-subscription-endpoint.png)

El valor *Punto de conexión de suscriptor* se compone de tres valores diferentes. El prefijo es la dirección URL HTTPS generada por *ngrok*. El resto de la dirección URL procede de la dirección URL que se encuentra en el archivo de código de función, con el nombre de la función agregado al final. A partir de la dirección URL del archivo de código de función, la dirección URL de *ngrok* reemplaza a `http://localhost:7071` y el nombre de la función reemplaza a `{functionname}`.

La dirección URL final debe ser similar a la siguiente captura de pantalla:

![Selección de punto de conexión](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-event-subscription-endpoint-selection.png)

Una vez que haya escrito el valor adecuado, haga clic en **Confirmar selección**.

> [!IMPORTANT]
> Cada vez que inicie *ngrok*, se vuelve a generar la dirección URL HTTPS y cambia el valor. Por lo tanto, debe crear una nueva suscripción a eventos cada vez que exponga la función en Azure mediante *ngrok*.

## <a name="upload-a-file"></a>Cargar un archivo

Ahora puede cargar un archivo a su cuenta de almacenamiento para desencadenar un evento de Event Grid para que lo controle su función local. 

Abra [Explorador de Storage](https://azure.microsoft.com/features/storage-explorer/) y conéctelo a la cuenta de almacenamiento. 

- Expanda **Contenedores de blobs**. 
- Haga clic con el botón derecho en **Crear contenedor de blobs**.
- Asigne al contenedor el nombre **test**.
- Seleccione el contenedor *test*.
- Haga clic en el botón **Cargar**.
- Haga clic en **Cargar archivos**.
- Seleccione un archivo y cárguelo en el contenedor de blobs.

## <a name="debug-the-function"></a>Depurar la función

Una vez que Event Grid reconoce que se ha cargado un nuevo archivo en el contenedor de almacenamiento, se alcanza el punto de interrupción en la función local.

![Iniciar ngrok](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-breakpoint.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Para limpiar los recursos creados en este artículo, elimine el contenedor **test** de su cuenta de almacenamiento.

## <a name="next-steps"></a>Pasos siguientes

- [Automatización del cambio de tamaño de las imágenes cargadas mediante Event Grid](../event-grid/resize-images-on-storage-blob-upload-event.md)
- [Desencadenador de Event Grid para Azure Functions](./functions-bindings-event-grid.md)
