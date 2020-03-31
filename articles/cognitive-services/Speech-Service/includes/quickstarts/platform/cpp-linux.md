---
title: 'Inicio rápido: Configuración de la plataforma para usar C++ en Linux con el SDK de Voz: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Use esta guía para configurar la plataforma para usar C++ en Linux con el SDK del servicio de voz.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/14/2019
ms.author: erhopf
ms.openlocfilehash: 4f211e4b90dcc8bffa2fbba6fa4783caf846f50c
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "78383845"
---
En esta guía se muestra cómo instalar el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para Linux.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="system-requirements"></a>Requisitos del sistema

Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9, RHEL 8, CentOS 8)

## <a name="prerequisites"></a>Prerrequisitos

Para realizar este inicio rápido, necesita lo siguiente:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)

* Las plataformas Linux admitidas requerirán la instalación de determinadas bibliotecas (`libssl` para la compatibilidad con la capa de sockets seguros y `libasound2` para la compatibilidad con el audio). Consulte a continuación su distribución para saber cuáles son los comandos necesarios para instalar las versiones correctas de estas bibliotecas.

   * En Ubuntu:

     ```sh
     sudo apt-get update
     sudo apt-get install build-essential libssl1.0.0 libasound2 wget
     ```

   * En Debian 9:

     ```sh
     sudo apt-get update
     sudo apt-get install build-essential libssl1.0.2 libasound2 wget
     ```

   * En On RHEL/CentOS 8:

     ```sh
     sudo yum update
     sudo yum groupinstall "Development tools"
     sudo yum install alsa-lib openssl wget
     ```

> [!NOTE]
> En RHEL/CentOS 8, siga las instrucciones sobre [cómo configurar OpenSSL para Linux](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md).

## <a name="install-speech-sdk"></a>Instalación del SDK de Voz

El SDK de Voz para Linux se puede usar para crear aplicaciones de 64 bits y 32 bits. Las bibliotecas y los archivos de encabezado necesarios se pueden descargar como un archivo tar desde https://aka.ms/csspeech/linuxbinary.

Descargue e instale el SDK de la forma siguiente:

1. Seleccione el directorio al que desea extraer los archivos del SDK de Voz y configure la variable de entorno `SPEECHSDK_ROOT` para que apunte a ese directorio. Esta variable facilita la referencia al directorio en futuros comandos. Por ejemplo, si desea usar el directorio `speechsdk` en el directorio principal, use un comando similar al siguiente:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Cree el directorio si aún no existe.

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Descargue y extraiga el archivo `.tar.gz` que contienen los archivos binarios del SDK de Voz:

   ```sh
   wget -O SpeechSDK-Linux.tar.gz https://aka.ms/csspeech/linuxbinary
   tar --strip 1 -xzf SpeechSDK-Linux.tar.gz -C "$SPEECHSDK_ROOT"
   ```

1. Valide el contenido del directorio de nivel superior del paquete extraído:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   La lista de directorios debe contener los archivos de avisos y licencias de terceros, así como un directorio `include` que contenga archivos de encabezado (`.h`) y un directorio `lib` que contenga bibliotecas.

   [!INCLUDE [Linux Binary Archive Content](~/includes/cognitive-services-speech-service-linuxbinary-content.md)]

Ahora puede continuar con la sección [Pasos siguientes](#next-steps).

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [windows](../quickstart-list.md)]
