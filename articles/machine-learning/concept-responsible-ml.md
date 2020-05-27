---
title: Machine Learning (ML) responsable
titleSuffix: Azure Machine Learning
description: Conozca lo que es ML responsable y cómo usarlo en Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: luquinta
author: luisquintanilla
ms.date: 05/08/2020
ms.openlocfilehash: 3cef3c2179019f6d84de5596e61abaf8d7d3182c
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83595014"
---
# <a name="responsible-machine-learning-ml"></a>Machine Learning (ML) responsable

En este artículo, aprenderá qué es ML responsable y las formas en que se puede poner en práctica con Azure Machine Learning.

A lo largo del desarrollo y el uso de sistemas de AI, la confianza debe ser el centro de todo. Debe confiar en la plataforma, el proceso y los modelos. En Microsoft, los siguientes valores y principios se abarcan en ML responsable:

- Descripción de los modelos de Machine Learning
  - Interpretación y explicación del comportamiento del modelo
  - Evaluación y mitigación de la parcialidad del modelo
- Protección de los usuarios y sus datos
  - Prevención de la exposición de datos con privacidad diferencial  
- Control del proceso de aprendizaje automático de un extremo a otro
  - Documentación del ciclo de vida del aprendizaje automático con hojas de datos

:::image type="content" source="media/concept-responsible-ml/responsible-ml-pillars.png" alt-text="Fundamentos de ML responsable":::

La inteligencia artificial y los sistemas autónomos están cada vez más integrados en la sociedad, por lo que es importante hacer un esfuerzo para prever y mitigar las consecuencias no deseadas de estas tecnologías.

## <a name="interpret-and-explain-model-behavior"></a>Interpretación y explicación del comportamiento del modelo

Los sistemas de caja negra o aquellos que son difíciles de explicar pueden suponer un problema, ya que las partes interesadas como desarrolladores de sistemas, reguladores, usuarios y responsables de negocios, pueden no entender con claridad por qué estos sistemas toman ciertas decisiones. Algunos sistemas de IA son más fáciles de explicar que otros y, a veces, existe cierto equilibrio entre un sistema que cuenta con una precisión mayor y otro más descriptivo.

Para compilar sistemas de AI interpretables, use [InterpretML](https://github.com/interpretml/interpret), que es un paquete de código abierto que ha compilado Microsoft. [InterpretML se puede usar en Azure Machine Learning](how-to-machine-learning-interpretability.md) para [interpretar y explicar los modelos de Machine Learning](how-to-machine-learning-interpretability-aml.md), incluidos los [modelos automatizados de Machine Learning](how-to-machine-learning-interpretability-automl.md).

## <a name="assess-and-mitigate-model-unfairness"></a>Evaluación y mitigación de la parcialidad del modelo

A medida que los sistemas de IA se involucran cada vez más en el día a día de nuestra sociedad, es extremadamente importante que estos sistemas funcionen bien para que puedan proporcionar buenos resultados a todos los usuarios.

La parcialidad de los sistemas de IA puede dar lugar a las siguientes consecuencias no deseadas:

- Pérdida de oportunidades, recursos o información de usuarios.
- Se refuerzan los sesgos y los estereotipos.

Muchos aspectos de la imparcialidad no se pueden capturar ni representar mediante métricas. Existen herramientas y prácticas que pueden mejorar la imparcialidad en el diseño y desarrollo de sistemas de IA.

Dos pasos clave para reducir la parcialidad en los sistemas de IA son la valoración y la mitigación. Le recomendamos que use [FairLearn](https://github.com/fairlearn/fairlearn), un paquete de código abierto que puede valorar y mitigar la posible parcialidad de los sistemas de IA. Para obtener más información sobre la imparcialidad y el paquete FairLearn, consulte [el artículo sobre la imparcialidad en ML](./concept-fairness-ml.md).

## <a name="prevent-data-exposure-with-differential-privacy"></a>Prevención de la exposición de datos con privacidad diferencial

Cuando se usan datos para el análisis, es importante que los estos sean privados y confidenciales a lo largo del proceso. La privacidad diferencial es un conjunto de sistemas y prácticas que ayudan a mantener los datos de los usuarios seguros y privados.

En escenarios tradicionales, los datos sin procesar se almacenan en archivos y bases de datos. Cuando los usuarios analizan datos, suelen usar los datos sin procesar. Esto supone un problema, ya que se podría infringir la privacidad de los usuarios. La privacidad diferencial intenta solucionar este problema agregando "ruido" o aleatoriedad a los datos para que los usuarios no puedan identificar puntos de datos individuales.

La implementación de sistemas privados de forma diferencial es difícil. [WhiteNoise](https://github.com/opendifferentialprivacy/whitenoise-core) es un proyecto de código abierto que contiene distintos componentes para la creación de sistemas privados globales de forma diferencial. Para obtener más información sobre la privacidad diferencial y el proyecto WhiteNoise, consulte el artículo para [mantener la privacidad de los datos mediante la privacidad diferencial y WhiteNoise](./concept-differential-privacy.md).

## <a name="document-the-machine-learning-lifecycle-with-datasheets"></a>Documentación del ciclo de vida del aprendizaje automático con hojas de datos

Documentar la información adecuada en el proceso de aprendizaje automático es indispensable para poder tomar decisiones responsables en cada fase. Las hojas de datos son una forma de documentar los recursos de aprendizaje automático que se usan y se crean como parte del ciclo de vida del aprendizaje automático.

Los modelos suelen considerarse como "cajas negras" y, a menudo, hay poca información sobre ellos. Dado que los sistemas de aprendizaje automático están cada vez más generalizados y se usan en la toma de decisiones, el uso de hojas de datos es un paso más hacia el desarrollo de sistemas de aprendizaje automático más responsables.

Aquí tiene información del modelo que tal vez quiera documentar como parte de la hoja de datos:

- Uso previsto
- Arquitectura del modelo
- Datos de aprendizaje usados
- Datos de evaluación usados
- Métricas de rendimiento del modelo de entrenamiento
- Información de imparcialidad

Consulte el ejemplo siguiente para obtener información sobre cómo usar el SDK de Azure Machine Learning para implementar las [hojas de datos para modelos](https://github.com/microsoft/MLOps/blob/master/pytorch_with_datasheet/model_with_datasheet.ipynb).

## <a name="additional-resources"></a>Recursos adicionales

- Obtenga más información sobre el conjunto de directrices de [ABOUT ML](https://www.partnershiponai.org/about-ml/) referentes a la documentación del sistema de aprendizaje automático.