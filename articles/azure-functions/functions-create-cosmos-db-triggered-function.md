---
title: Creación de una función desencadenada por Azure Cosmos DB
description: Use Azure Functions para crear una función sin servidor que se invoque cuando se agreguen datos a una base de datos de Azure Cosmos DB.
ms.assetid: bc497d71-75e7-47b1-babd-a060a664adca
ms.topic: how-to
ms.date: 10/02/2018
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 6045c61dc9837667bfaf01c685f687fcf5816e4c
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80754199"
---
# <a name="create-a-function-triggered-by-azure-cosmos-db"></a>Creación de una función desencadenada por Azure Cosmos DB

Aprenda a crear una función que se desencadena cuando se agregan o se cambian datos en Azure Cosmos DB. Para más información sobre Azure Cosmos DB, consulte [Azure Cosmos DB: informática de base de datos sin servidor con Azure Functions](../cosmos-db/serverless-computing-database.md).

![Vea el mensaje en los registros.](./media/functions-create-cosmos-db-triggered-function/quickstart-completed.png)

## <a name="prerequisites"></a>Prerequisites

Para completar este tutorial:

+ Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

> [!NOTE]
> [!INCLUDE [SQL API support only](../../includes/functions-cosmosdb-sqlapi-note.md)]

## <a name="create-an-azure-cosmos-db-account"></a>Creación de una cuenta de Azure Cosmos DB

Debe tener una cuenta de Azure Cosmos DB que use la API de SQL para poder crear el desencadenador.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="create-an-azure-function-app"></a>Creación de una Function App de Azure

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Después, cree una función en la nueva Function App.

<a name="create-function"></a>

## <a name="create-azure-cosmos-db-trigger"></a>Creación de un desencadenador de Azure Cosmos DB

1. Expanda su instancia de Function App y haga clic en el botón **+** , que se encuentra junto a **Functions**. Si se trata de la primera función de Function App, seleccione **En el portal** y, después, **Continuar**. En caso contrario, vaya al paso tres.

   ![Página de inicio rápido de Functions en Azure Portal](./media/functions-create-cosmos-db-triggered-function/function-app-quickstart-choose-portal.png)

1. Elija **Más plantillas** y, a continuación, **Finish and view templates** (Finalizar y ver plantillas).

    ![Guía de inicio rápido de Functions para elegir más plantillas](./media/functions-create-cosmos-db-triggered-function/add-first-function.png)

1. En el campo de búsqueda, escriba `cosmos` y seleccione la plantilla del **desencadenador de Azure Cosmos DB**.

1. Si se le solicita, seleccione **Instalar** para instalar la extensión de Azure Cosmos DB en la aplicación de función. Una vez finalizada correctamente la instalación, seleccione **Continuar**.

    ![Instalación de extensiones de enlace](./media/functions-create-cosmos-db-triggered-function/functions-create-cosmos-db-trigger-portal.png)

1. Configure el nuevo desencadenador según la configuración especificada en la tabla que aparece debajo de la imagen.

    ![Creación de la función desencadenada de Azure Cosmos DB](./media/functions-create-cosmos-db-triggered-function/functions-cosmosdb-trigger-settings.png)

    | Configuración      | Valor sugerido  | Descripción                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | **Nombre** | Valor predeterminado | Use el nombre de función predeterminado sugerido por la plantilla.|
    | **Conexión de la cuenta de Azure Cosmos DB** | Nueva configuración | Seleccione **Nuevo**, a continuación, elija su **suscripción**, la **cuenta de la base de datos** que creó anteriormente y **Seleccionar**. Con esto se crea una configuración de la aplicación para la conexión de la cuenta. Esta configuración la usa el enlace para establecer la conexión a la base de datos. |
    | **Nombre del contenedor** | Elementos | Nombre del contenedor que se va a supervisar. |
    | **Creación de un contenedor de concesiones si no existe** | Activado | El contenedor no existe, así que créelo. |
    | **Nombre de la base de datos** | Tareas | Nombre de la base de datos con el contenedor que se va a supervisar. |

1. Haga clic en **Crear** para crear la función desencadenada de Azure Cosmos DB. Después de crea la función, se muestra el código de función basado en la plantilla.  

    ![Plantilla de función de Cosmos DB en C#](./media/functions-create-cosmos-db-triggered-function/function-cosmosdb-template.png)

    Esta plantilla de función escribe el número de documentos y el identificador del primer documento en los registros.

A continuación, se conectará a la cuenta de Azure Cosmos DB y creará el contenedor `Items` en la base de datos `Tasks`.

## <a name="create-the-items-container"></a>Creación del contenedor de elementos

1. Abra una segunda instancia de [Azure Portal](https://portal.azure.com) en una nueva pestaña del explorador.

1. En el lado izquierdo del portal, expanda la barra de iconos, escriba `cosmos` en el campo de búsqueda y seleccione **Azure Cosmos DB**.

    ![Búsqueda del servicio Azure Cosmos DB](./media/functions-create-cosmos-db-triggered-function/functions-search-cosmos-db.png)

1. Elija la cuenta de Azure Cosmos DB y luego seleccione el **Explorador de datos**. 

1. En **SQL API**, elija la base de datos **Tasks** y seleccione **Nuevo contenedor**.

    ![Crear un contenedor](./media/functions-create-cosmos-db-triggered-function/cosmosdb-create-container.png)

1. En **Agregar contenedor**, use la configuración que se muestra en la tabla situada debajo de la imagen. 

    ![Definición del contenedor Tasks](./media/functions-create-cosmos-db-triggered-function/cosmosdb-create-container2.png)

    | Configuración|Valor sugerido|Descripción |
    | ---|---|--- |
    | **Identificador de base de datos** | Tareas |Nombre de la nueva base de datos. Debe coincidir con el nombre definido en el enlace de función. |
    | **ID de contenedor** | Elementos | El nombre del nuevo contenedor. Debe coincidir con el nombre definido en el enlace de función.  |
    | **[Clave de partición](../cosmos-db/partition-data.md)** | /categoría|Una clave de partición que distribuye los datos uniformemente a cada partición. La selección de la clave de partición correcta es importante al crear un contenedor con rendimiento. | 
    | **Rendimiento** |400 RU| Use el valor predeterminado. Si quiere reducir la latencia, puede escalar verticalmente el rendimiento más adelante. |    

1. Haga clic en **Aceptar** para crear el contenedor Items. Puede que el contenedor tarde un poco en crearse.

Una vez que el contenedor especificado en el enlace de función existe, puede probar la función mediante la adición de elementos a este nuevo contenedor.

## <a name="test-the-function"></a>Prueba de la función

1. Expanda el nuevo contenedor **Items** en Data Explorer, elija **Elementos** y, a continuación, seleccione **Nuevo elemento**.

    ![Creación de un elemento en el contenedor Items](./media/functions-create-cosmos-db-triggered-function/create-item-in-container.png)

1. Reemplace el contenido del nuevo elemento por el siguiente contenido, y luego elija **Guardar**.

        {
            "id": "task1",
            "category": "general",
            "description": "some task"
        }

1. Cambie a la primera pestaña del explorador que contiene la función del portal. Expanda los registros de función y compruebe que el nuevo documento ha desencadenado la función. Compruebe que el valor de identificador de documento `task1` se escribe en los registros. 

    ![Vea el mensaje en los registros.](./media/functions-create-cosmos-db-triggered-function/functions-cosmosdb-trigger-view-logs.png)

1. (Opcional) Vuelva a su documento, realice un cambio y haga clic en **Actualizar**. A continuación, vuelva a los registros de función y compruebe que la actualización también ha desencadenado la función.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Ha creado una función que se ejecuta cuando se agrega o modifica un documento en su instancia de Azure Cosmos DB. Para más información sobre los desencadenadores de Azure Cosmos DB, consulte [Enlaces de Azure Cosmos DB para Azure Functions](functions-bindings-cosmosdb.md).

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
