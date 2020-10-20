---
author: v-jaswel
ms.service: cognitive-services
ms.topic: include
ms.date: 10/09/2020
ms.author: v-jawe
ms.openlocfilehash: 36d12b29054f736b65af5ac411adbc26d870b982
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92014159"
---
En este inicio rápido, aprenderá a convertir voz en texto mediante el servicio de voz y cURL.

Para obtener una visión general de los conceptos de conversión de voz en texto, consulte el artículo de [información general](../../../speech-to-text.md).

## <a name="prerequisites"></a>Requisitos previos

En este artículo se da por sentado que tiene una cuenta de Azure y una suscripción al servicio de voz. Si no dispone de una cuenta y una suscripción, [pruebe el servicio de voz de forma gratuita](../../../overview.md#try-the-speech-service-for-free).

## <a name="convert-speech-to-text"></a>Conversión de voz en texto

En el símbolo del sistema, ejecute el siguiente comando. Tendrá que insertar los siguientes valores en el comando.
- Clave de suscripción del servicio de voz.
- Región del servicio de voz.
- Ruta de acceso del archivo de audio de entrada. Puede generar archivos de audio mediante la conversión de [texto en voz](../../../get-started-text-to-speech.md).

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/speech-to-text.sh" id="request":::

Debería recibir una respuesta similar a la siguiente.

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/speech-to-text.sh" id="response":::

Para más información, consulte la [referencia de la API REST de la conversión de voz en texto](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-speech-to-text).
