---
title: Desarrollo con AutoML y Azure Databricks
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo configurar un entorno de desarrollo en Azure Machine Learning y Azure Databricks. Use los SDK de Azure ML para Databricks y Databricks con ML automatizado.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.date: 10/21/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: ef8ee7718aabb443fda6cd7b276ee53472261913
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423850"
---
# <a name="set-up-a-development-environment-with-azure-databricks-and-automl-in-azure-machine-learning"></a>Configuración de un entorno de desarrollo con Azure Databricks y AutoML en Azure Machine Learning 

Aprenda a configurar un entorno de desarrollo en Azure Machine Learning que usa Azure Databricks y ML automatizado.

Azure Databricks es idóneo para ejecutar flujos de trabajo de aprendizaje automático intensivos y a gran escala en la plataforma escalable de Apache Spark en la nube de Azure. Proporciona un entorno de colaboración basado en cuadernos con un clúster de proceso basado en CPU o GPU.

Para obtener información sobre otros entornos de desarrollo de aprendizaje automático, consulte [Configuración de un entorno de desarrollo para Azure Machine Learning](how-to-configure-environment.md).


## <a name="prerequisite"></a>Requisito previo

Área de trabajo de Azure Machine Learning. Si no tiene ninguna, puede crear un área de trabajo de Azure Machine Learning mediante [Azure Portal](how-to-manage-workspace.md), la [CLI de Azure](how-to-manage-workspace-cli.md#create-a-workspace) y las [plantillas de Azure Resource Manager](how-to-create-workspace-template.md).


## <a name="azure-databricks-with-azure-machine-learning-and-automl"></a>Azure Databricks con Azure Machine Learning y ML automatizado

Azure Databricks se integra con Azure Machine Learning y sus funcionalidades de AutoML. 

Puede usar Azure Databricks:

+ Para entrenar un modelo con Spark MLlib e implementar el modelo en ACI o AKS.
+ Con funcionalidades de [aprendizaje automático automatizado](concept-automated-ml.md) mediante un SDK de Azure ML.
+ Como destino de proceso desde una [canalización de Azure Machine Learning](concept-ml-pipelines.md).

## <a name="set-up-a-databricks-cluster"></a>Configuración de un clúster de Databricks

Cree un [clúster de Databricks](/azure/databricks/scenarios/quickstart-create-databricks-workspace-portal). Algunas opciones solo se aplican si instala el SDK para el aprendizaje automático automatizado en Databricks.

**Se tardan unos minutos en crear el clúster.**

Use estos valores de configuración:

| Configuración |Se aplica a| Value |
|----|---|---|
| Cluster Name |Siempre| nombredelclúster |
| Versión de Databricks Runtime |Siempre| Runtime 7.1 (scala 2.21, spark 3.0.0): no ML|
| Versión de Python |Siempre| 3 |
| Tipo de trabajo <br>(determina el número máximo de iteraciones simultáneas) |Machine Learning Automatizado<br>solo| Se prefieren las máquinas virtuales optimizadas para memoria |
| Trabajos |Siempre| 2 o más |
| Habilitar escalado automático |Machine Learning Automatizado<br>solo| Desactivar |

Espere hasta que el clúster se ejecute antes de continuar.

## <a name="add-the-azure-ml-sdk-to-databricks"></a>Incorporación del SDK de Azure ML a Databricks

Una vez que se esté ejecutando el clúster, [cree una biblioteca](https://docs.databricks.com/user-guide/libraries.html#create-a-library) para adjuntar al clúster el paquete del SDK adecuado de Azure Machine Learning. 

Para usar ML automatizado, vaya a [Incorporación del SDK de Azure ML con ML automatizado](#add-the-azure-ml-sdk-with-automl-to-databricks).


1. Haga clic con el botón derecho en la carpeta del área de trabajo actual en la que quiere almacenar la biblioteca. Seleccione **Crear** > **Biblioteca**.
    
    > [!TIP]
    > Si tiene una versión anterior del SDK, anule la selección de este desde las bibliotecas instaladas del clúster y muévalo a la Papelera. Instale la nueva versión del SDK y reinicie el clúster. Si hay un problema después del reinicio, desasocie y asocie nuevamente el clúster.

1. Elija la siguiente opción (no se admite ninguna otra instalación de SDK).

   |Elementos adicionales del&nbsp;paquete&nbsp;del SDK|Source|Nombre de&nbsp;PyPi&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
   |----|---|---|
   |Para Databricks| Cargar un huevo o PyPi de Python | azureml-sdk[databricks]|

   > [!WARNING]
   > No se pueden instalar otros elementos adicionales del SDK. Elija solo la opción [`databricks`].

   * No seleccione **Attach automatically to all clusters** (Asociar automáticamente a todos los clústeres).
   * Seleccione **Asociar** junto al nombre del clúster.

1. Supervise los errores hasta que el estado cambie a **Asociado**, lo que podría tardar varios minutos.  Si se produce un error en este paso:

   Pruebe a reiniciar el clúster de la manera siguiente:
   1. En el panel izquierdo, seleccione **Clústeres**.
   1. En la tabla, seleccione el nombre del clúster.
   1. En la ficha **Bibliotecas**, seleccione **Reiniciar**.

   Una instalación correcta tiene un aspecto similar al siguiente: 

  ![SDK de Azure Machine Learning para Databricks](./media/how-to-configure-environment/amlsdk-withoutautoml.jpg) 

## <a name="add-the-azure-ml-sdk-with-automl-to-databricks"></a>Incorporación del SDK de Azure ML con AutoML a Databricks
Si el clúster se creó con Databricks Runtime 7.1 o superior (*no* ML), ejecute el siguiente comando en la primera celda del cuaderno para instalar el SDK de AML.

```
%pip install --upgrade --force-reinstall -r https://aka.ms/automl_linux_requirements.txt
```
Para Databricks Runtime 7.0 o anterior, instale el SDK de Azure Machine Learning mediante el [script init](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks/automl/README.md).

### <a name="automl-config-settings"></a>Opciones de configuración de AutoML

En la configuración de AutoML, al usar Azure Databricks, agregue los siguientes parámetros:

- ```max_concurrent_iterations``` se basa en el número de nodos de trabajo del clúster.
- ```spark_context=sc``` se basa en el contexto de Spark predeterminado.

## <a name="ml-notebooks-that-work-with-azure-databricks"></a>Cuadernos de ML que funcionan con Azure Databricks

Pruebe lo siguiente:
+ Aunque hay muchos cuadernos de ejemplo disponibles, **solo [estos cuadernos de ejemplo](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks) funcionan con Azure Databricks.**

+ Importe estos ejemplos directamente desde el área de trabajo. Observe a continuación: ![Selección de Importar](./media/how-to-configure-environment/azure-db-screenshot.png)
![Panel Importar](./media/how-to-configure-environment/azure-db-import.png)

+ Obtenga información sobre cómo [crear una canalización con Databricks como proceso de entrenamiento](how-to-create-your-first-pipeline.md).

## <a name="next-steps"></a>Pasos siguientes

- [Entrenar un modelo](tutorial-train-models-with-aml.md) en Azure Machine Learning con el conjunto de datos de MNIST.
- Consulte [¿Qué es el SDK de Azure Machine Learning para Python?](/python/api/overview/azure/ml/intro?preserve-view=true&view=azure-ml-py)
