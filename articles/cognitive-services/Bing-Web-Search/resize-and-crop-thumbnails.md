---
title: 'Cambiar el tamaño y recortar miniaturas de Bing: Bing Web Search API'
titleSuffix: Azure Cognitive Services
description: Algunas respuestas de las API de Bing Search incluyen URL a imágenes en miniatura que muestra Bing, que puede cambiar de tamaño y recortar, y que pueden contener parámetros de consulta.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: 05A08B01-89FF-4781-AFE7-08DA92F25047
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: aahi
ms.openlocfilehash: ac1b636f644784092b5f68f59d787c328478df69
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93096927"
---
# <a name="resize-and-crop-thumbnail-images"></a>Cambio de tamaño y recorte de miniaturas

> [!WARNING]
> Bing Search APIs se mueve de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](https://aka.ms/cogsvcs/bingmove).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](https://aka.ms/cogsvcs/bingmigration).

Algunas respuestas de las API de Bing Search incluyen URL a imágenes en miniatura que muestra Bing, que puede cambiar de tamaño y recortar, y que pueden contener parámetros de consulta. Por ejemplo:

`https://<host>/th?id=AMMS_92772df988...&w=110&h=73&rs=1&qlt=80&cdv=1&pid=16.1`

Si va a mostrar un subconjunto de miniaturas, proporcione una opción para visualizar las imágenes restantes.

> [!NOTE]
> Asegúrese de que, al recortar y cambiar el tamaño de las imágenes en miniatura, proporcionará un escenario de búsqueda que respeta los derechos de terceros, según lo especificado en los [requisitos de uso y visualización](use-display-requirements.md) de la API de Bing Search.

## <a name="resize-a-thumbnail"></a>Cambiar el tamaño de una miniatura 

Para cambiar el tamaño de una miniatura, Bing recomienda especificar solo uno de parámetros de consulta `w` (ancho) o `h` (alto) en la dirección URL de la miniatura. Al especificar solo el alto o el ancho, Bing mantiene el aspecto original de la imagen. Especifique el ancho y alto en píxeles. 

Por ejemplo, si la miniatura original tiene el tamaño 480 x 620:

`https://<host>/th?id=JN.5l3yzwy%2f%2fHj59U6XhssIQ&pid=Api&w=480&h=620`

Y quiere reducir su tamaño, establezca el parámetro `w` en un nuevo valor (por ejemplo, `336`) y quite el parámetro `h`:

`https://<host>/th?id=JN.5l3yzwy%2f%2fHj59U6XhssIQ&pid=Api&w=336`

Si especifica solo el alto o el ancho de una miniatura, se mantendrá la relación de aspecto original de la imagen. Si especifica ambos parámetros y no se mantiene la relación de aspecto, Bing agregará relleno en blanco al borde de la imagen.

Por ejemplo, si cambia el tamaño de una imagen de 480 x 359 a 200 x 200 sin recortar, el ancho total contendrá la imagen, pero el alto contendrá 25 píxeles de relleno blanco en la parte superior e inferior de la imagen. Si la imagen fuese de 359 x 480, los bordes izquierdo y derecho contendrían el relleno blanco. Si recorta la imagen, no se agrega relleno blanco.  

La siguiente imagen muestra el tamaño original de una imagen en miniatura (480 x 300).  
  
![Imagen original en horizontal](./media/resize-crop/bing-resize-crop-landscape.png)  
  
La siguiente imagen muestra la imagen cambiada a un tamaño de 200 x 200. La proporción de aspecto se mantiene y los bordes superior e inferior se rellenan de blanco (el borde negro se incluye aquí para mostrar el relleno).  
  
![Imagen con el tamaño cambiado en horizontal](./media/resize-crop/bing-resize-crop-landscape-resized.png)  

Si especifica dimensiones mayores que el ancho y alto de la imagen original, Bing agregará relleno blanco a los bordes izquierdo y superior.  

## <a name="request-different-thumbnail-sizes"></a>Solicitar tamaños de miniatura diferentes

Para solicitar un tamaño de imagen en miniatura diferente, quite todos los parámetros de consulta de la dirección URL de la miniatura, excepto los parámetros `id` y `pid`. A continuación, agregue el parámetro de consulta `&w` (ancho) o `&h` (alto) con el tamaño de imagen deseado en píxeles, pero no ambos. Bing mantendrá la relación de aspecto original de la imagen. 

Para aumentar el ancho de la imagen especificado por la dirección URL anterior a 165 píxeles, usaría la siguiente dirección URL:

`https://<host>/th?id=AMMS_92772df988...&w=165&pid=16.1`

Si solicita una imagen que supera el tamaño original, Bing agrega relleno blanco alrededor de la imagen según sea necesario. Por ejemplo, si el tamaño original de la imagen es 474 x 316 y establece `&w` en 500, Bing devolverá una imagen de 500 x 333. Esta imagen tendrá 8,5 píxeles de relleno blanco a lo largo de los bordes superior e inferior y 13 píxeles de relleno en los bordes izquierdo y derecho.

Para evitar que Bing agregue relleno blanco si el tamaño solicitado es mayor que el tamaño original de la imagen, establezca el parámetro de consulta `&p` en 0. Por ejemplo, si incluye el parámetro `&p=0` en la dirección URL anterior, Bing devolverá una imagen de 474 x 316, en lugar de una imagen de 500 x 333:

`https://<host>/th?id=AMMS_92772df988...&w=500&p=0&pid=16.1`

Si especifica los parámetros de consulta `&w` y `&h`, Bing mantendrá la relación de aspecto de la imagen y agregará relleno blanco según sea necesario. Por ejemplo, si el tamaño original de la imagen es 474 x 316 y establece los parámetros de ancho y alto en 200 x 200 (`&w=200&h=200`), Bing devuelve una imagen que contiene 33 píxeles de relleno en blanco en la parte superior e inferior. Si incluye el parámetro de consulta `&p`, Bing devuelve una imagen de 200 x 134.

## <a name="crop-a-thumbnail"></a>Recortar una miniatura 

Para recortar una imagen, incluya el parámetro de consulta `c` (recortar). Puede usar los valores siguientes:
  
- `4` &mdash; proporción oculta  
- `7` &mdash; proporción inteligente  

### <a name="smart-ratio-cropping"></a>Recorte de proporción inteligente

Si solicita un recorte de proporción inteligente (definiendo el parámetro `c` en `7`), Bing recortará una imagen desde el centro de la región de interés de la imagen hacia fuera manteniendo la relación de aspecto de la imagen. La región de interés de la imagen es el área de la imagen que Bing determina que contiene las partes de mayor importancia. A continuación se muestra un ejemplo de región de interés.  
  
![Región de interés](./media/resize-crop/bing-resize-crop-regionofinterest.png)

Si cambia el tamaño de una imagen y solicita el recorte de proporción inteligente, Bing reduce la imagen al tamaño solicitado y mantiene la relación de aspecto. A continuación, Bing recorta la imagen conforme a las nuevas dimensiones. Por ejemplo, si el nuevo ancho es igual o inferior al alto, Bing recortará la imagen por la izquierda y derecha del centro de la región de interés. En caso contrario, Bing la recortará por la parte superior e inferior del centro de la región de interés.  
  
 
A continuación se muestra la imagen que se reduce a 200 x 200 usando recortar con Smart Ratio (proporción inteligente). Dado que Bing mide la imagen desde la esquina superior izquierda, la parte inferior de la imagen se recorta. 
  
![Imagen en horizontal recortada a 200 x 200](./media/resize-crop/bing-resize-crop-landscape200x200c7.png) 
  
A continuación se muestra la imagen que se reduce a 200 x 100 usando recortar con Smart Ratio (proporción inteligente). Dado que Bing mide la imagen desde la esquina superior izquierda, la parte inferior de la imagen se recorta. 
   
![Imagen en horizontal recortada a 200 x 100](./media/resize-crop/bing-resize-crop-landscape200x100c7.png)
  
A continuación se muestra la imagen que se reduce a 100 x 200 usando recortar con Smart Ratio (proporción inteligente). Dado que Bing mide la imagen desde el centro, las partes izquierda y derecha de la imagen se recortan.
  
![Imagen en horizontal recortada a 100 x 200](./media/resize-crop/bing-resize-crop-landscape100x200c7.png) 

Si Bing no puede determinar la región de interés de la imagen, el servicio usará el recorte de proporción oculta.  

### <a name="blind-ratio-cropping"></a>Recorte de proporción oculta

Si solicita el recorte de proporción oculta (definiendo el parámetro `c` en `4`), Bing usa las reglas siguientes para recortar la imagen.  
  
- Si `(Original Image Width / Original Image Height) < (Requested Image Width / Requested Image Height)`, la imagen se mide desde la esquina superior izquierda y se recorta por la parte inferior.  
- Si `(Original Image Width / Original Image Height) > (Requested Image Width / Requested Image Height)`, la imagen se mide desde el centro y se recorta por la izquierda y la derecha.  

A continuación se muestra una imagen vertical de 225 x 300.  
  
![Imagen de girasol original](./media/resize-crop/bing-resize-crop-sunflower.png)
  
A continuación se muestra la imagen reducida a 200 x 200 usando recortar con Blind Ratio (proporción oculta). La imagen se mide desde la esquina superior izquierda y se recorta la parte inferior de la imagen.  
  
![Imagen de girasol recortada a 200 x 200](./media/resize-crop/bing-resize-crop-sunflower200x200c4.png)
  
A continuación se muestra la imagen reducida a 200 x 100 usando recortar con Blind Ratio (proporción oculta). La imagen se mide desde la esquina superior izquierda y se recorta la parte inferior de la imagen.  
  
![Imagen de girasol recortada a 200 x 100](./media/resize-crop/bing-resize-crop-sunflower200x100c4.png)
  
A continuación se muestra la imagen reducida a 100 x 200 usando recortar con Blind Ratio (proporción oculta). La imagen se mide desde el centro y se recortan las partes izquierda y derecha de la imagen.  
  
![Imagen de girasol recortada a 100 x 200](./media/resize-crop/bing-resize-crop-sunflower100x200c4.png)

## <a name="next-steps"></a>Pasos siguientes

* [¿Qué son las versiones de Bing Search API?](bing-api-comparison.md)
* [Requisitos de uso y visualización de Bing Search API](use-display-requirements.md)
