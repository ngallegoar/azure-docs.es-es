---
author: v-demjoh
ms.service: cognitive-services
ms.topic: include
ms.date: 04/13/2020
ms.author: v-demjoh
ms.openlocfilehash: a5796dfb066ea8ac7c59c02b29ef67b184c67edb
ms.sourcegitcommit: 17b36b13857f573639d19d2afb6f2aca74ae56c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94425394"
---
Una de las características principales del servicio de voz es la capacidad para reconocer la voz humana y traducirla a otros idiomas. En este inicio rápido, aprenderá a usar Speech SDK en sus aplicaciones y productos para realizar la traducción de voz de alta calidad. Este inicio rápido traduce la voz que sale por el micrófono en texto en otro idioma.

## <a name="prerequisites"></a>Requisitos previos

En este artículo se da por sentado que tiene una cuenta de Azure y una suscripción al servicio de voz. Si no dispone de una cuenta y una suscripción, [pruebe el servicio de voz de forma gratuita](../../../overview.md#try-the-speech-service-for-free).

[!INCLUDE [SPX Setup](../../spx-setup.md)]

## <a name="set-source-and-target-language"></a>Establecimiento del idioma de origen y destino

Este comando llama a la CLI de voz para traducir la voz del micrófono de italiano a francés.

```shell
 spx translate --microphone --source it-IT --target fr
```