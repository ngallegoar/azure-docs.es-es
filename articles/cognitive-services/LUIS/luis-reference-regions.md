---
title: 'Regiones de publicación y puntos de conexión: LUIS'
description: La región especificada en Azure Portal es la misma donde se publicará la aplicación LUIS y se generará una dirección URL de punto de conexión para esta misma región.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 11/09/2020
ms.openlocfilehash: f7c6c3f9c3f468c7b55baf2b0cc45c094d693de5
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2020
ms.locfileid: "95018657"
---
# <a name="authoring-and-publishing-regions-and-the-associated-keys"></a>Creación y publicación de regiones y las claves asociadas

Tres regiones de creación son compatibles con los portales correspondientes de LUIS. Para publicar una aplicación de LUIS en más de una región, necesitará al menos una clave por región.

<a name="luis-website"></a>

## <a name="luis-authoring-regions"></a>Regiones de creación de LUIS
Hay tres portales de creación de LUIS, en función de la región. Debe crear y publicar el contenido en la misma región.

|LUIS|Región de creación|Nombre de la región de Azure|
|--|--|--|
|[www.luis.ai][www.luis.ai] |EE. UU.<br>Fuera de Europa<br>Fuera de Australia| `westus`|
|[au.luis.ai][au.luis.ai] |Australia| `australiaeast`|
|[eu.luis.ai][eu.luis.ai] |Europa|`westeurope`|

Las regiones de creación tienen [regiones de conmutación por error emparejadas](../../best-practices-availability-paired-regions.md).

<a name="regions-and-azure-resources"></a>

## <a name="publishing-regions-and-azure-resources"></a>Regiones de publicación y recursos de Azure
La aplicación se publica en todas las regiones asociadas a los recursos de LUIS agregados en el portal de LUIS. Por ejemplo, para una aplicación creada en [www.luis.ai][www.luis.ai], si crea un recurso de LUIS o de Cognitive Service en **westus** y lo [agrega a la aplicación como recurso](luis-how-to-azure-subscription.md), la aplicación se publica en dicha región.

## <a name="public-apps"></a>Aplicaciones públicas
Se publica una aplicación pública en todas las regiones para que un usuario con una clave de recurso de LUIS basada en regiones pueda tener acceso a la aplicación en cualquier región que esté asociado a su clave de recurso.

<a name="publishing-regions"></a>

## <a name="publishing-regions-are-tied-to-authoring-regions"></a>Las regiones de publicación están asociadas a las regiones de creación

La aplicación de regiones de creación solo se puede publicar en la región de publicación correspondiente. Si la aplicación está en la región de creación incorrecta, expórtela e impórtela en la región de creación correcta para su región de publicación.

Las aplicaciones de LUIS creadas en https://www.luis.ai se pueden publicar en todos los puntos de conexión, salvo en [Europa](#publishing-to-europe) y [Australia](#publishing-to-australia).

## <a name="publishing-to-europe"></a>Publicación en Europa

Para publicar en las regiones europeas, solo debe crear aplicaciones de LUIS en https://eu.luis.ai. Si intenta publicar en cualquier otro lugar con una clave en la región europea, LUIS mostrará un mensaje de advertencia. En su lugar, use https://eu.luis.ai. Las aplicaciones de LUIS creadas en [https://eu.luis.ai][eu.luis.ai] no migran automáticamente a otras regiones. Exporte e importe la aplicación de LUIS para poder migrarla.

## <a name="europe-publishing-regions"></a>Regiones de publicación de Europa

 Región global | Región de la API de creación y sitio web de creación| Región de publicación y de consulta<br>`API region name`   |  Formato de dirección URL de punto de conexión   |
|-----|------|------|------|
| [Europa](#publishing-to-europe)| `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Centro de Francia<br>`francecentral`     | `https://francecentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| [Europa](#publishing-to-europe)| `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Norte de Europa<br>`northeurope`     | `https://northeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| [Europa](#publishing-to-europe) | `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Oeste de Europa<br>`westeurope`    |  `https://westeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| [Europa](#publishing-to-europe) | `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Sur de Reino Unido 2<br>`uksouth`    |  `https://uksouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |

## <a name="publishing-to-australia"></a>Publicación en Australia

Para publicar en las regiones australianas, solo debe crear aplicaciones de LUIS en https://au.luis.ai. Si intenta publicar en cualquier otro lugar con una clave en la región australiana, LUIS mostrará un mensaje de advertencia. En su lugar, use https://au.luis.ai. Las aplicaciones de LUIS creadas en [https://au.luis.ai][au.luis.ai] no migran automáticamente a otras regiones. Exporte e importe la aplicación de LUIS para poder migrarla.

## <a name="australia-publishing-regions"></a>Regiones de publicación de Australia

 Región global | Región de la API de creación y sitio web de creación| Región de publicación y de consulta<br>`API region name`   |  Formato de dirección URL de punto de conexión   |
|-----|------|------|------|
| [Australia](#publishing-to-australia) | `australiaeast`<br>[au.luis.ai][au.luis.ai]| Este de Australia<br>`australiaeast`     |  `https://australiaeast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |

## <a name="publishing-to-other-regions"></a>Publicación en otras regiones

Para publicar en otras regiones, cree aplicaciones de LUIS en [https://www.luis.ai](https://www.luis.ai).

## <a name="other-publishing-regions"></a>Otras regiones de publicación

 Región global | Región de la API de creación y sitio web de creación| Región de publicación y de consulta<br>`API region name`   |  Formato de dirección URL de punto de conexión   |
|-----|------|------|------|
| África | `westus`<br>[www.luis.ai][www.luis.ai]| Norte de Sudáfrica<br>`southafricanorth` |  `https://southafricanorth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Asia | `westus`<br>[www.luis.ai][www.luis.ai]| Centro de la India<br>`centralindia` |  `https://centralindia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Asia | `westus`<br>[www.luis.ai][www.luis.ai]| Este de Asia<br>`eastasia`     |  `https://eastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Asia | `westus`<br>[www.luis.ai][www.luis.ai]| Japón Oriental<br>`japaneast`     |   `https://japaneast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Asia | `westus`<br>[www.luis.ai][www.luis.ai]| Japón Occidental<br>`japanwest`     |   `https://japanwest.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Asia | `westus`<br>[www.luis.ai][www.luis.ai]| Centro de Corea del Sur<br>`koreacentral`     |   `https://koreacentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Asia | `westus`<br>[www.luis.ai][www.luis.ai]| Sudeste de Asia<br>`southeastasia`     |   `https://southeastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Norteamérica |`westus`<br>[www.luis.ai][www.luis.ai] | Centro de Canadá<br>`canadacentral`     |   `https://canadacentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Norteamérica |`westus`<br>[www.luis.ai][www.luis.ai] | Centro de EE. UU.<br>`centralus`     |   `https://centralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Norteamérica |`westus`<br>[www.luis.ai][www.luis.ai] | Este de EE. UU.<br>`eastus`      |  `https://eastus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Norteamérica | `westus`<br>[www.luis.ai][www.luis.ai] | Este de EE. UU. 2<br>`eastus2`     |  `https://eastus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Norteamérica | `westus`<br>[www.luis.ai][www.luis.ai] | Centro-Norte de EE. UU<br>`northcentralus`  |  `https://northcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Norteamérica | `westus`<br>[www.luis.ai][www.luis.ai] | Centro-sur de EE. UU.<br>`southcentralus`  |  `https://southcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Norteamérica |`westus`<br>[www.luis.ai][www.luis.ai] | Centro-Oeste de EE. UU.<br>`westcentralus`    |  `https://westcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Norteamérica | `westus`<br>[www.luis.ai][www.luis.ai] | Oeste de EE. UU.<br>`westus`  |   `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`  |
| Norteamérica |`westus`<br>[www.luis.ai][www.luis.ai] | Oeste de EE. UU. 2<br>`westus2`    |  `https://westus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`  |
| Sudamérica | `westus`<br>[www.luis.ai][www.luis.ai] | Sur de Brasil<br>`brazilsouth`    |  `https://brazilsouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |

## <a name="endpoints"></a>Puntos de conexión

Más información sobre la [creación y predicción de puntos de conexión](developer-reference-resource.md).

## <a name="failover-regions"></a>Regiones de conmutación por error

Cada región tiene una región secundaria a la que conmutar por error. Europa conmuta por error dentro de Europa y Australia conmuta por error dentro de Australia.

Las regiones de creación tienen [regiones de conmutación por error emparejadas](../../best-practices-availability-paired-regions.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de entidades creadas previamente](./luis-reference-prebuilt-entities.md)

 [www.luis.ai]: https://www.luis.ai
 [au.luis.ai]: https://au.luis.ai
 [eu.luis.ai]: https://eu.luis.ai