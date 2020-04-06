---
title: Novedades en Form Recognizer
titleSuffix: Azure Cognitive Services
description: Comprenda los últimos cambios en la API de Form Recognizer.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 03/20/2020
ms.author: pafarley
ms.openlocfilehash: 7f20244906581dd2869bbc7fcd997d5245540eda
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80155178"
---
# <a name="whats-new-in-form-recognizer"></a>Novedades en Form Recognizer

El servicio Form Recognizer se actualiza de forma continuada. Use este artículo para mantenerse al día con mejoras de características, correcciones y actualizaciones de la documentación.

> [!NOTE]
> Los inicios rápidos y las guías de Form Recognizer usan siempre la versión más reciente de la API, a menos que se especifique lo contrario.

## <a name="march-2020"></a>Marzo de 2020 

### <a name="extraction-enhancements"></a>Mejoras de extracción

Esta versión incluye mejoras de extracción y mejoras de precisión, en concreto, la capacidad de etiquetar y extraer varios pares clave-valor en la misma línea de texto. 
 
### <a name="form-recognizer-sample-labeling-tool-is-now-open-source"></a>La herramienta de etiquetado de ejemplo de Form Recognizer ahora es de código abierto

La herramienta de etiquetado de ejemplo de Form Recognizer ahora está disponible como proyecto de código abierto. Puede integrarla en sus soluciones y realizar cambios específicos del cliente para satisfacer sus necesidades.

Para más información sobre la herramienta de etiquetado de ejemplo de Form Recognizer, revise la documentación disponible en [GitHub](https://github.com/microsoft/OCR-Form-Tools/blob/master/README.md).

### <a name="labeling-value-types"></a>Etiquetado de tipos de valor

Los tipos de valor ahora están disponibles para su uso con la herramienta de etiquetado de ejemplo de Form Recognizer. Actualmente se admiten estos tipos de valor: 

* String
* Number 
* Entero
* Date 
* Time

En esta imagen se muestra qué apariencia tiene la selección del tipo de valor en la herramienta de etiquetado de ejemplo de Form Recognizer:

> [!div class="mx-imgBorder"]
> ![Selección del tipo de valor con la herramienta de etiquetado de ejemplo](./media/whats-new/formre-value-type.png)

La tabla extraída está disponible en la salida JSON en `pageResults`.

### <a name="table-visualization"></a>Visualización de tablas 

La herramienta de etiquetado de Form Recognizer muestra ahora las tablas que se reconocieron en el documento. Esto le permite ver las tablas que se han reconocido y extraído del documento antes de etiquetarlas y analizarlas con la herramienta de etiquetado de ejemplo de Form Recognizer. Esta característica se puede activar o desactivar mediante la opción de capas. 

Este es un ejemplo de cómo se reconocen y extraen las tablas:

> [!div class="mx-imgBorder"]
> ![Visualización de tablas mediante la herramienta de etiquetado de ejemplo](./media/whats-new/formre-table-viz.png)

> [!IMPORTANT]
> No se admite el etiquetado de tablas. Si las tablas no se reconocen ni se extraen automáticamente, solo se pueden etiquetar como pares clave-valor. Al etiquetar tablas como pares clave-valor, etiquete cada celda como un valor.

### <a name="tls-12-enforcement"></a>Cumplimiento de TLS 1.2

* TLS 1.2 ya se exige en todas las solicitudes HTTP para este servicio. Para más información, consulte [Seguridad de Azure Cognitive Services](../cognitive-services-security.md).

## <a name="january-2020"></a>Enero de 2020

En esta versión se introduce Form Recognizer 2.0 (versión preliminar). En las secciones siguientes encontrará más información sobre nuevas características, mejoras y cambios. 

### <a name="new-features"></a>Nuevas características

* **Modelo personalizado**
  * **Entrenar con etiquetas** Ahora puede entrenar un modelo personalizado con datos etiquetados manualmente. Esto da como resultado modelos con un mejor rendimiento y puede generar modelos que funcionen con formularios complejos o formularios que contengan valores sin claves.
  * **API asincrónica** Puede usar llamadas API asincrónicas para entrenar y analizar grandes conjuntos de datos y archivos.
  * **Compatibilidad con archivos TIFF** Ahora puede entrenar y extraer datos de documentos TIFF.
  * **Mejoras en la precisión de la extracción**

* **Modelo de recibo pregenerado**
  * **Importes de propinas** Ahora puede extraer importes de propina y otros valores escritos a mano.
  * **Extracción de elementos de línea** Puede extraer valores de elementos de línea de recibos.
  * **Valores de confianza** Puede ver la confianza del modelo para cada valor extraído.
  * **Mejoras en la precisión de la extracción**

* **Extracción de diseño** Ahora puede usar la API de diseño para extraer datos de texto y datos de tabla de los formularios.

### <a name="custom-model-api-changes"></a>Cambios en la API del modelo personalizado

Se ha cambiado el nombre de todas las API de entrenamiento y uso de modelos personalizados y algunos métodos sincrónicos son asincrónicos ahora. A continuación se indican los cambios principales:

* El proceso de entrenamiento de un modelo es ahora asincrónico. Inicie el entrenamiento mediante la llamada API **/custom/models**. Esta llamada devuelve un identificador de operación, que se puede pasar a **custom/models/{modelID}** para devolver los resultados del entrenamiento.
* La extracción de clave y valor se inicia ahora mediante la llamada API **/custom/models/{modelID}/analyze**. Esta llamada devuelve un identificador de operación, que se puede pasar a **custom/models/{modelID}/analyzeResults/{resultID}** para devolver los resultados de la extracción.
* Los identificadores de operación de la operación de entrenamiento se encuentran ahora en el encabezado **Location** de las respuestas HTTP, no en el encabezado **Operation-Location**.

### <a name="receipt-api-changes"></a>Cambios en la API del recibo

Se ha cambiado el nombre de las API para leer recibos de venta.

* La extracción de datos de recibo ahora se inicia mediante la llamada API **/prebuilt/receipt/analyze**. Esta llamada devuelve un identificador de operación, que se puede pasar a **/prebuilt/receipt/analyzeResults/{resultID}** para devolver los resultados de la extracción.

### <a name="output-format-changes"></a>Cambios del formato de salida

Las respuestas JSON para todas las llamadas API tienen nuevos formatos. Algunas claves y valores se han agregado, quitado o cambiado de nombre. Consulte las guías de inicio rápido para obtener ejemplos de los formatos JSON actuales.

## <a name="next-steps"></a>Pasos siguientes

Realice un [inicio rápido](quickstarts/curl-train-extract.md) para empezar a usar las [API de Form Recognizer](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm).