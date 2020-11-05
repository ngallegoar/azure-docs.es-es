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
ms.date: 09/29/2020
ms.openlocfilehash: 72cdfe2ebe79abb9649ac497a6ecddda8918b951
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93322293"
---
# <a name="what-are-compute-targets-in-azure-machine-learning"></a>¿Qué son los destinos de proceso en Azure Machine Learning?

Un *destino de proceso* es un entorno o recurso de proceso designado en el que se ejecuta el script de entrenamiento o se hospeda la implementación del servicio. Esta ubicación podría ser su equipo local o un recurso de proceso basado en la nube. El uso de los destinos de proceso facilita el cambio posterior del entorno de proceso sin tener que cambiar el código.

En un ciclo de vida de desarrollo de modelos típico, puede:

1. Comenzar desarrollando y experimentando con una pequeña cantidad de datos. En esta etapa, use su entorno local, como un equipo local o una máquina virtual (VM) basada en la nube, como destino de proceso.
1. Escalar verticalmente a datos más grandes o realizar un entrenamiento distribuido mediante el uso de uno de estos [destinos de proceso de entrenamiento](#train).
1. Después de que el modelo esté listo, impleméntelo en un entorno de hospedaje web o en un dispositivo de IoT con uno de estos [destinos de proceso de implementación](#deploy).

Los recursos de proceso que use para los destinos de proceso están asociados a un [área de trabajo](concept-workspace.md). Los usuarios del área de trabajo comparten los recursos de proceso que no sean el equipo local.

## <a name="training-compute-targets"></a><a name="train"></a> Entrenamiento de destinos de proceso

Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes destinos de proceso. Un ciclo de vida de desarrollo de modelos típico comienza con el desarrollo o la experimentación en una pequeña cantidad de datos. En esta etapa, use un entorno local, como el equipo local o una máquina virtual basada en la nube. Si escala verticalmente el entrenamiento a conjuntos de datos grandes o realiza el entrenamiento distribuido, use un proceso de Azure Machine Learning para crear un clúster con uno o varios nodos que se escala automáticamente cada vez que se envía una ejecución. También puede adjuntar su propio recurso de proceso, aunque el soporte técnico para escenarios distintos puede variar.

[!INCLUDE [aml-compute-target-train](../../includes/aml-compute-target-train.md)]

Obtenga más información sobre cómo [enviar una ejecución de entrenamiento a un destino de proceso](how-to-set-up-training-targets.md).

## <a name="compute-targets-for-inference"></a><a name="deploy"></a> Destinos de proceso para inferencia

Los siguientes recursos de proceso se pueden usar hospedar la implementación del modelo.

[!INCLUDE [aml-compute-target-deploy](../../includes/aml-compute-target-deploy.md)]

Al realizar la inferencia, Azure Machine Learning crea un contenedor de Docker que hospeda el modelo y los recursos asociados necesarios para utilizarlo. Este contenedor se usa después en uno de los siguientes escenarios de implementación:

* Como *servicio web* que se usa para la inferencia en tiempo real. Las implementaciones de servicios web usan uno de los siguientes destinos de proceso:

    * [Equipo local](how-to-attach-compute-targets.md#local)
    * [Instancia de proceso de Azure Machine Learning](how-to-create-manage-compute-instance.md)
    * [Azure Container Instances](how-to-attach-compute-targets.md#aci)
    * [Azure Kubernetes Service](how-to-create-attach-kubernetes.md)
    * Azure Functions (versión preliminar). La implementación en Functions solo usa Azure Machine Learning para crear el contenedor de Docker. Desde allí, se implementa mediante el uso de Functions. Para obtener más información, consulte [Implementación de un modelo de Machine Learning en Azure Functions (versión preliminar)](how-to-deploy-functions.md).

* Como punto de conexión de _inferencia de lotes_ que se usa para procesar periódicamente lotes de datos. Las inferencias de lotes usan [clústeres de proceso de Azure Machine Learning](how-to-create-attach-compute-cluster.md).

* A un _dispositivo IoT_ (versión preliminar). La implementación en un dispositivo IoT solo usa Azure Machine Learning para compilar el contenedor de Docker. Desde allí, se implementa mediante Azure IoT Edge. Para más información, consulte [Implementación del módulo IoT Edge (versión preliminar)](../iot-edge/tutorial-deploy-machine-learning.md).

Aprenda [dónde y cómo implementar el modelo en un destino de proceso](how-to-deploy-and-where.md).

<a name="amlcompute"></a>
## <a name="azure-machine-learning-compute-managed"></a>Proceso de Azure Machine Learning (administrado)

Azure Machine Learning crea y administra un recurso de proceso administrado. Dicho proceso está optimizado para cargas de trabajo de Machine Learning. Los clústeres de procesos y las [instancias de procesos](concept-compute-instance.md) de Azure Machine Learning son los únicos procesos administrados.

Puede crear instancias de procesos o clústeres de procesos de Azure Machine Learning con cualquiera de las siguientes opciones:

* [Azure Machine Learning Studio](how-to-create-attach-compute-studio.md).
* La CLI y el SDK de Python:
    * [Instancia de proceso](how-to-create-manage-compute-instance.md).
    * [Clúster de proceso](how-to-create-attach-compute-cluster.md).
* El [SDK de R](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-compute-targets) (versión preliminar).
* Una plantilla de Azure Resource Manager Para ver una plantilla de ejemplo, consulte [Creación de un clúster de proceso de Azure Machine Learning](https://github.com/Azure/azure-quickstart-templates/tree/master/101-machine-learning-compute-create-amlcompute).
* Una [extensión de aprendizaje automático para la CLI de Azure](reference-azure-machine-learning-cli.md#resource-management).

Cuando se crean, estos recursos de proceso forman parte automáticamente del área de trabajo, a diferencia de otros tipos de destinos de proceso.


|Capacidad  |Clúster de proceso  |Instancia de proceso  |
|---------|---------|---------|
|Clúster de uno o varios nodos     |    **&check;**       |         |
|Se escala automáticamente cada vez que se envía una ejecución     |     **&check;**      |         |
|Administración del clúster automático y programación de trabajos     |   **&check;**        |     **&check;**      |
|Es compatible con recursos de CPU y GPU     |  **&check;**         |    **&check;**       |


> [!NOTE]
> Cuando un *clúster* de proceso está inactivo, se escala automáticamente a 0 nodos, por lo que no paga cuando no se usa. Una *instancia* de proceso está siempre activa y no se escala automáticamente. Debe [detener la instancia de proceso](how-to-create-manage-compute-instance.md#manage) cuando no la esté usando para evitar costos adicionales.

### <a name="supported-vm-series-and-sizes"></a>Series y tamaños de maquina virtual compatibles

Al seleccionar un tamaño de nodo para un recurso de proceso administrado en Azure Machine Learning, puede elegir entre varios tamaños de máquina virtual disponibles en Azure. Azure ofrece una variedad de tamaños para Windows y Linux para diferentes cargas de trabajo. Para más información, consulte [Tamaños de las máquinas virtuales Linux en Azure](../virtual-machines/sizes.md).

Hay algunas excepciones y limitaciones a la hora de elegir un tamaño de máquina virtual:

* Algunas series de máquinas virtuales no se admiten en Azure Machine Learning.
* Algunas series de máquinas virtuales están restringidas. Para usar una serie restringida, póngase en contacto con el soporte técnico y solicite un aumento de la cuota para la serie. Para información sobre cómo ponerse en contacto con el soporte técnico, consulte [Opciones de soporte técnico de Azure](https://azure.microsoft.com/support/options/).

Consulte la tabla siguiente para más información sobre las series admitidas y las restricciones.

| **Series de maquinas virtuales compatibles**  | **Restricciones** |
|------------|------------|
| D | Ninguno. |
| Dv2 | Ninguno. |  
| Dv3 | Ninguno.|
| DSv2 | Ninguno. | 
| DSv3 | Ninguno.|
| FSv2 | Ninguno. | 
| HBv2 | Requiere aprobación. |  
| HCS | Requiere aprobación. |  
| M | Requiere aprobación. |
| NC | Ninguno. |    
| NCsv2 | Requiere aprobación. |
| NCsv3 | Requiere aprobación. |  
| NDs | Requiere aprobación. |
| NDv2 | Requiere aprobación. |
| NV | Ninguno. |
| NVv3 | Requiere aprobación. | 


Aunque Azure Machine Learning admite estas series de máquinas virtuales, puede que no estén disponibles en todas las regiones de Azure. Para comprobar si las series de máquinas virtuales están disponibles o no, consulte [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines).

### <a name="compute-isolation"></a>Aislamiento de proceso

El proceso de Azure Machine Learning ofrece tamaños de máquina virtual que están aislados para un tipo concreto de hardware y dedicados a un solo cliente. Los tamaños de máquina virtual aislados son más adecuados para cargas de trabajo que requieren un alto grado de aislamiento de otros clientes por motivos como, por ejemplo, el cumplimiento normativo y de requisitos legales. Usar un tamaño aislado garantiza que la máquina virtual será la única que se ejecute en esa instancia de servidor específica.

Las ofertas de máquinas virtuales aisladas actuales incluyen:

* Standard_M128ms
* Standard_F72s_v2
* Standard_NC24s_v3
* Standard_NC24rs_v3*

*Compatible con RDMA

Para más información sobre el aislamiento, consulte [Aislamiento en la nube pública de Azure](../security/fundamentals/isolation-choices.md).

## <a name="unmanaged-compute"></a>Proceso no administrado

Azure Machine Learning *no* administra un destino de proceso no administrado. Cree este tipo de destino de proceso fuera de Azure Machine Learning y luego adjúntelo al área de trabajo. Estos recursos de proceso no administrados pueden requerir pasos adicionales para mantener o mejorar el rendimiento de las cargas de trabajo de Machine Learning.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo:
* [Usar un destino de proceso para entrenar el modelo](how-to-set-up-training-targets.md)
* [Implementar el modelo para un destino de proceso](how-to-deploy-and-where.md)