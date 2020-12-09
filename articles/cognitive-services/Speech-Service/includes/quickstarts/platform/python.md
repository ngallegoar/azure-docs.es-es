---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 10/15/2020
ms.author: trbye
ms.openlocfilehash: d50d4c554f47629f6e04adf957e02f8ffcc48fe5
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96509255"
---
En esta guía se muestra cómo instalar el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para Python. Si desea simplemente que el nombre del paquete comience por su cuenta, ejecute `pip install azure-cognitiveservices-speech`.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="supported-operating-systems"></a>Sistemas operativos admitidos

- El paquete del SDK de Voz de Python está disponible para estos sistemas operativos:
  - Windows: x64 y x86
  - Mac: macOS X versión 10.12 o posterior
  - Linux: consulte la lista de [distribuciones y arquitecturas de destino de Linux admitidas](~/articles/cognitive-services/speech-service/speech-sdk.md).

## <a name="prerequisites"></a>Prerrequisitos

- Las plataformas Linux admitidas requerirán la instalación de determinadas bibliotecas (`libssl` para la compatibilidad con la capa de sockets seguros y `libasound2` para la compatibilidad con el audio). Consulte a continuación su distribución para saber cuáles son los comandos necesarios para instalar las versiones correctas de estas bibliotecas.

  - En Ubuntu y Debian, ejecute los siguientes comandos para instalar los paquetes requeridos:

    ```sh
    sudo apt-get update
    sudo apt-get install build-essential libssl1.0.0 libasound2
    ```

    Si libssl 1.0.0 no está disponible, instale libssl 1.0. x (donde x es mayor que 0) o libssl 1.1.

  - En RHEL o CentOS, ejecute los siguientes comandos para instalar los paquetes necesarios:

    ```sh
    sudo yum update
    sudo yum install alsa-lib openssl python3
    ```

> [!NOTE]
> - En RHEL/CentOS 7, siga las instrucciones sobre [cómo configurar RHEL/CentOS 7 para el SDK de voz](~/articles/cognitive-services/speech-service/how-to-configure-rhel-centos-7.md).
> - En RHEL/CentOS 8, siga las instrucciones sobre [cómo configurar OpenSSL para Linux](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md).

- En Windows, necesita [Microsoft Visual C++ Redistributable para Visual Studio 2019](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) para su plataforma. Tenga en cuenta que si es la primera vez que instala este paquete, puede que deba reiniciar Windows antes de seguir con esta guía.
- Y, por último, necesitará [Python, versiones 3.5 a 3.8](https://www.python.org/downloads/). Para comprobar la instalación, abra un símbolo del sistema y escriba el comando `python --version` y compruebe el resultado. Si está instalado correctamente, obtendrá una respuesta "Python 3.5.1" o similar.

## <a name="install-the-speech-sdk-from-pypi"></a>Instalación del SDK de Voz desde PyPI

Si usa su propio entorno o herramientas de compilación, ejecute el siguiente comando para instalar el SDK de Voz desde [PyPI](https://pypi.org/). Si usa Visual Studio Code, pase a la siguiente subsección para una instalación guiada.

```sh
pip install azure-cognitiveservices-speech
```

Si está en macOS, es posible que tenga que ejecutar el siguiente comando para que funcione el comando `pip` anterior:

```sh
python3 -m pip install --upgrade pip
```

Una vez que haya usado correctamente `pip` para instalar `azure-cognitiveservices-speech`, puede usar el SDK de Voz importando el espacio de nombres en los proyectos de Python.

```py
import azure.cognitiveservices.speech as speechsdk
```

## <a name="install-the-speech-sdk-using-visual-studio-code"></a>Instalación del SDK de Voz mediante Visual Studio Code

1. Descargue e instale la última versión compatible de [Python](https://www.python.org/downloads/) para la plataforma, las versiones 3.5 a 3.8.
   - Los usuarios de Windows deben asegurarse de que seleccionan "Add Python to your PATH" (Agregar Python a su RUTA) durante el proceso de instalación.
1. Descargue e instale [Visual Studio Code](https://code.visualstudio.com/Download).
1. Abra Visual Studio Code e instale la extensión de Python. Seleccione **File** > **Preferences** > **Extensions** (Archivo > Preferencias > Extensiones) en el menú. Busque **Python** y haga clic en **Instalar**.

   ![Instalación de la extensión de Python](~/articles/cognitive-services/speech-service/media/sdk/qs-python-vscode-python-extension.png)

1. También desde dentro de Visual Studio Code, instale el paquete de Python del SDK de Voz desde la línea de comandos integrada:
   1. Abra un terminal (en los menús desplegables **Terminal** > **Nuevo terminal**)
   1. En el terminal que se abre, escriba el comando `python -m pip install azure-cognitiveservices-speech`.

Si no está familiarizado con Visual Studio Code, consulte la [documentación más extensa sobre Visual Studio Code](https://code.visualstudio.com/docs). Para más información sobre Visual Studio Code y Python, consulte el [tutorial de Python para Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial).

## <a name="support-and-updates"></a>Soporte técnico y actualizaciones

Las actualizaciones del paquete de Python del SDK de Voz se distribuirán mediante PyPI y se anunciarán en la página [Notas de la versión](~/articles/cognitive-services/speech-service/releasenotes.md).
Si hay disponible una nueva versión, puede actualizarse a ella con el comando `pip install --upgrade azure-cognitiveservices-speech`.
Para comprobar qué versión está instalada actualmente, inspeccione la variable `azure.cognitiveservices.speech.__version__`.

Si tiene un problema o falta una característica, consulte las [opciones de ayuda y soporte técnico](../../../../cognitive-services-support-options.md?context=%2fazure%2fcognitive-services%2fspeech-service%2fcontext%2fcontext%253fcontext%253d%2fazure%2fcognitive-services%2fspeech-service%2fcontext%2fcontext).

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [windows](../quickstart-list.md)]