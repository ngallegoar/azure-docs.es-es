---
title: Establecimiento de alias en Azure Cosmos DB
description: Aprenda a usar el alias en las consultas SQL de Azure Cosmos DB para diferenciar dos propiedades con el mismo nombre
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mjbrown
ms.openlocfilehash: 74849eec4c5808a584894321269c49c41f0b8a5c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74873478"
---
# <a name="aliasing-in-azure-cosmos-db"></a>Establecimiento de alias en Azure Cosmos DB

Puede establecer alias para valores de manera explícita en las consultas. Si una consulta tiene dos propiedades con el mismo nombre, establezca alias para cambiar el nombre de una o ambas propiedades de modo que se elimine su ambigüedad en el resultado proyectado.

## <a name="examples"></a>Ejemplos

La palabra clave AS usada para el alias es opcional, tal como se muestra en el ejemplo siguiente al proyectar el segundo valor como `NameInfo`:

```sql
    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo,
           { "name": f.id } NameInfo
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Los resultados son:

```json
    [{
      "AddressInfo": {
        "state": "WA",
        "city": "Seattle"
      },
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]
```

## <a name="next-steps"></a>Pasos siguientes

- [Ejemplos de .NET de Azure Cosmos DB](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Cláusula SELECT](sql-query-select.md)
- [Cláusula FROM](sql-query-from.md)
