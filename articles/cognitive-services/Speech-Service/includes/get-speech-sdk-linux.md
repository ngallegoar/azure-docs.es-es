---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: trbye
ms.openlocfilehash: a93c478f0621bf62b710f58f3e6f06298bad9954
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83673057"
---
:::row:::
    :::column span="3":::
        El SDK de Voz solo admite **Ubuntu 16.04/18.04**, **Debian 9**, **Red Hat Enterprise Linux (RHEL) 7/8** y **CentOS 7/8** en las siguientes arquitecturas de destino cuando se usa con Linux:
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Linux" src="https://docs.microsoft.com/media/logos/logo_linux-color.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

- x86 (Debian/Ubuntu), x64, ARM32 (Debian/Ubuntu) y ARM64 (Debian/Ubuntu) para C++
- x64, ARM32 (Debian/Ubuntu) y ARM64 (Debian/Ubuntu) para Java
- x64, ARM32 (Debian/Ubuntu), y ARM64 (Debian/Ubuntu) para .NET Core
- x64 para Python

> [!IMPORTANT]
> Para C# en Linux ARM64, se requiere .NET Core 3.x (paquete dotnet-sdk-3.x).

### <a name="system-requirements"></a>Requisitos del sistema

En el caso de una aplicación nativa, el SDK de Voz se basa en `libMicrosoft.CognitiveServices.Speech.core.so`. Asegúrese de que la arquitectura de destino (x86, x64) coincide con la aplicación. Dependiendo de la versión de Linux, es posible que se requieran dependencias adicionales.

- Las bibliotecas compartidas de la biblioteca GNU C (incluida la biblioteca de programación de subprocesos de POSIX, `libpthreads`)
- La biblioteca OpenSSL (`libssl.so.1.0.0` o `libssl.so.1.0.2`)
- La biblioteca compartida para las aplicaciones ALSA (`libasound.so.2`)

# <a name="ubuntu-16041804"></a>[Ubuntu 16.04/18.04](#tab/ubuntu)

```Bash
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libasound2
```

# <a name="debian-9"></a>[Debian 9](#tab/debian)

```Bash
sudo apt-get update
sudo apt-get install build-essential libssl1.0.2 libasound2
```

# <a name="rhel-78-and-centos-78"></a>[RHEL 7/8 y CentOS 7/8](#tab/rhel-centos)

```Bash
sudo yum update
sudo yum install alsa-lib openssl
```

> [!IMPORTANT]
> - En RHEL/CentOS 7, siga las instrucciones sobre [cómo configurar RHEL/CentOS 7 para el SDK de voz](~/articles/cognitive-services/speech-service/how-to-configure-rhel-centos-7.md).
> - En RHEL/CentOS 8, siga las instrucciones sobre [cómo configurar OpenSSL para Linux](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md).

---

### <a name="c"></a>C#

[!INCLUDE [Get .NET Speech SDK](get-speech-sdk-dotnet.md)]

### <a name="c"></a>C++

[!INCLUDE [Get C++ Speech SDK](get-speech-sdk-cpp.md)]

### <a name="python"></a>Python

[!INCLUDE [Get Python Speech SDK](get-speech-sdk-python.md)]

### <a name="java"></a>Java

[!INCLUDE [Get Java Speech SDK](get-speech-sdk-java.md)]
