---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/14/2019
ms.author: glenga
ms.openlocfilehash: 6fd8c3c5839d4cc897caa2dff70af87980e547eb
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2020
ms.locfileid: "88206749"
---
## <a name="local-settings-file"></a>Archivo de configuración local

El archivo local.settings.json almacena la configuración de la aplicación, las cadenas de conexión y la configuración que usan las herramientas locales de desarrollo. La configuración del archivo local.settings.json solo se usa al ejecutar los proyectos de forma local. El archivo de configuración local tiene la siguiente estructura:

```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "<language worker>",
    "AzureWebJobsStorage": "<connection-string>",
    "AzureWebJobsDashboard": "<connection-string>",
    "MyBindingConnection": "<binding-connection-string>",
    "AzureWebJobs.HttpExample.Disabled": "true"
  },
  "Host": {
    "LocalHttpPort": 7071,
    "CORS": "*",
    "CORSCredentials": false
  },
  "ConnectionStrings": {
    "SQLConnectionString": "<sqlclient-connection-string>"
  }
}
```

Esta configuración se admite al ejecutar los proyectos de forma local:

| Configuración      | Descripción                            |
| ------------ | -------------------------------------- |
| **`IsEncrypted`** | Cuando la configuración se establece en `true`, todos los valores se cifran con una clave de máquina local. Se usa con los comandos `func settings`. El valor predeterminado es `false`. |
| **`Values`** | Matriz de opciones de configuración de la aplicación y cadenas de conexión que se usan al ejecutar un proyecto de forma local. Estos pares clave-valor (cadena-cadena) corresponden a la configuración de la aplicación en su aplicación de funciones de Azure, como [`AzureWebJobsStorage`]. Muchos desencadenadores y enlaces tienen una propiedad que hace referencia a una configuración de la aplicación de cadena de conexión, por ejemplo, `Connection` para el [desencadenador del almacén de blobs](../articles/azure-functions/functions-bindings-storage-blob-trigger.md#configuration). Para estas propiedades, se necesita una configuración de la aplicación definida en la matriz `Values`. <br/>[`AzureWebJobsStorage`] es una configuración de aplicación necesaria para los desencadenadores que no sean HTTP. <br/>La versión 2.x y posteriores del entorno en tiempo de ejecución de Functions requiere la configuración [`FUNCTIONS_WORKER_RUNTIME`] que Core Tools genera para su proyecto. <br/> Si tiene el [emulador de Azure Storage](../articles/storage/common/storage-use-emulator.md) instalado localmente y ha establecido [`AzureWebJobsStorage`] en `UseDevelopmentStorage=true`, Core Tools usará el emulador. Dicho emulador es útil durante el desarrollo, pero conviene probarlo con una conexión de almacenamiento real antes de la implementación.<br/> Los valores deben ser cadenas y no objetos JSON o matrices. Los nombres de la configuración no pueden incluir dos puntos (`:`) ni un subrayado doble (`__`). Estos caracteres están reservados para el entorno de ejecución. <br/>Para deshabilitar una función cuando la ejecución se realizar en un entorno local, agregue `"AzureWebJobs.<FUNCTION_NAME>.Disabled": "true"` a la colección, donde `<FUNCTION_NAME>` es el nombre de la función. Para más información, consulte [Deshabilitamiento de funciones en Azure Functions](../articles/azure-functions/disable-function.md#localsettingsjson)  |
| **`Host`** | La configuración que se muestra en esta sección permite personalizar el proceso de host de Functions al ejecutar los proyectos de forma local. Dicha configuración es independiente de la de host.json, que también se aplica al ejecutar proyectos en Azure. |
| **`LocalHttpPort`** | Establece el puerto predeterminado que se usa cuando al ejecutar el host de Functions local (`func host start` y `func run`). La opción de línea de comandos `--port` tiene prioridad sobre esta configuración. |
| **`CORS`** | Define los orígenes permitidos para el [uso compartido de recursos entre orígenes (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Los orígenes se proporcionan en una lista de valores separados por comas y sin espacios. Se admite el valor comodín (\*), lo que permite realizar solicitudes desde cualquier origen. |
| **`CORSCredentials`** |  Al establecer el valor en `true`, se permiten las solicitudes `withCredentials`. |
| **`ConnectionStrings`** | Identificador de colección. No use dicha colección para las cadenas de conexión que empleen enlaces de función. Esta colección solo la usan los marcos que habitualmente obtienen las cadenas de conexión de la sección `ConnectionStrings` de un archivo de configuración, como [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx). Las cadenas de conexión de este objeto se agregan al entorno con el tipo de proveedor de [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx). Los elementos de esta colección no se publican en Azure con otra configuración de aplicación. Debe agregar explícitamente estos valores a la colección `Connection strings` de la configuración de la aplicación de función. Si quiere crear un objeto [`SqlConnection`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(v=vs.110).aspx) en el código de la función, debe almacenar el valor de la cadena de conexión con las otras conexiones en **Configuración de la aplicación**, en el portal. |

[`AzureWebJobsStorage`]: ../articles/azure-functions/functions-app-settings.md#azurewebjobsstorage
