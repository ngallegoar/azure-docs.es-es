---
title: Preguntas más frecuentes
titleSuffix: Azure Cognitive Search
description: Obtenga respuestas a preguntas comunes sobre el servicio Microsoft Azure Cognitive Search, un servicio de búsqueda hospedado en la nube en Microsoft Azure.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: fee74cb6ec5acd5fa0f171eab9769a833f04ad66
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "72792910"
---
# <a name="azure-cognitive-search---frequently-asked-questions-faq"></a>Azure Cognitive Search: preguntas más frecuentes (P+F)

 Encuentre respuestas a preguntas habituales sobre conceptos, código y escenarios relacionados con Azure Cognitive Search.

## <a name="platform"></a>Plataforma

### <a name="how-is-azure-cognitive-search-different-from-full-text-search-in-my-dbms"></a>¿En qué se diferencia Azure Cognitive Search de la búsqueda de texto completo de mi DBMS?

Azure Cognitive Search admite varios orígenes de datos, el [análisis lingüístico en muchos lenguajes](https://docs.microsoft.com/rest/api/searchservice/language-support), [análisis personalizados para entradas de datos interesantes e inusuales](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search), controles del rango de búsqueda a través de [perfiles de puntuación](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) y características de la experiencia del usuario como la escritura anticipada, el resaltado de referencias y la navegación por facetas. También incluye otras características, como los sinónimos y la sintaxis de consulta completa, pero no suelen ser relevantes.

### <a name="what-is-the-difference-between-azure-cognitive-search-and-elasticsearch"></a>¿Cuál es la diferencia entre Azure Cognitive Search y Elasticsearch?

Cuando se comparan las tecnologías de búsqueda, los clientes a menudo preguntan información específica sobre Azure Cognitive Search y Elasticsearch. Los clientes que eligen Azure Cognitive Search en lugar de Elasticsearch para sus proyectos de aplicaciones de búsqueda suelen hacerlo porque hemos conseguido facilitar una tarea clave o porque necesitan integración incorporada con otras tecnologías de Microsoft:

+ Azure Cognitive Search es un servicio en la nube completamente administrado con acuerdos de nivel de servicio (SLA) del 99,9 % cuando se aprovisiona con suficiente redundancia (dos réplicas para el acceso de lectura y tres para lectura y escritura).
+ Los [procesadores de lenguaje Natural](https://docs.microsoft.com/rest/api/searchservice/language-support) de Microsoft ofrecen un análisis lingüístico de vanguardia.  
+ Los [indexadores de Azure Cognitive Search](search-indexer-overview.md) pueden rastrear diversos orígenes de datos de Azure para llevar a cabo la indexación inicial e incremental.
+ Si necesita una respuesta rápida a las fluctuaciones de volúmenes de indexación o de consultas, puede usar [controles deslizantes](search-manage.md#scale-up-or-down) en Azure Portal o ejecutar un [script de PowerShell](search-manage-powershell.md), omitiendo la administración de particiones directamente.  
+ Las [características de optimización y puntuación](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) proporcionan medios para influir en las clasificaciones de las búsquedas más allá de lo que puede proporcionar únicamente el motor de búsqueda.

### <a name="can-i-pause-azure-cognitive-search-service-and-stop-billing"></a>¿Se puede pausar el servicio Azure Cognitive Search y dejar de facturar?

El servicio no se puede pausar. Cuando se crea el servicio, se asignan los recursos de proceso y almacenamiento para su uso exclusivo. No es posible liberar y recuperar esos recursos a petición.

## <a name="indexing-operations"></a>Operaciones de indexación

### <a name="move-backup-and-restore-indexes-or-index-snapshots"></a>¿Índices de copia de seguridad, restauración y movimiento o instantáneas de índices?

Durante la fase de desarrollo, es posible que quiera mover el índice entre los servicios de búsqueda. Por ejemplo, puede usar un plan de tarifa Básico o Gratis para desarrollar el índice y, después, puede que quiera moverlo al nivel Estándar o a uno más alto para usar en producción. 

O puede que quiera realizar una copia de seguridad de una instantánea de índice que se pueda usar para restaurarla más tarde. 

Puede realizar todas estas acciones con el ejemplo de código **index-backup-restore** de este [repositorio de ejemplo .NET de Azure Cognitive Search](https://github.com/Azure-Samples/azure-search-dotnet-samples). 

También puede [obtener una definición de índice](https://docs.microsoft.com/rest/api/searchservice/get-index) en cualquier momento mediante la API REST de Azure Cognitive Search.

Actualmente no hay ninguna característica de extracción de índices, instantáneas o restauración de copias de seguridad en Azure Portal. Sin embargo, estamos considerando la opción de agregar la funcionalidad de restauración y copia de seguridad en una futura versión. Si quiere mostrar su apoyo para esta característica, vote en [User Voice](https://feedback.azure.com/forums/263029-azure-search/suggestions/8021610-backup-snapshot-of-index).

### <a name="can-i-restore-my-index-or-service-once-it-is-deleted"></a>¿Puedo restaurar mi índice o servicio una vez que los he eliminado?

No, si elimina un servicio o índice de Azure Cognitive Search, no se puede recuperar. Cuando elimina un servicio de Azure Cognitive Search, se eliminarán permanentemente todos los índices en el servicio. Si elimina un grupo de recursos de Azure que contiene uno o varios servicios de Azure Cognitive Search, se eliminarán permanentemente todos los servicios.  

Para volver a crear recursos, como índices, indexadores, orígenes de datos y conjuntos de habilidades, es necesario volver a crearlos desde el código. 

Para volver a crear un índice, debe volver a indexar los datos de orígenes externos. Por este motivo, se recomienda conservar una copia maestra o copia de seguridad de los datos originales en otro almacén de datos, como Azure SQL Database o Cosmos DB.

También puede usar el ejemplo de código **index-backup-restore** de este [repositorio de ejemplo .NET de Azure Cognitive Search](https://github.com/Azure-Samples/azure-search-dotnet-samples) para realizar la copia de seguridad de una definición de índice e indexar instantáneas en una serie de archivos JSON. Más tarde, puede usar la herramienta y los archivos para restaurar el índice, en caso de que sea necesario.  

### <a name="can-i-index-from-sql-database-replicas-applies-to-azure-sql-database-indexers"></a>¿Se pueden indexar réplicas de bases de datos SQL? (Se aplica a [indexadores de Azure SQL Database](https://docs.microsoft.com/azure/search/search-howto-connecting-azure-sql-database-to-azure-search-using-indexers))

No hay ninguna restricción en el uso de réplicas principales o secundarias como origen de datos al generar un índice desde el principio. Sin embargo, la actualización de un índice con actualizaciones incrementales (basadas en los registros modificados) requiere que réplica principal. Este requisito procede de SQL Database, que garantiza el seguimiento de los cambios solo en las réplicas principales. Si intenta usar réplicas secundarias para una carga de trabajo de actualización de índices, no hay ninguna garantía de que obtenga todos los datos.

## <a name="search-operations"></a>Operaciones de búsqueda

### <a name="can-i-search-across-multiple-indexes"></a>¿Se puede buscar en varios índices?

No, esta operación no se admite. La búsqueda siempre se centra en un solo índice.

### <a name="can-i-restrict-search-index-access-by-user-identity"></a>¿Se puede restringir el acceso al índice de búsqueda según la identidad del usuario?

Puede implementar [filtros de seguridad](https://docs.microsoft.com/azure/search/search-security-trimming-for-azure-search) con el filtro `search.in()`. El filtro se integra bien con [servicios de administración de identidades como Azure Active Directory (AAD)](https://docs.microsoft.com/azure/search/search-security-trimming-for-azure-search-with-aad) para recortar los resultados de la búsqueda en función de la pertenencia a un grupo de usuarios definido.

### <a name="why-are-there-zero-matches-on-terms-i-know-to-be-valid"></a>¿Por qué hay cero coincidencias en términos que sé que son válidos?

El caso más común no es saber que cada tipo de consulta admite niveles de análisis lingüísticos y comportamientos de búsqueda diferentes. La búsqueda de texto completo, que es la carga de trabajo predominante, incluye una fase de análisis del lenguaje que divide los términos hasta la forma raíz. Este aspecto del análisis de las consultas abarca una red más amplia para buscar las posibles coincidencias, porque el término con tokens coincide con un número mayor de variantes.

Las consultas con comodín, aproximadas y regex, sin embargo, no se analizan como consultas de términos o frases regulares y, por ello, pueden generar una coincidencia baja si la consulta no coincide con la forma analizada de la palabra en el índice de búsqueda. Para más información sobre el análisis de consultas, vea la [arquitectura de consulta](https://docs.microsoft.com/azure/search/search-lucene-query-architecture).

### <a name="my-wildcard-searches-are-slow"></a>Mis búsquedas con comodín son lentas.

La mayoría de las consultas de búsqueda con comodín, como las que llevan prefijo, las aproximadas y regex, se reescriben internamente con términos de coincidencia en el índice de búsqueda. Este procesamiento adicional de exploración del índice de búsqueda se agrega a la latencia. Además, las consultas de búsqueda amplias, como, por ejemplo, `a*`, que suelen reescribirse con muchos términos, pueden ser muy lentas. Para que las búsquedas con comodín tengan mayor rendimiento, considere la posibilidad de definir un [analizador personalizado](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search).

### <a name="why-is-the-search-rank-a-constant-or-equal-score-of-10-for-every-hit"></a>¿Por qué el rango de búsqueda es una clasificación igual o constante de 1.0 para cada acierto?

De forma predeterminada, los resultados de la búsqueda se puntúan según las [propiedades estadísticas de los términos de búsqueda de coincidencias](search-lucene-query-architecture.md#stage-4-scoring) y se ordena de mayor a menor en el conjunto de resultados. Sin embargo, algunos tipos de consulta (comodín, prefijo y regex) siempre aportan una puntuación constante a la puntuación total del documento. Este comportamiento es así por diseño. Azure Cognitive Search impone una puntuación constante que permite la inclusión de las coincidencias encontradas a través de la expansión de consultas en los resultados, pero sin que ello afecte a la clasificación.

Por ejemplo, suponga que la entrada "pase*" en una búsqueda con caracteres comodín genera coincidencias en "pasear", "paseo" y "paseante". Dada la naturaleza de estos resultados, no hay ninguna manera razonable de deducir qué términos son más valiosos que los demás. Por este motivo, se omiten las frecuencias de los términos cuando se realiza la puntuación de los resultados en consultas de los tipos caracteres comodín, prefijo y regex. Los resultados de la búsqueda en función de una entrada parcial reciben una puntuación constante para evitar un sesgo hacia coincidencias potencialmente inesperadas.

## <a name="design-patterns"></a>Patrones de diseño

### <a name="what-is-the-best-approach-for-implementing-localized-search"></a>¿Cuál es el mejor método para implementar la búsqueda localizada?

La mayoría de los clientes eligen campos dedicados en una colección cuando se trata de admitir distintas configuraciones regionales (idiomas) en el mismo índice. Los campos específicos de la configuración regional permiten asignar un analizador adecuado. Por ejemplo, asignar Microsoft French Analyzer a un campo que contiene cadenas en francés. También simplifica el filtrado. Si sabe que una consulta se inicia en una página fr-fr, puede limitar los resultados de búsqueda a este campo. O bien, puede crear un [perfil de puntuación](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) para dar al campo más peso relativo. Azure Cognitive Search admite más de [50 analizadores de lenguaje](https://docs.microsoft.com/azure/search/search-language-support) entre los que puede elegir.

## <a name="next-steps"></a>Pasos siguientes

¿Es su pregunta acerca de una característica o funcionalidad que falta? Solicite la característica en el [sitio web de User Voice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="see-also"></a>Consulte también

 [StackOverflow: Azure Cognitive Search](https://stackoverflow.com/questions/tagged/azure-search)   
 [Funcionamiento de la búsqueda de texto completo en Azure Cognitive Search](search-lucene-query-architecture.md)  
 [¿Qué es Azure Cognitive Search?](search-what-is-azure-search.md)
