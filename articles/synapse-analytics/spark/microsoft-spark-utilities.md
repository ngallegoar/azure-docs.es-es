---
title: Introducción a las utilidades Spark para Microsoft
description: Tutorial sobre el uso de MSSparkutils en los cuadernos de Azure Synapse Analytics.
author: ruxu
services: synapse-analytics
ms.service: synapse-analytics
ms.topic: reference
ms.subservice: spark
ms.date: 09/10/2020
ms.author: ruxu
ms.reviewer: ''
zone_pivot_groups: programming-languages-spark-all-minus-sql
ms.openlocfilehash: 3c4e062393f9d75d478720041436c2e0f54485a3
ms.sourcegitcommit: 6a770fc07237f02bea8cc463f3d8cc5c246d7c65
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95795066"
---
# <a name="introduction-of-microsoft-spark-utilities"></a>Introducción a las utilidades Spark para Microsoft

Las utilidades de Spark para Microsoft (MSSparkUtils) son un paquete integrado que le ayuda a realizar las tareas más comunes con mayor facilidad. Puede usar MSSparkUtils para trabajar con sistemas de archivos, obtener variables de entorno y trabajar con secretos. MSSparkUtils está disponible en los cuadernos de `PySpark (Python)`, `Scala` y `.NET Spark (C#)`, así como en canalizaciones de Synapse.

## <a name="pre-requisites"></a>Requisitos previos

### <a name="configure-access-to-azure-data-lake-storage-gen2"></a>Configuración del acceso a Azure Data Lake Storage Gen2 

Los cuadernos de Synapse usan el tránsito de Azure Active Directory (Azure AD) para acceder a las cuentas de ADLS Gen2. Debe ser un **colaborador de Blob Storage** para acceder a la cuenta (o carpeta) de ADLS Gen2. 

Las canalizaciones de Synapse usan la identidad del área de trabajo (MSI) para acceder a las cuentas de almacenamiento. Para usar MSSparkUtils en las actividades de canalización, la identidad del área de trabajo debe ser un **colaborador de Blob Storage** para acceder a la cuenta (o carpeta) de ADLS Gen2.

Siga los pasos a continuación para asegurarse de que Azure AD y la MSI del área de trabajo tienen acceso a la cuenta de ADLS Gen2:
1. Abra [Azure Portal](https://portal.azure.com/) y la cuenta de almacenamiento a la que quiere acceder. Puede desplazarse al contenedor específico al que quiere obtener acceso.
2. Seleccione **Control de acceso (IAM)** en el panel izquierdo.
3. Asigne su **cuenta de Azure AD** y la **identidad de su área de trabajo** (igual que el nombre del área de trabajo) al rol de **colaborador de datos de Storage Blob** en la cuenta de almacenamiento o compruebe que ya estén asignadas. 
4. Seleccione **Guardar**.

Puede acceder a los datos en ADLS Gen2 con Synapse Spark mediante la siguiente dirección URL:

<code>abfss://<container_name>@<storage_account_name>.dfs.core.windows.net/<path></code>

### <a name="configure-access-to-azure-blob-storage"></a>Configuración del acceso a Azure Blob Storage 

Synapse aprovecha las **firmas de acceso compartido (SAS)** para acceder a Azure Blob Storage. Para evitar la exposición de las claves de SAS en el código, se recomienda crear un nuevo servicio vinculado en el área de trabajo de Synapse para la cuenta de Azure Blob Storage a la que quiere acceder.

Siga los pasos a continuación para agregar un nuevo servicio vinculado para una cuenta de Azure Blob Storage:

1. Abra [Azure Synapse Studio](https://web.azuresynapse.net/).
2. Seleccione **Administrar** en el panel izquierdo y, a continuación, seleccione **Servicios vinculados** en **Conexiones externas**.
3. Busque **Azure Blob Storage** en el panel **Nuevo servicio vinculado** de la derecha.
4. Seleccione **Continuar**.
5. Seleccione la cuenta de Azure Blob Storage a la que quiere acceder y configure el nombre del servicio vinculado. Sugiera el uso de una **clave de cuenta** como **método de autenticación**.
6. Seleccione **Prueba de conexión** para validar la configuración.
7. Seleccione **Crear** primero y haga clic en **Publicar todo** para guardar los cambios. 

Puede acceder a los datos en Azure Blob Storage con Synapse Spark a través de la siguiente dirección URL:

<code>wasb[s]://<container_name>@<storage_account_name>.blob.core.windows.net/<path></code>

Este es un ejemplo de código:


:::zone pivot = "programming-language-python"

```python
from pyspark.sql import SparkSession

# Azure storage access info
blob_account_name = 'Your account name' # replace with your blob name
blob_container_name = 'Your container name' # replace with your container name
blob_relative_path = 'Your path' # replace with your relative folder path
linked_service_name = 'Your linked service name' # replace with your linked service name

blob_sas_token = mssparkutils.credentials.getConnectionStringOrCreds(linked_service_name)

# Allow SPARK to access from Blob remotely

wasb_path = 'wasbs://%s@%s.blob.core.windows.net/%s' % (blob_container_name, blob_account_name, blob_relative_path)

spark.conf.set('fs.azure.sas.%s.%s.blob.core.windows.net' % (blob_container_name, blob_account_name), blob_sas_token)
print('Remote blob path: ' + wasb_path)
```

::: zone-end

:::zone pivot = "programming-language-scala"

```scala
val blob_account_name = "" // replace with your blob name
val blob_container_name = "" //replace with your container name
val blob_relative_path = "/" //replace with your relative folder path
val linked_service_name = "" //replace with your linked service name


val blob_sas_token = mssparkutils.credentials.getConnectionStringOrCreds(linked_service_name)

val wasbs_path = f"wasbs://$blob_container_name@$blob_account_name.blob.core.windows.net/$blob_relative_path"
spark.conf.set(f"fs.azure.sas.$blob_container_name.$blob_account_name.blob.core.windows.net",blob_sas_token)

```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp

```

::: zone-end
 
###  <a name="configure-access-to-azure-key-vault"></a>Configuración de acceso a Azure Key Vault

Puede agregar una instancia de Azure Key Vault como servicio vinculado para administrar sus credenciales en Synapse. Siga estos pasos para agregar una instancia de Azure Key Vault como servicio vinculado de Synapse:
1. Abra [Azure Synapse Studio](https://web.azuresynapse.net/).
2. Seleccione **Administrar** en el panel izquierdo y, a continuación, seleccione **Servicios vinculados** en **Conexiones externas**.
3. Busque **Azure Key Vault** en el panel **Nuevo servicio vinculado** de la derecha.
4. Seleccione la cuenta de Azure Key Vault a la que quiere acceder y configure el nombre del servicio vinculado.
5. Seleccione **Prueba de conexión** para validar la configuración.
6. Seleccione **Crear** primero y haga clic en **Publicar todo** para guardar el cambio. 

Los cuadernos de Synapse usan el tránsito de Azure Active Directory (Azure AD) para acceder a Azure Key Vault. Las canalizaciones de Synapse usan la identidad del área de trabajo (MSI) para acceder a Azure Key Vault. Para asegurarse de que el código funciona en el cuaderno y en la canalización de Synapse, se recomienda conceder el permiso de acceso al secreto tanto a la cuenta de Azure AD como a la identidad del área de trabajo.

Siga los pasos a continuación para conceder acceso al secreto a la identidad del área de trabajo:
1. Abra [Azure Portal](https://portal.azure.com/) y la instancia de Azure Key Vault a la que quiere acceder. 
2. Seleccione **Directivas de acceso** en el panel izquierdo.
3. Seleccione **Agregar directiva de acceso**. 
    - Elija **Administración de claves, secretos y certificados** como plantilla de configuración.
    - Seleccione la **cuenta de Azure AD** y la **identidad del área de trabajo** (igual que el nombre del área de trabajo) en la opción Seleccionar entidad de seguridad, o asegúrese de que ya estén asignadas. 
4. Elija **Seleccionar**  y **Agregar**.
5. Seleccione el botón **Guardar** para confirmar los cambios.  

## <a name="file-system-utilities"></a>Utilidades del sistema de archivos

`mssparkutils.fs` ofrece utilidades para trabajar con varios sistemas de archivos, como Azure Data Lake Storage Gen2 (ADLS Gen2) y Azure Blob Storage. Asegúrese de configurar el acceso a [Azure Data Lake Storage Gen2](#configure-access-to-azure-data-lake-storage-gen2) y [Azure Blob Storage](#configure-access-to-azure-blob-storage) adecuadamente.

Ejecute los siguientes comandos para obtener información general sobre los métodos disponibles:

:::zone pivot = "programming-language-python"

```python
from notebookutils import mssparkutils
mssparkutils.fs.help()
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.fs.help()
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
using Microsoft.Spark.Extensions.Azure.Synapse.Analytics.Notebook.MSSparkUtils;
FS.Help()
```

::: zone-end

El resultado es:
```
mssparkutils.fs provides utilities for working with various FileSystems.

Below is overview about the available methods:

cp(from: String, to: String, recurse: Boolean = false): Boolean -> Copies a file or directory, possibly across FileSystems
mv(from: String, to: String, recurse: Boolean = false): Boolean -> Moves a file or directory, possibly across FileSystems
ls(dir: String): Array -> Lists the contents of a directory
mkdirs(dir: String): Boolean -> Creates the given directory if it does not exist, also creating any necessary parent directories
put(file: String, contents: String, overwrite: Boolean = false): Boolean -> Writes the given String out to a file, encoded in UTF-8
head(file: String, maxBytes: int = 1024 * 100): String -> Returns up to the first 'maxBytes' bytes of the given file as a String encoded in UTF-8
append(file: String, content: String, createFileIfNotExists: Boolean): Boolean -> Append the content to a file
rm(dir: String, recurse: Boolean = false): Boolean -> Removes a file or directory

Use mssparkutils.fs.help("methodName") for more info about a method.

```

### <a name="list-files"></a>Enumerar archivos
Enumerar el contenido de un directorio.


:::zone pivot = "programming-language-python"

```python
mssparkutils.fs.ls('Your directory path')
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.fs.ls("Your directory path")
```
::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
FS.Ls("Your directory path")
```

::: zone-end


### <a name="view-file-properties"></a>Vea las propiedades del archivo.
Devuelve las propiedades del archivo, incluido el nombre, la ruta de acceso, el tamaño y si es un directorio o un archivo.

:::zone pivot = "programming-language-python"

```python
files = mssparkutils.fs.ls('Your directory path')
for file in files:
    print(file.name, file.isDir, file.isFile, file.path, file.size)
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
val files = mssparkutils.fs.ls("/")
files.foreach{
    file => println(file.name,file.isDir,file.isFile,file.size)
}
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
var Files = FS.Ls("/");
foreach(var File in Files) {
    Console.WriteLine(File.Name+" "+File.IsDir+" "+File.IsFile+" "+File.Size);
}
```

::: zone-end

### <a name="create-new-directory"></a>Creación de un directorio

Crea el directorio especificado si no existe, y también crea los directorios principales necesarios.

:::zone pivot = "programming-language-python"

```python
mssparkutils.fs.mkdirs('new directory name')
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.fs.mkdirs("new directory name")
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
FS.Mkdirs("new directory name")
```

::: zone-end

### <a name="copy-file"></a>Copiar archivo

Copia un archivo o un directorio. Admite la copia entre sistemas de archivos.

:::zone pivot = "programming-language-python"

```python
mssparkutils.fs.cp('source file or directory', 'destination file or directory', True)# Set the third parameter as True to copy all files and directories recursively
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.fs.cp("source file or directory", "destination file or directory", true) // Set the third parameter as True to copy all files and directories recursively
```
::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
FS.Cp("source file or directory", "destination file or directory", true) // Set the third parameter as True to copy all files and directories recursively
```

::: zone-end

### <a name="preview-file-content"></a>Vista previa del contenido del archivo

Devuelve hasta los primeros "maxBytes" bytes del archivo especificado como una cadena codificada en UTF-8.

:::zone pivot = "programming-language-python"

```python
mssparkutils.fs.head('file path', maxBytes to read)
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.fs.head("file path", maxBytes to read)
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
FS.Head("file path", maxBytes to read)
```

::: zone-end

### <a name="move-file"></a>Mover archivo

Mueve un archivo o un directorio. Permite el movimiento entre sistemas de archivos.

:::zone pivot = "programming-language-python"

```python
mssparkutils.fs.mv('source file or directory', 'destination directory', True) # Set the last parameter as True to firstly create the parent directory if it does not exist
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.fs.mv("source file or directory", "destination directory", true) // Set the last parameter as True to firstly create the parent directory if it does not exist
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
FS.Mv("source file or directory", "destination directory", true)
```

::: zone-end

### <a name="write-file"></a>Escritura de archivos

Escribe la cadena especificada en un archivo, codificada en formato UTF-8.

:::zone pivot = "programming-language-python"

```python
mssparkutils.fs.put("file path", "content to write", True) # Set the last parameter as True to overwrite the file if it existed already
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.fs.put("file path", "content to write", true) // Set the last parameter as True to overwrite the file if it existed already
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
FS.Put("file path", "content to write", true) // Set the last parameter as True to overwrite the file if it existed already
```

::: zone-end

### <a name="append-content-to-a-file"></a>Adición de contenido a un archivo

Anexa la cadena especificada a un archivo, codificada en formato UTF-8.

:::zone pivot = "programming-language-python"

```python
mssparkutils.fs.append('file path','content to append',True) # Set the last parameter as True to create the file if it does not exist
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.fs.append("file path","content to append",true) // Set the last parameter as True to create the file if it does not exist
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
FS.Append("file path","content to append",true) // Set the last parameter as True to create the file if it does not exist
```

::: zone-end

### <a name="delete-file-or-directory"></a>Eliminación de un archivo o directorio

Elimina un archivo o directorio.

:::zone pivot = "programming-language-python"

```python
mssparkutils.fs.rm('file path', True) # Set the last parameter as True to remove all files and directories recursively 
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.fs.rm("file path", true) // Set the last parameter as True to remove all files and directories recursively 
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
FS.Rm("file path", true) // Set the last parameter as True to remove all files and directories recursively 
```

::: zone-end


## <a name="credentials-utilities"></a>Utilidades de credenciales

Puede usar las utilidades de credenciales de MSSparkUtils para obtener los tokens de acceso de los servicios vinculados y administrar los secretos en Azure Key Vault. 

Ejecute el siguiente comando para obtener información general sobre los métodos disponibles:

:::zone pivot = "programming-language-python"

```python
mssparkutils.credentials.help()
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.credentials.help()
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
Credentials.Help()
```

::: zone-end

Resultado obtenido:

```
getToken(audience, name): returns AAD token for a given audience, name (optional)
isValidToken(token): returns true if token hasn't expired
getConnectionStringOrCreds(linkedService): returns connection string or credentials for linked service
getSecret(akvName, secret, linkedService): returns AKV secret for a given AKV linked service, akvName, secret key
getSecret(akvName, secret): returns AKV secret for a given akvName, secret key
putSecret(akvName, secretName, secretValue, linkedService): puts AKV secret for a given akvName, secretName
putSecret(akvName, secretName, secretValue): puts AKV secret for a given akvName, secretName
```

### <a name="get-token"></a>Obtener el token

Devuelve un token de Azure AD para un público determinado por nombre (opcional). En la tabla siguiente se enumeran todos los tipos de público disponibles: 

|Tipo de público|Clave de público|
|--|--|
|Tipo de resolución de público|"Audience"|
|Recurso de público de Storage|"Storage"|
|Recurso de público de Data Warehouse|"DW"|
|Recurso de público de lago de datos|"AzureManagement"|
|Recurso de público de almacén|"DataLakeStore"|
|Recurso de público de base de datos de OSS de Azure|"AzureOSSDB"|
|Recurso de Azure Synapse|"Synapse"|
|Recurso de Azure Data Factory|"ADF"|

:::zone pivot = "programming-language-python"

```python
mssparkutils.credentials.getToken('audience Key')
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.credentials.getToken("audience Key")
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
Credentials.GetToken("audience Key")
```

::: zone-end


### <a name="validate-token"></a>Validación de token

Devuelve true si el token no ha expirado.

:::zone pivot = "programming-language-python"

```python
mssparkutils.credentials.isValidToken('your token')
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.credentials.isValidToken("your token")
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
Credentials.IsValidToken("your token")
```

::: zone-end


### <a name="get-connection-string-or-credentials-for-linked-service"></a>Obtención de la cadena de conexión o las credenciales para el servicio vinculado

Devuelve la cadena de conexión o las credenciales para el servicio vinculado. 

:::zone pivot = "programming-language-python"

```python
mssparkutils.credentials.getConnectionStringOrCreds('linked service name')
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.credentials.getConnectionStringOrCreds("linked service name")
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
Credentials.GetConnectionStringOrCreds("linked service name")
```

::: zone-end


### <a name="get-secret-using-workspace-identity"></a>Obtención del secreto mediante la identidad del área de trabajo

Devuelve el secreto de Azure Key Vault para un nombre de instancia de Azure Key Vault, un nombre de secreto y un nombre de servicio vinculado concretos mediante la identidad del área de trabajo. Asegúrese de configurar el acceso a [Azure Key Vault](#configure-access-to-azure-key-vault) adecuadamente.

:::zone pivot = "programming-language-python"

```python
mssparkutils.credentials.getSecret('azure key vault name','secret name','linked service name')
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.credentials.getSecret("azure key vault name","secret name","linked service name")
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
Credentials.GetSecret("azure key vault name","secret name","linked service name")
```

::: zone-end


### <a name="get-secret-using-user-credentials"></a>Obtención del secreto con las credenciales de usuario

Devuelve el secreto de Azure Key Vault para un nombre de instancia de Azure Key Vault, un nombre de secreto y un nombre de servicio vinculado concretos mediante las credenciales del usuario. 

:::zone pivot = "programming-language-python"

```python
mssparkutils.credentials.getSecret('azure key vault name','secret name')
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.credentials.getSecret("azure key vault name","secret name")
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
Credentials.GetSecret("azure key vault name","secret name")
```

::: zone-end

### <a name="put-secret-using-workspace-identity"></a>Asignación de secreto mediante la identidad del área de trabajo

Asigna el secreto de Azure Key Vault a un nombre de instancia de Azure Key Vault, un nombre de secreto y un nombre de servicio vinculado concretos mediante la identidad del área de trabajo. Debe configurar el acceso a [Azure Key Vault](#configure-access-to-azure-key-vault) adecuadamente.

:::zone pivot = "programming-language-python"

```python
mssparkutils.credentials.putSecret('azure key vault name','secret name','secret value','linked service name')
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.credentials.putSecret("azure key vault name","secret name","secret value","linked service name")
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp

```

::: zone-end


### <a name="put-secret-using-user-credentials"></a>Asignación del secreto con las credenciales de usuario

Asigna el secreto de Azure Key Vault a un nombre de instancia de Azure Key Vault, un nombre de secreto y un nombre de servicio vinculado concretos mediante las credenciales del usuario. 

:::zone pivot = "programming-language-python"

```python
mssparkutils.credentials.putSecret('azure key vault name','secret name','secret value')
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.credentials.putSecret("azure key vault name","secret name","secret value")
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp

```

::: zone-end


## <a name="environment-utilities"></a>Utilidades de entorno 

Ejecute los siguientes comandos para obtener información general sobre los métodos disponibles:

:::zone pivot = "programming-language-python"

```python
mssparkutils.env.help()
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.env.help()
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
Env.Help()
```

::: zone-end

Resultado obtenido:
```
getUserName(): returns user name
getUserId(): returns unique user id
getJobId(): returns job id
getWorkspaceName(): returns workspace name
getPoolName(): returns Spark pool name
getClusterId(): returns cluster id
```

### <a name="get-user-name"></a>Obtención del nombre de usuario

Devuelve el nombre del usuario actual.

:::zone pivot = "programming-language-python"

```python
mssparkutils.env.getUserName()
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.env.getUserName()
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
Env.GetUserName()
```

::: zone-end

### <a name="get-user-id"></a>Obtención del id. de usuario

Devuelve el id. de usuario actual.

:::zone pivot = "programming-language-python"

```python
mssparkutils.env.getUserId()
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.env.getUserId()
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
Env.GetUserId()
```

::: zone-end

### <a name="get-job-id"></a>Obtención del id. de trabajo

Devuelve el id. de trabajo.

:::zone pivot = "programming-language-python"

```python
mssparkutils.env.getJobId()
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.env.getJobId()
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
Env.GetJobId()
```

::: zone-end

### <a name="get-workspace-name"></a>Obtención del nombre del área de trabajo

Devuelve el nombre del área de trabajo.

:::zone pivot = "programming-language-python"

```python
mssparkutils.env.getWorkspaceName()
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.env.getWorkspaceName()
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
Env.GetWorkspaceName()
```

::: zone-end

### <a name="get-pool-name"></a>Obtención del nombre del grupo

Devuelve el nombre del grupo de Spark.

:::zone pivot = "programming-language-python"

```python
mssparkutils.env.getPoolName()
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.env.getPoolName()
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
Env.GetPoolName()
```

::: zone-end

### <a name="get-cluster-id"></a>Obtención del id. del clúster

Devuelve el id. de clúster actual.

:::zone pivot = "programming-language-python"

```python
mssparkutils.env.getClusterId()
```
::: zone-end

:::zone pivot = "programming-language-scala"

```scala
mssparkutils.env.getClusterId()
```

::: zone-end

:::zone pivot = "programming-language-csharp"

```csharp
Env.GetClusterId()
```

::: zone-end

## <a name="next-steps"></a>Pasos siguientes

- [Consulte los cuadernos de ejemplo de Synapse](https://github.com/Azure-Samples/Synapse/tree/master/Notebooks)
- [Inicio rápido: Creación de un grupo de Apache Spark (versión preliminar) en Azure Synapse Analytics mediante herramientas web](../quickstart-apache-spark-notebook.md)
- [Qué es Apache Spark en Azure Synapse Analytics](apache-spark-overview.md)
- [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)