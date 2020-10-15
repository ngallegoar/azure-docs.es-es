---
title: StringToNumber en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre la función del sistema SQL StringToNumber en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 5ca8d0c4a6d244823dda6f0f79a3cf5c743a12a9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "78296429"
---
# <a name="stringtonumber-azure-cosmos-db"></a>StringToNumber (Azure Cosmos DB)
 Devuelve la expresión traducida a un número. Si no se puede traducir la expresión, devuelve undefined.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
StringToNumber(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr*  
   Es una expresión de cadena que se va a analizar como una expresión de número JSON. Los números en JSON deben ser un número entero o de punto flotante. Para más información sobre el formato JSON, consulte [json.org](https://json.org/)  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión de número o undefined.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se muestra cómo se comporta `StringToNumber` en tipos diferentes. 

Se admiten espacios en blanco únicamente antes o después del número.

```sql
SELECT 
    StringToNumber("1.000000") AS num1, 
    StringToNumber("3.14") AS num2,
    StringToNumber("   60   ") AS num3, 
    StringToNumber("-1.79769e+308") AS num4
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
{{"num1": 1, "num2": 3.14, "num3": 60, "num4": -1.79769e+308}}
```  

En JSON, un número válido debe ser un número entero o de punto flotante.

```sql
SELECT   
    StringToNumber("0xF")
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
{{}}
```  

La expresión pasada se analizará como una expresión de número; estas entradas no se evalúan para el tipo número y, por tanto, se devuelven undefined. 

```sql
SELECT 
    StringToNumber("99     54"),   
    StringToNumber(undefined),
    StringToNumber("false"),
    StringToNumber(false),
    StringToNumber(" "),
    StringToNumber(NaN)
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
{{}}
```  

## <a name="remarks"></a>Observaciones

Esta función del sistema no usará el índice.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de cadena Azure Cosmos DB](sql-query-string-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
