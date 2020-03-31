---
title: azcopy jobs clean | Microsoft Docs
description: En este artículo se proporciona información de referencia sobre el comando azcopy jobs clean.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 7ae14c3606dfe6bffa8481682843f3f2e85c2131
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74033717"
---
# <a name="azcopy-jobs-clean"></a>azcopy jobs clean

Eliminación de todos los archivos de registro y plan de todos los trabajos

```
azcopy jobs clean [flags]
```

## <a name="related-conceptual-articles"></a>Artículos conceptuales relacionados

- [Introducción a AzCopy](storage-use-azcopy-v10.md)
- [Transferencia de datos con AzCopy y Blob Storage](storage-use-azcopy-blobs.md)
- [Transferencia de datos con AzCopy y File Storage](storage-use-azcopy-files.md)
- [Configurar, optimizar y solucionar problemas de AzCopy](storage-use-azcopy-configure.md)

## <a name="examples"></a>Ejemplos

```
  azcopy jobs clean --with-status=completed
```

## <a name="options"></a>Opciones

**-h, --help**                Ayuda de clean.

**--with-status** string   Solo quita los trabajos con este estado. Valores disponibles: Canceled, Completed, Failed, InProgress, All (el valor predeterminado es "All")

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

**--cap-mbps uint32**      Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.

**--output-type** string   Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es "text". (Valor predeterminado: "text").

## <a name="see-also"></a>Consulte también

- [azcopy jobs](storage-ref-azcopy-jobs.md)
