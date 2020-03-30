---
title: 'Desarrollo de aplicaciones con el SDK de Voz: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo implementar una aplicación que usa el SDK de Voz en plataformas compatibles.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/30/2020
ms.author: dapine
ms.custom: seodec18
ms.openlocfilehash: 9507428e63b337b3d8419a833d03d081d494c522
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78330811"
---
# <a name="ship-an-application"></a>Distribuir una aplicación

Respete la [licencia de Speech SDK](https://aka.ms/csspeech/license201809), así como los [avisos de software de terceros](https://csspeechstorage.blob.core.windows.net/drop/1.0.0/ThirdPartyNotices.html), cuando distribuya Speech SDK de Azure Cognitive Services. Además revise la [Declaración de privacidad de Microsoft](https://aka.ms/csspeech/privacy).

En función de la plataforma, existen otras dependencias para ejecutar la aplicación.

## <a name="windows"></a>Windows

El SDK de Voz de Cognitive Services se ha probado en Windows 10 y en Windows Server 2016.

Speech SDK de Cognitive Services requiere [Microsoft Visual C++ Redistributable para Visual Studio 2019](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) en el sistema. Puede descargar los instaladores para la versión más reciente de `Microsoft Visual C++ Redistributable for Visual Studio 2019` aquí:

- [Win32](https://aka.ms/vs/16/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/16/release/vc_redist.x64.exe)

Si en la aplicación se usa código administrado, se necesita `.NET Framework 4.6.1` o una versión posterior en el equipo de destino.

Para la entrada de micrófono, las bibliotecas de Media Foundation deben estar instaladas. Estas bibliotecas forman parte de Windows 10 y Windows Server 2016. Es posible usar Speech SDK sin estas bibliotecas, siempre y cuando no se use un micrófono como dispositivo de entrada de audio.

Los archivos necesarios del SDK de Voz se pueden implementar en el mismo directorio que la aplicación. De esta forma la aplicación puede acceder directamente a las bibliotecas. Asegúrese de seleccionar la versión correcta (Win32/x64) que coincida con la aplicación.

| Nombre | Función |
| :--- | :------- |
| `Microsoft.CognitiveServices.Speech.core.dll`   | SDK básico, necesario para la implementación nativa y administrada |
| `Microsoft.CognitiveServices.Speech.csharp.dll` | Necesario para la implementación administrada                      |

> [!NOTE]
> A partir de la versión 1.3.0, ya no es necesario incluir el archivo `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (incluido en versiones anteriores). La funcionalidad está ahora integrada en el SDK principal.

> [!NOTE]
> Para el proyecto C# de la aplicación Windows Forms (.NET Framework), asegúrese de que las bibliotecas estén incluidas en la configuración de implementación de su proyecto. Puede comprobar esto en `Properties -> Publish Section`. Haga clic en el botón `Application Files` y busque las bibliotecas correspondientes en la lista desplegable. Asegúrese de que el valor esté establecido en `Included`. Visual Studio incluirá el archivo cuando se publique o implemente el proyecto.

## <a name="linux"></a>Linux

El SDK de Voz admite actualmente las distribuciones Ubuntu 16.04 y 18.04, Debian 9, RHEL 8 y CentOS 8.
Para una aplicación nativa, se debe distribuir la biblioteca de Speech SDK, `libMicrosoft.CognitiveServices.Speech.core.so`.
Asegúrese de seleccionar la versión (x86, x64) que coincida con la aplicación. Según la versión de Linux, es posible que también tenga que incluir las dependencias siguientes:

- Las bibliotecas compartidas de la biblioteca GNU C (incluida la biblioteca de programación de subprocesos de POSIX, `libpthreads`)
- La biblioteca OpenSSL (`libssl.so.1.0.0` o `libssl.so.1.0.2`)
- La biblioteca compartida para las aplicaciones ALSA (`libasound.so.2`)

En Ubuntu, las bibliotecas de GNU C ya deberían estar instaladas de forma predeterminada. Las tres últimas se pueden instalar con estos comandos:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libasound2
```

En Debian 9 instale estos paquetes:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.2 libasound2
```

En On RHEL/CentOS 8:

```sh
sudo yum update
sudo yum install alsa-lib openssl
```

> [!NOTE]
> En RHEL/CentOS 8, siga las instrucciones sobre [cómo configurar OpenSSL para Linux](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md).

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
- [Vea cómo funciona el reconocimiento de voz en C#](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet)
