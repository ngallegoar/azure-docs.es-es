---
title: Creación y administración de variables para almacenar y pasar valores
description: Averigüe cómo puede almacenar, administrar, usar y pasar valores mediante el uso de variables en las tareas automatizadas y el flujo de trabajo que crea con Azure Logic Apps.
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 55984082a6b287e9f7cdca005a24ef3c18032491
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75456701"
---
# <a name="store-and-manage-values-by-using-variables-in-azure-logic-apps"></a>Creación y administración de valores mediante el uso de variables en Azure Logic Apps

En este artículo se muestra cómo crear y trabajar con variables que se usan para almacenar valores en su aplicación lógica. Por ejemplo, las variables pueden ayudarle a hacer un seguimiento del número de veces que se ejecuta un bucle. Al iterar una matriz o al comprobar una matriz para buscar un elemento específico, puede usar una variable para hacer referencia al número de índice de cada elemento de la matriz.

Puede crear variables para tipos de datos como, por ejemplo, "entero", "float", "booleano", "cadena", "matriz" y "objeto". Después de crear una variable, puede realizar otras tareas; por ejemplo:

* Obtener o hacer referencia al valor de la variable.
* Aumentar o disminuir la variable por un valor constante, también conocido como *incremento* y *decremento*.
* Asignar un valor diferente a la variable.
* Insertar o *anexar* el valor de la variable como la última hora en una cadena o una matriz.

Las variables solo existen y son globales en la instancia de la aplicación lógica que las crea. Además, persisten en todas las iteraciones de bucle de una instancia de esa aplicación lógica. Cuando se hace referencia a una variable, debe usar el nombre de la variable como token, y no el nombre de la acción; este último se usa de forma habitual para hacer referencia a los resultados de la acción.

> [!IMPORTANT]
> De forma predeterminada, los ciclos de un bucle "Para cada uno" se ejecutan en paralelo. Cuando se usan variables en bucles, ejecute el bucle [secuencialmente](../logic-apps/logic-apps-control-flow-loops.md#sequential-foreach-loop) para que las variables devuelvan resultados predecibles.

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si aún no tiene una, [regístrese para obtener una cuenta de Azure gratuita](https://azure.microsoft.com/free/).

* La aplicación lógica en la que desea crear una variable.

  Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md) e [Inicio rápido: Creación de la primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Un [desencadenador](../logic-apps/logic-apps-overview.md#logic-app-concepts) que será el primer paso de la aplicación lógica.

  Antes de poder agregar acciones para crear y trabajar con variables, debe iniciar la aplicación lógica con un desencadenador.

<a name="create-variable"></a>

## <a name="initialize-variable"></a>Inicializar la variable

Puede crear una variable y declarar el tipo de datos y el valor inicial de la misma; todo mediante una sola acción de la aplicación lógica. Solo se pueden declarar variables en el nivel global, y no en los ámbitos, condiciones y bucles.

1. En [Azure Portal](https://portal.azure.com) o Visual Studio, abra la aplicación lógica en el Diseñador de aplicaciones lógicas.

   En este ejemplo se usa Azure Portal y una aplicación lógica con un desencadenador existente.

1. En la aplicación lógica, en el paso donde quiere agregar una variable, siga uno de estos pasos: 

   * Para agregar una acción en el último paso, elija **Nuevo paso**.

     ![Agregar una acción](./media/logic-apps-create-variables-store-values/add-action.png)

   * Para agregar una acción entre los pasos existentes, mueva el mouse sobre la flecha de conexión para que el signo más ( **+** ) aparezca. Haga clic en el signo más y, a continuación, seleccione **Agregar una acción**.

1. En **Elegir una acción**, en el cuadro de búsqueda, escriba `variables` como filtro. En la lista de acciones, seleccione **Inicializar variable**.

   ![Acción Select](./media/logic-apps-create-variables-store-values/select-initialize-variable-action.png)

1. Proporcione esta información sobre su variable tal y como se describe a continuación:

   | Propiedad | Obligatorio | Value |  Descripción |
   |----------|----------|-------|--------------|
   | **Nombre** | Sí | <*variable-name*> | El nombre de la variable que se va a incrementar |
   | **Tipo** | Sí | <*variable-type*> | El tipo de datos de la variable |
   | **Valor** | No | <*start-value*> | Valor inicial de la variable <p><p>**Sugerencia**: aunque es opcional, establezca este valor como procedimiento recomendado para que sepa siempre el valor inicial de la variable. |
   |||||

   Por ejemplo:

   ![Inicializar la variable](./media/logic-apps-create-variables-store-values/initialize-variable.png)

1. Ahora continúe agregando las acciones que quiera. Cuando esté listo, seleccione **Guardar** en la barra de herramientas del diseñador.

Si cambia del diseñador al editor de la vista de código, esta es la manera en que la acción **Inicializar variable** aparece en la definición de la aplicación lógica, que se encuentra en formato de notación de objetos JavaScript (JSON):

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "Count",
               "type": "Integer",
               "value": 0
          } ]
      },
      "runAfter": {}
   }
},
```

> [!NOTE]
> Aunque la acción **Inicializar variable** tiene una sección `variables` que está estructurada como una matriz, la acción solo puede crear una variable a la vez. Cada nueva variable requiere una acción **Inicializar variable** individual.

Aquí tiene ejemplos de otros tipos de variable:

*Variable de cadena*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myStringVariable",
               "type": "String",
               "value": "lorem ipsum"
          } ]
      },
      "runAfter": {}
   }
},
```

*Variable booleana*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myBooleanVariable",
               "type": "Boolean",
               "value": false
          } ]
      },
      "runAfter": {}
   }
},
```

*Matriz con enteros*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myArrayVariable",
               "type": "Array",
               "value": [1, 2, 3]
          } ]
      },
      "runAfter": {}
   }
},
```

*Matriz con cadenas*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myArrayVariable",
               "type": "Array",
               "value": ["red", "orange", "yellow"]
          } ]
      },
      "runAfter": {}
   }
},
```

<a name="get-value"></a>

## <a name="get-the-variables-value"></a>Obtener el valor de la variable

Para recuperar o hacer referencia al contenido de una variable, también puede usar la [función variables()](../logic-apps/workflow-definition-language-functions-reference.md#variables) del Diseñador de la aplicación lógica y el editor de la vista de código. Cuando se hace referencia a una variable, debe usar el nombre de la variable como token, y no el nombre de la acción; este último se usa de forma habitual para hacer referencia a los resultados de la acción.

Por ejemplo, esta expresión obtiene los elementos de la variable de la matriz [que se creó anteriormente en este artículo](#append-value) mediante la función `variables()`. La función `string()` devuelve el contenido de la variable en formato de cadena: `"1, 2, 3, red"`

```json
@{string(variables('myArrayVariable'))}
```

<a name="increment-value"></a>

## <a name="increment-variable"></a>Incrementar variable 

Para aumentar o *incrementar* una variable con un valor constante, agregue la acción **Incrementar variable** a la aplicación lógica. Esta acción solo funciona con las variables de enteros y flotantes.

1. En el Diseñador de aplicaciones lógicas, en el paso donde quiera aumentar una variable existente, elija **Nuevo paso**. 

   Por ejemplo, esta aplicación lógica ya tiene un desencadenador y una acción que creó una variable. Por lo tanto, agregue una nueva acción según estos pasos:

   ![Agregar una acción](./media/logic-apps-create-variables-store-values/add-increment-variable-action.png)

   Para agregar una acción entre pasos existentes, mueva el mouse sobre la flecha de conexión para que el signo más (+) aparezca. Haga clic en el signo más y, a continuación, seleccione **Agregar una acción**.

1. En el cuadro de búsqueda, escriba "incrementar variable" como filtro. En la lista de acciones, seleccione **Incrementar variable**.

   ![Seleccione la acción "Incrementar variable".](./media/logic-apps-create-variables-store-values/select-increment-variable-action.png)

1. Proporcione esta información para la incrementar la variable:

   | Propiedad | Obligatorio | Value |  Descripción |
   |----------|----------|-------|--------------|
   | **Nombre** | Sí | <*variable-name*> | El nombre de la variable que se va a incrementar |
   | **Valor** | No | <*increment-value*> | El valor que se usa para incrementar la variable. El valor predeterminado es uno. <p><p>**Sugerencia**: aunque es opcional, establezca este valor como procedimiento recomendado para que sepa siempre el valor específico para incrementar la variable. |
   ||||

   Por ejemplo:

   ![Ejemplo del valor incremental](./media/logic-apps-create-variables-store-values/increment-variable-action-information.png)

1. Cuando esté listo, seleccione **Guardar** en la barra de herramientas del diseñador.

Si cambia del diseñador al editor de la vista de código, esta es la manera en que la acción **Incrementar variable** aparece en la definición de la aplicación lógica, que se encuentra en formato JSON:

```json
"actions": {
   "Increment_variable": {
      "type": "IncrementVariable",
      "inputs": {
         "name": "Count",
         "value": 1
      },
      "runAfter": {}
   }
},
```

## <a name="example-create-loop-counter"></a>Ejemplo: Creación de un contador de bucle

Las variables se usan habitualmente para contar el número de veces que se ejecuta un bucle. En este ejemplo se muestra cómo crear y usar variables para esta tarea, mediante la creación de un bucle que cuente los datos adjuntos de un correo electrónico.

1. En Azure Portal, cree una aplicación lógica en blanco. Agregue un desencadenador que compruebe si hay correos electrónicos y datos adjuntos nuevos.

   En este ejemplo usaremos el desencadenador de Office 365 Outlook **cuando llegue un nuevo correo electrónico**. Puede configurar este desencadenador para que solo active cuando el correo electrónico tenga datos adjuntos. Sin embargo, puede usar cualquier conector que compruebe si hay correos electrónicos con datos adjuntos nuevos; por ejemplo, el conector de Outlook.com.

1. Para que el desencadenador compruebe si hay datos adjuntos y los pase al flujo de trabajo de la aplicación lógica, seleccione **Sí** en estas propiedades:

   * **Tiene datos adjuntos**
   * **Include Attachments** (Incluir datos adjuntos)

   ![Buscar e incluir datos adjuntos](./media/logic-apps-create-variables-store-values/check-include-attachments.png)

1. Agregue la acción [**Inicializar variable**](#create-variable). Cree una variable de entero denominada `Count` con un valor de inicio de cero.

   ![Agregar la acción "Inicializar variable"](./media/logic-apps-create-variables-store-values/initialize-variable.png)

1. Para recorrer cada archivo adjunto, agregue un bucle *para cada uno*.

   1. En la acción **Inicializar variable**, elija **Nuevo paso**.

   1. En **Elegir una acción**, seleccione **Integrado**. En el cuadro de búsqueda, escriba "`for each` como filtro y seleccione **Para cada uno**.

      ![Agregar un bucle "Para cada uno"](./media/logic-apps-create-variables-store-values/add-loop.png)

1. En el bucle, haga clic en la casilla de **Select an output from previous steps** (Seleccionar una salida de los pasos anteriores). Cuando aparezca la lista de contenido dinámico, elija **Datos adjuntos**.

   ![Seleccionar "Attachments" (Datos adjuntos)](./media/logic-apps-create-variables-store-values/select-attachments.png)

   La propiedad **Datos adjuntos**  pasa una matriz que tiene los datos adjuntos del correo electrónico del resultado del desencadenador al bucle.

1. En el bucle **Para cada uno**, seleccione **Agregar una acción**.

   ![Seleccione "Add an action" (Agregar una acción).](./media/logic-apps-create-variables-store-values/add-action-2.png)

1. En el cuadro de búsqueda, escriba "incrementar variable" como filtro. En la lista de acciones, seleccione **Incrementar variable**.

   > [!NOTE]
   > Asegúrese de que la acción **Incrementar variable** aparezca dentro del bucle. Si la acción aparece fuera del bucle, arrástrela al mismo.

1. En la acción **Incrementar variable** de la lista **Nombre**, seleccione la variable **Recuento**.

   ![Seleccione la variable "Recuento".](./media/logic-apps-create-variables-store-values/add-increment-variable-example.png)

1. En el bucle, agregue cualquier acción que le envíe la cantidad de datos adjuntos. En la acción, incluya el valor de la variable **Recuento** como, por ejemplo:

   ![Agregar una acción que envíe resultados](./media/logic-apps-create-variables-store-values/send-email-results.png)

1. Guarde la aplicación lógica. En la barra de herramientas del diseñador, seleccione **Save** (Guardar).

### <a name="test-your-logic-app"></a>Comprobación de la aplicación lógica

1. Si la aplicación lógica no está habilitada, en el menú de la aplicación lógica, elija **Información general**. En la barra de herramientas, seleccione **Enable** (Habilitar).

1. En la barra de herramientas del Diseñador de aplicaciones lógicas, elija **Ejecutar**. Con este paso, se inicia manualmente la aplicación lógica.

1. Envíe un correo electrónico con uno o varios datos adjuntos a la cuenta de correo electrónico que usó en este ejemplo.

   En este paso se activa el desencadenador de la aplicación lógica, que crea y ejecuta una instancia para el flujo de trabajo de la aplicación lógica. Como resultado, la aplicación lógica envía un mensaje o un correo electrónico que muestra el número de datos adjuntos del correo electrónico que envió.

Si cambia del diseñador al editor de la vista de código, esta es la manera en que el bucle **Para cada uno** aparece con la acción **Incrementar variable** en la definición de la aplicación lógica, que se encuentra en formato JSON.

```json
"actions": {
   "For_each": {
      "type": "Foreach",
      "actions": {
         "Increment_variable": {
           "type": "IncrementVariable",
            "inputs": {
               "name": "Count",
               "value": 1
            },
            "runAfter": {}
         }
      },
      "foreach": "@triggerBody()?['Attachments']",
      "runAfter": {
         "Initialize_variable": [ "Succeeded" ]
      }
   }
},
```

<a name="decrement-value"></a>

## <a name="decrement-variable"></a>Reducir variable

Para disminuir o *reducir* una variable con un valor constante, siga los pasos para [aumentar una variable](#increment-value); en este caso encontrará y deberá seleccionar la acción **Reducir variable**. Esta acción solo funciona con las variables de enteros y flotantes.

Estas son las propiedades para la acción **Reducir variable**:

| Propiedad | Obligatorio | Value |  Descripción |
|----------|----------|-------|--------------|
| **Nombre** | Sí | <*variable-name*> | El nombre de la variable que se va a reducir | 
| **Valor** | No | <*increment-value*> | El valor para reducir la variable. El valor predeterminado es uno. <p><p>**Sugerencia**: aunque es opcional, establezca este valor como procedimiento recomendado para que sepa siempre el valor específico para reducir la variable. |
||||| 

Si cambia del diseñador al editor de la vista de código, esta es la manera en que la acción **Reducir variable** aparece en la definición de la aplicación lógica, que se encuentra en formato JSON.

```json
"actions": {
   "Decrement_variable": {
      "type": "DecrementVariable",
      "inputs": {
         "name": "Count",
         "value": 1
      },
      "runAfter": {}
   }
},
```

<a name="assign-value"></a>

## <a name="set-variable"></a>Establecer la variable

Para asignar un valor diferente a una variable existente, siga los pasos para [aumentar una variable](#increment-value), pero tendrá que realizar lo siguiente:

1. Buscar y seleccionar la acción **Establecer variable**.

1. Proporcionar el nombre de la variable y el valor que quiere asignar. Tanto el nuevo valor como la variable deben tener los mismos tipos de datos. El valor es necesario porque esta acción no tiene un valor predeterminado.

Estas son las propiedades para la acción **Establecer variable**:

| Propiedad | Obligatorio | Value |  Descripción |
|----------|----------|-------|--------------|
| **Nombre** | Sí | <*variable-name*> | El nombre de la variable que se va a establecer |
| **Valor** | Sí | <*new-value*> | El valor al que quiere asignar la variable. Ambos deben tener los mismos tipos de datos. |
||||| 

> [!NOTE]
> A menos que esté incrementando o reduciendo variables, el cambio de variables dentro de los bucles *podría*  crear resultados inesperados, porque los bucles se ejecutan en paralelo o de forma simultánea, de manera predeterminada. Para estos casos, intente configurar el bucle para que se ejecute secuencialmente. Por ejemplo, cuando quiera hacer referencia al valor de la variable dentro del bucle y esperar el mismo valor al inicio y al final de esa instancia de bucle, siga estos pasos para cambiar la forma en que se ejecuta el bucle: 
>
> 1. En la esquina superior derecha del bucle, elija el botón de los puntos suspensivos ( **...** ) y, a continuación, elija **Configuración**.
> 
> 2. En **Control de simultaneidad**, cambie la opción **Invalidar el valor predeterminado** en **Activado**.
>
> 3. Arrastre el control deslizante **Degree of Parallelism** (Grado de paralelismo) a **1**.

Si cambia del diseñador al editor de la vista de código, esta es la manera en que la acción **Establecer variable** aparece en la definición de la aplicación lógica, que se encuentra en formato JSON. En este ejemplo se cambia el valor actual de la variable `Count` a otro valor.

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "Count",
               "type": "Integer",
               "value": 0
          } ]
      },
      "runAfter": {}
   },
   "Set_variable": {
      "type": "SetVariable",
      "inputs": {
         "name": "Count",
         "value": 100
      },
      "runAfter": {
         "Initialize_variable": [ "Succeeded" ]
      }
   }
},
```

<a name="append-value"></a>

## <a name="append-to-variable"></a>Anexar a la variable

En cuanto a las variables que almacenan cadenas o matrices, puede insertar o *anexar* el valor de una variable como el último elemento de esas matrices o cadenas. Puede seguir los pasos para [aumenta una variable](#increment-value) salvo que en su lugar, deberá realizar lo siguiente: 

1. Busque y seleccione una de estas acciones en función de si la variable es una cadena o una matriz: 

   * **Anexar a la variable de cadena**
   * **Anexar a la variable de matriz** 

1. Proporcione el valor que se anexa como el último elemento de la cadena o matriz. Este valor es necesario.

Estas son las propiedades para la acción **Anexar a...** :

| Propiedad | Obligatorio | Value |  Descripción |
|----------|----------|-------|--------------|
| **Nombre** | Sí | <*variable-name*> | El nombre de la variable que se va a establecer |
| **Valor** | Sí | <*append-value*> | El valor que quiere anexar, que puede ser de cualquier tipo. |
|||||

Si cambia del diseñador al editor de la vista de código, esta es la manera en que la acción **Anexar a la variable de la matriz** aparece en la definición de la aplicación lógica, que se encuentra en formato JSON. En este ejemplo se crea una variable de matriz y se agrega otro valor como el último elemento de la matriz. El resultado es una variable actualizada que contiene esta matriz: `[1,2,3,"red"]`.

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
            "name": "myArrayVariable",
            "type": "Array",
            "value": [1, 2, 3]
         } ]
      },
      "runAfter": {}
   },
   "Append_to_array_variable": {
      "type": "AppendToArrayVariable",
      "inputs": {
         "name": "myArrayVariable",
         "value": "red"
      },
      "runAfter": {
        "Initialize_variable": [ "Succeeded" ]
      }
   }
},
```

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre los [conectores de Logic Apps](../connectors/apis-list.md)
