---
title: azcopy copy | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy copy.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 04/10/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 0325a71fb069f3d96f05d106afac1639fc38fe42
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2020
ms.locfileid: "81253346"
---
# <a name="azcopy-copy"></a>azcopy copy

Copia los datos de origen en una ubicación de destino.

## <a name="synopsis"></a>Sinopsis

Copia los datos de origen en una ubicación de destino. Estas son las direcciones que se admiten:

  - local <-> blob de Azure (autenticación con SAS u OAuth)
  - local <-> Azure Files (autenticación de recursos compartidos o directorios con SAS)
  - local <-> ADLS Gen 2 (autenticación con SAS, OAuth o SharedKey)
  - Azure Blob (SAS o público) -> Azure Blob (autenticación con SAS u OAuth)
  - Azure Blob (SAS o público) -> Azure Files (SAS)
  - Azure Files (SAS) -> Azure Files (SAS)
  - Azure Files (SAS) -> Azure Blob (autenticación con SAS u OAuth)
  - AWS S3 (clave de acceso) -> blob en bloques de Azure (autenticación con SAS u OAuth)

Consulte los ejemplos para más información.

## <a name="related-conceptual-articles"></a>Artículos conceptuales relacionados

- [Introducción a AzCopy](storage-use-azcopy-v10.md)
- [Transferencia de datos con AzCopy y Blob Storage](storage-use-azcopy-blobs.md)
- [Transferencia de datos con AzCopy y File Storage](storage-use-azcopy-files.md)
- [Configurar, optimizar y solucionar problemas de AzCopy](storage-use-azcopy-configure.md)

## <a name="advanced"></a>Avanzado

AzCopy detecta automáticamente el tipo de contenido de los archivos cuando se cargan desde el disco local, basándose en la extensión del archivo o en el contenido (si no se especifica ninguna extensión).

La tabla de búsqueda integrada es pequeña pero, en Unix, los archivos mime.types del sistema local la pueden aumentar si están disponibles en una o varias de estas ubicaciones:

- /etc/mime.types
- /etc/apache2/mime.types
- /etc/apache/mime.types

En Windows, los tipos MIME se extraen del registro. Esta característica se puede desactivar con la ayuda de una marca. Consulte la sección sobre marcas.

Si establece una variable de entorno mediante la línea de comandos, esa variable se podrá leer en el historial de la línea de comandos. Considere la posibilidad de borrar las variables que contengan credenciales del historial de la línea de comandos. Para evitar que aparezcan variables en el historial, puede usar un script para pedir al usuario sus credenciales y establecer la variable de entorno.

```
azcopy copy [source] [destination] [flags]
```

## <a name="examples"></a>Ejemplos

Carga de un solo archivo mediante la autenticación de OAuth. Si aún no ha iniciado sesión en AzCopy, use el comando azcopy login antes de ejecutar el comando siguiente.

- azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]"

Igual que antes, pero en esta ocasión también procesa el hash MD5 del contenido del archivo y lo guarda como la propiedad Content-MD5 del blob:

- azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]" --put-md5

Carga de un solo archivo mediante un token de SAS:

- azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"

Carga de un solo archivo mediante un token de SAS y una canalización (solo blobs en bloques):
  
- cat "/path/to/file.txt" | azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"

Carga de un directorio completo mediante un token de SAS:
  
- azcopy cp "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true

or

- azcopy cp "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true --put-md5

Carga de un conjunto de archivos mediante un token de SAS y caracteres comodín (*):

- azcopy cp "/path/*foo/* bar/*.pdf" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]"

Carga de archivos y directorios mediante un token de SAS y caracteres comodín (*):

- azcopy cp "/path/*foo/* bar*" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true

Descarga de un solo archivo mediante la autenticación de OAuth. Si aún no ha iniciado sesión en AzCopy, use el comando azcopy login antes de ejecutar el comando siguiente.

- azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]" "/path/to/file.txt"

Descarga de un solo archivo mediante un token de SAS:

- azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "/path/to/file.txt"

Descarga de un solo archivo mediante un token de SAS y, luego, canalización de la salida a un archivo (solo blobs en bloques):
  
- azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" > "/path/to/file.txt"

Descarga de un directorio completo mediante un token de SAS:
  
- azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" "/path/to/dir" --recursive=true

Una nota sobre el uso de un carácter comodín (*) en direcciones URL:

Hay solo dos formas admitidas de usar un carácter comodín en una dirección URL. 

- Puede usar uno justo después de la barra diagonal final (/) de una dirección URL. De esta manera, todos los archivos de un directorio se copian directamente en el destino sin colocarlos en un subdirectorio.

- También puede usar uno en el nombre de un contenedor siempre y cuando la dirección URL solo haga referencia a un contenedor y no a un blob. Puede usar este enfoque para obtener archivos de un subconjunto de contenedores.

Descarga del contenido de un directorio sin copiar el propio directorio contenedor.

- azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/folder]/*?[SAS]" "/path/to/dir"

Descarga de una cuenta de almacenamiento completa.

- azcopy cp "https://[srcaccount].blob.core.windows.net/" "/path/to/dir" --recursive

Descarga de un subconjunto de contenedores en una cuenta de almacenamiento mediante un símbolo comodín (*) en el nombre del contenedor.

- azcopy cp "https://[srcaccount].blob.core.windows.net/[container*name]" "/path/to/dir" --recursive

Copia de un blob único en otro blob mediante un token de SAS.

- azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"

Copia de un blob único en otro blob mediante un token de SAS y un token de OAuth. Tiene que usar un token de SAS al final de la dirección URL de la cuenta de origen, pero la cuenta de destino no necesita uno si inicia sesión en AzCopy mediante el comando azcopy login. 

- azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/blob]"

Copia de un directorio virtual de blobs a otro mediante un token de SAS:

- azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true

Copia de todos los contenedores de blobs, directorios y blobs de la cuenta de almacenamiento a otra mediante un token de SAS:

- azcopy cp "https://[srcaccount].blob.core.windows.net?[SAS]" "https://[destaccount].blob.core.windows.net?[SAS]" --recursive=true

Copia de un solo objeto en Blob Storage de Amazon Web Services (AWS) S3 mediante una clave de acceso y un token de SAS. En primer lugar, establezca la variable de entorno AWS_ACCESS_KEY_ID y AWS_SECRET_ACCESS_KEY para el origen AWS S3.
  
- azcopy cp "https://s3.amazonaws.com/ [bucket]/[object]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"

Copia de un directorio completo en Blob Storage de AWS S3 mediante una clave de acceso y un token de SAS. En primer lugar, establezca la variable de entorno AWS_ACCESS_KEY_ID y AWS_SECRET_ACCESS_KEY para el origen AWS S3.

- azcopy cp "https://s3.amazonaws.com/ [bucket]/[folder]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true

Consulte https://docs.aws.amazon.com/AmazonS3/latest/user-guide/using-folders.html para comprender mejor el marcador de posición [folder].

Copia de todos los cubos en Blob Storage de Amazon Web Services (AWS) mediante el uso de una clave de acceso y un token de SAS. En primer lugar, establezca la variable de entorno AWS_ACCESS_KEY_ID y AWS_SECRET_ACCESS_KEY para el origen AWS S3.

- azcopy cp "https://s3.amazonaws.com/ " "https://[destaccount].blob.core.windows.net?[SAS]" --recursive=true

Copia de todos los cubos en Blob Storage desde una región de Amazon Web Services (AWS) mediante el uso de una clave de acceso y un token de SAS. En primer lugar, establezca la variable de entorno AWS_ACCESS_KEY_ID y AWS_SECRET_ACCESS_KEY para el origen AWS S3.

- azcopy cp "https://s3- [region].amazonaws.com/" "https://[destaccount].blob.core.windows.net?[SAS]" --recursive=true

Copia de un subconjunto de cubos mediante un símbolo comodín (*) en el nombre del cubo. Al igual que en los ejemplos anteriores, necesitará una clave de acceso y un token de SAS. Asegúrese de establecer la variable de entorno AWS_ACCESS_KEY_ID y AWS_SECRET_ACCESS_KEY para el origen AWS S3.

- azcopy cp "https://s3.amazonaws.com/ [bucket*name]/" "https://[destaccount].blob.core.windows.net?[SAS]" --recursive=true

## <a name="options"></a>Opciones

**--backup** activa los privilegios SeBackupPrivilege de Windows para cargas, o SeRestorePrivilege para descargas, para que AzCopy pueda leer todos los archivos, independientemente de los permisos del sistema de archivos, y restaurar todos los permisos. Requiere que la cuenta que ejecuta AzCopy ya tenga estos permisos (por ejemplo, que tenga derechos de administrador o que sea miembro del grupo "Operadores de copia de seguridad"). Esta marca lo único que hace es activar los privilegios que ya tiene la cuenta.

**--blob-type** string                     Define el tipo de blob en el destino. Se usa para cargar blobs y al copiar entre cuentas (el valor predeterminado es "Detect"). Los valores válidos incluyen "Detect", "BlockBlob", "PageBlob" y "AppendBlob". Al copiar entre cuentas, un valor "Detect" hace que AzCopy use el tipo de blob de origen para determinar el tipo del blob de destino. Al cargar un archivo, "Detect" determina si el archivo es un archivo VHD o VHDX en función de la extensión del archivo. Si el archivo es VHD o VHDX, AzCopy lo trata como un blob en páginas. (el valor predeterminado es "Detect")

**--block-blob-tier** string               Carga los blobs en bloques directamente en el [nivel de acceso](../blobs/storage-blob-storage-tiers.md) que prefiera. (el valor predeterminado es "None") Entre los valores válidos se incluyen "None", "Hot", "Cool" y "Archive". Si el valor es "None" o no se pasa ningún nivel, el blob heredará el nivel de la cuenta de almacenamiento.

**--block-size-mb** float                  Usa este tamaño de bloque (especificado en MiB) al cargar y descargar de Azure Storage. El valor predeterminado se calcula automáticamente en función del tamaño del archivo. Se permiten fracciones decimales (por ejemplo: 0,25).

**--cache-control** string                 Establece el encabezado cache-control. Devuelto al descargar.

**--check-length**                         Comprueba la longitud de un archivo en el destino después de la transferencia. Si hay una discrepancia entre el origen y el destino, la transferencia se marca como errónea. (El valor predeterminado es true)

**--check-md5** string                     Especifica qué tan estrictamente se deben validar los hashes MD5 al descargarse. Solo está disponible al descargar. Opciones disponibles: NoCheck, LogOnly, FailIfDifferent, FailIfDifferentOrMissing. (el valor predeterminado es "FailIfDifferent")

**--content-disposition** string           Establece el encabezado content-disposition. Devuelto al descargar.

**--content-encoding** string              Establece el encabezado content-encoding. Devuelto al descargar.

**--content-language** string              Establece el encabezado content-language. Devuelto al descargar.

**--content-type** string                  Especifica el tipo de contenido del archivo. Implica el uso de no-guess-mime-type. Devuelto al descargar.

**--decompress**                           Descomprime automáticamente los archivos al realizar la descarga, si el encabezado content-encoding indica que están comprimidos. Los valores de content-encoding son "gzip" y "deflate". Las extensiones de archivo de ".gz"/".gzip" o ".zz" no son necesarias, pero se quitarán si existen.

**--exclude-attributes** string            (solo Windows) Excluye los archivos cuyos atributos coinciden con la lista de atributos. Por ejemplo: A;S;R

**--exclude-blob-type** string             Opcionalmente, especifica el tipo de blob (BlockBlob/PageBlob/AppendBlob) que se va a excluir al copiar blobs desde el contenedor o la cuenta. El uso de esta marca no es aplicable para copiar datos desde un servicio que no es de Azure a otro que sí lo es. Si hay más de un blob deberían separarse mediante ";".

**--exclude-path** string                  Excluye estas rutas de acceso al copiar. Esta opción no permite caracteres comodín (*). Comprueba los prefijos de ruta de acceso relativa (por ejemplo: myFolder;myFolder/subDirName/file.pdf). Cuando se usa en combinación con recorrido de cuentas, las rutas de acceso no incluyen el nombre del contenedor.

**--exclude-pattern** string               Excluye estos archivos al copiar. Esta opción admite caracteres comodín (*).

**--follow-symlinks**                      Sigue los vínculos simbólicos al cargar desde el sistema de archivos local.

**--from-to** string                       Opcionalmente, especifica la combinación de destino de origen. Por ejemplo: LocalBlob, BlobLocal, LocalBlobFS.

**-h, --help**                                 Ayuda de copy.

**--include-attributes** string            (solo Windows) Incluye los archivos cuyos atributos coinciden con la lista de atributos. Por ejemplo: A;S;R

**--include-path** string                  Incluye solo estas rutas de acceso al copiar. Esta opción no permite caracteres comodín (*). Comprueba los prefijos de ruta de acceso relativa (por ejemplo: myFolder;myFolder/subDirName/file.pdf).

**--include-pattern** string               Incluye solo estos archivos al copiar. Esta opción admite caracteres comodín (*). Separe los archivos con ";".

**--log-level** string                     Define el nivel de detalle del registro para el archivo de registro. Niveles disponibles: INFO (todas las solicitudes y respuestas), WARNING (respuestas lentas), ERROR (solo solicitudes con error) y NONE (sin registros de salida). (Valor predeterminado: "INFO").

**--metadata** string                      Carga en Azure Storage con estos pares clave-valor como metadatos.

**--no-guess-mime-type**                   Impide que AzCopy detecte el tipo de contenido en función de la extensión o el contenido del archivo.

**--overwrite** string                     Sobrescribe los archivos y blobs en conflicto en el destino si esta marca está establecida en true. Los valores posibles son "true", "false", "ifSourceNewer" y "prompt". (el valor predeterminado es "true")

**--page-blob-tier** string                Carga un blob en páginas en Azure Storage con este nivel de blobs. (el valor predeterminado es "None")

**--preserve-last-modified-time**          Solo está disponible si el destino es un sistema de archivos.

Cadena **--preserve-smb-permissions**      False de manera predeterminada. Conserva las listas de control de acceso de SMB entre los recursos compatibles (Windows y Azure Files). En el caso de las descargas, también tendrá que usar la marca `--backup` para restaurar los permisos si el nuevo propietario no es el usuario que ejecuta AzCopy. Esta marca se aplica tanto a archivos como a carpetas, salvo que se especifique un filtro de que solo permita archivos (por ejemplo, `include-pattern`).

Cadena **--preserve-smb-info**             False de manera predeterminada. Conserva la información de las propiedades de SMB (hora de la última escritura, hora de creación, bits de atributo) entre los recursos compatibles con SMB (Windows y Azure Files). Solo se transferirán los bits de atributo que admite Azure Files; los restantes se omitirán. Esta marca se aplica tanto a archivos como a carpetas, salvo que se especifique un filtro de que solo permita archivos (por ejemplo, include-pattern). La información transferida a las carpetas es la misma que la transferida a los archivos, excepto la hora de la última escritura, que nunca se conserva para las carpetas.

**--preserve-owner**                       Solo tiene efecto al descargar datos, y solo cuando se usa `--preserve-smb-permissions`. Si es true (valor predeterminado), el propietario del archivo y el grupo se conservan en las descargas. Si esta marca se establece en false, `--preserve-smb-permissions` conservará las listas de control de acceso, pero el propietario y el grupo se basarán en el usuario que ejecuta AzCopy.

**--put-md5**                             Crea un hash MD5 de cada archivo y lo guarda como la propiedad Content-MD5 del blob o archivo de destino. (De forma predeterminada, NO se crea el hash). Solo está disponible al cargar.

**--recursive**                            Busca en subdirectorios de forma recursiva al cargar desde el sistema de archivos local.

**--s2s-detect-source-changed**           Comprueba si el origen ha cambiado después de la enumeración.

**--s2s-handle-invalid-metadata** string   Especifica cómo se administran las claves de metadatos no válidas. Opciones disponibles: ExcludeIfInvalid, FailIfInvalid, RenameIfInvalid. (el valor predeterminado es "ExcludeIfInvalid")

**--s2s-preserve-access-tier**             Conserva el nivel de acceso durante la copia de servicio a servicio. Consulte [Azure Blob Storage: niveles de acceso frecuente, esporádico y de archivo](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers) para asegurarse de que la cuenta de almacenamiento de destino admite la configuración del nivel de acceso. En los casos en los que no se admite el establecimiento del nivel de acceso, use s2sPreserveAccessTier=false para omitir la copia del nivel de acceso. (El valor predeterminado es true)

**--s2s-preserve-properties**              Conserva las propiedades completas durante la copia de servicio a servicio. Para el origen de archivo no único de AWS S3 y Azure Files, la operación de lista no devuelve las propiedades completas de objetos y archivos. Para conservar las propiedades completas, AzCopy debe enviar una solicitud adicional por objeto o archivo. (El valor predeterminado es true)

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

**--cap-mbps uint32**      Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.

**--output-type** string   Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es "text". (Valor predeterminado: "text").

## <a name="see-also"></a>Consulte también

- [azcopy](storage-ref-azcopy.md)
