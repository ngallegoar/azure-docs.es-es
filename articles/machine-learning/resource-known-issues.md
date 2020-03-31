---
title: Problemas conocidos y soluciones
titleSuffix: Azure Machine Learning
description: Obtenga una lista de los problemas conocidos y soluciones para Azure Machine Learning.
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: d5525c02edb30eff0ee8971a382f2acb8f2e57ee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79455730"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning"></a>Problemas conocidos y soluciones de Azure Machine Learning

Este artículo le ayuda a buscar y corregir los errores que se producen al usar el servicio Azure Machine Learning.

## <a name="sdk-installation-issues"></a>Problemas de instalación de SDK

**Mensaje de error: no se puede desinstalar 'PyYAML'**

SDK de Azure Machine Learning para Python: PyYAML es un proyecto instalado de Distutils. Por lo tanto, no se puede determinar con precisión qué archivos le pertenecen en caso de una desinstalación parcial. Para continuar con la instalación del SDK sin tener en cuenta este error, use:

```Python
pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
```

**Mensaje de error: `ERROR: No matching distribution found for azureml-dataprep-native`**

La distribución Python 3.7.4 de Anaconda tiene un error que interrumpe la instalación de azureml-sdk. Este problema se trata en esta [incidencia de GitHub](https://github.com/ContinuumIO/anaconda-issues/issues/11195) y se puede solucionar creando un nuevo entorno de Conda mediante este comando:
```bash
conda create -n <env-name> python=3.7.3
```
El cual crea un entorno de Conda mediante Python 3.7.3, que no tiene la incidencia de instalación en la versión 3.7.4.

## <a name="training-and-experimentation-issues"></a>Problemas de entrenamiento y experimentación

### <a name="metric-document-is-too-large"></a>El documento de métricas es demasiado grande
Azure Machine Learning tiene límites internos en cuanto al tamaño de los objetos de métricas que se pueden registrar a la vez desde una ejecución de entrenamiento. Si aparece el error "El documento de métricas es demasiado grande" al registrar una métrica con valores de lista, intente dividir la lista en fragmentos más pequeños, por ejemplo:

```python
run.log_list("my metric name", my_metric[:N])
run.log_list("my metric name", my_metric[N:])
```

De forma interna, Azure ML concatena los bloques con el mismo nombre de métrica en una lista contigua.

### <a name="moduleerrors-no-module-named"></a>ModuleErrors (ningún módulo con nombre)
Si está ejecutando ModuleErrors mientras envía experimentos en Azure ML, significa que el script de entrenamiento espera que se instale un paquete pero no se agrega. Una vez que proporcione el nombre del paquete, Azure ML instalará el paquete en el entorno que se usa para la ejecución de su entrenamiento. 

Si usa [Estimadores](concept-azure-machine-learning-architecture.md#estimators) para enviar experimentos, puede especificar un nombre de paquete mediante el parámetro `pip_packages` o `conda_packages` en el estimador basado en el origen desde el que desea instalar el paquete. También puede especificar un archivo yml con todas sus dependencias mediante `conda_dependencies_file` o enumerar todos sus requisitos de pip en un archivo txt con el parámetro `pip_requirements_file`. Si tiene su propio objeto de entorno de Azure ML para invalidar la imagen predeterminada que usa el estimador, puede especificar ese entorno a través del parámetro `environment` del constructor del estimador.

Azure ML también proporciona estimadores específicos del marco para Tensorflow, PyTorch, Chainer y SKLearn. El uso de estos estimadores asegurará que las principales dependencias del marco se instalen en su nombre en el entorno utilizado para el entrenamiento. Tiene la opción de especificar dependencias adicionales como se describe anteriormente. 
 
Azure ML mantuvo las imágenes acopladas y su contenido se puede ver en [Contenedores AzureML](https://github.com/Azure/AzureML-Containers).
Las dependencias específicas del marco se enumeran en la documentación del marco respectivo: [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py#remarks), [PyTorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py#remarks), [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py#remarks), [SKLearn](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py#remarks).

> [!Note]
> Si cree que un paquete en particular es lo suficientemente común como para agregarlo en imágenes y entornos mantenidos por Azure ML, cree una incidencia de GitHub en [Contenedores de AzureML](https://github.com/Azure/AzureML-Containers). 
 
### <a name="nameerror-name-not-defined-attributeerror-object-has-no-attribute"></a>NameError (Nombre no definido), AttributeError (El objeto no tiene ningún atributo)
Esta excepción debería provenir de sus scripts de entrenamiento. Puede consultar los archivos de registro de Azure Portal para obtener más información sobre el nombre específico no definido o el error de atributo. Desde el SDK, puede usar `run.get_details()` para ver el mensaje de error. Esto también mostrará una lista de todos los archivos de registro generados para su ejecución. Asegúrese de revisar su script de entrenamiento y corrija el error antes de volver a enviar la ejecución. 

### <a name="horovod-has-been-shut-down"></a>Horovod se apagó
En la mayoría de los casos, si se muestra "AbortedError: Horovod has been shut down" (AbortedError: Horovod se cerró), esta excepción significa que hubo una excepción subyacente en uno de los procesos, y esto causó el apagado de Horovod. Cada clasificación en el trabajo MPI obtiene su propio archivo de registro dedicado en Azure ML. Estos registros son nombrados `70_driver_logs`. En caso de entrenamiento distribuido, los nombres de registro tienen el sufijo `_rank` para facilitar la diferenciación de los registros. Para encontrar el error exacto que provocó el apagado de Horovod, revise todos los archivos de registro y busque `Traceback` al final de los archivos driver_log. Uno de estos archivos le dará la excepción subyacente real. 

### <a name="sr-iov-availability-on-ncv3-machines-in-amlcompute-for-distributed-training"></a>Disponibilidad de SR-IOV en máquinas NCv3 en AmlCompute para el entrenamiento distribuido
Azure Compute ha implementado una [actualización SR-IOV](https://azure.microsoft.com/updates/sriov-availability-on-ncv3-virtual-machines-sku/) de máquinas NCv3, que los clientes pueden aprovechar con la oferta de proceso administrada de Azure ML (AmlCompute). Las actualizaciones habilitarán la compatibilidad de toda la pila de MPI y el uso de la red InfiniBand RDMA para mejorar el rendimiento del entrenamiento distribuido de varios nodos, especialmente para el aprendizaje profundo.

Consulte la [programación de la actualización](https://azure.microsoft.com/updates/sr-iov-availability-schedule-on-ncv3-virtual-machines-sku/) para ver cuándo se implementará la compatibilidad de su región.

### <a name="run-or-experiment-deletion"></a>Eliminación de ejecuciones o experimentos
Los experimentos se pueden archivar con el método [Experiment.archive](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#archive--) o desde la vista de la pestaña Experimento en el cliente de Azure Machine Learning Studio a través del botón "Archive experiment" (Archivar experimento). Esta acción oculta el experimento de listas de consultas y vistas, pero no lo elimina.

Actualmente no se admite la eliminación permanente de experimentos ni ejecuciones individuales. Para obtener más información sobre cómo eliminar recursos del área de trabajo, consulte [Exportación o eliminación de los datos del área de trabajo de Machine Learning Service](how-to-export-delete-data.md).

## <a name="azure-machine-learning-compute-issues"></a>Problemas de proceso de Azure Machine Learning
Problemas conocidos en relación con el uso de Azure Machine Learning (AmlCompute).

### <a name="trouble-creating-amlcompute"></a>Problemas al crear AmlCompute

Es posible que algunos usuarios que crearon su área de trabajo de Azure Machine Learning en Azure Portal antes de la versión de disponibilidad general no puedan crear la instancia de AmlCompute en esa área de trabajo. Puede generar una solicitud de soporte técnico en el servicio o crear una nueva área de trabajo mediante el portal o el SDK para desbloquearse a sí mismo inmediatamente.

### <a name="outage-sr-iov-upgrade-to-ncv3-machines-in-amlcompute"></a>Interrupción: Actualización de SR-IOV para máquinas NCv3 en AmlCompute

Azure Compute actualizará las SKU de NCv3 a partir de principios de noviembre de 2019 para admitir todas las implementaciones y versiones de MPI, así como los verbos RDMA para las máquinas virtuales equipadas con InfiniBand. Esto requerirá un breve tiempo de inactividad ([obtenga más información sobre la actualización de SR-IOV](https://azure.microsoft.com/updates/sriov-availability-on-ncv3-virtual-machines-sku)).

Como cliente de la oferta de proceso administrada de Azure Machine Learning (AmlCompute), no es necesario realizar ningún cambio en este momento. En función de la [programación de actualización](https://azure.microsoft.com/updates/sr-iov-availability-schedule-on-ncv3-virtual-machines-sku), deberá planear una interrupción breve del entrenamiento. El servicio adquirirá la responsabilidad de actualizar las imágenes de VM en los nodos del clúster y de escalar verticalmente el clúster de forma automática. Una vez completada la actualización, es posible que pueda usar todas las demás distribuciones de MPI (como OpenMPI con Pytorch) además de obtener un mayor ancho de banda de InfiniBand, menores latencias y un mejor rendimiento de la aplicación distribuida.

## <a name="azure-machine-learning-designer-issues"></a>Problemas del diseñador de Azure Machine Learning

Problemas conocidos con el diseñador.

### <a name="long-compute-preparation-time"></a>Tiempo prolongado de preparación de un proceso

Crear un nuevo proceso o evocar un proceso saliente lleva tiempo, puede ser de minutos o incluso más. El equipo está trabajando para optimizarlo.


### <a name="cannot-run-an-experiment-only-contains-a-dataset"></a>No se puede ejecutar un experimento que solo contiene un conjunto de datos. 

Puede querer ejecutar un experimento que solo contenga un conjunto de datos para visualizar el conjunto de datos. Sin embargo, en este momento no se permite ejecutar un experimento que solo contiene un conjunto de datos. Estamos corrigiendo este problema.
 
Antes de la corrección, puede conectar el conjunto de datos a cualquier módulo de transformación de datos (seleccione Select Columns in Dataset [Seleccionar columnas en el conjunto de datos], Edit Metadata [Editar metadatos], Split Data [Dividir datos], etc.) y ejecute el experimento. A continuación, puede visualizar el conjunto de datos. 

La imagen a continuación muestra cómo: ![visualize-data](./media/resource-known-issues/aml-visualize-data.png)

## <a name="image-building-failure"></a>Error de creación de imagen

Error de creación de imagen al implementar el servicio web. La solución alternativa es agregar "pynacl==1.2.1" como una dependencia pip al archivo Conda para la configuración de la imagen.

## <a name="deployment-failure"></a>Error de implementación

Si observa `['DaskOnBatch:context_managers.DaskOnBatch', 'setup.py']' died with <Signals.SIGKILL: 9>`, cambie la SKU de las máquinas virtuales usadas en la implementación por otra que tenga más memoria.

## <a name="fpgas"></a>FPGA

No podrá implementar modelos en FPGA hasta que haya solicitado y se haya aprobado para la cuota FPGA. Para solicitar acceso, rellene el formulario de solicitud de cuota: https://aka.ms/aml-real-time-ai

## <a name="automated-machine-learning"></a>Automated Machine Learning

El aprendizaje automático automatizado de TensorFlow no admite actualmente la versión 1.13 de TensorFlow. Instalar esta versión hará que las dependencias del paquete dejen de funcionar. Estamos trabajando para corregir este problema en una versión futura.

### <a name="experiment-charts"></a>Gráficos de experimento

Los gráficos de clasificación binaria (precisión-retirada, ROC, curva de ganancia, etc.) que se muestran en las iteraciones de experimentos de ML automatizados no se representan correctamente en la interfaz de usuario desde el 12/04. Los trazados de los gráficos actualmente muestran resultados inversos, donde los modelos con mejor rendimiento se muestran con resultados inferiores. Se está investigando una resolución.

## <a name="datasets-and-data-preparation"></a>Conjuntos de datos y preparación de datos

Estos son problemas conocidos de los conjuntos de datos de Azure Machine Learning.

### <a name="typeerror-filenotfound-no-such-file-or-directory"></a>TypeError: FileNotFound: No se encontró el archivo o directorio.

Este error se produce si la ruta de acceso al archivo que se proporcionó no es donde se encuentra el archivo. Debe asegurarse de que la forma en que hace referencia al archivo es coherente con la ubicación en la que montó el conjunto de archivos en el destino de proceso. Para garantizar un estado determinista, se recomienda usar la ruta de acceso abstracta al montar un conjunto de datos en un destino de proceso. Por ejemplo, en el código siguiente se monta el conjunto de datos en la raíz del sistema de archivos del destino de proceso, `/tmp`. 

```python
# Note the leading / in '/tmp/dataset'
script_params = {
    '--data-folder': dset.as_named_input('dogscats_train').as_mount('/tmp/dataset'),
} 
```

Si no incluye la barra diagonal inicial ("/"), tendrá que prefijar el directorio de trabajo (por ejemplo, `/mnt/batch/.../tmp/dataset`) en el destino de proceso para indicar dónde quiere que se monte el conjunto de datos.

### <a name="fail-to-read-parquet-file-from-http-or-adls-gen-2"></a>No se puede leer el archivo Parquet desde HTTP o ADLS Gen 2

Existe un problema conocido en la versión 1.1.25 del SDK de DataPrep de AzureML que produce un error al crear un conjunto de archivos mediante la lectura de archivos Parquet desde HTTP o ADLS Gen2. Se producirá un error con `Cannot seek once reading started.`. Para corregir esta incidencia, actualice `azureml-dataprep` a una versión superior a la 1.1.26, o bien cambie a una versión anterior a la 1.1.24.

```python
pip install --upgrade azureml-dataprep
```

### <a name="typeerror-mount-got-an-unexpected-keyword-argument-invocation_id"></a>TypeError: mount () ha recibido un argumento de palabra clave inesperado "invocation_id"

Este error se produce si tiene una versión incompatible entre `azureml-core` y `azureml-dataprep`. Si ve este error, actualice el paquete de `azureml-dataprep` a una versión más reciente (mayor o igual que la 1.1.29).

```python
pip install --upgrade azureml-dataprep
```

## <a name="databricks"></a>Databricks

Problemas de Databricks y Azure Machine Learning.

### <a name="failure-when-installing-packages"></a>Error al instalar paquetes

No es posible instalar el SDK de Azure Machine Learning en Azure Databricks cuando se instalan más paquetes. Algunos paquetes, como `psutil`, pueden provocar conflictos. Para evitar errores de instalación, inmovilice la versión de la biblioteca para instalar los paquetes. Este problema está relacionado con Databricks y no con el SDK de Azure Machine Learning. También puede experimentar este problema con otras bibliotecas. Ejemplo:

```python
psutil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
```

Como alternativa, puede usar scripts de init si sigue experimentando problemas de instalación con las bibliotecas de Python. Este enfoque no se admite oficialmente. Para más información, consulte [Cluster-scoped init scripts](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts) (Script de init del ámbito de clúster).

### <a name="cancel-an-automated-machine-learning-run"></a>Cancelar una ejecución de aprendizaje automático automatizado

Al usar las funcionalidades de aprendizaje automático automatizado en Azure Databricks, para cancelar una ejecución e iniciar una nueva ejecución de un experimento, reinicie el clúster de Azure Databricks.

### <a name="10-iterations-for-automated-machine-learning"></a>> 10 iteraciones para aprendizaje automático automatizado

En la configuración del aprendizaje automático automatizado, si tiene más de 10 iteraciones, establezca `show_output` en `False` cuando envíe la ejecución.

### <a name="widget-for-the-azure-machine-learning-sdk-and-automated-machine-learning"></a>Widget para el SDK de Azure Machine Learning y aprendizaje automático automatizado

El widget del SDK de Azure Machine Learning no se admite en un cuaderno de Databricks porque los cuadernos no pueden analizar los widgets HTML. Para ver el widget en el portal, use este código de Python en la celda del cuaderno de Azure Databricks:

```
displayHTML("<a href={} target='_blank'>Azure Portal: {}</a>".format(local_run.get_portal_url(), local_run.id))
```

### <a name="import-error-cannot-import-name-timedelta-from-pandas_libstslibs"></a>Error de importación: No se puede importar el nombre "Timedelta" de "pandas._libs.tslibs"

Si ve este error al usar el aprendizaje automático automatizado, ejecute las dos líneas siguientes en el cuaderno:
```
%sh rm -rf /databricks/python/lib/python3.7/site-packages/pandas-0.23.4.dist-info /databricks/python/lib/python3.7/site-packages/pandas
%sh /databricks/python/bin/pip install pandas==0.23.4
```

### <a name="import-error-no-module-named-pandascoreindexes"></a>Error de importación: No hay ningún módulo denominado “pandas.core.indexes”

Si ve este error al usar el aprendizaje automático automatizado:

1. Ejecute este comando para instalar dos paquetes en el clúster de Azure Databricks:

   ```bash
   scikit-learn==0.19.1
   pandas==0.22.0
   ```

1. Desasocie y, luego, vuelva a conectar el clúster al cuaderno.

Si estos pasos no resuelven el problema, pruebe a reiniciar el clúster.

### <a name="failtosendfeather"></a>FailToSendFeather

Si ve un error `FailToSendFeather` al leer datos en un clúster de Azure Databricks, consulte las soluciones siguientes:

* Actualice el paquete `azureml-sdk[automl]` a la versión más reciente.
* Agregue `azureml-dataprep` versión 1.1.8 o superior.
* Agregue `pyarrow` versión 0.11 o superior.

## <a name="azure-portal"></a>Portal de Azure

Si ve directamente el área de trabajo desde un vínculo de recurso compartido desde el SDK o el portal, no podrá ver la página de información general normal con la información de suscripción en la extensión. Tampoco será capaz de cambiar a otra área de trabajo. Si necesita ver otra área de trabajo, la solución consiste en ir directamente a [Azure Machine Learning Studio](https://ml.azure.com) y buscar el nombre su nombre.

## <a name="diagnostic-logs"></a>Registros de diagnóstico

A veces puede resultar útil proporcionar información de diagnóstico al solicitar ayuda. Para ver algunos registros, visite [Azure Machine Learning Studio](https://ml.azure.com), vaya al área de trabajo y seleccione **Área de trabajo > Experimento > Ejecutar > Registros**.  

> [!NOTE]
> Azure Machine Learning registra información de varios orígenes durante el entrenamiento, como AutoML o el contenedor de Docker que ejecuta el trabajo de entrenamiento. Muchos de estos registros no están documentados. Si encuentra problemas y se pone en contacto con el Soporte técnico de Microsoft, es posible que puedan usar estos registros durante la resolución de problemas.

## <a name="activity-logs"></a>Registros de actividad

Algunas acciones dentro del área de trabajo de Azure Machine Learning no registran información en el __registro de actividad__. Por ejemplo, el inicio de una ejecución de entrenamiento o el registro de un modelo.

Algunas de estas acciones aparecen en el área __Actividades__ del área de trabajo, pero no indican quién inició la actividad.

## <a name="resource-quotas"></a>Cuotas de recursos

Obtenga información sobre la [cuotas de recursos](how-to-manage-quotas.md) que puede encontrar al trabajar con Azure Machine Learning.

## <a name="authentication-errors"></a>Errores de autenticación

Si realiza una operación de administración en un destino de proceso desde un trabajo remoto, recibirá uno de los siguientes errores:

```json
{"code":"Unauthorized","statusCode":401,"message":"Unauthorized","details":[{"code":"InvalidOrExpiredToken","message":"The request token was either invalid or expired. Please try again with a valid token."}]}
```

```json
{"error":{"code":"AuthenticationFailed","message":"Authentication failed."}}
```

Por ejemplo, si intenta crear o asociar un destino de proceso desde una canalización de aprendizaje automático que se envía para ejecución remota, recibirá un error.

## <a name="overloaded-azurefile-storage"></a>Almacenamiento AzureFile sobrecargado

Si recibe un error `Unable to upload project files to working directory in AzureFile because the storage is overloaded`, aplique las siguientes soluciones alternativas.

Si usa un recurso compartido de archivos para otras cargas de trabajo, como la transferencia de datos, se recomienda usar blobs para que el recurso compartido de archivos se pueda usar para el envío de ejecuciones. También puede dividir la carga de trabajo entre dos áreas de trabajo diferentes.

## <a name="webservices-in-azure-kubernetes-service-failures"></a>Servicios web en errores de Azure Kubernetes Service

Muchos errores de servicio web de Azure Kubernetes Service se pueden depurar mediante la conexión al clúster con `kubectl`. Puede obtener el archivo `kubeconfig.json` para un clúster de Azure Kubernetes Service mediante la ejecución de:

```azurecli-interactive
az aks get-credentials -g <rg> -n <aks cluster name>
```

## <a name="updating-azure-machine-learning-components-in-aks-cluster"></a>Actualización de componentes de Azure Machine Learning en el clúster de AKS

Las actualizaciones a componentes de Azure Machine Learning instalados en un clúster de Azure Kubernetes Service se deben aplicar manualmente. 

Para aplicar estas actualizaciones, puede desconectar el clúster del área de trabajo de Azure Machine Learning y, luego, volver a conectarlo al área de trabajo. Si SSL está habilitado en el clúster, tendrá que proporcionar el certificado SSL y la clave privada al volver a conectar el clúster. 

```python
compute_target = ComputeTarget(workspace=ws, name=clusterWorkspaceName)
compute_target.detach()
compute_target.wait_for_completion(show_output=True)

attach_config = AksCompute.attach_configuration(resource_group=resourceGroup, cluster_name=kubernetesClusterName)

## If SSL is enabled.
attach_config.enable_ssl(
    ssl_cert_pem_file="cert.pem",
    ssl_key_pem_file="key.pem",
    ssl_cname=sslCname)

attach_config.validate_configuration()

compute_target = ComputeTarget.attach(workspace=ws, name=args.clusterWorkspaceName, attach_configuration=attach_config)
compute_target.wait_for_completion(show_output=True)
```

Si ya no tiene el certificado SSL y la clave privada, o si usa un certificado generado por Azure Machine Learning, puede recuperar los archivos antes de desasociar el clúster si se conecta al clúster mediante `kubectl` y recupera el secreto `azuremlfessl`.

```bash
kubectl get secret/azuremlfessl -o yaml
```

>[!Note]
>Kubernetes almacena los secretos en formato codificado en base-64. Deberá decodificar en base-64 los componentes `cert.pem` y `key.pem` de los secretos antes de proporcionarlos a `attach_config.enable_ssl`. 

## <a name="labeling-projects-issues"></a>Incidencias en el etiquetado de proyectos

Problemas conocidos con el etiquetado de proyectos.

### <a name="only-datasets-created-on-blob-datastores-can-be-used"></a>Solo se pueden usar los conjuntos de datos creados en almacenes de datos de blobs

Se trata de una limitación conocida de la versión actual.

### <a name="after-creation-the-project-shows-initializing-for-a-long-time"></a>Después de la creación, el proyecto muestra el mensaje "Initializing" (Inicializando) durante mucho tiempo

Actualice manualmente la página. La inicialización debería continuar aproximadamente en 20 puntos de datos por segundo. La falta de actualización automática es un problema conocido. 

### <a name="when-reviewing-images-newly-labeled-images-are-not-shown"></a>Al revisar imágenes, no se muestran las imágenes recién etiquetadas

Para cargar todas las imágenes etiquetadas, elija el botón **Primera**. El botón **Primera** le llevará al principio de la lista, pero carga todos los datos etiquetados.

### <a name="pressing-esc-key-while-labeling-for-object-detection-creates-a-zero-size-label-on-the-top-left-corner-submitting-labels-in-this-state-fails"></a>Al presionar la tecla ESC mientras se etiqueta para la detección de objetos, se crea una etiqueta de tamaño cero en la esquina superior izquierda. El envío de etiquetas en este estado produce un error.

Haga clic en la cruz junto a la etiqueta para eliminarla.

## <a name="moving-the-workspace"></a>Movimiento del área de trabajo

> [!WARNING]
> No se admite mover el área de trabajo de Azure Machine Learning a otra suscripción ni mover la suscripción propietaria a un nuevo inquilino. Si lo hace, pueden producirse errores.
