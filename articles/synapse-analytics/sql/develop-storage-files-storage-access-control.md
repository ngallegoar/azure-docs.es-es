---
title: Control del acceso a la cuenta de almacenamiento para SQL a petición (versión preliminar)
description: Describe el modo en que SQL a petición (versión preliminar) accede a Azure Storage y cómo puede controlar el acceso al almacenamiento para SQL a petición en Azure Synapse Analytics.
services: synapse-analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 0d2683091898e9c84457b3b538776f0e6b0469d4
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81420059"
---
# <a name="control-storage-account-access-for-sql-on-demand-preview-in-azure-synapse-analytics"></a>Control del acceso a la cuenta de almacenamiento para SQL a petición (versión preliminar) en Azure Synapse Analytics

Una consulta de SQL a petición (versión preliminar) lee los archivos directamente desde Azure Storage. Como la cuenta de almacenamiento es un objeto externo al recurso de SQL a petición, son necesarias las credenciales adecuadas. Un usuario necesita que se le concedan los permisos aplicables para usar las credenciales necesarias. En este artículo se describen los tipos de credenciales que puede usar y cómo se realiza la búsqueda de credenciales para los usuarios de SQL y Azure AD.

## <a name="supported-storage-authorization-types"></a>Tipos de autorización de almacenamiento admitidos

Un usuario que haya iniciado sesión en un recurso de SQL a petición debe estar autorizado para acceder a los archivos de Azure Storage y realizar consultas en ellos. Se admiten tres tipos de autorización:

- [Firma de acceso compartido](#shared-access-signature)
- [Identidad administrada](#managed-identity)
- [Identidad de usuario](#user-identity)

> [!NOTE]
> [Paso a través de Azure AD](#force-azure-ad-pass-through) es el comportamiento predeterminado cuando se crea un área de trabajo. Si lo usa, no tendrá que crear credenciales para cada cuenta de almacenamiento a la que se acceda mediante inicios de sesión de AD. Puede [deshabilitar este comportamiento](#disable-forcing-azure-ad-pass-through).

En la tabla siguiente, encontrará los distintos tipos de autorización que se admiten o que se admitirán pronto.

| Tipo de autorización                    | *Usuario de SQL*    | *Usuario de Azure AD*     |
| ------------------------------------- | ------------- | -----------    |
| [SAS](#shared-access-signature)       | Compatible     | Compatible      |
| [Identidad administrada](#managed-identity) | No compatible | No compatible  |
| [Identidad de usuario](#user-identity)       | No compatible | Compatible      |

### <a name="shared-access-signature"></a>Firma de acceso compartido

La **Firma de acceso compartido (SAS)** ofrece acceso delegado a los recursos de una cuenta de almacenamiento. Con SAS, un cliente puede conceder a los clientes acceso a los recursos de una cuenta de almacenamiento sin compartir las claves de la cuenta. SAS le ofrece un control detallado sobre el tipo de acceso que concede a los clientes que tienen una SAS, incluido el intervalo de validez, los permisos concedidos, el intervalo de direcciones IP aceptable y el protocolo aceptable (https/http).

Para obtener un token de SAS, vaya a **Azure portal -> Cuenta de Storage -> Firma de acceso compartido -> Configurar permisos -> Generar la cadena de conexión y SAS.**

> [!IMPORTANT]
> Cuando se genera un token de SAS, este incluye un signo de interrogación ("?") al principio del token. Para usar el token en SQL a petición, debe quitar el signo de interrogación ("?") al crear una credencial. Por ejemplo:
>
> SAS token: ?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D

### <a name="user-identity"></a>Identidad del usuario

La **identidad de usuario**, también conocida como "paso a través", es un tipo de autorización en el que la identidad del usuario de Azure AD que inició sesión en SQL a petición se usa para autorizar el acceso a los datos. Antes de acceder a los datos, el administrador de Azure Storage debe conceder permisos al usuario de Azure AD. Como se indica en la tabla anterior, no se admite para el tipo de usuario de SQL.

> [!NOTE]
> Si usa [paso a través de Azure AD](#force-azure-ad-pass-through), no tendrá que crear credenciales para cada cuenta de almacenamiento a la que se acceda mediante inicios de sesión de AD.

> [!IMPORTANT]
> Debe tener un rol de Propietario, Colaborador o Lector de datos de un blob de almacenamiento para usar su identidad para acceder a los datos.
> Incluso si es propietario de una cuenta de almacenamiento, deberá agregarse a uno de los roles de datos del blob de almacenamiento.
>
> Para más información sobre el control de acceso en Azure Data Lake Store Gen2, revise el artículo [Control de acceso en Azure Data Lake Storage Gen2](../../storage/blobs/data-lake-storage-access-control.md).
>

### <a name="managed-identity"></a>Identidad administrada

La **identidad administrada** también se conoce como MSI. Es una característica de Azure Active Directory (Azure AD) que proporciona servicios de Azure para SQL a petición. Además, implementa una identidad administrada automáticamente en Azure AD. Esta identidad se puede usar para autorizar la solicitud de acceso a los datos de Azure Storage.

Antes de acceder a los datos, el administrador de Azure Storage debe conceder permisos a la identidad administrada para acceder a los datos. La concesión de permisos a la identidad administrada se realiza de la misma forma que la concesión de permisos a cualquier otro usuario de Azure AD.

## <a name="create-credentials"></a>Crear credenciales

Para consultar un archivo ubicado en Azure Storage, el punto de conexión de SQL a petición necesita una CREDENCIAL en el nivel de servidor que contenga la información de autenticación. Para agregar una credencial, ejecute [CREATE CREDENTIAL](/sql/t-sql/statements/create-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). Deberá proporcionar un argumento CREDENTIAL NAME. Debe coincidir con una parte de la ruta de acceso o con toda la ruta de acceso a los datos de almacenamiento (consulte a continuación).

> [!NOTE]
> No se admite el argumento FOR CRYPTOGRAPHIC PROVIDER.

En todos los tipos de autorización admitidos, las credenciales pueden apuntar a una cuenta, un contenedor, cualquier directorio (no raíz) o un único archivo.

CREDENTIAL NAME debe coincidir con toda la ruta de acceso completa al contenedor, carpeta o archivo, en el siguiente formato: `<prefix>://<storage_account_path>/<storage_path>`

| Origen de datos externo       | Prefijo | Ruta de acceso a la cuenta de almacenamiento                                |
| -------------------------- | ------ | --------------------------------------------------- |
| Azure Blob Storage         | https  | <cuentaDeAlmacenamiento>.blob.core.windows.net             |
| Azure Data Lake Storage Gen1 | https  | <cuentaDeAlmacenamiento>.azuredatalakestore.net/webhdfs/v1 |
| Azure Data Lake Storage Gen2 | https  | <cuentaDeAlmacenamiento>.dfs.core.windows.net              |

 "<rutaDeAlmacenamiento>" es una ruta de acceso en el almacenamiento que apunta a la carpeta o archivo que desea leer.

> [!NOTE]
> Hay un argumento CREDENTIAL NAME `UserIdentity` especial que [fuerza el paso a través de Azure AD](#force-azure-ad-pass-through). Lea sobre el efecto que tiene en la [búsqueda de credenciales](#credential-lookup) mientras ejecuta las consultas.

Opcionalmente, para permitir a un usuario crear o anular una credencial, el administrador puede CONCEDER O DENEGAR un permiso para MODIFICAR CUALQUIER CREDENCIAL a un usuario:

```sql
GRANT ALTER ANY CREDENTIAL TO [user_name];
```

### <a name="supported-storages-and-authorization-types"></a>Tipos de almacenamiento y autorización admitidos

Puede usar las siguientes combinaciones de tipos de autorización y almacenamiento de Azure:

|                     | Blob Storage   | ADLS Gen1        | ADLS Gen2     |
| ------------------- | ------------   | --------------   | -----------   |
| *SAS*               | Compatible      | No compatible   | Compatible     |
| *Identidad administrada* | No compatible  | No compatible    | No compatible |
| *Identidad del usuario*    | Compatible      | Compatible        | Compatible     |

### <a name="examples"></a>Ejemplos

Según el [tipo de autorización](#supported-storage-authorization-types), puede crear credenciales mediante la sintaxis de T-SQL siguiente.

**Firma de acceso compartido y Blob Storage**

Reemplace <*mystorageaccountname*> por el nombre de la cuenta de almacenamiento real y <*mystorageaccountcontainername*> por el nombre real del contenedor:

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
WITH IDENTITY='SHARED ACCESS SIGNATURE'
, SECRET = 'sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D';
GO
```

**Identidad del usuario y Azure Data Lake Storage Gen1**

Reemplace <*mystorageaccountname*> por el nombre de la cuenta de almacenamiento real y <*mystorageaccountcontainername*> por el nombre real del contenedor:

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.azuredatalakestore.net/webhdfs/v1/<mystorageaccountcontainername>]
WITH IDENTITY='User Identity';
GO
```

**Identidad del usuario y Azure Data Lake Storage Gen2**

Reemplace <*mystorageaccountname*> por el nombre de la cuenta de almacenamiento real y <*mystorageaccountcontainername*> por el nombre real del contenedor:

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.dfs.core.windows.net/<mystorageaccountcontainername>]
WITH IDENTITY='User Identity';
GO
```

## <a name="force-azure-ad-pass-through"></a>Forzar paso a través de Azure AD

Forzar un paso a través de Azure AD es un comportamiento predeterminado que se logra mediante un nombre de credencial especial, `UserIdentity`, que se crea automáticamente durante el aprovisionamiento del área de trabajo de Azure Synapse. Este fuerza el uso de un paso a través de Azure AD para cada consulta de cada inicio de sesión de Azure AD, el cual se producirá a pesar de la existencia de otras credenciales.

> [!NOTE]
> El paso a través de Azure AD es un comportamiento predeterminado. No tiene que crear credenciales para cada cuenta de almacenamiento a la que se acceda mediante inicios de sesión de AD.

En caso de que haya [deshabilitado la opción para forzar el paso a través de Azure AD para cada consulta](#disable-forcing-azure-ad-pass-through) y desee habilitarla de nuevo, ejecute:

```sql
CREATE CREDENTIAL [UserIdentity]
WITH IDENTITY = 'User Identity';
```

Para habilitar la opción para forzar un paso a través de Azure AD para un usuario específico, puede conceder el permiso REFERENCE sobre el valor `UserIdentity` de la credencial a ese usuario determinado. En el ejemplo siguiente se habilita la opción para forzar un paso a través de Azure AD para un nombre de usuario:

```sql
GRANT REFERENCES ON CREDENTIAL::[UserIdentity] TO USER [user_name];
```

Para más información sobre cómo encuentra SQL a petición las credenciales a usar, consulte [búsqueda de credenciales](#credential-lookup).

## <a name="disable-forcing-azure-ad-pass-through"></a>Deshabilitar la opción para forzar un paso a través de Azure

Puede deshabilitar la opción para [forzar un paso a través de Azure AD para cada consulta](#force-azure-ad-pass-through). Para deshabilitarlo, quite el valor `Userdentity` de la credencial mediante:

```sql
DROP CREDENTIAL [UserIdentity];
```

Si desea volver a habilitarla, consulte la sección [Forzar paso a través de Azure AD](#force-azure-ad-pass-through).

Para deshabilitar la opción para forzar un paso a través de Azure AD para un usuario específico, puede denegar el permiso REFERENCE sobre el valor `UserIdentity` de la credencial a ese usuario determinado. En el ejemplo siguiente se deshabilita la opción para forzar un paso a través de Azure AD para un nombre de usuario:

```sql
DENY REFERENCES ON CREDENTIAL::[UserIdentity] TO USER [user_name];
```

Para más información sobre cómo encuentra SQL a petición las credenciales a usar, consulte [búsqueda de credenciales](#credential-lookup).

## <a name="grant-permissions-to-use-credential"></a>Concesión de permisos para usar credenciales

Para usar las credenciales, un usuario debe tener el permiso REFERENCES sobre una credencial específica. Para conceder un permiso REFERENCES sobre una credencial de almacenamiento para un usuario específico, ejecute:

```sql
GRANT REFERENCES ON CREDENTIAL::[storage_credential] TO [specific_user];
```

Para garantizar una experiencia de paso a través de Azure AD sin interrupciones, todos los usuarios tendrán, de forma predeterminada, derecho a usar la credencial de `UserIdentity`. Esto se logra mediante una ejecución automática de la siguiente instrucción en el aprovisionamiento del área de trabajo de Azure Synapse:

```sql
GRANT REFERENCES ON CREDENTIAL::[UserIdentity] TO [public];
```

## <a name="credential-lookup"></a>Búsqueda de credenciales

Al autorizar consultas, se usa la búsqueda de credenciales para acceder a una cuenta de almacenamiento y se basa en las siguientes reglas:

- El usuario ha iniciado sesión con un inicio de sesión de Azure AD

  - Si existe una credencial de UserIdentity y el usuario tiene permisos de referencia sobre ella, se usará el paso a través de Azure AD, en caso contrario se usará la [credencial de búsqueda por ruta de acceso](#lookup-credential-by-path).

- El usuario ha iniciado sesión con un inicio de sesión de SQL

  - Use la [credencial de búsqueda por ruta de acceso](#lookup-credential-by-path)

### <a name="lookup-credential-by-path"></a>Credencial de búsqueda por ruta de acceso

Si la opción para forzar el paso a través de Azure AD está deshabilitada, la búsqueda de credenciales se basará en la ruta de acceso del almacenamiento (primero en la profundidad) y en la existencia de un permiso REFERENCES para esa credencial concreta. Cuando hay varias credenciales que se pueden usar para acceder al mismo archivo, SQL a petición usará la más específica.  

A continuación se muestra un ejemplo de una consulta en la siguiente ruta de acceso de archivo: *account.dfs.core.windows.net/filesystem/folder1/.../folderN/fileX.ext*

La búsqueda de credenciales se completará en este orden:

```
account.dfs.core.windows.net/filesystem/folder1/.../folderN/fileX
account.dfs.core.windows.net/filesystem/folder1/.../folderN
account.dfs.core.windows.net/filesystem/folder1
account.dfs.core.windows.net/filesystem
account.dfs.core.windows.net
```

Si un usuario no tiene el permiso REFERENCES sobre la credencial número 5, SQL a petición comprobará que el usuario tenga dicho permiso sobre una credencial de un nivel superior hasta que encuentre las credenciales para las que el usuario tiene ese permiso. Si no se encuentra este permiso, se devolverá un mensaje de error.

### <a name="credential-and-path-level"></a>Nivel de credencial y de ruta de acceso

Según la forma de la ruta de acceso que desee, los siguientes requisitos estarán en vigor para ejecutar consultas:

- Si la consulta está dirigida a varios archivos (carpetas, con o sin caracteres comodín), el usuario tiene que acceder al menos a una credencial en el nivel del directorio raíz (nivel de contenedor). Este nivel de acceso es necesario, ya que los archivos de listas están relacionados con el directorio raíz (limitaciones de Azure Storage)
- Si la consulta está dirigida a un único archivo, el usuario necesita tener acceso a una credencial de cualquier nivel, ya que SQL a petición accede al archivo directamente, es decir, sin mostrar las carpetas.

|                  | *Cuenta* | *Directorio raíz* | *Cualquier otro directorio* | *Archivo*        |
| ---------------- | --------- | ---------------- | --------------------- | ------------- |
| *Un solo archivo*    | Compatible | Compatible        | Compatible             | Compatible     |
| *Varios archivos* | Compatible | Compatible        | No compatible         | No compatible |

## <a name="next-steps"></a>Pasos siguientes

Los artículos que se enumeran a continuación le ayudarán a aprender a consultar diferentes tipos de carpeta y de archivo, y a crear y usar vistas:

- [Consulta de archivos .csv](query-single-csv-file.md)
- [Consulta de carpetas y varios archivos .csv](query-folders-multiple-csv-files.md)
- [Consulta de archivos específicos](query-specific-files.md)
- [Consulta de archivos Parquet](query-parquet-files.md)
- [Creación y uso de vistas](create-use-views.md)
- [Consulta de archivos JSON](query-json-files.md)
- [Consulta de tipos anidados de Parquet](query-parquet-nested-types.md)
