---
author: v-demjoh
ms.service: cognitive-services
ms.topic: include
ms.date: 05/13/2020
ms.author: v-demjoh
ms.openlocfilehash: 398dfe36314f86614515168986023146d695e69b
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2020
ms.locfileid: "83806413"
---
## <a name="find-a-file-that-contains-speech"></a>Búsqueda de un archivo que contenga voz

La CLI de Voz puede reconocer voz en muchos formatos de archivo e idiomas naturales. En este inicio rápido, puede usar un archivo WAV (16 kHz o 8 kHz, 16 bits y PCM mono) que contenga voz en inglés.

1. Descargue el archivo <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/csharp/sharedcontent/console/whatstheweatherlike.wav" download="whatstheweatherlike" target="_blank">whatstheweatherlike.wav <span class="docon docon-download x-hidden-focus"></span></a>.
2. Copie el archivo `whatstheweatherlike.wav` en el mismo directorio que el archivo binario de la CLI de Voz.

## <a name="run-the-speech-cli"></a>Ejecución de la CLI de Voz

Ahora está listo para ejecutar la CLI de Voz y reconocer la voz que se encuentra en el archivo de sonido.

Desde la línea de comandos, cambie al directorio que contiene el archivo binario de la CLI de Voz y escriba:

```bash
spx recognize --file whatstheweatherlike.wav
```

> [!NOTE]
> La CLI de Voz tiene como valor predeterminado el inglés. Puede elegir un idioma diferente [en la tabla de voz a texto](../../../../language-support.md).
> Por ejemplo, agregue `--source de-DE` para reconocer voz en alemán.

La CLI de Voz mostrará una transcripción a texto de la voz en la pantalla y, luego, se cerrará.
