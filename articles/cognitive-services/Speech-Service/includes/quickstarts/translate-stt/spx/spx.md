---
author: v-demjoh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 05/18/2020
ms.author: v-demjoh
ms.openlocfilehash: 4d548c1b7614503919de2725acb02d7f808ceb93
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2020
ms.locfileid: "83806242"
---
## <a name="run-the-speech-cli"></a>Ejecución de la CLI de Voz

Ahora está listo para ejecutar la CLI de Voz para traducir la voz en texto en un idioma diferente.

Desde la línea de comandos, cambie al directorio que contiene el archivo binario de la CLI de Voz y escriba:

```bash
spx translate --microphone --target de-DE
```

La CLI de Voz traducirá el lenguaje natural hablado en inglés en texto impreso en alemán.
Presione Entrar para detener la herramienta.

> [!NOTE]
> La CLI de Voz tiene como valor predeterminado el inglés. Puede elegir un idioma diferente [en la tabla de voz a texto](../../../../language-support.md).
> Por ejemplo, agregue `--source ja-JP` para reconocer la voz en japonés.
