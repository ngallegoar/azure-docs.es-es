---
author: v-demjoh
ms.service: cognitive-services
ms.topic: include
ms.date: 05/13/2020
ms.author: v-demjoh
ms.openlocfilehash: a9beaea70dd3c8ce852344b11a50c5f20e5f8ca2
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2020
ms.locfileid: "83806293"
---
## <a name="enable-microphone"></a>Habilitación del micrófono

Conecte y encienda el micrófono de su PC y desactive las aplicaciones que puedan usarlo también. Algunos equipos tienen un micrófono integrado, mientras que otros requieren la configuración de un dispositivo Bluetooth.

## <a name="run-the-speech-cli"></a>Ejecución de la CLI de Voz

Ahora está listo para ejecutar la CLI de Voz y reconocer la voz que procede del micrófono.

1. **Inicie la aplicación**: desde la línea de comandos, cambie al directorio que contiene el archivo binario de la CLI de Voz y escriba:
    ```bash
    spx recognize --microphone
    ```

    > [!NOTE]
    > La CLI de Voz tiene como valor predeterminado el inglés. Puede elegir un idioma diferente [en la tabla de voz a texto](../../../../language-support.md).
    > Por ejemplo, agregue `--source de-DE` para reconocer voz en alemán.

2. **Inicie el reconocimiento**: hable por el micrófono. Verá la transcripción de las palabras a texto en tiempo real. La CLI de Voz se detendrá después de un período de silencio o cuando presione ctrl+C.
