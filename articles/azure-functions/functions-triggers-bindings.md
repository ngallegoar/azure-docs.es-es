---
title: Desencadenadores y enlaces en Azure Functions
description: Obtenga información sobre cómo usar desencadenadores y enlaces para conectar Azure Functions a eventos en línea y servicios basados en la nube.
author: craigshoemaker
ms.topic: conceptual
ms.date: 02/18/2019
ms.author: cshoe
ms.openlocfilehash: aa0d78d52ec13c91b82e6a8d10720269076f59a1
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96353551"
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Conceptos básicos sobre los enlaces y desencadenadores de Azure Functions

En este artículo obtendrá información sobre los conceptos de alto nivel que rodean los enlaces y desencadenadores de Azure Functions.

Los desencadenadores son lo que provocan que una función se ejecute. Un desencadenador define cómo se invoca una función y cada función debe tener exactamente un desencadenador. Los desencadenadores tienen datos asociados, que a menudo son la carga de la función. 

El enlace a una función es una manera de conectar otro recurso a la función mediante declaración. Los enlaces pueden estar conectados como *enlaces de entrada*, *enlaces de salida* o ambos. Los datos de los enlaces se proporcionan a la función como parámetros.

Puede mezclar y asignar enlaces diferentes para satisfacer sus necesidades. Los enlaces son opcionales y cada función puede tener uno o varios enlaces de entrada y de salida.

Los desencadenadores y enlaces evitan codificar el acceso a otros servicios. La función recibe los datos (por ejemplo, el contenido de un mensaje de cola) en parámetros de función. El usuario envía datos (por ejemplo, para crear un mensaje de la cola) mediante el valor devuelto de la función. 

Tenga en cuenta los siguientes ejemplos de cómo se pueden implementar las distintas funciones.

| Escenario de ejemplo | Desencadenador | Enlace de entrada | Enlace de salida |
|-------------|---------|---------------|----------------|
| Llega un nuevo mensaje de cola que ejecuta una función para escribir en otra cola. | Cola<sup>*</sup> | *None* | Cola<sup>*</sup> |
|Un trabajo programado lee los contenidos de Blob Storage y crea un nuevo documento de Cosmos DB. | Timer | Blob Storage | Cosmos DB |
|Event Grid se utiliza para leer una imagen en Blob Storage y un documento de Cosmos DB con el fin de enviar un correo. | Event Grid | Blob Storage y Cosmos DB | SendGrid |
| Un webhook que usa Microsoft Graph para actualizar una hoja de Excel. | HTTP | *None* | Microsoft Graph |

<sup>\*</sup> Representa diferentes colas

Estos ejemplos no pretenden ser exhaustivos, pero sirven para ilustrar cómo se pueden utilizar desencadenadores y enlaces de forma conjunta.

###  <a name="trigger-and-binding-definitions"></a>Definiciones de desencadenador y enlace

Los desencadenadores y enlaces se definen de forma diferente en función del enfoque de desarrollo.

| Plataforma | Los desencadenadores y enlaces se configuran por... |
|-------------|--------------------------------------------|
| Biblioteca de clases de C# | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;la decoración de métodos y parámetros con atributos de C# |
| Todos los demás (incluido Azure Portal) | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;la actualización de [function.json](./functions-reference.md) ([esquema](http://json.schemastore.org/function)) |

El portal proporciona una interfaz de usuario para esta configuración, pero puede editar el archivo directamente; para ello, abra el **Editor avanzado** disponible mediante la pestaña **Integrar** de la función.

En .NET, el tipo de parámetro define el tipo de datos de entrada. Por ejemplo, use `string` para enlazar al texto de un desencadenador de cola, una matriz de bytes que se lee como binaria y un tipo personalizado para deserializar a un objeto.

Para los lenguajes que se escriben dinámicamente, como JavaScript, use la propiedad `dataType` del archivo *function.json*. Por ejemplo, para leer el contenido de una solicitud HTTP en formato binario, establezca `dataType` en `binary`:

```json
{
    "dataType": "binary",
    "type": "httpTrigger",
    "name": "req",
    "direction": "in"
}
```

Otras opciones para `dataType` son `stream` y `string`.

## <a name="binding-direction"></a>Dirección de los enlaces

Todos los desencadenadores y enlaces tienen una propiedad `direction` en el archivo [function.json](./functions-reference.md):

- En el caso de los desencadenadores, esta propiedad siempre aparece como `in`
- Los enlaces de entrada y de salida usan `in` y `out`
- Algunos enlaces admiten la dirección especial `inout`. Si utiliza `inout`, solo estará disponible la opción **Editor avanzado** mediante la pestaña **Integrar** en el portal.

Cuando se usan [atributos en una biblioteca de clases](functions-dotnet-class-library.md) para configurar los desencadenadores y los enlaces, la dirección se proporciona en un constructor de atributos o se deduce del tipo de parámetro.

## <a name="add-bindings-to-a-function"></a>Incorporación de enlaces a una función

Puede conectar la función a otros servicios mediante enlaces de entrada o salida. Para incorporar un enlace, agregue sus definiciones específicas a la función. Para obtener información sobre cómo hacerlo, vea [Incorporación de enlaces a una función existente de Azure Functions](add-bindings-existing-function.md).  

## <a name="supported-bindings"></a>Enlaces admitidos

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

Para información sobre qué enlaces están en versión preliminar o aprobados para su uso en producción, consulte los [lenguajes admitidos](supported-languages.md).

## <a name="bindings-code-examples"></a>Ejemplos de código de enlaces

Use la tabla siguiente para buscar ejemplos de tipos de enlaces específicos que muestren cómo trabajar con enlaces en las funciones. En primer lugar, elija la pestaña de lenguaje que corresponde al proyecto. 

[!INCLUDE [functions-bindings-code-example-chooser](../../includes/functions-bindings-code-example-chooser.md)]

## <a name="custom-bindings"></a>Enlaces personalizados

Puede crear enlaces de entrada y salida personalizados. Los enlaces se deben crear en .NET, pero se pueden consumir en cualquier lenguaje admitido. Para más información sobre la creación de enlaces personalizados, vea [Creación de enlaces de entrada y salida personalizados](https://github.com/Azure/azure-webjobs-sdk/wiki/Creating-custom-input-and-output-bindings).

## <a name="resources"></a>Recursos
- [Patrones y expresiones de enlace](./functions-bindings-expressions-patterns.md)
- [Uso del valor devuelto de una función de Azure](./functions-bindings-return-value.md)
- [Cómo registrar una expresión de enlace](./functions-bindings-register.md)
- Prueba:
  - [Estrategias para probar el código en Azure Functions](functions-test-a-function.md)
  - [Ejecución manual de una función no desencadenada por HTTP](functions-manually-run-non-http.md)
- [Control de errores de enlace](./functions-bindings-errors.md)

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Registrar las extensiones de enlace de Azure Functions](./functions-bindings-register.md)
