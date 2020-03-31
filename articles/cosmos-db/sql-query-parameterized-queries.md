---
title: Consultas parametrizadas en Azure Cosmos DB
description: Aprenda el modo en que las consultas de SQL con parámetros permiten controlar y evitar de forma coherente la entrada por parte de los usuarios, e impiden la exposición accidental de datos mediante la inyección de código SQL.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: tisande
ms.openlocfilehash: e15a8236723c1efd80f27f2d253e9bbc44af4b0b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74870826"
---
# <a name="parameterized-queries-in-azure-cosmos-db"></a>Consultas parametrizadas en Azure Cosmos DB

Cosmos DB admite consultas con parámetros que se expresen con la notación @ ya conocida. El uso de SQL con parámetros permite controlar y evitar de forma coherente la entrada por parte de los usuarios, e impide la exposición accidental de datos a través de la inyección de código SQL.

## <a name="examples"></a>Ejemplos

Por ejemplo, puede escribir una consulta que tome `lastName` y `address.state` como parámetros, y ejecutarla para distintos valores de `lastName` y `address.state` basándose en la entrada del usuario.

```sql
    SELECT *
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState
```

Luego puede enviar esta solicitud a Cosmos DB como una consulta JSON parametrizada, similar a la siguiente:

```sql
    {
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",
        "parameters": [
            {"name": "@lastName", "value": "Wakefield"},
            {"name": "@addressState", "value": "NY"},
        ]
    }
```

En el ejemplo siguiente se establece el argumento TOP con una consulta parametrizada: 

```sql
    {
        "query": "SELECT TOP @n * FROM Families",
        "parameters": [
            {"name": "@n", "value": 10},
        ]
    }
```

Los valores de los parámetros pueden ser cualquier tipo de JSON válido: cadenas, números, booleanos, null o incluso matrices o JSON anidado. Dado que Cosmos DB no tiene ningún esquema, los parámetros no se validan con respecto a ningún tipo.


## <a name="next-steps"></a>Pasos siguientes

- [Ejemplos de .NET de Azure Cosmos DB](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Modelado de datos de documentos](modeling-data.md)
