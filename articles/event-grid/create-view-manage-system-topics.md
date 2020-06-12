---
title: Creación, visualización y administración de temas del sistema en Azure Event Grid
description: En este artículo se muestra cómo ver el tema del sistema existente y crear temas del sistema de Azure Event Grid mediante Azure Portal.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 06/02/2020
ms.author: spelluru
ms.openlocfilehash: ac22afb351973397960e66c2a8fe86031e0b213a
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84316346"
---
# <a name="create-view-and-manage-system-topics-in-azure-event-grid"></a>Creación, visualización y administración de temas del sistema en Azure Event Grid
En este artículo se muestra cómo realizar las tareas siguientes:

- Creación de un tema del sistema
- Visualización de todos los temas del sistema existentes 
- Elimina un tema del sistema.
- Creación de una suscripción de eventos para un tema del sistema


## <a name="create-a-system-topic"></a>Creación de un tema del sistema
Puede crear un tema del sistema para un recurso de Azure de dos maneras:

- Mediante la página del recurso, por ejemplo, la página de la cuenta de almacenamiento o la página del espacio de nombres de Event Hubs. 
- Mediante la página **Event Grid System Topics** (Temas del sistema de Event Grid). 

Consulte [este inicio rápido](blob-event-quickstart-portal.md) para ver un ejemplo de cómo crear un tema del sistema mediante una página de recursos (pestaña **Eventos** de una página de recursos de Azure Portal). Los siguientes pasos son para crear un tema del sistema mediante la página **Event Grid System Topics** (Temas del sistema de Event Grid). 

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

> [!NOTE]
> Anteriormente, cuando creaba una suscripción para un evento generado por orígenes de Azure, el servicio Event Grid creaba automáticamente un tema del sistema con un nombre generado de forma aleatoria. Ahora, puede especificar un nombre para el tema del sistema al crear el tema. Este recurso de tema del sistema se puede usar para detectar métricas y registros de diagnóstico.

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
