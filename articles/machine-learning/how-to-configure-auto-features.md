---
title: Caracterización en experimentos de AutoML
titleSuffix: Azure Machine Learning
description: Obtenga información acerca de los valores de caracterización que Azure Machine Learning ofrece y cómo se admite la ingeniería de características en experimentos de Machine Learning automatizados.
author: nibaccam
ms.author: nibaccam
ms.reviewer: nibaccam
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 05/28/2020
ms.custom: seodec18
ms.openlocfilehash: 93e18a95e30c21a44f9ca7df92925323930a9ce8
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84122311"
---
# <a name="featurization-with-automated-machine-learning"></a>Caracterización con aprendizaje automático automatizado

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En esta guía, aprenderá qué valores de caracterización de características se ofrecen y cómo personalizarlas para los [experimentos de aprendizaje automático automatizado](concept-automated-ml.md).

La ingeniería de características es el proceso de usar el conocimiento del dominio de los datos para crear características que ayuden a los algoritmos de ML a aprender mejor. En Azure Machine Learning, se aplican técnicas de escalado de datos y normalización para facilitar la ingeniería de características. El conjunto de estas técnicas y la ingeniería de características se conoce como caracterización en el área de aprendizaje automático automatizado, AutoML y experimentos.

En este artículo se da por supuesto que ya está familiarizado con la configuración de experimentos de AutoML. Para más información, consulte los artículos siguientes:

* Para conocer la experiencia de codificar por primera vez: [Configuración de experimentos de ML automatizado en el SDK de Python](how-to-configure-auto-train.md)
* Para conocer la experiencia con poca o nula codificación: [Creación, revisión e implementación de modelos de aprendizaje automático automatizado con Azure Machine Learning Studio](how-to-use-automated-ml-for-ml-models.md)

## <a name="configure-featurization"></a>Configuración de la caracterización

En todos los experimentos de aprendizaje automático automatizado se aplican [técnicas de escalado automático y normalización](#featurization) a los datos de forma predeterminada. Estas técnicas son tipos de caracterización que ayudan a *ciertos* algoritmos que son sensibles a las características a diferentes escalas. Sin embargo, también puede habilitar una caracterización adicional, como la **atribución de los valores que faltan, la codificación**  y las **transformaciones**.

> [!NOTE]
> Los pasos de la caracterización del aprendizaje automático automatizado (normalización de características, control de los datos que faltan, conversión de valores de texto a numéricos, etc.) se convierten en parte del modelo subyacente. Cuando se usa el modelo para realizar predicciones, se aplican automáticamente a los datos de entrada los mismos pasos de caracterización que se aplican durante el entrenamiento.

En el caso de los experimentos configurados con el SDK, puede habilitar o deshabilitar el valor `featurization` y especificar con más detalle los pasos de caracterización que se deben usar para el experimento. Si usa Azure Machine Learning Studio, consulte cómo habilitar la caracterización [con estos pasos](how-to-use-automated-ml-for-ml-models.md#customize-featurization).

En la tabla siguiente se muestran los valores aceptados para `featurization` en la clase [AutoMLConfig](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig). 

|Configuración de la caracterización | Descripción|
------------- | ------------- |
|**`"featurization": 'auto'`**| Indica que, como parte del preprocesamiento, los [pasos de caracterización y protección](#featurization) se realizan automáticamente. **Valor predeterminado**.|
|**`"featurization": 'off'`**| Indica que los pasos de caracterización no se deben realizar de forma automática.|
|**`"featurization":`&nbsp;`'FeaturizationConfig'`**| Indica que se debe usar un paso personalizado de caracterización. [Aprenda a personalizar la caracterización](#customize-featurization).|

<a name="featurization"></a>

## <a name="automatic-featurization"></a>Características automáticas

En la tabla siguiente se resumen las técnicas que se aplican automáticamente a los datos. Esto sucede en los experimentos configurados a través del SDK o de Studio. Para deshabilitar este comportamiento, establezca `"featurization": 'off'` en el objeto `AutoMLConfig`.

> [!NOTE]
> Si tiene previsto exportar los modelos creados por ML a un [modelo de ONNX](concept-onnx.md), solo se admiten las opciones de caracterización indicadas con * en el formato ONNX. Más información sobre la [conversión de modelos a ONNX](concept-automated-ml.md#use-with-onnx). 

|Pasos de caracterización&nbsp;| Descripción |
| ------------- | ------------- |
|**Eliminación de las características de cardinalidad alta o sin variación*** |Quítelas de los conjuntos de entrenamiento y validación, incluidas las características en las faltan todos los valores, que tienen el mismo valor en todas las filas o que tienen una cardinalidad alta (por ejemplo, hashes, identificadores o GUID).|
|**Atribución de los valores que faltan*** |Para las características numéricas, se atribuyen con el promedio de los valores de la columna.<br/><br/>Para las características de categorías, se atribuyen con el valor más frecuente.|
|**Generación de características adicionales*** |Para las características de fecha y hora: año, mes, día, día de la semana, día del año, trimestre, semana del año, hora, minuto, segundo.<br/><br/>Para las características de texto: Frecuencia de término basada en unigramas, bigramas y trigramas.|
|**Transformación y codificación***|Las características numéricas con pocos valores únicos se transforman en características de categorías.<br/><br/>La codificación "one-hot" se realiza para características de categorías de cardinalidad baja; para una cardinalidad alta, se lleva a cabo la codificación "one-hot-hash".|
|**Inserciones de palabras**|Caracterizador de texto que convierte los vectores de tokens de texto en vectores de oración mediante un modelo previamente entrenado. El vector de inserción de cada palabra en un documento se agrega para producir un vector de característica de documento.|
|**Codificaciones de destino**|Para características de categorías, se asigna a cada categoría con el valor de destino promedio para los problemas de regresión, y a la probabilidad de clase para cada clase para problemas de clasificación. La ponderación basada en la frecuencia y la validación cruzada de k iteraciones se aplican para reducir el ajuste excesivo de la asignación y el ruido causado por categorías de datos dispersos.|
|**Codificación de destino de texto**|Para la entrada de texto, se usa un modelo lineal apilado con bolsa de palabras para generar la probabilidad de cada clase.|
|**Ponderación de la evidencia (WoE)**|Calcula WoE como una medida de la correlación de las columnas de categorías para la columna de destino. Se calcula como el registro de la relación de las probabilidades dentro de la clase frente a las probabilidades fuera de la clase. Este paso genera una columna de característica numérica por clase y quita la necesidad de atribuir de forma explícita los valores que faltan y el tratamiento de valores atípicos.|
|**Distancia del clúster**|Entrena un modelo de agrupamiento k-medias en todas las columnas numéricas.  Genera k nuevas características, una nueva característica numérica por grupo, que contiene la distancia de cada muestra hasta el centroide de cada grupo.|

## <a name="data-guardrails"></a>Límites de protección de datos

Los límites de protección de datos le ayudan a identificar posibles problemas con los datos (por ejemplo, valores faltantes o [desequilibrio de clases](concept-manage-ml-pitfalls.md#identify-models-with-imbalanced-data)) y a realizar acciones correctivas para mejorar los resultados. 

Se aplican límites de protección de datos 

* **En el caso de los experimentos de SDK**, cuando se especifican los parámetros `"featurization": 'auto'` o `validation=auto` en el objeto `AutoMLConfig`.
* **En el caso de los experimentos de Studio**, cuando se habilita *Caracterización automática*.  

Puede examinar los límites de protección de datos que pertenecen al experimento

* Estableciendo `show_output=True` al enviar un experimento con el SDK de Python.

* En Studio, en la pestaña **Data guardrails** (Límites de protección de datos) de la ejecución del aprendizaje automático automatizado.

### <a name="data-guardrail-states"></a>Estados de límites de protección de datos

Los límites de protección de datos mostrarán uno de los tres estados siguientes: **Superado**, **Listo** o **Con alertas**.

|State| Descripción |
|----|---- |
|**Superado**| No se detectó ningún problema con los datos y no se requiere ninguna acción del usuario. |
|**Listo**| Los cambios se han aplicado a los datos. Se recomienda a los usuarios que revisen las acciones correctivas que realizó ML automatizado para asegurarse de que los cambios se alineen con los resultados esperados. |
|**Con alertas**| Se detectó un problema con los datos que no se pudo resolver. Se recomienda a los usuarios que revisen y solucionen el problema.| 

En la tabla siguiente se describen los límites de protección de datos admitidos actualmente, así como los estados asociados que pueden producirse al enviar el experimento.

Límite de protección|Estado|Condición&nbsp;para&nbsp;el desencadenador
---|---|---
**Atribución de los valores de características que faltan** |*Superado* <br><br><br> *Listo*| No se ha detectado que falten valores de característica en los datos de entrenamiento. Obtenga más información sobre la [imputación de valores que faltan](https://docs.microsoft.com/azure/machine-learning/how-to-use-automated-ml-for-ml-models#advanced-featurization-options). <br><br> Se detectaron valores de característica faltantes en los datos de entrenamiento y se imputaron.
**Control de características de cardinalidad alta** |*Superado* <br><br><br> *Listo*| Se han analizado las entradas y no se han detectado características de cardinalidad alta. Obtenga más información sobre la [detección de características de cardinalidad alta](https://docs.microsoft.com/azure/machine-learning/how-to-use-automated-ml-for-ml-models#advanced-featurization-options). <br><br> Se detectaron características de alta cardinalidad en las entradas y se controlaron.
**Control de división de validación** |*Listo*| La configuración de validación se estableció en "auto" y los datos de entrenamiento contenían **menos de 20 000 filas**. <br> Todas las iteraciones del modelo entrenado se han validado mediante validación cruzada. Más información sobre la [validación de datos.](https://docs.microsoft.com/azure/machine-learning/how-to-configure-auto-train#train-and-validation-data) <br><br> La configuración de validación se estableció en "auto" y los datos de entrenamiento contenían **más de 20 000 filas**. <br> Los datos de entrada se han dividido en un conjunto de datos de entrenamiento y un conjunto de datos de validación para comprobar el modelo.
**Detección de equilibrio de clases** |*Superado* <br><br><br><br><br> *Con alertas* | Se analizaron las entradas y todas las clases están equilibradas en los datos de entrenamiento. Se considera que un conjunto de datos está equilibrado si todas las clases tienen una representación adecuada en el conjunto de datos según el número y proporción de las muestras. <br><br><br> Se han detectado clases desequilibradas en las entradas. Para corregir el sesgo del modelo, corrija el problema de equilibrio. Más información sobre [datos desequilibrados.](https://docs.microsoft.com/azure/machine-learning/concept-manage-ml-pitfalls#identify-models-with-imbalanced-data)
**Detección de problemas de memoria** |*Superado* <br><br><br><br> *Listo* |<br> Se han analizado los valores seleccionados {horizonte, retardo y período acumulado} sin que se hayan detectado problemas potenciales de memoria insuficiente. Más información sobre las [configuraciones de previsión](https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-forecast#configure-and-run-experiment) de series temporales. <br><br><br>Los valores seleccionados {horizonte, retardo y período acumulado} se analizaron y pueden provocar que el experimento se quede sin memoria. Se desactivó la configuración de período acumulado o retardo.
**Detección de frecuencias** |*Superado* <br><br><br><br> *Listo* |<br> Se analizó la serie temporal y todos los puntos de datos están coordinados con la frecuencia detectada. <br> <br> Se analizó la serie temporal y se detectaron puntos de datos que no están coordinados con la frecuencia detectada. Estos puntos de datos se quitaron del conjunto de datos. Obtenga más información sobre la [preparación de datos para las previsiones de serie temporal](https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-forecast#preparing-data).

## <a name="customize-featurization"></a>Personalización de la caracterización

Puede personalizar los valores de la caracterización para asegurarse de que los datos y las características que se usan para entrenar el modelo de Machine Learning generan predicciones pertinentes. 

Para personalizar las caracterizaciones, especifique `"featurization": FeaturizationConfig` en el objeto `AutoMLConfig`. Si usa Azure Machine Learning Studio para el experimento, consulte el [procedimiento](how-to-use-automated-ml-for-ml-models.md#customize-featurization).

Entre las personalizaciones admitidas se incluyen:

|Personalización|Definición|
|--|--|
|**Actualización del propósito de la columna**|Reemplazar el tipo de característica para la columna especificada.|
|**Actualización de parámetros del transformador** |Actualizar los parámetros para el transformador especificado. Actualmente admite Imputer (media, más frecuente y mediana) y HashOneHotEncoder.|
|**Quitar columnas** |Las columnas que se van a quitar de la caracterización.|
|**Transformadores de bloque**| Los transformadores de bloque que se van a usar en el proceso de características.|

Cree el objeto FeaturizationConfig mediante llamadas a API:
```python
featurization_config = FeaturizationConfig()
featurization_config.blocked_transformers = ['LabelEncoder']
featurization_config.drop_columns = ['aspiration', 'stroke']
featurization_config.add_column_purpose('engine-size', 'Numeric')
featurization_config.add_column_purpose('body-style', 'CategoricalHash')
#default strategy mean, add transformer param for for 3 columns
featurization_config.add_transformer_params('Imputer', ['engine-size'], {"strategy": "median"})
featurization_config.add_transformer_params('Imputer', ['city-mpg'], {"strategy": "median"})
featurization_config.add_transformer_params('Imputer', ['bore'], {"strategy": "most_frequent"})
featurization_config.add_transformer_params('HashOneHotEncoder', [], {"number_of_bits": 3})
```
## <a name="next-steps"></a>Pasos siguientes

* Aprenda a configurar la cuenta del aprendizaje automático automatizado:

    * Para clientes con experiencia en codificación: [Configuración de experimentos de ML automatizado con el SDK de Azure Machine Learning](how-to-configure-auto-train.md).
    * Para clientes con poca o ninguna experiencia en codificación: [Creación de experimentos de aprendizaje automático automatizado en Azure Machine Learning Studio](how-to-use-automated-ml-for-ml-models.md).

* Obtenga más información sobre [cómo y dónde implementar un modelo](how-to-deploy-and-where.md).

* Obtenga más información sobre [cómo entrenar un modelo de regresión con aprendizaje automático automatizado](tutorial-auto-train-models.md) o [cómo entrenar con aprendizaje automático automatizado en un recurso remoto](how-to-auto-train-remote.md).
