---
title: Trabajo con cadenas en consultas de registro de Azure Monitor | Microsoft Docs
description: Describe cómo editar, comparar, buscar y realizar una amplia variedad de otras operaciones con cadenas en consultas de registro de Azure Monitor.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/16/2018
ms.openlocfilehash: a394fee7178b2e3e167c8bd905ab175b25d1d813
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75397464"
---
# <a name="work-with-strings-in-azure-monitor-log-queries"></a>Trabajo con cadenas en consultas de registro de Azure Monitor


> [!NOTE]
> Debe completar [Introducción a los análisis de registros de Azure Monitor](get-started-portal.md) y [Introducción a las consultas de registro en Azure Monitor](get-started-queries.md) antes de completar este tutorial.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

En este artículo se describe cómo editar, comparar, buscar y realizar una amplia variedad de otras operaciones con cadenas.

Cada carácter de una cadena contiene un número de índice, según su ubicación. El primer carácter está en el índice 0, el siguiente carácter en el 1 y así sucesivamente. Varias funciones de cadena usan números de índice, como se muestra en las secciones siguientes. Muchos de los ejemplos siguientes usan el comando **print** para mostrar la manipulación de cadenas sin usar un origen de datos específico.


## <a name="strings-and-escaping-them"></a>Cadenas y caracteres de escape
Los valores de cadena se encapsulan con los caracteres de comillas simples o dobles. La barra diagonal inversa (\\) se usa como carácter de escape para el carácter que le sigue, por ejemplo, \t para tabulación, \n para nueva línea y \" para el propio carácter de comillas.

```Kusto
print "this is a 'string' literal in double \" quotes"
```

```Kusto
print 'this is a "string" literal in single \' quotes'
```

Para evitar que "\\" actúe como carácter de escape, agregue "\@" como prefijo a la cadena:

```Kusto
print @"C:\backslash\not\escaped\with @ prefix"
```


## <a name="string-comparisons"></a>Comparaciones de cadenas

Operator       |Descripción                         |Distingue mayúsculas de minúsculas|Ejemplo (produce `true`)
---------------|------------------------------------|--------------|-----------------------
`==`           |Equals                              |Sí           |`"aBc" == "aBc"`
`!=`           |Not Equals                          |Sí           |`"abc" != "ABC"`
`=~`           |Equals                              |No            |`"abc" =~ "ABC"`
`!~`           |Not Equals                          |No            |`"aBc" !~ "xyz"`
`has`          |El lado derecho es un término completo en el lado izquierdo |No|`"North America" has "america"`
`!has`         |El lado derecho no es un término completo en el lado izquierdo       |No            |`"North America" !has "amer"` 
`has_cs`       |El lado derecho es un término completo en el lado izquierdo |Sí|`"North America" has_cs "America"`
`!has_cs`      |El lado derecho no es un término completo en el lado izquierdo       |Sí            |`"North America" !has_cs "amer"` 
`hasprefix`    |El lado derecho es un prefijo de término en el lado izquierdo         |No            |`"North America" hasprefix "ame"`
`!hasprefix`   |El lado derecho no es un prefijo de término en el lado izquierdo     |No            |`"North America" !hasprefix "mer"` 
`hasprefix_cs`    |El lado derecho es un prefijo de término en el lado izquierdo         |Sí            |`"North America" hasprefix_cs "Ame"`
`!hasprefix_cs`   |El lado derecho no es un prefijo de término en el lado izquierdo     |Sí            |`"North America" !hasprefix_cs "CA"` 
`hassuffix`    |El lado derecho es un sufijo de término en el lado izquierdo         |No            |`"North America" hassuffix "ica"`
`!hassuffix`   |El lado derecho no es un sufijo de término en el lado izquierdo     |No            |`"North America" !hassuffix "americ"`
`hassuffix_cs`    |El lado derecho es un sufijo de término en el lado izquierdo         |Sí            |`"North America" hassuffix_cs "ica"`
`!hassuffix_cs`   |El lado derecho no es un sufijo de término en el lado izquierdo     |Sí            |`"North America" !hassuffix_cs "icA"`
`contains`     |El lado derecho aparece como subsecuencia en el lado izquierdo  |No            |`"FabriKam" contains "BRik"`
`!contains`    |El lado derecho no aparece en el lado izquierdo           |No            |`"Fabrikam" !contains "xyz"`
`contains_cs`   |El lado derecho aparece como subsecuencia en el lado izquierdo  |Sí           |`"FabriKam" contains_cs "Kam"`
`!contains_cs`  |El lado derecho no aparece en el lado izquierdo           |Sí           |`"Fabrikam" !contains_cs "Kam"`
`startswith`   |El lado derecho es una subsecuencia inicial del lado izquierdo|No            |`"Fabrikam" startswith "fab"`
`!startswith`  |El lado derecho no es una subsecuencia inicial del lado izquierdo|No        |`"Fabrikam" !startswith "kam"`
`startswith_cs`   |El lado derecho es una subsecuencia inicial del lado izquierdo|Sí            |`"Fabrikam" startswith_cs "Fab"`
`!startswith_cs`  |El lado derecho no es una subsecuencia inicial del lado izquierdo|Sí        |`"Fabrikam" !startswith_cs "fab"`
`endswith`     |El lado derecho es una subsecuencia final del lado izquierdo|No             |`"Fabrikam" endswith "Kam"`
`!endswith`    |El lado derecho no es una subsecuencia final del lado izquierdo|No         |`"Fabrikam" !endswith "brik"`
`endswith_cs`     |El lado derecho es una subsecuencia final del lado izquierdo|Sí             |`"Fabrikam" endswith "Kam"`
`!endswith_cs`    |El lado derecho no es una subsecuencia final del lado izquierdo|Sí         |`"Fabrikam" !endswith "brik"`
`matches regex`|El lado izquierdo contiene a una coincidencia del lado derecho        |Sí           |`"Fabrikam" matches regex "b.*k"`
`in`           |Igual a uno de los elementos       |Sí           |`"abc" in ("123", "345", "abc")`
`!in`          |No es igual a uno de los elementos   |Sí           |`"bca" !in ("123", "345", "abc")`


## <a name="countof"></a>countof

Cuenta las apariciones de una subcadena en una cadena. Puede encontrar coincidencias de cadenas sin formato o usar expresiones regulares. Las coincidencias de cadena sin formato se pueden superponer, mientras que las coincidencias de expresión regular no.

### <a name="syntax"></a>Sintaxis
```
countof(text, search [, kind])
```

### <a name="arguments"></a>Argumentos:
- `text`: la cadena de entrada. 
- `search`: la cadena sin formato o la expresión regular que debe coincidir en el texto.
- `kind` - _normal_ | _regex_ (valor predeterminado: normal).

### <a name="returns"></a>Devuelve

El número de veces que la cadena de búsqueda puede coincidir en el contenedor. Las coincidencias de cadena sin formato se pueden superponer, mientras que las coincidencias de expresión regular no.

### <a name="examples"></a>Ejemplos

#### <a name="plain-string-matches"></a>Coincidencias de cadena sin formato

```Kusto
print countof("The cat sat on the mat", "at");  //result: 3
print countof("aaa", "a");  //result: 3
print countof("aaaa", "aa");  //result: 3 (not 2!)
print countof("ababa", "ab", "normal");  //result: 2
print countof("ababa", "aba");  //result: 2
```

#### <a name="regex-matches"></a>Coincidencias de expresiones regulares

```Kusto
print countof("The cat sat on the mat", @"\b.at\b", "regex");  //result: 3
print countof("ababa", "aba", "regex");  //result: 1
print countof("abcabc", "a.c", "regex");  // result: 2
```


## <a name="extract"></a>extract

Obtiene una coincidencia para una expresión regular a partir de una cadena determinada. Opcionalmente, también convierte la subcadena extraída en el tipo especificado.

### <a name="syntax"></a>Sintaxis

```Kusto
extract(regex, captureGroup, text [, typeLiteral])
```

### <a name="arguments"></a>Argumentos

- `regex`: una expresión regular.
- `captureGroup`: una constante de enteros positiva que indica el grupo de capturas que se va a extraer. 0 significa toda la coincidencia; 1, el valor que coincide con el primer "("paréntesis")" de la expresión regular; 2 o más, los posteriores paréntesis.
- `text`: la cadena que se va a buscar.
- `typeLiteral`: un literal de tipo opcional (por ejemplo, typeof(long)). Si se proporciona, la subcadena extraída se convierte a este tipo.

### <a name="returns"></a>Devuelve
La subcadena que coincide con el grupo de captura indicado, captureGroup, opcionalmente convertido a typeLiteral.
Si no hay ninguna coincidencia, o se produce un error en la conversión del tipo, devuelve null.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se extrae el último octeto de *ComputerIP* de un registro de latido:
```Kusto
Heartbeat
| where ComputerIP != "" 
| take 1
| project ComputerIP, last_octet=extract("([0-9]*$)", 1, ComputerIP) 
```

En el ejemplo siguiente se extrae el último octeto, se convierte a un tipo *real* (número) y se calcula el siguiente valor de IP
```Kusto
Heartbeat
| where ComputerIP != "" 
| take 1
| extend last_octet=extract("([0-9]*$)", 1, ComputerIP, typeof(real)) 
| extend next_ip=(last_octet+1)%255
| project ComputerIP, last_octet, next_ip
```

En el ejemplo siguiente, se busca la cadena *Trace* para una definición de "Duration". La coincidencia se convierte en *real* y se multiplica por una constante de tiempo (1 s) *que convierte la duración al tipo timespan*.
```Kusto
let Trace="A=12, B=34, Duration=567, ...";
print Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real));  //result: 567
print Duration_seconds =  extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s);  //result: 00:09:27
```


## <a name="isempty-isnotempty-notempty"></a>isempty, isnotempty, notempty

- *isempty* devuelve true si el argumento es una cadena vacía o null (vea también *isnull*).
- *isnotempty* devuelve true si el argumento no es una cadena vacía o null (vea también *isnotnull*). alias: *notempty*.

### <a name="syntax"></a>Sintaxis

```Kusto
isempty(value)
isnotempty(value)
```

### <a name="examples"></a>Ejemplos

```Kusto
print isempty("");  // result: true

print isempty("0");  // result: false

print isempty(0);  // result: false

print isempty(5);  // result: false

Heartbeat | where isnotempty(ComputerIP) | take 1  // return 1 Heartbeat record in which ComputerIP isn't empty
```


## <a name="parseurl"></a>parseurl

Divide una dirección URL en partes (protocolo, host, puerto, etc.) y devuelve un objeto de diccionario que contiene las partes como cadenas.

### <a name="syntax"></a>Sintaxis

```
parseurl(urlstring)
```

### <a name="examples"></a>Ejemplos

```Kusto
print parseurl("http://user:pass@contoso.com/icecream/buy.aspx?a=1&b=2#tag")
```

El resultado será:
```
{
    "Scheme" : "http",
    "Host" : "contoso.com",
    "Port" : "80",
    "Path" : "/icecream/buy.aspx",
    "Username" : "user",
    "Password" : "pass",
    "Query Parameters" : {"a":"1","b":"2"},
    "Fragment" : "tag"
}
```


## <a name="replace"></a>replace

Reemplaza todas las coincidencias de expresiones regulares por otra cadena. 

### <a name="syntax"></a>Sintaxis

```
replace(regex, rewrite, input_text)
```

### <a name="arguments"></a>Argumentos

- `regex`: la expresión regular con la que debe coincidir. Puede contener grupos de captura entre "("paréntesis")".
- `rewrite`: la expresión regular de sustitución para cualquier coincidencia de la expresión regular coincidente. Use \0 para hacer referencia a toda la coincidencia, \1 para el primer grupo de capturas, \2 y así sucesivamente para los grupos de capturas posteriores.
- `input_text`: la cadena de entrada que se va a buscar.

### <a name="returns"></a>Devuelve
El texto después de reemplazar todas las coincidencias de la expresión regular por evaluaciones de rewrite. Las coincidencias no se superponen.

### <a name="examples"></a>Ejemplos

```Kusto
SecurityEvent
| take 1
| project Activity 
| extend replaced = replace(@"(\d+) -", @"Activity ID \1: ", Activity) 
```

Puede tener los resultados siguientes:

Actividad                                        |reemplazado
------------------------------------------------|----------------------------------------------------------
4663: se ha intentado obtener acceso a un objeto.  |Identificador de actividad 4663: se ha intentado obtener acceso a un objeto.


## <a name="split"></a>split

Divide una cadena determinada según un delimitador especificado y devuelve una matriz de las subcadenas resultantes.

### <a name="syntax"></a>Sintaxis
```
split(source, delimiter [, requestedIndex])
```

### <a name="arguments"></a>Argumentos:

- `source`: la cadena que se va a dividir según el delimitador especificado.
- `delimiter`: el delimitador que se usará para dividir la cadena de origen.
- `requestedIndex`: un índice opcional de base cero. Si se proporciona, la matriz de cadenas devuelta contendrá solo ese elemento (si existe).


### <a name="examples"></a>Ejemplos

```Kusto
print split("aaa_bbb_ccc", "_");    // result: ["aaa","bbb","ccc"]
print split("aa_bb", "_");          // result: ["aa","bb"]
print split("aaa_bbb_ccc", "_", 1); // result: ["bbb"]
print split("", "_");               // result: [""]
print split("a__b", "_");           // result: ["a","","b"]
print split("aabbcc", "bb");        // result: ["aa","cc"]
```

## <a name="strcat"></a>strcat

Concatena los argumentos de cadena (admite de 1 a 16 argumentos).

### <a name="syntax"></a>Sintaxis
```
strcat("string1", "string2", "string3")
```

### <a name="examples"></a>Ejemplos
```Kusto
print strcat("hello", " ", "world") // result: "hello world"
```


## <a name="strlen"></a>strlen

Devuelve la longitud de una cadena.

### <a name="syntax"></a>Sintaxis
```
strlen("text_to_evaluate")
```

### <a name="examples"></a>Ejemplos
```Kusto
print strlen("hello")   // result: 5
```


## <a name="substring"></a>substring

Extrae una subcadena de una cadena de origen determinada empezando en un índice especificado. Opcionalmente, se puede especificar la longitud de la subcadena solicitada.

### <a name="syntax"></a>Sintaxis
```
substring(source, startingIndex [, length])
```

### <a name="arguments"></a>Argumentos:

- `source`: la cadena de origen de la que se tomará la subcadena.
- `startingIndex`: la posición del carácter inicial basado en cero de la subcadena solicitada.
- `length`: un parámetro opcional que puede usarse para especificar la longitud solicitada de la subcadena devuelta.

### <a name="examples"></a>Ejemplos
```Kusto
print substring("abcdefg", 1, 2);   // result: "bc"
print substring("123456", 1);       // result: "23456"
print substring("123456", 2, 2);    // result: "34"
print substring("ABCD", 0, 2);  // result: "AB"
```


## <a name="tolower-toupper"></a>tolower, toupper

Convierte una cadena determinada a todo minúsculas o todo mayúsculas.

### <a name="syntax"></a>Sintaxis
```
tolower("value")
toupper("value")
```

### <a name="examples"></a>Ejemplos
```Kusto
print tolower("HELLO"); // result: "hello"
print toupper("hello"); // result: "HELLO"
```



## <a name="next-steps"></a>Pasos siguientes
Continúe con los tutoriales avanzados:
* [Funciones de agregación](aggregations.md)
* [Agregaciones avanzadas](advanced-aggregations.md)
* [Gráficos y diagramas](charts.md)
* [Trabajar con JSON y estructuras de datos](json-data-structures.md)
* [Escritura de consultas avanzadas](advanced-query-writing.md)
* [Combinaciones: entre análisis](joins.md)
