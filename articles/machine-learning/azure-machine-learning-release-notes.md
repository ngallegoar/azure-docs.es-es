---
title: Novedades de la versión
titleSuffix: Azure Machine Learning
description: Conozca las actualizaciones más recientes de Azure Machine Learning y los SDK de Python para el aprendizaje automático y la preparación de datos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.author: jmartens
author: j-martens
ms.date: 03/10/2020
ms.openlocfilehash: 6bf26a739169c561e95c7376a75166daf9aa9fb0
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84309992"
---
# <a name="azure-machine-learning-release-notes"></a>Notas de la versión de Azure Machine Learning

En este artículo conocerá las versiones de Azure Machine Learning.  Para obtener el contenido completo de referencia del SDK, visite la página de referencia del [**SDK principal para Python**](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) de Azure Machine Learning.

Para obtener información acerca de errores conocidos y soluciones alternativas, consulte [la lista de problemas conocidos](resource-known-issues.md).

## <a name="2020-05-26"></a>26-05-2020

### <a name="azure-machine-learning-sdk-for-python-v160"></a>SDK de Azure Machine Learning para Python v1.6.0

+ **Nuevas características:**
  + **azureml-automl-runtime**
    + AutoML Forecasting ahora admite la previsión de clientes más allá del horizonte máximo especificado previamente sin necesidad de reentrenar el modelo. Si el destino de la previsión es posterior al horizonte máximo especificado, la función forecast() realizará predicciones puntuales hasta la fecha más alejada mediante un modo de operación recursivo. Para ver cómo se ilustra la nueva característica, consulte la sección "Previsión posterior al horizonte máximo" del cuaderno "forecasting-forecast-function" de la [carpeta](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning)".
  
  + **azureml-pipeline-steps**
    + Se ha publicado ParallelRunStep y forma parte del paquete **azureml-pipeline-steps**. La funcionalidad ParallelRunStep del paquete **azureml-contrib-pipeline-steps** está en desuso. Cambios en la versión preliminar pública:
      + Se ha agregado el parámetro configurable opcional `run_max_try` para controlar el número máximo de llamadas para ejecutar el método en un lote dado, el valor predeterminado es 3.
      + Ya no se generan PipelineParameters automáticamente. Los siguientes valores configurables se pueden establecer como PipelineParameter de forma explícita.
        + mini_batch_size
        + node_count
        + process_count_per_node
        + logging_level
        + run_invocation_timeout
        + run_max_try
      + El valor predeterminado de process_count_per_node se ha cambiado a 1. El usuario debe ajustar este valor para mejorar el rendimiento. El procedimiento recomendado es establecerlo en el número de GPU o CPU que tiene el nodo.
      + ParallelRunStep no inserta ningún paquete, el usuario debe incluir los paquetes **azureml-core** y **azureml-dataprep[pandas, fuse]** en la definición del entorno. Si la imagen de Docker personalizada se usa con user_managed_dependencies, el usuario debe instalar Conda en la imagen.
      
+ **Cambios importantes**
  + **azureml-pipeline-steps**
    + Ha quedado en desuso azureml.dprep.Dataflow como tipo válido de entrada para AutoMLConfig
  + **azureml-train-automl-client**
    + Ha quedado en desuso azureml.dprep.Dataflow como tipo válido de entrada para AutoMLConfig

+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Se ha corregido un error en el que aparecía una advertencia durante `get_output` que pedía al usuario cambiar a una versión anterior del cliente.
    + Se ha actualizado Mac para que use cudatoolkit=9.0, ya que aún no está disponible en la versión 10.
    + Se eliminan las restricciones en los modelos phrophet y xgboost cuando se entrenan en un proceso remoto.
    + Se ha mejorado el registro en AutoML.
    + Se ha mejorado el control de errores para la caracterización personalizada en las tareas de previsión.
    + Se ha agregado una funcionalidad que permite a los usuarios incluir características con desfase para generar previsiones.
    + Se han actualizado los mensajes de error para que muestren correctamente el error del usuario.
    + Se admite que cv_split_column_names se use con training_data
    + Se ha actualizado el registro del mensaje y seguimiento de la excepción.
  + **azureml-automl-runtime**
    + Se ha habilitado la protección para las imputaciones de los valores que faltan en la previsión.
    + Se ha mejorado el registro en AutoML.
    + Se ha agregado un control de errores minucioso para las excepciones que se produzcan en la preparación de datos.
    + Se eliminan las restricciones en los modelos phrophet y xgboost cuando se entrenan en un proceso remoto.
    + `azureml-train-automl-runtime` y `azureml-automl-runtime` han actualizado las dependencias de `pytorch`, `scipy` y `cudatoolkit`. Ahora se admiten `pytorch==1.4.0`, `scipy>=1.0.0,<=1.3.1` y `cudatoolkit==10.1.243`.
    + Se ha mejorado el control de errores para la caracterización personalizada en las tareas de previsión.
    + Se ha mejorado el mecanismo de detección de frecuencia del conjunto de datos de previsión.
    + Se ha corregido un problema con el entrenamiento del modelo Prophet en algunos conjuntos de datos.
    + Se ha mejorado la detección automática del horizonte máximo durante la previsión.
    + Se ha agregado una funcionalidad que permite a los usuarios incluir características con desfase para generar previsiones.
    +  Se han agregado funcionalidades en la función de pronostico que permite realizar previsiones más allá del horizonte entrenado sin tener que volver a entrenar el modelo de previsión.
    + Se admite que cv_split_column_names se use con training_data
  + **azureml-contrib-automl-dnn-forecasting**
    + Se ha mejorado el registro en AutoML.
  + **azureml-contrib-mir**
    + Se ha agregado compatibilidad con servicios de Windows en ManagedInferencing
    + Se han eliminado flujos de trabajo de MIR antiguos, como el de asociación de procesos de MIR, clase SingleModelMirWebservice. Se ha incluido un generador de perfiles de modelos limpio en el paquete contrib-mir.
  + **azureml-contrib-pipeline-steps**
    + Corrección secundaria relacionada con la compatibilidad con YAML.
    + ParallelRunStep se ha publicado con disponibilidad general: azureml.contrib.pipeline.steps tiene un aviso de desuso y se mueve a azureml.pipeline.steps
  + **azureml-contrib-reinforcementlearning**
    + Herramienta para pruebas de carga de aprendizaje reforzado
    + El estimador de aprendizaje reforzado tiene valores predeterminados inteligentes
  + **azureml-core**
    + Se han eliminado flujos de trabajo de MIR antiguos, como el de asociación de procesos de MIR, clase SingleModelMirWebservice. Se ha incluido un generador de perfiles de modelos limpio en el paquete contrib-mir.
    + Se ha corregido la información que se proporciona al usuario en caso de error de generación de perfiles: se ha incluido el identificador de solicitud y se ha rehecho el mensaje para que sea más significativo. Se ha agregado un nuevo flujo de trabajo de generación de perfiles a los ejecutores de generación de perfiles.
    + Se ha mejorado considerablemente el texto de error en el caso de los errores de ejecución de conjuntos de datos.
    + Se ha agregado compatibilidad con la CLI de Private Link del área de trabajo.
    + Se ha agregado un parámetro opcional `invalid_lines` a `Dataset.Tabular.from_json_lines_files` que permite especificar cómo administrar las líneas que contienen código JSON no válido.
    + La creación de procesos basada en la ejecución quedará en desuso en la próxima versión. Se recomienda crear un clúster de Amlcompute real como destino de proceso persistente y usar el nombre del clúster como destino de proceso en la configuración de ejecución. Consulte un cuaderno de ejemplo aquí: aka.ms/amlcomputenb
    + Se han mejorado considerablemente los mensajes de error en el caso de los errores de ejecución de conjuntos de datos.
  + **azureml-dataprep**
    + Se ha realizado una advertencia para actualizar la versión de pyarrow de forma más explícita.
    + Se ha mejorado el control de errores y el mensaje que se devuelve en caso de error al ejecutar el flujo de datos.
  + **azureml-interpret**
    + Actualizaciones de la documentación para el paquete azureml-interpret.
    + Se han reparado los paquetes y cuadernos de interpretación para que sean compatibles con la actualización más reciente de sklearn
  + **azureml-opendatasets**
    + Se devuelve None cuando no se devuelven datos.
    + Se ha mejorado el rendimiento de to_pandas_dataframe.
  + **azureml-pipeline-core**
    + Reparación rápida de ParallelRunStep cuando se ha interrumpido la carga desde YAML.
    + ParallelRunStep se ha publicado con disponibilidad general: azureml.contrib.pipeline.steps tiene un aviso de desuso y se mueve a azureml.pipeline.steps. Entre las características nuevas se incluyen: 1. Conjuntos de datos como PipelineParameter 2. Nuevo parámetro run_max_retry 3. Nombre de archivo de salida append_row configurable
  + **azureml-pipeline-steps**
    + Deprecated azureml.dprep.Dataflow como un tipo válido para los datos de entrada.
    + Reparación rápida de ParallelRunStep cuando se ha interrumpido la carga desde YAML.
    + ParallelRunStep se ha publicado con disponibilidad general: azureml.contrib.pipeline.steps tiene un aviso de desuso y se mueve a azureml.pipeline.steps. Entre las características nuevas se incluyen:
      + Conjuntos de datos como PipelineParameter
      + Nuevo parámetro run_max_retry
      + Nombre de archivo de salida append_row configurable
  + **azureml-telemetry**
    + Se ha actualizado el registro del mensaje y seguimiento de la excepción.
  + **azureml-train-automl-client**
    + Se ha mejorado el registro en AutoML.
    + Se han actualizado los mensajes de error para que muestren correctamente el error del usuario.
    + Se admite que cv_split_column_names se use con training_data
    + Deprecated azureml.dprep.Dataflow como un tipo válido para los datos de entrada.
    + Se ha actualizado Mac para que use cudatoolkit=9.0, ya que aún no está disponible en la versión 10.
    + Se eliminan las restricciones en los modelos phrophet y xgboost cuando se entrenan en un proceso remoto.
    + `azureml-train-automl-runtime` y `azureml-automl-runtime` han actualizado las dependencias de `pytorch`, `scipy` y `cudatoolkit`. Ahora se admiten `pytorch==1.4.0`, `scipy>=1.0.0,<=1.3.1` y `cudatoolkit==10.1.243`.
    + Se ha agregado una funcionalidad que permite a los usuarios incluir características con desfase para generar previsiones.
  + **azureml-train-automl-runtime**
    + Se ha mejorado el registro en AutoML.
    + Se ha agregado un control de errores minucioso para las excepciones que se produzcan en la preparación de datos.
    + Se eliminan las restricciones en los modelos phrophet y xgboost cuando se entrenan en un proceso remoto.
    + `azureml-train-automl-runtime` y `azureml-automl-runtime` han actualizado las dependencias de `pytorch`, `scipy` y `cudatoolkit`. Ahora se admiten `pytorch==1.4.0`, `scipy>=1.0.0,<=1.3.1` y `cudatoolkit==10.1.243`.
    + Se han actualizado los mensajes de error para que muestren correctamente el error del usuario.
    + Se admite que cv_split_column_names se use con training_data
  + **azureml-train-core**
    + Se ha agregado un conjunto nuevo de excepciones específicas de HyperDrive. azureml.train.hyperdrive ahora generará excepciones detalladas.
  + **azureml-widgets**
    + Los widgets de AzureML no aparecen en JupyterLab.
  

## <a name="2020-05-11"></a>2020-05-11

### <a name="azure-machine-learning-sdk-for-python-v150"></a>SDK de Azure Machine Learning para Python v1.5.0

+ **Nuevas características:**
  + **Características en versión preliminar**
    + **azureml-contrib-reinforcementlearning**
        + Azure Machine Learning ofrecerá compatibilidad en versión preliminar con el aprendizaje de refuerzo mediante el marco [Ray](https://ray.io). `ReinforcementLearningEstimator` permite el entrenamiento de los agentes de aprendizaje de refuerzo entre destinos de proceso de la CPU y la GPU en Azure Machine Learning.

+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + Se corrigió un registro de advertencia que quedó activo por error en la solicitud de incorporación de cambios anterior. El registro se usó para la depuración y quedó activo por error.
    + Corrección del error: se informa a los clientes del error parcial durante la generación de perfiles.
  + **azureml-automl-core**
    + Se ha acelerado el modelo Prophet/AutoArima en la previsión de AutoML mediante la habilitación del ajuste paralelo para la serie temporal cuando los conjuntos de datos tienen varias series temporales. Para beneficiarse de esta nueva característica, le recomendamos que establezca "max_cores_per_iteration = -1" (es decir, que use todos los núcleos de la CPU disponibles) en AutoMLConfig.
    + Se corrigió la excepción KeyError al imprimir los límites de protección en la interfaz de la consola.
    + Se corrigió el mensaje de error de experimentation_timeout_hours.
    + Se dejaron de usar los modelos de TensorFlow para AutoML.
  + **azureml-automl-runtime**
    + Se corrigió el mensaje de error de experimentation_timeout_hours.
    + Se corrigió una excepción no clasificada al intentar realizar una deserialización desde el almacén de caché.
    + Se ha acelerado el modelo Prophet/AutoArima en la previsión de AutoML mediante la habilitación del ajuste paralelo para la serie temporal cuando los conjuntos de datos tienen varias series temporales.
    + Se corrigió la previsión con el periodo acumulado habilitado en los conjuntos de datos en los que el conjunto de pruebas o predicción no contiene uno de los intervalos del conjunto de entrenamiento.
    + Se mejoró la administración de los datos que faltan.
    + Se corrigió un problema con los intervalos de predicción durante la previsión en los conjuntos de datos, que contienen series temporales y que no están alineados en el tiempo.
    + Se agregó una validación mejorada de la forma de los datos para las tareas de previsión.
    + Se mejoró la detección de la frecuencia.
    + Se ha creado un mensaje de error mejorado que se muestra cuando no se puede generar la validación cruzada de iteraciones para las tareas de previsión.
    + Se corrigió la interfaz de la consola para imprimir el valor del límite de protección que falta correctamente.
    + Se aplicaron comprobaciones de tipos de datos en la entrada cv_split_indices de la clase AutoMLConfig.
  + **azureml-cli-common**
    + Corrección del error: se informa a los clientes del error parcial durante la generación de perfiles.
  + **azureml-contrib-mir**
    + Se agregó la clase azureml.contrib.mir.RevisionStatus, que retransmite información sobre la revisión de MIR implementada actualmente y la versión más reciente especificada por el usuario. Esta clase se incluye en el objeto MirWebservice en el atributo "deployment_status".
    + Se habilitó la actualización en Webservices del tipo MirWebservice y su clase secundaria SingleModelMirWebservice.
  + **azureml-contrib-reinforcementlearning**
    + Se agregó compatibilidad con Ray 0.8.3.
    + AmlWindowsCompute solo admite Azure Files como almacenamiento montado.
    + Se cambió el nombre de health_check_timeout por health_check_timeout_seconds.
    + Se corrigieron algunas descripciones de clases o métodos.
  + **azureml-core**
    + Se habilitaron las conversiones de WASB a blob en las nubes USGovernment y de China.
    + Se corrigió el error para permitir que los roles de lector usen los comandos az ml run de la CLI para obtener información de ejecución.
    + Se quitó el registro innecesario durante las ejecuciones remotas de Azure ML con conjuntos de datos de entrada.
    + RCranPackage ahora admite el parámetro "version" para la versión del paquete de CRAN.
    + Corrección del error: se informa a los clientes del error parcial durante la generación de perfiles.
    + Se agregó el control float de estilo europeo para azureml-core.
    + Se han habilitado características de Private Link del área de trabajo en el SDK de Azure ML.
    + Al crear un elemento TabularDataset con `from_delimited_files`, puede especificar si los valores vacíos se deben cargar como Ninguno o como una cadena vacía mediante el establecimiento del argumento booleano `empty_as_string`.
    + Se agregó el control float de estilo europeo para los conjuntos de datos.
    + Se mejoraron los mensajes de error en los errores de montaje del conjunto de datos.
  + **azureml-datadrift**
    + La consulta de resultados del desfase de datos del SDK producía un error en el que no se diferenciaban las métricas de características mínimas, máximas y medias, lo que daba lugar a valores duplicados. Para corregir este error, hemos agregado los prefijos "target" (destino) o "baseline" (base de referencia) a los nombres de las métricas. Los valores anteriores eran los siguientes: duplicate min, max, mean. Ahora son los siguientes: target_min, target_max, target_mean, baseline_min, baseline_max, baseline_mean.
  + **azureml-dataprep**
    + Se mejoró el control de los entornos de Python con la escritura restringida al garantizar las dependencias de .NET necesarias para la entrega de datos.
    + Se corrigió la creación del flujo de datos registrado con registros vacíos iniciales.
    + Se agregaron opciones de control de errores para `to_partition_iterator` similares a `to_pandas_dataframe`.
  + **azureml-interpret**
    + Se redujeron los límites de longitud de la ruta de acceso de explicación a fin de disminuir la probabilidad de superar el límite de Windows.
    + Se corrigió un error con las explicaciones dispersas creadas con la explicación de imitación mediante un modelo lineal suplente.
  + **azureml-opendatasets**
    + Se ha corregido el problema por el que las columnas MNIST se analizaban como si fueran una cadena, en lugar de como valores enteros.
  + **azureml-pipeline-core**
    + Se permitió la opción regenerate_outputs al usar un módulo insertado en una clase ModuleStep.
  + **azureml-train-automl-client**
    + Se dejaron de usar los modelos de TensorFlow para AutoML.
    + Se corrigieron los algoritmos no compatibles de la lista blanca de usuarios en modo local.
    + Se realizaron correcciones de la clase AutoMLConfig en la documentación.
    + Se aplicaron comprobaciones de tipos de datos en la entrada cv_split_indices de la clase AutoMLConfig.
    + Se corrigió el problema con el error de ejecución de AutoML en show_output.
  + **azureml-train-automl-runtime**
    + Se ha corregido un error con las iteraciones de conjunto que impedía que el tiempo de espera de descarga de modelos se iniciara correctamente.
  + **azureml-train-core**
    + Se corrigió un error ortográfico en la clase azureml.train.dnn.Nccl.
    + Se agregó compatibilidad con la versión 1.5 de PyTorch en el estimador de PyTorch.
    + Se ha corregido el problema en el que la imagen del marco no se podía capturar en la región de Fairfax al usar los estimadores del marco de entrenamiento.

  
## <a name="2020-05-04"></a>04-05-2020
**Nueva experiencia de cuaderno**

Ya puede crear, editar y compartir archivos y cuadernos de aprendizaje automático directamente dentro de la experiencia de web de Studio de Azure Machine Learning. Puede usar todas las clases y métodos disponibles en el [SDK de Python de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) desde estos cuadernos. Empiece [aquí](https://docs.microsoft.com/azure/machine-learning/how-to-run-jupyter-notebooks).

**Nuevas características presentadas:**

+ Editor mejorado (editor Monaco) usado por VS Code 
+ Mejoras de interfaz de usuario y experiencia del usuario
+ Barra de herramientas de celdas
+ Controles de Proceso y nueva barra de herramientas del cuaderno
+ Barra de estado del cuaderno 
+ Conmutación en línea de Kernel
+ Compatibilidad con R
+ Mejoras en la accesibilidad y la localización
+ Paleta de comandos
+ Métodos abreviados de teclado adicionales
+ Autoguardado
+ Rendimiento y confiabilidad mejorados

Acceda a las siguientes herramientas de creación basadas en web desde Studio:
    
| Herramienta basada en web  |     Descripción  | Edición | 
|---|---|---|
| Cuadernos de Azure ML Studio   |     Creación de primera clase de archivos de cuadernos y compatibilidad con todas las operaciones disponibles en el SDK de Python de Azure ML. | Basic y Enterprise  |   

## <a name="2020-04-27"></a>27-04-2020

### <a name="azure-machine-learning-sdk-for-python-v140"></a>SDK de Azure Machine Learning para Python v1.4.0

+ **Nuevas características:**
  + Los clústeres de AmlCompute ahora admiten la configuración de una identidad administrada en el clúster en el momento del aprovisionamiento. Simplemente, especifique si quiere usar una identidad asignada por el sistema o asignada por el usuario y, si opta por esta última, pase un valor de identityId. Después, puede configurar permisos para acceder a varios recursos, como Storage o ACR, de forma que la identidad del proceso se use para acceder de forma segura a los datos, en lugar del enfoque basado en tokens que AmlCompute emplea en la actualidad. Consulte nuestra referencia de SDK para obtener más información sobre los parámetros.
  

+ **Cambios importantes**
  + Los clústeres de AmlCompute admitían una característica en vista previa en torno a la creación basada en la ejecución que estamos planeando retirar en dos semanas. Puede continuar con la creación de destinos de proceso persistentes como siempre mediante la clase Amlcompute, pero el enfoque concreto para especificar el identificador "amlcompute" como destino de proceso en la configuración de ejecución no se admitirá en un futuro próximo. 

+ **Mejoras y correcciones de errores**
  + **azureml-automl-runtime**
    + Habilitar la compatibilidad para un tipo sin hash al calcular el número de valores únicos de una columna.
  + **azureml-core**
    + Estabilidad mejorada al leer desde Azure Blob Storage mediante TabularDataset.
    + Documentación mejorada para el parámetro `grant_workspace_msi` para `Datastore.register_azure_blob_store`.
    + Se corrigió el error con `datastore.upload` para admitir el argumento `src_dir` que termina con `/` o `\`.
    + Se ha agregado un mensaje de error accionable al intentar cargar en un almacén de datos de Azure Blob Storage que no tiene una clave de acceso ni un token de SAS.
  + **azureml-interpret**
    + Se ha agregado el límite superior al tamaño de archivo para los datos de visualización en las explicaciones cargadas.
  + **azureml-train-automl-client**
    + Comprobando explícitamente si hay parámetros label_column_name y weight_column_name para que AutoMLConfig sea del tipo cadena.
  + **azureml-contrib-pipeline-steps**
    + Ahora, ParallelRunStep admite ahora el parámetro de conjunto de datos como canalización. El usuario puede construir una canalización con un conjunto de datos de muestra y cambiar el conjunto de datos de entrada del mismo tipo (archivo o tabular) para la nueva ejecución de canalización.

  
## <a name="2020-04-13"></a>2020-04-13

### <a name="azure-machine-learning-sdk-for-python-v130"></a>SDK de Azure Machine Learning para Python v1.3.0

+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Se ha agregado telemetría adicional en torno a las operaciones posteriores al entrenamiento.
    + Acelera el aprendizaje automático de ARIMA mediante el entrenamiento de suma condicional de cuadrados (CSS) para la serie de longitud superior a 100. Tenga en cuenta que la longitud usada se almacena como la constante ARIMA_TRIGGER_CSS_TRAINING_LENGTH w/en la clase TimeSeriesInternal en /src/azureml-automl-core/azureml/automl/core/shared/constants.py
    + Se mejoró el registro de usuario de las ejecuciones de previsión; ahora se mostrará más información sobre qué fase se está ejecutando actualmente en el registro.
    + No se permite que target_rolling_window_size se establezca en valores inferiores a 2.
  + **azureml-automl-runtime**
    + Se ha mejorado el mensaje de error que se muestra cuando se encuentran marcas de tiempo duplicadas.
    + No se permite que target_rolling_window_size se establezca en valores inferiores a 2.
    + Se corrigió el error de imputación de retraso. El problema se debe a un número insuficiente de observaciones para descomponer una serie de forma estacional. Los datos "desestacionalizados" se usan para calcular una función de autocorrelación parcial (PACF) para determinar la longitud de retardo.
    + Se ha habilitado la personalización de caracterización de propósito de columna para las tareas de previsión mediante la configuración de caracterización. Ahora se admite el propósito de numérica y categórica como columna para las tareas de previsión.
    + Se ha habilitado la personalización de caracterización de eliminación de columnas para las tareas de previsión mediante la configuración de caracterización.
    + Se ha habilitado la personalización de imputación para las tareas de previsión mediante la configuración de caracterización. Ahora se admite la imputación de valores constantes para las columnas de destino y la imputación de valores de media, mediana, más frecuente y constante para los datos de entrenamiento.
  + **azureml-contrib-pipeline-steps**
    + Se acepta que los nombres de proceso de cadena se pasen a ParallelRunConfig.
  + **azureml-core**
    +  Se ha agregado la API Environment.clone(new_name) para crear una copia del objeto Environment.
    +  Environment.docker.base_dockerfile acepta filepath. Si puede resolver un archivo, el contenido se leerá en la propiedad del entorno base_dockerfile.
    + Se restablecen automáticamente los valores mutuamente excluyentes para base_image y base_dockerfile cuando el usuario establece manualmente un valor en Environment.docker.
    + Se agregó la marca user_managed en RSection, que indica si el entorno lo administra el usuario o AzureML.
    + Conjunto de datos: se ha corregido el error de descarga del conjunto de datos cuando la ruta de acceso a datos contiene caracteres Unicode.
    + Conjunto de datos: se ha mejorado el mecanismo de almacenamiento en caché del montaje de conjunto de datos para respetar el requisito de espacio en disco mínimo de Proceso de Azure Machine Learning, lo que evita que el nodo quede inutilizable y que se cancele el trabajo.
    + Conjunto de datos: se ha agregado un índice para la columna de serie temporal cuando se accede a un conjunto de datos de serie temporal en forma de tramas de datos de Pandas, que se usan para acelerar el acceso a los datos basado en la serie temporal.  Anteriormente, se daba al índice el mismo nombre que a la columna de marca de tiempo, lo que confundía a los usuarios sobre cuál era la columna de marca de tiempo real y cuál era el índice. Ahora no se proporciona ningún nombre específico para el índice, ya que no debe usarse como columna. 
    + Conjunto de datos: Se ha corregido el problema de autenticación del conjunto de datos en la nube soberana.
    + Conjunto de datos: Se ha corregido un error de `Dataset.to_spark_dataframe` para los conjuntos de datos creados desde almacenes de datos de Azure PostgreSQL.
  + **azureml-interpret**
    + Se han agregado puntuaciones globales a la visualización si los valores de importancia local están dispersos.
    + Se ha actualizado azureml-interpret para usar interpret-community 0.9.*
    + Se ha corregido un problema con la descarga de la explicación que tenía datos de evaluación dispersos.
    + Se ha agregado compatibilidad con el formato disperso del objeto de explicación en AutoML.
  + **azureml-pipeline-core**
    + Compatibilidad con ComputeInstance como destino de proceso en las canalizaciones.
  + **azureml-train-automl-client**
    + Se ha agregado telemetría adicional en torno a las operaciones posteriores al entrenamiento.
    + Se ha corregido la regresión en la detención temprana.
    + Deprecated azureml.dprep.Dataflow como un tipo válido para los datos de entrada.
    +  Se ha cambiado el tiempo de espera predeterminado del experimento de AutoML a seis días.
  + **azureml-train-automl-runtime**
    + Se ha agregado telemetría adicional en torno a las operaciones posteriores al entrenamiento.
    + Se ha agregado compatibilidad con sparse AutoML e2e
  + **azureml-opendatasets**
    + Se ha agregado telemetría adicional para el monitor de servicio.
    + Se ha habilitado la puerta principal para blobs para aumentar la estabilidad. 

## <a name="2020-03-23"></a>2020-03-23

### <a name="azure-machine-learning-sdk-for-python-v120"></a>SDK de Azure Machine Learning para Python v1.2.0

+ **Cambios importantes**
  + Se ha retirado la compatibilidad con Python 2.7.

+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + Agrega "--subscription-id" a los comandos `az ml model/computetarget/service` en la CLI.
    + Se ha agregado compatibilidad para pasar la clave administrada por el cliente (CMK) vault_url, key_name y key_version para la implementación de ACI.
  + **azureml-automl-core** 
    + Se ha habilitado la imputación personalizada con un valor constante para las tareas de previsión de datos X e Y.
    + Se ha corregido el problema que mostraba los mensajes de error al usuario.    
  + **azureml-automl-runtime**
    + Se ha corregido el problema con la previsión en los conjuntos de datos, que contenía granos con una sola fila.
    + Se ha disminuido la cantidad de memoria necesaria para las tareas de previsión.
    + Se han agregado mejores mensajes de error si la columna de tiempo tiene un formato incorrecto.
    + Se ha habilitado la imputación personalizada con un valor constante para las tareas de previsión de datos X e Y.
  + **azureml-core**
    + Se ha agregado compatibilidad para cargar ServicePrincipal desde las variables de entorno: AZUREML_SERVICE_PRINCIPAL_ID, AZUREML_SERVICE_PRINCIPAL_TENANT_ID y AZUREML_SERVICE_PRINCIPAL_PASSWORD
    + Se ha presentado un nuevo parámetro `support_multi_line` en `Dataset.Tabular.from_delimited_files`: De forma predeterminada (`support_multi_line=False`), todos los saltos de línea, incluidos los de los valores de campo entre comillas, se interpretarán como un salto de registro. Leer los datos de esta manera es una forma más rápida y optimizada de ejecutar en paralelo en varios núcleos de CPU. Sin embargo, puede provocar la generación silenciosa de más registros con valores de campo mal alineados. Debe establecerse en `True` cuando se sabe que los archivos delimitados contienen saltos de línea entre comillas.
    + Se ha agregado la capacidad de registrar ADLS Gen2 en la CLI de Azure Machine Learning
    + Se ha cambiado el nombre del parámetro "fine_grain_timestamp" a "timestamp", y el parámetro "coarse_grain_timestamp" a "partition_timestamp" para el método with_timestamp_columns () de TabularDataset con el objetivo de reflejar mejor el uso de los parámetros.
    + Se ha aumentado la longitud máxima del nombre del experimento en 255.
  + **azureml-interpret**
    + Se ha actualizado azureml-interpret a interpret-community 0.7*.
  + **azureml-sdk**
    + Se han cambiado las dependencias con una versión compatible de Tilde para agregar compatibilidad de la revisión de parches en versiones preliminares y estables.


## <a name="2020-03-11"></a>11-03-2020

### <a name="azure-machine-learning-sdk-for-python-v115"></a>SDK de Azure Machine Learning para Python v1.1.5

+ **Característica en desuso**
  + **Python 2.7**
    + Última versión compatible con Python 2.7

+ **Cambios importantes**
  + **Versionamiento Semántico 2.0.0**
    + A partir de la versión 1.1, el SDK de Python para Azure Machine Learning adopta Versionamiento Semántico 2.0.0. [Obtenga más información aquí.](https://semver.org/) Todas las versiones posteriores seguirán un nuevo esquema de numeración y un contrato de Versionamiento Semántico. 

+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + Se ha cambiado el nombre del comando de la CLI del punto de conexión de "az ml endpoint aks" a "az ml endpoint real time" para que sea coherente.
    + Se han actualizado las instrucciones de instalación de la CLI para la CLI de rama estable y experimental.
    + La generación de perfiles de instancia única se ha corregido para generar una recomendación y se ha puesto a disposición en el SDK de .NET Core.
  + **azureml-automl-core**
    + Se ha habilitado la inferencia del modo de lotes (tomando varias filas a la vez) para los modelos ONNX de AutoML.
    + Se ha mejorado la detección de frecuencia en los conjuntos de datos, de la falta de datos o que contienen puntos de datos irregulares.
    + Se ha agregado la capacidad de quitar los puntos de datos que no cumplen con la frecuencia dominante.
    + Se ha cambiado la entrada del constructor para tomar una lista de opciones para aplicar las opciones de imputación para las columnas correspondientes.
    + El registro de errores se ha mejorado.
  + **azureml-automl-runtime**
    + Se ha corregido el problema con el error que se producía si el intervalo de agregación no estaba presente en el conjunto de entrenamiento que aparecía en el conjunto de pruebas.
    + Se ha quitado el requisito y_query durante la puntuación en el servicio de previsión.
    + Se ha corregido el problema con la previsión que se producía cuando el conjunto de datos contenía períodos cortos de agregación en intervalos de tiempo prolongados.
    + Se ha corregido el problema que se producía cuando se activaba el horizonte máximo automático y la columna de fecha contenía fechas en forma de cadenas. Se han agregado mensajes de error y conversión adecuados para cuando la conversión a fecha no es posible.
    + Uso de NumPy y SciPy nativos para serializar y deserializar los datos intermedios para FileCacheStore (se usa para las ejecuciones de AutoML locales)
    + Se ha corregido un problema por el que las ejecuciones secundarias con error se quedaban bloqueadas en el estado "En ejecución".
    + Mayor velocidad de caracterización.
    + Se ha corregido la comprobación de frecuencia durante la puntuación. Ahora, en las tareas de previsión, no se requiere una equivalencia de frecuencia estricta entre el conjunto de entrenamiento y de prueba.
    + Se ha cambiado la entrada del constructor para tomar una lista de opciones para aplicar las opciones de imputación para las columnas correspondientes.
    + Se han corregido errores relacionados con la selección del tipo de retardo.
    + Se ha corregido el error no clasificado generado en los conjuntos de datos, que tenían intervalos con la fila única.
    + Se ha corregido la lentitud de la detección de frecuencia.
    + Se ha corregido un error en el control de excepciones de AutoML que provocaba que el motivo real de un error de entrenamiento se reemplazara por un error de atributo.
  + **azureml-cli-common**
    + La generación de perfiles de instancia única se ha corregido para generar una recomendación y se ha puesto a disposición en el SDK de .NET Core.
  + **azureml-contrib-mir**
    + Agrega funcionalidad en la clase MirWebservice para recuperar el token de acceso.
    + Se usa la autenticación de tokens para MirWebservice de forma predeterminada durante la llamada de MirWebservice.run(). Solo se actualiza si se produce un error en la llamada.
    + La implementación de MirWebservice ahora requiere SKU adecuadas[Standard_DS2_v2, Standard_F16, Standard_A2_v2] en lugar de [Ds2v2, A2v2 y F16] respectivamente.
  + **azureml-contrib-pipeline-steps**
    + Se ha agregado el parámetro opcional side_inputs a ParallelRunStep. Este parámetro se puede utilizar para montar la carpeta en el contenedor. Los tipos que se admiten actualmente son DataReference y PipelineData.
    + Los parámetros pasados en ParallelRunConfig ahora se pueden sobrescribir pasando parámetros de canalización. Se admiten los nuevos parámetros de canalización siguientes: aml_mini_batch_size, aml_error_threshold, aml_logging_level y aml_run_invocation_timeout (aml_node_count y aml_process_count_per_node ya formaban parte de una versión anterior).
  + **azureml-core**
    + Los servicios web implementados de AzureML ahora tendrán como valor predeterminado el registro `INFO`. Esto se puede controlar estableciendo la variable de entorno `AZUREML_LOG_LEVEL` en el servicio implementado.
    + El SDK de Python usa el servicio de detección para usar el punto de conexión "API" en lugar de "canalizaciones".
    + Intercambio a las nuevas rutas en todas las llamadas del SDK.
    + Cambios en el enrutamiento de llamadas a ModelManagementService a una nueva estructura unificada.
      + El método de actualización de áreas de trabajo se ha puesto a disposición del público.
      + Se agregó el parámetro image_build_compute al método de actualización de áreas de trabajo para permitir que el usuario actualice el proceso para la compilación de imágenes.
    + Se han agregado mensajes de desuso al flujo de trabajo de generación de perfiles antiguos. Se han corregido los límites de CPU y memoria de generación de perfiles.
    + Se ha agregado RSection como parte del entorno para ejecutar trabajos de R.
    + Se ha agregado validación a `Dataset.mount` para generar un error cuando el origen del conjunto de datos no esté accesible o no contenga datos.
    + Se ha agregado `--grant-workspace-msi-access` como parámetro adicional para la CLI del almacén de datos para registrar el contenedor de blobs de Azure, lo que le permitirá registrar el contenedor de blobs que está detrás de una red virtual.
    + La generación de perfiles de instancia única se ha corregido para generar una recomendación y se ha puesto a disposición en el SDK de .NET Core.
    + Se ha corregido el problema en aks.py _deploy.
    + Valida la integridad de los modelos que se cargan para evitar errores de almacenamiento silenciosos.
    + Ahora, el usuario puede especificar un valor para la clave de autenticación al volver a generar las claves para los servicios web.
    + Se ha corregido un error por el que las letras mayúsculas no se podían usar como nombre de entrada de un conjunto de datos.
  + **azureml-defaults**
    + `azureml-dataprep` se instalará ahora como parte de `azureml-defaults`. Ya no es necesario instalar data prep[fuse] manualmente en los destinos de proceso para montar conjuntos de datos.
  + **azureml-interpret**
    + Se ha actualizado azureml-interpret a interpret-community 0.6*.
    + Se ha actualizado azureml-interpret para que dependa de interpret-community 0.5.0.
    + Se han agregado excepciones de azureml-style a azureml-interpret.
    + Se ha corregido la serialización de DeepScoringExplainer para los modelos de Keras.
  + **azureml-mlflow**
    + Se ha agregado compatibilidad para nubes soberanas a azureml.mlflow.
  + **azureml-pipeline-core**
    + El cuaderno de puntuación por lotes de la canalización ahora usa ParallelRunStep
    + Se ha corregido un error que hacía que los resultados de PythonScriptStep se reutilizaran incorrectamente a pesar de cambiar la lista de argumentos.
    + Se ha agregado la capacidad de establecer el tipo de las columnas cuando se llama a los métodos parse_ * en `PipelineOutputFileDataset`.
  + **azureml-pipeline-steps**
    + Se ha movido `AutoMLStep` al paquete `azureml-pipeline-steps`. Se ha puesto en desuso `AutoMLStep` en `azureml-train-automl-runtime`.
    + Se ha agregado un ejemplo de documentación para el conjunto de datos como entrada PythonScriptStep.
  + **azureml-tensorboard**
    + Se ha actualizado azureml-tensorboard para que admita Tensorflow 2.0
    + Se muestra el número de puerto correcto al usar un puerto Tensorboard personalizado en una instancia de proceso.
  + **azureml-train-automl-client**
    + Se ha corregido un problema por el que algunos paquetes se instalaban en versiones incorrectas en ejecuciones remotas.
    + Se ha corregido el problema de invalidación de FeaturizationConfig que filtra la configuración de características personalizada.
  + **azureml-train-automl-runtime**
    + Se ha corregido el problema con la detección de frecuencia en las ejecuciones remotas.
    + Se ha movido `AutoMLStep` del paquete `azureml-pipeline-steps`. Se ha puesto en desuso `AutoMLStep` en `azureml-train-automl-runtime`.
  + **azureml-train-core**
    + Compatibilidad con la versión 1.4 de PyTorch en el estimador de PyTorch
  
## <a name="2020-03-02"></a>2020-03-02

### <a name="azure-machine-learning-sdk-for-python-v112rc0-pre-release"></a>SDK de Azure Machine Learning para Python v1.1.2rc0 (versión preliminar)

+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Se ha habilitado la inferencia del modo de lotes (tomando varias filas a la vez) para los modelos ONNX de AutoML.
    + Se ha mejorado la detección de frecuencia en los conjuntos de datos, de la falta de datos o que contienen puntos de datos irregulares.
    + Se ha agregado la capacidad de quitar los puntos de datos que no cumplen con la frecuencia dominante.
  + **azureml-automl-runtime**
    + Se ha corregido el problema con el error que se producía si el intervalo de agregación no estaba presente en el conjunto de entrenamiento que aparecía en el conjunto de pruebas.
    + Se ha quitado el requisito y_query durante la puntuación en el servicio de previsión.
  + **azureml-contrib-mir**
    + Agrega funcionalidad en la clase MirWebservice para recuperar el token de acceso.
  + **azureml-core**
    + Los servicios web implementados de AzureML ahora tendrán como valor predeterminado el registro `INFO`. Esto se puede controlar estableciendo la variable de entorno `AZUREML_LOG_LEVEL` en el servicio implementado.
    + Se ha corregido la iteración en `Dataset.get_all` para devolver todos los conjuntos de datos registrados en el área de trabajo.
    + Se ha mejorado el mensaje de error cuando se pasa un tipo no válido al argumento `path` de las API de creación del conjunto de datos.
    + El SDK de Python usa el servicio de detección para usar el punto de conexión "API" en lugar de "canalizaciones".
    + Intercambio a las nuevas rutas en todas las llamadas del SDK.
    + Cambios en el enrutamiento de llamadas a ModelManagementService a una nueva estructura unificada.
      + El método de actualización de áreas de trabajo se ha puesto a disposición del público.
      + Se agregó el parámetro image_build_compute al método de actualización de áreas de trabajo para permitir que el usuario actualice el proceso para la compilación de imágenes.
    +  Se han agregado mensajes de desuso al flujo de trabajo de generación de perfiles antiguos. Se han corregido los límites de CPU y memoria de generación de perfiles.
  + **azureml-interpret**
    + Se ha actualizado azureml-interpret a interpret-community 0.6*.
  + **azureml-mlflow**
    + Se ha agregado compatibilidad para nubes soberanas a azureml.mlflow.
  + **azureml-pipeline-steps**
    + `AutoMLStep` se ha movido a `azureml-pipeline-steps package`. Se ha puesto en desuso `AutoMLStep` en `azureml-train-automl-runtime`.
  + **azureml-train-automl-client**
    + Se ha corregido un problema por el que algunos paquetes se instalaban en versiones incorrectas en ejecuciones remotas.
  + **azureml-train-automl-runtime**
    + Se ha corregido el problema con la detección de frecuencia en las ejecuciones remotas.
    + `AutoMLStep` se ha movido a `azureml-pipeline-steps package`. Se ha puesto en desuso `AutoMLStep` en `azureml-train-automl-runtime`.
  + **azureml-train-core**
    + `AutoMLStep` se ha movido a `azureml-pipeline-steps package`. Se ha puesto en desuso `AutoMLStep` en `azureml-train-automl-runtime`.

## <a name="2020-02-18"></a>18-02-2020

### <a name="azure-machine-learning-sdk-for-python-v111rc0-pre-release"></a>SDK de Azure Machine Learning para Python v1.1.1rc0 (versión preliminar)

+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + La generación de perfiles de instancia única se ha corregido para generar una recomendación y se ha puesto a disposición en el SDK de .NET Core.
  + **azureml-automl-core**
    + El registro de errores se ha mejorado.
  + **azureml-automl-runtime**
    + Se ha corregido el problema con la previsión que se producía cuando el conjunto de datos contenía períodos cortos de agregación en intervalos de tiempo prolongados.
    + Se ha corregido el problema que se producía cuando se activaba el horizonte máximo automático y la columna de fecha contenía fechas en forma de cadenas. Hemos agregado una conversión adecuada y la opción de que aparezca un error si la conversión a fecha no es posible
    + Uso de NumPy y SciPy nativos para serializar y deserializar los datos intermedios para FileCacheStore (se usa para las ejecuciones de AutoML locales)
    + Se ha corregido un problema por el que las ejecuciones secundarias con error se quedaban bloqueadas en el estado "En ejecución".
  + **azureml-cli-common**
    + La generación de perfiles de instancia única se ha corregido para generar una recomendación y se ha puesto a disposición en el SDK de .NET Core.
  + **azureml-core**
    + Se ha agregado `--grant-workspace-msi-access` como parámetro adicional para la CLI del almacén de datos para registrar el contenedor de blobs de Azure, lo que le permitirá registrar el contenedor de blobs que está detrás de una red virtual.
    + La generación de perfiles de instancia única se ha corregido para generar una recomendación y se ha puesto a disposición en el SDK de .NET Core.
    + Se ha corregido el problema en aks.py _deploy
    + Valida la integridad de los modelos que se cargan para evitar errores de almacenamiento silenciosos.
  + **azureml-interpret**
    + Se han agregado excepciones de azureml-style a azureml-interpret
    + Se ha corregido la serialización de DeepScoringExplainer para los modelos de Keras
  + **azureml-pipeline-core**
    + El cuaderno de puntuación por lotes de la canalización ahora usa ParallelRunStep
  + **azureml-pipeline-steps**
    + Se ha movido `AutoMLStep` del paquete `azureml-pipeline-steps`. Se ha puesto en desuso `AutoMLStep` en `azureml-train-automl-runtime`.
  + **azureml-contrib-pipeline-steps**
    + Se ha agregado el parámetro opcional side_inputs a ParallelRunStep. Este parámetro se puede utilizar para montar la carpeta en el contenedor. Los tipos que se admiten actualmente son DataReference y PipelineData.
  + **azureml-tensorboard**
    + Se ha actualizado azureml-tensorboard para que admita Tensorflow 2.0
  + **azureml-train-automl-client**
    + Se ha corregido el problema de invalidación de FeaturizationConfig que filtra la configuración de características personalizada.
  + **azureml-train-automl-runtime**
    + Se ha movido `AutoMLStep` del paquete `azureml-pipeline-steps`. Se ha puesto en desuso `AutoMLStep` en `azureml-train-automl-runtime`.
  + **azureml-train-core**
    + Compatibilidad con la versión 1.4 de PyTorch en el estimador de PyTorch
  
## <a name="2020-02-04"></a>04-02-2020

### <a name="azure-machine-learning-sdk-for-python-v110rc0-pre-release"></a>SDK de Azure Machine Learning para Python v1.1.0rc0 (versión preliminar)

+ **Cambios importantes**
  + **Versionamiento Semántico 2.0.0**
    + A partir de la versión 1.1, el SDK de Python para Azure Machine Learning adopta Versionamiento Semántico 2.0.0. [Obtenga más información aquí.](https://semver.org/) Todas las versiones posteriores seguirán un nuevo esquema de numeración y un contrato de Versionamiento Semántico. 
  
+ **Mejoras y correcciones de errores**
  + **azureml-automl-runtime**
    + Mayor velocidad de caracterización.
    + Se corrigió la comprobación de frecuencia durante la puntuación. Ahora, en las tareas de previsión, no se requiere una equivalencia de frecuencia estricta entre el conjunto de entrenamiento y de prueba.
  + **azureml-core**
    + Ahora, el usuario puede especificar un valor para la clave de autenticación al volver a generar las claves para los servicios web.
  + **azureml-interpret**
    + Se ha actualizado azureml-interpret para que dependa de interpret-community 0.5.0.
  + **azureml-pipeline-core**
    + Se ha corregido un error que hacía que los resultados de PythonScriptStep se reutilizaran incorrectamente a pesar de cambiar la lista de argumentos.
  + **azureml-pipeline-steps**
    + Se ha agregado un ejemplo de documentación para el conjunto de datos como entrada PythonScriptStep.
  + **azureml-contrib-pipeline-steps**
    + Los parámetros pasados en ParallelRunConfig ahora se pueden sobrescribir pasando parámetros de canalización. Se admiten los nuevos parámetros de canalización siguientes: aml_mini_batch_size, aml_error_threshold, aml_logging_level y aml_run_invocation_timeout (aml_node_count y aml_process_count_per_node ya formaban parte de una versión anterior).
  
## <a name="2020-01-21"></a>2020-01-21

### <a name="azure-machine-learning-sdk-for-python-v1085"></a>SDK de Azure Machine Learning para Python v1.0.85

+ **Nuevas características:**
  + **azureml-core**
    + Obtenga el uso principal actual y la limitación de cuota para los recursos de AmlCompute en un área de trabajo y una suscripción determinadas
  
  + **azureml-contrib-pipeline-steps**
    + Permita al usuario pasar un conjunto de datos tabulares como resultado intermedio del paso anterior a parallelrunstep

+ **Mejoras y correcciones de errores**
  + **azureml-automl-runtime**
    + Se ha quitado el requisito de la columna y_query de la solicitud al servicio de previsión implementado. 
    + "y_query" se ha quitado de la sección de solicitud de servicio del cuaderno Orange Juice de Dominick.
    + Se ha corregido el error que impide la previsión en los modelos implementados y que funciona en conjuntos de datos con columnas de fecha y hora.
    + Se ha agregado el coeficiente de correlación de Matthews como una métrica de clasificación, tanto para la clasificación binaria como la multiclase.
  + **azureml-contrib-interpret**
    + Se han eliminado las explicaciones de texto de azureml-contrib-interpret, ya que la explicación del texto se ha movido al repositorio interpret-text que se publicará pronto.
  + **azureml-core**
    + Conjunto de datos: los usos del conjunto de datos de archivo ya no dependen de que Numpy y Pandas estén instalados en el entorno de Python.
    + Se ha cambiado LocalWebservice.wait_for_deployment () para comprobar el estado del contenedor de Docker local antes de intentar hacer ping a su punto de conexión de mantenimiento, lo que reduce considerablemente el tiempo que se tarda en informar de un error de implementación.
    + Se ha corregido la inicialización de una propiedad interna usada en LocalWebservice.reload() cuando el objeto de servicio se crea a partir de una implementación existente mediante el constructor LocalWebservice ().
    + Mensaje de error editado para clarificación.
    + Se ha agregado un nuevo método llamado get_access_token () a AksWebservice que devolverá el objeto AksServiceAccessToken, que contiene el token de acceso, la actualización después de la marca de tiempo, la expiración en la marca de tiempo y el tipo de token. 
    + El método get_token () existente está en desuso en AksWebservice, ya que el nuevo método devuelve toda la información que devuelve este método.
    + Se ha modificado la salida del comando get-access-token de az ml service. Se ha cambiado el nombre del token a accessToken y de refreshBy a refreshAfter. Se han agregado las propiedades expiryOn y tokenType.
    + Se ha reparado get_active_runs
  + **azureml-explain-model**
    + se ha actualizado shap a la versión 0.33.0 e interpret-community a la 0.4.*
  + **azureml-interpret**
    + se ha actualizado shap a la versión 0.33.0 e interpret-community a la 0.4.*
  + **azureml-train-automl-runtime**
    + Se ha agregado el coeficiente de correlación de Matthews como una métrica de clasificación, tanto para la clasificación binaria como la multiclase.
    + Se ha dejado en desuso la marca de preprocesamiento del código y se ha reemplazado por características-características está activada de forma predeterminada

## <a name="2020-01-06"></a>2020-01-06

### <a name="azure-machine-learning-sdk-for-python-v1083"></a>SDK de Azure Machine Learning para Python v 1.0.83

+ **Nuevas características:**
  + Conjunto de datos: Agregue dos opciones `on_error` y `out_of_range_datetime` para que se produzca un error en `to_pandas_dataframe` cuando los datos tengan valores de error en lugar de llenarlos con `None`.
  + Área de trabajo: Se ha agregado la marca `hbi_workspace` para las áreas de trabajo con información confidencial que habilita un cifrado adicional y deshabilita el diagnóstico avanzado en áreas de trabajo. También hemos agregado compatibilidad con la posibilidad de traer sus propias claves para la instancia de Cosmos DB asociada, mediante la especificación de los parámetros `cmk_keyvault` y `resource_cmk_uri` al crear un área de trabajo, lo que crea una instancia de Cosmos DB en su suscripción mientras aprovisiona el área de trabajo. [Obtenga más información aquí.](https://docs.microsoft.com/azure/machine-learning/concept-enterprise-security#azure-cosmos-db)

+ **Mejoras y correcciones de errores**
  + **azureml-automl-runtime**
    + Se corrigió una regresión que provocaba un TypeError cuando se ejecutaba AutoML en las versiones de Python inferiores a 3.5.4.
  + **azureml-core**
    + Se corrigió un error en `datastore.upload_files` donde la ruta de acceso relativa que no se inició con `./` no se podía usar.
    + Se han agregado mensajes de desuso para toda la clase de imagen rutas
    + Se ha corregido la construcción de URL de Administración de modelos para la región Azure China 21Vianet.
    + Se corrigió un problema en el que los modelos que usan source_dir no se podían empaquetar para Azure Functions.    
    + Se ha agregado una opción a [Environment.build_local ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) para introducir una imagen en el registro de contenedor del área de trabajo de AzureML
    + Se actualizó el SDK para usar la nueva biblioteca de tokens en Azure Synapse de forma que sea compatible con versiones anteriores.
  + **azureml-interpret**
    + Se corrigió un error en el que no se devolvió ninguno cuando no había ninguna explicación disponible para su descarga. Ahora genera una excepción, que coincide con otro lugar.
  + **azureml-pipeline-steps**
    + No se permite pasar `DatasetConsumptionConfig`s al parámetro `Estimator` de `inputs` cuando se utiliza el `Estimator` en una `EstimatorStep`.
  + **azureml-sdk**
    + Se ha agregado el cliente AutoML al paquete azureml-SDK, lo que permite que se envíen los AutoML remotos sin necesidad de instalar el paquete completo de AutoML.
  + **azureml-train-automl-client**
    + Se corrigió la alineación en la salida de la consola para ejecuciones de AutoML.
    + Se corrigió un error que hacía que la versión incorrecta de pandas se instalara en amlcompute remoto.

## <a name="2019-12-23"></a>2019-12-23

### <a name="azure-machine-learning-sdk-for-python-v1081"></a>SDK de Azure Machine Learning para Python v 1.0.81

+ **Mejoras y correcciones de errores**
  + **azureml-contrib-interpret**
    + aplazar la dependencia de la forma a interpretar-comunidad de azureml-interpretar
  + **azureml-core**
    + El destino de proceso ahora se puede especificar como un parámetro para los objetos de configuración de implementación correspondientes. Es específicamente el nombre del destino de proceso en el que se va a realizar la implementación, no el objeto SDK.
    + Se agregó información de CreatedBy al modelo y a los objetos de servicio. Se puede obtener acceso a través de <var>. created_by
    + Se corrigió ContainerImage.run(), que no configuraba correctamente el puerto HTTP del contenedor de Docker.
    + Se ha hecho que `azureml-dataprep` sea opcional para el comando de la CLI `az ml dataset register`.
    + Se corrigió un error por el cual `TabularDataset.to_pandas_dataframe` volvía incorrectamente a un lector alternativo e imprimía una advertencia.
  + **azureml-explain-model**
    + aplazar la dependencia de la forma a interpretar-comunidad de azureml-interpretar
  + **azureml-pipeline-core**
    + Se ha agregado un nuevo paso de canalización `NotebookRunnerStep` para ejecutar un cuaderno local como un paso de la canalización.
    + Se quitaron funciones desusadas get_all para PublishedPipelines, programaciones y PipelineEndpoints
  + **azureml-train-automl-client**
    + Se inició el desuso de data_script como entrada para AutoML.


## <a name="2019-12-09"></a>2019-12-09

### <a name="azure-machine-learning-sdk-for-python-v1079"></a>SDK de Azure Machine Learning para Python v1.0.79

+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Se quitó el registro de featurizationConfig
      + Registro actualizado para registrar solo "auto"/"off"/"customized".
  + **azureml-automl-runtime**
    + Se agregó compatibilidad con pandas. Series y pandas. Categoría para detectar el tipo de datos de la columna. Anteriormente solo se admitía numpy.ndarray.
      + Se agregaron cambios de código relacionados para administrar dtype categórico correctamente.
    + Se ha mejorado la interfaz de la función de pronóstico: el parámetro y_pred se hizo opcional. -Se mejoraron las propiedades docstring.
  + **azureml-contrib-dataset**
    + Se corrigió un error donde los conjuntos de datos con etiqueta no se podían montar.
  + **azureml-core**
    + Corrección de errores de `Environment.from_existing_conda_environment(name, conda_environment_name)`. El usuario puede crear una instancia del entorno que sea una réplica exacta del entorno local.
    + Se cambiaron los métodos de conjuntos de datos relacionados con series temporales por `include_boundary=True` de forma predeterminada.
  + **azureml-train-automl-client**
    + Se corrigió un problema donde los resultados de validación no se imprimían cuando show output se establecía en false.


## <a name="2019-11-25"></a>2019-11-25

### <a name="azure-machine-learning-sdk-for-python-v1076"></a>SDK de Azure Machine Learning para Python v1.0.76

+ **Cambios importantes**
  + Problemas de actualización de Azureml-Train-AutoML
    + La actualización a una versión superior o igual a azureml-train-automl 1.0.76 desde una versión inferior puede generar instalaciones parciales, lo que provocaría el error de algunas importaciones de AutoML. Para solucionar este problema, puede ejecutar el script de configuración que se encuentra en https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/automl_setup.cmd. O bien, si usa PIP directamente, puede usar lo siguiente:
      + "pip install --upgrade azureml-train-automl"
      + "pip install --ignore-installed azureml-train-automl-client"
    + También puede desinstalar la versión anterior antes de actualizar:
      + "pip uninstall azureml-train-automl"
      + "pip install azureml-train-automl"

+ **Mejoras y correcciones de errores**
  + **azureml-automl-runtime**
    + AutoML ahora tendrá en cuenta las clases true y false al calcular las métricas escalares medias para tareas de clasificación binaria.
    + Se ha trasladado el código de entrenamiento y el aprendizaje automático en AzureML-AutoML-Core a un nuevo paquete, AzureML-AutoML-Runtime.
  + **azureml-contrib-dataset**
    + Cuando llama a `to_pandas_dataframe` en un conjunto de datos con etiqueta con la opción de descarga, ahora ya puede especificar si desea sobrescribir los archivos existentes o no.
    + Cuando se llama a `keep_columns` o `drop_columns` y el resultado es la eliminación de una columna de serie temporal, etiqueta o imagen, también se eliminan las funcionalidades correspondientes del conjunto de datos.
    + Se ha corregido un problema con el cargador de PyTorch para la tarea de detección de objetos.
  + **azureml-contrib-interpret**
    + Se ha quitado el widget de panel de explicación de azureml-contrib-interpretate; se ha cambiado el paquete para hacer referencia al nuevo en interpret_community.
    + Se ha actualizado la versión de interpret-community a 0.2.0.
  + **azureml-core**
    + Mejora del rendimiento de `workspace.datasets`.
    + Se ha agregado la capacidad de registrar el almacén de datos de Azure SQL Database mediante la autenticación de nombre de usuario y contraseña.
    + Corrección para cargar RunConfigurations desde rutas de acceso relativas.
    + Cuando se llama a `keep_columns` o `drop_columns` y el resultado es la eliminación de una columna de serie temporal, también se eliminan las funcionalidades correspondientes del conjunto de datos.
  + **azureml-interpret**
    + Se ha actualizado la versión de interpret-community a 0.2.0
  + **azureml-pipeline-steps**
    + Se han documentado los valores admitidos para `runconfig_pipeline_params` para los pasos de canalización de Azure Machine Learning.
  + **azureml-pipeline-core**
    + Se ha agregado la opción de CLI para descargar la salida en formato JSON para comandos de canalización.
  + **azureml-train-automl**
    + Se ha dividido AzureML-Train-AutoML en dos paquetes, un paquete de cliente AzureML-Train-AutoML-Client y un paquete de entrenamiento de Machine Learning AzureML-Train-AutoML-Runtime.
  + **azureml-train-automl-client**
    + Se ha agregado un cliente ligero para enviar experimentos de AutoML sin necesidad de instalar localmente dependencias de aprendizaje automático.
    + Se ha corregido el registro de retardos detectados automáticamente, tamaños de ventanas con desplazamiento y horizontes máximos en ejecuciones remotas.
  + **azureml-train-automl-runtime**
    + Se ha agregado un nuevo paquete AutoML para aislar los componentes de aprendizaje automático y tiempo de ejecución del cliente.
  + **azureml-contrib-train-rl**
    + Se ha agregado compatibilidad con aprendizaje de refuerzo en SDK.
    + Se ha agregado compatibilidad con AmlWindowsCompute en el SDK de RL.


## <a name="2019-11-11"></a>2019-11-11

### <a name="azure-machine-learning-sdk-for-python-v1074"></a>SDK de Azure Machine Learning para Python v1.0.74

  + **Características en versión preliminar**
    + **azureml-contrib-dataset**
      + Después de importar azureml-contrib-dataset, puede llamar a `Dataset.Labeled.from_json_lines` en lugar de `._Labeled` para crear un conjunto de datos con etiqueta.
      + Cuando llama a `to_pandas_dataframe` en un conjunto de datos con etiqueta con la opción de descarga, ahora ya puede especificar si desea sobrescribir los archivos existentes o no.
      + Cuando se llama a `keep_columns` o `drop_columns` y el resultado es la eliminación de una columna de serie temporal, etiqueta o imagen, también se eliminan las funcionalidades correspondientes del conjunto de datos.
      + Se han solucionado los problemas con el cargador de PyTorch cuando se llama a `dataset.to_torchvision()`.

+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + Se ha agregado la generación de perfiles de modelos a la versión preliminar de la CLI.
    + Soluciona un cambio importante en Azure Storage que provocaba que la CLI de Azure ML tuviera un error.
    + Se ha agregado un tipo de Load Balancer a MLC para tipos de AKS
  + **azureml-automl-core**
    + Se ha solucionado el problema con la detección del horizonte máximo de la serie temporal por el que faltaban valores y varios intervalos.
    + Se ha corregido el problema de los errores que se producían durante la generación de divisiones de validación cruzada.
    + Reemplace esta sección por un mensaje en formato Markdown para que aparezca en las notas de la versión: control mejorado de los intervalos cortos de los conjuntos de datos de previsión.
    + Se ha solucionado el problema con el enmascaramiento de parte de la información de usuario durante el registro. Se ha mejorado el registro de errores durante la ejecución de previsiones.
    + Se ha agregado Psutil como dependencia de Conda al archivo de implementación yml generado automáticamente.
  + **azureml-contrib-mir**
    + Soluciona un cambio importante en Azure Storage que provocaba que la CLI de Azure ML tuviera un error.
  + **azureml-core**
    + Se ha solucionado un error que provocaba que los modelos implementados en Azure Functions produjeran errores 500.
    + Se ha solucionado un problema que hacía que el archivo amlignore no se aplicara a las instantáneas.
    + Se ha agregado una nueva API amlcompute.get_active_runs que devuelve un generador para la ejecución y las ejecuciones en cola en un amlcompute determinado.
    + Se ha agregado un tipo de Load Balancer a MLC para tipos de AKS.
    + Se ha agregado un parámetro booleano append_prefix a download_files en run.py y a download_artifacts_from_prefix en artifacts_client. Esta marca se usa para simplificar selectivamente la ruta de acceso al archivo original, con lo que solo se agrega el nombre de archivo o carpeta a output_directory
    + Se ha solucionado el problema de deserialización de `run_config.yml` con el uso del conjunto de datos.
    + Cuando se llama a `keep_columns` o `drop_columns` y el resultado es la eliminación de una columna de serie temporal, también se eliminan las funcionalidades correspondientes del conjunto de datos.
  + **azureml-interpret**
    + Se ha actualizado interpretar la versión de interpret-community a 0.1.0.3
  + **azureml-train-automl**
    + Se ha solucionado un problema por el que en automl_step no se imprimían los problemas de validación.
    + Se ha solucionado un problema de register_model para que pueda completarse correctamente incluso si faltan dependencias localmente en el entorno del modelo.
    + Se ha solucionado un problema que hacía que algunas ejecuciones remotas no estuvieran habilitadas para Docker.
    + Agregue el registro de la excepción que está causando un error en una ejecución local prematuramente.
  + **azureml-train-core**
    + Tenga en cuenta las ejecuciones de resume_from en el cálculo de las mejores ejecuciones secundarias de optimización de hiperparámetros automatizada.
  + **azureml-pipeline-core**
    + Se ha corregido el control de parámetros en la construcción del argumento de una canalización.
    + Se ha agregado una descripción de la canalización y un parámetro yaml de tipo de paso.
    + Hay un nuevo formato yaml para el paso de canalización y se ha agregado una advertencia de desuso para el formato anterior.



## <a name="2019-11-04"></a>2019-11-04

### <a name="web-experience"></a>Experiencia web

La página de aterrizaje del área de trabajo de colaboración en [https://ml.azure.com](https://ml.azure.com) se ha mejorado y renombrado como Azure Machine Learning Studio (versión preliminar).

Desde Studio, puede entrenar, probar, implementar y administrar recursos de Azure Machine Learning, como conjuntos de datos, canalizaciones, modelos, puntos de conexión y mucho más.

Acceda a las siguientes herramientas de creación basadas en web desde Studio:

| Herramienta basada en web | Descripción | Edición |
|-|-|-|
| Máquina virtual de Notebook (versión preliminar) | Estación de trabajo totalmente administrada basada en la nube | Basic y Enterprise |
| [Aprendizaje automático automatizado](tutorial-first-experiment-automated-ml.md) (versión preliminar) | Experiencia sin código para automatizar el desarrollo de modelos de aprendizaje automático | Enterprise |
| [Diseñador](concept-designer.md) (versión preliminar) | Herramienta de modelado de aprendizaje automático de arrastrar y colocar conocida anteriormente como el diseñador | Enterprise |


### <a name="azure-machine-learning-designer-enhancements"></a>Mejoras del diseñador de Azure Machine Learning

+ Anteriormente conocido como la interfaz visual 
+    11 nuevos [módulos](algorithm-module-reference/module-reference.md) que incluyen utilidades de recomendación, clasificación y entrenamiento, como la ingeniería de características, la validación cruzada y la transformación de datos.

### <a name="r-sdk"></a>SDK de R 
 
Los científicos de datos y los desarrolladores de inteligencia artificial usan el [SDK de Azure Machine Learning para R](tutorial-1st-r-experiment.md) para crear y ejecutar flujos de trabajo de aprendizaje automático con Azure Machine Learning.

El SDK de Azure Machine Learning para R usa el paquete de `reticulate` para enlazar con el SDK de Python. Al enlazar directamente a Python, el SDK para R permite acceder a los objetos y métodos principales implementados en el SDK de Python desde cualquier entorno de R que elija.

Entre las principales capacidades del SDK se incluyen:

+    Administre los recursos en la nube para la supervisión, registro y organización de los experimentos de aprendizaje automático.
+    Entrenar modelos mediante recursos en la nube, incluido el entrenamiento del modelo de aceleración por GPU.
+    Implementar sus modelos como servicios web en Azure Container Instances (ACI) y Azure Kubernetes Service (AKS).

Para obtener la documentación completa, vaya al [sitio web del paquete](https://azure.github.io/azureml-sdk-for-r).

### <a name="azure-machine-learning-integration-with-event-grid"></a>Integración de Azure Machine Learning con Event Grid 

Azure Machine Learning es ahora un proveedor de recursos para Event Grid, puede configurar eventos de aprendizaje automático mediante el Azure Portal o el CLI de Azure. Los usuarios pueden crear eventos para la finalización de la ejecución, el registro del modelo, la implementación de modelo y el desplazamiento de datos. Estos eventos se pueden enrutar a los controladores de eventos admitidos por Event Grid para su consumo. Consulte los artículos del [esquema](https://docs.microsoft.com/azure/event-grid/event-schema-machine-learning) y el [tutorial](how-to-use-event-grid.md) del evento de aprendizaje automático para obtener más detalles.

## <a name="2019-10-31"></a>2019-10-31

### <a name="azure-machine-learning-sdk-for-python-v1072"></a>SDK de Azure Machine Learning para Python v1.0.72

+ **Nuevas características:**
  + Se han agregado supervisores de conjuntos de datos mediante el paquete [**azureml-datadrift**](https://docs.microsoft.com/python/api/azureml-datadrift), que permite supervisar a lo largo del tiempo el desfase de datos u otros cambios estadísticos en los conjuntos de datos de serie temporal. Se pueden desencadenar alertas y eventos si se detecta un desfase o se cumplen otras condiciones en los datos. Consulte [nuestra documentación](https://aka.ms/datadrift) para más información.
  + Anuncio de dos nuevas ediciones (también denominadas SKU indistintamente) en Azure Machine Learning. Con esta versión, puede crear un área de trabajo con el nivel Básico o Enterprise de Azure Machine Learning. Todas las áreas de trabajo existentes se establecerán de forma predeterminada en la edición Basic y puede ir a Azure Portal o Studio para actualizar el área de trabajo en cualquier momento. Puede crear un área de trabajo Basic o Enterprise desde Azure Portal. Lea [nuestra documentación](https://docs.microsoft.com/azure/machine-learning/how-to-manage-workspace) para obtener más información. Desde el SDK, se puede determinar la edición del área de trabajo mediante la propiedad "sku" del objeto de área de trabajo.
  + También hemos mejorado el proceso de Azure Machine Learning: ahora se pueden ver las métricas de los clústeres (como el total de nodos, los nodos en ejecución o la cuota total del núcleo) en Azure Monitor, además de ver los registros de diagnóstico para la depuración. Además, también puede ver las ejecuciones activas o en cola actualmente en el clúster y los detalles, como las direcciones IP de los distintos nodos del clúster. Puede verlos en el portal o mediante las funciones correspondientes en el SDK o la CLI.

  + **Características en versión preliminar**
    + Estamos lanzando soporte técnico para la versión preliminar del cifrado de disco de la SSD local en el proceso de Azure Machine Learning. Genere una incidencia de soporte técnico para que su suscripción se incluya en la lista blanca para usar esta característica.
    + Versión preliminar pública de la inferencia por lotes de Azure Machine Learning. La inferencia por lotes de Azure Machine Learning tiene como destino trabajos de inferencia grandes que no dependen del tiempo. La inferencia por lotes proporciona un escalado del proceso de inferencia rentable, con un rendimiento sin precedentes para aplicaciones asincrónicas. Está optimizada para la inferencia de alto rendimiento, de tipo "fire-and-forget" (envíelo y olvídese), en colecciones de datos de gran tamaño.
    + [**azureml-contrib-dataset**](https://docs.microsoft.com/python/api/azureml-contrib-dataset)
        + Habilitación de funcionalidades para el conjunto de datos etiquetado
        ```Python
        import azureml.core
        from azureml.core import Workspace, Datastore, Dataset
        import azureml.contrib.dataset
        from azureml.contrib.dataset import FileHandlingOption, LabeledDatasetTask

        # create a labeled dataset by passing in your JSON lines file
        dataset = Dataset._Labeled.from_json_lines(datastore.path('path/to/file.jsonl'), LabeledDatasetTask.IMAGE_CLASSIFICATION)

        # download or mount the files in the `image_url` column
        dataset.download()
        dataset.mount()

        # get a pandas dataframe
        from azureml.data.dataset_type_definitions import FileHandlingOption
        dataset.to_pandas_dataframe(FileHandlingOption.DOWNLOAD)
        dataset.to_pandas_dataframe(FileHandlingOption.MOUNT)

        # get a Torchvision dataset
        dataset.to_torchvision()
        ```

+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + La CLI ahora admite el empaquetado de modelos.
    + Agregación de una CLI de conjunto de datos. Para obtener más información: `az ml dataset --help`
    + Agregación de compatibilidad para la implementación y el empaquetado de modelos admitidos (ONNX, scikit-learn y TensorFlow) sin una instancia de InferenceConfig.
    + Agregación de una marca de sobrescritura para la implementación del servicio (ACI y AKS) en el SDK y la CLI. Si se proporciona, sobrescribirá el servicio existente si ya existe un servicio con ese nombre. Si el servicio no existe, creará un servicio nuevo.
    + Los modelos se pueden registrar con dos nuevos marcos, Onnx y Tensorflow. El registro del modelo acepta los datos de entrada de ejemplo, los datos de salida de ejemplo y la configuración de recursos para el modelo.
  + **azureml-automl-core**
    + El entrenamiento de una iteración se ejecutará en un proceso secundario solo cuando se establecen restricciones en tiempo de ejecución.
    + Agregación de una medida de protección para las tareas de previsión, para comprobar si un elemento max_horizon especificado producirá o no un problema de memoria en el equipo correspondiente. Si es así, se mostrará un mensaje de medida de protección.
    + Se ha agregado compatibilidad con frecuencias complejas como, por ejemplo, dos años y un mes. Se ha agregado un mensaje de error comprensible si no se puede determinar la frecuencia.
    + Agregación de un elemento azureml-defaults al entorno Conda generado automáticamente para resolver el error de implementación de modelo.
    + Se permite que los datos intermedios de la canalización de Azure Machine Learning se conviertan en un conjunto de datos tabular y se usen en `AutoMLStep`.
    + Implementación de la actualización del propósito de columna para streaming.
    + Implementación de la actualización de parámetros de transformador para Imputer y HashOneHotEncoder para streaming.
    + Agregación del tamaño de datos actual y el tamaño de datos mínimo necesario para los mensajes de error de validación.
    + Actualización del tamaño de datos mínimo necesario para la validación cruzada a fin de garantizar un mínimo de dos muestras en cada iteración de validación.
  + **azureml-cli-common**
    + La CLI ahora admite el empaquetado de modelos.
    + Los modelos se pueden registrar con dos nuevos marcos, Onnx y Tensorflow.
    + El registro del modelo acepta los datos de entrada de ejemplo, los datos de salida de ejemplo y la configuración de recursos para el modelo.
  + **azureml-contrib-gbdt**
    + Corrección del canal de versión del cuaderno
    + Se ha agregado una advertencia para destinos de proceso que no son de AmlCompute que no se admiten.
    + Agregación del estimador LightGMB al paquete azureml-contrib-gbdt
  + [**azureml-core**](https://docs.microsoft.com/python/api/azureml-core)
    + La CLI ahora admite el empaquetado de modelos.
    + Agregación de una advertencia de desuso para las API de conjunto de los conjuntos de datos. Consulte el aviso de cambio de API de conjunto de datos en https://aka.ms/tabular-dataset.
    + Cambie [`Dataset.get_by_id`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29#get-by-id-workspace--id-) para devolver el nombre y la versión del registro si el conjunto de datos está registrado.
    + Corrección del error por el que ScriptRunConfig con el conjunto de datos como argumento no se puede usar repetidamente para enviar la ejecución del experimento.
    + Se realizará un seguimiento de los conjuntos de datos recuperados durante una ejecución y estos se pueden ver en la página de detalles de la ejecución, o bien mediante una llamada a [`run.get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29#get-details--) una vez completada la ejecución.
    + Permita que los datos intermedios de la canalización de Azure Machine Learning se conviertan en un conjunto de datos tabular y que se usen en [`AutoMLStep`](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlstep).
    + Agregación de compatibilidad para la implementación y el empaquetado de modelos admitidos (ONNX, scikit-learn y TensorFlow) sin una instancia de InferenceConfig.
    + Agregación de una marca de sobrescritura para la implementación del servicio (ACI y AKS) en el SDK y la CLI. Si se proporciona, sobrescribirá el servicio existente si ya existe un servicio con ese nombre. Si el servicio no existe, creará un servicio nuevo.
    +  Los modelos se pueden registrar con dos nuevos marcos, Onnx y Tensorflow. El registro del modelo acepta los datos de entrada de ejemplo, los datos de salida de ejemplo y la configuración de recursos para el modelo.
    + Agregación de un nuevo almacén de datos para Azure Database for MySQL. Agregación de un ejemplo para usar Azure Database for MySQL en DataTransferStep en las canalizaciones de Azure Machine Learning.
    + Agregación de la funcionalidad para agregar y quitar etiquetas de experimentos. Agregación de la funcionalidad para quitar etiquetas de las ejecuciones
    + Agregación de una marca de sobrescritura para la implementación del servicio (ACI y AKS) en el SDK y la CLI. Si se proporciona, sobrescribirá el servicio existente si ya existe un servicio con ese nombre. Si el servicio no existe, creará un servicio nuevo.
  + [**azureml-datadrift**](https://docs.microsoft.com/python/api/azureml-datadrift)
    + Se ha movido de `azureml-contrib-datadrift` a `azureml-datadrift`
    + Se ha agregado compatibilidad con la supervisión de desfases y otras medidas estadísticas en conjuntos de datos de serie temporal.
    + Los nuevos métodos `create_from_model()` y `create_from_dataset()` de la clase [`DataDriftDetector`](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector(class)). Método `create()` en desuso.
    + Ajustes en las visualizaciones en Python y en la interfaz de usuario de Azure Machine Learning Studio.
    + Compatibilidad de la programación de supervisión semanal y mensual, además de la diaria para los supervisores de conjunto de datos.
    + Compatibilidad de la reposición de las métricas de supervisor de datos para analizar los datos históricos de los supervisores de conjunto de datos.
    + Varias correcciones de errores
  + [**azureml-pipeline-core**](https://docs.microsoft.com/python/api/azureml-pipeline-core)
    + azureml-dataprep ya no es necesario para enviar una ejecución de canalización de Azure Machine Learning desde el archivo de canalización `yaml`.
  + [**azureml-train-automl**](/python/api/azureml-train-automl-runtime/)
    + Agregación de un elemento azureml-defaults al entorno Conda generado automáticamente para resolver el error de implementación de modelo.
    + El aprendizaje remoto de AutoML ahora incluye azureml-defaults para permitir la reutilización del entorno de aprendizaje para la inferencia.
  + **azureml-train-core**
    + Agregación de compatibilidad con PyTorch 1.3 en el calculador de [`PyTorch`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch)

## <a name="2019-10-21"></a>2019-10-21

### <a name="visual-interface-preview"></a>Interfaz visual (versión preliminar)

+ La interfaz visual de Azure Machine Learning (versión preliminar) se ha revisado para ejecutarse en [canalizaciones de Azure Machine Learning](concept-ml-pipelines.md). Las canalizaciones (anteriormente conocidas como experimentos) creadas en la interfaz visual ahora están totalmente integradas con la experiencia principal de Azure Machine Learning.
  + Experiencia de administración unificada con recursos de SDK
  + Control de versiones y seguimiento de modelos de interfaz visual, canalizaciones y puntos de conexión
  + Interfaz de usuario rediseñada
  + Agregación de implementación de inferencia por lotes
  + Agregación de compatibilidad con Azure Kubernetes Service (AKS) para los destinos de proceso de inferencia
  + Nuevo flujo de trabajo de creación de canalizaciones de paso de Python
  + Nueva [página de aterrizaje](https://ml.azure.com) para herramientas de creación visual

+ **Nuevos módulos**
  + Aplicación de operación matemática
  + Aplicación de transformaciones de SQL
  + Recorte de valores
  + Resumen de datos
  + Importación desde Azure SQL Database

## <a name="2019-10-14"></a>2019-10-14

### <a name="azure-machine-learning-sdk-for-python-v1069"></a>SDK de Azure Machine Learning para Python v1.0.69

+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Limitación de las explicaciones de modelo a la mejor ejecución en vez de calcular las explicaciones para cada ejecución. Cambio de este comportamiento para el modo local, remoto y ADB.
    + Se ha agregado compatibilidad con las explicaciones del modelo a petición para la interfaz de usuario.
    + Agregación de psutil como dependencia de `automl` e inclusión de psutil como una dependencia de Conda en amlcompute.
    + Se corrigió el problema con intervalos heurísticos y los tamaños de ventanas graduales en los conjuntos de datos de previsión de los cuales algunas series pueden provocar errores de álgebra lineal.
      + Se ha agregado la impresión para los parámetros que se han determinado heurísticamente en las ejecuciones de previsión.
  + **azureml-contrib-datadrift**
    + Se ha agregado protección al crear métricas de salida si el desfase del nivel de conjunto de resultados no está en la primera sección.
  + **azureml-contrib-interpret**
    + Se ha cambiado el nombre del paquete azureml-contrib-explain a azureml-contrib-interpret.
  + **azureml-core**
    + Se ha agregado una API para cancelar el registro de conjuntos de registros. `dataset.unregister_all_versions()`
    + Se ha cambiado el nombre del paquete azureml-contrib-explain-model a azureml-contrib-interpret.
  + **[azureml-core](https://docs.microsoft.com/python/api/azureml-core)**
    + Se ha agregado una API para cancelar el registro de conjuntos de registros. dataset.[unregister_all_versions()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.abstract_datastore.abstractdatastore#unregister--).
    + Se ha agregado una API de conjunto de datos para comprobar la hora del cambio de los datos. `dataset.data_changed_time`.
    + Capacidad de consumir `FileDataset` y `TabularDataset` como entradas para `PythonScriptStep`, `EstimatorStep` y `HyperDriveStep` en la canalización de Azure Machine Learning.
    + Se ha mejorado el rendimiento de `FileDataset.mount` para las carpetas con un gran número de archivos.
    + Posibilidad de usar [FileDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset) y [TabularDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset) como entradas para [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep), [EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimatorstep) y [HyperDriveStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyperdrivestep) en la canalización de Azure Machine Learning.
    + Se ha mejorado el rendimiento de FileDataset.[mount()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset#mount-mount-point-none----kwargs-) para las carpetas con un gran número de archivos.
    + Se ha agregado la dirección URL a las recomendaciones de errores conocidas en los detalles de ejecución.
    + Se ha corregido un error en la ejecución run.get_metrics por el cual las solicitudes darían error si una ejecución tuviera demasiados elementos secundarios.
    + Se ha corregido un error en [run.get_metrics](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run#get-metrics-name-none--recursive-false--run-type-none--populate-false-) por el cual las solicitudes darían error si una ejecución tuviera demasiados elementos secundarios.
    + Se ha agregado compatibilidad para la autenticación en el clúster de Arcadia.
    + La creación de un objeto de experimento obtiene o crea el experimento en el área de trabajo Azure Machine Learning para el seguimiento del historial de ejecución. El id. de experimento y el tiempo de archivado se rellenan en el objeto de experimento al crearse. Ejemplo: experiment = Experiment(workspace, "New Experiment") experiment_id = experiment.id archive() y reactivate() son funciones a las que se puede llamar en un experimento para ocultar y volver a mostrar el experimento en la experiencia de usuario o para mostrarlo de forma predeterminada en una llamada para enumerar experimentos. Si se crea un nuevo experimento con el mismo nombre que un experimento archivado, puede cambiar el nombre del experimento archivado al reactivarlo proporcionando un nuevo nombre. Solo puede haber un experimento activo con un nombre determinado. Ejemplo: experiment1 = Experiment(workspace, "Active Experiment") experiment1.archive() # Crear un nuevo experimento activo con el mismo nombre que el archivado. experiment2. = Experiment(workspace, "Active Experiment") experiment1.reactivate(new_name="Previous Active Experiment") La lista de métodos estáticos () del experimento puede tomar un filtro de nombre y un filtro ViewType. Los valores ViewType son "ACTIVE_ONLY", "ARCHIVED_ONLY" y "ALL" Ejemplo: archived_experiments = Experiment.list(workspace, view_type="ARCHIVED_ONLY") all_first_experiments = Experiment.list(workspace, name="First Experiment", view_type="ALL")
    + Compatibilidad con el uso del entorno para la implementación de modelos y la actualización del servicio
  + **azureml-datadrift**
    + El atributo show de la clase DataDriftDector ha dejado de ser compatible con el argumento opcional "with_details". El atributo show solo presentará el coeficiente de desfase de datos y la contribución del desfase de datos de las columnas de características.
    + Cambios de comportamiento del atributo "get_output" de DataDriftDetector:
      + Los parámetros de entrada start_time, end_time son opcionales en lugar de obligatorios;
      + La especificación de un valor start_time o end_time particular con un run_id determinado en la misma invocación provocará una excepción de error de valor porque son mutualmente excluyentes.
      + Al indicar un valor específico de start_time o end_time, solo se devolverán los resultados de las ejecuciones programadas.
      + El parámetro "daily_latest_only" está en desuso.
    + Compatibilidad con la recuperación de salidas de desfase de datos basadas en conjuntos de datos.
  + **azureml-explain-model**
    + Cambia el nombre del paquete AzureML-explain-model a AzureML-interpret, manteniendo por ahora el paquete anterior por compatibilidad con versiones anteriores.
    + Se corrige el error `automl` consistente en la presencia de explicaciones sin procesar en la tarea de clasificación en lugar de regresión de forma predeterminada en la descarga de ExplanationClient.
    + Se agregó compatibilidad con `ScoringExplainer` para que se cree directamente con `MimicWrapper`.
  + **azureml-pipeline-core**
    + Se mejoró el rendimiento para la creación de canalizaciones de gran tamaño.
  + **azureml-train-core**
    + Se agregó compatibilidad con TensorFlow 2.0 en un estimador de TensorFlow.
  + **azureml-train-automl**
    + La creación de un objeto [Experimento](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment) obtiene o crea el experimento en el área de trabajo Azure Machine Learning para el seguimiento del historial de ejecución. El id. de experimento y el tiempo de archivado se rellenan en el objeto de experimento al crearse. Ejemplo:

        ```py
        experiment = Experiment(workspace, "New Experiment")
        experiment_id = experiment.id
        ```
        [archive()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment#archive--) y [reactivate()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment#reactivate-new-name-none-) son funciones a las que se puede llamar en un experimento para ocultar y volver a mostrar el experimento en la experiencia de usuario o para devolverlo de forma predeterminada en una llamada para enumerar experimentos. Si se crea un nuevo experimento con el mismo nombre que un experimento archivado, puede cambiar el nombre del experimento archivado al reactivarlo proporcionando un nuevo nombre. Solo puede haber un experimento activo con un nombre determinado. Ejemplo:

        ```py
        experiment1 = Experiment(workspace, "Active Experiment")
        experiment1.archive()
        # Create new active experiment with the same name as the archived.
        experiment2 = Experiment(workspace, "Active Experiment")
        experiment1.reactivate(new_name="Previous Active Experiment")
        ```
        El método estático [list()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment#list-workspace--experiment-name-none--view-type--activeonly---tags-none-) en el experimento puede tomar un filtro de nombre y un filtro ViewType. Los valores ViewType son "ACTIVE_ONLY", "ARCHIVED_ONLY" y "ALL". Ejemplo:

        ```py
        archived_experiments = Experiment.list(workspace, view_type="ARCHIVED_ONLY")
        all_first_experiments = Experiment.list(workspace, name="First Experiment", view_type="ALL")
        ```
    + Compatibilidad con el uso del entorno para la implementación de modelos y la actualización del servicio.
  + **[azureml-datadrift](https://docs.microsoft.com/python/api/azureml-datadrift)**
    + El atributo show de la clase [DataDriftDetector](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector.datadriftdetector) ha dejado de ser compatible con el argumento opcional "with_details". El atributo show solo presentará el coeficiente de desfase de datos y la contribución del desfase de datos de las columnas de características.
    + Cambios de comportamiento de la función [get_output]https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector.datadriftdetector#get-output-start-time-none--end-time-none--run-id-none-) de DataDriftDetector:
      + Los parámetros de entrada start_time, end_time son opcionales en lugar de obligatorios;
      + La especificación de un valor start_time o end_time particular con un run_id determinado en la misma invocación provocará una excepción de error de valor porque son mutualmente excluyentes.
      + Al indicar un valor específico de start_time o end_time, solo se devolverán los resultados de las ejecuciones programadas.
      + El parámetro "daily_latest_only" está en desuso.
    + Compatibilidad con la recuperación de salidas de desfase de datos basadas en conjuntos de datos.
  + **azureml-explain-model**
    + Cambia el nombre del paquete AzureML-explain-model a AzureML-interpret, manteniendo por ahora el paquete anterior por compatibilidad con versiones anteriores.
    + Se ha corregido el error AutoML consistente en la presencia de explicaciones sin procesar en la tarea de clasificación en lugar de regresión de forma predeterminada en la descarga de ExplanationClient.
    + Agregación de compatibilidad con [ScoringExplainer](/python/api/azureml-interpret/azureml.interpret.scoring.scoring_explainer.scoringexplainer?view=azure-ml-py) que se creará directamente con MimicWrapper
  + **[azureml-pipeline-core](https://docs.microsoft.com/python/api/azureml-pipeline-core)**
    + Mejora del rendimiento para la creación de canalizaciones de gran tamaño.
  + **[azureml-train-core](https://docs.microsoft.com/python/api/azureml-train-core)**
    + Agregación de compatibilidad con TensorFlow 2.0 en un estimador de [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow).
  + **[azureml-train-automl](/python/api/azureml-train-automl-runtime/)**
    + Ya no se producirá un error en la ejecución primaria cuando se produzca un error en la iteración de instalación, ya que la orquestación se encarga de ello.
    + Se agregó compatibilidad con local-docker y local-conda para los experimentos de AutoML.
    + Agregación de compatibilidad con local-docker y local-conda para los experimentos de AutoML.


## <a name="2019-10-08"></a>2019-10-08

### <a name="new-web-experience-preview-for-azure-machine-learning-workspaces"></a>Nueva experiencia web (versión preliminar) para áreas de trabajo de Azure Machine Learning

La pestaña Experimento del [nuevo portal de áreas de trabajo](https://ml.azure.com) se ha actualizado para que los científicos de datos puedan supervisar los experimentos de forma más eficaz. Puede explorar las características siguientes:
+ Los metadatos de experimentos para filtrar y ordenar fácilmente la lista de experimentos
+ Se han simplificado y mejorado las páginas de detalles de los experimentos que le permiten visualizar y comparar las ejecuciones.
+ Nuevo diseño para ejecutar páginas de detalles para comprender y supervisar las ejecuciones de entrenamiento

## <a name="2019-09-30"></a>2019-09-30

### <a name="azure-machine-learning-sdk-for-python-v1065"></a>SDK de Azure Machine Learning para Python v1.0.65

  + **Nuevas características:**
    + Incorporación de entornos mantenidos. Estos entornos se han configurado previamente con bibliotecas para tareas comunes de aprendizaje automático, y se han creado previamente y almacenado en caché como imágenes de Docker para que su ejecución sea más rápida. Aparecen de forma predeterminada en la lista de entornos del área de trabajo, con el prefijo "AzureML".
    + Incorporación de entornos mantenidos. Estos entornos se han configurado previamente con bibliotecas para tareas comunes de aprendizaje automático, y se han creado previamente y almacenado en caché como imágenes de Docker para que su ejecución sea más rápida. Aparecen de forma predeterminada en la lista de [Área de trabajo](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29), con el prefijo "AzureML".

  + **azureml-train-automl**
  + **[azureml-train-automl](/python/api/azureml-train-automl-runtime/)**
    + Incorporación de la compatibilidad con la conversión de ONNX para ADB y HDI

+ **Características en versión preliminar**
  + **azureml-train-automl**
  + **[azureml-train-automl](/python/api/azureml-train-automl-runtime/)**
    + BERT y BiLSTM admitidos como texto caracterizador (solo versión preliminar)
    + Personalización de características admitida para los parámetros de propósito de columna y transformador (solo versión preliminar)
    + Admisión de las explicaciones sin procesar cuando el usuario habilita la explicación del modelo durante el entrenamiento (solo versión preliminar)
    + Incorporación de Prophet para la previsión de `timeseries` como una canalización que se pueda entrenar (solo versión preliminar)

  + **azureml-contrib-datadrift**
    + Paquetes reubicados de azureml-contrib-datadrift a azureml-datadrift; el paquete `contrib` se quitará en una versión futura

+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Introducción de FeaturizationConfig a AutoMLConfig y AutoMLBaseSettings
    + Introducción de FeaturizationConfig en [AutoMLConfig](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig) y AutoMLBaseSettings
      + Invalidación del propósito de columna para características con la columna y el tipo de característica dados
      + Invalidación de los parámetros de transformador
    + Incorporación del mensaje de desuso para explain_model() y retrieve_model_explanations()
    + Incorporación de Prophet como una canalización que se puede entrenar (solo versión preliminar)
    + Incorporación del mensaje de desuso para explain_model() y retrieve_model_explanations().
    + Incorporación de Prophet como una canalización que se puede entrenar (solo versión preliminar).
    + Incorporación de la compatibilidad para la detección automática de los intervalos de destino, el tamaño del periodo acumulado y el horizonte máximo. Si uno de los valores de target_lags, target_rolling_window_size o max_horizon está establecido como "automático", se aplicará la heurística para calcular el valor del parámetro correspondiente en base a los datos de entrenamiento.
    + Previsión fija en el caso de que el conjunto de datos contenga una columna de grano, este grano es de un tipo numérico y hay un intervalo entre el conjunto de entrenamiento y el de pruebas
    + Corrección del mensaje de error sobre el índice duplicado en la ejecución remota en las tareas de previsión
    + Previsión fija en el caso de que el conjunto de datos contenga una columna de grano, este grano es de un tipo numérico y hay un intervalo entre el conjunto de entrenamiento y el de pruebas.
    + Corrección del mensaje de error sobre el índice duplicado en la ejecución remota en las tareas de previsión.
    + Incorporación de una barrera protectora para comprobar si un conjunto de datos está desequilibrado o no. Si es así, se escribirá un mensaje de barrera protectora en la consola.
  + **azureml-core**
    + Incorporación de la capacidad de recuperar la dirección URL de SAS para el modelo en el almacenamiento mediante el objeto de modelo. Ejemplo: model.get_sas_url()
    + Introducción de `run.get_details()['datasets']` para obtener los conjuntos de datos asociados a la ejecución enviada
    + Incorporación de la API `Dataset.Tabular.from_json_lines_files` para crear un TabularDataset a partir de archivos de líneas JSON. Para más información sobre estos datos tabulares en archivos de líneas JSON en TabularDataset, acuda a https://aka.ms/azureml-data para obtener documentación.
    + Incorporación de campos de tamaño de máquina virtual adicionales (disco del sistema operativo, número de los GPU) a la función supported_vmsizes ()
    + Incorporación de campos adicionales a la función list_nodes () para mostrar la ejecución, las direcciones IP privada y pública, el puerto, etc.
    + Posibilidad de especificar un campo nuevo durante el aprovisionamiento de clústeres: remotelogin_port_public_access, que se puede establecer en habilitado o deshabilitado,dependiendo de si desea dejar el puerto SSH abierto o cerrado en el momento de crear el clúster. Si no lo especifica, el servicio abrirá o cerrará el puerto de forma inteligente, en función de si va a implementar el clúster dentro de una red virtual.
  + **azureml-explain-model**
  + **[azureml-core](https://docs.microsoft.com/python/api/azureml-core/azureml.core)**
    + Incorporación de la capacidad de recuperar la dirección URL de SAS para el modelo en el almacenamiento mediante el objeto de modelo. Ex: model.[get_sas_url()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model#get-sas-urls--)
    + Introducción de run.[get_details](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29#get-details--)['datasets'] para obtener los conjuntos de datos asociados a la ejecución enviada
    + Incorporación de la API `Dataset.Tabular`.[from_json_lines_files()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory#from-json-lines-files-path--validate-true--include-path-false--set-column-types-none--partition-format-none-) para crear un TabularDataset a partir de archivos de líneas JSON. Para más información sobre estos datos tabulares en archivos de líneas JSON en TabularDataset, acuda a https://aka.ms/azureml-data para obtener documentación.
    + Incorporación de campos de tamaño de máquina virtual adicionales (disco del sistema operativo, número de GPU) a la función [supported_vmsizes()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#supported-vmsizes-workspace--location-none-).
    + Incorporación de campos adicionales a la función [list_nodes()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#list-nodes--) para mostrar la ejecución, las direcciones IP privada y pública, el puerto, etc.
    + Posibilidad de especificar un campo nuevo durante el [aprovisionamiento](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none--remote-login-port-public-access--notspecified--) `--remotelogin_port_public_access` que se puede establecer en habilitado o deshabilitado, en función de si desea dejar el puerto SSH abierto o cerrado en el momento de crear el clúster. Si no lo especifica, el servicio abrirá o cerrará el puerto de forma inteligente, en función de si va a implementar el clúster dentro de una red virtual.
  + **azureml-explain-model**
    + Documentación mejorada para las salidas de explicación en el escenario de clasificación.
    + Incorporación de la capacidad de cargar los valores y previstos en la explicación de los ejemplos de evaluación. Desbloquea visualizaciones más útiles.
    + Incorporación de la propiedad de explicación a MimicWrapper para habilitar la obtención de MimicExplainer subyacente.
  + **azureml-pipeline-core**
    + Se agregó un cuaderno para describir Module, ModuleVersion y ModuleStep.
  + **azureml-pipeline-steps**
    + Se ha agregado RScriptStep para admitir la ejecución de scripts de R mediante la canalización de AML.
    + Se han corregido los parámetros de metadatos que se analizan en AzureBatchStep que provocaban el mensaje de error "assignment for parameter SubscriptionId is not specified" (no se ha especificado la asignación para el parámetro SubscriptionId).
  + **azureml-train-automl**
    + Compatibilidad de training_data, validation_data, label_column_name, weight_column_name como formato de entrada de datos
    + Incorporación del mensaje de desuso para explain_model() y retrieve_model_explanations()
  + **[azureml-pipeline-core](https://docs.microsoft.com/python/api/azureml-pipeline-core)**
    + Se ha agregado un [cuaderno](https://aka.ms/pl-modulestep) para describir [Module](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.module(class)), ModuleVersion y [ModuleStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.modulestep).
  + **[azureml-pipeline-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps)**
    + Incorporación de [RScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.rscriptstep) para admitir la ejecución del script R mediante la canalización de AML.
    + Se han corregido los parámetros de metadatos que se analizan en AzureBatchStep que provocaban el mensaje de error "assignment for parameter SubscriptionId is not specified" (no se ha especificado la asignación para el parámetro SubscriptionId).
  + **[azureml-train-automl](/python/api/azureml-train-automl-runtime/)**
    + Compatibilidad de training_data, validation_data, label_column_name, weight_column_name como formato de entrada de datos.
    + Incorporación del mensaje de desuso para [explain_model()](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlexplainer#explain-model-fitted-model--x-train--x-test--best-run-none--features-none--y-train-none----kwargs-) y [retrieve_model_explanations()](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlexplainer#retrieve-model-explanation-child-run-).


## <a name="2019-09-16"></a>2019-09-16

### <a name="azure-machine-learning-sdk-for-python-v1062"></a>SDK de Azure Machine Learning para Python v1.0.62

+ **Nuevas características:**
  + Introducción del rasgo de `timeseries` en TabularDataset. Este rasgo permite filtrar fácilmente los datos de un objeto TabularDataset por las marcas de tiempo; por ejemplo, tomar todos los datos de un intervalo de tiempo o los datos más recientes. Para información sobre este rasgo de `timeseries` de TabularDataset, visite https://aka.ms/azureml-data para encontrar documentación o https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/timeseries-datasets/tabular-timeseries-dataset-filtering.ipynb para encontrar un cuaderno de ejemplo.
  + Se ha habilitado el entrenamiento con TabularDataset y FileDataset. Visite https://aka.ms/dataset-tutorial para ver un cuaderno de ejemplo.

  + **azureml-train-core**
      + Agregación de compatibilidad con `Nccl` y `Gloo` en el estimador de PyTorch

+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Se ha dejado de usar el valor "lag_length" de AutoML y LaggingTransformer.
    + Se ha corregido la validación correcta de los datos de entrada si se especifican en formato de Dataflow.
    + Se ha modificado fit_pipeline.py para generar el JSON de grafo y cargarlo en los artefactos.
    + Representación del gráfico en `userrun` mediante `Cytoscape`.
  + **azureml-core**
    + Se ha revisado el control de excepciones en el código ADB y se han realizado cambios según el nuevo control de errores.
    + Se ha agregado la autenticación automática de MSI para máquinas virtuales de cuaderno.
    + Corrige el error en el que los modelos dañados o vacíos se podían cargar debido a reintentos con error.
    + Se ha corregido el error donde el nombre `DataReference` cambia cuando cambia el modo `DataReference` (por ejemplo, al llamar a `as_upload`, `as_download` o `as_mount`).
    + Puede permitir que `mount_point` y `target_path` sean opcionales para `FileDataset.mount` y `FileDataset.download`.
    + Se producirá la excepción de que no se encuentra la columna de marca de tiempo si se llama a la API relacionada con las series temporales sin la columna de marca de tiempo específica asignada o se quitan las columnas de marca de tiempo asignadas.
    + Las columnas de serie temporal se deben asignar con columnas cuyo tipo sea Fecha, si no, se espera una excepción.
    + Las columnas de serie temporal que asignan la API "with_timestamp_columns" pueden tomar el nombre de columna de marca de tiempo específica o general con un valor de Ninguno, que borrará las columnas de marca de tiempo asignadas previamente.
    + Se produce una excepción cuando la columna de marca de tiempo específica o general se elimina con la indicación al usuario de que la eliminación se puede realizar tras excluir la columna de marca de tiempo en la lista de eliminación o llamar a with_time_stamp con el valor Ninguno para liberar las columnas de marca de tiempo.
    + Se produce una excepción cuando la columna de marca de tiempo específica o general no se incluye en la lista de mantenimiento de columnas con la indicación al usuario de que el mantenimiento se puede realizar tras incluir la columna de marca de tiempo en la lista de mantenimiento o llamar a with_time_stamp con el valor Ninguno para liberar las columnas de marca de tiempo.
    + Se ha agregado el registro para el tamaño de un modelo registrado.
  + **azureml-explain-model**
    + Se ha corregido una advertencia impresa cuando el paquete de Python "packaging" no está instalado: "Using older than supported version of lightgbm, please upgrade to version greater than 2.2.1" (Se está usando una versión anterior a la admitida de LightGBM. Actualice a una versión superior a 2.2.1).
    + Se ha corregido la explicación del modelo de descarga con particionamiento en el caso de explicaciones globales con muchas características.
    + Se han corregido ejemplos de inicialización que faltan de Mimic Explainer en la explicación de salida.
    + Se ha corregido un error inmutable en las propiedades establecidas al cargar con el cliente de explicación mediante dos tipos diferentes de modelos.
    + Se ha agregado un parámetro get_raw a la puntuación explainer.explain(), de modo que una explicación de puntuación puede devolver valores sin formato y con ingeniería.
  + **azureml-train-automl**
    + Se han introducido API públicas de AutoML para la compatibilidad con explicaciones del SDK de explicación de `automl`; existe una nueva manera de admitir explicaciones de AutoML desacoplando las características de AutoML y el SDK de explicación; también se ha incluido la compatibilidad con explicaciones sin procesar integradas del SDK de explicación de Azure ML para los modelos de AutoML.
    + Se va a quitar azureml-defaults de los entornos de entrenamiento remoto.
    + Se ha cambiado la ubicación predeterminada del almacén de caché de una basada en FileCacheStore a una de AzureFileCacheStore para AutoML en la ruta de acceso del código de Azure Databricks.
    + Se ha corregido la validación correcta de los datos de entrada si se especifican en formato de Dataflow.
  + **azureml-train-core**
    + Se ha revertido source_directory_data_store que estaba en desuso.
    + Se ha agregado la capacidad para invalidar las versiones de paquetes instaladas de azureml.
    + Se ha agregado compatibilidad con dockerfile en el parámetro `environment_definition` en los estimadores.
    + Se han simplificado los parámetros de entrenamiento distribuido en los estimadores.

         ```py
        from azureml.train.dnn import TensorFlow, Mpi, ParameterServer
        ```

## <a name="2019-09-09"></a>2019-09-09

### <a name="new-web-experience-preview-for-azure-machine-learning-workspaces"></a>Nueva experiencia web (versión preliminar) para áreas de trabajo de Azure Machine Learning
La nueva experiencia web permite a los científicos de datos y a los ingenieros de datos completar su ciclo de vida completo de aprendizaje automático a partir de la preparación y la visualización de los datos para entrenar e implementar modelos en una sola ubicación.

![Interfaz de usuario del área de trabajo de Azure Machine Learning (versión preliminar)](./media/azure-machine-learning-release-notes/new-ui-for-workspaces.jpg)

**Características principales:**

Con esta nueva interfaz de Azure Machine Learning, ahora puede:
+ Administrar los cuadernos o vincular a Jupyter.
+ [Ejecutar experimentos de ML automatizados](tutorial-first-experiment-automated-ml.md).
+ [Crear conjuntos de datos a partir de archivos locales, almacenes de datos y archivos web](how-to-create-register-datasets.md).
+ Explorar y preparar conjuntos de datos para la creación de modelos.
+ Supervisar el desfase de datos de los modelos.
+ Ver recursos recientes desde un panel.

En el momento en que se publica este artículo, se admiten los siguientes exploradores: Chrome, Firefox, Safari y la versión preliminar de Microsoft Edge.

**Problemas conocidos:**

1. Actualice el explorador si ve el mensaje "Se ha producido un error al cargar los archivos de fragmentos" cuando la implementación está en curso.

1. No se puede eliminar ni cambiar el nombre de un archivo en Notebooks y Files. Durante la versión preliminar pública, puede usar la interfaz de usuario de Jupyter o la terminal de la máquina virtual de Notebook para realizar operaciones de actualización de archivos. Al ser un sistema de archivos de red montado, todos los cambios que realice en una máquina virtual de Notebook se reflejan de inmediato en el área de trabajo de Notebook.

1. Para SSH en la VM de Notebook:
   1. Busque las claves SSH que se crearon durante la configuración de la máquina virtual. También puede buscar las claves en el área de trabajo de Azure Machine Learning; para ello, abra la pestaña Compute, busque la máquina virtual de Notebook en la lista, abra sus propiedades y copie las claves del cuadro de diálogo.
   1. Importe esas claves SSH públicas y privadas en el equipo local.
   1. Úselas para SSH en la VM de Notebook.

## <a name="2019-09-03"></a>2019-09-03
### <a name="azure-machine-learning-sdk-for-python-v1060"></a>SDK de Azure Machine Learning para Python v1.0.60

+ **Nuevas características:**
  + Incorporó FileDataset, que hace referencia a uno o varios archivos de sus almacenes de archivos o direcciones URL públicas. Los archivos pueden tener cualquier formato. FileDataset proporciona la capacidad de descargar o montar los archivos en el proceso. Para obtener información sobre FileDataset, visite https://aka.ms/file-dataset.
  + Se agregó compatibilidad de canalización con YAML para PythonScript Step, Adla Step, Databricks Step, DataTransferStep y AzureBatch Step.

+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + AutoArima es ahora una canalización sugerida solo para la vista previa.
    + Informe de errores mejorado para pronósticos.
    + Se mejoró el registro mediante el uso de excepciones personalizadas en lugar de genéricas en las tareas de previsión.
    + Se quito la comprobación de max_concurrent_iterations para que sea menor que el número total de iteraciones.
    + Los modelos AutoML ahora devuelven las excepciones AutoMLE
    + Esta versión mejora el rendimiento de ejecución de las ejecuciones locales automatizadas de aprendizaje automático.
  + **azureml-core**
    + Se incorporó Dataset.get_all(workspace), que devuelve un diccionario de objetos `TabularDataset` y `FileDataset` con clave por su nombre de registro.

    ```py
    workspace = Workspace.from_config()
    all_datasets = Dataset.get_all(workspace)
    mydata = all_datasets['my-data']
    ```

    + Introduzca `parition_format` como argumento para `Dataset.Tabular.from_delimited_files` y `Dataset.Tabular.from_parquet.files`. La información de partición de cada ruta de acceso de datos se extraerá en columnas según el formato especificado. "{column_name}" crea una columna de cadena y "{column_name:yyyy/MM/dd/HH/mm/ss}" crea una columna DateTime, donde "yyyy", "MM", "dd", "HH", "mm" y "ss" se usan para extraer año, mes, día, hora, minuto y segundo para el tipo datetime. El partition_format debe empezar por la posición de la primera clave de partición hasta el final de la ruta de acceso del archivo. Por ejemplo, dada la ruta de acceso “ .. /USA/2019/01/01/data.csv” donde la partición es por país y hora, partition_format = “ /{Country}/{PartitionDate: YYYY/MM/DD}/Data. csv ” crea la columna de cadena “Country” con el valor “EE. UU. ” y la columna de fecha y hora “PartitionDate ” con el valor “2019-01-01 ”.
        ```py
        workspace = Workspace.from_config()
        all_datasets = Dataset.get_all(workspace)
        mydata = all_datasets['my-data']
        ```

    + Introduzca `partition_format` como argumento para `Dataset.Tabular.from_delimited_files` y `Dataset.Tabular.from_parquet.files`. La información de partición de cada ruta de acceso de datos se extraerá en columnas según el formato especificado. "{column_name}" crea una columna de cadena y "{column_name:yyyy/MM/dd/HH/mm/ss}" crea una columna DateTime, donde "yyyy", "MM", "dd", "HH", "mm" y "ss" se usan para extraer año, mes, día, hora, minuto y segundo para el tipo datetime. El partition_format debe empezar por la posición de la primera clave de partición hasta el final de la ruta de acceso del archivo. Por ejemplo, dada la ruta de acceso “ .. /USA/2019/01/01/data.csv” donde la partición es por país y hora, partition_format = “ /{Country}/{PartitionDate: YYYY/MM/DD}/Data. csv ” crea la columna de cadena “Country” con el valor “EE. UU. ” y la columna de fecha y hora “PartitionDate ” con el valor “2019-01-01 ”.
    + `to_csv_files` y los `to_parquet_files` métodos y se han agregado a `TabularDataset`. Estos métodos permiten la conversión entre un `TabularDataset` y un `FileDataset` al convertir los datos a archivos del formato especificado.
    + Inicie sesión automáticamente en el registro de imágenes base al guardar un Dockerfile generado por Model.package ().
    + "gpu_support" ya no es necesario; AML ahora detecta y usa automáticamente la extensión de Docker de Nvidia cuando está disponible. Se eliminará en una próxima versión.
    + Se agregó compatibilidad para crear, actualizar y usar PipelineDrafts.
    + Esta versión mejora el rendimiento de ejecución de las ejecuciones locales automatizadas de aprendizaje automático.
    + Los usuarios pueden consultar las métricas del historial de ejecución por nombre.
    + Se mejoró el registro mediante el uso de excepciones personalizadas en lugar de genéricas en las tareas de previsión.
  + **azureml-explain-model**
    + Se agregó el parámetro feature_maps al nuevo MimicWrapper, lo que permite a los usuarios obtener explicaciones de características sin procesar.
    + Las cargas de conjunto de los conjuntos de datos ahora están desactivadas de forma predeterminada para la carga explicativa y se pueden volver a habilitar con upload_datasets = True
    + Se agregaron parámetros de filtrado "is_law" a la lista de explicaciones y funciones de descarga.
    + Agrega el método `get_raw_explanation(feature_maps)` a los objetos de explicación global y local.
    + Comprobación de versión agregada a lightgbm con advertencia impresa si está por debajo de la versión compatible
    + Uso optimizado de memoria al explicar el procesamiento por lotes
    + Los modelos AutoML ahora devuelven las excepciones AutoMLE
  + **azureml-pipeline-core**
    + Se agregó compatibilidad para crear, actualizar y usar PipelineDrafts; se puede usar para mantener las definiciones de canalización mutable y usarlas de forma interactiva para ejecutar.
  + **azureml-train-automl**
    + Se creó la característica para instalar versiones específicas de pytorch v1.1.0 compatible con gpu, :::no-loc text="cuda"::: toolkit 9.0, pytorch-transformers, que se requiere para habilitar BERT o XLNet en el entorno remoto de tiempo de ejecución de Python.
  + **azureml-train-core**
    + Error temprano de algunos errores de definición de espacio de hiperparámetros directamente en el SDK en lugar del lado del servidor.

### <a name="azure-machine-learning-data-prep-sdk-v1114"></a>SDK de preparación de datos de Azure Machine Learning v1.1.14
+ **Mejoras y correcciones de errores**
  + Se ha habilitado la escritura en ADLS/ADLSGen2 mediante la ruta de acceso sin formato y las credenciales.
  + Se ha corregido un error que provocaba que `include_path=True` no funcionara para `read_parquet`.
  + Se ha corregido el error `to_pandas_dataframe()` causado por la excepción "Valor de propiedad no válido: hostSecret".
  + Se ha corregido un error por el que no se podían leer los archivos en DBFS en modo de Spark.

## <a name="2019-08-19"></a>2019-08-19

### <a name="azure-machine-learning-sdk-for-python-v1057"></a>SDK de Azure Machine Learning para Python v1.0.57
+ **Nuevas características:**
  + Se ha habilitado `TabularDataset` para su uso en AutomatedML. Para obtener más información sobre `TabularDataset`, visite https://aka.ms/azureml/howto/createdatasets.

+ **Mejoras y correcciones de errores**
  + **automl-client-core-nativeclient**
    + Se ha corregido el error que se generaba al proporcionar las etiquetas de entrenamiento y/o validación (y e y_valid) en forma de dataframe pandas, pero no como matriz numpy.
    + Se ha actualizado la interfaz para crear un objeto `RawDataContext` con la finalidad de requerir solo los datos y el objeto `AutoMLBaseSettings`.
    +  Se permite a los usuarios de AutoML quitar series de entrenamiento que no son lo suficientemente largas al realizar la previsión. Se permite a los usuarios de AutoML quitar granos del conjunto de pruebas que no existen en el conjunto de entrenamiento al realizar la previsión.
  + **azure-cli-ml**
    + Ahora puede actualizar el certificado TLS/SSL del punto de conexión de puntuación implementado en el clúster de AKS para los certificados de cliente y generados por Microsoft.
  + **azureml-automl-core**
    + Se ha corregido un problema en AutoML en que las filas donde faltaban etiquetas no se quitaban correctamente.
    + Se ha mejorado el registro de errores en AutoML; ahora los mensajes de error completos siempre se escribirán en el archivo de registro.
    + AutoML ha actualizado el anclaje de paquetes para incluir `azureml-defaults`, `azureml-explain-model` y `azureml-dataprep`. AutoML dejará de advertir sobre las discrepancias de paquetes (excepto del paquete `azureml-train-automl`).
    + Se ha corregido un problema de `timeseries` en que las divisiones de validación cruzada tenían un tamaño distinto, lo que provocaba un error en el cálculo de rangos.
    + Al ejecutar una iteración de conjunto para el tipo de entrenamiento de validación cruzada, si acabábamos teniendo problemas al descargar los modelos entrenados en todo el conjunto de datos, había una incoherencia entre los pesos del modelo y los modelos que se estaban introduciendo en el conjunto de votación.
    + Se ha corregido el error que se generaba al proporcionar las etiquetas de entrenamiento y/o validación (y e y_valid) en forma de dataframe pandas, pero no como matriz numpy.
    + Se ha corregido un problema con las tareas de previsión cuando se encontraba Ninguno en las columnas booleanas de las tablas de entrada.
    + Se permite a los usuarios de AutoML quitar series de entrenamiento que no son lo suficientemente largas al realizar la previsión. Se permite a los usuarios de AutoML quitar granos del conjunto de pruebas que no existen en el conjunto de entrenamiento al realizar la previsión.
  + **azureml-core**
    + Se ha corregido un problema con el orden de los parámetros blob_cache_timeout.
    + Se han agregado los tipos de excepción fit y transform externos a los errores del sistema.
    + Se ha agregado compatibilidad con los secretos de Key Vault para las ejecuciones remotas. Agregue una clase azureml.core.keyvault.Keyvault para agregar, obtener y enumerar los secretos del objeto keyvault asociado con el área de trabajo. Las operaciones compatibles son:
      + azureml.core.workspace.Workspace.get_default_keyvault()
      + azureml.core.keyvault.Keyvault.set_secret(name, value)
      + azureml.core.keyvault.Keyvault.set_secrets(secrets_dict)
      + azureml.core.keyvault.Keyvault.get_secret(name)
      + azureml.core.keyvault.Keyvault.get_secrets(secrets_list)
      + azureml.core.keyvault.Keyvault.list_secrets()
    + Métodos adicionales para obtener el objeto keyvault predeterminado y los secretos durante la ejecución remota:
      + azureml.core.workspace.Workspace.get_default_keyvault()
      + azureml.core.run.Run.get_secret(name)
      + azureml.core.run.Run.get_secrets(secrets_list)
    + Se han agregado parámetros de reemplazo adicionales al comando submit-hyperdrive de la CLI.
    + Se ha mejorado la confiabilidad de las llamadas a la API mediante la ampliación de los reintentos a las excepciones comunes de la biblioteca de solicitudes.
    + Se ha agregado compatibilidad para el envío de ejecuciones desde una ejecución enviada.
    + Se ha corregido un problema en que el token de SAS expiraba en FileWatcher, lo que provocaba que los archivos dejaran de cargarse después de expirar su token inicial.
    + Ahora se admite la importación de archivos CSV/TSV de HTTP en el SDK de Python del conjunto de datos.
    + Se ha dejado de usar el método Workspace.setup(). El mensaje de advertencia que se muestra a los usuarios sugiere usar create() o get()/from_config() en su lugar.
    + Se ha agregado Environment.add_private_pip_wheel(), que permite cargar paquetes `whl` de Python personalizados privados en el área de trabajo y usarlos con seguridad para compilar o materializar el entorno.
    + Ahora puede actualizar el certificado TLS/SSL del punto de conexión de puntuación implementado en el clúster de AKS para los certificados de cliente y generados por Microsoft.
  + **azureml-explain-model**
    + Se ha agregado un parámetro para agregar un identificador de modelo a las explicaciones en la carga.
    + Se ha agregado el etiquetado `is_raw` a las explicaciones en la memoria y la carga.
    + Se han agregado compatibilidad con PyTorch y pruebas para el paquete azureml-explain-model.
  + **azureml-opendatasets**
    + Compatibilidad con la detección y el registro del entorno de pruebas automáticas.
    + Se han agregado clases para obtener la población de EE. UU. por condado y código postal.
  + **azureml-pipeline-core**
    + Se ha agregado la propiedad label a las definiciones de puertos de entrada y salida.
  + **azureml-telemetry**
    + Se ha corregido una configuración de telemetría incorrecta.
  + **azureml-train-automl**
    + Se ha corregido un error de instalación. El error no se registraba en el campo "errores" de la ejecución del programa de instalación y, por tanto, no se almacenaba en los "errores" de la ejecución principal.
    + Se ha corregido un problema en AutoML en que las filas donde faltaban etiquetas no se quitaban correctamente.
    + Se permite a los usuarios de AutoML quitar series de entrenamiento que no son lo suficientemente largas al realizar la previsión.
    + Se permite a los usuarios de AutoML quitar granos del conjunto de pruebas que no existen en el conjunto de entrenamiento al realizar la previsión.
    + Ahora, AutoMLStep pasa a través de la configuración de `automl` para evitar problemas en los cambios o las adiciones de nuevos parámetros de configuración.
    + AutoML Data Guardrail está ahora en versión preliminar pública. El usuario verá un informe de Data Guardrail (para tareas de clasificación o regresión) después del entrenamiento y también podrá acceder a él a través del API de SDK.
  + **azureml-train-core**
    + Se ha agregado compatibilidad con Torch 1.2 en el estimador de PyTorch.
  + **azureml-widgets**
    + Los gráficos de matriz de confusión se han mejorado para el entrenamiento de la clasificación.

### <a name="azure-machine-learning-data-prep-sdk-v1112"></a>SDK de preparación de datos de Azure Machine Learning v1.1.12
+ **Nuevas características:**
  + Ahora, las listas de cadenas se pueden pasar como entrada a los métodos `read_*`.

+ **Mejoras y correcciones de errores**
  + El rendimiento de `read_parquet` ha mejorado considerablemente cuando se ejecuta en Spark.
  + Se ha corregido un problema en que se producía un error `column_type_builder` en el caso de una sola columna con formatos de fecha ambiguos.

### <a name="azure-portal"></a>Azure portal
+ **Característica en vista previa**
  + El streaming de archivos de registro y de salida ahora está disponible para las páginas de detalles de la ejecución. Los archivos transmitirán las actualizaciones en tiempo real cuando se active la alternancia de la vista previa.
  + Se ofrece en versión preliminar la capacidad de establecer la cuota en el nivel de área de trabajo. Las cuotas de AmlCompute se asignan en el nivel de suscripción, pero ahora puede distribuir esa cuota entre las áreas de trabajo y asignarla con el fin de conseguir un uso compartido y una gobernanza equitativos. Solo tiene que hacer clic en la hoja **Usages+Quotas** (Usos y cuotas) de la barra de navegación izquierda del área de trabajo y seleccionar la pestaña **Configure Quotas** (Configurar cuotas). Tenga en cuenta que debe ser administrador de la suscripción para poder establecer cuotas en el nivel de área de trabajo, ya que se trata de una operación entre áreas de trabajo.

## <a name="2019-08-05"></a>2019-08-05

### <a name="azure-machine-learning-sdk-for-python-v1055"></a>SDK de Azure Machine Learning para Python v1.0.55

+ **Nuevas características:**
  + Ahora se admite la autenticación basada en token para las llamadas realizadas al punto de conexión de puntuación implementado en AKS. Se seguirá admitiendo la actual autenticación basada en clave y los usuarios podrán cualquiera de los dos mecanismos.
  + Capacidad para registrar cualquier almacenamiento de blobs que esté detrás de la red virtual (VNet) como un almacén de datos.

+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Corrige un error por el que el tamaño de validación de las divisiones de CV es pequeño y, como resultado, los gráficos de regresión y predicción con datos reales y previstos son incorrectos.
    + Se ha mejorado el registro de las tareas de previsión de las ejecuciones remotas, ahora el usuario recibe un completo mensaje de error si la operación no se ha podido realizar.
    + Se han corregido errores de `Timeseries` si la marca de preprocesamiento es True.
    + Se ha hecho que se puedan realizar más acciones tras algunos mensajes de error de validación de datos de previsión.
    + Reducción del consumo de memoria de las ejecuciones de AutoML mediante la eliminación o la carga diferida de conjuntos de valores, especialmente entre las generaciones de procesos
  + **azureml-contrib-explain-model**
    + Se ha agregado la marca model_task a las explicaciones para que el usuario pueda anular la lógica de inferencia automática predeterminada para el tipo de modelo
    + Cambios en los widgets: se instala automáticamente con `contrib`, ya no es necesario instalar ni habilitar `nbextension`. Permite explicaciones tan solo con la importancia global de la característica (por ejemplo, permutativa)
    + Cambios en el panel: trazados de cuadros y trazados de violín, además de trazado de `beeswarm` en la página de resumen. Representación mucho más rápida del trazado de `beeswarm` en el cambio del control deslizante "Top-k". Mensaje útil que explica cómo se calcula top-k. Mensajes personalizables útiles, en lugar de gráficos, cuando no se proporcionan datos.
  + **azureml-core**
    + Se ha agregado el método Model.package() para crear imágenes de Docker y Dockerfiles que encapsulan modelos y sus dependencias.
    + Se han actualizado los servicios web locales para aceptar instancias de InferenceConfig que contengan objetos de entorno.
    + Se ha solucionado el problema de que Model.register() generaba modelos no válidos cuando "." (en el directorio actual) se pasa como parámetro model_path.
    + Se ha agregado Run. submit_child, la funcionalidad refleja Experiment.submit al especificar la ejecución como elemento primario de la ejecución secundaria enviada.
    + Compatibilidad con las opciones de configuración de Model.register en Run.register_model.
    + Capacidad para ejecutar trabajos JAR en el clúster existente.
    + Ahora se admiten los parámetros instance_pool_id y cluster_log_dbfs_path.
    + Se ha agregado compatibilidad con el uso de objetos de entorno al implementar modelos en servicios web. Ahora se puede proporcionar el objeto de entorno como parte del objeto InferenceConfig.
    + Se ha agregado la asignación de appinsifht a las nuevas regiones - centralus - westus - northcentralus
    + Se ha agregado documentación de todos los atributos de todas las clases del almacén de datos.
    + Se ha agregado el parámetro blob_cache_timeout a `Datastore.register_azure_blob_container`.
    + Se han agregado los métodos save_to_directory y load_from_directory a azureml.core.environment.Environment.
    + Se han agregado los comandos "az ml environment download" y "az ml environment register" a la CLI.
    + Se ha agregado el método Environment.add_private_pip_wheel.
  + **azureml-explain-model**
    + Se ha agregado el seguimiento de conjuntos de cambios a las explicaciones mediante el servicio Dataset (versión preliminar).
    + Se ha reducido el tamaño, de lote predeterminado, de 10 000 a 100, al transmitir en secuencias explicaciones globales.
    + Se ha agregado la marca model_task a las explicaciones para que el usuario pueda anular la lógica de inferencia automática predeterminada para el tipo de modelo.
  + **azureml-mlflow**
    + Se ha corregido en mlflow.azureml.build_image el error de que se ignoraban los directorios anidados.
  + **azureml-pipeline-steps**
    + Se ha agregado la capacidad para ejecutar trabajos JAR en clústeres de Azure Databricks existentes.
    + Se han agregado compatibilidad con los parámetros instance_pool_id y cluster_log_dbfs_path para el paso DatabricksStep.
    + Se ha agregado compatibilidad con los parámetros de canalización en el paso de DatabricksStep.
  + **azureml-train-automl**
    + Se ha agregado `docstrings` para los archivos relacionados con Ensemble.
    + Se han actualizado los documentos a un idioma más adecuado en `max_cores_per_iteration` y `max_concurrent_iterations`
    + Se ha mejorado el registro de las tareas de previsión de las ejecuciones remotas, ahora el usuario recibe un completo mensaje de error si la operación no se ha podido realizar.
    + Se ha quitado get_data del cuaderno de la canalización `automlstep`.
    + Se inició la compatibilidad con `dataprep` en `automlstep`.

### <a name="azure-machine-learning-data-prep-sdk-v1110"></a>SDK de preparación de datos de Azure Machine Learning v1.1.10

+ **Nuevas características:**
  + Ya puede solicitar la ejecución de inspectores concretos (por ejemplo, histograma, gráfico de dispersión, etc.) en columnas específicas.
  + Se ha agregado un argumento parallelize a `append_columns`. Si es True, los datos se cargan en la memoria, pero la ejecución se ejecutará en paralelo; si es False, la ejecución se transmitirá en secuencias, pero tendrá un solo subproceso.

## <a name="2019-07-23"></a>2019-07-23

### <a name="azure-machine-learning-sdk-for-python-v1053"></a>SDK de Azure Machine Learning para Python v1.0.53

+ **Nuevas características:**
  + Automated Machine Learning ahora admite modelos de ONNX de entrenamiento en el destino de proceso remoto
  + Ahora Azure Machine Learning proporciona la posibilidad de reanudar el entrenamiento desde una ejecución, punto de comprobación o archivos de modelo anteriores.
    + Obtenga información sobre [cómo usar los estimadores para reanudar el entrenamiento desde una ejecución anterior](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/tensorflow/training/train-tensorflow-resume-training/train-tensorflow-resume-training.ipynb)

+ **Mejoras y correcciones de errores**
  + **automl-client-core-nativeclient**
    + Se corrige el error sobre la pérdida de tipos de columnas después de la transformación (error vinculado).
    + Se permite que y_query sea un tipo de objeto que contiene None(s) al principio (#459519).
  + **azure-cli-ml**
    + Los comandos de la CLI de implementación de modelos y de actualización de servicio ahora aceptan parámetros, archivos de configuración o una combinación de ambos. Los parámetros tienen prioridad sobre los atributos en los archivos.
    + La descripción del modelo ahora se puede actualizar después del registro.
  + **azureml-automl-core**
    + Actualice la dependencia de NimbusML a la versión 1.2.0 (la más reciente actualmente).
    + Se agrega compatibilidad con los estimadores y canalizaciones de NimbusML que se van a usar dentro de los estimadores de AutoML.
    + Se ha corregido el error en el procedimiento de selección de conjuntos que hacía aumentar innecesariamente el conjunto resultante aunque las puntuaciones permanecieran constantes.
    + Se ha habilitado la reutilización de algunas características en las divisiones de CV para las tareas de previsión. De este modo, se acelera la ejecución del programa de instalación aproximadamente en un factor de n_cross_validations para las características caras, como las ventanas con desplazamiento y retraso.
    + Se soluciona un problema si el tiempo está fuera del intervalo de tiempo admitido de pandas. Ahora se genera una excepción DataException si el tiempo es menor que pd.Timestamp.min o mayor que pd.Timestamp.max.
    + La previsión permite ahora frecuencias diferentes en los conjuntos de entrenamiento y de prueba si se pueden alinear. Por ejemplo, "quarterly starting in January" y "quarterly starting in October" se pueden alinear.
    + La propiedad "parameters" se ha agregado a TimeSeriesTransformer.
    + Se quitan las clases de excepción antiguas.
    + En las tareas de previsión, el parámetro `target_lags` ahora acepta un valor entero único o una lista de enteros. Si se proporcionó el entero, solo se creará un retardo. Si se proporciona una lista, se tomarán los valores únicos de los retardos. target_lags=[1, 2, 2, 4] creará retardos de uno, dos y cuatro períodos.
    + Se corrige el error sobre la pérdida de tipos de columnas después de la transformación (error vinculado).
    + En `model.forecast(X, y_query)` se permite que y_query sea un tipo de objeto que contiene None(s) al principio (#459519).
    + Se agregan los valores esperados a la salida de `automl`.
  + **azureml-contrib-datadrift**
    +  Se incluyen mejoras en el cuaderno de ejemplo, incluido el cambio a azureml-opendatasets en lugar de azureml-contrib-opendatasets y mejoras de rendimiento al enriquecer datos.
  + **azureml-contrib-explain-model**
    + Se corrige el argumento de transformaciones para el explicador LIME con respecto a la importancia de la característica sin formato en el paquete azureml-contrib-explain-model.
    + Se han agregado segmentaciones a las explicaciones de imágenes en el explicador de imágenes del paquete AzureML-contrib-explain-model.
    + Se ha agregado compatibilidad dispersa con SciPy para LimeExplainer.
    + Se ha agregado `batch_size` para imitar el explicador cuando `include_local=False` para las explicaciones globales de streaming en lotes a fin de mejorar el tiempo de ejecución de DecisionTreeExplainableModel.
  + **azureml-contrib-featureengineering**
    + Se corrige para llamar a set_featurizer_timeseries_params(): el cambio de tipo de valor dict y null check. Se agrega un cuaderno para el caracterizador `timeseries`.
    + Actualice la dependencia de NimbusML a la versión 1.2.0 (la más reciente actualmente).
  + **azureml-core**
    + Se agrega la capacidad de asociar almacenes de datos DBFS en la CLI de AzureML.
    + Se corrige el error con la carga del almacén de datos en que se creaba una carpeta vacía si `target_path` se iniciaba con `/`.
    + Se corrige el problema de `deepcopy` en ServicePrincipalAuthentication.
    + Se han agregado los comandos "az ml environment show" y "az ml environment list" a la CLI.
    + Los entornos ahora admiten la especificación de un base_dockerfile como alternativa a una base_image ya creada.
    + La configuración de RunConfiguration sin usar en auto_prepare_environment se ha marcado como en desuso.
    + La descripción del modelo ahora se puede actualizar después del registro.
    + Corrección de los errores: Ahora, la eliminación del modelo y la imagen proporciona más información sobre la recuperación de objetos ascendentes que dependen de ellos si la eliminación produce un error debido a una dependencia ascendente.
    + Se corrige un error que imprimía la duración en blanco para las implementaciones que se producen al crear un área de trabajo para algunos entornos.
    + Se han mejorado las excepciones de error de la creación de áreas de trabajo. Así, los usuarios no verán el mensaje "Unable to create workspace. Unable to find... " (No se puede crear el área de trabajo. No se puede encontrar.) y, en su lugar, verán el error de creación real.
    + Se agrega compatibilidad con la autenticación de token en los servicios web de AKS.
    + Agregue el método `get_token()` a los objetos `Webservice`.
    + Se ha agregado compatibilidad con la CLI para administrar conjuntos de datos de aprendizaje automático.
    + `Datastore.register_azure_blob_container` ahora, opcionalmente, acepta un valor `blob_cache_timeout` (en segundos) que configura los parámetros de montaje de blobfuse para habilitar la expiración de la memoria caché para este almacén de datos. El valor predeterminado es sin tiempo de espera, es decir, cuando se lea un blob, permanecerá en la memoria caché local hasta que el trabajo finalice. La mayoría de los trabajos preferirán esta configuración, pero algunos trabajos necesitan leer más datos de un conjunto de datos grande que quepa en sus nodos. Para estos trabajos, el ajuste de este parámetro les ayudará a funcionar correctamente. Tenga cuidado al optimizar este parámetro: si establece un valor demasiado bajo, puede producirse un rendimiento deficiente, ya que los datos que se usan en una época pueden expirar antes de volver a usarse. Esto significa que todas las lecturas se realizarán desde el almacenamiento de blobs (es decir, la red) en lugar de la memoria caché local, lo que afecta desfavorablemente a los tiempos de entrenamiento.
    + La descripción del modelo ahora se puede actualizar correctamente después del registro.
    + La eliminación de modelos e imágenes ahora proporciona más información sobre los objetos de nivel superior que dependen de ellos, lo que provoca un error en la eliminación.
    + Se mejora la utilización de recursos de las ejecuciones remotas que usan azureml.mlflow.
  + **azureml-explain-model**
    + Se corrige el argumento de transformaciones para el explicador LIME con respecto a la importancia de la característica sin formato en el paquete azureml-contrib-explain-model.
    + Se agrega compatibilidad dispersa con SciPy para LimeExplainer.
    + Se agrega el contenedor de explicación lineal, así como otro nivel de explicación tabular para explicar los modelos lineales.
    + Para imitar la explicación en la biblioteca de modelos de explicación, se corrige el error cuando include_local=False para la entrada de datos dispersos.
    + Se agregan los valores esperados a la salida de `automl`.
    + Se corrige la importancia de la característica de permutación cuando se proporcionaba el argumento de transformaciones para obtener la importancia de la característica sin procesar.
    + Se agrega `batch_size` para imitar el explicador cuando `include_local=False` para las explicaciones globales de streaming en los lotes a fin de mejorar el tiempo de ejecución de DecisionTreeExplainableModel.
    + Para la biblioteca de explicación de modelos, se corrigieron las explicaciones de la caja negra en los que la entrada de trama de datos de Pandas es necesaria para la predicción.
    + Se corrige el error por el que `explanation.expected_values` a veces devolvería un valor float en lugar de una lista con dicho valor incluido.
  + **azureml-mlflow**
    + Se mejora el rendimiento de mlflow.set_experiment(experiment_name).
    + Se corrige el error en el uso de InteractiveLoginAuthentication para tracking_uri de mlflow.
    + Se mejora la utilización de recursos de las ejecuciones remotas que usan azureml.mlflow.
    + Se mejora la documentación del paquete azureml-mlflow.
    + Se corrige el error que hacía que mlflow.log_artifacts("my_dir") guardara artefactos en "my_dir/artifact-paths" en lugar de "artifact-paths".
  + **azureml-opendatasets**
    + Se ancla `pyarrow` de `opendatasets` para las versiones anteriores (<0.14.0) debido al problema de memoria reciente.
    + Se mueve azureml-contrib-opendatasets a azureml-opendatasets.
    + Se permite que las clases del conjunto de datos abierto se registren en el área de trabajo de Azure Machine Learning y aprovechen sin problemas las funcionalidades del conjunto de datos de AML.
    + Se mejoró significativamente el rendimiento de enriquecimiento de NoaaIsdWeather en una versión que no es de SPARK.
  + **azureml-pipeline-steps**
    + El almacén de datos DBFS se admite ahora en las entradas y salidas de DatabricksStep.
    + Se actualiza la documentación para Azure Batch Step con respecto a las entradas y salidas.
    + En AzureBatchStep, se cambió el valor predeterminado de *delete_batch_job_after_finish* a *true*.
  + **azureml-telemetry**
    +  Se mueve azureml-contrib-opendatasets a azureml-opendatasets.
    + Se permite que las clases del conjunto de datos abierto se registren en el área de trabajo de Azure Machine Learning y aprovechen sin problemas las funcionalidades del conjunto de datos de AML.
    + Se mejoró significativamente el rendimiento de enriquecimiento de NoaaIsdWeather en una versión que no es de SPARK.
  + **azureml-train-automl**
    + Se actualiza la documentación sobre get_output para reflejar el tipo de valor devuelto real y proporcionar notas adicionales sobre cómo recuperar las propiedades de clave.
    + Actualice la dependencia de NimbusML a la versión 1.2.0 (la más reciente actualmente).
    + Se agregan los valores esperados a la salida de `automl`.
  + **azureml-train-core**
    + Ahora se aceptan cadenas como destino de proceso para la optimización automática de hiperparámetros.
    + La configuración de RunConfiguration sin usar en auto_prepare_environment se ha marcado como en desuso.

### <a name="azure-machine-learning-data-prep-sdk-v119"></a>SDK de preparación de datos de Azure Machine Learning v1.1.9

+ **Nuevas características:**
  + Se ha agregado compatibilidad para leer un archivo directamente desde una dirección URL HTTP o HTTPS.

+ **Mejoras y correcciones de errores**
  + Se mejora el mensaje de error al intentar leer un conjunto de datos Parquet de un origen remoto (que no se admite actualmente).
  + Se corrige un error al escribir en el formato de archivo Parquet en ADLS Gen 2 y al actualizar el nombre del contenedor ADLS Gen 2 en la ruta de acceso.

## <a name="2019-07-09"></a>09/07/2019

### <a name="visual-interface"></a>Interfaz visual
+ **Características en versión preliminar**
  + Se ha agregado el módulo "Ekecutar script R" en la interfaz visual.

### <a name="azure-machine-learning-sdk-for-python-v1048"></a>SDK de Azure Machine Learning para Python v1.0.48

+ **Nuevas características:**
  + **azureml-opendatasets**
    + **azureml-contrib-opendatasets** ahora está disponible como **azureml-opendatasets**. El paquete antiguo puede seguir funcionando, pero le recomendamos que use **azureml-opendatasets** de ahora en adelante para obtener mejoras y funcionalidades más avanzadas.
    + Este nuevo paquete le permite registrar conjuntos de datos abiertos como un conjunto de datos en el área de trabajo de Azure Machine Learning y aprovechar completamente las funcionalidades que ofrece dicho conjunto de datos.
    + También incluye funcionalidades existentes, por ejemplo, el consumo de conjuntos de datos abiertos como dataframes de SPARK o Pandas y combinaciones de ubicaciones para algún conjunto de datos como el tiempo.

+ **Características en versión preliminar**
    + Ahora, HyperDriveConfig puede aceptar el objeto de canalización como un parámetro a fin de admitir la optimización de hiperparámetros mediante una canalización.

+ **Mejoras y correcciones de errores**
  + **azureml-train-automl**
    + Se corrigió el error sobre la pérdida de tipos de columnas después de la transformación.
    + Se corrigió el error para permitir que y_query sea un tipo de objeto que contiene None(s) al principio.
    + Se corrigió el problema en el que el procedimiento de selección de conjuntos hacía aumentar innecesariamente el conjunto resultante aunque las puntuaciones permanecieran constantes.
    + Se corrigió el problema con la configuración whitelist_models y blacklist_models en AutoMLStep.
    + Se corrigió el problema que impedía el uso del preprocesamiento cuando AutoML se habría usado en el contexto de las canalizaciones de Azure ML.
  + **azureml-opendatasets**
    + Se movió azureml-contrib-opendatasets a azureml-opendatasets.
    + Se permitió que las clases del conjunto de datos abierto se registraran en el área de trabajo de Azure Machine Learning y aprovecharan completamente las funcionalidades del conjunto de datos de AML.
    + Se mejoró significativamente el rendimiento de enriquecimiento de NoaaIsdWeather en una versión que no es de SPARK.
  + **azureml-explain-model**
    + Se actualizó la documentación en línea sobre los objetos de interpretabilidad.
    + Se agregó `batch_size` para imitar la explicación cuando `include_local=False` para las explicaciones globales de streaming de los lotes a fin de mejorar el tiempo de ejecución de DecisionTreeExplainableModel para la biblioteca de explicación del modelo.
    + Se corrigió el problema por el que `explanation.expected_values` a veces devolvería un valor float en lugar de una lista con dicho valor incluido.
    + Se agregaron los valores esperados en la salida de `automl` para la explicación de imitación en la biblioteca de modelos de explicación.
    + Se corrigió la importancia de la característica de permutación cuando se proporcionaba el argumento de transformaciones para obtener la importancia de la característica sin procesar.
  + **azureml-core**
    + Se agregó la capacidad de asociar almacenes de datos DBFS en la CLI de AzureML.
    + Se corrigió el problema con la carga del almacén de datos en que se creaba una carpeta vacía si `target_path` se iniciaba con `/`.
    + Se habilitó la comparación de dos conjuntos de datos:
    + Ahora, la eliminación del modelo y la imagen proporciona más información sobre la recuperación de objetos ascendentes que dependen de ellos si la eliminación produce un error debido a una dependencia ascendente.
    + Se dejó de usar la configuración de RunConfiguration sin usar en auto_prepare_environment.
  + **azureml-mlflow**
    + Se mejoró la utilización de recursos de las ejecuciones remotas que usan azureml.mlflow.
    + Se mejoró la documentación del paquete azureml-mlflow.
    + Se corrigió el problema que hacía que mlflow.log_artifacts("my_dir") guardara artefactos en "my_dir/artifact-paths" en lugar de "artifact-paths".
  + **azureml-pipeline-core**
    + El parámetro hash_paths de todos los pasos de la canalización está en desuso y se quitará en el futuro. De forma predeterminada, se aplica hash al contenido de source_directory (excepto en los archivos que se enumeran en .amlignore o .gitignore).
    + Se ha continuado la mejora de los valores Module y ModuleStep para que admitan módulos específicos de tipo de proceso, para preparar la integración de RunConfiguration y otros cambios para desbloquear el uso del módulo específico del proceso en canalizaciones.
  + **azureml-pipeline-steps**
    + AzureBatchStep: Se mejoró la documentación con respecto a las entradas y salidas.
    + AzureBatchStep: Se cambió el valor predeterminado de delete_batch_job_after_finish a true.
  + **azureml-train-core**
    + Ahora se aceptan cadenas como destino de proceso para la optimización automática de hiperparámetros.
    + Se dejó de usar la configuración de RunConfiguration sin usar en auto_prepare_environment.
    + Se dejaron de usar los parámetros `conda_dependencies_file_path` y `pip_requirements_file_path` para usar `conda_dependencies_file` y `pip_requirements_file`, respectivamente.
  + **azureml-opendatasets**
    + Se mejoró significativamente el rendimiento de enriquecimiento de NoaaIsdWeather en una versión que no es de SPARK.

### <a name="azure-machine-learning-data-prep-sdk-v118"></a>SDK de preparación de datos de Azure Machine Learning v1.1.8

+ **Nuevas características:**
 + Ahora se puede procesar una iteración en los objetos de flujo de datos, lo que genera una secuencia de registros. Consulte la documentación de `Dataflow.to_record_iterator`.
  + Ahora se puede procesar una iteración en los objetos de flujo de datos, lo que genera una secuencia de registros. Consulte la documentación de `Dataflow.to_record_iterator`.

+ **Mejoras y correcciones de errores**
 + Se aumentó la solidez del SDK DataPrep.
 + Se mejoró el control de los DataFrames de Pandas con índices de columna que no son cadenas.
 + Se mejoró el rendimiento de `to_pandas_dataframe` en los conjuntos de datos.
 + Se corrigió un error en que no se podía completar la ejecución de Spark de los conjuntos de datos cuando se realizaba en un entorno de varios nodos.
  + Se aumentó la solidez del SDK DataPrep.
  + Se mejoró el control de los DataFrames de Pandas con índices de columna que no son cadenas.
  + Se mejoró el rendimiento de `to_pandas_dataframe` en los conjuntos de datos.
  + Se corrigió un error en que no se podía completar la ejecución de Spark de los conjuntos de datos cuando se realizaba en un entorno de varios nodos.

## <a name="2019-07-01"></a>2019-07-01

### <a name="azure-machine-learning-data-prep-sdk-v117"></a>SDK de preparación de datos de Azure Machine Learning v1.1.7

Hemos revertido un cambio que mejoraba el rendimiento, ya que provocaba problemas a algunos clientes que usaban Azure Databricks. Si ha tenido algún problema en Azure Databricks, puede actualizar a la versión 1.1.7, para lo que puede seguir cualquiera de estos métodos:
1. Ejecute este script para realizar la actualización: `%sh /home/ubuntu/databricks/python/bin/pip install azureml-dataprep==1.1.7`
2. Vuelva a crear el clúster, que instalará la versión más reciente del SDK de preparación de datos.

## <a name="2019-06-25"></a>2019-06-25

### <a name="azure-machine-learning-sdk-for-python-v1045"></a>SDK de Azure Machine Learning para Python v1.0.45

+ **Nuevas características:**
  + Se ha agregado un modelo suplente del árbol de decisión a la explicación de imitación en el paquete azureml-explain-model
  + Capacidad de especificar una versión de CUDA que se va instalar en imágenes de inferencia. Compatibilidad con CUDA 9.0, 9.1 y 10.0.
  + La información acerca de las imágenes base del entrenamiento de Azure Machine Learning ya está disponible en el [repositorio de GitHub de contenedores de Azure Machine Learning](https://github.com/Azure/AzureML-Containers) y [DockerHub](https://hub.docker.com/_/microsoft-azureml)
  + Se ha agregado compatibilidad de la CLI con la programación de canalizaciones. Para más información, ejecute "az ml pipeline -h"
  + Se ha agregado el parámetro del espacio de nombres de Kubernetes personalizado a la configuración de implementación del servicio Web AKS y a la CLI.
  + El parámetro hash_paths está en desuso en todos los pasos de las canalizaciones
  + Model.register ahora admite el registro de varios archivos individuales como un único modelo con el uso del parámetro `child_paths`.

+ **Características en versión preliminar**
    + Ahora, las explicaciones de la puntuación pueden guardar información de conda y pip para que la serialización y deserialización sean más confiables.
    + Corrección de errores en el selector automático de características.
    + Mlflow.azureml.build_image actualizado a la nueva API, los errores revisados se han expuesto en la nueva implementación.

+ **Mejoras y correcciones de errores**
  + Se ha quitado la dependencia de `paramiko` de azureml-core. Se han agregado advertencias de desuso para los métodos para adjuntar destinos de proceso heredados.
  + Mejora del rendimiento de run.create_children
  + En la explicación de imitación con clasificador binario, se ha corregido el orden de las probabilidades cuando se usa la probabilidad del profesor para escalar los valores de forma.
  + Se ha mejorado el control de errores y los mensajes para el aprendizaje automático automatizado.
  + Se ha corregido el problema de tiempo de expiración de iteraciones para el aprendizaje automático automatizado.
  + Ha mejorado el rendimiento de la transformación de las series temporales para el aprendizaje automático automatizado.

## <a name="2019-06-24"></a>2019-06-24

### <a name="azure-machine-learning-data-prep-sdk-v116"></a>SDK de preparación de datos de Azure Machine Learning v1.1.6

+ **Nuevas características:**
  + Se han agregado funciones de resumen tanto a los valores superiores (`SummaryFunction.TOPVALUES`) como a los inferiores (`SummaryFunction.BOTTOMVALUES`).

+ **Mejoras y correcciones de errores**
  + Se ha mejorado considerablemente el rendimiento de `read_pandas_dataframe`.
  + Se ha corregido un error que provocaría que `get_profile()` en un flujo de datos que apunte a archivos binarios no funcione.
  + Se ha expuesto `set_diagnostics_collection()` para permitir la habilitación y deshabilitación por programación de la colección de telemetría.
  + Ha cambiado el comportamiento de `get_profile()`. Los valores NaN ahora se omiten ahora en Min, Mean, Std y Sum, lo que se alinea con el comportamiento de Pandas.


## <a name="2019-06-10"></a>2019-06-10

### <a name="azure-machine-learning-sdk-for-python-v1043"></a>SDK de Azure Machine Learning para Python v1.0.43

+ **Nuevas características:**
  + Azure Machine Learning ahora proporciona soporte técnico de primera clase para el popular marco de análisis de datos y aprendizaje automático Scikit-learn. Mediante un [`SKLearn` estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py), los usuarios pueden entrenar e implementar fácilmente modelos de Scikit-learn.
    + Aprenda a [ejecutar el ajuste de hiperparámetros con Scikit-learn mediante HyperDrive](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/scikit-learn/training/train-hyperparameter-tune-deploy-with-sklearn/train-hyperparameter-tune-deploy-with-sklearn.ipynb).
  + Se ha agregado compatibilidad para crear ModuleStep en las canalizaciones junto con las clases Module y ModuleVersion para administrar unidades de proceso reutilizables.
  + Los servicios web ACI ahora admiten scoring_uri persistente a través de actualizaciones. scoring_uri cambiará de IP a nombre de dominio completo. La etiqueta de nombre DNS del nombre de dominio completo se puede configurar estableciendo dns_name_label en deploy_configuration.
  + Nuevas características del aprendizaje automático automatizado:
    + Administrador de características de STL para la previsión
    + La agrupación en clústeres de KMeans está habilitada para el barrido de características
  + Se han agilizado las aprobaciones de las cuotas de AmlCompute. Se ha automatizado el proceso para aprobar las solicitudes de cuota dentro de unos márgenes. Para más información acerca del funcionamiento de las cuotas, aprenda a[administrar las cuotas](https://docs.microsoft.com/azure/machine-learning/how-to-manage-quotas).

+ **Características en versión preliminar**
    + Integración con el seguimiento de [MLflow](https://mlflow.org) 1.0.0 a través del paquete azureml mlflow ([cuadernos de ejemplo](https://aka.ms/azureml-mlflow-examples)).
    + Envío de Jupyter Notebook como una ejecución. [Documentación de referencia de API](https://docs.microsoft.com/python/api/azureml-contrib-notebook/azureml.contrib.notebook?view=azure-ml-py)
    + Versión preliminar pública del [detector de desfase de datos](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector(class)) a través del paquete azureml-contrib-datadrift ([cuadernos de ejemplo](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/monitor-models/data-drift)). El desfase de datos es una de las principales razones por las que la precisión del modelo empeora con el tiempo. Se produce cuando los datos que se sirven a un modelo en producción no coinciden con los que se usaron para entrenar dicho modelo. El detector de desfase de datos de Azure Machine Learning ayuda a los clientes a supervisar el desfase de datos y envía una alerta cada vez que se detecta un desfase.

+ **Cambios importantes**

+ **Mejoras y correcciones de errores**
  + La carga y almacenamiento de RunConfiguration admite la especificación de una ruta de acceso completa al archivo con total compatibilidad con el comportamiento anterior.
  + Se ha agregado almacenamiento en caché en ServicePrincipalAuthentication, que estaba desactivado de forma predeterminada.
  + Habilitar el registro de varios trazados en el mismo nombre de métrica.
  + La clase del modelo ahora se puede importar correctamente de azureml.core (`from azureml.core import Model`).
  + En los pasos de la canalización, el parámetro `hash_path` está en desuso. El nuevo comportamiento es hacer hash a todo source_directory, excepto a los archivos que aparecen en .amlignore o .gitignore a.
  + En los paquetes de la canalización, varios métodos `get_all` y `get_all_*` han quedado en desuso en favor de `list` y `list_*`, respectivamente.
  + azureml.core.get_run ya no requiere que se importen clases antes de volver al tipo de ejecución original.
  + Se ha corregido el problema por el que algunas llamadas a WebService Update no desencadenaban una actualización.
  + El tiempo de espera de la puntuación en los servicios web de AKS debe oscilar entre 5 y 300 000 ms. El valor máximo permitido de scoring_timeout_ms para las solicitudes de anotación pasa de 1 a 5 minutos.
  + Los objetos de LocalWebservice ahora tienen las propiedades `scoring_uri` y `swagger_uri`.
  + La creación del directorio de salidas y la carga de directorio de salidas se ha sacado del proceso del usuario. Se ha habilitado el SDK del historial de ejecución para que se ejecute en todos los procesos de usuario. Esta acción debería resolver algunos problemas de sincronización experimentados por las ejecuciones del entrenamiento distribuido.
  + El nombre del registro de azureml escrito desde el nombre del proceso del usuario ahora incluirá el nombre del proceso (solo para el entrenamiento distribuido) y el PID.

### <a name="azure-machine-learning-data-prep-sdk-v115"></a>SDK de preparación de datos de Azure Machine Learning v1.1.5

+ **Mejoras y correcciones de errores**
  + Para los valores de fecha y hora interpretados que tengan un formato de año de 2 dígitos, el rango de años válidos se ha actualizado para que coincida con la versión de mayo de Windows. Este rango se ha cambiado de 1930-2029 a 1950-2049.
  + Si se leen en un archivo y se establecen en `handleQuotedLineBreaks=True`, `\r` se tratará como una línea nueva.
  + Se ha corregido un error que provocó algunos errores en `read_pandas_dataframe`.
  + Rendimiento mejorado de `get_profile`.
  + Han mejoraron los mensajes de error.

## <a name="2019-05-28"></a>2019-05-28

### <a name="azure-machine-learning-data-prep-sdk-v114"></a>SDK de preparación de datos de Azure Machine Learning v1.1.4

+ **Nuevas características:**
  + Ahora puede usar las siguientes funciones del lenguaje de expresiones para extraer y analizar los valores de fecha y hora en nuevas columnas.
    + `RegEx.extract_record()` extrae los elementos de fecha y hora en una nueva columna.
    + `create_datetime()` crea objetos DateTime a partir de elementos de fecha y hora independientes.
  + Al llamar a `get_profile()`, ahora puede ver que las columnas de cuantil se etiquetan como (est.), con el fin de indicar que los valores son aproximaciones.
  + Ahora puede usar los comodines ** al leer desde Azure Blob Storage.
    + p. ej. `dprep.read_csv(path='https://yourblob.blob.core.windows.net/yourcontainer/**/data/*.csv')`

+ **Correcciones de errores**
  + Se ha corregido un error relacionado con la lectura de un archivo Parquet desde un origen remoto (Blob de Azure).

## <a name="2019-05-14"></a>2019-05-14

### <a name="azure-machine-learning-sdk-for-python-v1039"></a>SDK de Azure Machine Learning para Python v1.0.39
+ **Cambios**
  + La ejecución de la opción auto_prepare_environment de configuración está en desuso y la preparación automática es el valor predeterminado.

## <a name="2019-05-08"></a>2019-05-08

### <a name="azure-machine-learning-data-prep-sdk-v113"></a>SDK de preparación de datos de Azure Machine Learning v1.1.3

+ **Nuevas características:**
  + Se agregó compatibilidad para leer desde una base de datos de PostgresSQL, ya sea mediante una llamada a read_postgresql o mediante un almacén de datos.
    + Puede ver ejemplos en las guías de procedimientos:
      + [Cuaderno de ingesta de datos](https://aka.ms/aml-data-prep-ingestion-nb)
      + [Cuaderno de almacén de datos](https://aka.ms/aml-data-prep-datastore-nb)

+ **Mejoras y correcciones de errores**
  + Se han corregido problemas de conversión de tipos de columna:
  + Ahora una columna numérica o booleana se convierte correctamente en una columna booleana.
  + Ahora no se producirá un error al intentar establecer que una columna de fecha sea del tipo de fecha.
  + De ha mejorado los tipos de JoinType y la documentación de referencia que los acompaña. Al unir dos flujos de datos, ahora puede especificar uno de estos tipos de combinación:
    + NONE, MATCH, INNER, UNMATCHLEFT, LEFTANTI, LEFTOUTER, UNMATCHRIGHT, RIGHTANTI, RIGHTOUTER, FULLANTI, FULL.
  + Mejora en la inferencia de los tipos de datos para reconocer más formatos de datos.

## <a name="2019-05-06"></a>06-05-2019

### <a name="azure-portal"></a>Azure portal

En Azure Portal, ahora puede:
+ Crear y ejecutar experimentos de Azure Machine Learning automatizados
+ Crear una máquina virtual de Jupyter Notebook para probar tanto cuadernos de Jupyter Notebook de ejemplo como el suyo propio.
+ Una nueva sección de creación (versión preliminar) en el área de trabajo de Azure Machine Learning, que incluye las máquinas virtuales de aprendizaje automático automatizado, de interfaz visual y de cuaderno hospedado
    + Creación automática de un modelo mediante aprendizaje automático automatizado
    + Uso de una interfaz visual de arrastrar y colocar para ejecutar experimentos
    + Creación de una máquina virtual de Notebook para explorar datos, crear modelos e implementar servicios.
+ Gráfico en vivo y actualización de métricas en informes de ejecución y páginas de detalles de ejecución
+ Visor de archivos actualizado para registros, salidas y instantáneas de las páginas de detalles de ejecución.
+ Mejoras y novedades en la creación de informes en la pestaña Experimentos.
+ Se ha agregado la capacidad para descargar el archivo config.json de la página de información general del área de trabajo de Azure Machine Learning.
+ Compatibilidad con la creación de espacios de trabajo de Azure Machine Learning desde el área de trabajo de Azure Databricks.

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033"></a>SDK de Azure Machine Learning para Python v1.0.33
+ **Nuevas características:**
  + El método _Workspace.create_ ahora acepta las configuraciones predeterminadas para los clústeres de CPU y GPU.
  + Si se produce un error en la creación del área de trabajo, se borran los recursos dependientes.
  + La SKU predeterminada de Azure Container Registry se ha cambiado a una modalidad básica.
  + Azure Container Registry se crea de forma diferida, cuando se necesita para la creación de una imagen o ejecución.
  + Compatibilidad con los entornos para ejecuciones de entrenamiento.

### <a name="notebook-virtual-machine"></a>Máquina virtual de Notebook 

Use una máquina virtual de Notebook como un entorno de hospedaje seguro y preparado para la empresa para cuadernos de Jupyter Notebook en el que se pueden programar experimentos de aprendizaje automático, implementar modelos como puntos de conexión web y realizar todas las demás operaciones compatibles con el SDK de Azure Machine Learning mediante Python. Proporciona varias funcionalidades:
+ [Inicie rápidamente una máquina virtual de Notebook preconfigurada](tutorial-1st-experiment-sdk-setup.md) que tenga la versión más reciente del SDK de Azure Machine Learning y los paquetes relacionados.
+ El acceso se protege mediante tecnologías probadas, como HTTPS y autorización y autenticación de Azure Active Directory.
+ Almacenamiento en la nube confiable tanto de cuadernos como de código en la cuenta de almacenamiento del blob del área de trabajo de Azure Machine Learning. Puede eliminar la máquina virtual de Notebook de forma segura sin perder su trabajo.
+ Cuadernos de ejemplo preinstalados para explorar y experimentar con las características de Azure Machine Learning.
+ Completas funcionalidades de personalización completa de máquinas virtuales de Azure, cualquier tipo de máquina virtual, todos los paquetes y todos los controladores. 

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033-released"></a>SDK de Azure Machine Learning para Python v1.0.33.

+ Los modelos acelerados mediante hardware de Azure Machine Learning de la [matriz de puertas programables](how-to-deploy-fpga-web-service.md) están disponible con carácter general.
  + Ya se puede [usar el paquete de modelos de azureml-accel](how-to-deploy-fpga-web-service.md) para:
    + Entrenar las ponderaciones de una red neuronal profunda compatible (ResNet 50, ResNet 152, DenseNet 121, VGG-16 y SSD VGG)
    + Usar el aprendizaje de transferencia con la DNN compatible
    + Registrar el modelo con el servicio Administración de modelos e incluir el modelo en un contenedor
    + Implementar el modelo en una máquina virtual de Azure con una matriz de puertas programables en un clúster de Azure Kubernetes Service (AKS)
  + Implementar el contenedor en un dispositivo servidor de [Azure Data Box Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview)
  + Puntuar los datos con el punto de conexión gRPC con este [ejemplo](https://github.com/Azure-Samples/aml-hardware-accelerated-models)

### <a name="automated-machine-learning"></a>Automated Machine Learning

+ Barrido de características para habilitar la incorporación dinámica de :::no-loc text="featurizers"::: para la optimización del rendimiento. Nuevos :::no-loc text="featurizers":::: incrustaciones de trabajos, peso de la evidencia, codificaciones de destino, codificación de destino de texto, distancia del clúster
+ CV inteligente para controlar el entrenamiento o las divisiones válidas en Machine Learning automatizado
+ Algunos cambios en la optimización de la memoria y mejora del rendimiento del runtime
+ Mejora del rendimiento en la explicación de modelos
+ Conversión del modelo de ONNX para la ejecución local
+ Se ha agregado compatibilidad con submuestreos
+ Detención inteligente cuando haya criterios de salida definidos
+ Conjuntos apilados

+ Previsión de series temporales
  + Nueva función de previsión de predicciones
  + Ahora puede usar la validación cruzada de origen variable en los datos de series temporales
  + Se ha agregado una nueva funcionalidad para configurar los retardos de las series temporales
  + Se ha agregado una nueva funcionalidad que admita las características agregadas del periodo acumulado
  + Nueva detección de festivos y administrador de características cuando el código de país se define en la configuración del experimento

+ Azure Databricks
  + Se han habilitado la previsión de series temporales y la funcionalidad de explicabilidad e interpretabilidad de modelos
  + Ya se pueden cancelar y reanudar (continuar) experimentos de Machine Learning automatizado
  + Se ha agregado compatibilidad con el procesamiento con múltiples núcleos

### <a name="mlops"></a>MLOps
+ **Implementación y depuración locales para la puntuación de contenedores**<br/> Ya se puede implementar un modelo de Machine Learning localmente y realizar iteraciones rápidas en un archivo de puntuación y en las dependencias para asegurarse de que se comportan según lo previsto.

+ **Se han introducido InferenceConfig y Model.deploy()**<br/> La implementación de modelos ahora admite la especificación de una carpeta de origen con un script de entrada, igual que en RunConfig.  Además, se ha simplificado la implementación de modelos a un solo comando.

+ **Seguimiento de referencias de Git**<br/> Los clientes llevan tiempo solicitando funcionalidades de integración de Git básicas, ya que son una ayuda para mantener un registro de auditoría de un extremo a otro. Hemos implementado el seguimiento por las entidades principales de Azure Machine Learning de los metadatos relacionados con Git (repo, commit, clean state). Esta información la recopilarán automáticamente el SDK y la CLI.

+ **Servicio de validación y generación de perfiles de modelos**<br/> Los clientes se quejan con frecuencia de la dificultad que conlleva ajustar correctamente el tamaño del proceso asociado a su servicio de inferencia. Con nuestro servicio de generación de perfiles de modelos, el cliente puede proporcionar entradas de ejemplo y generaremos perfiles en 16 configuraciones de memoria/CPU diferentes para determinar el tamaño óptimo para la implementación.

+ **Traiga su propia imagen base para la inferencia**<br/> Otra queja habitual era la dificultad para moverse de las dependencias de experimentación al uso compartido de RE de inferencia. Con la nueva funcionalidad de uso compartido de imágenes base, ahora es posible reutilizar las imágenes base de la experimentación, las dependencias y todo en general, para la inferencia. Esto debería acelerar las implementaciones y reducir la brecha entre el bucle interno y el externo.

+ **Mejora en la generación de esquemas de Swagger**<br/> El anterior método de generación de Swagger era imposible de automatizar y propenso a errores. Hay una nueva forma en línea de generar esquemas de Swagger desde cualquier función de Python mediante los decoradores. Ahora este es un código abierto y el protocolo de generación de esquema no está emparejado a la plataforma de Azure Machine Learning.

+ **La CLI de Azure ML está disponible con carácter general**<br/> Ahora se pueden implementar modelos con un solo comando de la CLI. Recibimos comentarios de clientes comunes que informan de que nadie implementa un modelo de Machine Learning desde un cuaderno de Jupyter Notebook. La [**documentación de referencia de la CLI**](https://aka.ms/azmlcli) se ha actualizado.


## <a name="2019-04-22"></a>2019-04-22

SDK de Azure Machine Learning para Python v1.0.30.

[`PipelineEndpoint`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline_endpoint.pipelineendpoint?view=azure-ml-py) se ha introducido para agregar una nueva versión de una canalización publicada sin perder el mismo punto de conexión.

## <a name="2019-04-17"></a>2019-04-17

### <a name="azure-machine-learning-data-prep-sdk-v112"></a>SDK de preparación de datos de Azure Machine Learning v1.1.2

Nota: El SDK de Python de preparación de datos no instalará los paquetes `numpy` y `pandas`. Consulte [las instrucciones de instalación actualizadas](https://github.com/Microsoft/AMLDataPrepDocs).

+ **Nuevas características:**
  + Ahora puede usar la transformación de dinamización.
    + Guía paso a paso: [Cuaderno de dinamización](https://aka.ms/aml-data-prep-pivot-nb)
  + Ahora puede usar expresiones regulares en funciones nativas.
    + Ejemplos:
      + `dflow.filter(dprep.RegEx('pattern').is_match(dflow['column_name']))`
      + `dflow.assert_value('column_name', dprep.RegEx('pattern').is_match(dprep.value))`
  + Ahora puede usar las funciones `to_upper` y `to_lower` en el lenguaje de expresiones.
  + Ahora puede ver el número de valores únicos de cada columna de un perfil de datos.
  + En algunos de los pasos de lector usados habitualmente, ahora se puede usar el argumento `infer_column_types`. Si se establece en `True`, la preparación de datos intentará detectar los tipos de columna y convertirlos automáticamente.
    + `inference_arguments` ahora está en desuso.
  + Ahora puede llamar a `Dataflow.shape`.

+ **Mejoras y correcciones de errores**
  + `keep_columns` ahora acepta un argumento opcional adicional `validate_column_exists`, que comprueba si el resultado de `keep_columns` contendrá todas las columnas.
  + Todos los pasos de lector (que se leen de un archivo) ahora aceptan un argumento opcional adicional `verify_exists`.
  + Ha mejorado el rendimiento de lectura del dataframe de pandas y de la obtención de perfiles de datos.
  + Se ha corregido un error que se producía al segmentar un solo paso de flujo de datos con un índice único.

## <a name="2019-04-15"></a>2019-04-15

### <a name="azure-portal"></a>Azure portal
  + Ahora puede volver a enviar un ciclo de script existente de un clúster de proceso remoto existente.
  + Ahora puede ejecutar una canalización publicada con parámetros nuevos en la pestaña Canalizaciones.
  + Los detalles de la ejecución ahora admiten un nuevo visor de archivos de instantánea. Puede ver una instantánea del directorio cuando envió una ejecución concreta. También puede descargar el cuaderno que se envió para iniciar la ejecución.
  + Ahora puede cancelar las ejecuciones primarias desde Azure Portal.

## <a name="2019-04-08"></a>2019-04-08

### <a name="azure-machine-learning-sdk-for-python-v1023"></a>SDK de Azure Machine Learning para Python v1.0.23

+ **Nuevas características:**
  + El SDK de Azure Machine Learning ahora admite Python 3.7.
  + Ahora, los estimadores de DNN de Azure Machine Learning proporcionan compatibilidad con múltiples versiones integrada. Por ejemplo, el estimador `TensorFlow`  ahora acepta un parámetro `framework_version` y los usuarios pueden especificar las versiones "1.10" o "1.12". Para obtener una lista de las versiones compatibles con la versión actual del SDK, llame a `get_supported_versions()` en la clase de marco deseada (por ejemplo, `TensorFlow.get_supported_versions()`).
  Para obtener una lista de las versiones compatibles con la versión más reciente del SDK, consulte la [documentación del estimador de DNN](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn?view=azure-ml-py).

### <a name="azure-machine-learning-data-prep-sdk-v111"></a>SDK de preparación de datos de Azure Machine Learning v1.1.1

+ **Nuevas características:**
  + Puede leer varios orígenes de Datastore/DataPath/DataReference mediante transformaciones de read_*.
  + Puede realizar las siguientes operaciones en columnas para crear una columna:division, floor, modulo, power, length.
  + La preparación de datos ahora forma parte del conjunto de diagnósticos de Azure Machine Learning y registrará la información de diagnóstico de forma predeterminada.
    + Para desactivar esta característica, establezca esta variable de entorno en true: DISABLE_DPREP_LOGGER

+ **Mejoras y correcciones de errores**
  + Mejora de la documentación del código mejorada para funciones y clases usadas habitualmente.
  + Se ha corregido un error en auto_read_file que impedía leer archivos de Excel.
  + Se ha agregado la opción de sobrescribir la carpeta en read_pandas_dataframe.
  + Se ha mejorado el rendimiento de la instalación de la dependencia de dotnetcore2 y se ha agregado compatibilidad con Fedora 27/28 y Ubuntu 1804.
  + Se ha mejorado el rendimiento de la lectura de blobs de Azure.
  + La detección del tipo de columna ahora admite columnas del tipo Long.
  + Se ha corregido un error por el que algunos valores se muestran como marcas de tiempo, en lugar de objetos DateTime de Python.
  + Se ha corregido un error por el que algunos tipos de recuentos se muestran algunos recuentos de tipo como dobles, en lugar de como enteros.


## <a name="2019-03-25"></a>25-03-2019

### <a name="azure-machine-learning-sdk-for-python-v1021"></a>SDK de Azure Machine Learning para Python v1.0.21

+ **Nuevas características:**
  + El método *azureml.core.Run.create_children* permite la creación de baja latencia de varias ejecuciones secundarias con una sola llamada.

### <a name="azure-machine-learning-data-prep-sdk-v110"></a>SDK de preparación de datos de Azure Machine Learning v1.1.0

+ **Cambios importantes**
  + El concepto del paquete de preparación de datos está en desuso y ya no se admite. En lugar de almacenar varios flujos de datos en un solo paquete, puede hacerlo individualmente.
    + Guía paso a paso: [Apertura y almacenamiento del cuaderno Flujos de datos](https://aka.ms/aml-data-prep-open-save-dataflows-nb)

+ **Nuevas características:**
  + Preparación de datos ahora puede reconocer columnas que se ajustes a un tipo semántico concreto y se dividen en consecuencia. Los STypes compatibles actualmente incluyen: dirección de correo electrónico, coordenadas geográficas (latitud y longitud), direcciones IPv4 e IPv6, número de teléfono de EE. UU. y código postal de EE.UU.
    + Guía paso a paso: [Cuaderno Tipos semánticos](https://aka.ms/aml-data-prep-semantic-types-nb)
  + Preparación de datos ahora admite las siguientes operaciones para generar una columna resultante a partir de dos columnas numéricas: subtract, multiply, divide y modulo.
  + Puede llamar a `verify_has_data()` en un flujo de datos para comprobar si este generaría registros si se ejecutara.

+ **Mejoras y correcciones de errores**
  + Ahora puede especificar el número de rangos que se usarán en un histograma para los perfiles de las columnas numéricas.
  + La transformación `read_pandas_dataframe` ahora requiere que DataFrame tenga nombres de columna de los tipos de byte o de cadena.
  + Se ha corregido un error en la transformación `fill_nulls`, donde los valores no se rellenaron correctamente si la columna faltara.

## <a name="2019-03-11"></a>2019-03-11

### <a name="azure-machine-learning-sdk-for-python-v1018"></a>SDK de Azure Machine Learning para Python v1.0.18

 + **Cambios**
   + El paquete azureml tensorboard reemplaza azureml-contrib-tensorboard.
   + Con esta versión, puede configurar una cuenta de usuario en el clúster de proceso administrado (amlcompute) al crearlo. Esto puede realizarse usando estas propiedades en la configuración de aprovisionamiento. Encontrará más información en la [documentación de referencia del SDK](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none--remote-login-port-public-access--notspecified--).

### <a name="azure-machine-learning-data-prep-sdk-v1017"></a>SDK de preparación de datos de Azure Machine Learning v1.0.17

+ **Nuevas características:**
  + Ahora admite la adición de dos columnas numéricas para generar una columna resultante mediante el lenguaje de expresiones.

+ **Mejoras y correcciones de errores**
  + Ha mejorado la documentación y la comprobación de parámetros de random_split.

## <a name="2019-02-27"></a>2019-02-27

### <a name="azure-machine-learning-data-prep-sdk-v1016"></a>SDK de preparación de datos de Azure Machine Learning v1.0.16

+ **Corrección de error**
  + Se ha corregido un problema de autenticación de una entidad de servicio que se produjo por un cambio de API.

## <a name="2019-02-25"></a>2019-02-25

### <a name="azure-machine-learning-sdk-for-python-v1017"></a>SDK de Azure Machine Learning para Python v1.0.17

+ **Nuevas características:**

  + Ahora, Azure Machine Learning proporciona compatibilidad de primera clase con Chainer del marco DNN popular. Mediante [`Chainer`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) los usuarios de la clase pueden entrenar e implementar fácilmente modelos de Chainer.
    + Aprenda a [ejecutar el entrenamiento distribuido con ChainerMN](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/chainer/training/distributed-chainer/distributed-chainer.ipynb)
    + Aprenda a [ejecutar el ajuste de hiperparámetros con Chainer mediante HyperDrive](https://github.com/Azure/MachineLearningNotebooks/blob/b881f78e4658b4e102a72b78dbd2129c24506980/how-to-use-azureml/ml-frameworks/chainer/deployment/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb)
  + Se ha agregado a las canalizaciones de Azure Machine Learning la posibilidad de desencadenar una ejecución de una canalización según las modificaciones del almacén de datos. El [cuaderno de programación](https://aka.ms/pl-schedule) de la canalización se actualice para presentar esta característica.

+ **Mejoras y correcciones de errores**
  + Se ha agregado compatibilidad en las canalizaciones de Azure Machine Learning con el establecimiento de la propiedad source_directory_data_store en un almacén de datos deseado (como un almacenamiento de blobs) en [RunConfigurations](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) que se suministran a [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py). De forma predeterminada los pasos utilizan el almacén de Azure File como el almacén de datos de respaldo, lo que provocar problemas de limitación cuando un gran número de pasos se ejecutan simultáneamente.

### <a name="azure-portal"></a>Azure portal

+ **Nuevas características:**
  + Nueva experiencia de arrastrar y soltar el editor de tablas en informes. Los usuarios pueden arrastrar una columna desde el cuadro al área de la tabla en la que se mostrará una vista previa de la tabla. Las columnas se pueden reorganizar.
  + Nuevo visor de archivos de registros
  + Vínculos a ejecuciones de experimentos, proceso, modelos, imágenes e implementaciones desde la pestaña de actividades

### <a name="azure-machine-learning-data-prep-sdk-v1015"></a>SDK de preparación de datos de Azure Machine Learning v1.0.15

+ **Nuevas características:**
  + La preparación de datos ahora admite la escritura de secuencias de archivos desde un flujo de datos. También proporciona la capacidad de manipular los nombres de las secuencias de archivo para crear nombres de archivo.
    + Guía paso a paso: [Cuaderno Trabajo con secuencias de archivo](https://aka.ms/aml-data-prep-file-stream-nb)

+ **Mejoras y correcciones de errores**
  + Mejora en el rendimiento mejorado de t-Digest en conjuntos de datos grandes.
  + La preparación de datos ahora admite la lectura de datos desde DataPath.
  + Ahora se activa una codificación frecuente en columnas booleanas y numéricas.
  + Correcciones de otros errores varios.

## <a name="2019-02-11"></a>2019-02-11

### <a name="azure-machine-learning-sdk-for-python-v1015"></a>SDK de Azure Machine Learning para Python v1.0.15

+ **Nuevas características:**
  + Las canalizaciones de Azure Machine Learning AzureBatchStep ([cuaderno](https://aka.ms/pl-azbatch)), HyperDriveStep (cuaderno) y la funcionalidad de programación basada en tiempo ([cuaderno](https://aka.ms/pl-schedule)).
  +  DataTranferStep se actualizó para funcionar con Azure SQL Database y Azure Database para PostgreSQL ([cuaderno](https://aka.ms/pl-data-trans)).

+ **Cambios**
  + Entra en desuso `PublishedPipeline.get_published_pipeline` en favor de `PublishedPipeline.get`.
  + Entra en desuso `Schedule.get_schedule` en favor de `Schedule.get`.

### <a name="azure-machine-learning-data-prep-sdk-v1012"></a>SDK de preparación de datos de Azure Machine Learning v1.0.12

+ **Nuevas características:**
  + La preparación de los datos admite la lectura desde una base de datos de Azure SQL con Almacén de datos.

+ **Cambios**
  + Ha mejorado el rendimiento de la memoria de determinadas operaciones en datos de gran tamaño.
  + La opción `read_pandas_dataframe()` requiere que se especifique `temp_folder`.
  + La propiedad `name` en `ColumnProfile` está en desuso, use `column_name` en su lugar.

## <a name="2019-01-28"></a>28-01-2019

### <a name="azure-machine-learning-sdk-for-python-v1010"></a>SDK de Azure Machine Learning para Python v1.0.10

+ **Cambios**:
  + SDK de Azure Machine Learning ya no tiene los paquetes azure-cli como dependencia. En concreto, las dependencias azure-cli-core y azure-cli-profile azureml-core se quitaron de azureml-core. Estos son los cambios que afectan al usuario:
      + Si va a realizar "az login" y, después, va a usar azureml-sdk, el SDK iniciará la sesión del código del dispositivo o del explorador una vez más. No usará ningún estado de las credenciales creado por "az login".
    + Para la autenticación de la CLI de Azure, como el uso de "az login", use la clase _azureml.core.authentication.AzureCliAuthentication_. Para la autenticación de la CLI de Azure, siga _pip install azure-cli_ en el entorno de Python donde ha instalado azureml-sdk.
    + Si está realizando "az login" mediante una entidad de servicio para Automation, se recomienda usar la clase _azureml.core.authentication.ServicePrincipalAuthentication_, ya que azureml-sdk no usará el estado de las credenciales creadas mediante la CLI de Azure.

+ **Correcciones de errores**: Esta versión contiene principalmente correcciones de errores menores.

### <a name="azure-machine-learning-data-prep-sdk-v108"></a>SDK de preparación de datos de Azure Machine Learning v1.0.8

+ **Correcciones de errores**
  + Ha mejorado el rendimiento de la obtención de perfiles de datos.
  + Se corrigieron los errores menores relacionados con los informes de error.

### <a name="azure-portal-new-features"></a>Azure Portal: nuevas características
+ Nuevo experiencia de arrastrar y colocar gráficos en informes. Los usuarios podrán arrastrar una columna o atributo desde el pozo al área del gráfico donde el sistema seleccionará automáticamente un tipo de gráfico adecuado para el usuario según el tipo de datos. Los usuarios pueden cambiar el tipo de gráfico por otros tipos aplicables o agregar atributos adicionales.

    Tipos de gráficos compatibles:
    - Gráfico de líneas
    - Histograma
    - Gráfico de barras apiladas
    - Diagrama de caja
    - Gráfico de dispersión
    - Gráfico de burbujas
+ Ahora el portal genera dinámicamente los informes para los experimentos. Cuando un usuario envía una ejecución para un experimento, se generará automáticamente un informe con las métricas registradas y los grafos a fin de permitir la comparación entre distintas ejecuciones.

## <a name="2019-01-14"></a>14-01-2019

### <a name="azure-machine-learning-sdk-for-python-v108"></a>SDK de Azure Machine Learning para Python v1.0.8

+ **Correcciones de errores**: Esta versión contiene principalmente correcciones de errores menores.

### <a name="azure-machine-learning-data-prep-sdk-v107"></a>SDK de preparación de datos de Azure Machine Learning v1.0.7

+ **Nuevas características:**
  + Mejoras de almacén de datos (documentadas en la [guía sobre almacenes de datos](https://aka.ms/aml-data-prep-datastore-nb))
    + Se agregó la capacidad de leer y escribir en el recurso compartido de archivos de Azure y almacenes de datos de ADLS en escalado vertical.
    + Al utilizar almacenes de datos, la preparación de datos ahora admite el uso de autenticación de la entidad de servicio en lugar de la autenticación interactiva.
    + Se agregó compatibilidad para las direcciones URL wasb y wasbs.

## <a name="2019-01-09"></a>09-01-2019

### <a name="azure-machine-learning-data-prep-sdk-v106"></a>SDK de preparación de datos de Azure Machine Learning v1.0.6

+ **Correcciones de errores**
  + Se corrigió el error en la lectura desde contenedores de Azure Blob públicamente legibles en Spark

## <a name="2018-12-20"></a>20-12-2018

### <a name="azure-machine-learning-sdk-for-python-v106"></a>SDK de Azure Machine Learning para Python v1.0.6
+ **Correcciones de errores**: Esta versión contiene principalmente correcciones de errores menores.

### <a name="azure-machine-learning-data-prep-sdk-v104"></a>SDK de preparación de datos de Azure Machine Learning v1.0.4

+ **Nuevas características:**
  + La función `to_bool` ahora permite que los valores no coincidentes se convierten en valores Error. Este es el nuevo comportamiento de error de coincidencia predeterminado para `to_bool` y `set_column_types`, mientras que el comportamiento predeterminado anterior era convertir los valores no coincidentes en False.
  + Al llamar a `to_pandas_dataframe`, hay una nueva opción para interpretar los valores que faltan o NULL en columnas numéricas como NaN.
  + Se agregó la posibilidad de comprobar el tipo de valor devuelto de algunas expresiones para garantizar la coherencia de los tipos y errores pronto.
  + Ahora puede llamar a `parse_json` para analizar los valores de una columna como objetos JSON y expandirlos en varias columnas.

+ **Correcciones de errores**
  + Se ha corregido un error que bloqueaba a `set_column_types` en Python 3.5.2.
  + Se ha corregido un error que ocasionaba un bloqueo al conectarse al almacén de datos mediante una imagen de Azure Machine Learning.

+ **Actualizaciones**
  * [Cuadernos de ejemplo](https://aka.ms/aml-data-prep-notebooks) para obtener tutoriales de introducción, casos prácticos y guías de procedimientos.

## <a name="2018-12-04-general-availability"></a>04-12-2018: Disponibilidad general

Azure Machine Learning está disponible con carácter general.

### <a name="azure-machine-learning-compute"></a>Proceso de Azure Machine Learning
Con esta versión, anunciamos una nueva experiencia de proceso administrado a través del [Proceso de Machine Learning](how-to-set-up-training-targets.md#amlcompute). Este destino de proceso reemplaza el proceso de Azure Batch AI para Azure Machine Learning.

Este destino de proceso:
+ Se usa para el entrenamiento de modelos y la inferencia y puntuación de lotes
+ Es un proceso de uno a varios nodos
+ Realiza la administración del clúster y la programación de trabajos para el usuario
+ Se escala automáticamente de forma predeterminada
+ Es compatible con recursos de CPU y GPU
+ Permite el uso de máquinas virtuales de prioridad baja por un costo reducido

El proceso de Azure Machine Learning se puede crear en Python, mediante Azure Portal o la CLI. Debe crearse en la región de su área de trabajo y no se puede asociar a ninguna otra área de trabajo. Este destino de proceso usa un contenedor de Docker para su ejecución y empaqueta sus dependencias para replicar el mismo entorno en todos sus nodos.

> [!Warning]
> Recomendamos crear una nueva área de trabajo para usar el proceso de Azure Machine Learning. Existe la remota posibilidad de que los usuarios que tratan de crear el proceso de Azure Machine Learning a partir de un área de trabajo existente puedan ver un error. El proceso existente en su área de trabajo debería continuar funcionando del modo habitual.

### <a name="azure-machine-learning-sdk-for-python-v102"></a>SDK de Azure Machine Learning para Python v1.0.2
+ **Cambios importantes**
  + Con esta versión, eliminamos el soporte para crear una máquina virtual desde Azure Machine Learning. Aún puede asociar una máquina virtual de la nube existente o un servidor local remoto.
  + También eliminamos el soporte para BatchAI, que ahora debe recibirlo a través del proceso de Azure Machine Learning.

+ **Nuevo**
  + Para las canalizaciones de Machine Learning:
    + [EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimator_step.estimatorstep?view=azure-ml-py)
    + [HyperDriveStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyper_drive_step.hyperdrivestep?view=azure-ml-py)
    + [MpiStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.mpi_step.mpistep?view=azure-ml-py)


+ **Updated**
  + Para las canalizaciones de Machine Learning:
    + [DatabricksStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricks_step.databricksstep?view=azure-ml-py) ahora acepta runconfig
    + [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) ahora se copia en y desde un origen de datos de SQL
    + Programación de la funcionalidad en el SDK para crear y actualizar programaciones a fin de ejecutar canalizaciones publicadas

<!--+ **Bugs fixed**-->

### <a name="azure-machine-learning-data-prep-sdk-v052"></a>SDK de preparación de datos de Azure Machine Learning v0.5.2
+ **Cambios importantes**
  * Se ha cambiado el nombre de `SummaryFunction.N` a `SummaryFunction.Count`.

+ **Correcciones de errores**
  * Use el token de ejecución de AML más reciente al leer y escribir en almacenes de datos en ejecuciones remotas. Anteriormente, si el token de ejecución de AML se actualizaba en Python, el tiempo de ejecución de preparación de datos no se actualizaba con el token de ejecución de AML actualizado.
  * Mensajes de error más claros adicionales
  * to_spark_dataframe() ya no se bloqueará cuando Spark use la serialización `Kryo`
  * Ahora, el inspector del recuento de valores puede mostrar más de 1000 valores únicos
  * La división aleatoria ya no producirá ningún error si el flujo de datos original no tiene nombre

+ **Más información**
  * [SDK de preparación de datos de Azure Machine Learning](https://aka.ms/data-prep-sdk)

### <a name="docs-and-notebooks"></a>Documentos y cuadernos
+ Canalizaciones de Machine Learning
  + Cuadernos nuevos y actualizados para empezar a trabajar con canalizaciones, ámbitos de lote y ejemplos de transferencia del estilo: https://aka.ms/aml-pipeline-notebooks
  + Aprenda a [crear su primera canalización](how-to-create-your-first-pipeline.md)
  + Aprenda a [ejecutar predicciones por lotes mediante canalizaciones](how-to-use-parallel-run-step.md)
+ Destino de proceso de Azure Machine Learning
  + Los [cuadernos de ejemplo](https://aka.ms/aml-notebooks) ahora están actualizados para usar este nuevo proceso administrado.
  + [Obtenga información sobre este proceso](how-to-set-up-training-targets.md#amlcompute)

### <a name="azure-portal-new-features"></a>Azure Portal: nuevas características
+ Cree y administre los tipos de [proceso de Azure Machine Learning](how-to-set-up-training-targets.md#amlcompute) en el portal.
+ Supervise el uso de cuota y [solicite una cuota](how-to-manage-quotas.md) para el proceso de Azure Machine Learning.
+ Vea el estado del clúster del Proceso de Machine Learning en tiempo real.
+ La compatibilidad con la red virtual se agregó para la creación de Azure Kubernetes Service y el proceso de Azure Machine Learning.
+ Vuelva a ejecutar sus canalizaciones publicadas con los parámetros existentes.
+ Nuevos [gráficos de aprendizaje automático automatizados](how-to-understand-automated-ml.md) para modelos de clasificación (gráfico de importancia de características, elevación, ganancias y calibración con explicabilidad de modelo) y modelos de regresión (gráfico de importancia de características y valores residuales con explicabilidad de modelo).
+ Las canalizaciones se pueden ver en Azure Portal




## <a name="2018-11-20"></a>2018-11-20

### <a name="azure-machine-learning-sdk-for-python-v0180"></a>SDK de Azure Machine Learning para Python v0.1.80

+ **Cambios importantes**
  * El espacio de nombres *azureml.train.widgets* se ha movido a *azureml.widgets*.
  * *azureml.core.compute.AmlCompute* deja de usar las siguientes clases: *azureml.core.compute.BatchAICompute* y *azureml.core.compute.DSVMCompute*. La última clase se quitará en versiones posteriores. Ahora la clase AmlCompute tiene una definición más fácil. Solo necesita los valores de vm_size y max_nodes, y escala automáticamente su clúster de 0 al valor de max_nodes cuando se envía un trabajo. Nuestros [cuadernos de ejemplo](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training) se actualizaron con esta información y deberían ofrecer ejemplos de uso. Esperamos que disfrute de esta simplificación y de una gran cantidad de características atractivas adicionales en una versión posterior.

### <a name="azure-machine-learning-data-prep-sdk-v051"></a>SDK de preparación de datos de Azure Machine Learning v0.5.1

Obtenga más información sobre el SDK de preparación de datos mediante la lectura de [documentos de referencia](https://aka.ms/data-prep-sdk).
+ **Nuevas características**
   * Se ha creado una nueva CLI de DataPrep para ejecutar paquetes de DataPrep y ver el perfil de datos de un conjunto de datos o del flujo de datos.
   * Se ha rediseñado la API de SetColumnType para mejorar la facilidad de uso.
   * El nombre del archivo smart_read_file se ha cambiado por auto_read_file.
   * Ahora incluye sesgo y curtosis en el perfil de datos.
   * Puede realizar un muestreo con el muestreo estratificado.
   * Puede leer archivos ZIP que contienen archivos CSV.
   * Puede dividir conjuntos de datos por filas con la división aleatoria (por ejemplo, en conjuntos de prueba-aprendizaje).
   * Puede obtener los tipos de datos de todas las columna de un flujo de datos o un perfil de datos mediante una llamada a `.dtypes`.
   * Puede obtener el recuento de filas de un flujo de datos o un perfil de datos mediante una llamada a `.row_count`.

+ **Correcciones de errores**
   * Se ha corregido la conversión de larga a doble.
   * Se ha corregido la aserción después de cualquier adición de columna.
   * Se ha corregido un problema con FuzzyGrouping, en que no se detectaban grupos en algunos casos.
   * Se ha corregido la función de ordenación para respetar el criterio de ordenación de varias columnas.
   * Se han corregido las expresiones and/or para que se gestionen como en `pandas`.
   * Se ha corregido la lectura de la ruta de acceso de dbfs.
   * Se ha simplificado la comprensión de los mensajes de error.
   * Ya no se produce ningún error al leer en el destino de proceso remoto mediante un token de AML.
   * Ya no se produce ningún error en Linux DSVM.
   * Ya no se bloquea cuando los valores que no son de cadena están en predicados de cadena.
   * Ahora controla correctamente los errores de aserción cuando el flujo de datos generaba un error.
   * Ahora es compatible con las ubicaciones de almacenamiento montadas en dbutils en Azure Databricks.

## <a name="2018-11-05"></a>05-11-2018

### <a name="azure-portal"></a>Azure portal
Azure Portal para Azure Machine Learning tiene las siguientes actualizaciones:
  * Una nueva pestaña **Canalizaciones** para las canalizaciones publicadas.
  * Se agregó compatibilidad para adjuntar un clúster de HDInsight existente como un destino de proceso.

### <a name="azure-machine-learning-sdk-for-python-v0174"></a>SDK de Azure Machine Learning para Python v0.1.74

+ **Cambios importantes**
  * *Workspace.compute_targets, almacenes de datos, experimentos, imágenes, modelos y *servicios web* son propiedades en lugar de métodos. Por ejemplo, sustituya *Workspace.compute_targets()* por *Workspace.compute_targets*.
  * *Run.get_context* reemplaza a *Run.get_submitted_run*. El último método se quitará en versiones posteriores.
  * La clase *PipelineData* espera ahora un objeto de almacén de datos como un parámetro en lugar de datastore_name. De forma similar, *Canalización* acepta default_datastore en lugar de default_datastore_name.

+ **Nuevas características:**
  * El [cuaderno de ejemplo](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines) de las canalizaciones de Azure Machine Learning ahora usa los pasos de MPI.
  * El widget RunDetails para instancias de Jupyter Notebook se actualiza para mostrar una visualización de la canalización.

### <a name="azure-machine-learning-data-prep-sdk-v040"></a>SDK de preparación de datos de Azure Machine Learning v0.4.0

+ **Nuevas características:**
  * Recuento de tipos agregados al perfil de datos.
  * Recuento de valores e histograma ahora disponibles.
  * Más percentiles en el perfil de datos.
  * La mediana está disponible en el resumen.
  * Ahora se admite Python 3.7.
  * Cuando se guarda un flujo de datos que contiene almacenes de datos en un paquete DataPrep, la información del almacén de datos se guardará como parte del paquete DataPrep.
  * Ahora se admite la escritura en el almacén de datos.

+ **Error corregido**
  * Los desbordamientos de enteros sin signo de 64 bits ahora se controlan correctamente en Linux.
  * Etiqueta de texto incorrecta corregida para archivos de texto sin formato en smart_read.
  * El tipo de columna de cadenas se muestra ahora en la vista de métricas.
  * El recuento de tipos ahora se corrige para mostrar los tipos de valores asignados a un tipo de campo única en lugar de valores individuales.
  * Write_to_csv ya no genera ningún error cuando la ruta de acceso se proporciona como una cadena.
  * Cuando se usa Reemplazar, dejar "Buscar" en blanco no generará ningún error.

## <a name="2018-10-12"></a>12 de octubre de 2018

### <a name="azure-machine-learning-sdk-for-python-v0168"></a>SDK de Azure Machine Learning para Python v0.1.68

+ **Nuevas características:**
  * Compatibilidad con varios inquilinos al crear una nueva área de trabajo.

+ **Errores corregidos**
  * Ya no es necesario anclar la versión de la biblioteca pynacl durante la implementación del servicio web.

### <a name="azure-machine-learning-data-prep-sdk-v030"></a>SDK de preparación de datos de Azure Machine Learning v0.3.0

+ **Nuevas características:**
  * Se ha agregado el método transform_partition_with_file(script_path), lo que permite a los usuarios usar la ruta de acceso del archivo de Python que va a ejecutar

## <a name="2018-10-01"></a>2018-10-01

### <a name="azure-machine-learning-sdk-for-python-v0165"></a>SDK de Azure Machine Learning para Python v0.1.65
La [versión 0.1.65](https://pypi.org/project/azureml-sdk/0.1.65) incluye nuevas características, más documentación, correcciones de los errores y más [cuadernos de ejemplo](https://aka.ms/aml-notebooks).

Para obtener información acerca de errores conocidos y soluciones alternativas, consulte [la lista de problemas conocidos](resource-known-issues.md).

+ **Cambios importantes**
  * Workspace.experiments, Workspace.models, Workspace.compute_targets, Workspace.images, Workspace.web_services devuelven una lista de diccionarios devuelto que se ha devuelto previamente. Consulte la documentación de la API [azureml.core.Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py).

  * Automated Machine Learning ha eliminado un error de la media cuadrática normalizada de la métrica principal.

+ **HyperDrive**
  * Varias correcciones de errores de HyperDrive para obtener mejoras bayesianas del rendimiento para la obtención de llamadas a métricas.
  * Actualización de Tensorflow 1.9 a 1.10
  * Optimización de la imagen de Docker para el arranque en frío.
  * Los trabajos ahora informan acerca del estado correcto, aunque salgan con un código de error que no sea 0.
  * Validación del atributo RunConfig en SDK.
  * El objeto de ejecución de HyperDrive admite una cancelación similar a la de una ejecución normal: no es preciso usar parámetros.
  * Mejoras en los widgets para mantener el estado de los valores de la lista desplegable de ejecuciones distribuidas y ejecuciones de HyperDrive.
  * Se ha corregido el soporte de TensorBoard y otros archivos de registro para el servidor del parámetro.
  * Compatibilidad con Intel (R) MPI en el lado del servicio.
  * Corrección de errores en el ajuste de parámetros para la corrección de ejecuciones distribuidas durante la validación de BatchAI.
  * El administrador de contextos ahora identifica la instancia principal.

+ **Experiencia de Azure Portal**
  * log_table() y log_row() se admiten en los detalles de la ejecución.
  * Creación automáticamente de gráficos para tablas y filas con 1, 2 o 3 columnas numéricas y una columna de categorías opcional.

+ **Automated Machine Learning**
  * Mejora en la control de errores y la documentación
  * Se han corregido los problemas de rendimiento de la recuperación de la propiedad de ejecución.
  * Se ha corregido el problema de la ejecución continua.
  * Se han corregido problemas de iteración de :::no-loc text="ensembling":::.
  * Se corrigió un error de entrenamiento en macOS que provocaba que el sistema dejara de responder.
  * Reducción de la curva de PR/ROC media de macros en un escenario de validación personalizada.
  * Se ha eliminado la lógica de índices adicional.
  * Se ha eliminado el filtro de la API get_output.

+ **Canalizaciones**
  * Agrega un método Pipeline.publish() para publicar una canalización directamente, sin necesidad de que antes se realice una ejecución.
  * Se ha agregado un método PipelineRun.get_pipeline_runs() para capturar las ejecuciones de la canalización que se generaron desde una canalización publicada.

+ **Project Brainwave**
  * Compatibilidad actualizada con los nuevos modelos de IA disponibles en FPGA.

### <a name="azure-machine-learning-data-prep-sdk-v020"></a>SDK de preparación de datos de Azure Machine Learning v0.2.0
La [versión 0.2.0](https://pypi.org/project/azureml-dataprep/0.2.0/) incluye las siguientes características y correcciones de errores:

+ **Nuevas características:**
  * Compatibilidad con la codificación "one-hot"
  * Compatibilidad con la transformación de cuantiles

+ **Error corregido:**
  * Funciona con cualquier versión de Tornado sin necesidad de bajar de versión de Tornado
  * Recuentos de valores para todos los valores, no solo para los tres primeros

## <a name="2018-09-public-preview-refresh"></a>2018-09 (Actualización de la versión preliminar pública)

Una versión nueva y actualizada de Azure Machine Learning: obtenga más información sobre esta versión: https://azure.microsoft.com/blog/what-s-new-in-azure-machine-learning-service/


## <a name="next-steps"></a>Pasos siguientes

Consulte la introducción de [Azure Machine Learning](overview-what-is-azure-ml.md).
