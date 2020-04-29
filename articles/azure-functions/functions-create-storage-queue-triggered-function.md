---
title: Creación de una función en Azure desencadenada por mensajes en cola
description: Use Azure Functions para crear una función sin servidor que se invoca mediante mensajes enviados a una cola de Azure Storage.
ms.assetid: 361da2a4-15d1-4903-bdc4-cc4b27fc3ff4
ms.topic: how-to
ms.date: 10/01/2018
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: 98f0290aad9971bdb0c0b265d96e96d8ac34b99d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "80756521"
---
# <a name="create-a-function-triggered-by-azure-queue-storage"></a>Crear una función desencadenada por Azure Queue Storage

Obtenga información sobre cómo crear una función que se desencadena cuando se envían mensajes a una cola de Azure Storage.

![Vea el mensaje en los registros.](./media/functions-create-storage-queue-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Prerequisites

- Descargue e instale el [Explorador de Microsoft Azure Storage](https://storageexplorer.com/).

- Suscripción a Azure. Si no tiene una, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-an-azure-function-app"></a>Creación de una Function App de Azure

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Function App creada correctamente.](./media/functions-create-first-azure-function/function-app-create-success.png)

Después, cree una función en la nueva Function App.

<a name="create-function"></a>

## <a name="create-a-queue-triggered-function"></a>Creación de una función desencadenada por el servicio Queue

1. Expanda su instancia de Function App y haga clic en el botón **+** , que se encuentra junto a **Functions**. Si se trata de la primera función de Function App, seleccione **En el portal** y, después, **Continuar**. En caso contrario, vaya al paso tres.

   ![Página de inicio rápido de Functions en Azure Portal](./media/functions-create-storage-queue-triggered-function/function-app-quickstart-choose-portal.png)

1. Elija **Más plantillas** y, a continuación, **Finish and view templates** (Finalizar y ver plantillas).

    ![Guía de inicio rápido de Functions para elegir más plantillas](./media/functions-create-storage-queue-triggered-function/add-first-function.png)

1. En el campo de búsqueda, escriba `queue` y, a continuación, elija la plantilla **Desencadenador de cola**.

1. Si se le solicita, seleccione **Instalar** para instalar la extensión de Azure Storage y cualquiera de las dependencias de la aplicación de funciones. Una vez finalizada correctamente la instalación, seleccione **Continuar**.

    ![Instalación de extensiones de enlace](./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal.png)

1. Utilice la configuración que se especifica en la tabla debajo de la imagen.

    ![Configuración de la función desencadenada por la cola de almacenamiento](./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal-2.png)

    | Configuración | Valor sugerido | Descripción |
    |---|---|---|
    | **Nombre** | Único en la Function App | Nombre de la función desencadenada por la cola. |
    | **Nombre de la cola**   | myqueue-items    | Nombre de la cola a la que se va a conectar en la cuenta de almacenamiento. |
    | **Conexión de la cuenta de almacenamiento** | AzureWebJobsStorage | Puede usar la conexión de cuenta de almacenamiento que ya usa la Function App o crear una nueva.  |    

1. Haga clic en **Crear** para crear la función.

Después, conéctese a su cuenta de Azure Storage y cree la cola de almacenamiento **myqueue-items**.

## <a name="create-the-queue"></a>Creación de la cola

1. En la función, haga clic en **Integrar**, expanda **Documentación** y copie los dos valores de **Nombre de cuenta** y **Clave de cuenta**. Use estas credenciales para conectarse a la cuenta de almacenamiento en el Explorador de Azure Storage. Si ya se ha conectado a la cuenta de almacenamiento, vaya al paso 4.

    ![Obtenga las credenciales de conexión de la cuenta de almacenamiento.](./media/functions-create-storage-queue-triggered-function/functions-storage-account-connection.png)

1. Ejecute la herramienta [Explorador de Microsoft Azure Storage](https://storageexplorer.com/), haga clic en el icono de conexión situado a la izquierda, seleccione **Use a storage account name and key** (Usar el nombre y la clave de una cuenta de almacenamiento) y haga clic en **Siguiente**.

    ![Ejecute la herramienta Explorador de la cuenta de almacenamiento.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-1.png)

1. Escriba los valores de **Nombre de cuenta** y **Clave de cuenta** del paso 1, haga clic en **Siguiente** y, después, en **Conectar**.

    ![Escriba las credenciales de almacenamiento y conéctese.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-2.png)

1. Expanda la cuenta de almacenamiento asociada, haga clic con el botón derecho en **Colas**, haga clic en **Crear cola**, escriba `myqueue-items` y, después, presione ENTRAR.

    ![Cree una cola de almacenamiento.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-create-queue.png)

Ahora que tiene una cola de almacenamiento, puede probar la función. Para ello, agregue un mensaje a la cola.

## <a name="test-the-function"></a>Prueba de la función

1. De nuevo en Azure Portal, vaya a la función. Expanda **Registros** en la parte inferior de la página y asegúrese de que el streaming de registros no está en pausa.

1. En el Explorador de Storage, expanda la cuenta de almacenamiento, **Colas** y **myqueue-items** y, después, haga clic en **Agregar mensaje**.

    ![Agregue un mensaje a la cola.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-add-message.png)

1. Escriba su mensaje "Hola mundo" en **Texto del mensaje** y haga clic en **Aceptar**.

1. Espere unos segundos y, después, vuelva a los registros de función para comprobar que se ha leído el mensaje nuevo de la cola.

    ![Vea el mensaje en los registros.](./media/functions-create-storage-queue-triggered-function/functions-queue-storage-trigger-view-logs.png)

1. En el Explorador de Storage, haga clic en **Actualizar** y compruebe que el mensaje se ha procesado y ya no está en la cola.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Ha creado una función que se ejecuta cuando se agrega un mensaje a una cola de almacenamiento. Para obtener más información sobre los desencadenadores de Queue Storage, vea [Enlaces de colas de Storage en Azure Functions](functions-bindings-storage-queue.md).

Ahora que ha creado su primera función, vamos a agregar un enlace de salida a la función que escribe un mensaje en otra cola.

> [!div class="nextstepaction"]
> [Agregar mensajes a una cola de Azure Storage mediante funciones](functions-integrate-storage-queue-output-binding.md)
