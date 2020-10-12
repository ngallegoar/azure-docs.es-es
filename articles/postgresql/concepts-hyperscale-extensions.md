---
title: Extensiones de Hiperescala (Citus) en Azure Database for PostgreSQL
description: Describe la capacidad de ampliar la funcionalidad de la base de datos mediante extensiones de Azure Database for PostgreSQL con Hiperescala (Citus).
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 07/09/2020
ms.openlocfilehash: ed023f17e19c6a018f55cb95df6447c6e5474ce8
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91314885"
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql--hyperscale-citus"></a>Extensiones de PostgreSQL en Azure Database for PostgreSQL - Hiperescala (Citus)

PostgreSQL ofrece la capacidad de ampliar la funcionalidad de la base de datos mediante extensiones. Las extensiones permiten agrupar varios objetos SQL relacionados en un solo paquete que se puede cargar o quitar de la base de datos con un solo comando. Después de cargarse en la base de datos, las extensiones pueden funcionar como características integradas. Para más información sobre las extensiones de PostgreSQL, consulte  [Packaging Related Objects into an Extension](https://www.postgresql.org/docs/current/static/extend-extensions.html) (Empaquetado de objetos relacionados en una extensión).

## <a name="use-postgresql-extensions"></a>Uso de extensiones de PostgreSQL

Para poder usar las extensiones de PostgreSQL, es preciso que estén instaladas en su base de datos. Para instalar una extensión específica, ejecute el comando  [CREATE EXTENSION](https://www.postgresql.org/docs/current/static/sql-createextension.html)  desde la herramienta psql para cargar los objetos empaquetados en la base de datos.

Hiperescala (Citus) de Azure Database for PostgreSQL admite actualmente un subconjunto de extensiones clave, que se enumeran a continuación. No se admiten extensiones distintas de las que se incluyen. No puede crear su propia extensión con Azure Database for PostgreSQL.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>Extensiones admitidas por Azure Database for PostgreSQL

En las tablas siguientes se enumeran las extensiones estándar de PostgreSQL que Azure Database for PostgreSQL admite actualmente. Esta información también está disponible al ejecutar `SELECT * FROM pg_available_extensions;`.

### <a name="data-types-extensions"></a>Extensiones de tipos de datos

> [!div class="mx-tableFixed"]
> | **Extensión** | **Descripción** |
> |---|---|
> | [citext](https://www.postgresql.org/docs/current/static/citext.html) | Proporciona un tipo de cadena de caracteres que no distingue entre mayúsculas y minúsculas. |
> | [cube](https://www.postgresql.org/docs/current/static/cube.html) | Proporciona un tipo de datos para los cubos multidimensionales. |
> | [hstore](https://www.postgresql.org/docs/current/static/hstore.html) | Proporciona un tipo de datos para almacenar conjuntos de pares clave-valor. |
> | [hll](https://github.com/citusdata/postgresql-hll) | Proporciona una estructura de datos HyperLogLog. |
> | [isn](https://www.postgresql.org/docs/current/static/isn.html) | Proporciona tipos de datos para los estándares internacionales de numeración de productos. |
> | [lo](https://www.postgresql.org/docs/current/lo.html) | Mantenimiento de objetos grandes. |
> | [ltree](https://www.postgresql.org/docs/current/static/ltree.html) | Proporciona un tipo de datos para las estructuras de árbol jerárquicas. |
> | [seg](https://www.postgresql.org/docs/current/seg.html) | Tipo de datos para representar los segmentos de línea o intervalos de punto flotante. |
> | [topn](https://github.com/citusdata/postgresql-topn/) | Tipo para JSONB de n principales. |

### <a name="full-text-search-extensions"></a>Extensiones de búsqueda de texto completo

> [!div class="mx-tableFixed"]
> | **Extensión** | **Descripción** |
> |---|---|
> | [dict\_int](https://www.postgresql.org/docs/current/static/dict-int.html) | Proporciona una plantilla de diccionario de búsqueda de texto para números enteros. |
> | [dict\_xsyn](https://www.postgresql.org/docs/current/dict-xsyn.html) | Plantilla de diccionario de búsqueda de texto para el procesamiento de sinónimos extendido. |
> | [unaccent](https://www.postgresql.org/docs/current/static/unaccent.html) | Un diccionario de búsqueda de texto que quita los acentos (signos diacríticos) de lexemas. |

### <a name="functions-extensions"></a>Extensiones de funciones

> [!div class="mx-tableFixed"]
> | **Extensión** | **Descripción** |
> |---|---|
> | [autoinc](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.7) | Funciones para campos de incremento automático. |
> | [earthdistance](https://www.postgresql.org/docs/current/static/earthdistance.html) | Proporciona un medio para calcular distancias de círculo máximo en la superficie de la Tierra. |
> | [fuzzystrmatch](https://www.postgresql.org/docs/current/static/fuzzystrmatch.html) | Proporciona varias funciones para determinar las similitudes y la distancia entre las cadenas. |
> | [insert\_username](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.8) | Funciones para el seguimiento de quién cambió una tabla. |
> | [intagg](https://www.postgresql.org/docs/current/intagg.html) | El agregador y enumerador de enteros (obsoleto). |
> | [intarray](https://www.postgresql.org/docs/current/static/intarray.html) | Proporciona funciones y operadores para manipular matrices de enteros sin valores nulos. |
> | [moddatetime](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.9) | Funciones para el seguimiento de la hora de última modificación. |
> | [pgcrypto](https://www.postgresql.org/docs/current/static/pgcrypto.html) | Proporciona funciones de cifrado. |
> | [pg\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Administra las tablas con particiones por hora o identificador. |
> | [pg\_trgm](https://www.postgresql.org/docs/current/static/pgtrgm.html) | Proporciona funciones y operadores para determinar la similitud del texto alfanumérico basado en la coincidencia de trigrama. |
> | [refint](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.5) | Funciones para implementar la integridad referencial (obsoleto). |
> | session\_analytics | Funciones para realizar consultas a matrices hstore. |
> | [tablefunc](https://www.postgresql.org/docs/current/static/tablefunc.html) | Proporciona funciones que manipulan la totalidad del contenido de las tablas, incluidas tablas de referencias cruzadas. |
> | [tcn](https://www.postgresql.org/docs/current/tcn.html) | Desencadenó notificaciones de cambio. |
> | [timetravel](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.6) | Funciones para implementar el viaje en el tiempo. |
> | [uuid-ossp](https://www.postgresql.org/docs/current/static/uuid-ossp.html) | Genera identificadores únicos universales (UUID). |

### <a name="hyperscale-citus-extensions"></a>Extensiones de Hiperescala (Citus)

> [!div class="mx-tableFixed"]
> | **Extensión** | **Descripción** |
> |---|---|
> | [citus](https://github.com/citusdata/citus) | Base de datos distribuida de Citus. |
> | shard\_rebalancer | Volver a equilibrar los datos con seguridad en un grupo de servidores en el caso de adición o eliminación del nodo. |

### <a name="index-types-extensions"></a>Extensiones de tipos de índices

> [!div class="mx-tableFixed"]
> | **Extensión** | **Descripción** |
> |---|---|
> | [bloom](https://www.postgresql.org/docs/current/bloom.html) | Método de acceso de bloom: índice basado en archivos de firma. |
> | [btree\_gin](https://www.postgresql.org/docs/current/static/btree-gin.html) | Proporciona clases de operador GIN de ejemplo que implementan el comportamiento similar al del árbol B para determinados tipos de datos. |
> | [btree\_gist](https://www.postgresql.org/docs/current/static/btree-gist.html) | Proporciona clases de operador de índice de GiST que implementan el árbol B. |

### <a name="language-extensions"></a>Extensiones de lenguaje

> [!div class="mx-tableFixed"]
> | **Extensión** | **Descripción** |
> |---|---|
> | [plpgsql](https://www.postgresql.org/docs/current/static/plpgsql.html) | Lenguaje de procedimientos que puede cargar PL/pgSQL. |

### <a name="miscellaneous-extensions"></a>Extensiones variadas

> [!div class="mx-tableFixed"]
> | **Extensión** | **Descripción** |
> |---|---|
> | [adminpack](https://www.postgresql.org/docs/current/adminpack.html) | Funciones administrativas para PostgreSQL. |
> | [amcheck](https://www.postgresql.org/docs/current/amcheck.html) | Funciones de comprobación de la integridad de la relación. |
> | [file\_fdw](https://www.postgresql.org/docs/current/file-fdw.html) | Contenedor de datos externo para el acceso de archivos planos. |
> | [pageinspect](https://www.postgresql.org/docs/current/pageinspect.html) | Inspeccione el contenido de páginas de bases de datos a un nivel bajo. |
> | [pg\_buffercache](https://www.postgresql.org/docs/current/static/pgbuffercache.html) | Proporciona un medio para examinar lo que sucede en la caché del búfer compartido en tiempo real. |
> | [pg\_cron](https://github.com/citusdata/pg_cron) | Programador de trabajos para PostgreSQL. |
> | [pg\_freespacemap](https://www.postgresql.org/docs/current/pgfreespacemap.html) | Examine la asignación de espacio libre (FSM). |
> | [pg\_prewarm](https://www.postgresql.org/docs/current/static/pgprewarm.html) | Proporciona una manera de cargar datos de relación en la caché del búfer. |
> | [pg\_stat\_statements](https://www.postgresql.org/docs/current/static/pgstatstatements.html) | Proporciona un medio para realizar el seguimiento de las estadísticas de ejecución de todas las instrucciones SQL ejecutadas por un servidor. Consulte la sección "pg_stat_statements" para información sobre esta extensión. |
> | [pg\_visibility](https://www.postgresql.org/docs/current/pgvisibility.html) | Permite examinar el mapa de visibilidad y la información de visibilidad de nivel de página. |
> | [pgrowlocks](https://www.postgresql.org/docs/current/static/pgrowlocks.html) | Proporciona un medio para mostrar información de bloqueo a nivel de fila. |
> | [pgstattuple](https://www.postgresql.org/docs/current/static/pgstattuple.html) | Proporciona un medio para mostrar estadísticas de nivel de tupla. |
> | [postgres\_fdw](https://www.postgresql.org/docs/current/static/postgres-fdw.html) | Se trata de un contenedor de datos externo utilizado para tener acceso a los datos almacenados en los servidores externos de PostgreSQL. Consulte la sección "dblink y postgres_fdw" para información sobre esta extensión.|
> | [sslinfo](https://www.postgresql.org/docs/current/sslinfo.html) | Información sobre los certificados TLS/SSL. |
> | [tsm\_system\_rows](https://www.postgresql.org/docs/current/tsm-system-rows.html) | Método TABLESAMPLE que acepta el número de filas como un límite. |
> | [tsm\_system\_time](https://www.postgresql.org/docs/current/tsm-system-time.html) | Método TABLESAMPLE que acepta el tiempo en milisegundos como un límite. |
> | [hypopg](https://hypopg.readthedocs.io/en/latest/) | Proporciona un medio de creación de índices hipotéticos que no consume CPU ni disco. |
> | [dblink](https://www.postgresql.org/docs/current/dblink.html) | Módulo que admite conexiones a otras bases de datos de PostgreSQL desde una sesión de base de datos. Consulte la sección "dblink y postgres_fdw" para información sobre esta extensión. |
> | [xml2](https://www.postgresql.org/docs/current/xml2.html) | Consulta de XPath y XSLT. |


### <a name="postgis-extensions"></a>Extensiones de PostGIS

> [!div class="mx-tableFixed"]
> | **Extensión** | **Descripción** |
> |---|---|
> | [PostGIS](https://www.postgis.net/), postgis\_topology, postgis\_tiger\_geocoder, postgis\_sfcgal | Objetos espaciales y geográficos para PostgreSQL. |
> | address\_standardizer, address\_standardizer\_data\_us | Se utilizan para analizar una dirección en los elementos que la componen. Se utilizan para admitir el paso de normalización de la dirección de geocodificación. |
> | postgis\_sfcgal | Funciones de PostGIS SFCGAL. |
> | postgis\_tiger\_geocoder | Geocoder de PostGIS tiger y geocoder inverso. |
> | postgis\_topology | Funciones y tipos espaciales de topología PostGIS. |


## <a name="pg_stat_statements"></a>pg_stat_statements
La [extensión pg\_stat\_statements](https://www.postgresql.org/docs/current/pgstatstatements.html) está precargada en cada servidor de Azure Database for PostgreSQL para proporcionarle un medio de seguimiento de las estadísticas de ejecución de las instrucciones SQL.

El valor `pg_stat_statements.track` controla qué instrucciones cuenta la extensión. El valor predeterminado es `top`, lo que significa que se realiza el seguimiento de todas las instrucciones emitidas directamente por los clientes. Los otros dos niveles de seguimiento son `none` y `all`. Esta configuración es configurable como un parámetro de servidor a través de [Azure Portal](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-portal) o la [CLI de Azure](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-cli).

Hay un equilibrio entre la información de ejecución de consulta que proporciona pg_stat_statements y el efecto en el rendimiento del servidor al registrar cada instrucción SQL. Si no está usando activamente la extensión pg_stat_statements, le recomendamos que establezca `pg_stat_statements.track` en `none`. Tenga en cuenta que algunos servicios de supervisión de terceros pueden basarse en pg_stat_statements para entregar información de rendimiento de consultas, por lo que debe confirmar si este es su caso o no.

## <a name="dblink-and-postgres_fdw"></a>dblink y postgres_fdw

Puede usar dblink y postgres\_fdw para conectarse desde un servidor PostgreSQL a otro, o a otra base de datos en el mismo servidor.  El servidor de recepción debe permitir conexiones del servidor de envío a través de su firewall.  Para usar estas extensiones para conectarse entre servidores de Azure Database for PostgreSQL o grupos de servidores de Hiperescala (Citus), establezca **Permitir que los servicios y recursos de Azure accedan a este grupo de servidores (o servidor)** en Activado.  También debe activar este valor si desea usar las extensiones para volver en bucle al mismo servidor.
La opción **Permitir que los servicios y recursos de Azure accedan a este grupo de servidores** se encuentra en la página Azure Portal del grupo de servidores de Hiperescala (Citus) en **Redes**.  Actualmente no se admiten más conexiones salientes desde servidores de Azure Database for PostgreSQL e Hiperescala (Citus),excepto las conexiones a otros servidores de Azure Database for PostgreSQL y grupos de servidores de Hiperescala (Citus).
