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
ms.date: 09/17/2020
ms.openlocfilehash: cdf2c3d1840f64b267f1aac1f8877d5f0bf76082
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91841969"
---
**Los destinos de proceso se pueden reutilizar de un trabajo de entrenamiento al siguiente**. Por ejemplo, después de adjuntar una VM remota al área de trabajo, puede reutilizarla para varios trabajos. En el caso de las canalizaciones de aprendizaje automático, use el [paso de canalización](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps?view=azure-ml-py&preserve-view=true) adecuado para cada destino de proceso.

Puede usar cualquiera de los siguientes recursos para un destino de proceso de entrenamiento para la mayoría de los trabajos. No todos los recursos se pueden usar para el aprendizaje automático automatizado, canalizaciones de aprendizaje automático o el diseñador.

|&nbsp;Objetivos del aprendizaje|[Aprendizaje automático automatizado](../articles/machine-learning/concept-automated-ml.md) | [Canalizaciones de aprendizaje automático](../articles/machine-learning/concept-ml-pipelines.md) | [Diseñador de Azure Machine Learning](../articles/machine-learning/concept-designer.md)
|----|:----:|:----:|:----:|
|[Equipo local](../articles/machine-learning/how-to-attach-compute-targets.md#local)| Sí | &nbsp; | &nbsp; |
|[Clúster de proceso de Azure Machine Learning](../articles/machine-learning/how-to-create-attach-compute-cluster.md)| Sí | Sí | Sí |
|[Instancia de proceso de Azure Machine Learning](../articles/machine-learning/how-to-create-manage-compute-instance.md) | Sí (mediante el SDK)  | Sí |  |
|[Máquina virtual remota](../articles/machine-learning/how-to-attach-compute-targets.md#vm) | Sí  | Sí | &nbsp; |
|[Azure&nbsp;Databricks](../articles/machine-learning/how-to-attach-compute-targets.md#databricks)| Sí (solo en el modo local del SDK) | Sí | &nbsp; |
|[Análisis con Azure Data Lake](../articles/machine-learning/how-to-attach-compute-targets.md#adla) | &nbsp; | Sí | &nbsp; |
|[HDInsight de Azure](../articles/machine-learning/how-to-attach-compute-targets.md#hdinsight) | &nbsp; | Sí | &nbsp; |
|[Azure Batch](../articles/machine-learning/how-to-attach-compute-targets.md#azbatch) | &nbsp; | Sí | &nbsp; |
