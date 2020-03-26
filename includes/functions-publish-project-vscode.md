---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/12/2020
ms.author: glenga
ms.openlocfilehash: 256510f855256e648ae9203f46eb9f66c9ffaed6
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "77029057"
---
## <a name="publish-the-project-to-azure"></a>Publicar el proyecto en Azure

En esta sección, va a crear una aplicación de funciones y los recursos relacionados en su suscripción de Azure y, después, va a implementar el código. 

1. Seleccione el icono de Azure en la barra de actividades y después en el área **Azure: Functions**, seleccione el botón de **implementación en la aplicación de funciones**.

    ![Publicación del proyecto en Azure](media/functions-publish-project-vscode/function-app-publish-project.png)

1. Escriba la siguiente información cuando se le indique:

    + **Seleccione la suscripción**: elija la suscripción que desee usar. No se mostrará esta opción si solo tiene una suscripción.

    + **Seleccione la aplicación de funciones en Azure**: Elija `+ Create new Function App` (no `Advanced`). En este artículo no se admite el [flujo de publicación avanzada](../articles/azure-functions/functions-develop-vs-code.md#enable-publishing-with-advanced-create-options). 
    
    >[!IMPORTANT]
    > La publicación en una aplicación de función existente sobrescribe el contenido de esa aplicación en Azure. 
    
    + **Escriba un nombre único global para la aplicación de funciones**: Escriba un nombre que sea válido en una ruta de acceso de la dirección URL. El nombre que escriba se valida para asegurarse de que es único en Azure Functions. 
    
    ::: zone pivot="programming-language-python"
    + **Seleccione un entorno de ejecución**: Elija la versión de Python en la que se ha estado ejecutando localmente. Ejecute el comando `python --version` para comprobar la versión.
    ::: zone-end

    ::: zone pivot="programming-language-javascript,programming-language-typescript"
    + **Seleccione un entorno de ejecución**: Elija la versión de Node.js en la que se ha estado ejecutando localmente. Ejecute el comando `node --version` para comprobar la versión.
    ::: zone-end

    + **Seleccione una ubicación para los nuevos recursos**:  Para mejorar el rendimiento, elija una [región](https://azure.microsoft.com/regions/) cerca de usted. 
    
1.  Cuando se complete, se crearán los siguientes recursos de Azure en la suscripción:

    + **[Grupo de recursos](../articles/azure-resource-manager/management/overview.md)** : contiene todos los recursos de Azure creados. El nombre se basa en el nombre de la aplicación de función.
    + **[Cuenta de almacenamiento](../articles//storage/common/storage-introduction.md#types-of-storage-accounts)** : se crea una cuenta de almacenamiento estándar con un nombre único en función del nombre de la aplicación de función.
    + **[Plan de hospedaje](../articles/azure-functions/functions-scale.md)** : se crea un plan de consumo en la región Oeste de EE. UU. para hospedar la aplicación de función sin servidor.
    + **Aplicación de función**: el proyecto se implementa y ejecuta en esta aplicación de función nueva.
    + **Application Insights**: Se crea una instancia, que está conectada a la aplicación de funciones, en función del nombre de la función.

    Una vez que se haya creado la aplicación de función se mostrará una notificación y se aplicará el paquete de implementación. 
    
1. Seleccione **View Output** (Ver salida) en esta notificación para ver la creación y los resultados de la implementación, incluidos los recursos de Azure que ha creado. Si se pierde la notificación, seleccione el icono de campana en la esquina inferior derecha para verlo de nuevo.

    ![Creación de la notificación completa](media/functions-publish-project-vscode/function-create-notifications.png)
