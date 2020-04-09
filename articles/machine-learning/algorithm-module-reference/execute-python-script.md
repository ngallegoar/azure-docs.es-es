---
title: 'Ejecución de script de Python: referencia para los módulos'
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo usar el módulo Ejecución de script de Python en Azure Machine Learning para ejecutar código de Python.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 03/10/2020
ms.openlocfilehash: 0f86d1ad03062797764af6a0d49beacaa3458a8f
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "80365562"
---
# <a name="execute-python-script-module"></a>Módulo Ejecutar script de Python

En este artículo se describe un módulo del diseñador de Azure Machine Learning (versión preliminar).

Utilice este módulo para ejecutar código de Python. Para obtener más información acerca de los principios de diseño y arquitectura de Python, consulte [el siguiente artículo.](https://docs.microsoft.com/azure/machine-learning/machine-learning-execute-python-scripts)

Con Python, puede realizar las tareas que no son compatibles actualmente con los módulos existentes, como:

+ Visualizar datos mediante `matplotlib`
+ Usar bibliotecas de Python para enumerar los conjuntos de datos y modelos en el área de trabajo
+ Leer, cargar y manipular datos de orígenes que no admitidos por el módulo [Import Data](./import-data.md) (Importar datos)
+ Ejecutar código de aprendizaje profundo propio 


Azure Machine Learning usa la distribución Anaconda de Python, que incluye muchas utilidades comunes para el procesamiento de datos. La versión de Anaconda se actualizará automáticamente. La versión actual es:
 -  Anaconda 4.5+ distribution for Python 3.6 

Los paquetes instalados previamente son:
-    adal==1.2.2
-    applicationinsights==0.11.9
-    attrs==19.3.0
-    azure-common==1.1.25
-    azure-core==1.3.0
-    azure-graphrbac==0.61.1
-    azure-identity==1.3.0
-    azure-mgmt-authorization==0.60.0
-    azure-mgmt-containerregistry==2.8.0
-    azure-mgmt-keyvault==2.2.0
-    azure-mgmt-resource==8.0.1
-    azure-mgmt-storage==8.0.0
-    azure-storage-blob==1.5.0
-    azure-storage-common==1.4.2
-    azureml-core==1.1.5.5
-    azureml-dataprep-native==14.1.0
-    azureml-dataprep==1.3.5
-    azureml-defaults==1.1.5.1
-    azureml-designer-classic-modules==0.0.118
-    azureml-designer-core==0.0.31
-    azureml-designer-internal==0.0.18
-    azureml-model-management-sdk==1.0.1b6.post1
-    azureml-pipeline-core==1.1.5
-    azureml-telemetry==1.1.5.3
-    backports.tempfile==1.0
-    backports.weakref==1.0.post1
-    boto3==1.12.29
-    botocore==1.15.29
-    cachetools==4.0.0
-    certifi==2019.11.28
-    cffi==1.12.3
-    chardet==3.0.4
-    click==7.1.1
-    cloudpickle==1.3.0
-    configparser==3.7.4
-    contextlib2==0.6.0.post1
-    cryptography==2.8
-    cycler==0.10.0
-    dill==0.3.1.1
-    distro==1.4.0
-    docker==4.2.0
-    docutils==0.15.2
-    dotnetcore2==2.1.13
-    flask==1.0.3
-    fusepy==3.0.1
-    gensim==3.8.1
-    google-api-core==1.16.0
-    google-auth==1.12.0
-    google-cloud-core==1.3.0
-    google-cloud-storage==1.26.0
-    google-resumable-media==0.5.0
-    googleapis-common-protos==1.51.0
-    gunicorn==19.9.0
-    idna==2.9
-    imbalanced-learn==0.4.3
-    isodate==0.6.0
-    itsdangerous==1.1.0
-    jeepney==0.4.3
-    jinja2==2.11.1
-    jmespath==0.9.5
-    joblib==0.14.0
-    json-logging-py==0.2
-    jsonpickle==1.3
-    jsonschema==3.0.1
-    kiwisolver==1.1.0
-    liac-arff==2.4.0
-    lightgbm==2.2.3
-    markupsafe==1.1.1
-    matplotlib==3.1.3
-    more-itertools==6.0.0
-    msal-extensions==0.1.3
-    msal==1.1.0
-    msrest==0.6.11
-    msrestazure==0.6.3
-    ndg-httpsclient==0.5.1
-    nimbusml==1.6.1
-    numpy==1.18.2
-    oauthlib==3.1.0
-    pandas==0.25.3
-    pathspec==0.7.0
-    pip==20.0.2
-    portalocker==1.6.0
-    protobuf==3.11.3
-    pyarrow==0.16.0
-    pyasn1-modules==0.2.8
-    pyasn1==0.4.8
-    pycparser==2.20
-    pycryptodomex==3.7.3
-    pyjwt==1.7.1
-    pyopenssl==19.1.0
-    pyparsing==2.4.6
-    pyrsistent==0.16.0
-    python-dateutil==2.8.1
-    pytz==2019.3
-    requests-oauthlib==1.3.0
-    requests==2.23.0
-    rsa==4.0
-    ruamel.yaml==0.15.89
-    s3transfer==0.3.3
-    scikit-learn==0.22.2
-    scipy==1.4.1
-    secretstorage==3.1.2
-    setuptools==46.1.1.post20200323
-    six==1.14.0
-    smart-open==1.10.0
-    urllib3==1.25.8
-    websocket-client==0.57.0
-    werkzeug==0.16.1
-    wheel==0.34.2

 Para instalar otros paquetes que no están en la lista instalada previamente, por ejemplo *scikit-misc*, agregue el código siguiente al script: 

 ```python
import os
os.system(f"pip install scikit-misc")
```

## <a name="upload-files"></a>Carga de archivos
**Ejecución de script de Python** admite la carga de archivos con el [SDK de Python de Azure Machine Learning](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py#upload-file-name--path-or-stream-).

En el ejemplo siguiente se muestra cómo cargar un archivo de imagen en **Ejecución de script de Python**:

```Python

# The script MUST contain a function named azureml_main
# which is the entry point for this module.

# imports up here can be used to
import pandas as pd

# The entry point function can contain up to two input arguments:
#   Param<dataframe1>: a pandas.DataFrame
#   Param<dataframe2>: a pandas.DataFrame
def azureml_main(dataframe1 = None, dataframe2 = None):

    # Execution logic goes here
    print(f'Input pandas.DataFrame #1: {dataframe1}')

    from matplotlib import pyplot as plt
    plt.plot([1, 2, 3, 4])
    plt.ylabel('some numbers')
    img_file = "line.png"
    plt.savefig(img_file)

    from azureml.core import Run
    run = Run.get_context(allow_offline=True)
    run.upload_file(f"graphics/{img_file}", img_file)

    # Return value must be of a sequence of pandas.DataFrame
    # E.g.
    #   -  Single return value: return dataframe1,
    #   -  Two return values: return dataframe1, dataframe2
    return dataframe1,
}
```

Una vez que termina la ejecución de la canalización, se puede obtener una vista previa de la imagen en el panel derecho del módulo.

> [!div class="mx-imgBorder"]
> ![Imagen cargada](media/module/upload-image-in-python-script.png).

## <a name="how-to-configure-execute-python-script"></a>Procedimiento para configurar Ejecución de script de Python

El módulo **Ejecución de script de Python** contiene código de Python de ejemplo que puede usar como punto de partida. Para configurar el módulo **Ejecución de script de Python**, debe proporcionar un conjunto de entradas y código de Python que se ejecutará en el cuadro de texto **Python script** (Script de Python).

1. Agregue el módulo **Execute Python Script** (Ejecutar script de Python) al experimento.

2. Agregue y conecte en **Dataset1** los conjuntos de datos del diseñador que quiera usar para la entrada. Haga referencia a este conjunto de datos en el script de Python como **DataFrame1**.

    El uso de un conjunto de datos es opcional, si desea generar datos mediante Python, o usar código de Python para importar los datos directamente al módulo.

    Este módulo admite la adición de un segundo conjunto de datos en **Dataset2**. Haga referencia al segundo conjunto de datos en el script de Python como DataFrame2.

    Los conjuntos de datos almacenados en Azure Machine Learning se convierten automáticamente en data.frames de **pandas** cuando se cargan con este módulo.

    ![Ejecución de asignación de entrada de Python](media/module/python-module.png)

4. Para incluir nuevos paquetes o código de Python, agregue el archivo comprimido que contiene estos recursos personalizados a **Script bundle** (Agrupación de scripts). La entrada para **Script bundle** (Agrupación de scripts) debe ser un archivo comprimido cargado en el área de trabajo como conjunto de datos de tipo de archivo. Se puede cargar el conjunto de datos en la página de recursos **Conjuntos de datos**, y arrastrar y colocar el módulo de conjunto de datos desde la lista **My datasets** (Mis conjuntos de datos) del árbol del módulo situado a la izquierda en la página de creación del diseñador. 

    Cualquier archivo incluido en el archivo comprimido cargado puede usarse durante la ejecución de la canalización. Si el archivo incluye una estructura de directorios, se conserva la estructura, pero debe anteponerse un directorio llamado **src** a la ruta de acceso.

5. En el cuadro de texto **Python Script** (Script de Python), escriba o pegue el script de Python válido.

    El cuadro de texto **Python Script** (Script de Python) se rellena previamente con algunas instrucciones de comentarios y código de ejemplo para el acceso a datos y salida. Debe editar o reemplazar este código. Asegúrese de seguir las convenciones de Python sobre la aplicación de sangría y mayúsculas y minúsculas.

    + El script debe contener una función denominada `azureml_main` como punto de entrada para este módulo.
    + La función de punto de entrada puede contener hasta dos argumentos de entrada: `Param<dataframe1>` y `Param<dataframe2>`
    + Los archivos comprimidos conectados al tercer puerto de entrada se descomprimen y almacenan en el directorio, `.\Script Bundle`, que también se agrega a `sys.path` de Python. 

    Por lo tanto, si el archivo ZIP contiene `mymodule.py`, impórtelo mediante `import mymodule`.

    + Se pueden devolver dos conjuntos de datos al diseñador, que deben ser una secuencia de tipo `pandas.DataFrame`. Puede crear otras salidas en el código de Python y escribirlas directamente en Azure Storage.

6. Envíe la canalización, o seleccione el módulo y haga clic en **Run selected** (Ejecutar seleccionados) para ejecutar solo el script de Python.

    Todos los datos y el código se cargan en una máquina virtual y se ejecutan con el entorno de Python especificado.

## <a name="results"></a>Results

Los resultados de todos los cálculos realizados por el código de Python insertado deben proporcionarse como pandas.DataFrame, que se convierte automáticamente en el formato de conjunto de datos de Azure Machine Learning, por lo que puede usar los resultados con otros módulos en la canalización.

El módulo devuelve dos conjuntos de datos:  
  
+ **Conjunto de datos de resultados 1**, definido por el primer dataframe de pandas devuelto en el script de Python

+ **Conjunto de datos de resultados 2**, definido por el segundo dataframe de pandas devuelto en el script de Python


## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning. 