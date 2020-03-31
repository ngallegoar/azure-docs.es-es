---
title: Conversión de datos JSON con transformaciones Liquid
description: Creación de transformaciones o asignaciones para transformaciones JSON avanzadas mediante Logic Apps y una plantilla Liquid
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 08/16/2018
ms.openlocfilehash: fb9f9cfdba07ebe0bc5800def6d93950869e9727
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75456650"
---
# <a name="perform-advanced-json-transformations-with-liquid-templates-in-azure-logic-apps"></a>Realización de transformaciones JSON avanzadas con plantillas Liquid en Azure Logic Apps

Puede realizar transformaciones JSON básicas en las aplicaciones lógicas mediante acciones de operación de datos nativas como **Redactar** o **Análisis del archivo JSON**. Para llevar a cabo transformaciones JSON avanzadas, puede crear plantillas o asignaciones con [Liquid](https://shopify.github.io/liquid/), que es un lenguaje de plantilla de código abierto para aplicaciones web flexibles. Una plantilla de Liquid define cómo transformar la salida JSON y admite transformaciones JSON más complejas, como iteraciones, flujos de control, variables, etc. 

Por lo tanto, para realizar una transformación de Liquid en la aplicación lógica, primero debe definir la asignación de JSON a JSON con una plantilla de Liquid y almacenarla en la cuenta de integración. En este artículo se muestra cómo crear y usar esta plantilla o asignación de Liquid. 

## <a name="prerequisites"></a>Prerequisites

* Suscripción a Azure. Si no tiene una suscripción, puede [comenzar con una cuenta de Azure gratuita](https://azure.microsoft.com/free/). También puede [registrarse para obtener una suscripción de pago por uso](https://azure.microsoft.com/pricing/purchase-options/).

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Una [cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) básica

* Conocimientos básicos sobre el [lenguaje de plantilla de Liquid](https://shopify.github.io/liquid/)

## <a name="create-liquid-template-or-map-for-your-integration-account"></a>Creación de una plantilla o asignación de Liquid para la cuenta de integración

1. Para este ejemplo, cree la plantilla de Liquid que aquí se describe. En la plantilla de Liquid, puede usar los [filtros de Liquid](https://shopify.github.io/liquid/basics/introduction/#filters), que usan [DotLiquid](https://dotliquidmarkup.org/) y las convenciones de nomenclatura de C#. 

   > [!NOTE]
   > Asegúrese de que los nombres de filtro usan *mayúscula al principio de la oración* en la plantilla. De lo contrario, los filtros no funcionarán. Además, las asignaciones tienen [límites de tamaño de archivo](../logic-apps/logic-apps-limits-and-config.md#artifact-capacity-limits).

   ```json
   {%- assign deviceList = content.devices | Split: ', ' -%}
   
   {
      "fullName": "{{content.firstName | Append: ' ' | Append: content.lastName}}",
      "firstNameUpperCase": "{{content.firstName | Upcase}}",
      "phoneAreaCode": "{{content.phone | Slice: 1, 3}}",
      "devices" : [
         {%- for device in deviceList -%}
            {%- if forloop.Last == true -%}
            "{{device}}"
            {%- else -%}
            "{{device}}",
            {%- endif -%}
        {%- endfor -%}
        ]
   }
   ```

2. Inicie sesión en [Azure Portal](https://portal.azure.com). En el menú principal de Azure, seleccione **Todos los recursos**. En el cuadro de búsqueda, busque y seleccione la cuenta de integración.

   ![Seleccionar cuenta de integración](./media/logic-apps-enterprise-integration-liquid-transform/select-integration-account.png)

3.  En **Componentes**, seleccione **Asignaciones**.

    ![Seleccionar asignaciones](./media/logic-apps-enterprise-integration-liquid-transform/add-maps.png)

4. Elija **Agregar** y proporcione estos detalles para la asignación:

   | Propiedad | Value | Descripción | 
   |----------|-------|-------------|
   | **Nombre** | JsonToJsonTemplate | Nombre de la asignación, que es "JsonToJsonTemplate" en este ejemplo | 
   | **Tipo de asignación** | **liquid** | Tipo de la asignación. Para la transformación de JSON a JSON, debe seleccionar **liquid**. | 
   | **Map** | "SimpleJsonToJsonTemplate.liquid" | Archivo de asignación o plantilla de Liquid existente para su uso en la transformación, "SimpleJsonToJsonTemplate.liquid" en este ejemplo. Puede usar el selector de archivos para buscar el archivo. Para conocer los límites de tamaño de las asignaciones, consulte [Límites y configuración](../logic-apps/logic-apps-limits-and-config.md#artifact-capacity-limits). |
   ||| 

   ![Agregar la plantilla Liquid](./media/logic-apps-enterprise-integration-liquid-transform/add-liquid-template.png)
    
## <a name="add-the-liquid-action-for-json-transformation"></a>Agregar la acción Liquid a la transformación JSON

1. En Azure Portal, siga estos pasos para [crear una aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md).

2. En el Diseñador de aplicaciones lógicas, agregue el [desencadenador de solicitud](../connectors/connectors-native-reqres.md#add-request) a la aplicación lógica.

3. En el desencadenador, elija **New step** (Nuevo paso). 
   En el cuadro de búsqueda, escriba "liquid" como filtro y seleccione esta acción: **Transform JSON to JSON - Liquid** (Transformar de JSON a JSON: Liquid)

   ![Búsqueda y selección de la acción Liquid](./media/logic-apps-enterprise-integration-liquid-transform/search-action-liquid.png)

4. Haga clic en el cuadro **Content** (Contenido) para que aparezca la lista de contenido dinámico y seleccione el token **Body** (Cuerpo).
  
   ![Selección de cuerpo](./media/logic-apps-enterprise-integration-liquid-transform/select-body.png)
 
5. Desde la lista **Asignación**, seleccione la plantilla Liquid, que, en este ejemplo, es "JsonToJsonTemplate".

   ![Selección de asignación](./media/logic-apps-enterprise-integration-liquid-transform/select-map.png)

   Si la lista de asignaciones está vacía, probablemente, la aplicación lógica no está vinculada a la cuenta de integración. 
   Para vincular la aplicación lógica a la cuenta de integración que tiene la plantilla o asignación Liquid, siga estos pasos:

   1. En el menú de la aplicación lógica, seleccione **Configuración del flujo de trabajo**.

   2. En la lista **Seleccione una cuenta de integración**, seleccione su cuenta de integración y elija **Guardar**.

      ![Vincular la aplicación lógica a la cuenta de integración](./media/logic-apps-enterprise-integration-liquid-transform/link-integration-account.png)

## <a name="test-your-logic-app"></a>Comprobación de la aplicación lógica

Publique la entrada JSON en la aplicación lógica desde [Postman](https://www.getpostman.com/postman) u otra herramienta similar. La salida JSON transformada de la aplicación lógica tiene este aspecto:
  
![Salida de ejemplo](./media/logic-apps-enterprise-integration-liquid-transform/example-output-jsontojson.png)

## <a name="more-liquid-action-examples"></a>Más ejemplos de acciones Liquid
Liquid no está limitado únicamente a transformaciones JSON. Estos son otras acciones de transformación disponibles que utilizan Liquid.

* Transformación de JSON en texto
  
  Esta es la plantilla Liquid utilizada en este ejemplo:
   
   ``` json
   {{content.firstName | Append: ' ' | Append: content.lastName}}
   ```
   Estas son las salidas y entradas de ejemplo:
  
   ![Ejemplo de salida JSON a texto](./media/logic-apps-enterprise-integration-liquid-transform/example-output-jsontotext.png)

* Transformación XML a JSON
  
  Esta es la plantilla Liquid utilizada en este ejemplo:
   
   ``` json
   [{% JSONArrayFor item in content -%}
        {{item}}
    {% endJSONArrayFor -%}]
   ```
   Estas son las salidas y entradas de ejemplo:

   ![Ejemplo de salida XML a JSON](./media/logic-apps-enterprise-integration-liquid-transform/example-output-xmltojson.png)

* Transformación XML a texto
  
  Esta es la plantilla Liquid utilizada en este ejemplo:

   ``` json
   {{content.firstName | Append: ' ' | Append: content.lastName}}
   ```

   Estas son las salidas y entradas de ejemplo:

   ![Ejemplo de salida XML a texto](./media/logic-apps-enterprise-integration-liquid-transform/example-output-xmltotext.png)

## <a name="next-steps"></a>Pasos siguientes

* [Más información acerca de Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "Información sobre Enterprise Integration Pack")  
* [Más información sobre las asignaciones](../logic-apps/logic-apps-enterprise-integration-maps.md "Información sobre las asignaciones de integración empresarial")  

