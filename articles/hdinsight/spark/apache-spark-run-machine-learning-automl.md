---
title: Ejecución de cargas de trabajo de Azure Machine Learning en Apache Spark en HDInsight
description: Aprenda a ejecutar cargas de trabajo de Azure Machine Learning con aprendizaje automático automatizado (AutoML) en Apache Spark en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.date: 12/13/2019
ms.openlocfilehash: 6e472c65897fa57cdb1e0b09d94c62913e268040
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86087479"
---
# <a name="run-azure-machine-learning-workloads-with-automated-machine-learning-on-apache-spark-in-hdinsight"></a>Ejecución de cargas de trabajo de Azure Machine Learning con aprendizaje automático automatizado en Apache Spark en HDInsight

Azure Machine Learning simplifica la creación y agiliza la creación, entrenamiento e implementación de modelos de Machine Learning. En el aprendizaje automático automatizado (AutoML), se comienza con datos de entrenamiento que tienen una característica de destino definida y después se realiza una iteración por las combinaciones de algoritmos y selecciones de características para elegir de forma automática el mejor modelo para los datos según las puntuaciones de entrenamiento. HDInsight permite que los clientes aprovisionen los clústeres con cientos de nodos. Cuando AutoML ejecuta Spark en un clúster de HDInsight, permite a los usuarios usar la capacidad de proceso de estos nodos para ejecutar trabajos de entrenamiento en escalado horizontal, así como ejecutar varios trabajos de entrenamiento en paralelo. Esto permite que los usuarios ejecuten experimentos de AutoML mientras comparten el proceso con sus otras cargas de trabajo de macrodatos.

## <a name="install-azure-machine-learning-on-an-hdinsight-cluster"></a>Instalación de Azure Machine Learning en un clúster de HDInsight

Para ver tutoriales generales sobre el aprendizaje automático automatizado, consulte el [Tutorial: Uso del aprendizaje automático para crear un modelo de regresión](../../machine-learning/tutorial-auto-train-models.md).
Todos los nuevos clústeres de Spark con HDInsight tienen preinstalado el SDK de Azure Machine Learning y AutoML.

> [!Note]
> Los paquetes de Azure Machine Learning se instalan en el entorno de Python 3 Conda. Se debe ejecutar el cuaderno de Jupyter Notebook instalado mediante el kernel PySpark3.

También puede usar cuadernos de Zeppelin Notebook para usar AutoML.

> [!Note]
> Zeppelin tiene un [problema conocido](https://community.hortonworks.com/content/supportkb/207822/the-livypyspark3-interpreter-uses-python-2-instead.html) en el que PySpark3 no elige la versión correcta de Python. Use la solución alternativa documentada.

## <a name="authentication-for-workspace"></a>Autenticación de áreas de trabajo

La creación de un área de trabajo y el envío de experimentos requieren un token de autenticación. Este token se puede generar mediante una [aplicación de Azure AD](../../active-directory/develop/app-objects-and-service-principals.md). Un [usuario de Azure AD](/azure/python/python-sdk-azure-authenticate) también puede usarse para generar el token de autenticación necesario en caso de que no esté habilitada la autenticación multifactor en la cuenta.  

El siguiente fragmento de código crea un token de autenticación mediante una **aplicación de Azure AD**.

```python
from azureml.core.authentication import ServicePrincipalAuthentication
auth_sp = ServicePrincipalAuthentication(
    tenant_id='<Azure Tenant ID>',
    service_principal_id='<Azure AD Application ID>',
    service_principal_password='<Azure AD Application Key>'
)
```

El siguiente fragmento de código crea un token de autenticación mediante un **usuario de Azure AD**.

```python
from azure.common.credentials import UserPassCredentials
credentials = UserPassCredentials('user@domain.com', 'my_smart_password')
```

## <a name="loading-dataset"></a>Carga de conjunto de datos

El aprendizaje automático automatizado en Spark usa **flujos de datos**, que son operaciones con evaluación diferida, inmutables, que se realizan en los datos.  Un flujo de datos puede cargar un conjunto de datos desde un blob con acceso de lectura público, o desde una dirección URL de un blob con un token de SAS.

```python
import azureml.dataprep as dprep

dataflow_public = dprep.read_csv(
    path='https://commonartifacts.blob.core.windows.net/automl/UCI_Adult_train.csv')

dataflow_with_token = dprep.read_csv(
    path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv?st=2018-06-15T23%3A01%3A42Z&se=2019-06-16T23%3A01%3A00Z&sp=r&sv=2017-04-17&sr=b&sig=ugQQCmeC2eBamm6ynM7wnI%2BI3TTDTM6z9RPKj4a%2FU6g%3D')
```

También puede registrar el almacén de datos con el área de trabajo mediante un registro de un solo uso.

## <a name="experiment-submission"></a>Realización de experimentos

En la [configuración del aprendizaje automático automatizado](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig), la propiedad `spark_context` debe establecerse para que el paquete se ejecute en modo distribuido. La propiedad `concurrent_iterations`, que es el número máximo de iteraciones que se ejecutan en paralelo, se debe establecer en un número menor que los núcleos de ejecutor de la aplicación de Spark.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre los motivos para usar el aprendizaje automático automatizado, consulte [Release models at pace using Microsoft’s automated machine learning!](https://azure.microsoft.com/blog/release-models-at-pace-using-microsoft-s-automl/) (Lanzamiento periódico de modelos mediante el aprendizaje automático automatizado de Microsoft).
* Para más información sobre el uso de las funcionalidades de aprendizaje automático de Azure ML, consulte [Nuevas funcionalidades automatizadas de aprendizaje automático en Azure Machine Learning](https://azure.microsoft.com/blog/new-automated-machine-learning-capabilities-in-azure-machine-learning-service/)
* [Proyecto de AutoML de Microsoft Research](https://www.microsoft.com/research/project/automl/)
