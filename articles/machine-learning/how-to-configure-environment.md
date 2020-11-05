---
title: Configuración del entorno de desarrollo | Python
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo configurar un entorno de desarrollo de Python para Azure Machine Learning. Use entornos Conda para crear archivos de configuración y configurar su propio servidor de cuadernos basado en la nube, Jupyter Notebook, Azure Databricks, IDE, editores de código y Data Science Virtual Machine.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.date: 09/30/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python, contperfq1, devx-track-azurecli
ms.openlocfilehash: 7e189885fbf7befcaea3f63148a42c81dc1da03e
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93320485"
---
# <a name="set-up-a-development-environment-for-azure-machine-learning"></a>Configuración de un entorno de desarrollo para Azure Machine Learning

Descubra cómo configurar un entorno de desarrollo de Python para Azure Machine Learning.

En la tabla siguiente se muestra cada entorno de desarrollo que se trata en este artículo, junto con las ventajas y desventajas.

| Entorno | Ventajas | Desventajas |
| --- | --- | --- |
| [Entorno local](#local) | Control total del entorno de desarrollo y las dependencias. Funciona con cualquier herramienta de compilación, entorno o IDE de su elección. | Tarda más tiempo en comenzar. Se deben instalar los paquetes de SDK necesarios y también debe instalarse un entorno si aún no tiene uno. |
| [Instancia de proceso de Azure Machine Learning](#compute-instance) | Manera más fácil de empezar. El SDK completo ya está instalado en la máquina virtual del área de trabajo y los tutoriales de cuaderno están clonados previamente y listos para ejecutarse. | Falta de control sobre el entorno de desarrollo y las dependencias. Costo adicional por la máquina virtual Linux (la máquina virtual se puede detener cuando no se use para evitar cargos). Consulte los [detalles de los precios](https://azure.microsoft.com/pricing/details/virtual-machines/linux/). |
| [Azure Databricks](#aml-databricks) | Perfecto para ejecutar flujos de trabajo de aprendizaje automático intensivos y a gran escala en la plataforma escalable de Apache Spark. | Excesivo para aprendizaje automático experimental o experimentos y flujos de trabajo a pequeña escala. Costo adicional por Azure Databricks. Consulte los [detalles de los precios](https://azure.microsoft.com/pricing/details/databricks/). |
| [Data Science Virtual Machine (DSVM)](#dsvm) | De forma parecida a la instancia de proceso basada en la nube (con Python y el SDK preinstalados), pero con herramientas adicionales de ciencia de datos y aprendizaje automático conocidas instaladas previamente. Fácil de escalar y combinar con otras herramientas y flujos de trabajo personalizados. | Una experiencia de inicio más lenta en comparación con la instancia de proceso basada en la nube. |

En este artículo también se proporcionan sugerencias de uso adicionales para las siguientes herramientas:

* Jupyter Notebook: si ya usa instancias de Jupyter Notebook, el SDK incluye algunos elementos adicionales que debe instalar.

* Visual Studio Code: Si usa Visual Studio Code, la [extensión de Azure Machine Learning](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai) incluye un amplio soporte de lenguaje para Python, así como características para que trabajar con Azure Machine Learning sea mucho más cómodo y productivo.

## <a name="prerequisites"></a>Requisitos previos

* Área de trabajo de Azure Machine Learning. Si no tiene ninguna, puede crear un área de trabajo de Azure Machine Learning a través de [Azure Portal](how-to-manage-workspace.md), la [CLI de Azure](how-to-manage-workspace-cli.md#create-a-workspace) y las [plantillas de Azure Resource Manager](how-to-create-workspace-template.md).

### <a name="local-and-dsvm-only-create-a-workspace-configuration-file"></a><a id="workspace"></a> (Solo local y DSVM) Creación de un archivo de configuración del área de trabajo

El archivo de configuración del área de trabajo es un archivo JSON que le indica al SDK cómo comunicarse con el área de trabajo de Azure Machine Learning. El archivo se denomina *config.json* y tiene el formato siguiente:

```json
{
    "subscription_id": "<subscription-id>",
    "resource_group": "<resource-group>",
    "workspace_name": "<workspace-name>"
}
```

Este archivo JSON debe estar en la estructura de directorios que contiene los scripts de Python o las instancias de Jupyter Notebook. Puede estar en el mismo directorio, en un subdirectorio denominado *.azureml* o en un directorio principal.

Para utilizar este archivo desde el código, utilice el método [`Workspace.from_config`](/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#from-config-path-none--auth-none---logger-none---file-name-none-&preserve-view=true). Este código carga la información del archivo y se conecta a su área de trabajo.

Cree un archivo de configuración del área de trabajo con uno de los métodos siguientes:

* Azure portal

    **Descargue el archivo** : En [Azure Portal](https://ms.portal.azure.com), seleccione **Descargar config.json** desde la sección **Información general** del área de trabajo.

    ![Azure portal](./media/how-to-configure-environment/configure.png)

* SDK de Python de Azure Machine Learning

    Cree un script para conectarse a su área de trabajo de Azure Machine Learning y use el método [`write_config`](/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#write-config-path-none--file-name-none-&preserve-view=true) para generar el archivo y guárdelo como *.azureml/config.json*. Asegúrese de reemplazar `subscription_id`, `resource_group` y `workspace_name` por sus propios valores.

    ```python
    from azureml.core import Workspace

    subscription_id = '<subscription-id>'
    resource_group  = '<resource-group>'
    workspace_name  = '<workspace-name>'

    try:
        ws = Workspace(subscription_id = subscription_id, resource_group = resource_group, workspace_name = workspace_name)
        ws.write_config()
        print('Library configuration succeeded')
    except:
        print('Workspace not found')
    ```

## <a name="local-computer"></a><a id="local"></a>Equipo local

Para configurar un entorno de desarrollo local (que también podría ser una máquina virtual remota, como una instancia de proceso de Azure Machine Learning o DSVM):

1. Cree un entorno virtual de Python (virtualenv, conda).

    > [!NOTE]
    > Aunque no es necesario, se recomienda usar [Anaconda](https://www.anaconda.com/download/) o [Miniconda](https://www.anaconda.com/download/) para administrar entornos virtuales de Python e instalar paquetes.

    > [!IMPORTANT]
    > Si usa Linux o Mac OS y un shell distinto de bash (por ejemplo, zsh), es posible que reciba errores al ejecutar algunos comandos. Para solucionar este problema, use el comando `bash` para iniciar un nuevo shell de bash y ejecutar sus comandos.

1. Active el entorno virtual de Python recién creado.
1. Instale el [SDK de Azure Machine Learning para Python](/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py).
1. Con el fin de configurar el entorno local para usar el área de trabajo de Azure Machine Learning, [cree un archivo de configuración del área de trabajo](#workspace) o use uno que ya tenga.

Ahora que ha configurado el entorno local, ya podrá empezar a trabajar con Azure Machine Learning. Consulte la [guía de introducción de Azure Machine Learning para Python](tutorial-1st-experiment-sdk-setup-local.md) para comenzar.

### <a name="jupyter-notebooks"></a><a id="jupyter"></a>Jupyter Notebook

Al ejecutar un servidor de Jupyter Notebook local, se recomienda crear un kernel de IPython para el entorno virtual de Python. Esto ayuda a garantizar el comportamiento esperado de la importación del paquete y el kernel.

1. Habilitación de los kernels de IPython específicos del entorno

    ```bash
    conda install notebook ipykernel
    ```

1. Cree un kernel para el entorno virtual de Python. Asegúrese de reemplazar `<myenv>` por el nombre de su entorno virtual de Python.

    ```bash
    ipython kernel install --user --name <myenv> --display-name "Python (myenv)"
    ```

1. Inicio del servidor de Jupyter Notebook

Consulte el [repositorio de cuadernos de Azure Machine Learning](https://github.com/Azure/MachineLearningNotebooks) para empezar a trabajar con Azure Machine Learning y Jupyter Notebook.

> [!NOTE]
> Puede encontrar un repositorio de ejemplos administrado por la comunidad en https://github.com/Azure/azureml-examples.

### <a name="visual-studio-code"></a><a id="vscode"></a>Visual Studio Code

Para usar Visual Studio Code para el desarrollo:

1. Instale [Visual Studio Code](https://code.visualstudio.com/Download).
1. Instale la [extensión Azure Machine Learning para Visual Studio Code](tutorial-setup-vscode-extension.md) (versión preliminar).

Una vez instalada la extensión de Visual Studio Code, puede administrar los [recursos de Azure Machine Learning](how-to-manage-resources-vscode.md), [ejecutar y depurar los experimentos](how-to-debug-visual-studio-code.md) e [implementar modelos entrenados](tutorial-train-deploy-image-classification-model-vscode.md).

## <a name="azure-machine-learning-compute-instance"></a><a id="compute-instance"></a>Instancia de Proceso de Azure Machine Learning

La [instancia de proceso](concept-compute-instance.md) de Azure Machine Learning es una estación de trabajo de Azure segura y basada en la nube que proporciona a los científicos de datos un servidor de Jupyter Notebook, JupyterLab y un entorno de aprendizaje automático totalmente administrado.

No hay nada que instalar o configurar para una instancia de proceso.  

Cree una en cualquier momento desde el área de trabajo de Azure Machine Learning. Basta con que proporcione un nombre y especifique un tipo de máquina virtual de Azure. Pruébelo ahora con este [Tutorial: Configuración del entorno y el área de trabajo](tutorial-1st-experiment-sdk-setup.md).

Para obtener más información acerca de las instancias de proceso, incluyendo cómo instalar paquetes, consulte [Creación y administración de una instancia de proceso de Azure Machine Learning](how-to-create-manage-compute-instance.md).

> [!TIP]
> Para evitar incurrir en cargos por una instancia de proceso no usada, [detenga la instancia de proceso](how-to-create-manage-compute-instance.md#manage).

Además de un servidor de Jupyter Notebook y JupyterLab, puede usar instancias de proceso en la [característica de cuadernos integrada en Estudio de Azure Machine Learning](how-to-run-jupyter-notebooks.md).

También puede usar la extensión de Visual Studio Code de Azure Machine Learning para [configurar una instancia de proceso de Azure Machine Learning como un servidor de Jupyter Notebook remoto](how-to-set-up-vs-code-remote.md#configure-compute-instance-as-remote-notebook-server).

## <a name="data-science-virtual-machine"></a><a id="dsvm"></a>Data Science Virtual Machine

DSVM es una imagen de máquina virtual personalizada. Está diseñada para realizar trabajos de ciencia de datos que estén preconfigurados con herramientas y software como:

  - Paquetes, como TensorFlow, PyTorch, Scikit-learn, XGBoost y el SDK de Azure Machine Learning
  - Herramientas de ciencia de datos populares, como Spark Standalone o Drill
  - Herramientas de Azure, como la CLI de Azure, AzCopy y el Explorador de Storage
  - Entornos de desarrollo integrado (IDE), como Visual Studio Code y PyCharm
  - Servidor de Jupyter Notebook

Para ver una lista más completa de las herramientas, consulte la [guía de herramientas incluidas de DSVM](data-science-virtual-machine/tools-included.md).

> [!IMPORTANT]
> Si planea usar la instancia de DSVM como [destino de proceso](concept-compute-target.md) para sus trabajos de entrenamiento o inferencia, solo se admite Ubuntu.

Para usar la instancia de DSVM como entorno de desarrollo:

1. Cree una instancia de DSVM con uno de los métodos siguientes:

    * Use Azure Portal para crear una instancia de DSVM de [Ubuntu](data-science-virtual-machine/dsvm-ubuntu-intro.md) o [Windows](data-science-virtual-machine/provision-vm.md).
    * [Cree una instancia de DSVM con plantillas de Resource Manager](data-science-virtual-machine/dsvm-tutorial-resource-manager.md).
    * Uso de la CLI de Azure

        Para crear una instancia de DSVM de Ubuntu, use el siguiente comando:

        ```azurecli-interactive
        # create a Ubuntu DSVM in your resource group
        # note you need to be at least a contributor to the resource group in order to execute this command successfully
        # If you need to create a new resource group use: "az group create --name YOUR-RESOURCE-GROUP-NAME --location YOUR-REGION (For example: westus2)"
        az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:linux-data-science-vm-ubuntu:linuxdsvmubuntu:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --generate-ssh-keys --authentication-type password
        ```

        Para crear una instancia de DSVM de Windows, use el siguiente comando:

        ```azurecli-interactive
        # create a Windows Server 2016 DSVM in your resource group
        # note you need to be at least a contributor to the resource group in order to execute this command successfully
        az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:dsvm-windows:server-2016:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --authentication-type password
        ```

1. Active el entorno de conda que contiene el SDK de Azure Machine Learning.

    * Para la instancia de DSVM de Ubuntu:

        ```bash
        conda activate py36
        ```

    * Para la instancia de DSVM de Windows:

        ```bash
        conda activate AzureML
        ```

1. Para configurar la instancia de DSVM que usará el área de trabajo de Azure Machine Learning, [cree un archivo de configuración del área de trabajo](#workspace) o use uno que ya tenga.

De forma similar a los entornos locales, puede usar Visual Studio Code y la [extensión de Azure Machine Learning para Visual Studio Code](#vscode) con el fin de interactuar con Azure Machine Learning.

Consulte [Data Science Virtual Machine](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/) para obtener más información.

## <a name="azure-databricks"></a><a name="aml-databricks"></a> Azure Databricks

Azure Databricks es un entorno basado en Apache Spark de la nube de Azure. Proporciona un entorno de colaboración basado en cuadernos con un clúster de proceso basado en CPU o GPU.

Funcionamiento de Azure Databricks con Azure Machine Learning:

+ Puede entrenar un modelo con Spark MLlib e implementar el modelo en ACI o AKS desde Azure Databricks.
+ También puede usar funcionalidades de [aprendizaje automático automatizado](concept-automated-ml.md) en un SDK de Azure ML especial con Azure Databricks.
+ Puede usar Azure Databricks como destino de proceso desde una [canalización de Azure Machine Learning](concept-ml-pipelines.md).

### <a name="set-up-your-databricks-cluster"></a>Configuración del clúster de Databricks

Cree un [clúster de Databricks](/azure/databricks/scenarios/quickstart-create-databricks-workspace-portal). Algunas opciones solo se aplican si instala el SDK para el aprendizaje automático automatizado en Databricks.
**La operación de creación del clúster tardará unos minutos.**

Use estos valores de configuración:

| Configuración |Se aplica a| Value |
|----|---|---|
| Nombre del clúster |Siempre| nombredelclúster |
| Entorno de tiempo de ejecución de Databricks |Siempre|Runtime no de ML 7.1 (Scala 2.21, Spark 3.0.0) |
| Versión de Python |Siempre| 3 |
| Trabajos |Siempre| 2 o más |
| Tipos de máquinas virtuales con nodos de trabajo <br>(determina el número máximo de iteraciones simultáneas) |Machine Learning Automatizado<br>solo| Se prefieren las máquinas virtuales optimizadas para memoria |
| Habilitar escalado automático |Machine Learning Automatizado<br>solo| Desactivar |

Espere hasta que el clúster se ejecute antes de continuar.

### <a name="install-the-correct-sdk-into-a-databricks-library"></a>Instalación del SDK correcto en una biblioteca de Databricks

Una vez que se esté ejecutando el clúster, [cree una biblioteca](https://docs.databricks.com/user-guide/libraries.html#create-a-library) para adjuntar al clúster el paquete del SDK adecuado de Azure Machine Learning. En el caso de los ML de aprendizaje automático, vaya a la sección [SDK para Databricks con aprendizaje automático automatizado](#sdk-for-databricks-with-automated-machine-learning).

1. Haga clic con el botón derecho en la carpeta del área de trabajo actual en la que quiere almacenar la biblioteca. Seleccione **Crear** > **Biblioteca**.

1. Elija la siguiente opción (no se admite ninguna otra instalación de SDK).

   |Elementos adicionales del&nbsp;paquete&nbsp;del SDK|Source|Nombre de&nbsp;PyPi&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
   |----|---|---|
   |Para Databricks| Cargar un huevo o PyPi de Python | azureml-sdk[databricks]|

   > [!Warning]
   > No se pueden instalar otros elementos adicionales del SDK. Elija solo la opción [`databricks`].

   * No seleccione **Attach automatically to all clusters** (Asociar automáticamente a todos los clústeres).
   * Seleccione **Asociar** junto al nombre del clúster.

1. Supervise los errores hasta que el estado cambie a **Asociado** , lo que podría tardar varios minutos.  Si se produce un error en este paso:

   Pruebe a reiniciar el clúster de la manera siguiente:
   1. En el panel izquierdo, seleccione **Clústeres**.
   1. En la tabla, seleccione el nombre del clúster.
   1. En la ficha **Bibliotecas** , seleccione **Reiniciar**.

   También debe tener en cuenta lo siguiente:
   + En la configuración de AutoML, al usar Azure Databricks, agregue los siguientes parámetros:
       1. ```max_concurrent_iterations``` se basa en el número de nodos de trabajo del clúster.
        2. ```spark_context=sc``` se basa en el contexto de Spark predeterminado.
   + O bien, si tiene una versión anterior del SDK, anule la selección de este desde las bibliotecas instaladas del clúster y muévalo a la Papelera. Instale la nueva versión del SDK y reinicie el clúster. Si hay un problema después del reinicio, desasocie y asocie nuevamente el clúster.

Si la instalación fue correcta, la biblioteca importada debería tener un aspecto similar a uno de los ejemplos siguientes:

#### <a name="sdk-for-databricks"></a>SDK para Databricks
![SDK de Azure Machine Learning para Databricks](./media/how-to-configure-environment/amlsdk-withoutautoml.jpg)

#### <a name="sdk-for-databricks-with-automated-machine-learning"></a>SDK para Databricks con aprendizaje automático automatizado
Si el clúster se creó con el runtime de Databricks no de ML 7.1 o superior, ejecute el siguiente comando en la primera celda del cuaderno para instalar el SDK de AML.

```
%pip install --upgrade --force-reinstall -r https://aka.ms/automl_linux_requirements.txt
```
Para el runtime de Databricks no de ML 7.0 o anterior, instale el SDK de AML mediante el [script init](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks/automl/README.md).


### <a name="start-exploring"></a>Comienzo de la exploración

Pruebe lo siguiente:
+ Aunque hay muchos cuadernos de ejemplo disponibles, **solo [estos cuadernos de ejemplo](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks) funcionan con Azure Databricks.**

+ Importe estos ejemplos directamente desde el área de trabajo. Observe a continuación: ![Selección de Importar](./media/how-to-configure-environment/azure-db-screenshot.png)
![Panel Importar](./media/how-to-configure-environment/azure-db-import.png)

+ Obtenga información sobre cómo [crear una canalización con Databricks como proceso de entrenamiento](how-to-create-your-first-pipeline.md).

## <a name="next-steps"></a>Pasos siguientes

- [Entrenar un modelo](tutorial-train-models-with-aml.md) en Azure Machine Learning con el conjunto de datos de MNIST
- Consultar las referencias del [SDK de Azure Machine Learning para Python](/python/api/overview/azure/ml/intro?preserve-view=true&view=azure-ml-py)