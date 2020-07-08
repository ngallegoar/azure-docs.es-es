---
title: 'Notas de la versión: Custom Vision Service'
titleSuffix: Azure Cognitive Services
description: Obtenga la información más reciente sobre las nuevas versiones del equipo de Custom Vision.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: pafarley
ms.openlocfilehash: f7259fe486a473f994d7795de5926b86717bb1b6
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2020
ms.locfileid: "85391713"
---
# <a name="custom-vision-service-release-notes"></a>Notas de la versión de Custom Vision Service

## <a name="may-2-2019-and-may-10-2019"></a>2 de mayo de 2019 y 10 de mayo de 2019

- Corrección de errores y mejoras del back-end

## <a name="may-23-2019"></a>23 de mayo de 2019

- Experiencia de usuario mejorada en el portal relativa a las suscripciones de Azure, que facilita la selección de los directorios de Azure.

## <a name="april-18-2019"></a>18 de abril de 2019 

- Se ha agregado la exportación de detección de objetos para el kit de desarrollo de Vision AI.
- Ajustes de la interfaz de usuario, incluida la búsqueda de proyectos

## <a name="april-3-2019"></a>3 de abril de 2019

- Aumento del límite en el número de rectángulos delimitadores por imagen a 200. 
- Correcciones de errores, que incluye una importante actualización del rendimiento para los modelos exportados a TensorFlow. 

## <a name="march-26-2019"></a>26 de marzo de 2019

- Custom Vision Service ya se encuentra disponible con carácter general en Azure.
- Se ha agregado la característica de entrenamiento avanzado con un nuevo back-end de aprendizaje automático que mejora el rendimiento, en especial con conjuntos de datos difíciles y clasificaciones pormenorizadas. Con el entrenamiento avanzado, puede especificar un presupuesto de tiempo de proceso para el entrenamiento y Custom Vision identificará de forma experimental la mejor configuración de entrenamiento y aumento. Para iteraciones rápidas, puede seguir usando el entrenamiento rápido existente.
- Se han introducido las API 3.0. Se ha anunciado la próxima entrada en desuso de las API anteriores a 3.0 el 1 de octubre de 2019. Consulte los inicios rápidos de documentación de [.Net](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/csharp-tutorial), [Python](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/python-tutorial), [nodo](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/node-tutorial), [Java](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/java-tutorial) o [Go](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/go-tutorial) para obtener ejemplos sobre cómo empezar a trabajar.
- Se han reemplazado las iteraciones predeterminadas por la opción para publicar o cancelar la publicación en las API 3.0.
- Se han agregado nuevos destinos de exportación de modelos. La exportación de Dockerfile se ha actualizado para admitir ARM para Raspberry Pi 3. Se ha agregado compatibilidad con la exportación al [kit de desarrollo de Vision AI](https://visionaidevkit.com/).
- Se ha aumentado el límite de etiquetas por proyecto a 500 para el nivel S0. Se ha aumentado el límite de imágenes por proyecto a 100 000 para el nivel S0.
- Se ha quitado el dominio para adultos. Se recomienda en su lugar el dominio general.
- Se han anunciado los [precios](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) de disponibilidad general.  

## <a name="february-25-2019"></a>25 de febrero de 2019

- Se ha anunciado el final de los proyectos de prueba limitada (no asociados con un recurso de Azure), dado que Custom Vision se acerca al final de su migración a la versión preliminar pública de Azure. A partir del 25 de marzo de 2019, el sitio CustomVision.ai solo admite proyectos de visualización asociados con un recurso de Azure, como el recurso gratuito de Custom Vision. Hasta el 1 de octubre de 2019, podrá acceder a los proyectos de prueba limitada existentes mediante las API de Custom Vision. De modo que tiene tiempo para actualizar las claves de API de las aplicaciones escritas con Custom Vision. Después del 1 de octubre de 2019, todos los proyectos de prueba limitada que no haya migrado a Azure se eliminarán.

## <a name="january-22-2019"></a>22 de enero de 2019

- Se ha agregado compatibilidad para nuevas regiones de Azure: Oeste de EE. UU. 2, Este de EE. UU., Este de EE. UU. 2, Oeste de Europa, Norte de Europa, Sudeste de Asia, Este de Australia, Centro de la India, Sur de Reino Unido, Este de Japón, Centro-norte de EE. UU. Se mantiene la compatibilidad para Centro-sur de EE. UU.

## <a name="december-12-2018"></a>12 de diciembre de 2018

- Admite la exportación de modelos de detección de objetos (se ha introducido el dominio compacto de detección de objetos).
- Se han corregido varios problemas de accesibilidad para mejorar el lector de pantalla y el soporte para navegación del teclado.
- Actualizaciones de la experiencia de usuario en el visor de imágenes y se ha mejorado el etiquetado de la detección de objetos, con el fin de agilizar el etiquetado.  
- Actualización del modelo base del dominio de detección de objetos para mejorar la calidad de la detección de objetos.
- Correcciones de errores.

## <a name="november-6-2018"></a>6 de noviembre de 2018

- Se ha agregado compatibilidad con el dominio de logotipo en la detección de objetos.

## <a name="october-9-2018"></a>9 de octubre de 2018

- Detección de objetos entra en la versión preliminar de pago. Ahora puede crear proyectos de detección de objetos con un recurso de Azure.
- La característica "Mover a Azure" se ha agregado al sitio web, para que resulte más fácil actualizar un proyecto de prueba limitado para vincular a un proyecto de recursos vinculados de Azure (F0 o S0). Puede encontrarlo en la página Configuración del producto.  
- Se ha agregado exportación a ONNX 1.2, para admitir la versión Actualización de octubre de 2018 de Windows de Windows ML.
Correcciones de errores, incluida la exportación ONNX con caracteres especiales.

## <a name="august-14-2018"></a>14 de agosto de 2018

- Se agregó un widget de "Introducción" al sitio customvision.ai para orientar a los usuarios a lo largo del aprendizaje del proyecto.
- Otras mejoras en la canalización de aprendizaje automático para beneficiar a los proyectos multietiqueta (nueva capa de pérdida).

## <a name="june-28-2018"></a>28 de junio de 2018

- Corrección de errores y mejoras del back-end.
- Clasificación multiclase habilitada, para proyectos en los que las imágenes tienen exactamente una etiqueta. En las predicciones para el modo multiclase, las probabilidades suman uno (todas las imágenes se clasifican entre las etiquetas especificadas).

## <a name="june-13-2018"></a>13 de junio de 2018

- Actualización de la experiencia del usuario centrada en la facilidad de uso y la accesibilidad.
- Mejoras en la canalización de aprendizaje automático para beneficiar a los proyectos multietiqueta con una gran cantidad de etiquetas.
- Se corrigió el error en la exportación de TensorFlow. Se han habilitado las versiones de modelo exportado, para que las iteraciones se puedan exportar más de una vez.

## <a name="may-7-2018"></a>7 de mayo de 2018

- Se ha incorporado la versión preliminar de la característica de detección de objetos para proyectos con versiones de prueba limitadas.
- Las API se han actualizado a la versión 2.0.
- El nivel S0 se ha ampliado hasta 250 etiquetas y 50 000 imágenes.
- Se han incorporado mejoras importantes en los componentes back-end de la canalización de aprendizaje automático para los proyectos de clasificación de imágenes. Los proyectos entrenados después del 27 de abril de 2018 se beneficiarán de estas actualizaciones.
- Se ha agregado la exportación de modelos en ONNX para que pueda usarse con aprendizaje automático de Windows.
- Se ha agregado la exportación de modelos en Dockerfile. Esto le permite descargar los artefactos necesarios para generar sus propios contenedores de Windows o Linux, como un archivo DockerFile, un modelo de TensorFlow y un código del servicio.
- En el caso de los modelos recién entrenados que se exportan a TensorFlow en los dominios General (compacto) y Landmark (compacto), [ahora los valores medios son (0,0,0)](https://github.com/azure-samples/cognitive-services-android-customvision-sample) para que sean coherentes en todos los proyectos.

## <a name="march-1-2018"></a>1 de marzo de 2018

- Se ha incorporado una versión preliminar de pago y se ha integrado en Azure Portal. Ahora, los proyectos pueden vincularse a recursos de Azure utilizando un nivel F0 (gratis) o S0 (estándar). Se han incorporado proyectos en el nivel S0, lo que permite tener hasta 100 etiquetas y 25 000 imágenes.
- Se han realizado cambios en los componentes back-end del parámetro de normalización y la canalización de aprendizaje automático. De este modo, los clientes tendrán un mayor control sobre el equilibrio entre precisión y exhaustividad al ajustar el umbral de probabilidad. Como parte de estos cambios, el umbral de probabilidad predeterminado del portal CustomVision.ai se ha establecido en 50 %.

## <a name="december-19-2017"></a>19 de diciembre de 2017

- Se ha incorporado la exportación a Android (TensorFlow), junto con la exportación a iOS (CoreML) publicada anteriormente. Esto permite exportar un modelo compacto entrenado para ejecutarlo sin conexión en una aplicación.
- Se han agregado los dominios "compactos" Retail y Landmark para permitir la exportaciones de modelos en estos dominios.
- Se ha publicado la versión [1.2 de Training API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/f2d62aa3b93843d79e948fe87fa89554/operations/5a3044ee08fa5e06b890f11f) y la versión [1.1 de Prediction API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/57982f59b5964e36841e22dfbfe78fc1/operations/5a3044f608fa5e06b890f164). Las API actualizadas admiten la exportación de modelos, una nueva operación de predicción que no guarda las imágenes en "Predictions" (Predicciones). Además, se han incorporado operaciones por lotes en Training API.
- Se han realizado pequeños ajustes en la experiencia del usuario, como la capacidad para ver qué dominio se ha utilizado para entrenar una iteración.
- Se ha actualizado el [SDK y el ejemplo de C#](https://github.com/Microsoft/Cognitive-CustomVision-Windows).