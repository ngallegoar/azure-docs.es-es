---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 5a06a0663601c221dd456b9cf4437cb9f32a18f8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "81399905"
---
:::row:::
    :::column span="3":::
        El SDK de voz de C++ está disponible en Windows, Linux y macOS. Para más información, consulte <a href="https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech" target="_blank">Microsoft.CognitiveServices.Speech <span class="docon docon-navigate-external x-hidden-focus"></span></a>.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="C++" src="https://docs.microsoft.com/media/logos/logo_Cplusplus.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

#### <a name="c-nuget-package"></a>Paquete NuGet de C++

El SDK de voz de C++ se puede instalar desde el **administrador de paquetes** con el siguiente comando `Install-Package`.

```powershell
Install-Package Microsoft.CognitiveServices.Speech
```

#### <a name="c-binaries-and-header-files"></a>Archivos binarios y de encabezado de C++

El SDK de voz de C++ también se puede instalar desde archivos binarios. Descargue el SDK en forma de <a href="https://aka.ms/csspeech/linuxbinary" target="_blank">paquete .tar<span class="docon docon-navigate-external x-hidden-focus"></span></a> y descomprima los archivos en el directorio que prefiera. El contenido de este paquete (que incluye los archivos de encabezado para las arquitecturas de destino x86 y x64) se estructura de la manera siguiente:

  | Path                   | Descripción                                          |
  |------------------------|------------------------------------------------------|
  | `license.md`           | Licencia                                              |
  | `ThirdPartyNotices.md` | Avisos de terceros                                  |
  | `include`              | Archivos de encabezado para C++                                 |
  | `lib/x64`              | Biblioteca x64 nativa para vincular con la aplicación |
  | `lib/x86`              | Biblioteca x86 nativa para vincular con la aplicación |

  Para crear una aplicación, copie o mueva los binarios (y bibliotecas) necesarios a su entorno de desarrollo. Inclúyalos según sea necesario en el proceso de compilación.

#### <a name="additional-resources"></a>Recursos adicionales

- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp" target="_blank">Código fuente de C++ del inicio rápido de Windows, Linux y macOS <span class="docon docon-navigate-external x-hidden-focus"></span></a>