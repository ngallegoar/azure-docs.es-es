---
title: Qué son los destinos de proceso
titleSuffix: Azure Machine Learning
description: Defina dónde desea entrenar o implementar el modelo con Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 03/30/2020
ms.openlocfilehash: f9ca75943eaec2ae018b54145d872fc09294035e
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2020
ms.locfileid: "80398178"
---
#  <a name="what-are-compute-targets-in-azure-machine-learning"></a>¿Qué son los destinos de proceso en Azure Machine Learning? 

Un **destino de proceso** es un entorno o recurso de proceso en el que se ejecuta el script de entrenamiento o se hospeda la implementación del servicio web. Esta ubicación puede ser su equipo local o un recurso de proceso en la nube. El uso de los destinos de proceso le facilita cambiar posteriormente el entorno de proceso sin tener que cambiar el código.  

En un ciclo de vida de desarrollo de modelos típico, puede:
1. Comenzar desarrollando y experimentando con una pequeña cantidad de datos. En esta etapa, le recomendamos su entorno local (equipo local o máquina virtual basada en la nube) como destino de proceso. 
2. Escalar verticalmente a datos más grandes o realizar un entrenamiento distribuido mediante uno de estos [destinos de proceso de entrenamiento](#train).  
3. Una vez que el modelo esté listo, impleméntelo en un entorno de hospedaje web o en un dispositivo de IoT con uno de estos [destinos de proceso de implementación](#deploy).

Los recursos de proceso que use para los destinos de proceso están asociados a un [área de trabajo](concept-workspace.md). Los usuarios del área de trabajo comparten los recursos de proceso que no sean el equipo local.

## <a name="training-compute-targets"></a><a name="train"></a> Entrenamiento de destinos de proceso

Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes recursos de proceso.  También puede adjuntar su propio recurso de proceso, aunque el soporte técnico para varios escenarios puede variar.

[!INCLUDE [aml-compute-target-train](../../includes/aml-compute-target-train.md)]

Obtenga más información sobre la [configuración y el uso de un destino de proceso para el entrenamiento del modelo](how-to-set-up-training-targets.md).

## <a name="deployment-targets"></a><a name="deploy"></a>Destinos de implementación

Los siguientes recursos de proceso se pueden usar hospedar la implementación del modelo.

[!INCLUDE [aml-compute-target-deploy](../../includes/aml-compute-target-deploy.md)]

Aprenda [dónde y cómo implementar el modelo en un destino de proceso](how-to-deploy-and-where.md).

<a name="amlcompute"></a>
## <a name="azure-machine-learning-compute-managed"></a>Proceso de Azure Machine Learning (administrado)

Azure Machine Learning crea y administra un recurso de proceso administrado. Dicho proceso está optimizado para cargas de trabajo de Machine Learning. Los clústeres de procesos y las [instancias de procesos](concept-compute-instance.md) de Azure Machine Learning son los únicos procesos administrados. En el futuro podrán agregarse recursos de proceso administrados adicionales.

Puede crear instancias de procesos (versión preliminar) o clústeres de procesos de Azure Machine Learning a partir de lo siguiente:
* Azure Machine Learning Studio
* Portal de Azure
* SDK de Python [ComputeInstance](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computeinstance(class)?view=azure-ml-py) y clases de [AmlCompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute(class)?view=azure-ml-py)
* [SDK de R](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-compute-targets)
* Plantilla de Resource Manager

También puede crear un clúster de proceso con la [extensión de aprendizaje automático de la CLI de Azure](tutorial-train-deploy-model-cli.md#create-the-compute-target-for-training).

Cuando se crean, estos recursos de proceso forman parte automáticamente del área de trabajo, a diferencia de otros tipos de destinos de proceso.

### <a name="compute-clusters"></a>Clústeres de proceso

Puede usar clústeres de procesos de Azure Machine Learning para el entrenamiento y para inferencia de lotes (versión preliminar).  Con este recurso de proceso, debe:

* Clúster de uno o varios nodos
* Se escala automáticamente cada vez que se envía una ejecución 
* Administración del clúster automático y programación de trabajos 
* Es compatible con recursos de CPU y GPU



## <a name="unmanaged-compute"></a>Proceso no administrado

Azure Machine Learning *no*administra un destino de proceso no administrado. Cree este tipo de destino de proceso fuera de Azure Machine Learning y luego adjúntelo al área de trabajo. Estos recursos de proceso no administrados pueden requerir pasos adicionales para mantener o mejorar el rendimiento de las cargas de trabajo de Machine Learning.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo:
* [Configurar un destino de proceso para entrenar el modelo](how-to-set-up-training-targets.md)
* [Implementar el modelo para un destino de proceso](how-to-deploy-and-where.md)