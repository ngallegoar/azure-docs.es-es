---
title: Conversión de código de experimento de aprendizaje automático en código de producción
titleSuffix: Azure Machine Learning
description: Aprenda a convertir código experimental de aprendizaje automático en código de producción mediante la plantilla de código MLOpsPython.
author: bjcmit
ms.author: brysmith
ms.service: machine-learning
ms.topic: tutorial
ms.date: 03/13/2020
ms.openlocfilehash: f40c2b5f7134458b3f8cb492652bebf14388634c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "79477143"
---
# <a name="tutorial-convert-ml-experimental-code-to-production-code"></a>Tutorial: Conversión de código experimental de Machine Learning en código de producción

Un proyecto de aprendizaje automático requiere experimentación al probar hipótesis con herramientas ágiles, como Jupyter Notebook, con conjuntos de datos auténticos. Una vez que el modelo está listo para producción, el código del modelo debe colocarse en un repositorio de código de producción. En algunos casos, el código del modelo se debe convertir en scripts de Python que se colocarán en el repositorio de código de producción. En este tutorial se describe un enfoque recomendado sobre cómo exportar código de experimentación a scripts de Python.

En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Limpiar código no esencial
> * Refactorizar código de Jupyter Notebook en funciones
> * Crear scripts de Python para tareas relacionadas
> * Crear pruebas unitarias

## <a name="prerequisites"></a>Prerrequisitos

- Genere la [plantilla MLOpsPython](https://github.com/microsoft/MLOpsPython/generate) y use los cuadernos `experimentation/Diabetes Ridge Regression Training.ipynb` y `experimentation/Diabetes Ridge Regression Scoring.ipynb`. Estos cuadernos se utilizan como ejemplo de conversión de experimentación en producción. Estos cuadernos se pueden encontrar en [https://github.com/microsoft/MLOpsPython/tree/master/experimentation](https://github.com/microsoft/MLOpsPython/tree/master/experimentation).
- Instale nbconvert. Siga únicamente las instrucciones de instalación de la sección de __instalación de nbconvert__ en la página de [instalación](https://nbconvert.readthedocs.io/en/latest/install.html).

## <a name="remove-all-nonessential-code"></a>Eliminación de todo el código no esencial

Parte del código escrito durante la experimentación solo tiene fines de exploración. Por lo tanto, el primer paso para convertir código experimental en código de producción es quitar este código no esencial. La eliminación del código no esencial también hará que el código sea más fácil de mantener. En esta sección, quitará el código del cuaderno `experimentation/Diabetes Ridge Regression Training.ipynb`. Las instrucciones que imprimen la forma de `X` y `y` y la celda que llama a `features.describe` son solo para la exploración de datos y se pueden quitar. Después de quitar el código no esencial, `experimentation/Diabetes Ridge Regression Training.ipynb` debe tener un aspecto similar al código siguiente sin marcado:

```python
from sklearn.datasets import load_diabetes
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
import joblib

X, y = load_diabetes(return_X_y=True)

X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=0)
data = {"train": {"X": X_train, "y": y_train},
        "test": {"X": X_test, "y": y_test}}

alpha = 0.5

reg = Ridge(alpha=alpha)
reg.fit(data["train"]["X"], data["train"]["y"])

preds = reg.predict(data["test"]["X"])
print("mse", mean_squared_error(preds, data["test"]["y"]))

model_name = "sklearn_regression_model.pkl"
joblib.dump(value=reg, filename=model_name)
```

## <a name="refactor-code-into-functions"></a>Refactorización del código en funciones

En segundo lugar, el código de Jupyter debe refactorizarse en funciones. La refactorización del código en funciones facilita la realización de pruebas unitarias y hace que el código sea más sencillo de mantener. En esta sección, refactorizará lo siguiente:

- El cuaderno Diabetes Ridge Regression Training (`experimentation/Diabetes Ridge Regression Training.ipynb`)
- El cuaderno Diabetes Ridge Regression Scoring (`experimentation/Diabetes Ridge Regression Scoring.ipynb`)

### <a name="refactor-diabetes-ridge-regression-training-notebook-into-functions"></a>Refactorización del cuaderno Diabetes Ridge Regression Training en funciones

En `experimentation/Diabetes Ridge Regression Training.ipynb`, complete los pasos siguientes:

1. Cree una función llamada `train_model`, que toma los parámetros `data` y `alpha` y devuelve un modelo.
1. Copie el código que se encuentra debajo de los encabezados "Train Model on Training Set" y "Validate Model on Validation Set" en la función `train_model`.

La función `train_model` debe ser similar al código siguiente:

```python
def train_model(data, alpha):
    reg = Ridge(alpha=alpha)
    reg.fit(data["train"]["X"], data["train"]["y"])
    preds = reg.predict(data["test"]["X"])
    print("mse", mean_squared_error(
        preds, data["test"]["y"]))
    return reg
```

Una vez que se creala función `train_model`, reemplace el código que se encuentra bajo los encabezados "Train Model on Training Set" y "Validate Model on Validation Set" por la siguiente instrucción:

```python
reg = train_model(data, alpha)
```

La instrucción anterior llama a la función `train_model` pasando los parámetros `data` y `alpha` y devuelve el modelo.

En `experimentation/Diabetes Ridge Regression Training.ipynb`, complete los pasos siguientes:

1. Cree una nueva función llamada `main`, que no toma parámetros y no devuelve nada.
1. Copie el código que hay bajo los encabezados "Load Data", "Split Data into Training and Validation Sets" y "Save Model" en la función `main`.
1. Copie la llamada recién creada en `train_model` en la función `main`.

La función `main` debe ser similar al código siguiente:

```python
def main():

    model_name = "sklearn_regression_model.pkl"
    alpha = 0.5

    X, y = load_diabetes(return_X_y=True)

    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=0)
    data = {"train": {"X": X_train, "y": y_train},
            "test": {"X": X_test, "y": y_test}}

    reg = train_model(data, alpha)

    joblib.dump(value=reg, filename=model_name)
```

Una vez que se crea la función `main`, reemplace todo el código que se encuentra bajo de los encabezados "Load Data", "Split Data into Training and Validation Sets" y "Save Model", junto con la llamada recién creada a `train_model` por la siguiente instrucción:

```python
main()
```

Después de la refactorización, `experimentation/Diabetes Ridge Regression Training.ipynb` debe ser similar al código siguiente sin el marcado:

```python
from sklearn.datasets import load_diabetes
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
import joblib


def train_model(data, alpha):
    reg = Ridge(alpha=alpha)
    reg.fit(data["train"]["X"], data["train"]["y"])
    preds = reg.predict(data["test"]["X"])
    print("mse", mean_squared_error(
        preds, data["test"]["y"]))
    return reg

def main():

    model_name = "sklearn_regression_model.pkl"
    alpha = 0.5

    X, y = load_diabetes(return_X_y=True)

    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=0)
    data = {"train": {"X": X_train, "y": y_train},
            "test": {"X": X_test, "y": y_test}}

    reg = train_model(data, alpha)

    joblib.dump(value=reg, filename=model_name)

main()
```

### <a name="refactor-diabetes-ridge-regression-scoring-notebook-into-functions"></a>Refactorización del cuaderno Diabetes Ridge Regression Scoring en funciones

En `experimentation/Diabetes Ridge Regression Scoring.ipynb`, complete los pasos siguientes:

1. Cree una nueva función llamada `init`, que no toma parámetros y no devuelve nada.
1. Copie el código que se encuentra bajo del encabezado "Load Model" en la función `init`.

La función `init` debe ser similar al código siguiente:

```python
def init():
    model_path = Model.get_model_path(
        model_name="sklearn_regression_model.pkl")
    model = joblib.load(model_path)
```

Una vez creada la función `init`, reemplace todo el código que se encuentra bajo del encabezado "Load Model" por una única llamada a `init`, como se indica a continuación:

```python
init()
```

En `experimentation/Diabetes Ridge Regression Scoring.ipynb`, complete los pasos siguientes:

1. Cree una nueva función llamada `run`, que toma `raw_data` y `request_headers` como parámetros y devuelve un diccionario de resultados como se indica a continuación:

    ```python
    {"result": result.tolist()}
    ```

1. Copie el código de los encabezados "Prepare Data" y "Score Data" en la función `run`.

    La función `run` debe ser similar al código siguiente (recuerde quitar las instrucciones que establecen las variables `raw_data` y `request_headers`, que se usarán más adelante cuando se llame a la función `run`):

    ```python
    def run(raw_data, request_headers):
        data = json.loads(raw_data)["data"]
        data = numpy.array(data)
        result = model.predict(data)

        return {"result": result.tolist()}
    ```

Una vez creada la función `run`, reemplace todo el código de los encabezados "Prepare Data" y "Score Data" por el siguiente:

```python
raw_data = '{"data":[[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}'
request_header = {}
prediction = run(raw_data, request_header)
print("Test result: ", prediction)
```

El código anterior establece las variables `raw_data` y `request_header`, llama a la función `run` con `raw_data` y `request_header` e imprime las predicciones.

Después de la refactorización, `experimentation/Diabetes Ridge Regression Scoring.ipynb` debe ser similar al código siguiente sin el marcado:

```python
import json
import numpy
from azureml.core.model import Model
import joblib

def init():
    model_path = Model.get_model_path(
        model_name="sklearn_regression_model.pkl")
    model = joblib.load(model_path)

def run(raw_data, request_headers):
    data = json.loads(raw_data)["data"]
    data = numpy.array(data)
    result = model.predict(data)

    return {"result": result.tolist()}

init()
test_row = '{"data":[[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}'
request_header = {}
prediction = run(test_row, {})
print("Test result: ", prediction)
```

## <a name="combine-related-functions-in-python-files"></a>Combinación de funciones relacionadas en archivos de Python

En tercer lugar, las funciones relacionadas deben combinarse en archivos de Python para mejorar la reutilización del código. En esta sección, creará archivos de Python para los siguientes cuadernos:

- El cuaderno Diabetes Ridge Regression Training (`experimentation/Diabetes Ridge Regression Training.ipynb`)
- El cuaderno Diabetes Ridge Regression Scoring (`experimentation/Diabetes Ridge Regression Scoring.ipynb`)

### <a name="create-python-file-for-the-diabetes-ridge-regression-training-notebook"></a>Creación de archivo de Python para el cuaderno Diabetes Ridge Regression Training

Convierta el cuaderno en un script ejecutable mediante la ejecución de la siguiente instrucción en un símbolo del sistema, que usa el paquete nbconvert y la ruta de acceso de `experimentation/Diabetes Ridge Regression Training.ipynb`:

```
jupyter nbconvert -- to script "Diabetes Ridge Regression Training.ipynb" –output train
```

Una vez convertido el cuaderno en `train.py`, quite todos los comentarios. El archivo `train.py` debe ser similar al siguiente código:

```python
from sklearn.datasets import load_diabetes
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
import joblib


def train_model(data, alpha):
    reg = Ridge(alpha=alpha)
    reg.fit(data["train"]["X"], data["train"]["y"])
    preds = reg.predict(data["test"]["X"])
    print("mse", mean_squared_error(
        preds, data["test"]["y"]))
    return reg

def main():
    model_name = "sklearn_regression_model.pkl"
    alpha = 0.5

    X, y = load_diabetes(return_X_y=True)

    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=0)
    data = {"train": {"X": X_train, "y": y_train},
            "test": {"X": X_test, "y": y_test}}

    reg = train_model(data, alpha)

    joblib.dump(value=reg, filename=model_name)

main()
```

El archivo `train.py` que se encuentra en el directorio `diabetes_regression/training` en el repositorio de MLOpsPython admite argumentos de línea de comandos (en concreto, `build_id`, `model_name`y `alpha`). La compatibilidad con los argumentos de línea de comandos se puede agregar al archivo `train.py` para admitir nombres de modelo dinámicos y valores `alpha`, pero no es necesaria para que el código se ejecute correctamente.

### <a name="create-python-file-for-the-diabetes-ridge-regression-scoring-notebook"></a>Creación de archivo de Python para el cuaderno Diabetes Ridge Regression Scoring

Convierta el cuaderno en un script ejecutable mediante la ejecución de la siguiente instrucción en un símbolo del sistema, que usa el paquete nbconvert y la ruta de acceso de `experimentation/Diabetes Ridge Regression Scoring.ipynb`:

```
jupyter nbconvert -- to script "Diabetes Ridge Regression Scoring.ipynb" –output score
```

Una vez convertido el cuaderno en `score.py`, quite todos los comentarios. El archivo `score.py` debe ser similar al siguiente código:

```python
import json
import numpy
from azureml.core.model import Model
import joblib

def init():
    model_path = Model.get_model_path(
        model_name="sklearn_regression_model.pkl")
    model = joblib.load(model_path)

def run(raw_data, request_headers):
    data = json.loads(raw_data)["data"]
    data = numpy.array(data)
    result = model.predict(data)

    return {"result": result.tolist()}

init()
test_row = '{"data":[[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}'
request_header = {}
prediction = run(test_row, request_header)
print("Test result: ", prediction)
```

La función `train_model` debe modificarse para crear una instancia de un modelo de variable global de modo que esté visible en todo el script. Agregue la siguiente instrucción al principio de la función `init`:

```python
global model
```

Después de agregar la instrucción anterior, la función `init` debe ser similar al código siguiente:

```python
def init():
    global model

    # load the model from file into a global object
    model_path = Model.get_model_path(
        model_name="sklearn_regression_model.pkl")
    model = joblib.load(model_path)
```

## <a name="create-unit-tests-for-each-python-file"></a>Creación de pruebas unitarias para cada archivo de Python

En cuarto lugar, es necesario crear pruebas unitarias para cada archivo de Python, lo que hace que el código sea más fiable y fácil de mantener. En esta sección, creará una prueba unitaria para una de las funciones de `train.py`.

`train.py` archivo contiene dos funciones: `train_model` y `main`. Cada función necesita una prueba unitaria, pero solo se creará una prueba unitaria para la función `train_model` mediante el marco Pytest en este tutorial. Pytest no es el único marco de pruebas unitarias de Python, pero es uno de los más usados. Para más información, visite [Pytest](https://pytest.org).

Una prueba unitaria normalmente contiene tres acciones principales:

- Organizar el objeto: crear y configurar los objetos necesarios
- Actuar sobre un objeto
- Declarar lo que se espera

Una condición común para `train_model` es cuando se pasan `data` y un valor `alpha`. El resultado esperado es que se debe llamar a las funciones `Ridge.train` y `Ridge.predict`. Dado que los métodos de entrenamiento de aprendizaje automático no suelen ser de ejecución rápida, se simulará la llamada a `Ridge.train`. Como el valor devuelto de `Ridge.train` es un objeto ficticio, también simularemos `Ridge.predict`. La prueba unitaria para `train_model` que prueba el paso de `data` y un valor de `alpha` con el resultado esperado de las funciones `Ridge.train` y `Ridge.predict` a las que se llama mediante la simulación y el marco de Pytest debe ser similar al código siguiente:

```python
import pytest
from code.training.train import train_model

class TestTrain:

    @staticmethod
    def test_train_model(mocker):
        # Arrange
        test_data = {"train": {"X": [[1, 2, 3]], "y": [0]},
                     "test": {"X": [[4, 5, 6]], "y": [0]}}
        test_alpha = 0.5
        mock_ridge_fit = mocker.patch('Ridge.fit')
        mock_ridge_predict = mocker.patch('Ridge.predict')

        # Act
        train_model(test_data, test_alpha)

        # Assert
        mock_ridge_fit.assert_called()
        mock_ridge_predict.assert_called()
```

## <a name="use-your-own-model-with-mlopspython-code-template"></a>Uso de su propio modelo con la plantilla de código de MLOpsPython

Si ha seguido los pasos descritos en esta guía, tendrá un conjunto de scripts que se correlacionan con los scripts de entrenamiento, puntuación y prueba disponibles en el repositorio de MLOpsPython.  Según la estructura mencionada anteriormente, los siguientes pasos le guiarán a través de lo que se necesita para usar estos archivos para su proyecto de aprendizaje automático:

1. Siga la guía [Introducción](https://github.com/microsoft/MLOpsPython/blob/master/docs/getting_started.md) de MLOpsPython.
2. Siga las [instrucciones de arranque](https://github.com/microsoft/MLOpsPython/blob/master/bootstrap/README.md) de MLOpsPython para crear el punto de inicio del proyecto.
3. Reemplazar el código de entrenamiento.
4. Reemplazar el código de puntuación.
5. Actualizar el código de evaluación.

### <a name="follow-the-getting-started-guide"></a>Seguimiento de la guía de introducción
Es necesario seguir la guía [Introducción](https://github.com/microsoft/MLOpsPython/blob/master/docs/getting_started.md) para que la infraestructura y las canalizaciones auxiliares ejecuten MLOpsPython.

### <a name="follow-the-bootstrap-instructions"></a>Siga las instrucciones de arranque

La guía [Arranque desde el repositorio de MLOpsPython](https://github.com/microsoft/MLOpsPython/blob/master/bootstrap/README.md) le ayudará a preparar rápidamente el repositorio para el proyecto.

**Nota:** Dado que el script de arranque cambiará el nombre de la carpeta diabetes_regression por el nombre de proyecto que elija, haremos referencia al proyecto como `[project name]` cuando se trate de rutas de acceso.

### <a name="replace-training-code"></a>Reemplazo del código de entrenamiento

Se requiere reemplazar el código usado para entrenar el modelo y quitar o reemplazar las pruebas unitarias correspondientes para que la solución funcione con su propio código. Siga estos pasos específicamente:

1. Reemplace `[project name]/training/train.py`. Este script entrena el modelo de forma local o en el proceso de Azure Machine Learning.
1. Quite o reemplace las pruebas unitarias de entrenamiento encontradas en `[project name]/training/test_train.py`.

### <a name="replace-score-code"></a>Reemplazo del código de puntuación

Para que el modelo proporcione funcionalidades de inferencia en tiempo real, el código de puntuación debe reemplazarse. La plantilla MLOpsPython usa el código de puntuación para implementar el modelo para realizar la puntuación en tiempo real en aplicaciones web, ACI o AKS. Si desea mantener la puntuación, reemplace `[project name]/scoring/score.py`.

### <a name="update-evaluation-code"></a>Actualización del código de evaluación

La plantilla MLOpsPython usa el script evaluate_model para comparar el rendimiento del modelo recién entrenado y el modelo de producción actual basado en un error cuadrático medio. Si el rendimiento del modelo recién entrenado es mejor que el del modelo de producción actual, las canalizaciones continuarán. De lo contrario, las canalizaciones se cancelan. Para seguir con la evaluación, reemplace todas las instancias de `mse` en `[project name]/evaluate/evaluate_model.py` por la métrica que desee.

Para detener la evaluación, establezca la variable de canalización de DevOps `RUN_EVALUATION` de `.pipelines/[project name]-variables-template.yml` en `false`.

## <a name="next-steps"></a>Pasos siguientes

Ahora que sabe cómo convertir un experimento en código de producción, use los vínculos siguientes para aprender a supervisar las ejecuciones de los experimentos y los modelos implementados como servicios web:

> [!div class="nextstepaction"]
> [Supervisión de métricas y ejecuciones de experimentos de Azure ML](https://docs.microsoft.com/azure/machine-learning/how-to-track-experiments)
> [Supervisión y recopilación de datos de los puntos de conexión del servicio web ML](https://docs.microsoft.com/azure/machine-learning/how-to-enable-app-insights)
