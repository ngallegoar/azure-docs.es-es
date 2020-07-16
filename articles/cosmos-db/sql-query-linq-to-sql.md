---
title: Traducción de LINQ a SQL en Azure Cosmos DB
description: Obtenga información sobre los operadores de LINQ admitidos y cómo se asignan las consultas LINQ a consultas SQL en Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: tisande
ms.openlocfilehash: 3f8753518e1d54ddba4fc15a5a030308d0c112a1
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86042499"
---
# <a name="linq-to-sql-translation"></a>LINQ para traducción de lenguaje SQL

El proveedor de consulta de Azure Cosmos DB realiza una mejor opción de asignación desde una consulta de LINQ a una consulta SQL de Cosmos DB. Si quiere obtener la consulta SQL que se traduce a LINQ, use el método `ToString()` en el objeto `IQueryable` generado. En la descripción siguiente se da por supuesto un conocimiento básico de LINQ.

El sistema de tipos del proveedor de consultas admite solo los tipos primitivos de JSON: numérico, booleano, cadena y null.

El proveedor de consultas admite las siguientes expresiones escalares:

- Valores constantes, incluidos los de los tipos de datos primitivos durante la evaluación de la consulta.
  
- Expresiones de índice de propiedad o matriz que hacen referencia a la propiedad de un objeto o a un elemento de matriz. Por ejemplo:
  
  ```
    family.Id;
    family.children[0].familyName;
    family.children[0].grade;
    family.children[n].grade; //n is an int variable
  ```
  
- Expresiones aritméticas, incluidas las expresiones aritméticas comunes con valores numéricos y booleanos. Para obtener una lista completa, consulte la [Introducción a las consultas SQL](https://go.microsoft.com/fwlink/p/?LinkID=510612).
  
  ```
    2 * family.children[0].grade;
    x + y;
  ```
  
- Expresiones de comparación de cadenas, que incluyen la comparación de un valor de cadena con algún valor de cadena constante.  
  
  ```
    mother.familyName == "Wakefield";
    child.givenName == s; //s is a string variable
  ```
  
- Expresiones de creación de objetos o matrices, que devuelven un objeto de tipo de valor compuesto o tipo anónimo, o una matriz de estos objetos. Puede anidar estos valores.
  
  ```
    new Parent { familyName = "Wakefield", givenName = "Robin" };
    new { first = 1, second = 2 }; //an anonymous type with two fields  
    new int[] { 3, child.grade, 5 };
  ```

## <a name="supported-linq-operators"></a><a id="SupportedLinqOperators"></a>Operadores de LINQ admitidos

El proveedor LINQ incluido con el SDK de .NET para SQL admite los operadores siguientes:

- **Select**: Las proyecciones se traducen a la instrucción SQL SELECT, incluida la construcción de objetos.
- **Where**: Los filtros se traducen a la instrucción SQL WHERE y admiten la traducción entre `&&`, `||` y `!` a los operadores SQL.
- **SelectMany**: Permite desenredar las matrices a la cláusula SQL JOIN. Úselo para encadenar o anidar expresiones para filtrar los elementos de la matriz.
- **OrderBy** y **OrderByDescending**: Se traducen a ORDER BY con ASC o DESC.
- Los operadores **Count**, **Sum**, **Min**, **Max** y **Average** para la agregación y sus equivalentes asincrónicos **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync** y **AverageAsync**.
- **CompareTo**: Se traduce a las comparaciones de intervalos. Se usa frecuentemente para las cadenas, debido a que no es comparable en .NET.
- **Skip** y **Take**: se traducen en OFFSET y LIMIT de SQL para limitar los resultados de una consulta y realizar la paginación.
- **Funciones matemáticas**: Admite la traducción desde .NET `Abs`, `Acos`, `Asin`, `Atan`, `Ceiling`, `Cos`, `Exp`, `Floor`, `Log`, `Log10`, `Pow`, `Round`, `Sign`, `Sin`, `Sqrt`, `Tan` y `Truncate` a las funciones integradas equivalentes de SQL.
- **Funciones de cadena**: Admite la traducción desde .NET `Concat`, `Contains`, `Count`, `EndsWith`, `IndexOf`, `Replace`, `Reverse`, `StartsWith`, `SubString`, `ToLower`, `ToUpper`, `TrimEnd` y `TrimStart` a las funciones integradas equivalentes de SQL.
- **Funciones de matriz**: Admite la traducción desde .NET `Concat`, `Contains` y `Count` a las funciones integradas equivalentes de SQL.
- **Funciones de extensión geoespacial**: Admite la traducción desde los métodos de código auxiliar `Distance`, `IsValid`, `IsValidDetailed` y `Within` a las funciones integradas equivalentes de SQL.
- **Función de extensión de función definida por el usuario**: Admite la traducción desde el método de código auxiliar `UserDefinedFunctionProvider.Invoke` a la correspondiente función definida por el usuario.
- **Varios**: Admite la traducción de los operadores `Coalesce` y condicional. Puede traducir `Contains` a String CONTAINS, ARRAY_CONTAINS o SQL IN, según el contexto.

## <a name="examples"></a>Ejemplos

Los ejemplos siguientes ilustran la traducción de algunos de los operadores de consulta de LINQ estándar a consultas de Cosmos DB.

### <a name="select-operator"></a>Operador Select

La sintaxis es `input.Select(x => f(x))`, donde `f` es una expresión escalar.

**Operador Select, ejemplo 1:**

- **Expresión lambda de LINQ**
  
  ```csharp
      input.Select(family => family.parents[0].familyName);
  ```
  
- **SQL**
  
  ```sql
      SELECT VALUE f.parents[0].familyName
      FROM Families f
    ```
  
**Operador Select, ejemplo 2:** 

- **Expresión lambda de LINQ**
  
  ```csharp
      input.Select(family => family.children[0].grade + c); // c is an int variable
  ```
  
- **SQL**
  
  ```sql
      SELECT VALUE f.children[0].grade + c
      FROM Families f
  ```
  
**Operador Select, ejemplo 3:**

- **Expresión lambda de LINQ**
  
  ```csharp
    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });
  ```
  
- **SQL** 
  
  ```sql
      SELECT VALUE {"name":f.children[0].familyName,
                    "grade": f.children[0].grade + 3 }
      FROM Families f
  ```

### <a name="selectmany-operator"></a>Operador SelectMany

La sintaxis es `input.SelectMany(x => f(x))`, donde `f` es una expresión escalar que devuelve un tipo de contenedor.

- **Expresión lambda de LINQ**
  
  ```csharp
      input.SelectMany(family => family.children);
  ```
  
- **SQL**

  ```sql
      SELECT VALUE child
      FROM child IN Families.children
  ```

### <a name="where-operator"></a>Operador Where

La sintaxis es `input.Where(x => f(x))`, donde `f` es una expresión escalar que devuelve un valor booleano.

**Operador Where, ejemplo 1:**

- **Expresión lambda de LINQ**
  
  ```csharp
      input.Where(family=> family.parents[0].familyName == "Wakefield");
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE f.parents[0].familyName = "Wakefield"
  ```
  
**Operador Where, ejemplo 2:**

- **Expresión lambda de LINQ**
  
  ```csharp
      input.Where(
          family => family.parents[0].familyName == "Wakefield" &&
          family.children[0].grade < 3);
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE f.parents[0].familyName = "Wakefield"
      AND f.children[0].grade < 3
  ```

## <a name="composite-sql-queries"></a>Composición de consultas SQL

Puede componer los operadores anteriores para formar consultas más eficaces. Dado que Cosmos DB admite contenedores anidados, puede concatenar o anidar la composición.

### <a name="concatenation"></a>Concatenación

La sintaxis es `input(.|.SelectMany())(.Select()|.Where())*`. Una consulta concatenada puede empezar por una consulta `SelectMany` opcional, seguida de varios operadores `Select` o `Where`.

**Concatenación, ejemplo 1:**

- **Expresión lambda de LINQ**
  
  ```csharp
      input.Select(family => family.parents[0])
          .Where(parent => parent.familyName == "Wakefield");
  ```

- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE f.parents[0].familyName = "Wakefield"
  ```

**Concatenación, ejemplo 2:**

- **Expresión lambda de LINQ**
  
  ```csharp
      input.Where(family => family.children[0].grade > 3)
          .Select(family => family.parents[0].familyName);
  ```

- **SQL**
  
  ```sql
      SELECT VALUE f.parents[0].familyName
      FROM Families f
      WHERE f.children[0].grade > 3
  ```

**Concatenación, ejemplo 3:**

- **Expresión lambda de LINQ**
  
  ```csharp
      input.Select(family => new { grade=family.children[0].grade}).
          Where(anon=> anon.grade < 3);
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE ({grade: f.children[0].grade}.grade > 3)
  ```

**Concatenación, ejemplo 4:**

- **Expresión lambda de LINQ**
  
  ```csharp
      input.SelectMany(family => family.parents)
          .Where(parent => parents.familyName == "Wakefield");
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM p IN Families.parents
      WHERE p.familyName = "Wakefield"
  ```

### <a name="nesting"></a>Anidamiento

La sintaxis es `input.SelectMany(x=>x.Q())`, donde `Q` es un operador `Select`, `SelectMany` o `Where`.

Una consulta anidada aplica la consulta interna a cada elemento del contenedor externo. Una característica importante es que la consulta interna puede hacer referencia a los campos de los elementos del contenedor externo, como una autocombinación.

**Anidamiento, ejemplo 1:**

- **Expresión lambda de LINQ**
  
  ```csharp
      input.SelectMany(family=>
          family.parents.Select(p => p.familyName));
  ```

- **SQL**
  
  ```sql
      SELECT VALUE p.familyName
      FROM Families f
      JOIN p IN f.parents
  ```

**Anidamiento, ejemplo 2:**

- **Expresión lambda de LINQ**
  
  ```csharp
      input.SelectMany(family =>
          family.children.Where(child => child.familyName == "Jeff"));
  ```

- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      JOIN c IN f.children
      WHERE c.familyName = "Jeff"
  ```

**Anidamiento, ejemplo 3:**

- **Expresión lambda de LINQ**
  
  ```csharp
      input.SelectMany(family => family.children.Where(
          child => child.familyName == family.parents[0].familyName));
  ```

- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      JOIN c IN f.children
      WHERE c.familyName = f.parents[0].familyName
  ```


## <a name="next-steps"></a>Pasos siguientes

- [Ejemplos de .NET de Azure Cosmos DB](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Modelado de datos de documentos](modeling-data.md)
