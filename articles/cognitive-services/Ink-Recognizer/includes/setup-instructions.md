---
author: aahill
ms.author: aahi
ms.service: cognitive-services
ms.topic: include
ms.date: 06/20/2019
ms.openlocfilehash: c202ba1d7363af9791daa801f0c447c49a80859b
ms.sourcegitcommit: bf8c447dada2b4c8af017ba7ca8bfd80f943d508
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/25/2020
ms.locfileid: "85378428"
---
>[!NOTE]
> Los puntos de conexión de los recursos creados después del 1 de julio de 2019 usan el formato de subdominio personalizado que se muestra a continuación. Para más información y para obtener una lista completa de los puntos de conexión regionales, consulte [Nombres de subdominios personalizados para Cognitive Services.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains) 

Los servicios de Azure Cognitive Services se representan por medio de recursos de Azure a los que se suscribe. Cree un recurso para Ink Recognizer mediante [Azure Portal](../../cognitive-services-apis-create-account.md).

Después de crear un recurso, obtenga el punto de conexión y la clave; para ello, abra el recurso en [Azure Portal](https://ms.portal.azure.com#blade/HubsExtension/BrowseResourceGroupBlade) y haga clic en **Inicio rápido**.

Cree dos [variables de entorno](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource):

* `INK_RECOGNITION_SUBSCRIPTION_KEY`: el punto de conexión del recurso. Tendrá el siguiente aspecto: <br> `https://<your-custom-subdomain>.api.cognitive.microsoft.com` 

* `INK_RECOGNITION_ENDPOINT`: la clave de suscripción para autenticar las solicitudes.   
