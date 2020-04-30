---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 5545b10a4228448d49849e7cd52728febf14ce2d
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "81399873"
---
:::row:::
    :::column span="3":::
        El SDK de voz de JavaScript está disponible como un paquete npm; vea <a href="https://www.npmjs.com/package/microsoft-cognitiveservices-speech-sdk" target="_blank">microsoft-cognitiveservices-speech-sdk <span class="docon docon-navigate-external x-hidden-focus"></span></a> y su repositorio de GitHub complementario <a href="https://github.com/Microsoft/cognitive-services-speech-sdk-js" target="_blank">cognitive-services-speech-sdk-js <span class="docon docon-navigate-external x-hidden-focus"></span></a>.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="JavaScript" src="https://docs.microsoft.com/media/logos/logo_js.svg"  width="60px">
        </div>
    :::column-end:::
:::row-end:::

> [!TIP]
> Aunque el SDK de voz de JavaScript está disponible como un paquete npm, los exploradores web de cliente y Node.js pueden utilizarlo; tenga en cuenta las distintas implicaciones arquitectónicas de cada entorno. Por ejemplo, la instancia de <a href="https://en.wikipedia.org/wiki/Document_Object_Model" target="_blank">Document Object Model (DOM) <span class="docon docon-navigate-external x-hidden-focus"></span></a> no está disponible para las aplicaciones del lado servidor, igual que el <a href="https://nodejs.org/api/fs.html" target="_blank">sistema de archivos <span class="docon docon-navigate-external x-hidden-focus"></span></a> no está disponible para las aplicaciones del lado cliente.

### <a name="nodejs-package-manager-npm"></a>Administrador de paquetes de Node.js (NPM)

Para instalar el SDK de voz de JavaScript, ejecute el siguiente comando `npm install`.

```nodejs
npm install microsoft-cognitiveservices-speech-sdk
```

### <a name="html-script-tag"></a>Etiqueta de script HTML

Como alternativa, podría incluir directamente una etiqueta `<script>` en el elemento HTML `<head>`, confiando en la distribución NPM <a href="https://www.jsdelivr.com/package/npm/microsoft-cognitiveservices-speech-sdk" target="_blank">**JSDelivr** <span class="docon docon-navigate-external x-hidden-focus"></span></a>.

```html
<script src="https://cdn.jsdelivr.net/npm/microsoft-cognitiveservices-speech-sdk@1.10.1/distrib/lib/microsoft.cognitiveservices.speech.sdk.min.js">
</script>
```

Para más información, vea la <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/browser" target="_blank">guía de inicio rápido del SDK de voz del explorador web <span class="docon docon-navigate-external x-hidden-focus"></span></a>.
