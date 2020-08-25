---
title: 'Inicio rápido: Biblioteca cliente de Administración de Azure para Node.js'
description: En este inicio rápido, comience con la biblioteca cliente de Administración de Azure para Node.js.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/05/2020
ms.author: pafarley
ms.openlocfilehash: 59063912b2976890976e3c27bc4f2d93b6392e37
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2020
ms.locfileid: "88607641"
---
[Documentación de referencia](https://docs.microsoft.com/javascript/api/@azure/arm-cognitiveservices/?view=azure-node-latest) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/arm-cognitiveservices) | [Paquete (NPM)](https://www.npmjs.com/package/@azure/arm-cognitiveservices) | [Ejemplos](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/arm-cognitiveservices#sample-code)

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure válida: [cree una de manera gratuita](https://azure.microsoft.com/free/).
* La versión actual de [Node.js](https://nodejs.org/)

## <a name="create-an-azure-service-principal"></a>Creación de una entidad de servicio de Azure

Para que la aplicación interactúe con su cuenta de Azure, necesita una entidad de servicio de Azure para administrar los permisos. Siga las instrucciones de [Creación de una entidad de servicio de Azure](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps?view=azps-4.4.0&viewFallbackFrom=azps-3.3.0).

Al crear una entidad de servicio, verá que tiene un valor secreto, un identificador y un identificador de aplicación. Guarde el identificador de aplicación y el secreto en una ubicación temporal para los pasos posteriores.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Antes de crear un recurso de Cognitive Services, la cuenta debe tener un grupo de recursos de Azure que contenga el recurso. Si aún no tiene un grupo de recursos, créelo en [Azure Portal](https://ms.portal.azure.com/).

## <a name="create-a-new-nodejs-application"></a>Creación de una aplicación Node.js

En una ventana de la consola (como cmd, PowerShell o Bash), cree un directorio para la aplicación y vaya a él. 

```console
mkdir myapp && cd myapp
```

Ejecute el comando `npm init` para crear una aplicación de nodo con un archivo `package.json`. 

```console
npm init
```

Cree un archivo denominado _index.js_ antes de continuar.

### <a name="install-the-client-library"></a>Instalación de la biblioteca cliente

Instale los siguientes paquetes NPM:

```console
npm install @azure/arm-cognitiveservices
npm install @azure/ms-rest-js
npm install @azure/ms-rest-nodeauth
```

el archivo `package.json` de la aplicación se actualizará con las dependencias.

### <a name="import-libraries"></a>Importación de bibliotecas

Abra el script _index.js_ e importe las siguientes bibliotecas.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_imports)]

## <a name="authenticate-the-client"></a>Autenticar el cliente

Agregue los campos siguientes a la raíz del script y rellene sus valores, para lo que debe usar la entidad de servicio que creó y la información de su cuenta de Azure.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_constants)]

A continuación, agregue la siguiente función `quickstart` para controlar el trabajo principal del programa. El primer bloque de código construye un objeto **CognitiveServicesManagementClient**, para lo que usa las variables de credenciales que especificó anteriormente. Este objeto es necesario para todas las operaciones de Administración de Azure.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_main_auth)]

## <a name="call-management-functions"></a>Funciones de administración de llamadas

Agregue el código siguiente al final de la función `quickstart` para enumerar los recursos disponibles, crear un recurso de ejemplo, enumerar los recursos que posee y, después, eliminar el recurso de ejemplo. Estas funciones las definirá en los siguientes pasos.

## <a name="create-a-cognitive-services-resource"></a>Creación de un recurso de Cognitive Services

### <a name="choose-a-service-and-pricing-tier"></a>Elección de un servicio y un plan de tarifa

Al crear un recurso, necesitará conocer el "tipo" de servicio que desea usar y el [plan de tarifa](https://azure.microsoft.com/pricing/details/cognitive-services/) (o la SKU) que desee. Usará esta y otra información como parámetros al crear el recurso. La siguiente función enumera los "tipos" de Cognitive Services disponibles.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_list_avail)]

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

Para crear un recurso de Cognitive Services y suscribirse a él, use la función **Create**. Esta función agrega un nuevo recurso facturable al grupo de recursos que se pasa. Cuando cree el recurso, deberá conocer el "tipo" de servicio que desea usar, el plan de tarifa (o SKU) y la ubicación de Azure. La siguiente función usa todos estos valores como argumentos y crea un recurso.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_create)]

## <a name="view-your-resources"></a>Visualización de los recursos

Para ver todos los recursos de su cuenta de Azure (de todos los grupos de recursos), use la siguiente función:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_list)]

## <a name="delete-a-resource"></a>Eliminar un recurso

La función siguiente elimina el recurso especificado del grupo de recursos dado.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_delete)]

## <a name="run-the-application"></a>Ejecución de la aplicación

Agregue el siguiente código al final del script para llamar a la función `quickstart` principal con control de errores.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_main)]

Luego, en la ventana de consola, ejecute la aplicación con el comando `node`.

```console
node index.js
```

## <a name="see-also"></a>Consulte también

* [Documentación de referencia del SDK de Administración de Azure](https://docs.microsoft.com/javascript/api/@azure/arm-cognitiveservices/?view=azure-node-latest)
* [¿Qué es Azure Cognitive Services?](../../Welcome.md)
* [Autenticación de solicitudes en Azure Cognitive Services](../../authentication.md)
* [Creación de un recurso mediante Azure Portal](../../cognitive-services-apis-create-account.md)