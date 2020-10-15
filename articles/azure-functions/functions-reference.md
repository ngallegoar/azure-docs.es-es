---
title: Guía para desarrollar Azure Functions
description: Obtenga información sobre los conceptos y las técnicas de Azure Functions que necesita para desarrollar funciones en Azure, en todos los lenguajes de programación y enlaces.
ms.assetid: d8efe41a-bef8-4167-ba97-f3e016fcd39e
ms.topic: conceptual
ms.date: 10/12/2017
ms.openlocfilehash: 9a3c0643f4fc965ff64106758320aeb445aaf9ae
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85921737"
---
# <a name="azure-functions-developer-guide"></a>Guía para desarrolladores de Azure Functions
En Azure Functions, determinadas funciones comparten algunos componentes y conceptos técnicos básicos, independientemente del idioma o el enlace que use. Antes de ir a detalles de aprendizaje específicos de un idioma o un enlace determinados, asegúrese de leer al completo esta información general que se aplica a todos ellos.

En este artículo se supone que ya ha leído la [Información general sobre Azure Functions](functions-overview.md).

## <a name="function-code"></a>Código de función
Una *función* es el concepto principal en las funciones de Azure. Una función contiene dos elementos importantes: el código, que se puede escribir en diversos lenguajes, y la configuración, el archivo function.json. Con los lenguajes compilados, este archivo de configuración se genera automáticamente a partir de las anotaciones del código. Para los lenguajes de scripting, debe proporcionar el archivo de configuración.

El archivo function.json define el desencadenador de la función, los enlaces y otras opciones de configuración. Cada función tiene un solo desencadenador. Este archivo de configuración se usa en tiempo de ejecución para determinar los eventos que se supervisarán y cómo pasar datos y devolverlos al ejecutarse una función. El siguiente es un ejemplo de archivo function.json.

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Para más información, consulte [Conceptos básicos sobre los enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md).

La propiedad `bindings` es donde configura los enlaces y los desencadenadores. Cada enlace comparte unos ajustes de configuración comunes y algunos parámetros que son específicos de un determinado tipo de enlace. Cada enlace requiere la siguiente configuración:

| Propiedad | Valores/tipos | Comentarios |
| --- | --- | --- |
| `type` |string |Tipo de enlace. Por ejemplo, `queueTrigger`. |
| `direction` |'in', 'out' |Indica si el enlace está disponible para recibir datos en la función o enviar datos de la función. |
| `name` |string |El nombre que se usa para los datos enlazados en la función. En C# es un nombre de argumento; en JavaScript es la clave en una lista de clave-valor. |

## <a name="function-app"></a>Aplicación de función
La aplicación de función proporciona un contexto de ejecución en Azure donde ejecutar las funciones. Como tal, es la unidad de implementación y administración de las funciones. Una aplicación de función se compone de una o varias funciones individuales que se administran, implementan y escalan conjuntamente. Todas las funciones de una aplicación de función comparten el mismo plan de precios, el mismo método de implementación y la misma versión en tiempo de ejecución. Una aplicación de función es como una forma de organizar y administrar las funciones de manera colectiva. Para obtener más información, vea [Administración de una Function App en Azure Portal](functions-how-to-use-azure-function-app-settings.md). 

> [!NOTE]
> Todas las funciones de una aplicación de función deben crearse en el mismo lenguaje. En [versiones anteriores](functions-versions.md) del tiempo de ejecución de Azure Functions, esto no era necesario.

## <a name="folder-structure"></a>Estructura de carpetas
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

La estructura de carpetas anterior es la predeterminada (y recomendada) para una aplicación de función. Si desea cambiar la ubicación del archivo de código de una función, modifique la sección `scriptFile` del archivo _function.json_. Asimismo, para implementar el proyecto en la aplicación de función en Azure, se recomienda usar la [implementación de paquetes](deployment-zip-push.md). También puede usar herramientas existentes como la [integración e implementación continuas](functions-continuous-deployment.md) y Azure DevOps.

> [!NOTE]
> Si implementa un paquete manualmente, asegúrese de implementar el archivo _host.json_ y las carpetas de la función directamente en la carpeta `wwwroot`. No incluya la carpeta `wwwroot` en sus implementaciones. De lo contrario, acabará con carpetas `wwwroot\wwwroot`.

#### <a name="use-local-tools-and-publishing"></a>Uso de herramientas locales y publicación
Las aplicaciones de función pueden crearse y publicarse con diversas herramientas, incluidas [Visual Studio](./functions-develop-vs.md), [Visual Studio Code](functions-create-first-function-vs-code.md), [IntelliJ](./functions-create-maven-intellij.md), [Eclipse](./functions-create-maven-eclipse.md) y [Azure Functions Core Tools](./functions-develop-local.md). Para más información, consulte [Codificación y comprobación de las funciones de Azure Functions en un entorno local](./functions-develop-local.md).

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on the consumption plan --glenga -->

## <a name="how-to-edit-functions-in-the-azure-portal"></a><a id="fileupdate"></a> Edición de funciones en Azure Portal
El editor de funciones integrado en Azure Portal le permite actualizar el código y el archivo *function.json* directamente. Esto solo se recomienda para pequeños cambios o pruebas de concepto: el procedimiento recomendado es usar una herramienta de desarrollo local como VS Code.

## <a name="parallel-execution"></a>Ejecución en paralelo
Cuando se producen varios eventos de desencadenado más rápido de lo que un tiempo de ejecución de función de un solo subproceso pueda procesarlos, el tiempo de ejecución puede invocar la función varias veces en paralelo.  Si una aplicación de función usa el [plan de hospedaje de consumo](functions-scale.md#how-the-consumption-and-premium-plans-work), esta aplicación podría escalarse horizontalmente de forma automática.  Cada instancia de la aplicación de función, tanto si la aplicación se ejecuta en el plan de hospedaje de consumo como en el [plan de hospedaje de App Service](../app-service/overview-hosting-plans.md) normal, puede procesar invocaciones de función simultáneas en paralelo mediante varios subprocesos.  El número máximo de invocaciones de función simultáneas en cada instancia de aplicación de función varía según el tipo de desencadenador usado y lo recursos empleados por otras funciones dentro de la aplicación de función.

## <a name="functions-runtime-versioning"></a>Versiones del entorno en tiempo de ejecución de Functions

Puede configurar la versión del entorno en tiempo de ejecución de Functions mediante la configuración de la aplicación `FUNCTIONS_EXTENSION_VERSION`. Por ejemplo, el valor "~3" indica que la aplicación de funciones utilizará 3.x como versión principal. Las aplicaciones de funciones se actualizan a las nuevas versiones secundarias a medida que se lanzan. Para más información y saber cómo ver la versión exacta de la aplicación de función, consulte [Cómo seleccionar un destino para versiones en tiempo de ejecución de Azure Functions](set-runtime-version.md).

## <a name="repositories"></a>Repositorios
El código de Azure Functions es código abierto y está almacenado en repositorios de GitHub:

* [Funciones de Azure](https://github.com/Azure/Azure-Functions)
* [Host de Azure Functions](https://github.com/Azure/azure-functions-host/)
* [Portal de Azure Functions](https://github.com/azure/azure-functions-ux)
* [Plantillas de Azure Functions](https://github.com/azure/azure-functions-templates)
* [SDK de Azure WebJobs](https://github.com/Azure/azure-webjobs-sdk/)
* [Extensiones del SDK de Azure WebJobs](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Enlaces
Esta es una tabla de todos los enlaces admitidos.

[!INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

¿Tiene algún problema con errores procedentes de los enlaces? Revise la documentación de los [códigos de error de los enlaces de Azure Functions](functions-bindings-error-pages.md).

## <a name="reporting-issues"></a>Problemas de informes
[!INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)]

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información, consulte los siguientes recursos:

* [Enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md)
* [Codificación y comprobación de Azure Functions en un entorno local](./functions-develop-local.md)
* [Procedimientos recomendados de Azure Functions](functions-best-practices.md)
* [Referencia para desarrolladores de C# de Azure Functions](functions-dotnet-class-library.md)
* [Referencia para desarrolladores de Node.js de Azure Functions](functions-reference-node.md)
