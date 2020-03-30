---
title: 'Reconocimiento de texto manuscrito e impreso: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con el reconocimiento del texto manuscrito e impreso en imágenes mediante Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: a4c90ed12c8023e0b9ebc509b20d8d9224b49f1b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218866"
---
# <a name="recognize-printed-and-handwritten-text"></a>Reconocimiento de texto manuscrito e impreso

Computer Vision proporciona varios servicios que detectan y extraen texto manuscrito o impreso que aparece en las imágenes. Esto es útil en una variedad de escenarios, como la toma de notas, los registros médicos, seguridad y banca. Las tres secciones siguientes detallan tres API de reconocimiento de texto diferentes, cada una optimizada para distintos casos de uso.

## <a name="read-api"></a>Read API

Read API detecta el contenido de texto de una imagen con los últimos modelos de reconocimiento y convierte el texto identificado en una secuencia de caracteres legible por una máquina. Está optimizada para imágenes con gran cantidad de texto (por ejemplo, documentos que han sido digitalizados) y para imágenes con mucho ruido visual. Determinará qué modelo de reconocimiento se usará para cada línea de texto y admite imágenes con texto escrito a mano e impreso. Read API se ejecuta de forma asincrónica porque los documentos más grandes pueden tardar varios minutos en devolver un resultado.

La operación de lectura mantiene las agrupaciones de líneas originales de palabras reconocidas en la salida. Cada línea incluye las coordenadas del rectángulo delimitador y cada palabra dentro de la línea también tiene sus propias coordenadas. Si una palabra se ha reconocido con confianza baja, dicha información se muestra también. Consulte los documentos de referencia de [Read API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/2afb498089f74080d7ef85eb) y [Read API Preview](https://go.microsoft.com/fwlink/?linkid=2118322) para obtener más información.

> [!NOTE]
> Esta característica solo está disponible para texto en inglés y español (versión preliminar).

### <a name="image-requirements"></a>Requisitos de imagen

Read API funciona con imágenes que cumplan los requisitos siguientes:

- La imagen debe estar en formato JPEG, PNG, BMP, PDF o TIFF.
- El tamaño de la imagen debe estar entre 50 x 50 y 10 000 x 10 000 píxeles. Las páginas PDF deben ser de 17 x 17 pulgadas o más pequeñas.
- El tamaño del archivo de la imagen debe ser menor de 20 megabytes (MB).

### <a name="limitations"></a>Limitaciones

Si usa una suscripción gratuita, Read API solo procesará las dos primeras páginas de un documento PDF o TIFF. Con una suscripción de pago, procesará hasta 200 páginas. Tenga en cuenta también que la API detectará un máximo de 300 líneas por página.

## <a name="ocr-optical-character-recognition-api"></a>OCR API (reconocimiento óptico de caracteres)

La API de reconocimiento óptico de caracteres (OCR) de Computer Vision es similar a Read API, pero se ejecuta de forma sincrónica y no está optimizada para documentos de gran tamaño. Usa un modelo de reconocimiento anterior, pero funciona con más idiomas; consulte [Idiomas admitidos](language-support.md#text-recognition) para obtener una lista completa de los idiomas admitidos.

Si es necesario, OCR corrige el giro del texto reconocido devolviendo el desplazamiento de rotación en grados alrededor del eje horizontal de la imagen. OCR también proporciona las coordenadas del marco de cada palabra, como se observa en la ilustración siguiente.

![Una imagen que se gira y su texto siendo leído y definido](./Images/vision-overview-ocr.png)

Consulte los [documentos de referencia de OCR](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) para más información.

### <a name="image-requirements"></a>Requisitos de imagen

OCR API funciona con imágenes que cumplan los requisitos siguientes:

* La imagen debe estar en formato JPEG, PNG, GIF o BMP.
* El tamaño de la imagen de entrada debe estar entre 50 x 50 y 4200 x 4200 píxeles.
* El texto de la imagen se puede girar cualquier múltiplo de 90 grados, además de un pequeño ángulo de hasta 40 grados.

### <a name="limitations"></a>Limitaciones

En las fotografías en las que predomina el texto, las palabras que se reconocen de forma parcial pueden producir falsos positivos. En algunas fotografías, especialmente las que no tienen texto, la precisión puede variar según el tipo de imagen.

## <a name="recognize-text-api"></a>Recognize Text API

> [!NOTE]
> Recognize Text API está en desuso en favor de Read API. Read API tiene funcionalidades similares y se ha actualizado para trabajar con archivos de varias páginas, TIFF y PDF.

Recognize Text API es similar a OCR, pero se ejecuta de forma asincrónica y usa modelos de reconocimiento actualizados. Consulte los [documentos de referencia de Recognize Text API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) para más información.

### <a name="image-requirements"></a>Requisitos de imagen

Recognize Text API funciona con imágenes que cumplan los requisitos siguientes:

- La imagen debe estar en formato JPEG, PNG o BMP.
- El tamaño de la imagen debe estar entre 50 x 50 y 4200 x 4200 píxeles.
- El tamaño de archivo de la imagen debe ser inferior a 4 megabytes (MB).

## <a name="limitations"></a>Limitaciones

La precisión de las operaciones de reconocimiento de texto depende de la calidad de las imágenes. Los siguientes factores pueden provocar una lectura imprecisa:

* Imágenes borrosas.
* Texto escrito a mano o en cursiva.
* Estilos de fuente artísticos.
* Tamaño de texto pequeño.
* Fondos complejos, sombras, brillo sobre el texto o distorsión de la perspectiva.
* Letra mayúscula grande o falta de letras mayúsculas iniciales.
* Texto de superíndice, subíndice o tachado.

## <a name="next-steps"></a>Pasos siguientes

Siga la guía de inicio rápido sobre la [extracción de texto (Read)](./QuickStarts/CSharp-hand-text.md) para implementar el reconocimiento de texto en una sencilla aplicación de C#.
