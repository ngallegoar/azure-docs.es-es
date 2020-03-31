---
title: Conexión a Dynamics 365
description: Crear y administrar registros con las API REST de Dynamics 365 (en línea) y Azure Logic Apps
services: logic-apps
ms.suite: integration
author: Mattp123
ms.author: matp
ms.reviewer: estfan, logicappspm
ms.topic: conceptual
ms.date: 08/18/2018
tags: connectors
ms.openlocfilehash: 9837b68fbfba783a468712d8ba1883b198af4954
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789891"
---
# <a name="manage-dynamics-365-records-with-azure-logic-apps"></a>Administración de registros de Dynamics 365 con Azure Logic Apps

Con Azure Logic Apps y el conector de Dynamics 365, puede crear tareas y flujos de trabajo automatizados en función de los registros de Dynamics 365. Los flujos de trabajo pueden crear registros, actualizar elementos, devolver registros y mucho más en su cuenta de Dynamics 365. Puede incluir acciones en las aplicaciones lógicas que obtengan respuestas de Dynamics 365 y que permitan que la salida esté disponible para otras acciones. Por ejemplo, cuando se actualice un elemento en Dynamics 365, puede enviar un correo electrónico mediante Office 365.

En este artículo se explica cómo crear una aplicación lógica que crea una tarea en Dynamics 365 cada vez que se crea un registro de cliente potencial en Dynamics 365.
Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Prerequisites

* Suscripción a Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Una [cuenta de Dynamics 365](https://dynamics.microsoft.com)

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* La aplicación lógica desde donde quiere acceder a la cuenta de Dynamics 365. Para iniciar la aplicación lógica con un desencadenador de Dynamics 365, necesita una [aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-dynamics-365-trigger"></a>Adición del desencadenador de Dynamics 365

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

En primer lugar, agregue un desencadenador de Dynamics 365 que se activa cuando aparece un nuevo registro de cliente potencial en Dynamics 365.

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en blanco en el Diseñador de aplicación lógica, si aún no está abierta.

1. En el cuadro de búsqueda, escriba "Dynamics 365" como filtro. En este ejemplo, en la lista de desencadenadores, seleccione este desencadenador: **Al crear un registro**.

   ![Seleccionar un desencadenador](./media/connectors-create-api-crmonline/select-dynamics-365-trigger.png)

1. Si se le pide que inicie sesión en Dynamics 365, hágalo ahora.

1. Proporcione estos detalles para el desencadenador:

   | Propiedad | Obligatorio | Descripción |
   |----------|----------|-------------|
   | **Nombre de la organización** | Sí | El nombre de la instancia de Dynamics 365 de la organización que desea supervisar; por ejemplo, "Contoso" |
   | **Nombre de entidad** | Sí | El nombre de la entidad que desea supervisar; por ejemplo, "Clientes potenciales" | 
   | **Frecuencia** | Sí | La unidad de tiempo que se usará con intervalos de comprobación de actualizaciones relacionadas con el desencadenador |
   | **Intervalo** | Sí | El número de segundos, minutos, horas, días, semanas o meses que deben pasar antes de la siguiente comprobación |
   ||| 

   ![Detalles del desencadenador](./media/connectors-create-api-crmonline/trigger-details.png)

## <a name="add-dynamics-365-action"></a>Adición de acción de Dynamics 365

Ahora, agregue la acción de Dynamics 365 que crea un registro de tareas para el nuevo registro de cliente potencial.

1. En el desencadenador, elija **Nuevo paso**.

1. En el cuadro de búsqueda, escriba "Dynamics 365" como filtro. En la lista de acciones, seleccione esta acción: **Crear un nuevo registro**.

   ![Acción Select](./media/connectors-create-api-crmonline/select-action.png)

1. Proporcione estos detalles para la acción:

   | Propiedad | Obligatorio | Descripción |
   |----------|----------|-------------|
   | **Nombre de la organización** | Sí | La instancia de Dynamics 365 donde desea crear el registro, que no tiene que ser la misma instancia del desencadenador; en este ejemplo es "Contoso" |
   | **Nombre de entidad** | Sí | La entidad donde desea crear el registro; por ejemplo, "Tareas" |
   | | |

   ![Detalles de la acción](./media/connectors-create-api-crmonline/action-details.png)

1. Si aparece el cuadro **Asunto** en la acción, haga clic dentro del cuadro **Asunto** para que aparezca la lista de contenido dinámico. En la lista, seleccione los valores de campo que desea incluir en el registro de tareas asociado con el nuevo registro de cliente potencial:

   | Campo | Descripción |
   |-------|-------------|
   | **Apellidos** | El apellido del cliente potencial como contacto principal en el registro |
   | **Tema.** | El nombre descriptivo del cliente potencial en el registro |
   | | |

   ![Detalles del registro de tareas](./media/connectors-create-api-crmonline/create-record-details.png)

1. En la barra de herramientas del diseñador, haga clic en **Guardar** para guardar la aplicación lógica. 

1. Para iniciar manualmente la aplicación lógica, en la barra de herramientas del diseñador, elija **Ejecutar**.

   ![Ejecución de la aplicación lógica](./media/connectors-create-api-crmonline/designer-toolbar-run.png)

1. Ahora cree un registro de clientes potenciales en Dynamics 365, para poder desencadenar el flujo de trabajo de la aplicación lógica.

## <a name="add-filter-or-query"></a>Adición de filtro o consulta

Para especificar cómo filtrar datos en una acción de Dynamics 365, elija **Mostrar opciones avanzadas** en esa acción. A continuación, puede agregar un filtro u ordenar por consulta.
Por ejemplo, puede usar una consulta de filtro para obtener solo las cuentas activas y ordenar tales registros según el nombre de la cuenta. Para esta tarea, siga estos pasos:

1. En **Consulta de filtro**, escriba esta consulta de filtro de OData: `statuscode eq 1`.

2. En **Ordenar por**, cuando aparezca la lista de contenido dinámico, seleccione **Nombre de cuenta**. 

   ![Definición de filtro y orden](./media/connectors-create-api-crmonline/advanced-options.png)

Para más información, vea estas opciones de consulta del sistema de API web de Dynamics 365 Customer Engagement:

* [$filter](https://docs.microsoft.com/dynamics365/customer-engagement/developer/webapi/query-data-web-api#filter-results)
* [$orderby](https://docs.microsoft.com/dynamics365/customer-engagement/developer/webapi/query-data-web-api#order-results)

### <a name="best-practices-for-advanced-options"></a>Procedimientos recomendados para las opciones avanzadas

Cuando se especifica un valor para un campo de una acción o desencadenador, el tipo de datos del valor debe coincidir con el tipo de campo, tanto si escribe el valor manualmente como si selecciona el valor de la lista de contenido dinámico.

En esta tabla se describen algunos tipos de campos y los tipos de datos necesarios para sus valores.

| Tipo de campo | Tipo de datos necesario | Descripción | 
|------------|--------------------|-------------|
| Campos de texto | Línea de texto única | Estos campos requieren una sola línea de texto o de contenido dinámico que tenga el tipo de texto. <p><p>*Campos de ejemplo*: **Descripción** y **Categoría** | 
| Campos numéricos enteros | Número entero | Algunos campos requieren un número entero o un contenido dinámico que tenga el tipo numérico entero. <p><p>*Campos de ejemplo*: **Porcentaje completado** y **Duración** | 
| Campos de fecha | Fecha y hora | Algunos campos requieren una fecha con formato mm/dd/yyyy o contenido dinámico que tenga el tipo de fecha. <p><p>*Campos de ejemplo*: **Fecha de creación**, **Fecha de inicio**, **Inicio real**, **Finalización real** y **Fecha de vencimiento** | 
| Campos que requieren un identificador de registro y un tipo de búsqueda | Clave principal | Algunos campos que hacen referencia a otro registro de entidad requieren un identificador de registro y un tipo de búsqueda. | 
||||

Para ampliar estos tipos de campos, a continuación se indican campos de ejemplo de desencadenadores y acciones de Dynamics 365 que requieren un identificador de registro y el tipo de búsqueda. Este requisito implica que los valores seleccionados de la lista dinámica no funcionarán.

| Campo | Descripción |
|-------|-------------|
| **Propietario** | Debe ser un identificador de usuario válido o un identificador de registro de equipo. |
| **Tipo de propietario** | Debe ser `systemusers` o `teams`. |
| **Referente** | Debe ser un identificador de registro válido, como un identificador de cuenta o un identificador de registro de contacto. |
| **Tipo de referente** | Debe ser un tipo de búsqueda, como `accounts` o `contacts`. |
| **Cliente** | Debe ser un identificador de registro válido, como un identificador de cuenta o un identificador de registro de contacto. |
| **Tipo de cliente** | Debe ser el tipo de búsqueda, como `accounts` o `contacts`. |
|||

En este ejemplo, la acción denominada **Crear un nuevo registro** crea un registro de tareas:

![Crear registro de tareas con identificadores de registro y tipos de búsqueda](./media/connectors-create-api-crmonline/create-record-advanced.png)

Esta acción asigna el registro de tareas a un identificador de usuario específico o un identificador de registro de equipo, según el identificador de registro del campo **Propietario** y el tipo de búsqueda del campo **Tipo de propietario**:

![Tipo de búsqueda e identificador de registro del propietario](./media/connectors-create-api-crmonline/owner-record-id-and-lookup-type.png)

Esta acción también agrega un registro de cuenta asociado al identificador de registro agregado en el campo **Referente** y el tipo de búsqueda del campo **Tipo de referente**:

![Tipo de búsqueda e identificador de registro del referente](./media/connectors-create-api-crmonline/regarding-record-id-lookup-type-account.png)

## <a name="find-record-id"></a>Búsqueda del identificador de registro

Para buscar un identificador de registro, siga estos pasos:

1. En Dynamics 365, abra un registro, como un registro de cuenta.

2. En la barra de herramientas de acciones, elija uno de estos pasos:

   * Seleccione el elemento **emergente**. ![Registro emergente](./media/connectors-create-api-crmonline/popout-record.png) 
   * Elija **ENVIAR UN VÍNCULO POR CORREO ELECTRÓNICO** para que pueda copiar la dirección URL completa en el programa de correo electrónico predeterminado.

   El identificador de registro aparece en la dirección URL entre los caracteres de codificación `%7b` y `%7d`:

   ![Búsqueda del identificador de registro](./media/connectors-create-api-crmonline/find-record-ID.png)

## <a name="troubleshoot-failed-runs"></a>Solución de problemas de ejecuciones con errores

Para buscar y revisar los pasos con error en la aplicación lógica, puede consultar el historial de ejecuciones de la aplicación lógica, los estados, las entradas, las salidas y muchos otros parámetros.

1. En Azure Portal, en el menú principal de la aplicación lógica, seleccione **Introducción**. En la sección **Historial de ejecuciones**, que muestra todos los estados de ejecución de la aplicación lógica, seleccione una ejecutar con error para obtener más información.

   ![Estado de ejecución de la aplicación lógica](./media/connectors-create-api-crmonline/run-history.png)

1. Expanda un paso con errores para poder ver más detalles.

   ![Expansión del paso con errores](./media/connectors-create-api-crmonline/expand-failed-step.png)

1. Revise los detalles del paso, como las entradas y salidas, ya que puede ayudar a encontrar la causa subyacente del error.

   ![Paso con error: entradas y salidas](./media/connectors-create-api-crmonline/expand-failed-step-inputs-outputs.png)

Para más información sobre cómo solucionar problemas de las aplicaciones lógicas, consulte [Diagnóstico de errores de aplicaciones lógicas](../logic-apps/logic-apps-diagnosing-failures.md).

## <a name="connector-reference"></a>Referencia de conectores

Para obtener datos técnicos, como los desencadenadores, las acciones y los límites, tal como lo describe el archivo OpenAPI (antes Swagger) del conector, consulte la [página de referencia del conector](/connectors/dynamicscrmonline/).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)
