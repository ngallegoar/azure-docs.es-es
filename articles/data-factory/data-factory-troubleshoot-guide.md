---
title: Solución de problemas de Azure Data Factory | Microsoft Docs
description: Obtenga información acerca de cómo solucionar problemas de actividades de control externo en Azure Data Factory.
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 8/26/2019
ms.author: abnarain
ms.reviewer: craigg
ms.openlocfilehash: c9a1ac831c4300c0523717fddc1fa53417068b89
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416570"
---
# <a name="troubleshoot-azure-data-factory"></a>Solución de problemas de Azure Data Factory
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se exploran métodos comunes de solución de problemas para actividades de control externo en Azure Data Factory.

## <a name="connector-and-copy-activity"></a>Conector y actividad de copia

Para más información sobre los problemas con el conector (por ejemplo, errores relacionados con la actividad de copia), consulte [Solución de problemas de conectores en Azure Data Factory](connector-troubleshoot-guide.md).
  

## <a name="azure-databricks"></a>Azure Databricks

### <a name="error-code--3200"></a>Código de error:  3200

- **Mensaje**: Error 403.

- **Causa**: `The Databricks access token has expired.`

- **Recomendación:** De forma predeterminada, el token de acceso de Azure Databricks es válido durante noventa días. Cree un nuevo token y actualice el servicio vinculado.


### <a name="error-code--3201"></a>Código de error:  3201

- **Mensaje**: `Missing required field: settings.task.notebook_task.notebook_path.`

- **Causa**: `Bad authoring: Notebook path not specified correctly.`

- **Recomendación:** Especifique la ruta de acceso de Notebook en la actividad de Databricks.

<br/>  

- **Mensaje**: `Cluster... does not exist.`

- **Causa**: `Authoring error: Databricks cluster does not exist or has been deleted.`

- **Recomendación:** Compruebe que existe el clúster de Databricks.

<br/>  

- **Mensaje**: `Invalid Python file URI... Please visit Databricks user guide for supported URI schemes.`

- **Causa**: `Bad authoring.`

- **Recomendación:** Especifique las rutas de acceso absolutas para los esquemas de direccionamiento del área de trabajo, o bien `dbfs:/folder/subfolder/foo.py` para los archivos almacenados en el sistema de archivos de Databricks.

<br/>  

- **Mensaje**: `{0} LinkedService should have domain and accessToken as required properties.`

- **Causa**: `Bad authoring.`

- **Recomendación:** Compruebe la [definición del servicio vinculado](compute-linked-services.md#azure-databricks-linked-service).

<br/>  

- **Mensaje**: `{0} LinkedService should specify either existing cluster ID or new cluster information for creation.`

- **Causa**:  `Bad authoring.`

- **Recomendación:** Compruebe la [definición del servicio vinculado](compute-linked-services.md#azure-databricks-linked-service).

<br/>  

- **Mensaje**: `Node type Standard_D16S_v3 is not supported. Supported node types:   Standard_DS3_v2, Standard_DS4_v2, Standard_DS5_v2, Standard_D8s_v3,   Standard_D16s_v3, Standard_D32s_v3, Standard_D64s_v3, Standard_D3_v2,   Standard_D8_v3, Standard_D16_v3, Standard_D32_v3, Standard_D64_v3,   Standard_D12_v2, Standard_D13_v2, Standard_D14_v2, Standard_D15_v2,   Standard_DS12_v2, Standard_DS13_v2, Standard_DS14_v2, Standard_DS15_v2,   Standard_E8s_v3, Standard_E16s_v3, Standard_E32s_v3, Standard_E64s_v3,   Standard_L4s, Standard_L8s, Standard_L16s, Standard_L32s, Standard_F4s,   Standard_F8s, Standard_F16s, Standard_H16, Standard_F4s_v2, Standard_F8s_v2,   Standard_F16s_v2, Standard_F32s_v2, Standard_F64s_v2, Standard_F72s_v2,   Standard_NC12, Standard_NC24, Standard_NC6s_v3, Standard_NC12s_v3,   Standard_NC24s_v3, Standard_L8s_v2, Standard_L16s_v2, Standard_L32s_v2,   Standard_L64s_v2, Standard_L80s_v2.`

- **Causa**: `Bad authoring.`

- **Recomendación:** Consulte el mensaje de error.

<br/>

### <a name="error-code--3202"></a>Código de error:  3202

- **Mensaje**: `There were already 1000 jobs created in past 3600 seconds, exceeding rate limit:   1000 job creations per 3600 seconds.`

- **Causa**: `Too many Databricks runs in an hour.`

- **Recomendación:** Compruebe todas las canalizaciones que usen esta área de trabajo de Databricks para la velocidad de creación de trabajos.  Si las canalizaciones inician demasiadas ejecuciones de Databricks en agregado, migre algunas canalizaciones a un área de trabajo nueva.

<br/>  

- **Mensaje**: `Could not parse request object: Expected 'key' and 'value' to be set for JSON map field base_parameters, got 'key: "..."' instead.`

- **Causa**: `Authoring error: No value provided for the parameter.`

- **Recomendación:** Inspeccione el json de canalización y asegúrese de que todos los parámetros del cuaderno baseParameters tienen especificado un valor no vacío.

<br/>  

- **Mensaje**: `User: `SimpleUserContext{userId=..., name=user@company.com, orgId=...}` is not   authorized to access cluster.`

- **Causa**: El usuario que ha generado el token de acceso no tiene permiso para obtener acceso al clúster de Databricks especificado en el servicio vinculado.

- **Recomendación:** Asegúrese de que el usuario tiene los permisos necesarios en el área de trabajo.


### <a name="error-code--3203"></a>Código de error:  3203

- **Mensaje**: `The cluster is in Terminated state, not available to receive jobs. Please fix the cluster or retry later.`

- **Causa**: El clúster se ha finalizado. Para un clúster interactivo, esto podría ser una condición de carrera.

- **Recomendación:** La mejor manera de evitar este error es usar clústeres de trabajo.


### <a name="error-code--3204"></a>Código de error:  3204

- **Mensaje**: `Job execution failed.`

- **Causa**:  Los mensajes de error indican varias incidencias, como un estado de clúster inesperado o una actividad específica. Normalmente, no aparece ningún mensaje de error.

- **Recomendación:** N/D
            

## <a name="azure-data-lake-analytics"></a>Análisis con Azure Data Lake

La tabla siguiente se aplica a U-SQL.
      
### <a name="error-code--2709"></a>Código de error:  2709

- **Mensaje**: `The access token is from the wrong tenant.`

- **Causa**:  Inquilino de Azure Active Directory (Azure AD) incorrecto.

- **Recomendación:** Inquilino de Azure Active Directory (Azure AD) incorrecto.

<br/>

- **Mensaje**: `We cannot accept your job at this moment. The maximum number of queued jobs for   your account is 200. `

- **Causa**:  Este error se debe a una limitación en Data Lake Analytics.

- **Recomendación:** Reduzca el número de trabajos enviados a Data Lake Analytics cambiando los desencadenadores de Data Factory y la configuración de simultaneidad en las actividades. Otra opción es aumentar los límites de Data Lake Analytics.

<br/>  

- **Mensaje**: `This job was rejected because it requires 24 AUs. This account's administrator-defined policy prevents a job from using more than 5 AUs.`

- **Causa**:  Este error se debe a una limitación en Data Lake Analytics.

- **Recomendación:** Reduzca el número de trabajos enviados a Data Lake Analytics cambiando los desencadenadores de Data Factory y la configuración de simultaneidad en las actividades. Otra opción es aumentar los límites de Data Lake Analytics.


### <a name="error-code--2705"></a>Código de error:  2705

- **Mensaje**: `Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/>  <br/>  User is   not able to access Data Lake Store.  <br/>  <br/>  User is  not authorized to use Data Lake Analytics.`

- **Causa**:  La entidad de servicio o el certificado no tiene acceso al archivo en el almacenamiento.

- **Recomendación:** Asegúrese de que la entidad de servicio o el certificado que el usuario proporciona para los trabajos de Data Lake Analytics tiene acceso a la cuenta de Data Lake Analytics y a la instancia de Data Lake Storage predeterminada de la carpeta raíz.


### <a name="error-code--2711"></a>Código de error:  2711

- **Mensaje**: `Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/>  <br/>  User is   not able to access Data Lake Store.  <br/>  <br/>  User is  not authorized to use Data Lake Analytics.`

- **Causa**:  La entidad de servicio o el certificado no tiene acceso al archivo en el almacenamiento.

- **Recomendación:** Asegúrese de que la entidad de servicio o el certificado que el usuario proporciona para los trabajos de Data Lake Analytics tiene acceso a la cuenta de Data Lake Analytics y a la instancia de Data Lake Storage predeterminada de la carpeta raíz.

<br/>  

- **Mensaje**: `Cannot find the 'Azure Data Lake Store' file or folder.`

- **Causa**:  La ruta de acceso al archivo U-SQL es incorrecta o las credenciales del servicio vinculado no tienen acceso.

- **Recomendación:** Compruebe la ruta de acceso y las credenciales proporcionadas en el servicio vinculado.


### <a name="error-code--2704"></a>Código de error:  2704

- **Mensaje**: `Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/>  <br/>  User is   not able to access Data Lake Store.  <br/>  <br/>  User is  not authorized to use Data Lake Analytics.`

- **Causa**:  La entidad de servicio o el certificado no tiene acceso al archivo en el almacenamiento.

- **Recomendación:** Asegúrese de que la entidad de servicio o el certificado que el usuario proporciona para los trabajos de Data Lake Analytics tiene acceso a la cuenta de Data Lake Analytics y a la instancia de Data Lake Storage predeterminada de la carpeta raíz.


### <a name="error-code--2707"></a>Código de error:  2707

- **Mensaje**: `Cannot resolve the account of AzureDataLakeAnalytics. Please check 'AccountName' and   'DataLakeAnalyticsUri'.`

- **Causa**:  La cuenta de Data Lake Analytics en el servicio vinculado es incorrecta.

- **Recomendación:** Compruebe que se proporciona la cuenta adecuada.


### <a name="error-code--2703"></a>Código de error:  2703

- **Mensaje**: `Error Id: E_CQO_SYSTEM_INTERNAL_ERROR (or any error that starts with "Error   Id:").`

- **Causa**:  El error es de Data Lake Analytics.

- **Recomendación:** Cualquier error similar al ejemplo significa que el trabajo se envió a Data Lake Analytics y el script produjo un error allí. Investigue el error en Data Lake Analytics. En el portal, vaya a la cuenta de Data Lake Analytics y busque el trabajo mediante el identificador de ejecución de actividad de Data Factory (no el identificador de ejecución de canalización). El trabajo proporcionará más información sobre el error y le ayudará a solucionar el problema. Si la resolución no está clara, póngase en contacto con el equipo de soporte técnico de Data Lake Analytics y proporcióneles la dirección URL del trabajo, que incluye el nombre de cuenta y el identificador del trabajo.
          

## <a name="azure-functions"></a>Azure Functions

### <a name="error-code--3602"></a>Código de error:  3602

- **Mensaje**: `Invalid HttpMethod: '%method;'.`

- **Causa**: La actividad de las funciones de Azure no admite el método HTTP especificado en la carga de la actividad.

- **Recomendación:**  Los métodos HTTP que se admiten son PUT, POST, GET, DELETE, OPTIONS, HEAD y TRACE.


### <a name="error-code--3603"></a>Código de error:  3603

- **Mensaje**: `Response Content is not a valid JObject.`

- **Causa**: la función de Azure a la que se llamó no devolvió una carga JSON en la respuesta. La actividad de la función de Azure de ADF solo admite contenido de respuesta JSON.

- **Recomendación:**  actualice la función de Azure para devolver una carga JSON válida; por ejemplo, una función C# puede devolver (ActionResult)new OkObjectResult("{\"Id\":\"123\"}");


### <a name="error-code--3606"></a>Código de error:  3606

- **Mensaje**: falta la clave de función de la actividad de Azure Function.

- **Causa**: La definición de actividad de funciones de Azure no está completa.

- **Recomendación:**  Compruebe que la definición JSON de la actividad AzureFunction de entrada tiene la propiedad denominada "functionKey".


### <a name="error-code--3607"></a>Código de error:  3607

- **Mensaje**: `Azure function activity missing function name.`

- **Causa**: La definición de actividad de funciones de Azure no está completa.

- **Recomendación:**  Compruebe que la definición JSON de la actividad AzureFunction de entrada tiene la propiedad denominada "functionName".


### <a name="error-code--3608"></a>Código de error:  3608

- **Mensaje**: `Call to provided Azure function '%FunctionName;' failed with status-'%statusCode;' and message - '%message;'.`

- **Causa**: Los detalles de las funciones de Azure en la definición de la actividad pueden ser incorrectos.

- **Recomendación:**  Corrija los detalles de las funciones de Azure y vuelva a intentarlo.


### <a name="error-code--3609"></a>Código de error:  3609

- **Mensaje**: `Azure function activity missing functionAppUrl.`

- **Causa**: La definición de actividad de funciones de Azure no está completa.

- **Recomendación:**  Compruebe que la definición JSON de la actividad AzureFunction de entrada tiene la propiedad denominada "functionAppUrl".


### <a name="error-code--3610"></a>Código de error:  3610

- **Mensaje**: `There was an error while calling endpoint.`

- **Causa**: La dirección URL de la función puede ser incorrecta.

- **Recomendación:**  Asegúrese de que el valor de "functionAppUrl" en el JSON de actividad es correcto e inténtelo de nuevo.


### <a name="error-code--3611"></a>Código de error:  3611

- **Mensaje**: `Azure function activity missing Method in JSON.`

- **Causa**: La definición de actividad de funciones de Azure no está completa.

- **Recomendación:**  compruebe que la definición JSON de la actividad AzureFunction de entrada tiene la propiedad denominada "method".


### <a name="error-code--3612"></a>Código de error:  3612

- **Mensaje**: `Azure function activity missing LinkedService definition in JSON.`

- **Causa**: La definición de actividad de funciones de Azure no está completa.

- **Recomendación:**  Compruebe que la definición JSON de la actividad AzureFunction de entrada tiene detalles del servicio vinculado.



## <a name="azure-machine-learning"></a>Azure Machine Learning

### <a name="error-code--4101"></a>Código de error:  4101

- **Mensaje**: `AzureMLExecutePipeline activity '%activityName;' has invalid value for property '%propertyName;'.`

- **Causa**: formato incorrecto o falta la definición de la propiedad "%propertyName;".

- **Recomendación:**  compruebe si la actividad "%activityName;" tiene la propiedad "%propertyName;" definida con los datos correctos.


### <a name="error-code--4110"></a>Código de error:  4110

- **Mensaje**: `AzureMLExecutePipeline activity missing LinkedService definition in JSON.`

- **Causa**: La definición de la actividad AzureMLExecutePipeline no está completa.

- **Recomendación:**  Compruebe que la definición JSON de la actividad AzureMLExecutePipeline de entrada tiene detalles del servicio vinculado.


### <a name="error-code--4111"></a>Código de error:  4111

- **Mensaje**: `AzureMLExecutePipeline activity has wrong LinkedService type in JSON. Expected LinkedService type: '%expectedLinkedServiceType;', current LinkedService type: Expected LinkedService type: '%currentLinkedServiceType;'.`

- **Causa**: Definición de actividad incorrecta.

- **Recomendación:**  Compruebe que la definición JSON de la actividad AzureMLExecutePipeline de entrada tiene detalles del servicio vinculado correctos.


### <a name="error-code--4112"></a>Código de error:  4112

- **Mensaje**: `AzureMLService linked service has invalid value for property '%propertyName;'.`

- **Causa**: formato incorrecto o falta la definición de la propiedad "%propertyName;".

- **Recomendación:**  compruebe si el servicio vinculado tiene la propiedad "%propertyName;" definida con los datos correctos.


### <a name="error-code--4121"></a>Código de error:  4121

- **Mensaje**: `Request sent to Azure Machine Learning for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure Machine Learning: '%externalMessage;'.`

- **Causa**: La credencial usada para acceder a Azure Machine Learning ha expirado.

- **Recomendación:**  Compruebe que la credencial es válida y vuelva a intentarlo.


### <a name="error-code--4122"></a>Código de error:  4122

- **Mensaje**: `Request sent to Azure Machine Learning for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure Machine Learning: '%externalMessage;'.`

- **Causa**: la credencial proporcionada en el servicio vinculado de Azure Machine Learning no es válida o no tiene permiso para la operación.

- **Recomendación:**  compruebe que la credencial del servicio vinculado es válida y tiene permiso para acceder a Azure Machine Learning.


### <a name="error-code--4123"></a>Código de error:  4123

- **Mensaje**: `Request sent to Azure Machine Learning for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure Machine Learning: '%externalMessage;'.`

- **Causa**: Las propiedades de la actividad, como pipelineParameters, no son válidas para la canalización de Azure Machine Learning.

- **Recomendación:**  Compruebe el valor de las propiedades de la actividad para que coincida con la carga esperada de la canalización de Azure ML publicada especificada en el servicio vinculado.


### <a name="error-code--4124"></a>Código de error:  4124

- **Mensaje**: `Request sent to Azure Machine Learning for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure Machine Learning: '%externalMessage;'.`

- **Causa**: El punto de conexión de canalización de Azure ML publicado no existe.

- **Recomendación:**  Compruebe que el punto de conexión de la canalización de Azure Machine Learning publicado que se especifica en el servicio vinculado existe en Azure Machine Learning.


### <a name="error-code--4125"></a>Código de error:  4125

- **Mensaje**: `Request sent to Azure Machine Learning for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure Machine Learning: '%externalMessage;'.`

- **Causa**: Error de servidor en Azure Machine Learning.

- **Recomendación:**  Inténtelo de nuevo más tarde. Póngase en contacto con el equipo de Azure Machine Learning para obtener ayuda si el problema persiste.


### <a name="error-code--4126"></a>Código de error:  4126

- **Mensaje**: `Azure ML pipeline run failed with status: '%amlPipelineRunStatus;'. Azure ML pipeline run Id: '%amlPipelineRunId;'. Please check in Azure Machine Learning for more error logs.`

- **Causa**: error en la ejecución de la canalización de Azure ML.

- **Recomendación:**  compruebe si en Azure Machine Learning hay más registros de errores y corrija la canalización de ML.



## <a name="common"></a>Comunes

### <a name="error-code--2103"></a>Código de error:  2103

- **Mensaje**: `Please provide value for the required property '%propertyName;'.`

- **Causa**: no se ha proporcionado el valor de la propiedad, pero es necesario en el escenario.

- **Recomendación:**  proporcione el valor del mensaje e inténtelo de nuevo.


### <a name="error-code--2104"></a>Código de error:  2104

- **Mensaje**: `The type of the property '%propertyName;' is incorrect.`

- **Causa**: el tipo de la propiedad proporcionada no es el esperado.

- **Recomendación:**  corrija el tipo de la propiedad e inténtelo de nuevo.


### <a name="error-code--2105"></a>Código de error:  2105

- **Mensaje**: `An invalid json is provided for property '%propertyName;'. Encountered an error while trying to parse: '%message;'.`

- **Causa**: el valor de la propiedad no es válido o no tiene el formato esperado.

- **Recomendación:**  busque la propiedad en la documentación y asegúrese de que el valor proporcionado tenga el formato y el tipo esperados.


### <a name="error-code--2106"></a>Código de error:  2106

- **Mensaje**: `The storage connection string is invalid. %errorMessage;`

- **Causa**: la cadena de conexión del almacenamiento no es válida o tiene un formato incorrecto.

- **Recomendación:**  vaya a Azure Portal, busque el almacenamiento, copie la cadena de conexión y péguela en el servicio vinculado. Luego, inténtelo de nuevo.


### <a name="error-code--2108"></a>Código de error:  2108

- **Mensaje**: `Error calling the endpoint '%url;'. Response status code: '%code;'`

- **Causa**: no se pudo realizar la solicitud debido a un problema subyacente (conectividad de red, error de DNS, validación del certificado de servidor o tiempo de espera).

- **Recomendación:**  Use Fiddler o Postman para validar la solicitud.


### <a name="error-code--2110"></a>Código de error:  2110

- **Mensaje**: `The linked service type '%linkedServiceType;' is not supported for '%executorType;' activities.`

- **Causa**: el servicio vinculado especificado en la actividad era incorrecto.

- **Recomendación:**  asegúrese de que el tipo de servicio vinculado sea uno de los tipos admitidos para la actividad. Por ejemplo, para las actividades de HDI, el tipo de servicio vinculado puede ser HDInsight o HDInsightOnDemand.


### <a name="error-code--2111"></a>Código de error:  2111

- **Mensaje**: `The type of the property '%propertyName;' is incorrect. The expected type is %expectedType;.`

- **Causa**: el tipo de la propiedad proporcionada no es el esperado.

- **Recomendación:**  corrija el tipo de la propiedad e inténtelo de nuevo.


### <a name="error-code--2112"></a>Código de error:  2112

- **Mensaje**: `The cloud type is unsupported or could not be determined for storage from the EndpointSuffix '%endpointSuffix;'.`

- **Causa**: el tipo de nube no es compatible o no se pudo determinar para el almacenamiento desde EndpointSuffix.

- **Recomendación:**  use el almacenamiento en otra nube e inténtelo de nuevo.


### <a name="error-code--2128"></a>Código de error:  2128

- **Mensaje**: `No response from the endpoint. Possible causes: network connectivity, DNS failure, server certificate validation or timeout.`

- **Causa**: conectividad de red, error de DNS, validación del certificado de servidor o tiempo de espera.

- **Recomendación:**  compruebe que el punto de conexión con el que intenta comunicarse responde a las solicitudes. Puede usar herramientas como Fiddler o Postman.



## <a name="custom"></a>Personalizado

La tabla siguiente se aplica a Azure Batch.
      
### <a name="error-code--2500"></a>Código de error:  2.500

- **Mensaje**: `Hit unexpected exception and execution failed.`

- **Causa**: `Can't launch command, or the program returned an error code.`

- **Recomendación:**  Compruebe que existe el archivo ejecutable. Si se inicia el programa, compruebe si los archivos *stdout.txt* y *stderr.txt* se cargaron en la cuenta de almacenamiento. Es un procedimiento recomendado emitir abundantes registros en el código para la depuración.


### <a name="error-code--2501"></a>Código de error:  2501

- **Mensaje**: `Cannot access user batch account; please check batch account settings.`

- **Causa**: Clave de acceso o nombre de grupo de Batch incorrectos.

- **Recomendación:** Compruebe el nombre del grupo y la clave de acceso de Batch en el servicio vinculado.


### <a name="error-code--2502"></a>Código de error:  2502

- **Mensaje**: `Cannot access user storage account; please check storage account settings.`

- **Causa**: Nombre de cuenta de almacenamiento o clave de acceso incorrectos.

- **Recomendación:** Compruebe la clave de acceso y el nombre de cuenta de almacenamiento en el servicio vinculado.


### <a name="error-code--2504"></a>Código de error:  2504

- **Mensaje**:  `Operation returned an invalid status code 'BadRequest'.`

- **Causa**: Hay demasiados archivos en folderPath de la actividad personalizada. El tamaño total de resourceFiles no puede superar los 32 768 caracteres.

- **Recomendación:** Elimine los archivos innecesarios. También puede comprimirlos y agregar un comando unzip para extraerlos. Por ejemplo, use `powershell.exe -nologo -noprofile   -command "& { Add-Type -A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ;  $folder\yourProgram.exe`.


### <a name="error-code--2505"></a>Código de error:  2505

- **Mensaje**: `Cannot create Shared Access Signature unless Account Key credentials are used.`

- **Causa**: Las actividades personalizadas solo admiten cuentas de almacenamiento que usen una clave de acceso.

- **Recomendación:** Consulte la descripción del error.


### <a name="error-code--2507"></a>Código de error:  2507

- **Mensaje**: `The folder path does not exist or is empty: ...`

- **Causa**: No hay archivos en la cuenta de almacenamiento en la ruta de acceso especificada.

- **Recomendación:** La ruta de acceso a la carpeta debe contener los archivos ejecutables que desea ejecutar.


### <a name="error-code--2508"></a>Código de error:  2508

- **Mensaje**:  `There are duplicate files in the resource folder.`

- **Causa**: Hay varios archivos con el mismo nombre en subcarpetas diferentes de folderPath.

- **Recomendación:** Estructura de carpetas de actividades personalizadas aplanada en folderPath.  Si necesita conservar la estructura de carpetas, comprima los archivos y extráigalos en Azure Batch mediante un comando unzip. Por ejemplo, use `powershell.exe -nologo -noprofile   -command "& { Add-Type -A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ;   $folder\yourProgram.exe`.


### <a name="error-code--2509"></a>Código de error:  2509

- **Mensaje**: `Batch   url ... is invalid; it must be in Uri format.`

- **Causa**: Las direcciones URL del lote tienen que ser similares a `https://mybatchaccount.eastus.batch.azure.com`

- **Recomendación:** Consulte la descripción del error.


### <a name="error-code--2510"></a>Código de error:  2510

- **Mensaje**: `An   error occurred while sending the request.`

- **Causa**: La dirección URL del lote no es válida.

- **Recomendación:** Compruebe la dirección URL del lote.
            

## <a name="hdinsight"></a>HDInsight

### <a name="error-code--200"></a>Código de error:  200

- **Mensaje**: `Unexpected error happened: '%error;'.`

- **Causa**: hay un problema de servicio interno.

- **Recomendación:**  póngase en contacto con el soporte técnico de ADF para obtener más ayuda.


### <a name="error-code--201"></a>Código de error:  201

- **Mensaje**: `JobType %jobType; is not found.`

- **Causa**: hay un nuevo tipo de trabajo que no es compatible con ADF.

- **Recomendación:**  póngase en contacto con el equipo de soporte técnico de ADF para obtener más ayuda.


### <a name="error-code--202"></a>Código de error:  202

- **Mensaje**: `Failed to create on demand HDI cluster. Cluster name or linked service name: '%clusterName;', error: '%message;'`

- **Causa**: el mensaje de error debe mostrar los detalles de lo que ha ido mal.

- **Recomendación:**  el mensaje de error debe ayudarle a solucionar el problema. Si no hay información suficiente, póngase en contacto con el soporte técnico de ADF para obtener más ayuda.


### <a name="error-code--203"></a>Código de error:  203

- **Mensaje**: `Failed to delete on demand HDI cluster. Cluster name or linked service name: '%clusterName;', error: '%message;'`

- **Causa**: el mensaje de error debe mostrar los detalles de lo que ha ido mal.

- **Recomendación:**  el mensaje de error debe ayudarle a solucionar el problema. Si no hay información suficiente, póngase en contacto con el soporte técnico de ADF para obtener más ayuda.


### <a name="error-code--204"></a>Código de error:  204

- **Mensaje**: `The resumption token is missing for runId '%runId;'.`

- **Causa**: hay un problema de servicio interno.

- **Recomendación:**  póngase en contacto con el soporte técnico de ADF para obtener más ayuda.


### <a name="error-code--205"></a>Código de error:  205

- **Mensaje**: `Failed to prepare cluster for LinkedService '%linkedServiceName;', the current resource status is '%status;'.`

- **Causa**: se produjo un error al crear el clúster de HDI a petición.

- **Recomendación:**  póngase en contacto con el soporte técnico de ADF para obtener más ayuda.


### <a name="error-code--206"></a>Código de error:  206

- **Mensaje**: `The batch ID for Spark job is invalid. Please retry your job, and if the problem persists, contact the ADF support for further assistance.`

- **Causa**: hubo un problema interno con el servicio que causó este error.

- **Recomendación:**  podría tratarse de un error transitorio. Vuelva a intentar el trabajo y, si el problema persiste, póngase en contacto con el soporte técnico de ADF para obtener más ayuda.


### <a name="error-code--207"></a>Código de error:  207

- **Mensaje**: `Could not determine the region from the provided storage account. Please try using another primary storage account for the on demand HDI or contact ADF support team and provide the activity run ID.`

- **Causa**: se produjo un error interno al intentar determinar la región desde la cuenta de almacenamiento principal.

- **Recomendación:**  pruebe con otro almacenamiento. En caso de que no sea una solución aceptable, póngase en contacto con el equipo de soporte técnico de ADF para obtener más ayuda.


### <a name="error-code--208"></a>Código de error:  208

- **Mensaje**: `Service Principal or the MSI authenticator are not instantiated. Please consider providing a Service Principal in the HDI on demand linked service which has permissions to create an HDInsight cluster in the provided subscription and try again. In case if this is not an acceptable solution, contact ADF support team for further assistance.`

- **Causa**: se produjo un error interno al intentar leer la entidad de servicio o crear una instancia de la autenticación de MSI.

- **Recomendación:**  considere la posibilidad de proporcionar una entidad de servicio que tenga permisos para crear un clúster de HDInsight en la suscripción proporcionada e inténtelo de nuevo. En caso de que no sea una solución aceptable, póngase en contacto con el equipo de soporte técnico de ADF para obtener más ayuda.


### <a name="error-code--2300"></a>Código de error:  2300

- **Mensaje**: `Failed to submit the job '%jobId;' to the cluster '%cluster;'. Error: %errorMessage;.`

<br>

- **Causa**: cuando el mensaje de error es similar a "No se pudo resolver el nombre remoto", es posible que el URI del clúster proporcionado no sea válido.


- **Recomendación:** Asegúrese de que el clúster no se ha eliminado y que el URI proporcionado es correcto. Al abrir el identificador URI en cualquier explorador, debe ver la interfaz de usuario de Ambari. Si el clúster está en una red virtual, el URI debe ser el URI privado. Para abrirlo, use una máquina virtual que forme parte de la misma red virtual. Para más información, consulte [aquí](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment#directly-connect-to-apache-hadoop-services).
                  

<br>

- **Causa**: cuando el mensaje de error es similar a "Se canceló una tarea", quiere decir que el envío del trabajo agotó el tiempo de espera.

- **Recomendación:**  podría tratarse de un problema de conectividad general de HDInsight o de un problema de conectividad de red. En primer lugar, confirme que la interfaz de usuario de HDInsight Ambari está disponible desde cualquier explorador. Confirme que las credenciales siguen siendo válidas. Si usa Integrated Runtime (IR) autohospedado, asegúrese de hacerlo desde la máquina virtual o la máquina donde está instalada la instancia de IR autohospedado. A continuación, intente volver a enviar el trabajo desde Data Factory. Si el problema persiste, póngase en contacto con el equipo de Data Factory para obtener ayuda.

<br>

- **Causa**: cuando el mensaje de error es similar a "El administrador del usuario está bloqueado en Ambari" o "No autorizado: El nombre de usuario o la contraseña de Ambari no son correctos", significa que las credenciales de HDInsight son incorrectas o han expirado.

- **Recomendación:**  Corrija las credenciales y vuelva a implementar el servicio vinculado. En primer lugar, asegúrese de que las credenciales funcionan en HDInsight; para ello, abra el URI del clúster en cualquier explorador e intente iniciar sesión. Si las credenciales no funcionan, puede restablecerlas desde Azure Portal.

<br>

- **Causa**: cuando el mensaje de error es similar a "502: el servidor web recibió una respuesta no válida mientras actuaba como puerta de enlace o servidor proxy", el servicio HDInsight devuelve este error.


- **Recomendación:** consulte la documentación de solución de problemas de Azure HDInsight, por ejemplo, https://hdinsight.github.io/ambari/ambari-ui-502-error.html, https://hdinsight.github.io/spark/spark-thriftserver-errors.htmlhttps://docs.microsoft.com/azure/application-gateway/application-gateway-troubleshooting-502.
                  

<br>

- **Causa**: cuando el mensaje de error es similar a "No se puede atender la solicitud de envío del trabajo porque el servicio Templeton está ocupado con demasiadas solicitudes de envío de trabajos" o "La cola root.joblauncher ya tiene 500 aplicaciones, no se puede aceptar el envío de la aplicación", significa que se están enviando demasiados trabajos a HDInsight al mismo tiempo.

- **Recomendación:**  considere la posibilidad de limitar el número de trabajos simultáneos que se envían a HDInsight. Consulte la simultaneidad de la actividad de Data Factory si los trabajos los envía la misma actividad. Cambie los desencadenadores para que las ejecuciones de canalización simultánea se repartan en el tiempo. Consulte la documentación de HDInsight para ajustar templeton.parallellism.job.submit como sugiere el error.


### <a name="error-code--2301"></a>Código de error:  2301

- **Mensaje**: `Could not get the status of the application '%physicalJobId;' from the HDInsight service. Received the following error: %message;. Please refer to HDInsight troubleshooting documentation or contact their support for further assistance.`

- **Causa**: el servicio o el clúster de HDInsight tiene problemas.


- **Recomendación:** este error se produce cuando ADF no recibe respuesta del clúster de HDInsight al intentar obtener el estado del trabajo en ejecución. Podría deberse a problemas en el propio clúster o a que el servicio HDInsight haya sufrido una interrupción. Consulte la documentación de solución de problemas de HDInsight en https://docs.microsoft.com/azure/hdinsight/hdinsight-troubleshoot-guide o póngase en contacto con el soporte técnico para obtener más ayuda.
                


### <a name="error-code--2302"></a>Código de error:  2302

- **Mensaje**: `Hadoop job failed with exit code '%exitCode;'. See '%logPath;/stderr' for more details. Alternatively, open the Ambari UI on the HDI cluster and find the logs for the job '%jobId;'. Contact HDInsight team for further support.`

- **Causa**: el trabajo se envió al clúster de HDI y generó un error.

- **Recomendación:**  siga el vínculo de registros de YARN en la salida de la ejecución de actividad y busque los errores en la salida de HDI. Si es necesario, póngase en contacto con el equipo de soporte técnico de HDInsight.


### <a name="error-code--2303"></a>Código de error:  2303

- **Mensaje**: `Hadoop job failed with transient exit code '%exitCode;'. See '%logPath;/stderr' for more details. Alternatively, open the Ambari UI on the HDI cluster and find the logs for the job '%jobId;'. Try again or contact HDInsight team for further support.`

- **Causa**: el trabajo se envió al clúster de HDI y generó un error.

- **Recomendación:**  siga el vínculo de registros de YARN en la salida de la ejecución de actividad y busque los errores en la salida de HDI. Vuelva a intentarlo o, si es necesario, póngase en contacto con el equipo de soporte técnico de HDInsight.


### <a name="error-code--2304"></a>Código de error:  2304

- **Mensaje**: `MSI authentication is not supported on storages for HDI activities.`

- **Causa**: los servicios vinculados de almacenamiento usados en el servicio vinculado o la actividad de HDI están configurados con la autenticación MSI, la cual no se admite.

- **Recomendación:**  proporcione cadenas de conexión completas para las cuentas de almacenamiento usadas en el servicio vinculado o la actividad de HDI.


### <a name="error-code--2305"></a>Código de error:  2305

- **Mensaje**: `Failed to initialize the HDInsight client for the cluster '%cluster;'. Error: '%message;'`

- **Causa**: la información de conexión del clúster de HDI es incorrecta, el usuario proporcionado no tiene los permisos para realizar la acción necesaria o el servicio HDInsight tenía problemas para responder a las solicitudes de ADF.

- **Recomendación:**  asegúrese de que la información del usuario sea correcta. Compruebe también que la interfaz de usuario de Ambari del clúster de HDI se pueda abrir en un explorador desde la máquina virtual en la que está instalado IR, en el caso de IR autohospedado o desde cualquier máquina, en el caso de Azure IR.


### <a name="error-code--2306"></a>Código de error:  2306

- **Mensaje**: `An invalid json is provided for script action '%scriptActionName;'. Error: '%message;'`

- **Causa**: el código JSON proporcionado para la acción de script no es válido.


- **Recomendación:** el mensaje de error debe ayudar a identificar el problema. Corrija la configuración de JSON e inténtelo de nuevo. Consulte https://docs.microsoft.com/azure/data-factory/compute-linked-services#azure-hdinsight-on-demand-linked-service para más información.
                


### <a name="error-code--2310"></a>Código de error:  2310

- **Mensaje**: `Failed to submit Spark job. Error: '%message;'`

- **Causa**: ADF intentó crear un lote en un clúster de Spark mediante Livy API (Livy/batch), pero recibió un error.

- **Recomendación:**  siga el mensaje de error para solucionar el problema. Si no hay suficiente información para resolverlo, póngase en contacto con el equipo de HDI y proporcione los identificadores de trabajo y lote, que se pueden encontrar en la salida de ejecución de actividad de la página de supervisión de ADF.


### <a name="error-code--2312"></a>Código de error:  2312

- **Mensaje**: `Spark job failed, batch id:%batchId;. Please follow the links in the activity run Output from ADF Monitoring page to troubleshoot the run on HDInsight Spark cluster. Please contact HDInsight support team for further assistance.`

- **Causa**: error en el trabajo en el clúster de HDInsight Spark.

- **Recomendación:**  siga los vínculos de la salida de ejecución de actividad de la página de supervisión de ADF para solucionar los problemas de ejecución en el clúster de HDInsight Spark. Póngase en contacto con el equipo de soporte técnico de HDInsight para obtener más ayuda.


### <a name="error-code--2313"></a>Código de error:  2313

- **Mensaje**: `The batch with ID '%batchId;' was not found on Spark cluster. Open the Spark History UI and try to find it there. Contact HDInsight support for further assistance.`

- **Causa**: el lote se eliminó en el clúster de HDInsight Spark.

- **Recomendación:**  solucione los problemas de lotes en el clúster de HDInsight Spark. Póngase en contacto con el soporte técnico de HDInsight para obtener más ayuda. 


### <a name="error-code--2328"></a>Código de error:  2328

- **Mensaje**: `Failed to create the on demand HDI cluster. Cluster or linked service name: '%clusterName;', error: '%message;'`

- **Causa**: `The error message should show the details of what went wrong.`

- **Recomendación:**  el mensaje de error debe ayudarle a solucionar el problema.


### <a name="error-code--2329"></a>Código de error:  2329

- **Mensaje**: `Failed to delete the on demand HDI cluster. Cluster or linked service name: '%clusterName;', error: '%message;'`

- **Causa**: el mensaje de error debe mostrar los detalles de lo que ha ido mal.

- **Recomendación:**  el mensaje de error debe ayudarle a solucionar el problema.


### <a name="error-code--2331"></a>Código de error:  2331

- **Mensaje**: `The file path should not be null or empty.`

- **Causa**: la ruta de acceso del archivo proporcionada está vacía.

- **Recomendación:**  proporcione una ruta de acceso para un archivo que exista.


### <a name="error-code--2340"></a>Código de error:  2340

- **Mensaje**: `HDInsightOnDemand linked service does not support execution via SelfHosted IR. Your IR name is '%IRName;'. Please select an Azure IR instead.`

- **Causa**: el servicio vinculado HDInsightOnDemand no admite la ejecución a través de IR autohospedado.

- **Recomendación:**  seleccione una instancia de Azure IR e inténtelo de nuevo.


### <a name="error-code--2341"></a>Código de error:  2341

- **Mensaje**: `HDInsight cluster URL '%clusterUrl;' is incorrect, it must be in URI format and the scheme must be 'https'.`

- **Causa**: la dirección URL proporcionada no tiene el formato correcto.

- **Recomendación:**  corrija la dirección URL del clúster e inténtelo de nuevo.


### <a name="error-code--2342"></a>Código de error:  2342

- **Mensaje**: `Failed to connect to HDInsight cluster: '%errorMessage;'.`

- **Causa**: las credenciales proporcionadas para el clúster son incorrectas o se produjo un problema de conexión o configuración de la red; también, puede que IR esté teniendo problemas para conectarse al clúster.

- **Recomendación:**  
      1. compruebe que las credenciales sean correctas; para ello, abra la interfaz de usuario de Ambari del clúster de HDInsight en un explorador.
      2. Si el clúster se encuentra en la red virtual y se está usando IR autohospedado, la dirección URL de HDI debe ser la dirección URL privada de las redes virtuales, lo que significa que debe tener "-int" después del nombre del clúster. Por ejemplo, "https://mycluster.azurehdinsight.net/" debe cambiarse a "https://mycluster-int.azurehdinsight.net/".
      2. Si el clúster está en la red virtual, se usa IR autohospedado, se ha utilizado la dirección URL privada y se sigue produciendo un error en la conexión, la máquina virtual en la que se instaló IR tenía problemas para conectarse a HDI. Conéctese a la máquina virtual en la que está instalado IR y abra la interfaz de usuario de Ambari en un explorador. Use la dirección URL privada del clúster. Esta conexión debería funcionar desde el explorador. Si no es así, póngase en contacto con el equipo de soporte técnico de HDInsight para obtener más ayuda.
      3. Si no se usa IR autohospedado, el clúster de HDI debe ser accesible públicamente. Abra la interfaz de usuario de Ambari en un explorador y asegúrese de que se abre. Si hay algún problema con el clúster o con los servicios que contiene, póngase en contacto con el equipo de soporte técnico de HDInsight para obtener ayuda.
      De modo que, en general, la dirección URL del clúster de HDI usada en el servicio vinculado de ADF debe ser accesible para IR de ADF (autohospedado o Azure) para que la conexión de prueba se supere y para que funcionen las ejecuciones. Una manera fácil de comprobarlo es abrir esa dirección URL en un explorador en una máquina virtual o cualquier máquina pública.
    


### <a name="error-code--2343"></a>Código de error:  2343

- **Mensaje**: `User name and password cannot be null or empty to connect to the HDInsight cluster.`

- **Causa**: el nombre de usuario o la contraseña están vacíos.

- **Recomendación:**  proporcione las credenciales correctas para conectarse a HDI e inténtelo de nuevo.


### <a name="error-code--2345"></a>Código de error:  2345

- **Mensaje**: `Failed to read the content of the hive script. Error: '%message;'`

- **Causa**: el archivo de script no existe o ADF no se pudo conectar a la ubicación del script.

- **Recomendación:**  compruebe que el script existe y que el servicio vinculado asociado tenga las credenciales adecuadas para la conexión.


### <a name="error-code--2346"></a>Código de error:  2346

- **Mensaje**: `Failed to create ODBC connection to the HDI cluster with error message '%message;'.`

- **Causa**: ADF intentó establecer una conexión ODBC con el clúster de HDI y se produjo un error.

- **Recomendación:**  el mensaje de error y el código de error deben ayudar a solucionar los errores de conexión ODBC. En caso de que no sean suficientes para resolver el problema, póngase en contacto con el equipo de Azure HDInsight.


### <a name="error-code--2347"></a>Código de error:  2347

- **Mensaje**: `Hive execution through ODBC failed with error message '%message;'.`

- **Causa**: ADF envió el script de Hive para ejecutar al clúster de HDI a través de la conexión ODBC y el script ha generado un error en HDI.

- **Recomendación:**  la ejecución del script de Hive ha dado error en el clúster de HDI y el código de error y el mensaje de error deberían ayudarle a solucionar el problema. En caso de que no sean suficientes para resolver el problema, póngase en contacto con el equipo de Azure HDInsight.


### <a name="error-code--2348"></a>Código de error:  2348

- **Mensaje**: `The main storage has not been initialized. Please check the properties of the storage linked service in the HDI linked service.`

- **Causa**: las propiedades del servicio vinculado de almacenamiento no se han establecido correctamente.

- **Recomendación:**  solo se admiten cadenas de conexión completas en el servicio vinculado de almacenamiento principal para las actividades de HDI. Asegúrese de que no usa la autenticación o las aplicaciones de MSI.


### <a name="error-code--2350"></a>Código de error:  2350

- **Mensaje**: `Failed to prepare the files for the run '%jobId;'. HDI cluster: '%cluster;', Error: '%errorMessage;'`

- **Causa**: las credenciales proporcionadas para conectarse al almacenamiento donde se deben ubicar los archivos son incorrectas o los archivos no existen ahí.

- **Recomendación:**  este error se produce cuando ADF realiza pasos de preparación para actividades de HDI. Así, intenta copiar archivos en el almacenamiento principal antes de enviar el trabajo a HDI. Asegúrese de que los archivos se encuentren en la ubicación proporcionada y que la conexión de almacenamiento sea correcta. Las actividades de ADF HDI no admiten la autenticación de MSI en cuentas de almacenamiento relacionadas con las actividades de HDI, por lo que debe asegurarse de que esos servicios vinculados tengan claves completas o usen Azure Key Vault.


### <a name="error-code--2351"></a>Código de error:  2351

- **Mensaje**: `Could not open the file '%filePath;' in container/fileSystem '%container;'.`

- **Causa**: el archivo no existe en la ruta de acceso especificada.

- **Recomendación:**  compruebe si el archivo existe realmente y si el servicio vinculado con información de conexión que apunta a este archivo tiene las credenciales correctas.


### <a name="error-code--2352"></a>Código de error:  2352

- **Mensaje**: `The file storage has not been initialized. Please check the properties of the file storage linked service in the HDI activity.`

- **Causa**: las propiedades del servicio vinculado de almacenamiento de archivos no se han establecido correctamente.

- **Recomendación:**  asegúrese de que las propiedades del servicio vinculado de almacenamiento de archivos estén configuradas correctamente.


### <a name="error-code--2353"></a>Código de error:  2353

- **Mensaje**: `The script storage has not been initialized. Please check the properties of the script storage linked service in the HDI activity.`

- **Causa**: las propiedades del servicio vinculado de almacenamiento de scripts no se han establecido correctamente.

- **Recomendación:**  asegúrese de que las propiedades del servicio vinculado de almacenamiento de scripts estén configuradas correctamente.


### <a name="error-code--2354"></a>Código de error:  2354

- **Mensaje**: `The storage linked service type '%linkedServiceType;' is not supported for '%executorType;' activities for property '%linkedServicePropertyName;'.`

- **Causa**: el tipo de servicio vinculado de almacenamiento no es compatible con la actividad.

- **Recomendación:**  asegúrese de que el servicio vinculado seleccionado tenga uno de los tipos admitidos de la actividad. Las actividades de HDI admiten los servicios vinculados AzureBlobStorage y AzureBlobFSStorage.


### <a name="error-code--2355"></a>Código de error:  2355

- **Mensaje**: `The '%value' provided for commandEnvironment is incorrect. The expected value should be an array of strings where each string has the format CmdEnvVarName=CmdEnvVarValue.`

- **Causa**: el valor proporcionado para commandEnvironment es incorrecto.

- **Recomendación:**  
      asegúrese de que el valor proporcionado sea similar a: \"commandEnvironment\": [ \"variableName=variableValue\" ] y que cada variable aparezca en la lista solo una vez.
    


### <a name="error-code--2356"></a>Código de error:  2356

- **Mensaje**: `The commandEnvironment already contains a variable named '%variableName;'.`

- **Causa**: la variable se proporcionó dos veces en commandEnvironment.

- **Recomendación:**  
      asegúrese de que el valor proporcionado sea similar a: \"commandEnvironment\": [ \"variableName=variableValue\" ] y que cada variable aparezca en la lista solo una vez.
    


### <a name="error-code--2357"></a>Código de error:  2357

- **Mensaje**: `The certificate or password is wrong for ADLS Gen 1 storage.`

- **Causa**: las credenciales proporcionadas son incorrectas.

- **Recomendación:**  compruebe la información de conexión en el servicio vinculado ADLS Gen 1 y asegúrese de que la conexión de prueba se realiza correctamente.


### <a name="error-code--2358"></a>Código de error:  2358

- **Mensaje**: `The value '%value;' for the required property 'TimeToLive' in the on demand HDInsight linked service '%linkedServiceName;' has invalid format. It should be a timespan between '00:05:00' and '24:00:00'.`

- **Causa**: el valor proporcionado para la propiedad necesaria "TimeToLive" tiene un formato no válido. 

- **Recomendación:**  actualice el valor para que esté en el intervalo sugerido e inténtelo de nuevo.


### <a name="error-code--2359"></a>Código de error:  2359

- **Mensaje**: `The value '%value;' for the property 'roles' is invalid. Expected types are 'zookeeper', 'headnode', and 'workernode'.`

- **Causa**: el valor proporcionado para la propiedad "roles" no es válido.

- **Recomendación:**  actualice el valor para que sea una de las sugerencias e inténtelo de nuevo.


### <a name="error-code--2360"></a>Código de error:  2360

- **Mensaje**: `The connection string in HCatalogLinkedService is invalid. Encountered an error while trying to parse: '%message;'.`

- **Causa**: la cadena de conexión proporcionada para HCatalogLinkedService no es válida.

- **Recomendación:**  actualice el valor a una cadena de conexión de Azure SQL correcta e inténtelo de nuevo.


### <a name="error-code--2361"></a>Código de error:  2361

- **Mensaje**: `Failed to create on demand HDI cluster. Cluster name is '%clusterName;'.`

- **Causa**: no se pudo crear el clúster y ADF no ha recibido un error del servicio HDInsight.

- **Recomendación:**  abra Azure Portal, intente encontrar el recurso de HDI con el nombre proporcionado y compruebe el estado de aprovisionamiento. Póngase en contacto con el equipo de soporte técnico de HDInsight para obtener más ayuda.


### <a name="error-code--2362"></a>Código de error:  2362

- **Mensaje**: `Only Azure Blob storage accounts are supported as additional storages for HDInsight on demand linked service.`

- **Causa**: el almacenamiento adicional proporcionado no era almacenamiento de blobs de Azure.

- **Recomendación:**  proporcione una cuenta de Azure Blob Storage como almacenamiento adicional para el servicio vinculado de HDInsight a petición.



## <a name="web-activity"></a>Actividad web

### <a name="error-code--2128"></a>Código de error:  2128

- **Mensaje**: `No response from the endpoint. Possible causes: network connectivity, DNS failure, server certificate validation or timeout.`

- **Causa**: conectividad de red, error de DNS, validación del certificado de servidor o tiempo de espera.

- **Recomendación:**  compruebe que el punto de conexión con el que intenta comunicarse responde a las solicitudes. Puede usar herramientas como Fiddler o Postman.


### <a name="error-code--2108"></a>Código de error:  2108

- **Mensaje**: `Error calling the endpoint '%url;'. Response status code: '%code;'`

- **Causa**: no se pudo realizar la solicitud debido a un problema subyacente (conectividad de red, error de DNS, validación del certificado de servidor o tiempo de espera).

- **Recomendación:**  Use Fiddler o Postman para validar la solicitud.
<br>


#### <a name="more-details"></a>Más detalles
Para usar Fiddler para crear una sesión HTTP de la aplicación web supervisada:

1. Descargue, instale y abra [Fiddler](https://www.telerik.com/download/fiddler).

1. Si su aplicación web usa HTTPS, vaya a **Tools** > **Fiddler Options** > **HTTPS** (Herramientas>Opciones de Fiddler>HTTPS). Seleccione **Capture HTTPS CONNECTs** (Capturar CONEXIONES HTTPS) y **Decrypt HTTPS Traffic** (Descifrar tráfico HTTPS).

   ![Opciones de Fiddler](media/data-factory-troubleshoot-guide/fiddler-options.png)

1. Si la aplicación usa certificados TLS/SSL, agregue el certificado de Fiddler al dispositivo. Vaya a **Tools** > **Fiddler Options** > **HTTPS** > **Actions** > **Export Root Certificate to Desktop** (Herramientas>Opciones de Fiddler>HTTPS>Acciones>Exportar el certificado raíz al escritorio).

1. Desactive la captura en **File** > **Capture Traffic** (Archivo > Capturar tráfico). O bien, presione **F12**.

1. Borre la caché del explorador para quitar todos los elementos almacenados en caché, que tendrá que volver a descargar.

1. Creación de una solicitud:

   1. Seleccione la pestaña **Composer** (Compositor).

   1. Establezca el método HTTP y la dirección URL.
   
   1. Agregue encabezados y un cuerpo de la solicitud si es necesario.

   1. Seleccione **Execute**(Ejecutar).

1. Active la captura de tráfico de nuevo y complete la transacción problemática en su página.

1. Vaya a **File** > **Save** > **All Sessions** (Archivo>Guardar>Todas las sesiones).

Para más información, consulte [Introducción a Fiddler](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/ConfigureFiddler)

## <a name="next-steps"></a>Pasos siguientes

Para obtener ayuda para solucionar problemas, pruebe estos recursos:

*  [Blog de Data Factory](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Solicitud de características de Data Factory](https://feedback.azure.com/forums/270578-data-factory)
*  [Vídeos de Azure](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Foro de MSDN](https://social.msdn.microsoft.com/Forums/home?sort=relevancedesc&brandIgnore=True&searchTerm=data+factory)
*  [Foro de Stack Overflow para Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Información de Twitter sobre Data Factory](https://twitter.com/hashtag/DataFactory)


            
