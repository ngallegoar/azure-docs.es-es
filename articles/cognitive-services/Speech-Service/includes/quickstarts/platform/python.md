---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 04/04/2020
ms.author: dapine
ms.openlocfilehash: 327000173b4c5c378f60c76eb1a24bd155901671
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2020
ms.locfileid: "80666917"
---
En esta guía se muestra cómo instalar el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para Python. Si desea simplemente que el nombre del paquete comience por su cuenta, ejecute `pip install azure-cognitiveservices-speech`.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="supported-operating-systems"></a>Sistemas operativos admitidos

- El paquete del SDK de Voz de Python está disponible para estos sistemas operativos:
  - Windows: x64 y x86
  - Mac: macOS X versión 10.12 o posterior
  - Linux: Ubuntu 16.04/18.04, Debian 9, RHEL 7/8, CentOS 7/8 en x64

## <a name="prerequisites"></a>Prerrequisitos

- Las plataformas Linux admitidas requerirán la instalación de determinadas bibliotecas (`libssl` para la compatibilidad con la capa de sockets seguros y `libasound2` para la compatibilidad con el audio). Consulte a continuación su distribución para saber cuáles son los comandos necesarios para instalar las versiones correctas de estas bibliotecas.

  - En Ubuntu, ejecute los siguientes comandos para instalar los paquetes necesarios:

        ```sh
        sudo apt-get update
        sudo apt-get install build-essential libssl1.0.0 libasound2
        ```

  - En Debian 9, ejecute los siguientes comandos para instalar los paquetes necesarios:

        ```sh
        sudo apt-get update
        sudo apt-get install build-essential libssl1.0.2 libasound2
        ```

  - En RHEL o CentOS 8, ejecute los siguientes comandos para instalar los paquetes necesarios:

        ```sh
        sudo yum update
        sudo yum install alsa-lib openssl python3
        ```

> [!NOTE]
> En RHEL/CentOS 8, siga las instrucciones sobre [cómo configurar OpenSSL para Linux](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md).

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

Si tiene un problema o falta una característica, consulte las [opciones de ayuda y soporte técnico](~/articles/cognitive-services/speech-service/support.md).

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [windows](../quickstart-list.md)]
