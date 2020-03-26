---
title: Canalizaciones del diseñador de ejemplo
titleSuffix: Azure Machine Learning
description: Use los ejemplos del diseñador de Azure Machine Learning para iniciar las canalizaciones de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 03/10/2020
ms.openlocfilehash: 82476b9cc8d92c815df602496ed3dcb33014a4fd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "79037293"
---
# <a name="designer-sample-pipelines"></a>Canalizaciones de ejemplo del diseñador

Use los ejemplos integrados en el diseñador de Azure Machine Learning para empezar a crear rápidamente sus propias canalizaciones de aprendizaje automático. El [repositorio de GitHub](https://github.com/Azure/MachineLearningDesigner) del diseñador de Azure Machine Learning contiene documentación detallada para ayudarle a conocer algunos escenarios comunes de aprendizaje automático.

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://aka.ms/AMLFree).
* Un área de trabajo de Azure Machine Learning con un SKU de Enterprise.


## <a name="how-to-use-sample-pipelines"></a>Cómo usar las canalizaciones de ejemplo

El diseñador guarda una copia de las canalizaciones de ejemplo en el área de trabajo del estudio. Puede editar la canalización para adaptarla a sus necesidades y guardarla como propia. Úselas como punto de partida para iniciar sus proyectos.

1. Inicie sesión en <a href="https://ml.azure.com?tabs=jre" target="_blank">ml.azure.com</a> y seleccione el área de trabajo con la que quiere trabajar.

1. Seleccione **Diseñador**.

1. Seleccione una canalización de ejemplo en la sección **Nueva canalización**.

    Seleccione **Mostrar más ejemplos** para obtener una lista completa de ejemplos.

## <a name="regression-samples"></a>Ejemplos de regresión

Obtenga más información sobre los ejemplos de regresión integrados.

| Título de ejemplo | Descripción | 
| --- | --- |
| [Ejemplo 1: Regresión. Predicción del precio de los automóviles (básica)](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-regression-automobile-price-basic.md) | Se predicen los precios de los automóviles mediante una regresión lineal. |
| [Ejemplo 2: Regresión. Predicción del precio de los automóviles (avanzado)](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-regression-automobile-price-compare-algorithms.md) | Se predicen los precios de los automóviles mediante los regresores de árbol de decisión impulsado y bosque de decisión. Compare los modelos para encontrar el mejor algoritmo.

## <a name="classification-samples"></a>Ejemplos de clasificación

Obtenga más información sobre los ejemplos de clasificación integrados. Para más información sobre los ejemplos sin vínculos de documentación, abra los ejemplos y vea los comentarios del módulo en su lugar.

| Título de ejemplo | Descripción | 
| --- | --- |
| [Ejemplo 3: Clasificación binaria con selección de características. Predicción de ingresos](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-predict-income.md) | Se predicen los ingresos como altos o bajos, mediante un árbol de decisión impulsado de dos clases. Use la correlación de Pearson para seleccionar las características.
| [Ejemplo 4: Clasificación binaria con un script de Python personalizado. Predicción del riesgo de crédito](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-credit-risk-cost-sensitive.md) | Clasifique las aplicaciones de crédito como de riesgo alto o bajo. Use el módulo para ejecutar script de Python para ponderar los datos.
| [Ejemplo 5: Clasificación binaria. Predicción de las relaciones de cliente](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-churn.md) | Se predice el abandono de los clientes mediante árboles de decisión impulsados de dos clases. Use SMOTE para muestrear los datos sesgados.
| [Ejemplo 7: Clasificación de texto. Conjunto de datos de Wikipedia SP 500](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-text-classification.md) | Clasifique los tipos de empresa de los artículos de Wikipedia con regresión logística multiclase. |
| Ejemplo 12: Clasificación multiclase. Reconocimiento de letras | Cree un conjunto de clasificadores binarios para clasificar las letras escritas. |

## <a name="recommender-samples"></a>Ejemplos de recomendación

Obtenga más información sobre los ejemplos integrados de recomendación. Para más información sobre los ejemplos sin vínculos de documentación, abra los ejemplos y vea los comentarios del módulo en su lugar.

| Título de ejemplo | Descripción | 
| --- | --- |
| Ejemplo 10: Recomendación. Tweets de clasificación de películas | Cree un motor de recomendación de películas a partir de títulos de películas y su clasificación. |

## <a name="utility-samples"></a>Ejemplos de utilidades

Obtenga más información sobre los ejemplos que muestran las características y utilidades de aprendizaje automático. Para más información sobre los ejemplos sin vínculos de documentación, abra los ejemplos y vea los comentarios del módulo en su lugar.

| Título de ejemplo | Descripción | 
| --- | --- |
| [Ejemplo 6: Script R personalizado. Predicción de retrasos de vuelos](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-flight-delay.md) |
| Ejemplo 8: Validación cruzada para clasificación binaria. Predicción de ingresos para adultos | Use la validación cruzada para crear un clasificador binario para los ingresos para los adultos.
| Ejemplo 9: Importancia de la característica de permutación | Use la importancia de la característica de permutación a fin de calcular las clasificaciones de importancia del conjunto de datos de prueba. 
| Ejemplo 11: Parámetros de optimización para la clasificación binaria. Predicción de ingresos para adultos | Utilice los hiperparámetros del modelo de optimización para encontrar los óptimos al crear un clasificador binario. |

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Aprenda a crear e implementar modelos de aprendizaje automático con [Tutorial: Predicción del precio de un automóvil con el diseñador](tutorial-designer-automobile-price-train-score.md)
