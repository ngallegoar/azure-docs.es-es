---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.custom: devx-track-js
ms.openlocfilehash: 3b7f459e0e95e798e5430414d23f34a455719a40
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91376390"
---
:::row:::
    :::column span="3":::
        El SDK de Voz para JavaScript está disponible como paquete npm. Consulte <a href="https://www.npmjs.com/package/microsoft-cognitiveservices-speech-sdk" target="_blank">microsoft-cognitiveservices-speech-sdk <span class="docon docon-navigate-external x-hidden-focus"></span></a> y su repositorio de GitHub complementario <a href="https://github.com/Microsoft/cognitive-services-speech-sdk-js" target="_blank">cognitive-services-speech-sdk-js <span class="docon docon-navigate-external x-hidden-focus"></span></a>.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Node.js" src="https://docs.microsoft.com/media/logos/logo_nodejs.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

> [!TIP]
> Aunque el SDK de Voz para JavaScript está disponible como paquete npm, tanto Node.js como los exploradores web de cliente pueden utilizarlo; tenga en cuenta las distintas implicaciones arquitectónicas de cada entorno. Por ejemplo, la instancia de <a href="https://en.wikipedia.org/wiki/Document_Object_Model" target="_blank">Document Object Model (DOM) <span class="docon docon-navigate-external x-hidden-focus"></span></a> no está disponible para las aplicaciones del lado servidor, igual que el <a href="https://nodejs.org/api/fs.html" target="_blank">sistema de archivos <span class="docon docon-navigate-external x-hidden-focus"></span></a> no está disponible para las aplicaciones del lado cliente.

### <a name="nodejs-package-manager-npm"></a>Administrador de paquetes de Node.js (NPM)

Para instalar el SDK de Voz para JavaScript, ejecute el siguiente comando `npm install`.

```nodejs
npm install microsoft-cognitiveservices-speech-sdk
```

Para obtener más información, consulte la <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/node" target="_blank">guía de inicio rápido del SDK de Voz de Node.js <span class="docon docon-navigate-external x-hidden-focus"></span></a>.
