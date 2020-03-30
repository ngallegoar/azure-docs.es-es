---
title: 'Moderación de imágenes con listas personalizadas y la consola de API: Content Moderator'
titleSuffix: Azure Content Moderator
description: Use List Management API en Azure Content Moderator para crear listas personalizadas de imágenes.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: pafarley
ms.openlocfilehash: 27d9b12d9e1a0237050243c2b5f07edaa8d8857a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "72757190"
---
# <a name="moderate-with-custom-image-lists-in-the-api-console"></a>Moderación con listas de imágenes personalizadas en la consola de API

Use [List Management API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672) en Azure Content Moderator para crear listas de imágenes personalizadas. Utilice las listas de imágenes personalizadas con Image Moderation API. La operación de moderación de imágenes evalúa la imagen. Si crea listas personalizadas, la operación la compara también con las imágenes de las listas personalizadas. Puede utilizar listas personalizadas para bloquear o permitir la imagen.

> [!NOTE]
> Hay un límite máximo de **5 listas de imágenes** y cada una de ellas **no debe superar las 10 000 imágenes**.
>

Puede utilizar List Management API para realizar las tareas siguientes:

- Crea una lista.
- Agregar imágenes a una lista.
- Filtrar imágenes con imágenes de una lista.
- Eliminar imágenes de una lista.
- Eliminar una lista.
- Editar información de la lista.
- Actualizar el índice para que los cambios en la lista se incluyan en un nuevo examen.

## <a name="use-the-api-console"></a>Uso de la consola de API
Antes de probar la API en la consola en línea, necesita la clave de suscripción. Se encuentra en la pestaña **Configuración**, en el cuadro **Ocp-Apim-Subscription-Key**. Para más información, consulte [Información general](overview.md).

## <a name="refresh-search-index"></a>Actualización del índice de búsqueda

Después de realizar cambios en una lista de imágenes, debe actualizar su índice para que los cambios se incluyan en análisis futuros. Este paso es similar a cómo un motor de búsqueda de escritorio (si está habilitado) o un motor de búsqueda en web actualiza continuamente su índice para incluir nuevos archivos o páginas.

1. En la [referencia de Image List Management API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672), en el menú de la izquierda, seleccione **Image Lists** (Listas de imágenes) y, a continuación, seleccione **Refresh Search Index** (Actualizar índice de búsqueda).

   Se abre la página **Image Lists - Refresh Search Index** (Listas de imágenes: actualizar índice de búsqueda).

2. En **Open API testing console** (Abrir consola de pruebas de API), seleccione la región que mejor describa su ubicación. 
 
    ![Selección de la región en la página Image Lists - Refresh Search Index (Listas de imágenes: actualizar índice de búsqueda)](images/test-drive-region.png)

    Se abre la consola de API **Image Lists - Refresh Search Index** (Listas de imágenes: actualizar índice de búsqueda).

3. En el cuadro **listId**, escriba el identificador de lista. Escriba la clave de suscripción y, a continuación, seleccione **Enviar**.

   ![Cuadro de contenido de la respuesta de la consola de Image Lists - Refresh Search Index (Listas de imágenes: actualizar índice de búsqueda)](images/try-image-list-refresh-1.png)


## <a name="create-an-image-list"></a>Creación de una lista de imágenes

1. Vaya a la [referencia de Image List Management API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672).

   Se abre la página **Image Lists - Create** (Listas de imágenes: crear). 

3. En **Open API testing console** (Abrir consola de pruebas de API), seleccione la región que mejor describa su ubicación.

   ![Selección de la región en la página Image Lists - Create (Listas de imágenes: crear)](images/test-drive-region.png)

   Se abre la consola de API **Image Lists - Create** (Listas de imágenes: crear).
 
4. En el cuadro **Ocp-Apim-Subscription-Key**, especifique la clave de suscripción.

5. En el cuadro **Cuerpo de la solicitud**, escriba los valores para **Nombre** (por ejemplo, MyList) y **Descripción**.

   ![Nombre y descripción del cuerpo de la solicitud de la consola Image Lists - Create (Listas de imágenes: crear)](images/try-terms-list-create-1.png)

6. Utilice marcadores de posición de par clave-valor para asignar más metadatos descriptivos a la lista.

       {
          "Name": "MyExclusionList",
          "Description": "MyListDescription",
          "Metadata": 
          {
            "Category": "Competitors",
            "Type": "Exclude"
          }
       }

   Agregue metadatos de lista como pares clave-valor y no las imágenes reales.
 
7. Seleccione **Enviar**. Se crea una lista. Tenga en cuenta que el valor de **ID** está asociado a la nueva lista. Necesitará este identificador para otras funciones de administración de la lista de imágenes.

   ![Cuadro de contenido de la respuesta de la consola Image Lists - Create (Listas de imágenes: crear) que muestra el identificador de lista](images/try-terms-list-create-2.png)
 
8. A continuación, agregue imágenes a MyList. En el menú de la izquierda, seleccione **Imagen**y, a continuación, seleccione **Agregar imagen**.

   Se abre la página **Image - Add Image** (Imagen: agregar imagen). 

9. En **Open API testing console** (Abrir consola de pruebas de API), seleccione la región que mejor describa su ubicación.

   ![Selección de la región de la página Image - Add Image (Imagen: agregar imagen)](images/test-drive-region.png)

   Se abre la consola de API **Image - Add Image** (Imagen: agregar imagen).
 
10. En el cuadro **listId**, escriba el identificador de lista que generó y, a continuación, escriba la dirección URL de la imagen que desea agregar. Escriba la clave de suscripción y, a continuación, seleccione **Enviar**.

11. Para comprobar que la imagen se ha agregado a la lista, en el menú izquierdo, seleccione **Image** (Imagen) y, a continuación, seleccione **Get All Image Ids** (Obtener todos los identificadores de imagen).

    Se abre la consola de API **Image - Get All Image Ids** (Imagen: obtener todos los identificadores de imagen).
  
12. En el cuadro **listId**, escriba el identificador de lista y, a continuación, escriba la clave de suscripción. Seleccione **Enviar**.

    ![Cuadro de contenido de la respuesta de la consola Image - Get All Image Ids (Imágenes: obtener todos los identificadores de imagen) que enumera los términos que especificó](images/try-image-list-create-11.png)
 
10. Agregue unas cuantas imágenes más. Ahora que ha creado una lista personalizada de imágenes, intente [evaluar imágenes](try-image-api.md) mediante el uso de esta lista. 

## <a name="delete-images-and-lists"></a>Eliminación de imágenes y listas

La eliminación de una imagen o una lista es sencilla. Puede utilizar la API para realizar las siguientes tareas:

- Elimine una imagen. (**Image - Delete** [Imagen: eliminar])
- Eliminar todas las imágenes de una lista sin eliminar la lista. (**Image - Delete All Images** [Imagen: eliminar todas las imágenes])
- Eliminar una lista y todo su contenido. (**Image Lists - Delete** [Listas de imágenes: eliminar])

En este ejemplo se elimina una única imagen:

1. En la [referencia de Image List Management API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672), en el menú de la izquierda, seleccione **Image** (Imagen) y, a continuación, seleccione **Eliminar**. 

   Se abre la página **Image - Delete** (Imagen: eliminar).

2. En **Open API testing console** (Abrir consola de pruebas de API), seleccione la región que mejor describa su ubicación. 

   ![Selección de la región en la página Image - Delete (Imagen: eliminar)](images/test-drive-region.png)
 
   Se abre la consola de API **Image - Delete** (Imagen: eliminar).
 
3. En el cuadro **listId**, escriba el identificador de la lista de la que quiere eliminar un término.  Este es el número que se devuelve en la consola **Image - Get All Image Ids** (Imagen: obtener todos los identificadores de imagen) para MyList. A continuación, escriba el **ImageId** de la imagen que desea eliminar. 

En nuestro ejemplo, el identificador de lista es **58953**, el valor de **ContentSource**. El identificador de la imagen es **59021**, el valor de **ContentIds**.

1. Escriba la clave de suscripción y, a continuación, seleccione **Enviar**.

1. Para comprobar que se ha eliminado la imagen, use la consola **Image - Get All Image Ids** (Imagen: obtener todos los identificadores de imagen).
 
## <a name="change-list-information"></a>Cambio de la información de la lista

Puede editar el nombre y la descripción de una lista y agregar elementos de metadatos.

1. En la [referencia de Image List Management API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672), en el menú de la izquierda, seleccione **Image Lists** (Listas de imágenes) y, a continuación, seleccione **Update Details** (Actualizar detalles). 

   Se abre la página **Image Lists - Update Details** (Listas de imágenes: actualizar detalles).

2. En **Open API testing console** (Abrir consola de pruebas de API), seleccione la región que mejor describa su ubicación.  

    ![Selección de la región en la página Image Lists - Update Details (Listas de imágenes: actualizar detalles)](images/test-drive-region.png)

    Se abre la consola de API **Image Lists - Update Details** (Listas de imágenes: actualizar detalles).
 
3. En el cuadro **listId**, escriba el identificador de lista y, a continuación, escriba la clave de suscripción.

4. En el **Cuerpo de la solicitud**, realice las ediciones y, a continuación, seleccione el botón **Enviar** que aparece en la página.

   ![Ediciones del cuerpo de la solicitud de la consola Image Lists - Update Details (Listas de imágenes: actualizar detalles)](images/try-terms-list-change-1.png)
 

## <a name="next-steps"></a>Pasos siguientes

Usar la API REST en el código o empezar en [Image lists .NET quickstart](image-lists-quickstart-dotnet.md) (Inicio rápido de las listas de imágenes con .NET) para integrar la aplicación.
