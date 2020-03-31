---
title: azcopy jobs resume | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy jobs resume.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 3ee7879475801660b5200dddca88a0a81b2b6b9b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74034137"
---
# <a name="azcopy-jobs-resume"></a>azcopy jobs resume

Reanuda el trabajo existente con el identificador de trabajo especificado.

## <a name="synopsis"></a>Sinopsis

```azcopy
azcopy jobs resume [jobID] [flags]
```

## <a name="related-conceptual-articles"></a>Artículos conceptuales relacionados

- [Introducción a AzCopy](storage-use-azcopy-v10.md)
- [Transferencia de datos con AzCopy y Blob Storage](storage-use-azcopy-blobs.md)
- [Transferencia de datos con AzCopy y File Storage](storage-use-azcopy-files.md)
- [Configurar, optimizar y solucionar problemas de AzCopy](storage-use-azcopy-configure.md)

## <a name="options"></a>Opciones

|Opción|Descripción|
|--|--|
|--destination-sas string|SAS de destino del destino para un identificador de trabajo determinado.|
|--exclude string|Filtro: excluye estas transferencias con error al reanudar el trabajo. Los archivos deben estar separados por ";".|
|-h, --help|Muestra el contenido de la ayuda para el comando resume.|
|--include string|Filtro: solo excluye estas transferencias con error al reanudar el trabajo. Los archivos deben estar separados por ";".|
|--source-sas string |SAS de origen del origen para un identificador de trabajo determinado.|

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

|Opción|Descripción|
|---|---|
|--cap-mbps uint32|Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.|
|--output-type string|Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es "text".|

## <a name="see-also"></a>Consulte también

- [azcopy jobs](storage-ref-azcopy-jobs.md)
