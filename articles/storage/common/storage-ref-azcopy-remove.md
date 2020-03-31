---
title: azcopy remove | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy remove.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: abce1acb88e920c0de7bbb6447ec9d838f10486c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74033996"
---
# <a name="azcopy-remove"></a>azcopy remove

Elimine blobs o archivos de una cuenta de almacenamiento de Azure.

## <a name="synopsis"></a>Sinopsis

```azcopy
azcopy remove [resourceURL] [flags]
```

## <a name="related-conceptual-articles"></a>Artículos conceptuales relacionados

- [Introducción a AzCopy](storage-use-azcopy-v10.md)
- [Transferencia de datos con AzCopy y Blob Storage](storage-use-azcopy-blobs.md)
- [Transferencia de datos con AzCopy y File Storage](storage-use-azcopy-files.md)
- [Configurar, optimizar y solucionar problemas de AzCopy](storage-use-azcopy-configure.md)

## <a name="examples"></a>Ejemplos

Elimine un solo blob con SAS:

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

Elimine un directorio virtual completo con una SAS:

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true
```

Elimine solo los blobs principales de un directorio virtual, pero no sus subdirectorios:

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --recursive=false
```

Elimine un subconjunto de blobs de un directorio virtual (por ejemplo, solo los archivos jpg y pdf, o un blob llamado "exactName"):

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true --include="*.jpg;*.pdf;exactName"
```

Elimine un directorio completo, pero con la exclusión de ciertos blobs del ámbito (por ejemplo: todos los blobs que comienzan por "foo" o terminan por "bar"):

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true --exclude="foo*;*bar"
```

Quite blobs y directorios virtuales específicos colocando sus rutas de acceso relativas (no codificadas como URL) en un archivo:

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/parent/dir]" --recursive=true --list-of-files=/usr/bar/list.txt
file content:
  dir1/dir2
  blob1
  blob2

```

Quite un solo archivo de una cuenta de Blob Storage que tenga un espacio de nombres jerárquico (no se admite incluir/excluir).

```azcopy
azcopy rm "https://[account].dfs.core.windows.net/[container]/[path/to/file]?[SAS]"
```

Quite un solo directorio de una cuenta de Blob Storage que tenga un espacio de nombres jerárquico (no se admite incluir/excluir).

```azcopy
azcopy rm "https://[account].dfs.core.windows.net/[container]/[path/to/directory]?[SAS]"
```

## <a name="options"></a>Opciones

**--exclude-path string**      Excluye estas rutas de acceso al quitar. Esta opción no permite caracteres comodín (*). Comprueba el prefijo de ruta de acceso relativa. Por ejemplo: myFolder;myFolder/subDirName/file.pdf.

**--exclude-pattern** string   Excluye los archivos en los que el nombre coincide con la lista de patrones. Por ejemplo: *.jpg;* .pdf; exactName.

**-h, --help**                     Ayuda de remove.

**--include-path** string      Incluye solo estas rutas de acceso al quitar. Esta opción no permite caracteres comodín (*). Comprueba el prefijo de ruta de acceso relativa. Por ejemplo: myFolder;myFolder/subDirName/file.pdf.

**--include-pattern** string   Incluye solo los archivos en los que el nombre coincide con la lista de patrones. Por ejemplo: *.jpg;* .pdf; exactName.

**--list-of-files** string     Define la ubicación de un archivo que contiene la lista de archivos y directorios que se van a eliminar. Las rutas de acceso relativas deben delimitarse mediante saltos de línea y las rutas de acceso NO deben estar codificadas como URL.

**--log-level** string         Define el nivel de detalle del registro para el archivo de registro. Los niveles disponibles incluyen: INFO (todas las solicitudes y respuestas), WARNING (respuestas lentas), ERROR (solo solicitudes con error) y NONE (sin registros de salida). (el valor predeterminado es "INFO') (el valor predeterminado es "INFO")

**--recursive**                Busca en los subdirectorios de forma recursiva al sincronizar entre directorios.

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

|Opción|Descripción|
|---|---|
|--cap-mbps uint32|Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.|
|--output-type string|Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es "text".|

## <a name="see-also"></a>Consulte también

- [azcopy](storage-ref-azcopy.md)
