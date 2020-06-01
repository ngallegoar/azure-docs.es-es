---
author: v-demjoh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 05/18/2020
ms.author: v-demjoh
ms.openlocfilehash: c73ee93d89a350763e8ced490187e1c47f8918de
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2020
ms.locfileid: "83806267"
---
## <a name="run-the-speech-cli"></a>Ejecución de la CLI de Voz

Ahora está listo para ejecutar la CLI de Voz para traducir la voz a texto en dos idiomas diferentes.

Desde la línea de comandos, cambie al directorio que contiene el archivo binario de la CLI de Voz y escriba:

```bash
spx translate --microphone --target de-DE --target es-MX
```

La CLI de Voz traducirá el lenguaje natural hablado en inglés en texto impreso en alemán y español (mexicano).
Presione Entrar para detener la herramienta.

> [!NOTE]
> La CLI de Voz tiene como valor predeterminado el inglés. Puede elegir un idioma diferente [en la tabla de voz a texto](../../../../language-support.md).
> Por ejemplo, agregue `--source ja-JP` para reconocer la voz en japonés.
