---
title: Procedimientos para realizar consultas de Azure Cognitive Search desde Power Apps
titleSuffix: Azure Cognitive Search
description: Instrucciones paso a paso sobre cómo crear un conector personalizado para Cognitive Search y cómo visualizarlo desde una aplicación de Power Apps.
author: luiscabrer
manager: eladz
ms.author: luisca
ms.service: cognitive-search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 03/25/2020
ms.openlocfilehash: c246f8652227a5ad2c0798880e530d6039cdeea8
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2020
ms.locfileid: "80385119"
---
# <a name="how-to-query-a-cognitive-search-index-from-power-apps"></a>Procedimientos para realizar consultas en un índice de Cognitive Search desde Power Apps

En este artículo se muestra cómo crear un conector personalizado de Power Apps para que pueda recuperar resultados de búsqueda desde un índice de búsqueda. También se explica cómo emitir una consulta de búsqueda y visualizar los resultados desde una aplicación de Power Apps. 

## <a name="prerequisites"></a>Prerrequisitos
*    Acceso a una cuenta de Power Apps con la capacidad de crear conectores personalizados.
*    Se da por hecho que ya ha creado un índice de Azure Search.

## <a name="create-a-custom-connector-to-query-azure-search"></a>Creación de un conector personalizado para consultar Azure Search

Debe seguir dos pasos principales para que una aplicación de Power Apps muestre los resultados de Azure Cognitive Search. En primer lugar, cree un conector que pueda consultar el índice de búsqueda. En la [siguiente sección](#visualize-results-from-the-custom-connector), actualizaremos la aplicación de Power Apps para poder visualizar los resultados devueltos por el conector.

1. Vaya a [make.powerapps.com](http://make.powerapps.com) y e **inicie sesión**.

1. Busque **Datos** > **Conectores personalizados**.
 
    :::image type="content" source="./media/search-howto-powerapps/1-2-custom-connector.png" alt-text="Menú con Conectores personalizados" border="true":::

1. Haga clic en **+ Nuevo conector personalizado** y, después, seleccione **Crear desde cero**.

    :::image type="content" source="./media/search-howto-powerapps/1-3-create-blank.png" alt-text="Menú Crear desde cero" border="true":::

1. Asigne un nombre al conector personalizado (por ejemplo, *AzureSearchQuery*) y, a continuación, haga clic en **Continuar**. Se abrirá un asistente para crear el nuevo conector.

1. Escriba la información general en la página correspondiente.

    - Color de fondo del icono (por ejemplo, #007ee5).
    - Descripción (por ejemplo, "Conector a Azure Cognitive Search").
    - En Host, tendrá que escribir la dirección URL del servicio de búsqueda (por ejemplo, `<yourservicename>.search.windows.net`).
    - Para la URL base, simplemente escriba "/".
    
    :::image type="content" source="./media/search-howto-powerapps/1-5-general-info.png" alt-text="Cuadro de diálogo Información general" border="true":::

1. En la página de seguridad, establezca *Clave de API* como **Tipo de autenticación**, y escriba *api-key* en los campos de etiqueta y nombre del parámetro. En el campo **Ubicación de parámetro**, seleccione *Encabezado*, tal y como se muestra a continuación.
 
    :::image type="content" source="./media/search-howto-powerapps/1-6-authentication-type.png" alt-text="Opción de tipo de autenticación" border="true":::

1. En la página de definiciones, seleccione **+ Nueva acción** para crear una acción que consultará el índice. Escriba el valor "Consulta" en el campo de resumen y el nombre del identificador de operación. Escriba una descripción, como "*Consulta el índice de búsqueda*".
 
    :::image type="content" source="./media/search-howto-powerapps/1-7-new-action.png" alt-text="Opciones de la nueva acción" border="true":::


1. Presione el botón **+ Importar desde ejemplo** para definir los parámetros y los encabezados. A continuación, definirá la solicitud de consulta.  

    * Seleccione el verbo `GET`.
    * En el campo URL, escriba una consulta de ejemplo para el índice de búsqueda, por ejemplo:
       
    >https://yoursearchservicename.search.windows.net/indexes/yourindexname/docs?search= *&api-version=2019-05-06-Preview
    

    **Power Apps** usará la sintaxis para extraer los parámetros de la consulta. Observe que hemos definido explícitamente el campo de búsqueda. 

    :::image type="content" source="./media/search-howto-powerapps/1-8-1-import-from-sample.png" alt-text="Importar desde ejemplo" border="false":::

1.  Haga clic en **Importar** para rellenar automáticamente el cuadro de diálogo Solicitud.

    :::image type="content" source="./media/search-howto-powerapps/1-8-2-import-from-sample.png" alt-text="Cuadro de diálogo Importar desde ejemplo" border="false":::


1. Para completar la configuración de los metadatos del parámetro, haga clic en el símbolo **…** situado junto a cada uno de los parámetros.

    - Parámetro *search*: Establezca `*` como el **valor predeterminado**; en **¿Es obligatorio?** , seleccione *No* y establezca la **visibilidad** en *ninguna*. 

    :::image type="content" source="./media/search-howto-powerapps/1-10-1-parameter-metadata-search.png" alt-text="Metadatos del parámetro de búsqueda" border="true":::

    - Parámetro *api-version*: Establezca `2019-05-06-Preview` como el **valor predeterminado**; seleccione que la **visibilidad** sea interna y, en **¿Es obligatorio?** , seleccione *Sí*.  

    :::image type="content" source="./media/search-howto-powerapps/1-10-2-parameter-metadata-version.png" alt-text="Metadatos del parámetro de versión" border="true":::

    - Del mismo modo, establezca el parámetro *api-key* como **obligatorio** y elija la opción *interna* para la **visibilidad**. Escriba la clave de API del servicio de búsqueda como el **valor predeterminado**.
    
    Después de realizar estas modificaciones, cambie a la vista **Editor de Swagger**. En la sección de parámetros debería ver la siguiente configuración:    

    ```
          parameters:
          - {name: search, in: query, required: false, type: string, default: '*'}
          - {name: api-version, in: query, required: true, type: string, default: 2019-05-06-Preview,
            x-ms-visibility: internal}
          - {name: api-key, in: header, required: true, type: string, default: YOURKEYGOESHERE,
            x-ms-visibility: internal}
    ```

1. En la sección de respuesta, haga clic en **Agregar respuesta predeterminada**. Esto es fundamental porque ayudará a **Power Apps** a comprender el esquema de la respuesta. Pegue una respuesta de ejemplo.

    > [!TIP] 
    > Hay un límite de caracteres para la respuesta JSON que puede escribir, por lo que es conveniente que la simplifique antes de pegarla. Un aspecto importante a tener en cuenta es el esquema o el formato de la respuesta. Los valores reales de la respuesta de ejemplo son menos importantes y se pueden simplificar para reducir el recuento de caracteres.
    

1.    Haga clic en el botón **Crear conector** en la parte superior derecha de la pantalla para poder probarlo.

1.  En la página prueba, haga clic en **+ Nueva conexión** y escriba la clave de consulta del servicio de búsqueda como el valor de *api-key*.

    Este paso puede hacerle salir del asistente y llevarle a la página de conexiones. Es posible que quiera volver al editor de conexiones personalizadas para probar la conexión. Vaya a **Conector personalizado** > seleccione el conector que acaba de crear > *…* > **Ver propiedades** > **Editar** > **4. Probar** para volver a la página de prueba.

1.    A continuación, haga clic en **Operación de prueba** para asegurarse de que obtiene resultados del índice. Si la operación se realiza correctamente, se devuelve un estado 200 y, en el cuerpo de la respuesta, debería ver un código JSON que describe los resultados de la búsqueda.




## <a name="visualize-results-from-the-custom-connector"></a>Visualización de resultados desde el conector personalizado
El objetivo de este tutorial es no enseñarle a crear experiencias de usuario sofisticadas con Power Apps, por lo que el diseño de la interfaz de usuario será minimalista. Vamos a crear una aplicación de Power Apps con un cuadro de búsqueda y un botón de búsqueda y vamos a mostrar los resultados en un control de galería.  La aplicación de Power Apps se conectará al conector personalizado que hemos creado para obtener los datos de Azure Search.

1. Cree una nueva aplicación de Power Apps. Vaya a la sección **Aplicaciones**, haga clic en **+ Nueva aplicación** y seleccione **Lienzo**.

    :::image type="content" source="./media/search-howto-powerapps/2-1-create-canvas.png" alt-text="Creación de una aplicación de lienzo" border="true":::

1. Seleccione el tipo de aplicación que quiera crear. En este tutorial, creará una **Aplicación en blanco** con **Diseño de teléfono**. Aparecerá la aplicación **Power Apps Studio**.

1. Una vez en Studio, seleccione la pestaña **Orígenes de datos** y haga clic en el nuevo conector que acaba de crear. En nuestro caso, este conector se denomina *AzureSearchQuery*. Haga clic en **Agregar una conexión**.

    :::image type="content" source="./media/search-howto-powerapps/2-3-connect-connector.png" alt-text="Conexión del conector" border="true":::

    Ahora *AzureSearchQuery* es un origen de datos que está disponible para su uso desde la aplicación.
    
1. Vaya a la pestaña **Insertar** para que podamos agregar algunos controles a nuestro formulario.

    :::image type="content" source="./media/search-howto-powerapps/2-4-add-controls.png" alt-text="Inserción de controles" border="true":::

1.  Inserte los siguientes elementos:
    -   Una etiqueta de texto con el valor "Consulta:".
    -   Un elemento de entrada de texto (llámelo *txtQuery* y establezca su valor predeterminado en "*").
    -   Un botón con el texto "Buscar". 
    -   Una galería vertical (llámela *galleryResults*).
    
    Su formulario debería tener el siguiente aspecto:

    :::image type="content" source="./media/search-howto-powerapps/2-5-controls-layout.png" alt-text="Diseño de los controles" border="true":::

1. Para que el **botón Buscar** emita una consulta, selecciónelo y pegue la siguiente acción en **OnSelect** para que se lleve a cabo al seleccionarlo:

    ```
    If(!IsBlank(txtQuery.Text),
        ClearCollect(azResult, AzureSearchQuery.Get({search: txtQuery.Text}).value))
    ```

    :::image type="content" source="./media/search-howto-powerapps/2-6-search-button-event.png" alt-text="Botón OnSelect" border="true":::
 
    Esta acción hará que el botón actualice una nueva colección denominada *azResult* con el resultado de la consulta de búsqueda; para ello, usará el texto del cuadro de texto *txtQuery* como el término de la consulta.
    
1.  A continuación, vincularemos la galería vertical que hemos creado a la colección *azResult*. Seleccione el control de galería y realice las siguientes acciones en el panel de propiedades.

    -  Establezca **Origen de datos** en *azResult*.
    
    -  Seleccione un **Diseño** adecuado en función del tipo de datos del índice. En este caso, hemos usado el diseño *Título, subtítulo y cuerpo*.
    
    -  **Edite los campos** y seleccione aquellos que quiera visualizar.

    Como hemos proporcionado un resultado de ejemplo al definir el conector, la aplicación reconoce los campos disponibles en el índice.
    
    :::image type="content" source="./media/search-howto-powerapps/2-7-gallery-select-fields.png" alt-text="Campos de la galería" border="true":::   
 
1.  Presione **F5** para obtener una versión preliminar de la aplicación.  

    Recuerde que los campos se pueden establecer en valores calculados.      
    En el ejemplo, si establece el diseño *Imagen, título y subtítulo* y especifica la función de *Imagen* como la concatenación de la ruta de acceso raíz para los datos y el nombre de archivo (por ejemplo, `"https://mystore.blob.core.windows.net/multilang/" & ThisItem.metadata_storage_name`), se generará el resultado que se muestra a continuación.

    :::image type="content" source="./media/search-howto-powerapps/2-8-2-final.png" alt-text="Aplicación final" border="true":::        

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información y cursos en línea, consulte el [catálogo de recursos de aprendizaje de Power Apps](https://docs.microsoft.com/powerapps/learning-catalog/get-started).

