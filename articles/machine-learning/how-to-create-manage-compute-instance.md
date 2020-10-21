---
title: Creación y administración de una instancia de proceso
titleSuffix: Azure Machine Learning
description: Aprenda a crear y administrar una instancia de proceso en el área de trabajo de Azure Machine Learning. Use la instancia de proceso como entorno de desarrollo o para fines de entrenamiento e inferencia de desarrollo/pruebas.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
ms.author: sgilley
author: sdgilley
ms.reviewer: sgilley
ms.date: 10/02/2020
ms.openlocfilehash: 3b5698c782b691dd8ae91913115db184fc83a2eb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91756626"
---
# <a name="create-and-manage-an-azure-machine-learning-compute-instance"></a>Creación y administración de una instancia de proceso de Azure Machine Learning

Aprenda a crear y administrar una [instancia de proceso](concept-compute-instance.md) en el área de trabajo de Azure Machine Learning.

Use una instancia de proceso como entorno de desarrollo completamente configurado y administrado en la nube. Para el desarrollo y las pruebas, también puede usar la instancia como un [destino de proceso de entrenamiento](concept-compute-target.md#train) o para un [destino de inferencia](concept-compute-target.md#deploy).   Una instancia de proceso puede ejecutar varios trabajos en paralelo y tiene una cola de trabajos. Como entorno de desarrollo, una instancia de proceso no se puede compartir con otros usuarios del área de trabajo.

En este artículo aprenderá a:

* Crear una instancia de proceso 
* Administrar (iniciar, detener, reiniciar y eliminar) una instancia de proceso
* Acceder a la ventana de terminal 
* Instalar paquetes de R o Python
* Crear entornos o kernels de Jupyter

Las instancias de proceso pueden ejecutar trabajos de manera segura en un [entorno de red virtual](how-to-secure-training-vnet.md), sin necesidad de que las empresas abran puertos SSH. El trabajo se ejecuta en un entorno con contenedores y empaqueta las dependencias del modelo en un contenedor de Docker. 

## <a name="prerequisites"></a>Requisitos previos

* Un área de trabajo de Azure Machine Learning. Para más información, consulte [Creación de un área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).

* La [extensión de la CLI de Azure para Machine Learning Service](reference-azure-machine-learning-cli.md), el [SDK de Python para Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py&preserve-view=true) o la [extensión de Visual Studio Code para Azure Machine Learning](tutorial-setup-vscode-extension.md).

## <a name="create"></a>Crear

**Tiempo estimado**: Aproximadamente 5 minutos.

La creación de una instancia de proceso es un proceso único en el área de trabajo. Puede volver a usar este proceso como una estación de trabajo de desarrollo o como un destino de proceso para el entrenamiento. Puede tener varias instancias de proceso asociadas al área de trabajo.

Los núcleos dedicados por región por cuota de familia de máquinas virtuales y cuota regional total, que se aplica a la creación de instancias de proceso, se unifica y comparte con la cuota de clúster de proceso de Azure Machine Learning. La detención de la instancia de proceso no libera la cuota para garantizar que pueda reiniciar la instancia de proceso. Tenga en cuenta que no es posible cambiar el tamaño de la máquina virtual de la instancia de proceso una vez creada.

En el siguiente ejemplo se muestra cómo crear una instancia de proceso:

# <a name="python"></a>[Python](#tab/python)

```python
import datetime
import time

from azureml.core.compute import ComputeTarget, ComputeInstance
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your instance
# Compute instance name should be unique across the azure region
compute_name = "ci{}".format(ws._workspace_id)[:10]

# Verify that instance does not exist already
try:
    instance = ComputeInstance(workspace=ws, name=compute_name)
    print('Found existing instance, use it.')
except ComputeTargetException:
    compute_config = ComputeInstance.provisioning_configuration(
        vm_size='STANDARD_D3_V2',
        ssh_public_access=False,
        # vnet_resourcegroup_name='<my-resource-group>',
        # vnet_name='<my-vnet-name>',
        # subnet_name='default',
        # admin_user_ssh_public_key='<my-sshkey>'
    )
    instance = ComputeInstance.create(ws, compute_name, compute_config)
    instance.wait_for_completion(show_output=True)
```

Para más información acerca de las clases, los métodos y los parámetros que se usan en este ejemplo, consulte los siguientes documentos de referencia:

* [Clase ComputeInstance](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computeinstance.computeinstance?view=azure-ml-py&preserve-view=true)
* [ComputeTarget.create](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computetarget?view=azure-ml-py&preserve-view=true#create-workspace--name--provisioning-configuration-)
* [ComputeInstance.wait_for_completion](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computeinstance(class)?view=azure-ml-py&preserve-view=true#wait-for-completion-show-output-false--is-delete-operation-false-)


# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

```azurecli-interactive
az ml computetarget create computeinstance  -n instance -s "STANDARD_D3_V2" -v
```

Para más información, vea la referencia de [az ml computetarget create computeinstance](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/create?view=azure-cli-latest&preserve-view=true#ext_azure_cli_ml_az_ml_computetarget_create_computeinstance).

# <a name="studio"></a>[Estudio](#tab/azure-studio)

En el área de trabajo de Azure Machine Learning Studio, cree una nueva instancia de proceso desde la sección **Proceso** o en la sección **Notebooks** cuando esté listo para ejecutar uno de sus cuadernos.

Para obtener información sobre cómo crear una instancia de proceso en Estudio, vea [Creación de destinos de proceso en Estudio de Azure Machine Learning](how-to-create-attach-compute-studio.md#compute-instance).

---

También puede crear una instancia de proceso con una [plantilla de Azure Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/101-machine-learning-compute-create-computeinstance). 

### <a name="create-on-behalf-of-preview"></a>Creación en nombre de alguien (versión preliminar)

Como administrador, puede crear una instancia de proceso en nombre de un científico de datos y asignarle la instancia con:
* [Plantilla de Azure Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/101-machine-learning-compute-create-computeinstance)  Para más información sobre cómo buscar los valores de TenantID y ObjectID necesarios en esta plantilla, consulte [Encontrar identificadores de objeto de identidad para la configuración de autenticación](../healthcare-apis/find-identity-object-ids.md).  También puede encontrar estos valores en el portal de Azure Active Directory.
* API DE REST

El científico de datos para el que se crea la instancia de proceso necesita los siguientes permisos de [control de acceso basado en rol (RBAC) de Azure](../role-based-access-control/overview.md): 
* *Microsoft.MachineLearningServices/workspaces/computes/start/action*
* *Microsoft.MachineLearningServices/workspaces/computes/stop/action*
* *Microsoft.MachineLearningServices/workspaces/computes/restart/action*
* *Microsoft.MachineLearningServices/workspaces/computes/applicationaccess/action*

El científico de datos puede iniciar, detener y reiniciar la instancia de proceso. Puede utilizar la instancia de proceso para:
* Jupyter
* JupyterLab
* RStudio
* Cuadernos integrados

## <a name="manage"></a>Administrar

Inicie, detenga, reinicie y elimine una instancia de proceso. Una instancia de proceso no se reduce verticalmente de forma automática, por lo que debe asegurarse de detener el recurso para evitar cargos continuos.

# <a name="python"></a>[Python](#tab/python)

En todos los siguientes ejemplos, el nombre de la instancia de proceso es **instance**.

* Obtener estado

    ```python
    # get_status() gets the latest status of the ComputeInstance target
    instance.get_status()
    ```

* Stop

    ```python
    # stop() is used to stop the ComputeInstance
    # Stopping ComputeInstance will stop the billing meter and persist the state on the disk.
    # Available Quota will not be changed with this operation.
    instance.stop(wait_for_completion=True, show_output=True)
    ```

* Start

    ```python
    # start() is used to start the ComputeInstance if it is in stopped state
    instance.start(wait_for_completion=True, show_output=True)
    ```

* Reinicio

    ```python
    # restart() is used to restart the ComputeInstance
    instance.restart(wait_for_completion=True, show_output=True)
    ```

* Eliminar

    ```python
    # delete() is used to delete the ComputeInstance target. Useful if you want to re-use the compute name 
    instance.delete(wait_for_completion=True, show_output=True)
    ```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

En todos los siguientes ejemplos, el nombre de la instancia de proceso es **instance**.

* Stop

    ```azurecli-interactive
    az ml computetarget stop computeinstance -n instance -v
    ```

    Para más información, consulte [az ml computetarget stop computeinstance](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/computeinstance?view=azure-cli-latest&preserve-view=true#ext-azure-cli-ml-az-ml-computetarget-computeinstance-stop).

* Start 

    ```azurecli-interactive
    az ml computetarget start computeinstance -n instance -v
    ```

    Para más información, consulte [az ml computetarget start computeinstance](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/computeinstance?view=azure-cli-latest&preserve-view=true#ext-azure-cli-ml-az-ml-computetarget-computeinstance-start).

* Reinicio 

    ```azurecli-interactive
    az ml computetarget restart computeinstance -n instance -v
    ```

    Para más información, consulte[az ml computetarget restart computeinstance](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/computeinstance?view=azure-cli-latest&preserve-view=true#ext-azure-cli-ml-az-ml-computetarget-computeinstance-restart).

* Eliminar

    ```azurecli-interactive
    az ml computetarget delete -n instance -v
    ```

    Para más información, consulte [az ml computetarget delete computeinstance](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget?view=azure-cli-latest&preserve-view=true#ext-azure-cli-ml-az-ml-computetarget-delete).

# <a name="studio"></a>[Estudio](#tab/azure-studio)

En el área de trabajo de Azure Machine Learning Studio, seleccione **Compute** (Proceso) y, luego, **Compute Instance** (Instancia de proceso) en la parte superior.

![Administración de una instancia de proceso](./media/concept-compute-instance/manage-compute-instance.png)

Puede realizar las siguientes acciones:

* Crear una nueva instancia de proceso 
* Actualizar la pestaña instancias de proceso.
* Iniciar, detener y reiniciar una instancia de proceso.  Se paga por la instancia cada vez que se ejecuta. Detenga la instancia de proceso cuando no la use para reducir el costo. Al detener una instancia de proceso, se cancela su asignación. A continuación, inícielo de nuevo cuando lo necesite.
* Elimine una instancia de proceso.
* Filtre la lista de instancias de proceso para mostrar solo las que haya creado.

Para cada instancia de proceso del área de trabajo que creó, o que se creó automáticamente, puede:

* Acceder a Jupyter, JupyterLab y RStudio en la instancia de proceso.
* SSH en la instancia de proceso. El acceso SSH está deshabilitado de forma predeterminada, pero se puede habilitar en el momento de la creación de la instancia de proceso. El acceso SSH se realiza a través del mecanismo de claves pública y privada. La pestaña le proporcionará detalles sobre la conexión SSH, como la dirección IP, el nombre de usuario y el número de puerto.
* Más detalles sobre una instancia de proceso específica, como la dirección IP y la región.

---

[RBAC](/azure/role-based-access-control/overview) permite controlar qué usuarios del área de trabajo pueden crear, eliminar, iniciar, detener y reiniciar una instancia de proceso. Todos los usuarios del rol colaborador y propietario del área de trabajo pueden crear, eliminar, iniciar, detener y reiniciar las instancias de proceso en el área de trabajo. Sin embargo, solo el creador de una instancia de proceso específica o el usuario asignado, si se creó en su nombre, tienen permiso para acceder a Jupyter, JupyterLab y RStudio en esa instancia de proceso. Una instancia de proceso está dedicada a un solo usuario que tiene acceso raíz y puede pasar por el terminal a través de Jupyter, JupyterLab o RStudio. La instancia de proceso incluirá el usuario que ha iniciado sesión y todas las acciones usarán la identidad de ese usuario para RBAC y la atribución de ejecuciones de experimentos. El acceso SSH se controla mediante un mecanismo de clave pública-privada.

RBAC puede controlar estas acciones:
* *Microsoft.MachineLearningServices/workspaces/computes/read*
* *Microsoft.MachineLearningServices/workspaces/computes/write*
* *Microsoft.MachineLearningServices/workspaces/computes/delete*
* *Microsoft.MachineLearningServices/workspaces/computes/start/action*
* *Microsoft.MachineLearningServices/workspaces/computes/stop/action*
* *Microsoft.MachineLearningServices/workspaces/computes/restart/action*


## <a name="access-the-terminal-window"></a>Acceder a la ventana de terminal

Abra la ventana de terminal de la instancia de proceso de cualquiera de estas maneras:

* RStudio: seleccione la pestaña **Terminal**, situada en la parte superior izquierda.
* Jupyter Lab:  seleccione el icono **Terminal** en el encabezado **Otros** de la pestaña Iniciador.
* Jupyter:  seleccione **Nuevo>Terminal** en la parte superior derecha de la pestaña Archivos.
* SSH en la máquina, si habilitó el acceso SSH cuando se creó la instancia de proceso.

Use la ventana de terminal para instalar paquetes y crear más kernels.

## <a name="install-packages"></a>Instalar paquetes

Puede instalar paquetes directamente en Jupyter Notebook o en RStudio:

* RStudio: use la pestaña **Paquetes**, situada en la parte inferior derecha, o la pestaña **Consola**, situada en la parte superior izquierda.  
* Python: agregue el código de instalación y ejecútelo en una celda de Jupyter Notebook.

O bien, puede realizar la instalación desde una ventana de terminal. Instale los paquetes de Python en el entorno de **Python 3.6: AzureML**.  Instale los paquetes de R en el entorno de **R**.

## <a name="add-new-kernels"></a>Incorporación de nuevos kernels

> [!WARNING]
>  Durante la personalización de la instancia de proceso, asegúrese de no eliminar el entorno de Conda **azureml_py36** ni el kernel de **Python 3.6: AzureML**. Esto es necesario para la funcionalidad de Jupyter/JupyterLab.

Para agregar un kernel nuevo de Jupyter a la instancia de proceso:

1. Cree un terminal desde Jupyter o JupyterLab, o bien desde el panel de Notebooks o SSH en la instancia de proceso.
2. Use la ventana de terminal para crear un nuevo entorno.  Por ejemplo, el código siguiente crea `newenv`:

    ```shell
    conda create --name newenv
    ```

3. Active el entorno.  Por ejemplo, después de crear `newenv`:

    ```shell
    conda activate newenv
    ```

4. Instalación del paquete PIP e ipykernel en el entorno nuevo y creación de un kernel para ese entorno conda

    ```shell
    conda install pip
    conda install ipykernel
    python -m ipykernel install --user --name newenv --display-name "Python (newenv)"
    ```

Se puede instalar cualquiera de los [kernels de Jupyter](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels) disponibles.



## <a name="next-steps"></a>Pasos siguientes

* [Envío de una ejecución de entrenamiento](how-to-set-up-training-targets.md) 
