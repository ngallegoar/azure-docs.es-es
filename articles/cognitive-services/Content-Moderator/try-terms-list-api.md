---
title: 'Moderación de texto con listas de términos personalizados: Content Moderator'
titleSuffix: Azure Cognitive Services
description: Utilice List Management API para crear listas personalizadas de términos con el fin de utilizarlos con Text Moderation API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: pafarley
ms.openlocfilehash: f1dfe88741a304da92901d3997c746654336ef54
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85800047"
---
# <a name="moderate-with-custom-term-lists-in-the-api-console"></a>Moderar con listas de términos personalizadas en la consola de API

La lista global predeterminada de términos de Azure Content Moderator es suficiente para la mayoría de las necesidades de moderación de contenido. Sin embargo, puede que deba filtrar términos específicos de la organización. Por ejemplo, es aconsejable etiquetar los nombres de la competencia para revisarlos en profundidad. 

Utilice [List Management API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f) para crear listas personalizadas de términos con el fin de utilizarlos con Text Moderation API. La operación **Text - Screen** (Texto: filtrar) examina si hay palabras soeces en el texto y también compara el texto con listas negras compartidas y personalizadas.

> [!NOTE]
> Hay un límite máximo de **5 listas de términos** y cada lista **no debe superar los 10 000 términos**.
>

Se puede utilizar List Management API para realizar las tareas siguientes:
- Crea una lista.
- Agregar términos a una lista.
- Filtrar términos con los términos de una lista.
- Eliminar términos de una lista.
- Eliminar una lista.
- Editar información de la lista.
- Actualizar el índice para que los cambios en la lista se incluyan en un nuevo examen.

## <a name="use-the-api-console"></a>Uso de la consola de API

Antes de probar la API en la consola en línea, necesita la clave de suscripción. Esta clave está ubicada en la pestaña **Configuración**, en el cuadro **Ocp-Apim-Subscription-Key**. Para más información, consulte [Información general](overview.md).

## <a name="refresh-search-index"></a>Actualización del índice de búsqueda

Después de realizar cambios en una lista de términos, debe actualizar su índice para que los cambios se incluyan en análisis futuros. Este paso es similar a cómo un motor de búsqueda de escritorio (si está habilitado) o un motor de búsqueda en web actualiza continuamente su índice para incluir nuevos archivos o páginas.

1. En la [referencia de Term List Management API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), en el menú de la izquierda, seleccione **Term Lists** (Listas de términos) y, a continuación, seleccione **Refresh Search Index** (Actualizar índice de búsqueda). 

   Se abre la página **Term Lists - Refresh Search Index** (Listas de términos: actualizar índice de búsqueda).

2. En **Open API testing console** (Abrir consola de pruebas de API), seleccione la región que mejor describa su ubicación. 

   ![Selección de la región en la página Term Lists - Refresh Search Index (Listas de términos: actualizar índice de búsqueda)](images/test-drive-region.png)

   Se abre la consola de API **Term Lists - Refresh Search Index** (Listas de términos: actualizar índice de búsqueda).

3. En el cuadro **listId**, escriba el identificador de lista. Escriba la clave de suscripción y, a continuación, seleccione **Enviar**.

   ![Cuadro de contenido de la respuesta de la consola de API Term Lists - Refresh Search Index (Listas de términos: actualizar índice de búsqueda)](images/try-terms-list-refresh-1.png)

## <a name="create-a-term-list"></a>Crear una lista de términos
1. Vaya a la [referencia de Term List Management API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f). 

   Se abre la página **Term Lists - Create** (Listas de términos: crear).

2. En **Open API testing console** (Abrir consola de pruebas de API), seleccione la región que mejor describa su ubicación. 

   ![Selección de la región en la página Term Lists - Create (Listas de términos: crear)](images/test-drive-region.png)

   Se abre la consola de API **Term Lists - Create** (Listas de términos: crear).
 
3. En el cuadro **Ocp-Apim-Subscription-Key**, especifique la clave de suscripción.

4. En el cuadro **Cuerpo de la solicitud**, escriba los valores para **Nombre** (por ejemplo, MyList) y **Descripción**.

   ![Nombre y descripción del cuerpo de la solicitud de la consola Terms Lists - Create (Listas de términos: crear)](images/try-terms-list-create-1.png)

5. Utilice marcadores de posición de par clave-valor para asignar más metadatos descriptivos a la lista.

    ```json
    {
        "Name": "MyExclusionList",
        "Description": "MyListDescription",
        "Metadata": 
        {
            "Category": "Competitors",
            "Type": "Exclude"
        }
    }
    ```

   Agregue metadatos de lista como pares de clave-valor y no términos reales.
 
6. Seleccione **Enviar**. Se crea una lista. Tenga en cuenta que el valor de **ID** está asociado a la nueva lista. Necesitará este identificador para otras funciones de administración de la lista de términos.

   ![Cuadro de contenido de la respuesta de la consola Term Lists - Create (Listas de términos: crear) que muestra la id. de lista](images/try-terms-list-create-2.png)
 
7. Agregar términos a MyList. En el menú izquierdo, en **Term** (Término), seleccione **Add Term** (Agregar término). 

   Se abre la página **Term - Add Term** (Término: agregar término). 

8. En **Open API testing console** (Abrir consola de pruebas de API), seleccione la región que mejor describa su ubicación. 

   ![Selección de región en la página Term - Add Term (Término: agregar término)](images/test-drive-region.png)

   Se abre la consola de API **Term - Add Term** (Término: agregar término).
 
9. En el cuadro **listId**, escriba la id. de lista que generó y seleccione un valor para **language**. Escriba la clave de suscripción y, a continuación, seleccione **Enviar**.

   ![Parámetros de la consulta de consola Term - Add Term (Término: agregar término)](images/try-terms-list-create-3.png)
 
10. Para comprobar que el término se ha agregado a la lista, en el menú izquierdo, seleccione **Term** (Término) y, a continuación, seleccione **Get All Terms** (Obtener todos los términos). 

    Se abre la consola de API **Term - Get All Terms** (Término: obtener todos los términos).

11. En el cuadro **listId**, escriba el identificador de lista y, a continuación, escriba la clave de suscripción. Seleccione **Enviar**.

12. En el cuadro **Contenido de la respuesta**, compruebe los términos que especificó.

    ![Cuadro de contenido de la respuesta de la consola Term - Get All Terms (Términos: obtener todos los términos) que lista los términos que especificó](images/try-terms-list-create-4.png)
 
13. Agregue unos cuantos términos más. Ahora que ha creado una lista personalizada de términos, intente [analizar parte de texto](try-text-api.md) con la lista de términos personalizados. 

## <a name="delete-terms-and-lists"></a>Eliminar términos y listas

La eliminación de un término o una lista es sencilla. Se utiliza la API para realizar las siguientes tareas:

- Eliminar un término. [(**Term - Delete**) (Término: eliminar)]
- Eliminar todos los términos de una lista sin eliminar la lista. [(**Term - Delete All Terms**) (Término: eliminar todos los términos)]
- Eliminar una lista y todo su contenido. [(**Term Lists - Delete**) (Listas de términos: eliminar)]

En este ejemplo se elimina un único término.

1. En la [referencia de Term List Management API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), en el menú de la izquierda, seleccione **Term** (Término) y, a continuación, seleccione **Eliminar**. 

   Se abre **Term - Delete** (Término: eliminar).

2. En **Open API testing console** (Abrir consola de pruebas de API), seleccione la región que mejor describa su ubicación. 

   ![Selección de la región en la página Term - Delete (Término: eliminar)](images/test-drive-region.png)

   Se abre la consola de API **Term - Delete** (Término: eliminar).
  
3. En el cuadro **listId**, escriba la id. de lista de la que quiere eliminar un término. Este identificador es el número (en nuestro ejemplo, **122**) que se devuelve en la consola **Term Lists - Get Details** (Listas de términos: obtener detalles) para MyList. Escriba el término y seleccione un idioma.
 
   ![Parámetros de la consulta de la consola Term - Delete (Término: eliminar)](images/try-terms-list-delete-1.png)

4. Escriba la clave de suscripción y, a continuación, seleccione **Enviar**.

5. Para comprobar que se ha eliminado el término, use la consola **Term Lists - Get All** (Listas de términos: obtener todos).

   ![Cuadro de contenido de la respuesta de la consola Term Lists - Get All (Listas de términos: obtener todos) que muestra que el término se ha eliminado](images/try-terms-list-delete-2.png)
 
## <a name="change-list-information"></a>Cambio de la información de la lista

Puede editar el nombre y la descripción de una lista y agregar elementos de metadatos.

1. En la [referencia de Term List Management API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), en el menú de la izquierda, seleccione **Term Lists** (Listas de términos) y, a continuación, seleccione **Update Details** (Actualizar detalles). 

   Se abre la página **Term Lists - Update Details** (Listas de términos: actualizar detalles).

2. En **Open API testing console** (Abrir consola de pruebas de API), seleccione la región que mejor describa su ubicación. 

   ![Selección de la región en la página Term Lists - Update Details (Listas de términos: actualizar detalles)](images/test-drive-region.png)

   Se abre la consola de API **Term Lists - Update Details** (Listas de términos: actualizar detalles).

3. En el cuadro **listId**, escriba el identificador de lista y, a continuación, escriba la clave de suscripción.

4. En el **Cuerpo de la solicitud**, realice las ediciones y, a continuación, seleccione **Enviar**.

   ![Ediciones del cuerpo de la solicitud de la consola Term Lists - Update Details (Listas de términos: actualizar detalles)](images/try-terms-list-change-1.png)
 

## <a name="next-steps"></a>Pasos siguientes

Usar la API de REST en el código o empezar en [Term lists .NET quickstart](term-lists-quickstart-dotnet.md) (Inicio rápido de las listas de términos con .NET) para integrar la aplicación.
