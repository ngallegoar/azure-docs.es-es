---
title: Azure Machine Learning frente a Machine Learning Studio (clásico)
description: ¿Cuál es la diferencia entre Azure Machine Learning y Machine Learning Studio (clásico)?
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
author: j-martens
ms.author: jmartens
ms.date: 03/25/2020
ms.openlocfilehash: 1d4896a2dd2115e5654baffe394618b30a294d5b
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2020
ms.locfileid: "86231750"
---
# <a name="azure-machine-learning-vs-machine-learning-studio-classic"></a>Azure Machine Learning frente a Machine Learning Studio (clásico)

En este artículo, aprenderá las diferencias entre Azure Machine Learning y Machine Learning Studio (clásico). 

Azure Machine Learning proporciona los SDK para Python y R **y** el diseñador de tipo "arrastrar y colocar" para crear e implementar modelos de aprendizaje automático. Studio (clásico) solo ofrece una experiencia independiente de arrastrar y colocar.

Se recomienda que los nuevos usuarios elijan Azure Machine Learning por su gama más amplia de herramientas de aprendizaje automático de vanguardia.

## <a name="quick-comparison"></a>Comparación rápida

En la tabla siguiente se resumen algunas de las diferencias principales entre Azure Machine Learning y Studio (clásico):

| Característica | Machine Learning Studio (clásico) | Azure Machine Learning |
|---| --- | --- |
| Interfaz que permite arrastrar y colocar | Compatible | Compatible: [diseñador de Azure Machine Learning (versión preliminar)](concept-designer.md) <br/>(requiere el área de trabajo de Enterprise) | 
| Experimento | Escalable (límite de datos de entrenamiento, 10 GB) | Escalar con destino de proceso |
| Entrenamiento de destinos de proceso | Destino de proceso propietario, solo admite CPU | Amplia gama de [destinos de proceso de entrenamiento](concept-compute-target.md#train) personalizables. Incluye compatibilidad con GPU y CPU | 
| Destinos de proceso de implementación | Formato del servicio web propietario, no personalizable | Amplia gama de [destinos de proceso de implementación](concept-compute-target.md#deploy) personalizables. Incluye compatibilidad con GPU y CPU |
| Canalización de Machine Learning | No compatible | Creación de [canalizaciones](concept-ml-pipelines.md) flexibles y modulares para automatizar flujos de trabajo |
| MLOps | Implementación y administración básicas de modelos | Control de versiones de entidades (modelo, datos, flujos de trabajo), automatización de flujos de trabajo, integración con herramientas de CICD [y más](concept-model-management-and-deployment.md) |
| Formato del modelo | Formato propietario, solo Studio (clásico) | Se admiten varios formatos en función del tipo de trabajo de entrenamiento |
| Entrenamiento de modelos y optimización de hiperparámetros automatizados |  No compatible | [Se admite en el SDK y en el área de trabajo visual](concept-automated-ml.md) | 
| Detección de desfase de datos | No compatible | [Se admite en el SDK y en el área de trabajo visual](how-to-monitor-datasets.md) |


## <a name="migrate-from-machine-learning-studio-classic"></a>Migración desde Machine Learning Studio (clásico)

Actualmente, no hay manera de migrar recursos de Studio (clásico) al diseñador de Azure Machine Learning (versión preliminar). Los usuarios de Studio (clásico) pueden seguir usando sus recursos de aprendizaje automático. Sin embargo, se recomienda que todos los usuarios consideren la posibilidad de usar el diseñador, que proporciona una experiencia de tipo arrastrar y colocar conocida con un flujo de trabajo mejorado **y** escalabilidad, control de versiones y seguridad empresarial.

## <a name="get-started-with-azure-machine-learning"></a>Introducción a Azure Machine Learning

Los siguientes recursos pueden ayudarle a empezar a trabajar con Azure Machine Learning. 

- Consulte [Introducción a Azure Machine Learning](overview-what-is-azure-ml.md).

- Cree su [primer experimento con el SDK para Python](tutorial-1st-experiment-sdk-setup.md).

- [Cree su primera canalización de diseñador](tutorial-designer-automobile-price-train-score.md) para predecir precios automáticos.

![Ejemplo del diseñador de Azure Machine Learning](media/concept-designer/designer-drag-and-drop.gif)

## <a name="next-steps"></a>Pasos siguientes

Además de las funcionalidades de arrastrar y colocar en el diseñador, Azure Machine Learning tiene otras herramientas disponibles:  
  + [Uso de cuadernos de Python para entrenar e implementar modelos de aprendizaje automático](tutorial-1st-experiment-sdk-setup.md)
  + [Uso de R Markdown para entrenar e implementar modelos de aprendizaje automático](tutorial-1st-r-experiment.md) 
  + [Uso del aprendizaje automático automatizado para entrenar e implementar modelos de aprendizaje automático](tutorial-first-experiment-automated-ml.md)  
  + [Uso de la CLI de Machine Learning para entrenar e implementar un modelo](tutorial-train-deploy-model-cli.md)

