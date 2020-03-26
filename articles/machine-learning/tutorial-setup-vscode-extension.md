---
title: 'Tutorial: Configuración de la extensión de Visual Studio Code'
titleSuffix: Azure Machine Learning
description: Aprenda a configurar la extensión Azure Machine Learning para Visual Studio Code.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: luisquintanilla
ms.author: luquinta
ms.date: 02/24/2020
ms.openlocfilehash: 583071ee22e4fb9cffc741520b1583790002a5bf
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "77604872"
---
# <a name="set-up-azure-machine-learning-visual-studio-code-extension"></a>Configuración de la extensión Azure Machine Learning para Visual Studio Code

Aprenda a instalar y ejecutar scripts con la extensión Azure Machine Learning para Visual Studio Code.

En este tutorial, ha aprendido las tareas siguientes:

> [!div class="checklist"]
> * Instalación de la extensión Azure Machine Learning para Visual Studio Code
> * Inicio de sesión en la cuenta de Azure desde Visual Studio Code
> * Uso de la extensión Azure Machine Learning para ejecutar un script de ejemplo

## <a name="prerequisites"></a>Prerrequisitos

- Suscripción de Azure. Si no tiene una, regístrese para probar la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).
- Código de Visual Studio. Si no lo tiene, [instálelo](https://code.visualstudio.com/docs/setup/setup-overview).
- [Python 3](https://www.python.org/downloads/)

## <a name="install-the-extension"></a>Instalación de la extensión

1. Abra Visual Studio Code.
1. Seleccione el icono **Extensiones** de la **barra de actividades** para abrir la vista Extensiones.
1. En la vista Extensiones, busque "Azure Machine Learning".
1. Seleccione **Instalar**.

    > [!div class="mx-imgBorder"]
    > ![Instalación de la extensión Azure Machine Learning para VS Code](./media/tutorial-setup-vscode-extension/install-aml-vscode-extension.PNG)

> [!NOTE]
> Como alternativa, puede instalar la extensión Azure Machine Learning al [descargar el instalador directamente](https://aka.ms/vscodetoolsforai) desde Visual Studio Marketplace. 

El resto de los pasos de este tutorial se han probado con la **versión 0.6.8** de la extensión.

## <a name="sign-in-to-your-azure-account"></a>Inicio de sesión en la cuenta de Azure

Para aprovisionar recursos y ejecutar cargas de trabajo en Azure, tiene que iniciar sesión con las credenciales de su cuenta de Azure. Para ayudar con la administración de cuentas, Azure Machine Learning instala automáticamente la extensión Azure Account. Visite el sitio siguiente para [más información sobre la extensión Azure Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

1. Seleccione **Ver > Paleta de comandos** en la barra de menús para abrir la paleta de comandos. 
1. Escriba el comando "Azure: Sign-In" en la paleta de comandos para empezar el proceso de inicio de sesión.

## <a name="run-a-machine-learning-model-training-script-in-azure"></a>Ejecución de un script de entrenamiento del modelo de Machine Learning en Azure

Ahora que ha iniciado sesión en Azure con sus credenciales de cuenta, siga los pasos de esta sección para aprender a usar la extensión para entrenar un modelo de aprendizaje automático.

1. Descargue y descomprima el [repositorio VS Code Tools for AI](https://github.com/microsoft/vscode-tools-for-ai/archive/master.zip) donde quiera en el equipo.
1. Abra este directorio `mnist-vscode-docs-sample` en Visual Studio Code.
1. Seleccione el icono de **Azure** en la barra de actividades.
1. Seleccione el icono **Run Experiment** (Ejecutar experimento) en la parte superior de la vista de Azure Machine Learning.

    > [!div class="mx-imgBorder"]
    > ![Run Experiment](./media/tutorial-setup-vscode-extension/run-experiment.PNG) (Ejecutar experimento)

1. Cuando se expanda la paleta de comandos, siga las indicaciones.

    1. Seleccione su suscripción a Azure.
    1. Seleccione **Create a new Azure ML workspace** (Crear un área de trabajo de Azure Machine Learning)
    1. Seleccione el tipo de trabajo **TensorFlow Single-Node Training** (Entrenamiento de nodo único de TensorFlow).
    1. Escriba `train.py` como el script que se va a entrenar. Este es el archivo que contiene el código de un modelo de aprendizaje automático que clasifica imágenes de dígitos manuscritos.
    1. Especifique los paquetes siguientes como requisitos de ejecución.

        ```text
        pip: azureml-defaults; conda: python=3.6.2, tensorflow=1.15.0
        ```

1. En este momento aparecerá un archivo de configuración similar al siguiente en el editor de texto. La configuración contiene la información necesaria para ejecutar el trabajo de entrenamiento, como el archivo que contiene el código para entrenar el modelo y las dependencias de Python especificadas en el paso anterior.

    ```json
    {
        "workspace": "WS01311608",
        "resourceGroup": "WS01311608-rg1",
        "location": "South Central US",
        "experiment": "WS01311608-exp1",
        "compute": {
            "name": "WS01311608-com1",
            "vmSize": "Standard_D1_v2, Cores: 1; RAM: 3.5GB;"
        },
        "runConfiguration": {
            "filename": "WS01311608-com1-rc1",
            "condaDependencies": [
                "python=3.6.2",
                "tensorflow=1.15.0"
            ],
            "pipDependencies": [
                "azureml-defaults"
            ]
        }
    }
    ```

1. Una vez que esté satisfecho con la configuración, abra la paleta de comandos y escriba el siguiente comando para enviar el experimento:

    ```text
    Azure ML: Submit Experiment
    ```

    Así se envían el archivo `train.py` y el de configuración al área de trabajo de Azure Machine Learning. A continuación, el trabajo de entrenamiento se inicia en un recurso de proceso de Azure.

### <a name="track-the-progress-of-the-training-script"></a>Seguimiento del progreso del script de entrenamiento

La ejecución del script puede tardar varios minutos. Para realizar el seguimiento de su progreso:

1. Seleccione el icono de **Azure** en la barra de actividades.
1. Expanda el nodo de la suscripción.
1. Expanda el nodo del experimento que se está ejecutando actualmente. Este se encuentra dentro del nodo `{workspace}/Experiments/{experiment}` en el que los valores del área de trabajo y del experimento son los mismos que las propiedades definidas en el archivo de configuración.
1. Se enumerarán todas las ejecuciones del experimento y su estado. Para obtener el estado más reciente, haga clic en el icono de actualización de la parte superior de la vista de Azure Machine Learning.

    > [!div class="mx-imgBorder"]
    > ![Seguimiento del progreso del experimento](./media/tutorial-setup-vscode-extension/track-experiment-progress.PNG)

### <a name="download-the-trained-model"></a>Descarga del modelo entrenado

Una vez completado el experimento, la salida es un modelo entrenado. Para descargar las salidas en un entorno local:

1. Haga clic con el botón derecho en la ejecución más reciente y seleccione **Download Outputs** (Descargar salidas).

    > [!div class="mx-imgBorder"]
    > ![Descarga del modelo entrenado](./media/tutorial-setup-vscode-extension/download-trained-model.PNG)

1. Seleccione la ubicación donde guardar las salidas.
1. En el entorno local se descarga una carpeta con el nombre de la ejecución. Vaya a esta.
1. Los archivos de modelo se encuentran en el directorio `outputs/outputs/model`.

## <a name="next-steps"></a>Pasos siguientes

* [Tutorial: Entrenamiento e implementación de un modelo de TensorFlow de clasificación de imágenes con la extensión Azure Machine Learning para Visual Studio Code](tutorial-train-deploy-image-classification-model-vscode.md).
