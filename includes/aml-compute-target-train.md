---
title: archivo de inclusión
description: archivo de inclusión
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 12/04/2019
ms.openlocfilehash: dc6c29793b71525ad64254aa9f57e12e56601adc
ms.sourcegitcommit: d662eda7c8eec2a5e131935d16c80f1cf298cb6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82726221"
---
**Los destinos de proceso se pueden reutilizar de un trabajo de entrenamiento al siguiente**. Por ejemplo, una vez que se adjunta una VM remota al área de trabajo, puede reutilizarla para varios trabajos.  En el caso de las canalizaciones de aprendizaje automático, use el [paso de canalización](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps?view=azure-ml-py) adecuado para cada destino de proceso.

|&nbsp;Objetivos del aprendizaje|[Machine Learning automatizado](../articles/machine-learning/concept-automated-ml.md) | [Canalizaciones de Machine Learning](../articles/machine-learning/concept-ml-pipelines.md) | [Diseñador de Azure Machine Learning](../articles/machine-learning/concept-designer.md)
|----|:----:|:----:|:----:|
|[Equipo local](../articles/machine-learning/how-to-set-up-training-targets.md#local)| sí | &nbsp; | &nbsp; |
|[Clúster de proceso de Azure Machine Learning](../articles/machine-learning/how-to-set-up-training-targets.md#amlcompute)| Sí <br/>&nbsp;ajuste de hiperparámetros | sí | sí |
|[Instancia de proceso de Azure Machine Learning](../articles/machine-learning/concept-compute-instance.md) | Sí <br/>ajuste de hiperparámetros | sí | sí |
|[Máquina virtual remota](../articles/machine-learning/how-to-set-up-training-targets.md#vm) | Sí <br/>ajuste de hiperparámetros | sí | &nbsp; |
|[Azure&nbsp;Databricks](../articles/machine-learning/how-to-create-your-first-pipeline.md#databricks)| sí (solo en el modo local del SDK) | sí | &nbsp; |
|[Análisis con Azure Data Lake](../articles/machine-learning/how-to-create-your-first-pipeline.md#adla) | &nbsp; | sí | &nbsp; |
|[HDInsight de Azure](../articles/machine-learning/how-to-set-up-training-targets.md#hdinsight) | &nbsp; | sí | &nbsp; |
|[Azure Batch](../articles/machine-learning/how-to-set-up-training-targets.md#azbatch) | &nbsp; | sí | &nbsp; |
