---
title: Uso de revisiones para realizar cambios que no producen interrupciones de forma segura en API Management
titleSuffix: Azure API Management
description: Siga los pasos de este tutorial para obtener información sobre cómo realizar cambios que no producen interrupciones a través de revisiones en API Management.
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
ms.openlocfilehash: 7a4655b20fabcc72e02037de05dd0ef7c4671e52
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86254918"
---
# <a name="use-revisions-to-make-non-breaking-changes-safely"></a>Uso de revisiones para realizar cambios que no producen interrupciones de forma segura
Cuando la API esté lista y los desarrolladores empiecen a usarla, tendrá que realizar cambios en dicha API y, al mismo tiempo, no interrumpir a quienes la llaman. También resulta útil informar a los desarrolladores de los cambios realizados. Esto se logra en Azure API Management con las **revisiones**. Para más información, consulte [Versions & revisions](https://azure.microsoft.com/blog/versions-revisions/) (Versiones y revisiones) y [API Versioning with Azure API Management](https://azure.microsoft.com/blog/api-versioning-with-azure-api-management/) (Control de versiones con Azure API Management).

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Agregar una nueva revisión
> * Realizar cambios que no producen interrupciones en la revisión
> * Convertir la revisión en actual y agregar una entrada en el registro de cambios
> * Examinar el portal para desarrolladores para ver los cambios y registro de cambios

![Registro de cambios en el portal para desarrolladores](media/api-management-getstarted-revise-api/azure_portal.PNG)

## <a name="prerequisites"></a>Prerrequisitos

+ Conocer la [terminología de API Management de Azure](api-management-terminology.md).
+ Complete el siguiente inicio rápido: [Creación de una instancia de Azure API Management](get-started-create-service-instance.md).
+ Además, realice el siguiente tutorial: [Importación y publicación de la primera API](import-and-publish.md).

## <a name="add-a-new-revision"></a>Agregar una nueva revisión

![Agregar revisión de API](media/api-management-getstarted-revise-api/07-AddRevisions-01-AddNewRevision.png)

1. Seleccione la página **API**.
2. Seleccione **Demo Conference API** en la lista de API (u otra API a la que desee agregar revisiones).
3. Haga clic en la pestaña **Revisiones** en el menú junto a la parte superior de la página.
4. Seleccione **+ Agregar revisión**.

    > [!TIP]
    > También puede seleccionar **Agregar revisión** en el menú contextual ( **...** ) de la API.

5. Incluya una descripción de la nueva revisión que ayude a saber para qué va a servir.
6. Seleccione **Crear**
7. Ya tenemos creada la revisión.

    > [!NOTE]
    > La API original permanece en **Revisión 1**. Esta es la revisión que los usuarios siguen llamando hasta que decida convertir otra en actual.

## <a name="make-non-breaking-changes-to-your-revision"></a>Realizar cambios que no producen interrupciones en la revisión

![Modificar revisión](media/api-management-getstarted-revise-api/07-AddRevisions-02-MakeChanges.png)

1. Seleccione **Demo Conference API** en la lista de API.
2. Seleccione la pestaña **Diseño** situada junto a la parte superior de la pantalla.
3. Fíjese en que el **selector de revisiones** (que está inmediatamente encima de la pestaña de diseño) muestra que la revisión seleccionada es **Revisión 2**.

    > [!TIP]
    > Use el selector de revisión para alternar entre las revisiones con las que vaya a trabajar.

4. Seleccione **+ Agregar operación**.
5. Establezca que la nueva operación sea **POST** y especifique **test** como nombre, nombre para mostrar y dirección URL de la operación.
6. **Guarde** la nueva operación.
7. Ya hemos hecho un cambio en **Revisión 2**. Use el **selector de revisión** junto a la parte superior de la página para volver a **Revisión 1**.
8. Observe que la nueva operación no figura en **Revisión 1**. 

## <a name="make-your-revision-current-and-add-a-change-log-entry"></a>Convertir la revisión en actual y agregar una entrada en el registro de cambios

1. Seleccione la pestaña **Revisiones** en el menú junto a la parte superior de la página.

    ![El menú de revisión en la pantalla de revisión.](media/api-management-getstarted-revise-api/RevisionsMenu.PNG)

2. Abra el menú contextual ( **...** ) de **Revisión 2**.
3. Seleccione **Convertir en actual**.
4. Active la casilla **Post to Public Change log for this API** (Publicar en el registro de cambios público de esta API), si quiere publicar notas sobre este cambio. Especifique una descripción del cambio que puedan ver los desarrolladores, por ejemplo: **Revisiones de prueba. Se agregó una nueva operación de "prueba".**
5. **Revisión 2** es ahora la revisión actual.

## <a name="browse-the-developer-portal-to-see-changes-and-change-log"></a>Examinar el portal para desarrolladores para ver los cambios y registro de cambios

1. En Azure Portal, seleccione **API**.
2. Seleccione **Portal para desarrolladores** en el menú superior.
3. Seleccione **API** y, después, **Demo Conference API**.
4. Observe que la nueva operación **prueba** ahora aparece disponible.
5. Haga clic en **Registro de cambios** cerca del nombre de la API.
6. Vea que la entrada en el registro de cambios aparece en esta lista.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Agregar una nueva revisión
> * Realizar cambios que no producen interrupciones en la revisión
> * Convertir la revisión en actual y agregar una entrada en el registro de cambios
> * Examinar el portal para desarrolladores para ver los cambios y registro de cambios

Avance hasta el siguiente tutorial:

> [!div class="nextstepaction"]
> [Publicación de varias versiones de la API](api-management-get-started-publish-versions.md)
