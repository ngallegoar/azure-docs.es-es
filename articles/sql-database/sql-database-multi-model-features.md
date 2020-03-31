---
title: Funcionalidades de varios modelos
description: Azure SQL Database permite trabajar con varios modelos de datos en la misma base de datos.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: ''
ms.date: 12/17/2018
ms.openlocfilehash: 2e8519fa8d96b7fe016b9da4ba84ce481a57d94e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73802812"
---
# <a name="multi-model-capabilities-of-azure-sql-database"></a>Funcionalidades multimodelo de Azure SQL Database

Las bases de datos de varios modelos permiten almacenar datos representados en varios formatos como datos relacionales, grafos, documentos JSON o XML, pares clave-valor, etc., así como trabajar con dichos datos.

## <a name="when-to-use-multi-model-capabilities"></a>Cuándo usar las funcionalidades multimodelo

Azure SQL Database está diseñado para trabajar con el modelo relacional que proporciona el mejor rendimiento en la mayoría de los casos para una variedad de aplicaciones de uso general. Sin embargo, no se limita únicamente a datos relacionales. Azure SQL Database permite usar una variedad de formatos no relacionales que están estrechamente integrados en el modelo relacional.
Considere la posibilidad de usar funcionalidades multimodelo de Azure SQL Database en los casos siguientes:
- Tiene cierta información o estructuras que se ajustan mejor a para los modelos de NoSQL y no quiere usar una base de datos de NoSQL independiente.
- La mayoría de los datos es adecuada para el modelo relacional y necesita modelar algunas partes de los datos en el estilo de NoSQL.
- Quiere aprovechar el amplio lenguaje Transact-SQL para consultar y analizar datos relacionales y NoSQL, e integrarlos con una variedad de herramientas y aplicaciones que pueden usar el lenguaje SQL.
- Quiere aplicar características de base de datos, como [tecnologías en memoria](sql-database-in-memory.md) para mejorar el rendimiento del análisis o el procesamiento de las estructuras de datos NoSQL, usar la [replicación transaccional](sql-database-managed-instance-transactional-replication.md) o [réplicas legibles](sql-database-read-scale-out.md) para crear la copia de los datos en el otro lugar y descargar algunas cargas de trabajo de análisis desde la base de datos principal.

## <a name="overview"></a>Información general

Azure SQL proporciona las siguientes características de varios modelos:
- Las [características de Graph](#graph-features) permiten representar los datos como un conjunto de nodos y bordes, además de usar las consultas de Transact-SQL estándares mejoradas con el operador `MATCH` de grafos para consultar los datos del grafo.
- Las [características de JSON](#json-features) permiten colocar documentos JSON en tablas, transformar datos relacionales para documentos JSON y viceversa. Puede usar el lenguaje Transact-SQL estándar mejorado con las funciones JSON para analizar documentos y usar índices no agrupados, índices de almacén de columnas o tablas optimizadas para memoria, a fin de optimizar las consultas.
- Las [características espaciales](#spatial-features) permiten almacenar datos geográficos y geométricos, indexarlos con los índices espaciales y recuperar los datos mediante consultas espaciales.
- Las [características XML](#xml-features) permiten almacenar e indexar datos XML en la base de datos y utilizar operaciones XQuery y XPath nativas para trabajar con datos XML. Azure SQL Database tiene un motor de consulta XML integrado y especializados que procesa los datos XML.
- Los [pares clave-valor](#key-value-pairs) no se admiten de forma explícita como características especiales, ya que se pueden modelar de forma nativa como tablas de dos columnas.

  > [!Note]
  > Puede usar la expresión de ruta de acceso JSON, las expresiones XQuery y XPath, las funciones espaciales y las expresiones de consulta de grafos en la misma consulta de Transact-SQL para acceder a los datos almacenados en la base de datos. Además, cualquier herramienta o lenguaje de programación que puede ejecutar consultas de Transact-SQL, también puede usar esa interfaz de consulta para obtener acceso a datos de varios modelos. Esta es la diferencia clave en comparación con las bases de datos de varios modelos como [Azure Cosmos DB](/azure/cosmos-db/) que proporciona una API especializada para diferentes modelos de datos.

En las secciones siguientes, puede obtener información acerca de las funcionalidades de varios modelos más importantes de Azure SQL Database.

## <a name="graph-features"></a>Características de grafos

Azure SQL Database ofrece funcionalidades de base de datos de grafos para modelar las relaciones de varios a varios en la base de datos. Un grafo es una colección de nodos (o vértices) y bordes (o relaciones). Un nodo representa una entidad (por ejemplo, una persona o una organización) y un borde representa una relación entre los dos nodos que conecta (por ejemplo, "Me gusta" o amigos). Estas son algunas características que hacen que una base de datos de grafos sea única:
- Los bordes o las relaciones son entidades de primera clase en una base de datos de grafos y pueden tener atributos o propiedades asociados a ellas.
- Un solo borde puede conectar flexiblemente varios nodos en una base de datos de grafos.
- Puede expresar fácilmente coincidencias de patrones y consultas de navegación en saltos múltiples.
- Puede expresar fácilmente consultas polimórficas y cierres transitivos.

Las relaciones de grafos y funcionalidades de consulta de grafos están integradas en Transact-SQL y reciben las ventajas de usar SQL Server como sistema fundamental de administración de base de datos.
El [procesamiento de grafos](https://docs.microsoft.com/sql/relational-databases/graphs/sql-graph-overview) es la principal característica de motor de base de datos de SQL Server, por lo que ahí encontrará más información acerca del procesamiento de grafos.

### <a name="when-to-use-a-graph-capability"></a>Cuándo usar una funcionalidad de grafo

No hay nada que una base de datos de grafos puede conseguir, que no se puede lograr mediante una base de datos relacional. Sin embargo, una base de datos de grafos puede facilitar la expresión de determinadas consultas. La decisión para elegir una u otra puede basarse en los siguientes factores:

- Datos jerárquicos del modelo donde un nodo puede tener varios elementos primarios, por lo que no se puede usar HierarchyId.
- La aplicación tiene relaciones complejas de varios a varios; a medida que la aplicación evoluciona, se agregan nuevas relaciones.
- Necesita analizar las relaciones y los datos interconectados.

## <a name="json-features"></a>Característica de JSON

Azure SQL Database permite analizar y consultar datos representados en formato de notación de objetos JavaScript [(JSON)](https://www.json.org/), y exportar los datos relacionales como texto JSON.

JSON es un formato de datos conocido que se usa para intercambiar datos en aplicaciones web y móviles modernas. JSON también se utiliza para almacenar datos semiestructurados en archivos de registro o en bases de datos NoSQL como [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). Muchos servicios web REST devuelven resultados con formato de texto JSON o bien aceptan datos con este formato. La mayoría de los servicios de Azure, como [Azure Cognitive Search](https://azure.microsoft.com/services/search/), [Azure Storage](https://azure.microsoft.com/services/storage/) y [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/), tienen puntos de conexión REST que devuelven o consumen JSON.

Azure SQL Database permite trabajar fácilmente con datos JSON e integrar su base de datos con servicios modernos. Azure SQL Database proporciona las siguientes funciones para trabajar con datos JSON:

![Funciones JSON](./media/sql-database-json-features/image_1.png)

Si tiene texto JSON, puede extraer datos de JSON o comprobar que el formato JSON sea correcto con las funciones integradas [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx) y [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx). La función [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) le permite actualizar valores dentro del texto JSON. Para consultas y análisis más avanzados, la función [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) puede transformar una matriz de objetos JSON en un conjunto de filas. Se puede ejecutar cualquier consulta SQL en el conjunto de resultados devuelto. Por último, hay una cláusula [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) que permite dar formato de texto JSON a los datos almacenados en las tablas relacionales.

Para más información, consulte [Cómo trabajar con datos JSON en Azure SQL Database](sql-database-json-features.md).
[JSON](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) es la principal característica de motor de base de datos de SQL Server, por lo que ahí encontrará más información acerca de la característica de JSON.

### <a name="when-to-use-a-json-capability"></a>Cuándo usar una funcionalidad de JSON

Los modelos de documento se pueden utilizar en lugar de los modelos relacionales en algunos escenarios concretos:
- La alta normalización del esquema no ofrece ventajas significativas, ya que accede a todos los campos de objetos a la vez o nunca actualiza las partes normalizadas de los objetos. Sin embargo, el modelo normalizado aumenta la complejidad de las consultas debido al gran número de tablas que se deben unir para obtener los datos.
- Está trabajando con las aplicaciones que usan de forma nativa los documentos de JSON como modelos de datos o comunicación y no desea introducir capas adicionales que transforman los datos relacionales en JSON y viceversa.
- Se necesita simplificar el modelo de datos anulando la normalización de las tablas secundarias o los patrones de valor de objeto de entidad.
- Deberá cargar o exportar los datos almacenados en formato JSON sin alguna herramienta adicional que analice los datos.

## <a name="spatial-features"></a>Características espaciales

Los datos espaciales representan información sobre la ubicación física y la forma de objetos geométricos. Estos objetos pueden ser ubicaciones de punto u objetos más complejos, como países o regiones, carreteras o lagos.

Azure SQL Database admite dos tipos de datos espaciales: el tipo de datos geométricos y el tipo de datos geográficos.
- El tipo geométrico representa los datos en un sistema de coordenadas euclidiano (plano).
- El tipo geográfico representa los datos en un sistema de coordenadas elipsoidales.

Hay varios objetos espaciales que se pueden utilizar en Azure SQL Database, como [Point](https://docs.microsoft.com/sql/relational-databases/spatial/point), [LineString](https://docs.microsoft.com/sql/relational-databases/spatial/linestring), [Polygon](https://docs.microsoft.com/sql/relational-databases/spatial/polygon), etc.

Azure SQL Database proporciona también [índices espaciales](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-indexes-overview) especializados que se pueden usar para mejorar el rendimiento de las consultas espaciales.

El [soporte espacial](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) es la principal característica de motor de base de datos de SQL Server, por lo que ahí encontrará más información acerca de la característica espacial.

## <a name="xml-features"></a>Características de XML

SQL Server proporciona una plataforma eficaz para desarrollar aplicaciones completas para la administración de datos semiestructurados. La compatibilidad con XML está integrada en todos los componentes de SQL Server e incluye lo siguiente:

- El tipo de datos XML. Los valores XML se pueden almacenar de forma nativa en una columna de tipo de datos xml cuyo tipo se puede determinar según una colección de esquemas XML, o se puede dejar sin tipo. Puede indexar la columna XML.
- La capacidad de especificar una consulta con datos XQuery almacenados en columnas y variables del tipo xml. Las funcionalidades de XQuery pueden usarse en cualquier consulta de Transact-SQL que tiene acceso a cualquier modelo de datos que se utiliza en la base de datos.
- Indexe automáticamente todos los elementos en documentos XML mediante el [índice XML principal](https://docs.microsoft.com/sql/relational-databases/xml/xml-indexes-sql-server#primary-xml-index) o especifique las rutas de acceso exactas que se deben indexar mediante el [índice XML secundario](https://docs.microsoft.com/sql/relational-databases/xml/xml-indexes-sql-server#secondary-xml-indexes).
- OPENROWSET que permite la carga masiva de datos XML.
- Transforme datos relacionales en formato XML.

[XML](https://docs.microsoft.com/sql/relational-databases/xml/xml-data-sql-server) es la principal característica de motor de base de datos de SQL Server, por lo que ahí encontrará más información acerca de la característica de XML.

### <a name="when-to-use-an-xml-capability"></a>Cuándo usar una funcionalidad de XML

Los modelos de documento se pueden utilizar en lugar de los modelos relacionales en algunos escenarios concretos:
- La alta normalización del esquema no ofrece ventajas significativas, ya que accede a todos los campos de objetos a la vez o nunca actualiza las partes normalizadas de los objetos. Sin embargo, el modelo normalizado aumenta la complejidad de las consultas debido al gran número de tablas que se deben unir para obtener los datos.
- Está trabajando con las aplicaciones que usan de forma nativa los documentos de XML como modelos de datos o comunicación y no desea introducir capas adicionales que transforman los datos relacionales en XML y viceversa.
- Se necesita simplificar el modelo de datos anulando la normalización de las tablas secundarias o los patrones de valor de objeto de entidad.
- Deberá cargar o exportar los datos almacenados en formato XML sin alguna herramienta adicional que analice los datos.

## <a name="key-value-pairs"></a>Pares de clave-valor

Azure SQL Database no tiene tipos estructuras o tipos especializados que admiten pares de clave-valor, puesto que las estructuras clave-valor pueden representarse de forma nativa como tablas relacionales estándar:

```sql
CREATE TABLE Collection (
  Id int identity primary key,
  Data nvarchar(max)
)
```

Puede personalizar esta estructura clave-valor para satisfacer sus necesidades sin restricciones. Por ejemplo, el valor puede ser el documento XML en lugar del tipo `nvarchar(max)`, si el valor es el documento JSON, puede poner la restricción `CHECK` que comprueba la validez del contenido JSON. Puede colocar cualquier número de valores relacionados con una clave en las columnas adicionales, agregar columnas calculadas e índices para simplificar y optimizar el acceso a datos, definir la tabla como tabla de solo esquema optimizado y de memoria para obtener un mejor rendimiento, etc.

Consulte [Cómo BWin usa OLTP en memoria para lograr un rendimiento y escala sin precedentes](https://blogs.msdn.microsoft.com/sqlcat/20../../how-bwin-is-using-sql-server-2016-in-memory-oltp-to-achieve-unprecedented-performance-and-scale/) para su solución de almacenamiento en caché de ASP.NET que logró 1.200.000 lotes por segundo, como ejemplo de cómo un modelo relacional se puede usar eficazmente como solución de par clave-valor en la práctica.

## <a name="next-steps"></a>Pasos siguientes
Las funcionalidades de varios modelos en Azure SQL Database también son las características principales del motor de base de datos de SQL Server que se comparten entre Azure SQL Database y SQL Server. Para obtener más información acerca de estas características, visite las páginas de documentación de base de datos relacional de SQL:

* [Procesamiento de Graph](https://docs.microsoft.com/sql/relational-databases/graphs/sql-graph-overview)
* [Datos de JSON](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server)
* [Soporte espacial](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [Datos XML](https://docs.microsoft.com/sql/relational-databases/xml/xml-data-sql-server)
