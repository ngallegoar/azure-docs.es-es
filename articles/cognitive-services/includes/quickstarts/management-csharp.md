---
title: 'Inicio rápido: Biblioteca cliente de Administración de Azure para .NET'
description: En este inicio rápido, comience con la biblioteca cliente de Administración de Azure para .NET.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/05/2020
ms.author: pafarley
ms.openlocfilehash: 93ea0ffc94b78e014b30ab1d45d589eba50fe524
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2020
ms.locfileid: "88607630"
---
[Documentación de referencia](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/management?view=azure-dotnet) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Microsoft.Azure.Management.CognitiveServices) | [Paquete (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Management.CognitiveServices/) | [Ejemplos](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Microsoft.Azure.Management.CognitiveServices/tests)

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure válida: [cree una de manera gratuita](https://azure.microsoft.com/free/).
* La versión actual de [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).

## <a name="create-an-azure-service-principal"></a>Creación de una entidad de servicio de Azure

Para que la aplicación interactúe con su cuenta de Azure, necesita una entidad de servicio de Azure para administrar los permisos. Siga las instrucciones de [Creación de una entidad de servicio de Azure](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps?view=azps-4.4.0&viewFallbackFrom=azps-3.3.0).

Al crear una entidad de servicio, verá que tiene un valor secreto, un identificador y un identificador de aplicación. Guarde el identificador de aplicación y el secreto en una ubicación temporal para los pasos posteriores.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Antes de crear un recurso de Cognitive Services, la cuenta debe tener un grupo de recursos de Azure que contenga el recurso. Si aún no tiene un grupo de recursos, créelo en [Azure Portal](https://ms.portal.azure.com/) antes de continuar.

## <a name="create-a-new-c-application"></a>Creación de una aplicación de C#

Cree una aplicación de .NET Core. En una ventana de consola (por ejemplo, cmd, PowerShell o Bash), use el comando `dotnet new` para crear una nueva aplicación de consola con el nombre `azure-management-quickstart`. Este comando crea un sencillo proyecto de C#, "Hola mundo", con un solo archivo de origen: *program.cs*. 

```console
dotnet new console -n azure-management-quickstart
```

Cambie el directorio a la carpeta de aplicaciones recién creada. Para compilar la aplicación:

```console
dotnet build
```

La salida de la compilación no debe contener advertencias ni errores. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

### <a name="install-the-client-library"></a>Instalación de la biblioteca cliente

En el directorio de aplicaciones, instale la biblioteca cliente de Administración de Azure para .NET con el siguiente comando:

```console
dotnet add package Microsoft.Azure.Management.CognitiveServices
dotnet add package Microsoft.Azure.Management.Fluent
dotnet add package Microsoft.Azure.Management.ResourceManager.Fluent
```

Si usa el IDE de Visual Studio, la biblioteca cliente estará disponible como un paquete de NuGet descargable.

### <a name="import-libraries"></a>Importación de bibliotecas

Abra *program.cs* y agregue las siguientes instrucciones`using` a la parte superior del archivo:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_using)]

## <a name="authenticate-the-client"></a>Autenticar el cliente

Agregue los campos siguientes a la raíz de *program.cs* y rellene sus valores con la entidad de servicio que creó y la información de su cuenta de Azure.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_constants)]

Luego, en el método **Main**, use estos valores para construir un objeto **CognitiveServicesManagementClient**. Este objeto es necesario para todas las operaciones de Administración de Azure.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_assigns)]

## <a name="call-management-methods"></a>Métodos de administración de llamada

Agregue el código siguiente al método **Main** para enumerar los recursos disponibles, crear un recurso de ejemplo, enumerar los recursos que posee y, después, eliminar el recurso de ejemplo. Estos métodos los definirá en los siguientes pasos.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_calls)]

## <a name="create-a-cognitive-services-resource"></a>Creación de un recurso de Cognitive Services

### <a name="choose-a-service-and-pricing-tier"></a>Elección de un servicio y un plan de tarifa

Al crear un recurso, necesitará conocer el "tipo" de servicio que desea usar y el [plan de tarifa](https://azure.microsoft.com/pricing/details/cognitive-services/) (o la SKU) que desee. Usará esta y otra información como parámetros al crear el recurso. Puede encontrar una lista de los "tipos" de Cognitive Service disponibles llamando al siguiente método en el script:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_list_avail)]

[!INCLUDE [cognitive-services-subscription-types](../../../../includes/cognitive-services-subscription-types.md)]

Consulte la lista de SKU e información sobre precios a continuación. 

#### <a name="multi-service"></a>Varios servicios

| Servicio                    | Clase                      |
|----------------------------|---------------------------|
| Varios servicios Consulte la [página de precios](https://azure.microsoft.com/pricing/details/cognitive-services/)para más información.            | `CognitiveServices`     |


#### <a name="vision"></a>Visión

| Servicio                    | Clase                      |
|----------------------------|---------------------------|
| Computer Vision            | `ComputerVision`          |
| Custom Vision: Prediction | `CustomVision.Prediction` |
| Custom Vision: Training   | `CustomVision.Training`   |
| Caras                       | `Face`                    |
| Form Recognizer            | `FormRecognizer`          |
| Ink Recognizer             | `InkRecognizer`           |

#### <a name="search"></a>Search

| Servicio            | Clase                  |
|--------------------|-----------------------|
| Bing Autosuggest   | `Bing.Autosuggest.v7` |
| Bing Custom Search | `Bing.CustomSearch`   |
| Bing Entity Search | `Bing.EntitySearch`   |
| Bing Search        | `Bing.Search.v7`      |
| Bing Spell Check   | `Bing.SpellCheck.v7`  |

#### <a name="speech"></a>Voz

| Servicio            | Clase                 |
|--------------------|----------------------|
| Speech Services    | `SpeechServices`     |
| Reconocimiento de voz | `SpeakerRecognition` |

#### <a name="language"></a>Idioma

| Servicio            | Clase                |
|--------------------|---------------------|
| Descripción del formulario | `FormUnderstanding` |
| LUIS               | `LUIS`              |
| QnA Maker          | `QnAMaker`          |
| Text Analytics     | `TextAnalytics`     |
| Traducción del texto   | `TextTranslation`   |

#### <a name="decision"></a>Decisión

| Servicio           | Clase               |
|-------------------|--------------------|
| Anomaly Detector  | `AnomalyDetector`  |
| Content Moderator | `ContentModerator` |
| Personalizer      | `Personalizer`     |


#### <a name="pricing-tiers-and-billing"></a>Planes de tarifas y facturación

Los planes de tarifas (y la cantidad que se factura) se basan en el número de transacciones que se envían con la información de autenticación. Cada plan de tarifa especifica lo siguiente:
* El número máximo de transacciones permitidas por segundo (TPS).
* Las características de servicio habilitadas en el plan de tarifa.
* El costo de un número predefinido de transacciones. Si se supera este número, se producirá un cargo adicional, tal como se especifica en los [detalles de precios](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) del servicio.

> [!NOTE]
> Muchas de las instancias de Cognitive Services tienen un nivel gratuito que se puede usar para probar el servicio. Para usar el nivel gratuito, utilice `F0` como SKU del recurso.

## <a name="create-a-cognitive-services-resource"></a>Creación de un recurso de Cognitive Services

Para crear un recurso de Cognitive Services y suscribirse a él, use el método **Create**. Este método agrega un nuevo recurso facturable al grupo de recursos que se pasa. Cuando cree el recurso, deberá conocer el "tipo" de servicio que desea usar, junto con su plan de tarifa (o SKU) y una ubicación de Azure. El siguiente método usa todos estos valores como argumentos y crea un recurso.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_create)]

## <a name="view-your-resources"></a>Visualización de los recursos

Para ver todos los recursos de su cuenta de Azure (de todos los grupos de recursos), use el siguiente método:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_list)]

## <a name="delete-a-resource"></a>Eliminación de un recurso

El método siguiente elimina el recurso especificado del grupo de recursos dado.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_delete)]

## <a name="run-the-application"></a>Ejecución de la aplicación

Ejecute la aplicación desde el directorio de la aplicación con el comando `dotnet run`.

```dotnet
dotnet run
```

## <a name="see-also"></a>Consulte también

* [Documentación de referencia del SDK de Administración de Azure](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/management?view=azure-dotnet)
* [¿Qué es Azure Cognitive Services?](../../Welcome.md)
* [Autenticación de solicitudes en Azure Cognitive Services](../../authentication.md)
* [Creación de un recurso mediante Azure Portal](../../cognitive-services-apis-create-account.md)