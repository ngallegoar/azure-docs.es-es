---
title: Etiquetas de imágenes de contenedor de Cognitive Services
titleSuffix: Azure Cognitive Services
description: Una lista completa de todas las etiquetas de imágenes de contenedor de Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: reference
ms.date: 11/06/2020
ms.author: aahi
ms.openlocfilehash: 0efb278c217ede94f67b47250633622501a24414
ms.sourcegitcommit: 17b36b13857f573639d19d2afb6f2aca74ae56c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94412551"
---
# <a name="azure-cognitive-services-container-image-tags-and-release-notes"></a>Etiquetas de imágenes de contenedor de Azure Cognitive Services notas de la versión

Azure Cognitive Services ofrece muchas imágenes de contenedor. Los registros de contenedor y los repositorios correspondientes varían entre las imágenes de contenedor. Cada nombre de imagen de contenedor ofrece varias etiquetas. Una etiqueta de imagen de contenedor es un mecanismo de control de versiones de la imagen de contenedor. Este artículo está pensado para usarse como una referencia completa para mostrar todas las imágenes de contenedor de Cognitive Services y sus etiquetas disponibles.

> [!TIP]
> Al usar [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/), preste especial atención a las mayúsculas y minúsculas del registro de contenedor, el repositorio, el nombre de la imagen del contenedor y la etiqueta correspondiente, ya que se **distingue mayúsculas de minúsculas**.

## <a name="anomaly-detector"></a>Anomaly Detector

La imagen del contenedor de [Anomaly Detector][ad-containers] se puede encontrar en el sindicato del registro de contenedor de `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/decision` y se denomina `anomaly-detector`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/decision/anomaly-detector`.

Esta imagen de contenedor tiene disponibles las siguientes etiquetas. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/decision/anomaly-detector/tags/list).

# <a name="latest-version"></a>[La versión más reciente](#tab/current)

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013560003-amd64-preview` |      |

# <a name="previous-versions"></a>[Versiones anteriores](#tab/previous)

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `1.1.012300001-amd64-preview` |       |

---

## <a name="read-ocr-optical-character-recognition"></a>Read OCR (reconocimiento óptico de caracteres)

La imagen de contenedor de [Computer Vision][cv-containers] Read OCR se puede encontrar en la distribución del registro de contenedor de `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services` y se denomina `read`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/vision/read`.

Esta imagen de contenedor tiene disponibles las siguientes etiquetas. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/vision/read/tags/list).

# <a name="latest-version"></a>[La versión más reciente](#tab/current)

Notas de la versión `v2.0.013250001-amd64-preview`:

* Se reduce aún más el uso de memoria para el contenedor.
* La memoria caché externa es necesaria para la configuración de varios pods. Por ejemplo, configure Redis para el almacenamiento en caché.
* Se han corregido los resultados que faltan cuando se configura Redis Cache y `ResultExpirationPeriod` se establece en 0.
* Se elimina el límite de 26 MB para el tamaño del cuerpo de la solicitud. Ahora, el contenedor puede aceptar archivos de más de 26 MB.
* Se agrega una marca de tiempo y una versión de compilación al registro de la consola.

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `latest`                      |       |
| `2.0.013250001-amd64-preview` |  |

# <a name="previous-versions"></a>[Versiones anteriores](#tab/previous)

Notas de la versión `1.1.013050001-amd64-preview`

* Se ha agregado la configuración de inicialización de contenedores `ReadEngineConfig:ResultExpirationPeriod` para especificar el momento en que el sistema debe limpiar los resultados del reconocimiento. 
    * La configuración se encuentra en horas y el valor predeterminado es de 48 h.
    * La configuración puede reducir el uso de memoria para el almacenamiento de resultados, en especial cuando se usa el almacenamiento en memoria del contenedor.
    * Ejemplo 1. ReadEngineConfig:ResultExpirationPeriod=1: el sistema borrará el resultado del reconocimiento 1 h después del proceso.
    * Si este valor se establece en 0, el sistema borrará el resultado de la operación de reconocimiento una vez recuperado.

* Se ha corregido un error interno del servidor (500) cuando se pasa un formato de imagen no válido al sistema. Ahora se devolverá un error 400:

    ```json
    {
        "error": {
        "code": "InvalidImageSize",
        "message": "Image must be between 1024 and 209715200 bytes."
        }
    }
    ```

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `1.1.013050001-amd64-preview` |       |
| `1.1.011580001-amd64-preview` |       |
| `1.1.009920003-amd64-preview` |       |
| `1.1.009910003-amd64-preview` |       |

---


## <a name="form-recognizer"></a>Form Recognizer

La imagen del contenedor de [Form Recognizer][fr-containers] se puede encontrar en la distribución del registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/custom-form` y se denomina `labeltool`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool`.

Esta imagen de contenedor tiene disponibles las siguientes etiquetas. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/custom-form/labeltool/tags/list).

# <a name="latest-version"></a>[La versión más reciente](#tab/current)

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |


# <a name="previous-versions"></a>[Versiones anteriores](#tab/previous)

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `1.1.008640001-amd64-preview` |       |
| `1.1.008510001-amd64-preview` |       |

---

## <a name="language-understanding-luis"></a>Language Understanding (LUIS)

La imagen de contenedor [LUIS][lu-containers] se puede encontrar en el sindicato de registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/language` y se denomina `luis`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/language/luis`.

Esta imagen de contenedor tiene disponibles las siguientes etiquetas. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/language/luis/tags/list).

# <a name="latest-version"></a>[La versión más reciente](#tab/current)

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.012280003-amd64-preview` |       |


# <a name="previous-version"></a>[Versión anterior](#tab/previous)

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `1.1.012130003-amd64-preview` |       |

---

## <a name="custom-speech-to-text"></a>Conversión de voz a texto personalizada

La imagen de contenedor [Custom Speech-to-text][sp-cstt] se puede encontrar en la distribución del registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/speechservices/` y se denomina `custom-speech-to-text`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-speech-to-text`. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/custom-speech-to-text/tags/list).


# <a name="latest-version"></a>[La versión más reciente](#tab/current)

Nota de la versión `2.6.0-amd64`:

**Características**
* Compatibilidad con Phraselist v2 
* Se admiten listas de frases en las siguientes configuraciones regionales:
    * en-au
    * en-ca
    * en-gb
    * en-in
    * en-nz
    * es-es
    * zh-CN
* Compatibilidad con la descarga del modelo base personalizado. 
    * Use `BaseModelLocale=<locale>` con el comando `docker run`.
* Completamente migrado a .NET 3.1

**Correcciones**
* Se ha corregido un problema por el que la puntuación de confianza era siempre 1 en el modo Diarización.
* Se ha migrado a la API TextAnalytics 3.0

Tenga en cuenta que debido a las listas de frases incluidas, el tamaño de esta imagen de contenedor ha aumentado.

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `latest`                      |       |
| `2.6.0-amd64`                 |       |


# <a name="previous-version"></a>[Versión anterior](#tab/previous)

Nota de la versión `2.5.0-amd64`:

**Características**
* Admite la pronunciación personalizada en modelos personalizados
* Admite las nubes de Azure y Azure for US Government

**Correcciones**
* Se ha corregido un problema con la ejecución de un usuario no raíz en el modo Diarización.

| Etiquetas de imagen                    | Notas               |
|-------------------------------|:--------------------|
| `2.5.0-amd64`                 |   Primera versión con disponibilidad general    |

---

## <a name="custom-text-to-speech"></a>Conversión de texto a voz personalizada

La imagen de contenedor [Conversión de texto a voz personalizada][sp-ctts] se puede encontrar en la distribución del registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/speechservices/` y se denomina `custom-text-to-speech`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-text-to-speech`. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/custom-text-to-speech/tags/list).


# <a name="latest-version"></a>[La versión más reciente](#tab/current)

Nota de la versión `1.8.0-amd64`:

**Características**
* Completamente migrado a .NET 3.1

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `latest`                      |       |
| `1.8.0-amd64`                 |       |


# <a name="previous-version"></a>[Versión anterior](#tab/previous)

Nota de la versión `1.7.0-amd64`:

**Característica**
* Parcialmente migrada a .NET 3.1

| Etiquetas de imagen                    | Notas               |
|-------------------------------|:--------------------|
| `1.7.0-amd64`                 |   Primera versión con disponibilidad general    |

---

## <a name="speech-to-text"></a>Voz a texto

La imagen de contenedor [Speech-to-text][sp-stt] se puede encontrar en la distribución del registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/speechservices/` y se denomina `speech-to-text`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/speechservices/speech-to-text`. Puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/speech-to-text/tags/list).

Desde la versión 2.5.0 de Speech-to-text, se admiten imágenes en la región de *Virginia del gobierno de Estados Unidos*. Use el punto de conexión de facturación de la región de *Virginia del gobierno de Estados Unidos* y las claves de API al usar esta región.

# <a name="latest-version"></a>[La versión más reciente](#tab/current)

Nota de la versión `2.6.0-amd64-<locale>`:

**Características**
* Se ha actualizado a los modelos más recientes y se ha migrado completamente a .NET 3.1
* Compatibilidad con Phraselist v2
* Se admiten listas de frases en las siguientes configuraciones regionales:
    * en-au
    * en-ca
    * en-gb
    * en-in
    * en-nz
    * es-es
    * zh-CN
* Se admite la nueva configuración regional `cs-CZ`. 
    * Actualmente, no se admite el uso de mayúsculas ni el de puntuación.

**Correcciones**
* Se ha corregido un problema por el que las puntuaciones de confianza eran siempre 1 en el modo Diarización.
* Se ha migrado a la API TextAnalytics 3.0

Tenga en cuenta que debido a las listas de frases incluidas, el tamaño de esta imagen de contenedor ha aumentado. 

| Etiquetas de imagen                    | Notas                                                                                                | 
|-------------------------------|:-----------------------------------------------------------------------------------------------------|
| `latest`                      | Imagen de contenedor con la configuración regional `en-US`.                                                             |
| `2.6.0-amd64-<locale>`        | Sustituya `<locale>` por una de las configuraciones regionales disponibles que se muestran a continuación. Por ejemplo, `2.6.0-amd64-en-us`. |

Este contenedor tiene las siguientes configuraciones regionales disponibles.

| Configuración regional para v2.6.0           | Notas                                    |
|-----------------------------|:-----------------------------------------|
| `ar-ae`                     | Imagen de contenedor con la configuración regional `ar-AE`. |
| `ar-eg`                     | Imagen de contenedor con la configuración regional `ar-EG`. |
| `ar-kw`                     | Imagen de contenedor con la configuración regional `ar-KW`. |
| `ar-qa`                     | Imagen de contenedor con la configuración regional `ar-QA`. |
| `ar-sa`                     | Imagen de contenedor con la configuración regional `ar-SA`. |
| `ca-es`                     | Imagen de contenedor con la configuración regional `ca-ES`. |
| `cs-cz`                     | Imagen de contenedor con la configuración regional `cs-CZ`. |
| `da-dk`                     | Imagen de contenedor con la configuración regional `da-DK`. |
| `de-de`                     | Imagen de contenedor con la configuración regional `de-DE`. |
| `en-au`                     | Imagen de contenedor con la configuración regional `en-AU`. |
| `en-ca`                     | Imagen de contenedor con la configuración regional `en-CA`. |
| `en-gb`                     | Imagen de contenedor con la configuración regional `en-GB`. |
| `en-in`                     | Imagen de contenedor con la configuración regional `en-IN`. |
| `en-nz`                     | Imagen de contenedor con la configuración regional `en-NZ`. |
| `en-us`                     | Imagen de contenedor con la configuración regional `en-US`. |
| `es-es`                     | Imagen de contenedor con la configuración regional `es-ES`. |
| `es-mx`                     | Imagen de contenedor con la configuración regional `es-MX`. |
| `fi-fi`                     | Imagen de contenedor con la configuración regional `fi-FI`. |
| `fr-ca`                     | Imagen de contenedor con la configuración regional `fr-CA`. |
| `fr-fr`                     | Imagen de contenedor con la configuración regional `fr-FR`. |
| `gu-in`                     | Imagen de contenedor con la configuración regional `gu-IN`. |
| `hi-in`                     | Imagen de contenedor con la configuración regional `hi-IN`. |
| `it-it`                     | Imagen de contenedor con la configuración regional `it-IT`. |
| `ja-jp`                     | Imagen de contenedor con la configuración regional `ja-JP`. |
| `ko-kr`                     | Imagen de contenedor con la configuración regional `ko-KR`. |
| `mr-in`                     | Imagen de contenedor con la configuración regional `mr-IN`. |
| `nb-no`                     | Imagen de contenedor con la configuración regional `nb-NO`. |
| `nl-nl`                     | Imagen de contenedor con la configuración regional `nl-NL`. |
| `pl-pl`                     | Imagen de contenedor con la configuración regional `pl-PL`. |
| `pt-br`                     | Imagen de contenedor con la configuración regional `pt-BR`. |
| `pt-pt`                     | Imagen de contenedor con la configuración regional `pt-PT`. |
| `ru-ru`                     | Imagen de contenedor con la configuración regional `ru-RU`. |
| `sv-se`                     | Imagen de contenedor con la configuración regional `sv-SE`. |
| `ta-in`                     | Imagen de contenedor con la configuración regional `ta-IN`. |
| `te-in`                     | Imagen de contenedor con la configuración regional `te-IN`. |
| `th-th`                     | Imagen de contenedor con la configuración regional `th-TH`. |
| `tr-tr`                     | Imagen de contenedor con la configuración regional `tr-TR`. |
| `zh-cn`                     | Imagen de contenedor con la configuración regional `zh-CN`. |
| `zh-hk`                     | Imagen de contenedor con la configuración regional `zh-HK`. |
| `zh-tw`                     | Imagen de contenedor con la configuración regional `zh-TW`. |


# <a name="previous-version"></a>[Versión anterior](#tab/previous)

Nota de la versión `2.5.0-amd64-<locale>`:

**Características**
* Compatibilidad con la nube de Azure for US Government

**Correcciones**
* Corrige un problema con la ejecución de un usuario no raíz en el modo Diarización

| Etiquetas de imagen                  | Notas                                    |
|-----------------------------|:-----------------------------------------|
| `2.5.0-amd64-<locale>`      | Sustituya `<locale>` por una de las configuraciones regionales disponibles que se muestran a continuación. Por ejemplo, `2.5.0-amd64-en-us`.  |

Este contenedor tiene las siguientes configuraciones regionales disponibles.

| Configuración regional para v2.5.0           | Notas                                    |
|-----------------------------|:-----------------------------------------|
| `ar-ae`                     | Imagen de contenedor con la configuración regional `ar-AE`. |
| `ar-eg`                     | Imagen de contenedor con la configuración regional `ar-EG`. |
| `ar-kw`                     | Imagen de contenedor con la configuración regional `ar-KW`. |
| `ar-qa`                     | Imagen de contenedor con la configuración regional `ar-QA`. |
| `ar-sa`                     | Imagen de contenedor con la configuración regional `ar-SA`. |
| `ca-es`                     | Imagen de contenedor con la configuración regional `ca-ES`. |
| `da-dk`                     | Imagen de contenedor con la configuración regional `da-DK`. |
| `de-de`                     | Imagen de contenedor con la configuración regional `de-DE`. |
| `en-au`                     | Imagen de contenedor con la configuración regional `en-AU`. |
| `en-ca`                     | Imagen de contenedor con la configuración regional `en-CA`. |
| `en-gb`                     | Imagen de contenedor con la configuración regional `en-GB`. |
| `en-in`                     | Imagen de contenedor con la configuración regional `en-IN`. |
| `en-nz`                     | Imagen de contenedor con la configuración regional `en-NZ`. |
| `en-us`                     | Imagen de contenedor con la configuración regional `en-US`. |
| `es-es`                     | Imagen de contenedor con la configuración regional `es-ES`. |
| `es-mx`                     | Imagen de contenedor con la configuración regional `es-MX`. |
| `fi-fi`                     | Imagen de contenedor con la configuración regional `fi-FI`. |
| `fr-ca`                     | Imagen de contenedor con la configuración regional `fr-CA`. |
| `fr-fr`                     | Imagen de contenedor con la configuración regional `fr-FR`. |
| `gu-in`                     | Imagen de contenedor con la configuración regional `gu-IN`. |
| `hi-in`                     | Imagen de contenedor con la configuración regional `hi-IN`. |
| `it-it`                     | Imagen de contenedor con la configuración regional `it-IT`. |
| `ja-jp`                     | Imagen de contenedor con la configuración regional `ja-JP`. |
| `ko-kr`                     | Imagen de contenedor con la configuración regional `ko-KR`. |
| `mr-in`                     | Imagen de contenedor con la configuración regional `mr-IN`. |
| `nb-no`                     | Imagen de contenedor con la configuración regional `nb-NO`. |
| `nl-nl`                     | Imagen de contenedor con la configuración regional `nl-NL`. |
| `pl-pl`                     | Imagen de contenedor con la configuración regional `pl-PL`. |
| `pt-br`                     | Imagen de contenedor con la configuración regional `pt-BR`. |
| `pt-pt`                     | Imagen de contenedor con la configuración regional `pt-PT`. |
| `ru-ru`                     | Imagen de contenedor con la configuración regional `ru-RU`. |
| `sv-se`                     | Imagen de contenedor con la configuración regional `sv-SE`. |
| `ta-in`                     | Imagen de contenedor con la configuración regional `ta-IN`. |
| `te-in`                     | Imagen de contenedor con la configuración regional `te-IN`. |
| `th-th`                     | Imagen de contenedor con la configuración regional `th-TH`. |
| `tr-tr`                     | Imagen de contenedor con la configuración regional `tr-TR`. |
| `zh-cn`                     | Imagen de contenedor con la configuración regional `zh-CN`. |
| `zh-hk`                     | Imagen de contenedor con la configuración regional `zh-HK`. |
| `zh-tw`                     | Imagen de contenedor con la configuración regional `zh-TW`. |

---

## <a name="text-to-speech"></a>Texto a voz

La imagen de contenedor [Text-to-speech][sp-tts] se puede encontrar en la distribución del registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/speechservices/` y se denomina `text-to-speech`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/speechservices/text-to-speech`.

Esta imagen de contenedor tiene disponibles las siguientes etiquetas. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/text-to-speech/tags/list).


# <a name="latest-version"></a>[La versión más reciente](#tab/current)

Nota de la versión `1.8.0-amd64-<locale-and-voice>`:

**Característica**
* Completamente migrado a .NET 3.1

| Etiquetas de imagen                                  | Notas                                                                                                         |
|---------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `latest`                                    | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-AriaRUS`.                                            | 
| `1.8.0-amd64-<locale-and-voice>`            | Sustituya `<locale>` por una de las configuraciones regionales disponibles que se muestran a continuación. Por ejemplo, `1.8.0-amd64-en-us-ariarus`.  |


| Configuraciones regionales para v1.8.0                          | Notas                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `ar-eg-hoda`                                | Imagen de contenedor con la configuración regional `ar-EG` y voz `ar-EG-Hoda`.            |
| `ar-sa-naayf`                               | Imagen de contenedor con la configuración regional `ar-SA` y voz `ar-SA-Naayf`.           |
| `bg-bg-ivan`                                | Imagen de contenedor con la configuración regional `bg-BG` y voz `bg-BG-Ivan`.            |
| `ca-es-herenarus`                           | Imagen de contenedor con la configuración regional `ca-ES` y voz `ca-ES-HerenaRUS`.       |
| `cs-cz-jakub`                               | Imagen de contenedor con la configuración regional `cs-CZ` y voz `cs-CZ-Jakub`.           |
| `da-dk-hellerus`                            | Imagen de contenedor con la configuración regional `da-DK` y voz `da-DK-HelleRUS`.        |
| `de-at-michael`                             | Imagen de contenedor con la configuración regional `de-AT` y voz `de-AT-Michael`.         |
| `de-ch-karsten`                             | Imagen de contenedor con la configuración regional `de-CH` y voz `de-CH-Karsten`.         |
| `de-de-hedda`                               | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-Hedda`.           |
| `de-de-heddarus`                            | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-Hedda`.           |
| `de-de-stefan-apollo`                       | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-Stefan-Apollo`.   |
| `el-gr-stefanos`                            | Imagen de contenedor con la configuración regional `el-GR` y voz `el-GR-Stefanos`.        |
| `en-au-catherine`                           | Imagen de contenedor con la configuración regional `en-AU` y voz `en-AU-Catherine`.       |
| `en-au-hayleyrus`                           | Imagen de contenedor con la configuración regional `en-AU` y voz `en-AU-HayleyRUS`.       |
| `en-ca-heatherrus`                          | Imagen de contenedor con la configuración regional `en-CA` y voz `en-CA-HeatherRUS`.      |
| `en-ca-linda`                               | Imagen de contenedor con la configuración regional `en-CA` y voz `en-CA-Linda`.           |
| `en-gb-george-apollo`                       | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-George-Apollo`.   |
| `en-gb-hazelrus`                            | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-HazelRUS`.        |
| `en-gb-susan-apollo`                        | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-Susan-Apollo`.    |
| `en-ie-sean`                                | Imagen de contenedor con la configuración regional `en-IE` y voz `en-IE-Sean`.            |
| `en-in-heera-apollo`                        | Imagen de contenedor con la configuración regional `en-IN` y voz `en-IN-Heera-Apollo`.    |
| `en-in-priyarus`                            | Imagen de contenedor con la configuración regional `en-IN` y voz `en-IN-PriyaRUS`.        |
| `en-in-ravi-apollo`                         | Imagen de contenedor con la configuración regional `en-IN` y voz `en-IN-Ravi-Apollo`.     |
| `en-us-benjaminrus`                         | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-BenjaminRUS`.     |
| `en-us-guy24krus`                           | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-Guy24kRUS`.       |
| `en-us-aria24krus`                          | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-Aria24kRUS`.      |
| `en-us-ariarus`                             | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-AriaRUS`.         |
| `en-us-zirarus`                             | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-ZiraRUS`.         |
| `es-es-helenarus`                           | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-HelenaRUS`.       |
| `es-es-laura-apollo`                        | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-Laura-Apollo`.    |
| `es-es-pablo-apollo`                        | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-Pablo-Apollo`.    |
| `es-mx-hildarus`                            | Imagen de contenedor con la configuración regional `es-MX` y voz `es-MX-HildaRUS`.        |
| `es-mx-raul-apollo`                         | Imagen de contenedor con la configuración regional `es-MX` y voz `es-MX-Raul-Apollo`.     |
| `fi-fi-heidirus`                            | Imagen de contenedor con la configuración regional `fi-FI` y voz `fi-FI-HeidiRUS`.        |
| `fr-ca-caroline`                            | Imagen de contenedor con la configuración regional `fr-CA` y voz `fr-CA-Caroline`.        |
| `fr-ca-harmonierus`                         | Imagen de contenedor con la configuración regional `fr-CA` y voz `fr-CA-HarmonieRUS`.     |
| `fr-ch-guillaume`                           | Imagen de contenedor con la configuración regional `fr-CH` y voz `fr-CH-Guillaume`.       |
| `fr-fr-hortenserus`                         | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-HortenseRUS`.     |
| `fr-fr-julie-apollo`                        | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-Julie-Apollo`.    |
| `fr-fr-paul-apollo`                         | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-Paul-Apollo`.     |
| `he-il-asaf`                                | Imagen de contenedor con la configuración regional `he-IL` y voz `he-IL-Asaf`.            |
| `hi-in-hemant`                              | Imagen de contenedor con la configuración regional `hi-IN` y voz `hi-IN-Hemant`.          |
| `hi-in-kalpana-apollo`                      | Imagen de contenedor con la configuración regional `hi-IN` y voz `hi-IN-Kalpana-Apollo`.  |
| `hi-in-kalpana`                             | Imagen de contenedor con la configuración regional `hi-IN` y voz `hi-IN-Kalpana`.         |
| `hr-hr-matej`                               | Imagen de contenedor con la configuración regional `hr-HR` y voz `hr-HR-Matej`.           |
| `hu-hu-szabolcs`                            | Imagen de contenedor con la configuración regional `hu-HU` y voz `hu-HU-Szabolcs`.        |
| `id-id-andika`                              | Imagen de contenedor con la configuración regional `id-ID` y voz `id-ID-Andika`.          |
| `it-it-cosimo-apollo`                       | Imagen de contenedor con la configuración regional `it-IT` y voz `it-IT-Cosimo-Apollo`.   |
| `it-it-luciarus`                            | Imagen de contenedor con la configuración regional `it-IT` y voz `it-IT-LuciaRUS`.        |
| `ja-jp-ayumi-apollo`                        | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-Ayumi-Apollo`.    |
| `ja-jp-harukarus`                           | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-HarukaRUS`.       |
| `ja-jp-ichiro-apollo`                       | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-Ichiro-Apollo`.   |
| `ko-kr-heamirus`                            | Imagen de contenedor con la configuración regional `ko-KR` y voz `ko-KR-HeamiRUS`.        |
| `ms-my-rizwan`                              | Imagen de contenedor con la configuración regional `ms-MY` y voz `ms-MY-Rizwan`.          |
| `nb-no-huldarus`                            | Imagen de contenedor con la configuración regional `nb-NO` y voz `nb-NO-HuldaRUS`.        |
| `nl-nl-hannarus`                            | Imagen de contenedor con la configuración regional `nl-NL` y voz `nl-NL-HannaRUS`.        |
| `pl-pl-paulinarus`                          | Imagen de contenedor con la configuración regional `pl-PL` y voz `pl-PL-PaulinaRUS`.      |
| `pt-br-daniel-apollo`                       | Imagen de contenedor con la configuración regional `pt-BR` y voz `pt-BR-Daniel-Apollo`.   |
| `pt-br-heloisarus`                          | Imagen de contenedor con la configuración regional `pt-BR` y voz `pt-BR-HeloisaRUS`.      |
| `pt-pt-heliarus`                            | Imagen de contenedor con la configuración regional `pt-PT` y voz `pt-PT-HeliaRUS`.        |
| `ro-ro-andrei`                              | Imagen de contenedor con la configuración regional `ro-RO` y voz `ro-RO-Andrei`.          |
| `ru-ru-ekaterinarus`                        | Imagen de contenedor con la configuración regional `ru-RU` y voz `ru-RU-EkaterinaRUS`.    |
| `ru-ru-irina-apollo`                        | Imagen de contenedor con la configuración regional `ru-RU` y voz `ru-RU-Irina-Apollo`.    |
| `ru-ru-pavel-apollo`                        | Imagen de contenedor con la configuración regional `ru-RU` y voz `ru-RU-Pavel-Apollo`.    |
| `sk-sk-filip`                               | Imagen de contenedor con la configuración regional `sk-SK` y voz `sk-SK-Filip`.           |
| `sl-si-lado`                                | Imagen de contenedor con la configuración regional `sl-SI` y voz `sl-SI-Lado`.            |
| `sv-se-hedvigrus`                           | Imagen de contenedor con la configuración regional `sv-SE` y voz `sv-SE-HedvigRUS`.       |
| `ta-in-valluvar`                            | Imagen de contenedor con la configuración regional `ta-IN` y voz `ta-IN-Valluvar`.        |
| `te-in-chitra`                              | Imagen de contenedor con la configuración regional `te-IN` y voz `te-IN-Chitra`.          |
| `th-th-pattara`                             | Imagen de contenedor con la configuración regional `th-TH` y voz `th-TH-Pattara`.         |
| `tr-tr-sedarus`                             | Imagen de contenedor con la configuración regional `tr-TR` y voz `tr-TR-SedaRUS`.         |
| `vi-vn-an`                                  | Imagen de contenedor con la configuración regional `vi-VN` y voz `vi-VN-An`.              |
| `zh-cn-huihuirus`                           | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-HuihuiRUS`.       |
| `zh-cn-kangkang-apollo`                     | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-Kangkang-Apollo`. |
| `zh-cn-yaoyao-apollo`                       | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-Yaoyao-Apollo`.   |
| `zh-hk-danny-apollo`                        | Imagen de contenedor con la configuración regional `zh-HK` y voz `zh-HK-Danny-Apollo`.    |
| `zh-hk-tracy-apollo`                        | Imagen de contenedor con la configuración regional `zh-HK` y voz `zh-HK-Tracy-Apollo`.    |
| `zh-hk-tracyrus`                            | Imagen de contenedor con la configuración regional `zh-HK` y voz `zh-HK-TracyRUS`.        |
| `zh-tw-hanhanrus`                           | Imagen de contenedor con la configuración regional `zh-TW` y voz `zh-TW-HanHanRUS`.       |
| `zh-tw-yating-apollo`                       | Imagen de contenedor con la configuración regional `zh-TW` y voz `zh-TW-Yating-Apollo`.   |
| `zh-tw-zhiwei-apollo`                       | Imagen de contenedor con la configuración regional `zh-TW` y voz `zh-TW-Zhiwei-Apollo`.   |


# <a name="previous-version"></a>[Versión anterior](#tab/previous)

Nota de la versión `1.7.0-amd64-<locale-and-voice>`:

**Característica**
* Componentes actualizados a .NET 3.1

| Etiquetas de imagen                                  | Notas                                                                                                         |
|---------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `1.7.0-amd64-<locale-and-voice>`            | Primera versión con disponibilidad general. Sustituya `<locale>` por una de las configuraciones regionales disponibles que se muestran a continuación. Por ejemplo, `1.7.0-amd64-en-us-ariarus`.  |


| Configuraciones regionales para v1.7.0                          | Notas                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `ar-eg-hoda`                                | Imagen de contenedor con la configuración regional `ar-EG` y voz `ar-EG-Hoda`.            |
| `ar-sa-naayf`                               | Imagen de contenedor con la configuración regional `ar-SA` y voz `ar-SA-Naayf`.           |
| `bg-bg-ivan`                                | Imagen de contenedor con la configuración regional `bg-BG` y voz `bg-BG-Ivan`.            |
| `ca-es-herenarus`                           | Imagen de contenedor con la configuración regional `ca-ES` y voz `ca-ES-HerenaRUS`.       |
| `cs-cz-jakub`                               | Imagen de contenedor con la configuración regional `cs-CZ` y voz `cs-CZ-Jakub`.           |
| `da-dk-hellerus`                            | Imagen de contenedor con la configuración regional `da-DK` y voz `da-DK-HelleRUS`.        |
| `de-at-michael`                             | Imagen de contenedor con la configuración regional `de-AT` y voz `de-AT-Michael`.         |
| `de-ch-karsten`                             | Imagen de contenedor con la configuración regional `de-CH` y voz `de-CH-Karsten`.         |
| `de-de-hedda`                               | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-Hedda`.           |
| `de-de-heddarus`                            | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-Hedda`.           |
| `de-de-stefan-apollo`                       | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-Stefan-Apollo`.   |
| `el-gr-stefanos`                            | Imagen de contenedor con la configuración regional `el-GR` y voz `el-GR-Stefanos`.        |
| `en-au-catherine`                           | Imagen de contenedor con la configuración regional `en-AU` y voz `en-AU-Catherine`.       |
| `en-au-hayleyrus`                           | Imagen de contenedor con la configuración regional `en-AU` y voz `en-AU-HayleyRUS`.       |
| `en-ca-heatherrus`                          | Imagen de contenedor con la configuración regional `en-CA` y voz `en-CA-HeatherRUS`.      |
| `en-ca-linda`                               | Imagen de contenedor con la configuración regional `en-CA` y voz `en-CA-Linda`.           |
| `en-gb-george-apollo`                       | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-George-Apollo`.   |
| `en-gb-hazelrus`                            | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-HazelRUS`.        |
| `en-gb-susan-apollo`                        | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-Susan-Apollo`.    |
| `en-ie-sean`                                | Imagen de contenedor con la configuración regional `en-IE` y voz `en-IE-Sean`.            |
| `en-in-heera-apollo`                        | Imagen de contenedor con la configuración regional `en-IN` y voz `en-IN-Heera-Apollo`.    |
| `en-in-priyarus`                            | Imagen de contenedor con la configuración regional `en-IN` y voz `en-IN-PriyaRUS`.        |
| `en-in-ravi-apollo`                         | Imagen de contenedor con la configuración regional `en-IN` y voz `en-IN-Ravi-Apollo`.     |
| `en-us-benjaminrus`                         | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-BenjaminRUS`.     |
| `en-us-guy24krus`                           | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-Guy24kRUS`.       |
| `en-us-aria24krus`                          | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-Aria24kRUS`.      |
| `en-us-ariarus`                             | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-AriaRUS`.         |
| `en-us-zirarus`                             | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-ZiraRUS`.         |
| `es-es-helenarus`                           | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-HelenaRUS`.       |
| `es-es-laura-apollo`                        | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-Laura-Apollo`.    |
| `es-es-pablo-apollo`                        | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-Pablo-Apollo`.    |
| `es-mx-hildarus`                            | Imagen de contenedor con la configuración regional `es-MX` y voz `es-MX-HildaRUS`.        |
| `es-mx-raul-apollo`                         | Imagen de contenedor con la configuración regional `es-MX` y voz `es-MX-Raul-Apollo`.     |
| `fi-fi-heidirus`                            | Imagen de contenedor con la configuración regional `fi-FI` y voz `fi-FI-HeidiRUS`.        |
| `fr-ca-caroline`                            | Imagen de contenedor con la configuración regional `fr-CA` y voz `fr-CA-Caroline`.        |
| `fr-ca-harmonierus`                         | Imagen de contenedor con la configuración regional `fr-CA` y voz `fr-CA-HarmonieRUS`.     |
| `fr-ch-guillaume`                           | Imagen de contenedor con la configuración regional `fr-CH` y voz `fr-CH-Guillaume`.       |
| `fr-fr-hortenserus`                         | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-HortenseRUS`.     |
| `fr-fr-julie-apollo`                        | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-Julie-Apollo`.    |
| `fr-fr-paul-apollo`                         | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-Paul-Apollo`.     |
| `he-il-asaf`                                | Imagen de contenedor con la configuración regional `he-IL` y voz `he-IL-Asaf`.            |
| `hi-in-hemant`                              | Imagen de contenedor con la configuración regional `hi-IN` y voz `hi-IN-Hemant`.          |
| `hi-in-kalpana-apollo`                      | Imagen de contenedor con la configuración regional `hi-IN` y voz `hi-IN-Kalpana-Apollo`.  |
| `hi-in-kalpana`                             | Imagen de contenedor con la configuración regional `hi-IN` y voz `hi-IN-Kalpana`.         |
| `hr-hr-matej`                               | Imagen de contenedor con la configuración regional `hr-HR` y voz `hr-HR-Matej`.           |
| `hu-hu-szabolcs`                            | Imagen de contenedor con la configuración regional `hu-HU` y voz `hu-HU-Szabolcs`.        |
| `id-id-andika`                              | Imagen de contenedor con la configuración regional `id-ID` y voz `id-ID-Andika`.          |
| `it-it-cosimo-apollo`                       | Imagen de contenedor con la configuración regional `it-IT` y voz `it-IT-Cosimo-Apollo`.   |
| `it-it-luciarus`                            | Imagen de contenedor con la configuración regional `it-IT` y voz `it-IT-LuciaRUS`.        |
| `ja-jp-ayumi-apollo`                        | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-Ayumi-Apollo`.    |
| `ja-jp-harukarus`                           | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-HarukaRUS`.       |
| `ja-jp-ichiro-apollo`                       | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-Ichiro-Apollo`.   |
| `ko-kr-heamirus`                            | Imagen de contenedor con la configuración regional `ko-KR` y voz `ko-KR-HeamiRUS`.        |
| `ms-my-rizwan`                              | Imagen de contenedor con la configuración regional `ms-MY` y voz `ms-MY-Rizwan`.          |
| `nb-no-huldarus`                            | Imagen de contenedor con la configuración regional `nb-NO` y voz `nb-NO-HuldaRUS`.        |
| `nl-nl-hannarus`                            | Imagen de contenedor con la configuración regional `nl-NL` y voz `nl-NL-HannaRUS`.        |
| `pl-pl-paulinarus`                          | Imagen de contenedor con la configuración regional `pl-PL` y voz `pl-PL-PaulinaRUS`.      |
| `pt-br-daniel-apollo`                       | Imagen de contenedor con la configuración regional `pt-BR` y voz `pt-BR-Daniel-Apollo`.   |
| `pt-br-heloisarus`                          | Imagen de contenedor con la configuración regional `pt-BR` y voz `pt-BR-HeloisaRUS`.      |
| `pt-pt-heliarus`                            | Imagen de contenedor con la configuración regional `pt-PT` y voz `pt-PT-HeliaRUS`.        |
| `ro-ro-andrei`                              | Imagen de contenedor con la configuración regional `ro-RO` y voz `ro-RO-Andrei`.          |
| `ru-ru-ekaterinarus`                        | Imagen de contenedor con la configuración regional `ru-RU` y voz `ru-RU-EkaterinaRUS`.    |
| `ru-ru-irina-apollo`                        | Imagen de contenedor con la configuración regional `ru-RU` y voz `ru-RU-Irina-Apollo`.    |
| `ru-ru-pavel-apollo`                        | Imagen de contenedor con la configuración regional `ru-RU` y voz `ru-RU-Pavel-Apollo`.    |
| `sk-sk-filip`                               | Imagen de contenedor con la configuración regional `sk-SK` y voz `sk-SK-Filip`.           |
| `sl-si-lado`                                | Imagen de contenedor con la configuración regional `sl-SI` y voz `sl-SI-Lado`.            |
| `sv-se-hedvigrus`                           | Imagen de contenedor con la configuración regional `sv-SE` y voz `sv-SE-HedvigRUS`.       |
| `ta-in-valluvar`                            | Imagen de contenedor con la configuración regional `ta-IN` y voz `ta-IN-Valluvar`.        |
| `te-in-chitra`                              | Imagen de contenedor con la configuración regional `te-IN` y voz `te-IN-Chitra`.          |
| `th-th-pattara`                             | Imagen de contenedor con la configuración regional `th-TH` y voz `th-TH-Pattara`.         |
| `tr-tr-sedarus`                             | Imagen de contenedor con la configuración regional `tr-TR` y voz `tr-TR-SedaRUS`.         |
| `vi-vn-an`                                  | Imagen de contenedor con la configuración regional `vi-VN` y voz `vi-VN-An`.              |
| `zh-cn-huihuirus`                           | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-HuihuiRUS`.       |
| `zh-cn-kangkang-apollo`                     | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-Kangkang-Apollo`. |
| `zh-cn-yaoyao-apollo`                       | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-Yaoyao-Apollo`.   |
| `zh-hk-danny-apollo`                        | Imagen de contenedor con la configuración regional `zh-HK` y voz `zh-HK-Danny-Apollo`.    |
| `zh-hk-tracy-apollo`                        | Imagen de contenedor con la configuración regional `zh-HK` y voz `zh-HK-Tracy-Apollo`.    |
| `zh-hk-tracyrus`                            | Imagen de contenedor con la configuración regional `zh-HK` y voz `zh-HK-TracyRUS`.        |
| `zh-tw-hanhanrus`                           | Imagen de contenedor con la configuración regional `zh-TW` y voz `zh-TW-HanHanRUS`.       |
| `zh-tw-yating-apollo`                       | Imagen de contenedor con la configuración regional `zh-TW` y voz `zh-TW-Yating-Apollo`.   |
| `zh-tw-zhiwei-apollo`                       | Imagen de contenedor con la configuración regional `zh-TW` y voz `zh-TW-Zhiwei-Apollo`.   |

---

## <a name="neural-text-to-speech"></a>Texto a voz neuronal

La imagen de contenedor [Neural Text-to-speech][sp-ntts] se puede encontrar en la distribución del registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/speechservices/` y se denomina `neural-text-to-speech`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/speechservices/neural-text-to-speech`.

Esta imagen de contenedor tiene disponibles las siguientes etiquetas. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/neural-text-to-speech/tags/list).

| Etiquetas de imagen                                  | Notas                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-AriaNeural`.      |
| `1.2.0-amd64-<locale-and-voice>-preview`    | Sustituya `<locale>` por una de las configuraciones regionales disponibles que se muestran a continuación. Por ejemplo, `1.2.0-amd64-en-us-arianeural-preview`. |


| Configuraciones regionales y voces en la versión preliminar de v1.2.0           | Notas                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-AriaNeural`.      |
| `de-de-katjaneural-preview`                 | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-KatjaNeural`.     |
| `en-au-natashaneural-preview`               | Imagen de contenedor con la configuración regional `en-AU` y voz `en-AU-NatashaNeural`.   |
| `en-ca-claraneural-preview`                 | Imagen de contenedor con la configuración regional `en-CA` y voz `en-CA-ClaraNeural`.     |
| `en-gb-libbyneural-preview`                 | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-LibbyNeural`.     |
| `en-gb-mianeural-preview`                   | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-MiaNeural`.       |
| `en-us-arianeural-preview`                  | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-AriaNeural`.      |
| `en-us-guyneural-preview`                   | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-GuyNeural`.       |
| `es-es-elviraneural-preview`                | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-ElviraNeural`.    |
| `es-mx-dalianeural-preview`                 | Imagen de contenedor con la configuración regional `es-MX` y voz `es-MX-DaliaNeural`.     |
| `fr-ca-sylvieneural-preview`                | Imagen de contenedor con la configuración regional `fr-CA` y voz `fr-CA-SylvieNeural`.    |
| `fr-fr-deniseneural-preview`                | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-DeniseNeural`.    |
| `it-it-elsaneural-preview`                  | Imagen de contenedor con la configuración regional `it-IT` y voz `it-IT-ElsaNeural`.      |
| `ja-jp-nanamineural-preview`                | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-NanamiNeural`.    |
| `ko-kr-sunhineural-preview`                 | Imagen de contenedor con la configuración regional `ko-KR` y voz `ko-KR-SunHiNeural`.     |
| `pt-br-franciscaneural-preview`             | Imagen de contenedor con la configuración regional `pt-BR` y voz `pt-BR-FranciscaNeural`. |
| `zh-cn-xiaoxiaoneural-preview`              | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-XiaoxiaoNeural`.  |

## <a name="speech-language-detection"></a>Detección de idioma de Voz

La imagen de contenedor [Speech language detection][sp-lid] se puede encontrar en la distribución del registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/speechservices/` y se denomina `language-detection`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/speechservices/language-detection`.

Esta imagen de contenedor tiene disponibles las siguientes etiquetas. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/language-detection/tags/list).

| Etiquetas de imagen                                  | Notas                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                       |      |
| `1.1.0-amd64-preview`                       |      |

## <a name="key-phrase-extraction"></a>Extracción de frases clave

se puede encontrar en la distribución del registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/textanalytics/` y se denomina `keyphrase`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/textanalytics/keyphrase`.

Esta imagen de contenedor tiene disponibles las siguientes etiquetas. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/keyphrase/tags/list).

# <a name="latest-version"></a>[La versión más reciente](#tab/current)


| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013570001-amd64` |       |

# <a name="previous-versions"></a>[Versiones anteriores](#tab/previous)

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `1.1.012840001-amd64` |       |
| `1.1.012830001-amd64`    |       |

---

## <a name="text-language-detection"></a>Detección de idioma del texto

La imagen de contenedor [Detección de idioma][ta-la] se puede encontrar en el sindicato de registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/textanalytics/` y se denomina `language`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/textanalytics/language`.


Esta imagen de contenedor tiene disponibles las siguientes etiquetas. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/language/tags/list).

# <a name="latest-versions"></a>[Últimas versiones](#tab/current)

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013570001-amd64` | |
   

# <a name="previous-versions"></a>[Versiones anteriores](#tab/previous)


| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.012840001-amd64` |   |
| `1.1.012830001-amd64` |   |

---

## <a name="sentiment-analysis"></a>análisis de opiniones

La imagen de contenedor [Análisis de sentimiento][ta-se] se puede encontrar en el sindicato de registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/textanalytics/` y se denomina `sentiment`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment`.

Esta imagen de contenedor tiene disponibles las siguientes etiquetas. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/sentiment/tags/list).

| Etiquetas de imagen | Notas                                         |
|------------|:----------------------------------------------|
| `latest`   |                                               |
| `3.0-en`   | Análisis de sentimiento, versión 3 (inglés)               |
| `3.0-es`   | Análisis de sentimiento, versión 3 (español)               |
| `3.0-fr`   | Análisis de sentimiento, versión 3 (francés)                |
| `3.0-it`   | Análisis de sentimiento, versión 3 (italiano)               |
| `3.0-de`   | Análisis de sentimiento, versión 3 (alemán)                |
| `3.0-zh`   | Análisis de sentimiento, versión 3 (chino simplificado)  |
| `3.0-zht`  | Análisis de sentimiento, versión 3 (chino tradicional) |
| `3.0-ja`   | Análisis de sentimiento, versión 3 (japonés)              |
| `3.0-pt`   | Análisis de sentimiento, versión 3 (portugués)            |
| `3.0-nl`   | Análisis de sentimiento, versión 3 (holandés)                 |
| `2.1`    | Análisis de sentimiento, versión 2      |

[ad-containers]: ../anomaly-Detector/anomaly-detector-container-howto.md
[cv-containers]: ../computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: ../face/face-how-to-install-containers.md
[fr-containers]: ../form-recognizer/form-recognizer-container-howto.md
[lu-containers]: ../luis/luis-container-howto.md
[sp-stt]: ../speech-service/speech-container-howto.md?tabs=stt
[sp-cstt]: ../speech-service/speech-container-howto.md?tabs=cstt
[sp-tts]: ../speech-service/speech-container-howto.md?tabs=tts
[sp-ctts]: ../speech-service/speech-container-howto.md?tabs=ctts
[sp-ntts]: ../speech-service/speech-container-howto.md?tabs=ntts
[sp-lid]: ../speech-service/speech-container-howto.md?tabs=lid
[ta-kp]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=keyphrase
[ta-la]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=language
[ta-se]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=sentiment
