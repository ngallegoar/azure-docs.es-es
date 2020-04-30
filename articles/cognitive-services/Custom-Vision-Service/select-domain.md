---
title: 'Selección de un dominio para un proyecto de Custom Vision: Computer Vision'
titleSuffix: Azure Cognitive Services
description: En este artículo se mostrará cómo selecciona un dominio para un proyecto en el servicio Custom Vision.
services: cognitive-services
author: shonohs
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/06/2020
ms.author: shono
ms.openlocfilehash: 1fb30cc0634224213dc9a188a16902e07d379904
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82127775"
---
# <a name="select-a-domain-for-a-custom-vision-project"></a>Selección de un dominio para un proyecto de Custom Vision

En la hoja de configuración del proyecto de Custom Vision, puede seleccionar un dominio para el proyecto. Elija el dominio más cercano a su escenario.

## <a name="image-classification"></a>Clasificación de imágenes

|Domain|Propósito|
|---|---|
|__Genérico__| Optimizado para una amplia gama de tareas de clasificación de imágenes. Si ninguno de los otros dominios es adecuado o si no está seguro de qué dominio elegir, seleccione el dominio genérico.|
|__Alimentos__|Optimizado para fotos de platos tal y como los vería en el menú de un restaurante. Si quiere clasificar fotos de frutas o verduras individuales, use el dominio de alimentos.|
|__Puntos de referencia__|Optimizado para lugares de interés reconocibles, tanto naturales como artificiales. Este dominio funciona mejor cuando el punto de referencia es claramente visible en la foto. Este dominio funciona incluso si hay gente delante del punto de referencia que obstruye parcialmente su visión.|
|__Minoristas__|Optimizado para imágenes que se encuentran en un catálogo de compra o un sitio web de compras. Si quiere una clasificación de alta precisión entre vestidos, pantalones y camisas, use este dominio.|
|__Dominios compactos__| Optimizado para las restricciones de clasificación en tiempo real en dispositivos perimetrales.|

## <a name="object-detection"></a>Detección de objetos

|Domain|Propósito|
|---|---|
|__General__| Optimizado para una amplia variedad de tareas de detección de objetos. Si ninguno de los otros dominios es adecuado, o no está seguro de qué dominio elegir, seleccione el dominio genérico.|
|__Logotipo__|Optimizado para buscar logotipos de marca en imágenes.|
|__Productos en las estanterías__|Optimizado para detectar y clasificar los productos que están en las estanterías.|
|__Dominios compactos__| Optimizados para las restricciones de detección de objetos en tiempo real en dispositivos perimetrales.|

## <a name="compact-domains"></a>Dominios compactos

Los modelos generados por los dominios compactos se pueden exportar para ejecutarse localmente. El rendimiento varía en función del dominio seleccionado. En la tabla siguiente, se indican el tamaño del modelo y el tiempo de inferencia en una CPU para equipos de sobremesa de Intel y en una GPU de NVidia \[1\]. 

> [!NOTE]
> Estas cifras no incluyen el tiempo de preprocesamiento y el de posprocesamiento.

|Tarea|Domain|Tamaño del modelo|Tiempo de inferencia de CPU|Tiempo de inferencia de GPU|
|---|---|---|---|---|
|clasificación|General (compacto)|5 MB|13 ms|5 ms|
|Detección de objetos|General (compacto)|45 MB|35 ms|5 ms|
|Detección de objetos|General (compacto) [S1]|14 MB|27 ms|7 ms|

## <a name="vaidk-vision-ai-dev-kit"></a>VAIDK (kit de desarrollo de IA de Vision)

Cuando se selecciona un dominio compacto, se proporciona una opción adicional, "Export Capabilities" (Exportar funcionalidades), que permite distinguir entre "Basic Platforms" (Plataformas básicas) "Vision AI Dev Kit" (Kit de desarrollo de IA de Vision).

En _Export Capabilities_ (Exportar funcionalidades) las dos opciones son:

- Basic platforms (Tensorflow, CoreML, ONNX, etc.) (Plataformas básicas [Tensorflow, CoreML, ONNX, etc.])
- Vision AI Dev Kit (Kit de desarrollo de IA de Vision)

Si se selecciona _Vision AI Dev Kit_ (Kit de desarrollo de IA de Vision), los dominios compactos _Generic_ (Genérico), _Landmarks_ (Puntos de referencia) y _Retail_ (Minorista), están disponibles en Image Classification (Clasificación de imágenes), pero _Food_ (Alimentos) no lo está, mientras que _General (compacto)_ y _General (compacto) [S1]_ están disponibles en Object Detection (Detección de objetos).

>[!NOTE]
>El dominio __General (compact)__ de Object Detection (Detección de objetos) requiere una lógica de posprocesamiento especial. Para más información, vea cualquier script de ejemplo del paquete comprimido exortado. Si necesita un modelo sin lógica de posprocesamiento, use __General (compacto) [S1]__ .

>[!IMPORTANT]
>No se garantiza que los modelos exportados proporcionen exactamente el mismo resultado que la API de predicción en la nube. Una leve diferencia en la plataforma de ejecución o en la implementación del preprocesamiento puede provocar una diferencia mayor en las salidas de los modelos. Para más información sobre la lógica de preprocesamiento, consulte [este documento](quickstarts/image-classification.md).

\[1\] CPU Intel Xeon E5-2690 CPU y NVIDIA Tesla M60
