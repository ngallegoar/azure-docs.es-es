---
title: Creación, visualización y administración de temas del sistema en Azure Event Grid (Portal)
description: En este artículo se muestra cómo ver el tema del sistema existente y crear temas del sistema de Azure Event Grid mediante Azure Portal.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 2dbb0893668d72cfebf2d64e6515ff6ab6fed9bc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86115118"
---
# <a name="create-view-and-manage-event-grid-system-topics-in-the-azure-portal"></a>Creación, visualización y administración de temas del sistema de Event Grid en Azure Portal
En este artículo se muestra cómo crear y administrar temas del sistema en Azure Portal. Para ver un resumen de los temas del sistema, consulte [Temas del sistema](system-topics.md).

## <a name="create-a-system-topic"></a>Creación de un tema del sistema
Puede crear un tema del sistema para un recurso de Azure (cuenta de almacenamiento, espacio de nombres de Event Hubs, etc.) de dos maneras:

- Mediante la página **Eventos** de un recurso, por ejemplo, la cuenta de almacenamiento o el espacio de nombres de Event Hubs. Cuando use la página **Eventos** en Azure Portal para crear una suscripción de eventos para un evento generado por un origen de Azure (por ejemplo: cuenta de Azure Storage), el portal crea un tema del sistema para el recurso de Azure y, después, crea una suscripción para el tema del sistema. Especifique el nombre del tema del sistema si va a crear una suscripción de eventos en el recurso de Azure por primera vez. A partir de la segunda vez en adelante, el nombre del tema del sistema se muestra automáticamente en modo de solo lectura. Consulte [Quickstart: Enrutamiento de eventos de Blob Storage a un punto de conexión web personalizado con Azure Portal](blob-event-quickstart-portal.md#subscribe-to-the-blob-storage) para ver los pasos detallados.
- Mediante la página **Event Grid System Topics** (Temas del sistema de Event Grid). Los siguientes pasos son para crear un tema del sistema mediante la página **Event Grid System Topics** (Temas del sistema de Event Grid). 

1. Inicie sesión en el [portal de Azure](https://portal.azure.com).
2. En el cuadro de búsqueda de la parte superior, escriba **Event Grid System Topics** (Temas del sistema de Event Grid) y, luego, presione **Entrar**. 

    ![Búsqueda de temas del sistema](./media/create-view-manage-system-topics/search-system-topics.png)
3. En la página **Event Grid System Topics** (Temas del sistema de Event Grid), seleccione **+ Add** (+Agregar) en la barra de herramientas.

    ![Agregar tema del sistema: botón de la barra de herramientas](./media/create-view-manage-system-topics/add-system-topic-menu.png)
4. En la página **Create Event Grid System Topic** (Crear tema del sistema de Event Grid), siga estos pasos:
    1. Seleccione el **tipo de tema**. En el ejemplo siguiente, se selecciona la opción **Storage Accounts** (Cuentas de almacenamiento). 
    2. Seleccione la **suscripción de Azure** que tiene el recurso de cuenta de almacenamiento. 
    3. Seleccione el **grupo de recursos** que tiene la cuenta de almacenamiento. 
    4. Seleccione la **cuenta de almacenamiento**. 
    5. Escriba un **nombre** para el tema del sistema que se va a crear. 
    
        > [!NOTE]
        > Puede usarlo para realizar búsquedas en métricas y registros de diagnóstico.
    6. Seleccione **Revisar + crear**.

        ![Creación de un tema del sistema](./media/create-view-manage-system-topics/create-event-grid-system-topic-page.png)
    5. Revise la configuración y seleccione **Create** (Crear). 
        
        ![Revisión y creación del tema del sistema](./media/create-view-manage-system-topics/system-topic-review-create.png)
    6. Una vez que la implementación se haya realizado correctamente, seleccione **Ir al recurso** para ver la página **Event Grid System Topic** (Tema del sistema de Event Grid) correspondiente al tema del sistema que ha creado. 

        ![Página del tema del sistema](./media/create-view-manage-system-topics/system-topic-page.png)


## <a name="view-all-system-topics"></a>Visualización de todos los temas del sistema
Siga estos pasos para ver todos los temas del sistema de Event Grid. 

1. Inicie sesión en el [portal de Azure](https://portal.azure.com).
2. En el cuadro de búsqueda de la parte superior, escriba **Event Grid System Topics** (Temas del sistema de Event Grid) y, luego, presione **Entrar**. 

    ![Búsqueda de temas del sistema](./media/create-view-manage-system-topics/search-system-topics.png)
3. En la página **Event Grid System Topics** (Temas del sistema de Event Grid), verá todos los temas del sistema. 

    ![Lista de temas del sistema](./media/create-view-manage-system-topics/list-system-topics.png)
4. Seleccione un **tema del sistema** de la lista para ver detalles sobre él. 

    ![Detalles del tema del sistema](./media/create-view-manage-system-topics/system-topic-details.png)

    Esta página muestra detalles sobre el tema del sistema, como la siguiente información: 
    - Origen. Nombre del recurso en el que se creó el tema del sistema.
    - Tipo de origen. Tipo de recurso. Por ejemplo, `Microsoft.Storage.StorageAccounts`, `Microsoft.EventHub.Namespaces`, `Microsoft.Resources.ResourceGroups`, etc.
    - Todas las suscripciones creadas para el tema del sistema.

    Esta página permite operaciones como las siguientes:
    - Crear una suscripción a eventos. Seleccione **+Event Subscription** (+Suscripción a evento) en la barra de herramientas. 
    - Eliminar una suscripción a eventos. Seleccione **Delete** (Eliminar) en la barra de herramientas. 
    - Agregar etiquetas para el tema del sistema. Seleccione **Tags** (Etiquetas) en el menú de la izquierda y especifique los nombres y valores de las etiquetas. 


## <a name="delete-a-system-topic"></a>Elimina un tema del sistema.
1. Siga las instrucciones de la sección [Visualización de temas del sistema](#view-all-system-topics) para ver todos los temas del sistema y seleccione el tema del sistema que quiera eliminar de la lista. 
2. En la página **Event Grid System Topic** (Tema del sistema de Event Grid), seleccione **Delete** (Eliminar) en la barra de herramientas. 

    ![Tema del sistema: botón para eliminar](./media/create-view-manage-system-topics/system-topic-delete-button.png)
3. En la página de confirmación, seleccione **OK** (Aceptar) para confirmar la eliminación. El tema del sistema se elimina y también todas las suscripciones de eventos relacionadas con él.  

## <a name="create-an-event-subscription"></a>Creación de una suscripción a evento
1. Siga las instrucciones de la sección [Visualización de temas del sistema](#view-all-system-topics) para ver todos los temas del sistema y seleccione el tema del sistema que quiera eliminar de la lista. 
2. En la página **Event Grid System Topic** (Tema del sistema de Event Grid), seleccione **+ Event Subscription** (+Suscripción a evento) en la barra de herramientas. 

    ![Tema del sistema: botón para agregar suscripción a evento](./media/create-view-manage-system-topics/add-event-subscription-button.png)
3. Confirme que los campos **Topic Type** (Tipo de tema), **Source Resource** (Recurso de origen) y **Topic Name** (Nombre del tema) se rellenan automáticamente. Escriba un nombre, seleccione un valor para **Endpoint Type** (Tipo de punto de conexión) y especifique el **punto de conexión**. Seleccione **Create** (Crear) para guardar la suscripción de eventos. 

    ![Tema del sistema: creación de suscripciones de eventos](./media/create-view-manage-system-topics/create-event-subscription.png)

## <a name="next-steps"></a>Pasos siguientes
Consulte los [temas del sistema de Azure Event Grid](system-topics.md) para más información sobre los temas del sistema y los tipos de temas admitidos en Azure Event Grid. 
