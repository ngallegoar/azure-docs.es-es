---
title: Conceptos básicos sobre la CLI de Voz
titleSuffix: Azure Cognitive Services
description: Aprenda a usar la herramienta de comandos CLI de Voz para trabajar con el servicio de voz sin código y con una configuración mínima.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 04/04/2020
ms.author: trbye
ms.openlocfilehash: 7ca290b7b7df9e4e03aec78b611361ba52942523
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93286690"
---
# <a name="learn-the-basics-of-the-speech-cli"></a>Conozca los aspectos básicos de la CLI de Voz.

En este artículo, conocerá los patrones de uso básicos de la CLI de Voz, una herramienta de línea de comandos que permite usar el servicio de voz sin necesidad de escribir código. Puede probar rápidamente las características principales del servicio de voz sin crear entornos de desarrollo ni escribir código, para ver si los casos de uso se pueden satisfacer adecuadamente. Además, la CLI de Voz está lista para producción y se puede usar para automatizar flujos de trabajo sencillos en el servicio de voz mediante scripts de shell o `.bat`.

[!INCLUDE [](includes/spx-setup.md)]

## <a name="basic-usage"></a>Uso básico

En esta sección se muestran algunos comandos básicos de SPX que a menudo resultan útiles para la prueba y la experimentación iniciales. Para empezar, vea la ayuda integrada en la herramienta, para lo que debe ejecutar el siguiente comando.

```shell
spx
```

Observe los temas de ayuda que **se muestran** a la derecha de los parámetros del comando. Puede escribir estos comandos para obtener ayuda detallada acerca de los subcomandos.

Puede buscar temas de ayuda por palabra clave. Por ejemplo, escriba el siguiente comando para ver una lista de ejemplos de uso de la CLI de Voz:

```shell
spx help find --topics "examples"
```

Escriba el siguiente comando para ver las opciones del comando recognize:

```shell
spx help recognize
```

Ahora, use el servicio de voz para realizar un reconocimiento de la voz con el micrófono predeterminado, para lo que se debe ejecutar el siguiente comando.

```shell
spx recognize --microphone
```

Después de escribir el comando, SPX comenzará a escuchar el audio en el dispositivo de entrada activo actual y se detendrá después de presionar `ENTER`. La voz grabada se reconoce y se convierte a texto en la salida de la consola. La síntesis de texto a voz también es fácil de usar con la CLI de Voz. 

Al ejecutar el comando siguiente, se toma el texto escrito como entrada y se genera la voz sintetizada en el dispositivo de salida activo actual.

```shell
spx synthesize --text "Testing synthesis using the Speech CLI" --speakers
```

Además del reconocimiento y la síntesis de voz, también puede realizar la traducción de voz con la CLI de Voz. Igual que con el comando de reconocimiento de voz anterior, ejecute el siguiente comando para capturar audio desde el micrófono predeterminado y realizar la traducción a texto en el idioma de destino.

```shell
spx translate --microphone --source en-US --target ru-RU --output file C:\some\file\path\russian_translation.txt
```

En este comando, se especifican los idiomas de origen (el idioma **del** que se va a traducir) y de destino (el idioma **al** que se traduce). Al usar el argumento `--microphone`, se escuchará el audio en el dispositivo de entrada activo actual y se detendrá después de presionar `ENTER`. El resultado es una traducción del texto en el idioma de destino, escrito en un archivo de texto.

> [!NOTE]
> Consulte el [artículo sobre los idiomas y las configuraciones regionales](language-support.md) para obtener una lista de todos los idiomas compatibles con sus códigos de configuración regional correspondientes.

### <a name="configuration-files-in-the-datastore"></a>Archivos de configuración en el almacén de archivos

El comportamiento de la CLI de voz utiliza los valores de los archivos de configuración, a los se puede hacer referencia en las llamadas de la CLI mediante un símbolo @.
La CLI de voz guarda los valores nuevos en un subdirectorio `./spx/data` nuevo que crea en el directorio de trabajo actual.
Al buscar un valor de configuración, la CLI de voz busca en el directorio de trabajo actual, luego en el almacén de archivos de `./spx/data`y, después, en otros almacenes de archivos, incluido un almacén de datos de solo lectura final que se encuentra en el binario `spx`.
Anteriormente se usaba el almacén de datos para guardar los valores `@key` y `@region`, por lo que no era necesario especificarlos con cada llamada a la línea de comandos.
También puede usar los archivos de configuración para almacenar su propia configuración, o incluso para pasar direcciones URL o cualquier otro contenido dinámico que se haya generado en el runtime.

En esta sección se muestra cómo se usa un archivo de configuración en el almacén de datos local para almacenar y capturar la configuración de comandos mediante `spx config`, así como para almacenar la salida de la CLI de Voz mediante la opción `--output`.

En el ejemplo siguiente se borra el archivo de configuración `@my.defaults`, se agregan pares clave-valor para **clave** y **región** en el archivo, y se usa la configuración en una llamada a `spx recognize`.

```shell
spx config @my.defaults --clear
spx config @my.defaults --add key 000072626F6E20697320636F6F6C0000
spx config @my.defaults --add region westus

spx config @my.defaults

spx recognize --nodefaults @my.defaults --file hello.wav
```

En los archivos de configuración también se puede escribir contenido dinámico. Por ejemplo, el siguiente comando crea un modelo de voz personalizado y almacena la dirección URL del mismo en un archivo de configuración. El siguiente comando espera hasta que el modelo de la dirección URL esté listo para su uso antes de volver.

```shell
spx csr model create --name "Example 4" --datasets @my.datasets.txt --output url @my.model.txt
spx csr model status --model @my.model.txt --wait
```

En el ejemplo siguiente se escriben dos direcciones URL en el archivo de configuración `@my.datasets.txt`.
En este escenario, existe la opción de que `--output` incluya la palabra clave **add** para crear un archivo de configuración o anexarlo al existente.


```shell
spx csr dataset create --name "LM" --kind Language --content https://crbn.us/data.txt --output url @my.datasets.txt
spx csr dataset create --name "AM" --kind Acoustic --content https://crbn.us/audio.zip --output add url @my.datasets.txt

spx config @my.datasets.txt
```

Para más información sobre los archivos del almacén de datos, incluido el uso de archivos de configuración predeterminados (`@spx.default`, `@default.config` y `@*.default.config` para la configuración predeterminada específica del comando), escriba este comando:

```shell
spx help advanced setup
```

## <a name="batch-operations"></a>Operaciones por lotes

Los comandos de la sección anterior son excelentes para ver rápidamente cómo funciona el servicio de voz. Sin embargo, al evaluar si se pueden satisfacer o no los casos de uso, es probable que necesite realizar operaciones por lotes en un intervalo de entrada que ya tenga para ver cómo el servicio controla una variedad de escenarios. En esta sección se muestra cómo:

* Ejecutar el reconocimiento de voz por lotes en un directorio de archivos de audio
* Recorrer en iteración un archivo `.tsv` y ejecutar la síntesis de texto a voz por lotes

## <a name="batch-speech-recognition"></a>Reconocimiento de voz por lotes

Si tiene un directorio de archivos de audio, con la CLI de Voz es fácil ejecutar rápidamente el reconocimiento de voz por lotes. Simplemente ejecute el siguiente comando y apunte al directorio con el comando `--files`. En este ejemplo, se anexa `\*.wav` al directorio para reconocer todos los archivos `.wav` que existen en el directorio. Además, especifique el argumento `--threads` para ejecutar el reconocimiento en 10 subprocesos paralelos.

> [!NOTE]
> El argumento `--threads` también se puede usar en la siguiente sección para los comandos `spx synthesize`, y los subprocesos que estén disponibles dependerán de la CPU y de su porcentaje de carga actual.

```shell
spx recognize --files C:\your_wav_file_dir\*.wav --output file C:\output_dir\speech_output.tsv --threads 10
```

La salida de voz reconocida se escribe en `speech_output.tsv` mediante el argumento `--output file`. Este es un ejemplo de la estructura del archivo de salida.

```output
audio.input.id    recognizer.session.started.sessionid    recognizer.recognized.result.text
sample_1    07baa2f8d9fd4fbcb9faea451ce05475    A sample wave file.
sample_2    8f9b378f6d0b42f99522f1173492f013    Sample text synthesized.
```

## <a name="batch-text-to-speech-synthesis"></a>Síntesis de texto a voz por lotes

La forma más sencilla de ejecutar la conversión de texto a voz por lotes es crear un archivo `.tsv` (con valores separados por tabulaciones) y aprovechar el comando `--foreach` de la CLI de Voz. Considere el siguiente archivo `text_synthesis.tsv`:

```output
audio.output    text
C:\batch_wav_output\wav_1.wav    Sample text to synthesize.
C:\batch_wav_output\wav_2.wav    Using the Speech CLI to run batch-synthesis.
C:\batch_wav_output\wav_3.wav    Some more text to test capabilities.
```

 A continuación, ejecutará un comando para que apunte a `text_synthesis.tsv`, realizará la síntesis en cada campo `text` y escribirá el resultado en la ruta de acceso de `audio.output` correspondiente como un archivo `.wav`. 

```shell
spx synthesize --foreach in @C:\your\path\to\text_synthesis.tsv
```

Este comando equivale a ejecutar `spx synthesize --text Sample text to synthesize --audio output C:\batch_wav_output\wav_1.wav` **para cada** registro en el archivo `.tsv`. Tenga en cuenta lo siguiente:

* Los encabezados de columna, `audio.output` y `text`, se corresponden con los argumentos de la línea de comandos `--audio output` y `--text`, respectivamente. Los argumentos de la línea de comandos de varias partes, como `--audio output`, deben tener el formato del archivo sin espacios, guiones iniciales y puntos que separen las cadenas, por ejemplo `audio.output`. Cualquier otro argumento de la línea de comandos existente se puede agregar al archivo como columnas adicionales mediante este patrón.
* Cuando el archivo se formatea de esta manera, no es necesario pasar argumentos adicionales a `--foreach`.
* Asegúrese de separar cada valor del archivo `.tsv` con una **tabulación**.

Sin embargo, si tiene un archivo `.tsv` como en el ejemplo siguiente, con encabezados de columna que **no coinciden** con los argumentos de la línea de comandos:

```output
wav_path    str_text
C:\batch_wav_output\wav_1.wav    Sample text to synthesize.
C:\batch_wav_output\wav_2.wav    Using the Speech CLI to run batch-synthesis.
C:\batch_wav_output\wav_3.wav    Some more text to test capabilities.
```

Estos nombres de campo se pueden invalidar con los argumentos correctos mediante la siguiente sintaxis en la llamada `--foreach`. Esta es la misma llamada que la anterior.

```shell
spx synthesize --foreach audio.output;text in @C:\your\path\to\text_synthesis.tsv
```

## <a name="next-steps"></a>Pasos siguientes

* Realice los inicios rápidos sobre el [reconocimiento de voz](get-started-speech-to-text.md?pivots=programmer-tool-spx) o la [síntesis de voz](get-started-text-to-speech.md?pivots=programmer-tool-spx) con la CLI de voz.
