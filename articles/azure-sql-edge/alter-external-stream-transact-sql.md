---
title: 'ALTER EXTERNAL STREAM (Transact-SQL): Azure SQL Edge (versión preliminar)'
description: Más información sobre la instrucción ALTER EXTERNAL STREAM en Azure SQL Edge (versión preliminar)
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 2559c4b4b875403b7c70671e27cb6222a3f1103a
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2020
ms.locfileid: "84235195"
---
# <a name="alter-external-stream-transact-sql"></a>ALTER EXTERNAL STREAM (Transact-SQL)

Modifica la definición de una transmisión externa. No se permite modificar una transmisión externa utilizada en un trabajo de streaming con el estado *En ejecución*. 



## <a name="syntax"></a>Sintaxis

```sql
  ALTER EXTERNAL STREAM external_stream_name 
  SET 
    [DATA_SOURCE] = <data_source_name> 
    , LOCATION = <location_name> 
    , EXTERNAL_FILE_FORMAT = <external_file_format_name> 
    , INPUT_OPTIONS = <input_options> 
    , OUTPUT_OPTIONS = <output_options> 
```

## <a name="arguments"></a>Argumentos

Para obtener más información sobre los argumentos del comando Alter External Stream, vea [CREATE EXTERNAL STREAM (Transact-SQL)](create-external-stream-transact-sql.md).

## <a name="return-code-values"></a>Valores de código de retorno

ALTER EXTERNAL STREAM devuelve 0 si se ejecuta correctamente. Un valor de retorno distinto de cero indica un error.


## <a name="see-also"></a>Consulte también

- [CREATE EXTERNAL STREAM (Transact-SQL)](create-external-stream-transact-sql.md) 
- [DROP EXTERNAL STREAM (Transact-SQL)](drop-external-stream-transact-sql.md) 
