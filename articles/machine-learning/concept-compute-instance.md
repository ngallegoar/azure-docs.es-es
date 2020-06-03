---
title: ¿Qué es una instancia de proceso de Azure Machine Learning?
titleSuffix: Azure Machine Learning
description: Más información sobre la instancia de proceso Azure Machine Learning, una estación de trabajo totalmente administrada basada en la nube.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 12/13/2019
ms.openlocfilehash: 8c03df8fb0cd8f5f092450ebe4c66266d2ff4293
ms.sourcegitcommit: fc0431755effdc4da9a716f908298e34530b1238
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83816359"
---
# <a name="what-is-an-azure-machine-learning-compute-instance"></a>¿Qué es una instancia de proceso de Azure Machine Learning?

Una instancia de proceso de Azure Machine Learning (versión preliminar) es una estación de trabajo basada en la nube totalmente administrada para científicos de datos. 

Las instancias de proceso permiten que sea fácil comenzar a desarrollar con Azure Machine Learning y proporcionan funcionalidades de administración y preparación empresarial para los administradores de TI.  

Use una instancia de proceso como entorno de desarrollo completamente configurado y administrado en la nube.

Las instancias de proceso se usan normalmente como entornos de desarrollo.  También se pueden usar como destino de proceso para la formación y la inferencia para el desarrollo y las pruebas.  Para las tareas de gran tamaño, un[ clúster de proceso de Azure Machine Learning](how-to-set-up-training-targets.md#amlcompute) con las funcionalidades de escalado de varios nodos es una mejor opción de destino de proceso.


## <a name="why-use-a-compute-instance"></a>¿Por qué usar una instancia de proceso?

Una instancia de proceso es una estación de trabajo totalmente administrada basada en la nube optimizada para el entorno de desarrollo de aprendizaje automático. Proporciona las siguientes ventajas:

|Ventajas principales||
|----|----|
|Productividad|Los científicos de datos pueden crear e implementar modelos mediante cuadernos integrados y las siguientes herramientas en su explorador web:<br/>-  Jupyter<br/>-  JupyterLab<br/>-  RStudio|
|Administrada y segura|Reduzca la superficie de seguridad y agregue compatibilidad con los requisitos de seguridad empresariales. Las instancias de proceso proporcionan directivas de administración sólidas y configuraciones de red seguras, como:<br/><br/>- Aprovisionamiento automatizado a partir de plantillas de Resource Manager o del SDK de Azure Machine Learning<br/>- [Control de acceso basado en rol (RBAC)](/azure/role-based-access-control/overview)<br/>- [Compatibilidad con redes virtuales](how-to-enable-virtual-network.md#compute-instance)<br/>- Directiva SSH para habilitar o deshabilitar el acceso SSH|
|Preconfigurada&nbsp;o&nbsp;ML|Ahorre tiempo en las tareas de configuración con paquetes de ML preconfigurados y actualizados, marcos de aprendizaje profundo y controladores de GPU.|
|Totalmente personalizable|La amplia compatibilidad con los tipos de máquina virtual de Azure, incluidas las GPU y la personalización de bajo nivel persistente, como la instalación de paquetes y controladores, hace que los escenarios avanzados sean muy sencillos. |

## <a name="tools-and-environments"></a><a name="contents"></a>Herramientas y entornos

La estancia de proceso Azure Machine Learning permite crear, formar e implementar modelos en una experiencia de cuaderno completamente integrada en el área de trabajo.


Las siguientes herramientas y entornos están instalados en la instancia de proceso: 

|Herramientas y entornos generales|Detalles|
|----|:----:|
|Controladores|`CUDA`</br>`cuDNN`</br>`NVIDIA`</br>`Blob FUSE` |
|Intel MPI Library||
|Azure CLI ||
|Ejemplos de Azure Machine Learning ||
|Docker||
|Nginx||
|NCCL 2.0 ||
|Protobuf|| 

|Herramientas y entornos de **R**|Detalles|
|----|:----:|
|RStudio Server Edición de código abierto||
|R kernel||
|SDK de Azure Machine Learning para R|[azuremlsdk](https://azure.github.io/azureml-sdk-for-r/reference/index.html)</br>Ejemplos del SDK|

|Herramientas y entornos de **PYTHON**|Detalles|
|----|----|
|Anaconda Python||
|Jupyter y extensiones||
|Jupyterlab y extensiones||
[SDK de Azure Machine Learning para Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)</br>desde PyPI|Incluye la mayoría de los paquetes adicionales de azureml.  Para ver la lista completa, [abra una ventana de terminal en la instancia de proceso](how-to-run-jupyter-notebooks.md#terminal) y ejecute <br/> `conda list -n azureml_py36 azureml*` |
|Otros paquetes de PyPI|`jupytext`</br>`tensorboard`</br>`nbconvert`</br>`notebook`</br>`Pillow`|
|Paquetes Conda|`cython`</br>`numpy`</br>`ipykernel`</br>`scikit-learn`</br>`matplotlib`</br>`tqdm`</br>`joblib`</br>`nodejs`</br>`nb_conda_kernels`|
|Paquetes de aprendizaje profundo|`PyTorch`</br>`TensorFlow`</br>`Keras`</br>`Horovod`</br>`MLFlow`</br>`pandas-ml`</br>`scrapbook`|
|Paquetes de ONNX|`keras2onnx`</br>`onnx`</br>`onnxconverter-common`</br>`skl2onnx`</br>`onnxmltools`|
|Ejemplos del SDK de Python y R para Azure Machine Learning||

Todos los paquetes de Python se instalan en el entorno de **Python 3.6: AzureML**.  

Las instancias de proceso se usan normalmente como entornos de desarrollo.  También se pueden usar como destino de proceso para la formación y la inferencia para el desarrollo y las pruebas.  Para las tareas de gran tamaño, un[ clúster de proceso de Azure Machine Learning](how-to-set-up-training-targets.md#amlcompute) con las funcionalidades de escalado de varios nodos es una mejor opción de destino de proceso.

### <a name="installing-packages"></a>Instalación de paquetes

Puede instalar paquetes directamente en Jupyter Notebook o en Rstudio:

* RStudio: use la pestaña **Paquetes**, situada en la parte inferior derecha, o la pestaña **Consola**, situada en la parte superior izquierda.  
* Python: agregue el código de instalación y ejecútelo en una celda de Jupyter Notebook.

También puede acceder a una ventana de terminal de cualquiera de estas formas:

* RStudio: seleccione la pestaña **Terminal**, situada en la parte superior izquierda.
* Jupyter Lab:  seleccione el icono **Terminal** en el encabezado **Otros** de la pestaña Iniciador.
* Jupyter:  seleccione **Nuevo>Terminal** en la parte superior derecha de la pestaña Archivos.
* Conéctese mediante SSH a la máquina virtual.  Después, instale los paquetes de Python en el entorno de **Python 3.6: AzureML**.  Instale los paquetes de R en el entorno de **R**.

## <a name="accessing-files"></a>Acceso a archivos

Los cuadernos y los scripts de R se almacenan en la cuenta de almacenamiento predeterminada del área de trabajo en el recurso compartido de archivos de Azure.  Estos archivos se encuentran en el directorio "Archivos de usuario". Este almacenamiento facilita el uso compartido de cuadernos entre instancias de proceso. La cuenta de almacenamiento también evita que los cuadernos se conserven de forma segura al detener o eliminar una instancia de proceso.

La cuenta de recurso compartido de archivos de Azure del área de trabajo se monta como una unidad en la instancia de proceso. Esta unidad es el directorio de trabajo predeterminado para Jupyter, Jupyter Labs y RStudio.

Se puede tener acceso a los archivos del recurso compartido de archivos desde todas las instancias de proceso de la misma área de trabajo. Los cambios que se realicen en estos archivos en la instancia de proceso se conservarán de forma confiable en el recurso compartido de archivos.

También puede clonar los ejemplos de Azure Machine Learning más recientes en la carpeta situada en el directorio de archivos de usuario del recurso compartido de archivos del área de trabajo.

La escritura de archivos pequeños puede ser más lenta en las unidades de red que escribir en la propia máquina virtual.  Si escribe muchos archivos pequeños, pruebe usar un directorio directamente en la instancia de proceso, como un directorio`/tmp`. Tenga en cuenta que estos archivos no serán accesibles desde otras instancias de proceso en el área de trabajo.

## <a name="managing-a-compute-instance"></a>Administración de una instancia de proceso

En el área de trabajo de Azure Machine Learning Studio, seleccione **Compute** (Proceso) y, luego, **Compute Instance** (Instancia de proceso) en la parte superior.

![Administración de una instancia de proceso](./media/concept-compute-instance/manage-compute-instance.png)

Puede realizar las siguientes acciones:

* Crear de una instancia de proceso. Especifique el nombre, el tipo de VM de Azure, incluidas las GPU (tenga en cuenta que el tipo de VM no puede cambiarse después de la creación), habilite o deshabilite el acceso SSH y, si lo desea, configure las opciones de red virtual. También puede crear una instancia directamente desde cuadernos integrados, Azure Portal, plantilla de Resource Manager o SDK de Azure Machine Learning. La cuota de núcleos dedicados por región que se aplica a la creación de instancias de proceso se unifica y comparte con la cuota de clúster de proceso de Azure Machine Learning.
* Actualice la pestaña instancias de proceso
* Iniciar, detener y reiniciar una instancia de proceso. Detenga la máquina virtual cuando no esté utilizándola para reducir los costos. A continuación, inícielo de nuevo cuando lo necesite.
* Eliminar una instancia de proceso.

Para cada instancia de proceso del área de trabajo, puede:

* Acceder a Jupyter, JupyterLab y RStudio en la instancia de proceso.
* SSH en la instancia de proceso. El acceso SSH está deshabilitado de forma predeterminada, pero se puede habilitar en el momento de la creación de la instancia de proceso. El acceso SSH se realiza a través del mecanismo de claves pública y privada. La pestaña le proporcionará detalles sobre la conexión SSH, como la dirección IP, el nombre de usuario y el número de puerto.
* Más detalles sobre una instancia de proceso específica, como la dirección IP y la región.

[RBAC](/azure/role-based-access-control/overview) permite controlar qué usuarios del área de trabajo pueden crear, eliminar, iniciar, detener y reiniciar una instancia de proceso. Todos los usuarios del rol colaborador y propietario del área de trabajo pueden crear, eliminar, iniciar, detener y reiniciar las instancias de proceso en el área de trabajo. Sin embargo, solo el creador de una instancia de proceso específica tiene permiso para acceder a Jupyter, JupyterLab y RStudio en esa instancia de proceso. El creador de la instancia de proceso tiene la instancia de proceso dedicada, tiene acceso raíz y puede conectarse mediante un terminal con Jupyter. Solo el usuario creador de la instancia de proceso puede iniciar sesión en ella y todas las acciones usarán la identidad de ese usuario para RBAC y la atribución de ejecuciones de experimentos. El acceso SSH se controla mediante un mecanismo de clave pública-privada.

Usted también puede crear una instancia
* Directamente de la experiencia de cuadernos integrados
* En Azure Portal
* A partir de una plantilla de Azure Resource Manager
* Con SDK de Azure Machine Learning

La cuota de núcleos dedicados por región que se aplica a la creación de instancias de proceso se unifica y comparte con la cuota de clúster de formación de Azure Machine Learning. 

## <a name="compute-target"></a>Destino de proceso

Las instancias de proceso se pueden usar como [destino de proceso de entrenamiento](concept-compute-target.md#train) de forma similar a los clústeres de entrenamiento de Azure Machine Learning. Aprovisione una máquina virtual de varias GPU para ejecutar trabajos de formación distribuidos con estimaciones de TensorFlow/PyTorch. También puede crear una configuración de ejecución y usarla para ejecutar el experimento en la instancia de proceso. Puede usar una instancia de proceso como destino de implementación de inferencia local para escenarios de prueba o depuración.

## <a name="what-happened-to-notebook-vm"></a><a name="notebookvm"></a>¿Qué ha ocurrido con la VM de cuaderno?

Las instancias de proceso reemplazan la VM de cuaderno.  

Los archivos de cuaderno almacenados en el recurso compartido de archivos del área de trabajo y los almacenes de datos del área de trabajo serán accesibles desde una instancia de proceso. Sin embargo, los paquetes personalizados que se instalaron previamente en una VM de Notebook deberán volver a instalarse en la instancia de proceso. Las limitaciones de cuota que se aplican a la creación de clústeres de proceso se aplicarán también a la creación de instancias de proceso. 

No se pueden crear las nuevas máquinas virtuales de Notebook. Sin embargo, todavía puede tener acceso a las VMs de Notebook que ha creado y utilizarlas con toda funcionalidad. Las instancias de proceso se pueden crear en la misma área de trabajo que las VMs de Notebook existentes. 


## <a name="next-steps"></a>Pasos siguientes

 * [Tutorial: Formar su primer modelo de ML](tutorial-1st-experiment-sdk-train.md) muestra cómo usar una instancia de proceso con un cuaderno integrado.
