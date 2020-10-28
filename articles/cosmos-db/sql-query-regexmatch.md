---
title: RegexMatch en el lenguaje de consulta de Azure Cosmos DB
description: Aprenda sobre la función del sistema de SQL RegexMatch en Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/13/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: 620ffcb379f9fa1402e422c13bcadf8da4047409
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92283333"
---
# <a name="regexmatch-azure-cosmos-db"></a>REGEXMATCH (Azure Cosmos DB)

Proporciona funcionalidades de expresiones regulares. Las expresiones regulares son una notación concisa y flexible para buscar patrones de texto. Azure Cosmos DB usa [expresiones regulares compatibles con PERL (PCRE)](http://www.pcre.org/). 

## <a name="syntax"></a>Sintaxis
  
```sql
RegexMatch(<str_expr1>, <str_expr2>, [, <str_expr3>])  
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr1*  
   La expresión de cadena en la que se va a buscar.  
  
*str_expr2*  
   Es la expresión regular.

*str_expr3*  
   Es la cadena de modificadores seleccionados que se va a usar con la expresión regular. Este valor de cadena es opcional. Si desea ejecutar RegexMatch sin modificadores, puede agregar una cadena vacía u omitirla por completo. 

Para más información, consulte la [sintaxis para crear expresiones regulares en Perl](https://perldoc.perl.org/perlre). 

Azure Cosmos DB admite los siguientes cuatro modificadores:

| Modificador | Descripción |
| ------ | ----------- |
| `m` | Trate la expresión de cadena en la que se va a buscar como varias líneas. Sin esta opción, "^" y "$" coincidirán al principio o al final de la cadena y no en cada línea individual. |
| `s` | Permita que "." coincida con cualquier carácter, incluido un carácter de nueva línea. | 
| `i` | Ignora las mayúsculas y minúsculas si se produce una coincidencia de patrones. |
| `x` | Ignora todos los caracteres de espacio en blanco. |

## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión condicional. Devuelve "undefined" si la expresión de cadena que se va a buscar, la expresión regular o los modificadores seleccionados no son válidos.
  
## <a name="examples"></a>Ejemplos
  
En el siguiente ejemplo de RegexMatch simple se comprueba la cadena "abcd" para buscar coincidencias de expresiones regulares mediante algunos modificadores diferentes.
  
```sql
SELECT RegexMatch ("abcd", "ABC", "") AS NoModifiers, 
RegexMatch ("abcd", "ABC", "i") AS CaseInsensitive, 
RegexMatch ("abcd", "ab.", "") AS WildcardCharacter,
RegexMatch ("abcd", "ab c", "x") AS IgnoreWhiteSpace, 
RegexMatch ("abcd", "aB c", "ix") AS CaseInsensitiveAndIgnoreWhiteSpace 
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[
    {
        "NoModifiers": false,
        "CaseInsensitive": true,
        "WildcardCharacter": true,
        "IgnoreWhiteSpace": true,
        "CaseInsensitiveAndIgnoreWhiteSpace": true
    }
]
```

Con RegexMatch, puede utilizar metacaracteres para realizar búsquedas de cadenas más complejas que no serían posibles de otra manera con las funciones del sistema StartsWith, EndsWith, Contains o StringEquals. A continuación se muestran algunos ejemplos adicionales que puede ejecutar mediante el conjunto de datos de nutrición disponible en el [Sitio de prueba de consultas de Azure Cosmos DB](https://www.documentdb.com/sql/demo). 

> [!NOTE] 
> Si tiene que usar un metacarácter en una expresión regular y no desea que tenga un significado especial, debe usar el metacarácter como carácter de escape mediante `\`.

**Compruebe los elementos que tengan una descripción que contenga la palabra "sal" exactamente una vez:**

```sql
SELECT * 
FROM c 
WHERE RegexMatch (c.description, "salt{1}","")
```

**Compruebe los elementos que tengan una descripción que contenga un número comprendido entre 0 y 99:**

```sql
SELECT * 
FROM c 
WHERE RegexMatch (c.description, "[0-99]","")
```

**Compruebe los elementos que tengan una descripción que contenga cuatro palabras que empiecen por "S" o "s":**

```sql
SELECT * 
FROM c 
WHERE RegexMatch (c.description, " s... ","i")
```

## <a name="remarks"></a>Observaciones

Esta función del sistema se beneficiará de un [índice de intervalo](index-policy.md#includeexclude-strategy) si la expresión regular se puede dividir en las funciones del sistema StartsWith, EndsWith, Contains o StringEquals.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de cadena Azure Cosmos DB](sql-query-string-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
