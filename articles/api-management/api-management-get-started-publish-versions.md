---
title: Publicar versiones de la API con Azure API Management | Microsoft Docs
description: Siga los pasos de este tutorial para obtener información sobre cómo publicar varias versiones en API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/04/2019
ms.author: apimpm
ms.openlocfilehash: b683910180e597cb8cbfa642bb2d9ac3200b42ee
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86255037"
---
# <a name="publish-multiple-versions-of-your-api"></a>Publicación de varias versiones de la API 

A veces resulta poco práctico que todos los que llaman a la API usen exactamente la misma versión. Si alguien que llama a una API quiere actualizarla a una versión posterior, querrá hacerlo con un método que sea fácil de comprender. Esto se puede hacer en Azure API Management con las **versiones**. Para más información, consulte [Versions & revisions](https://azure.microsoft.com/blog/versions-revisions/) (Versiones y revisiones).

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Agregar una nueva versión a una API existente
> * Elegir un esquema de versión
> * Agregar la versión a un producto
> * Examinar el portal para desarrolladores para ver la versión

![Versión que se muestra en el portal para desarrolladores](media/api-management-getstarted-publish-versions/azure_portal.PNG)

## <a name="prerequisites"></a>Prerequisites

+ Conocer la [terminología de API Management de Azure](api-management-terminology.md).
+ Completar la guía de inicio rápido siguiente: [Creación de una instancia de Azure API Management](get-started-create-service-instance.md).
+ Además, completar el tutorial siguiente: [Importación y publicación de la primera API](import-and-publish.md).

## <a name="add-a-new-version"></a>Agregar una nueva versión

![Menú contextual de la API: agregar versión](media/api-management-getstarted-publish-versions/AddVersionMenu.png)

1. Seleccione **Demo Conference API** en la lista de API.
2. Seleccione el menú contextual ( **...** ) junto a ella.
3. Seleccione **+ Agregar versión**.

> [!TIP]
> Las versiones también se pueden habilitar al crear una API desde cero (para ello, seleccione **¿Definir versión de esta API?** en la pantalla **Agregar API**).

## <a name="choose-a-versioning-scheme"></a>Elegir un esquema de control de versiones

Con Azure API Management, puede elegir la forma en la que quiere que quienes llamen a una API puedan especificar qué versión de dicha API quieren. Especifique qué versión de la API va a usar, para lo que seleccionará un **esquema de control de versiones**. Este esquema puede ser una **ruta de acceso, un encabezado o una cadena de consulta**. En el ejemplo siguiente, la ruta de acceso se utiliza para seleccionar el esquema de control de versiones.

![Pantalla Agregar versión](media/api-management-getstarted-publish-versions/AddVersion.PNG)

1. Deje **ruta de acceso** seleccionada como **esquema de control de versiones**.
2. Escriba **demo-conference-api-v1** en el campo **Nombre**.

    > [!NOTE]
    > Una versión, de hecho, es una nueva API que se basa en la revisión de otra API. **Nombre** es el nuevo nombre de la API y debe ser único en la instancia de API Management.

3. Escriba **v1** en el campo **Identificador de la versión**.

    > [!TIP]
    > Si selecciona **encabezado** o **cadena de consulta** como esquema de control de versiones, debe proporcionar un valor más: el nombre del parámetro de encabezado o de cadena de consulta.

4. Seleccione **Crear** para configurar la nueva versión.
5. Debajo de la directiva **Demo Conference API** en la lista de API, ahora verá dos API distintas: **Original** y **v1**.

    ![Versiones que aparecen en una API en Azure Portal](media/api-management-getstarted-publish-versions/VersionList.PNG)

    > [!Note]
    > Si agrega una versión a una API sin control de versiones, se creará automáticamente un **Original** (que responde a la dirección URL predeterminada). Esto garantiza que quienes estén llamando en ese momento no sufran ninguna interrupción a causa del proceso de adición de la versión. Si crea una API con las versiones habilitadas desde el principio, no se creará un original.

6. Ahora puede modificar y configurar **v1** como una API independiente de **Original**. Los cambios en una versión no afectan a otra.

## <a name="add-the-version-to-a-product"></a>Agregar la versión a un producto

Para que quienes realizan las llamadas vean la versión nueva, debe agregarse a un **producto**.

![Productos de API Management](media/api-management-getstarted-publish-versions/08-AddMultipleVersions-03-AddVersionToProduct.png)

1. Seleccione **Productos** en la página del modelo de implementación clásica.
2. Seleccione **Ilimitado**.
3. Seleccione **API**.
4. Seleccione **Agregar**.
5. Seleccione **Demo Conference API, versión v1**.
6. Haga clic en **Seleccionar**.

## <a name="browse-the-developer-portal-to-see-the-version"></a>Examinar el portal para desarrolladores para ver la versión

1. Seleccione **Portal para desarrolladores** en el menú superior.
2. Seleccione **API** y haga clic en **Demo Conference API**.
3. Debe ver una lista desplegable con varias versiones junto al nombre de la API.
4. Seleccione **v1**.
5. Fíjese en la **Dirección URL de la solicitud** de la primera operación de la lista. Refleja que la ruta de acceso de la dirección URL de la API incluye **v1**.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Agregar una nueva versión a una API existente
> * Elegir un esquema de versión 
> * Agregar la versión a un producto
> * Examinar el portal para desarrolladores para ver la versión

Avance hasta el siguiente tutorial:

> [!div class="nextstepaction"]
> [Personalización del estilo de las páginas del portal para desarrolladores](api-management-customize-styles.md)
