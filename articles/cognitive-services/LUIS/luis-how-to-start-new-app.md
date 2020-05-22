---
title: 'Creación de una aplicación: LUIS'
titleSuffix: Azure Cognitive Services
description: Cree y administre las aplicaciones en la página web de Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/07/2020
ms.author: diberry
ms.openlocfilehash: fc9f44739cd57eb46179ff17eba1d4f73d968799
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83585630"
---
# <a name="create-a-new-luis-app-in-the-luis-portal"></a>Creación de una aplicación de LUIS en el portal de LUIS
Hay un par de formas de crear aplicaciones de LUIS. Puede crear una aplicación de LUIS en el portal de LUIS o a través de las [API](developer-reference-resource.md) de creación de LUIS.

## <a name="using-the-luis-portal"></a>Mediante el portal de LUIS

Puede crear una aplicación nueva en el portal de varias maneras:

* Comience con una aplicación vacía y cree intenciones, expresiones y entidades.
* Comience con una aplicación vacía y agregue un [dominio creado previamente](luis-how-to-use-prebuilt-domains.md).
* Importe una aplicación de LUIS desde un archivo `.lu` o `.json` que ya contenga intenciones, expresiones y entidades.

## <a name="using-the-authoring-apis"></a>Mediante las API de creación
Puede crear una aplicación con las API de creación de dos maneras:

* [Agregar aplicación](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c2f): comience con una aplicación vacía y cree intenciones, expresiones y entidades.
* [Agregar aplicación de ejemplo](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/59104e515aca2f0b48c76be5): comience con un dominio precompilado que incluya intenciones, expresiones y entidades.


<a name="export-app"></a>
<a name="import-new-app"></a>
<a name="delete-app"></a>


[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

## <a name="create-new-app-in-luis"></a>Creación de una aplicación en LUIS

1. En la página **Mis aplicaciones**, seleccione su suscripción y el recurso de creación y luego **+ Create** (+Crear). Si usa una clave de evaluación gratuita, aprenda a [crear un recurso de creación](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal).

    ![Lista de aplicaciones de LUIS](./media/create-app-in-portal.png)


1. En el cuadro de diálogo, escriba el nombre de la aplicación, por ejemplo, `Pizza Tutorial`.

    ![Cuadro de diálogo para crear una aplicación](./media/create-pizza-tutorial-app-in-portal.png)

1. Elija la referencia cultural de la aplicación y seleccione **Listo**. En este momento, la descripción y el recurso de predicción son opcionales. Puede establecerlos en otro momento en la sección **Administrar** del portal.

    > [!NOTE]
    > La referencia cultural no se puede cambiar una vez creada la aplicación.

    Después de crear la aplicación, el portal de LUIS muestra la lista de **intenciones** con la intención `None` creada automáticamente. Ahora tiene una aplicación vacía.

    > [!div class="mx-imgBorder"]
    > ![Lista de intentos con la intención None creada sin intenciones de ejemplo.](media/pizza-tutorial-new-app-empty-intent-list.png)

## <a name="other-actions-available"></a>Otras acciones disponibles

La barra de herramientas de contexto ofrece otras acciones:

* Cambiar el nombre de la aplicación
* Importación desde el archivo con `.lu` o `.json`
* Exportación de la aplicación como `.lu` (para [LUDown](https://github.com/microsoft/botbuilder-tools/tree/master/packages/Ludown)), `.json` o `.zip` (para [contenedor LUIS](luis-container-howto.md))
* Importación de registros de punto de conexión de contenedor para revisar expresiones de punto de conexión
* Exportación de registros de punto de conexión, como `.csv`, para el análisis sin conexión
* Eliminar la aplicación

## <a name="next-steps"></a>Pasos siguientes

Si el diseño de la aplicación incluye la detección de intenciones, [cree otros intentos](luis-how-to-add-intents.md) y agregue expresiones de ejemplo. Si el diseño de la aplicación solo corresponde a extracción de datos, agregue expresiones de ejemplo la intención None, [cree entidades](luis-how-to-add-example-utterances.md) y etiquete las expresiones de ejemplo con esas entidades.
