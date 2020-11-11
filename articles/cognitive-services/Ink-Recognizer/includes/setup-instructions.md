---
author: aahill
ms.author: aahi
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: include
ms.date: 06/20/2019
ms.openlocfilehash: c68e5b7ab24e2d7e7f30ddc356ae3c4382137507
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2020
ms.locfileid: "94369082"
---
>[!NOTE]
> Los puntos de conexión de los recursos creados después del 1 de julio de 2019 usan el formato de subdominio personalizado que se muestra a continuación. Para más información y para obtener una lista completa de los puntos de conexión regionales, consulte [Nombres de subdominios personalizados para Cognitive Services.](../../cognitive-services-custom-subdomains.md) 

Los servicios de Azure Cognitive Services se representan por medio de recursos de Azure a los que se suscribe. Cree un recurso para Ink Recognizer mediante [Azure Portal](../../cognitive-services-apis-create-account.md).

Después de crear un recurso, obtenga el punto de conexión y la clave; para ello, abra el recurso en [Azure Portal](https://ms.portal.azure.com#blade/HubsExtension/BrowseResourceGroupBlade) y haga clic en **Inicio rápido**.

Cree dos [variables de entorno](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource):

* `INK_RECOGNITION_SUBSCRIPTION_KEY`: la clave de suscripción para autenticar las solicitudes. 

* `INK_RECOGNITION_ENDPOINT`: el punto de conexión del recurso. Tendrá el siguiente aspecto: <br> `https://<your-custom-subdomain>.api.cognitive.microsoft.com`