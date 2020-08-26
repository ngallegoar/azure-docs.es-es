---
title: Creación de conjuntos de datos con Azure Open Datasets
titleSuffix: Azure Open Datasets
description: Aprenda a crear un conjunto de datos de Azure Machine Learning en Azure Open Datasets.
ms.service: open-datasets
ms.topic: conceptual
ms.author: nibaccam
author: nibaccam
ms.date: 08/05/2020
ms.custom: how-to, tracking-python
ms.openlocfilehash: e6f3a541f1e9dbca2c9949fb0c5cde28cd43e8e5
ms.sourcegitcommit: faeabfc2fffc33be7de6e1e93271ae214099517f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2020
ms.locfileid: "88183069"
---
# <a name="create-azure-machine-learning-datasets-from-azure-open-datasets"></a>Creación de conjuntos de datos de Azure Machine Learning en Azure Open Datasets
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este artículo, aprenderá a crear un conjunto de datos de Azure Machine Learning en [Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/) con el fin de acceder a los datos de los experimentos locales o remotos. 

Mediante la creación de un [conjunto de datos de Azure Machine Learning](../machine-learning/how-to-create-register-datasets.md), creará una referencia a la ubicación del origen de datos, junto con una copia de sus metadatos. Dado que los datos permanecen en su ubicación existente, no incurre en ningún costo de almacenamiento adicional ni arriesga intencionadamente la integridad de los orígenes de datos. Además, los conjuntos de datos se evalúan de forma diferida, lo que contribuye a la velocidad del rendimiento del flujo de trabajo.
 
Para comprender el lugar de los almacenes de datos en el flujo de trabajo global de acceso a datos de Azure Machine Learning, consulte el artículo [Acceso seguro a los datos](../machine-learning/concept-data.md#data-workflow).


Los conjuntos de datos abiertos están en la nube en Microsoft Azure y se incluyen tanto en el [SDK de Azure Machine Learning para Python](#create-datasets-with-the-sdk) como en [Azure Machine Learning Studio](#create-datasets-with-the-studio).

## <a name="why-use-azure-open-datasets"></a>¿Por qué usar Azure Open Datasets? 

Azure Open Datasets son conjuntos de datos públicos mantenidos que puede usar para agregar características de escenarios específicos a soluciones de aprendizaje automático a fin de obtener modelos más precisos. Los conjuntos de datos incluyen datos de dominio público para el clima, censos, días festivos, seguridad pública y ubicación, que le ayudarán a entrenar los modelos de Machine Learning y enriquecer las soluciones predictivas. 

Para más información, consulte [¿Qué son los conjuntos de datos abiertos?](overview-what-are-open-datasets.md)

## <a name="prerequisites"></a>Requisitos previos

Para crear y trabajar con conjuntos de datos, necesita:

* Suscripción a Azure. Si no tiene una, cree una cuenta gratuita antes de empezar. Pruebe la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

* Un [área de trabajo de Azure Machine Learning](../machine-learning/how-to-manage-workspace.md).

* El [SDK de Azure Machine Learning para Python instalado](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), que incluye el paquete azureml-datasets.

    * Cree una [instancia de proceso de Azure Machine Learning](../machine-learning/concept-compute-instance.md#managing-a-compute-instance) que sea un entorno de desarrollo completamente configurado y administrado que incluya cuadernos integrados y el SDK ya instalado.

    **OR**

    * Trabaje en su propio cuaderno de Jupyter Notebook e instale el SDK con [estas instrucciones](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

> [!NOTE]
> Algunas clases de conjunto de tipos tienen dependencias en el paquete [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py), que solo es compatible con Python de 64 bits. Para los usuarios de Linux, estas clases solo se admiten en las siguientes distribuciones:  Red Hat Enterprise Linux (7, 8), Ubuntu (14.04, 16.04, 18.04), Fedora (27, 28), Debian (8, 9) y CentOS (7).

## <a name="create-datasets-with-the-sdk"></a>Creación de conjuntos de datos con el SDK

Para crear conjuntos de datos mediante clases de Open Datasets en el SDK de Azure Machine Learning para Python, el paquete se debe haber instalado con `pip install azureml-opendatasets`. Cada conjunto de datos discretos se representa mediante su propia clase en el SDK y ciertas clases están disponibles como `TabularDataset`, `FileDataset`, o ambos. Consulte la [documentación de referencia](https://docs.microsoft.com/python/api/azureml-opendatasets/azureml.opendatasets?view=azure-ml-py) para obtener una lista completa de clases.

Puede recuperar ciertas clases como `TabularDataset` o `FileDataset`, lo que le permite manipular y descargar los archivos directamente. Otras clases **solo** pueden obtener un conjunto de datos mediante las funciones `get_tabular_dataset()` o `get_file_dataset()`. En el ejemplo de código siguiente se muestran algunos ejemplos de estos tipos de clases.

```python
from azureml.opendatasets import MNIST

# MNIST class can return either TabularDataset or FileDataset
tabular_dataset = MNIST.get_tabular_dataset()
file_dataset = MNIST.get_file_dataset()

from azureml.opendatasets import Diabetes

# Diabetes class can return ONLY TabularDataset and must be called from the static function
diabetes_tabular = Diabetes.get_tabular_dataset()
```

Cuando se registra un conjunto de datos creado desde Open Datasets, los datos no se descargan inmediatamente, pero se tendrá acceso a ellos más adelante cuando se soliciten (durante el entrenamiento, por ejemplo) desde una ubicación de almacenamiento central.

## <a name="create-datasets-with-the-studio"></a>Creación de conjuntos de datos con Studio

También se pueden crear conjuntos de datos de Open Datasets con [Azure Machine Learning Studio](https://ml.azure.com).

1. En el área de trabajo, seleccione la pestaña **Conjunto de datos** en **Recursos**. En el menú desplegable **Crear conjunto de datos**, seleccione **From Open Datasets** (Desde Open Datasets).

    ![Abrir un conjunto de datos con la interfaz de usuario](./media/how-to-create-dataset-from-open-dataset/open-datasets-1.png)

1. Seleccione un conjunto de datos seleccionando su icono (tiene la opción de filtrar mediante la barra de búsqueda). Seleccione **Next** (Siguiente).

    ![Elegir conjunto de datos](./media/how-to-create-dataset-from-open-dataset/open-datasets-2.png)

1. Elija el nombre con que va a registrar el conjunto de datos y, opcionalmente, filtre los datos mediante los filtros disponibles. En este caso, para el conjunto de datos de **días festivos públicos**, filtre el período a un año y el código de país a solo EE. UU. Seleccione **Crear**.

    ![Establecer parámetros del conjunto de datos y crear conjunto de datos](./media/how-to-create-dataset-from-open-dataset/open-datasets-3.png)

    El conjunto de valores ahora está disponible en su área de trabajo, en **Conjunto de datos**. Puede usarlo de la misma manera que otros conjuntos de valores que haya creado.


## <a name="access-datasets-for-your-experiments"></a>Acceso a los conjuntos de datos de los experimentos

Use sus conjuntos de datos en los experimentos de aprendizaje automático para entrenar modelos de aprendizaje automático. [Obtenga más información sobre cómo entrenar con conjuntos de datos](../machine-learning/how-to-train-with-datasets.md).

## <a name="example-notebooks"></a>Cuadernos de ejemplo

Puede encontrar ejemplos y demostraciones de la funcionalidad de Open Datasets en estos [cuadernos](https://github.com/Azure/OpenDatasetsNotebooks/tree/master/tutorials).

## <a name="next-steps"></a>Pasos siguientes

* [Entrenamiento de un modelo con conjuntos de datos](../machine-learning/how-to-train-with-datasets.md).

* [Creación de un conjunto de datos de Azure Machine Learning](../machine-learning/how-to-create-register-datasets.md).



