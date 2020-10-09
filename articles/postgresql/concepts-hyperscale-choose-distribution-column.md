---
title: 'Selección de las columnas de distribución en Azure Database for PostgreSQL: Hiperescala (Citus)'
description: 'Aprenda a elegir columnas de distribución en escenarios habituales de Azure Database for PostgreSQL: Hiperescala (Citus).'
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: f3c86f4b194e6d3935434b35cae958dfcf772986
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91314919"
---
# <a name="choose-distribution-columns-in-azure-database-for-postgresql--hyperscale-citus"></a>Selección de las columnas de distribución en Azure Database for PostgreSQL: Hiperescala (Citus) (versión preliminar)

Elegir la columna de distribución de cada tabla es una de las decisiones de modelado más importantes que se tiene que adoptar. Hiperescala (Citus) de Azure Database for PostgreSQL almacena filas en particiones de base de datos basándose en el valor de la columna de distribución de las filas.

La elección correcta agrupa los datos relacionados en los mismos nodos físicos, lo que agiliza las consultas y agrega compatibilidad con todas las características de SQL. Si elige de forma incorrecta, el sistema se ejecutará lentamente y no admitirá todas las características de SQL en los nodos.

En este artículo se ofrecen consejos sobre las columnas de distribución para los dos escenarios más comunes de Hiperescala (Citus).

### <a name="multi-tenant-apps"></a>Aplicaciones multiinquilino

La arquitectura de multiinquilino usa una forma de modelado jerárquico de base de datos para distribuir consultas entre nodos en el grupo de servidores. La parte superior de la jerarquía de datos se conoce como *identificador de inquilino* y debe almacenarse en una columna en cada tabla.

Hiperescala (Citus) inspecciona las consultas para ver a qué identificador de inquilino implican y localiza la partición de tabla correspondiente. Asimismo, dirige la consulta a un único nodo de trabajo que contiene la partición. La ejecución de una consulta con todos los datos relevantes colocados en el mismo nodo se denomina colocación.

El siguiente diagrama ilustra la colocación en el modelo de datos del multiinquilino. Contiene dos tablas, Cuentas y Campañas, cada una distribuida mediante `account_id`. Los cuadros sombreados representan particiones. Las particiones verdes se almacenan juntas en un nodo de trabajo y las particiones azules se almacenan en otro. Observe cómo una consulta de combinación entre las tablas Accounts y Campaigns tiene todos los datos necesarios juntos en un nodo cuando ambas tablas se restringen al mismo valor de account\_id.

![Coubicación multiinquilino](media/concepts-hyperscale-choosing-distribution-column/multi-tenant-colocation.png)

Para aplicar este diseño en su propio esquema, debe identificar qué constituye un inquilino en su aplicación. Las instancias comunes incluyen la empresa, la cuenta, la organización o el cliente. El nombre de la columna será algo como `company_id` o `customer_id`. Examine cada una de las consultas y pregúntese: ¿funcionaría si tuviera cláusulas WHERE adicionales para restringir todas las tablas implicadas a filas con el mismo identificador de inquilino?
El ámbito de las consultas en el modelo multiinquilino se limita a un inquilino. Por ejemplo, el ámbito de las consultas de ventas o inventario se limita a una tienda determinada.

#### <a name="best-practices"></a>Procedimientos recomendados

-   **Tablas que una\_ columna de**id. de inquilino distribuye en particiones. Por ejemplo, en una aplicación SaaS donde los inquilinos son empresas, el valor de tenant\_id será probablemente company\_id.
-   **Convierta pequeñas tablas entre inquilinos en tablas de referencia.** Cuando varios inquilinos comparten una pequeña tabla de información, distribúyala como una tabla de referencia.
-   **Restrinja la opción para filtrar todas las consultas de las aplicaciones en función del id.\_del inquilino.** Cada consulta debe solicitar información para un inquilino a la vez.

Lea el [tutorial de multiinquilino](./tutorial-design-database-hyperscale-multi-tenant.md) para ver un ejemplo de cómo crear este tipo de aplicación.

### <a name="real-time-apps"></a>Aplicaciones en tiempo real

La arquitectura de multiinquilino introduce una estructura jerárquica y utiliza la colocación de datos para enrutar las consultas por cada inquilino. Por el contrario, las arquitecturas en tiempo real dependen de las propiedades de distribución específicas de sus datos para lograr un procesamiento altamente paralelo.

Usamos "id. de entidad" como término para las columnas de distribución en el modelo en tiempo real. Las entidades típicas son usuarios, hosts o dispositivos.

Las consultas en tiempo real suelen solicitar agregados numéricos agrupados en función de la fecha o la categoría. Hiperescala (Citus) envía estas consultas a cada partición para obtener resultados parciales y reúne la respuesta final en el nodo de coordinación. Las consultas se ejecutan más rápido cuando contribuyen tantos nodos como sea posible y cuando ningún nodo debe realizar una cantidad desproporcionada de trabajo.

#### <a name="best-practices"></a>Procedimientos recomendados

-   **Elija una columna con alta cardinalidad, como la columna de distribución.** A modo de comparación, un campo de estado en una tabla de pedidos con los valores Nuevo, Pagado y Enviado no es una buena opción como columna de distribución. Como asume solo esos pocos valores, se limita la cantidad de particiones que pueden contener los datos y la cantidad de nodos que pueden procesarlos. Entre las columnas con alta cardinalidad, también es recomendable elegir aquellas que se usan frecuentemente en cláusulas de agrupación o como claves de combinación.
-   **Elija una columna con distribución uniforme.** Si distribuye una tabla en una columna con tendencia a determinados valores comunes, sus datos también tenderán a acumularse en determinadas particiones. Los nodos que incluyen estas particiones terminan trabajando más que otros nodos.
-   **Distribuya las tablas de hechos y dimensiones en sus columnas comunes.**
    Su tabla de hechos solo puede tener una clave de distribución. Las tablas que se combinan con otra clave no se coubicarán con la tabla de hechos. Elija una dimensión para la coubicación en función de la frecuencia con la que se combina y del tamaño de las filas de combinación.
-   **Cambie algunas tablas de dimensiones para que sean tablas de referencia.** Si una tabla de dimensiones no puede coubicarse con la tabla de hechos, puede mejorar el rendimiento de la consulta distribuyendo copias de la tabla de dimensiones a todos los nodos en forma de una tabla de referencia.

Lea el [tutorial del panel en tiempo real](./tutorial-design-database-hyperscale-realtime.md) para ver un ejemplo de cómo crear este tipo de aplicación.

### <a name="time-series-data"></a>Datos de serie temporal

En una carga de trabajo de serie temporal, las aplicaciones consultan información reciente a la vez que archivan información antigua.

El error más común a la hora de modelar la información de serie temporal en Hiperescala (Citus) consiste en usar la propia marca de tiempo como columna de distribución. Una distribución de hash basada en el tiempo distribuye los tiempos aleatoriamente en diferentes particiones, en lugar de mantener intervalos de tiempo juntos en particiones. Las consultas que implican tiempo generalmente hacen referencia a intervalos de tiempo, por ejemplo, los datos más recientes. Este tipo de distribución de hash conduce a una sobrecarga de la red.

#### <a name="best-practices"></a>Procedimientos recomendados

-   **No elija una marca de tiempo como columna de distribución.** Elija una columna de distribución diferente. En una aplicación de multiinquilino use el id. del inquilino, o en una aplicación en tiempo real use el id. de la entidad.
-   **En su lugar, use la partición de la tabla PostgreSQL para el tiempo.** Use la creación de particiones de tablas para dividir una tabla grande de datos ordenados en función del tiempo en varias tablas heredadas, cada una con diferentes intervalos de tiempo. La distribución de una tabla con particiones de Postgres en Hiperescala (Citus) crea particiones de base de datos para las tablas heredadas.

Lea el [tutorial de la serie temporal](https://aka.ms/hyperscale-tutorial-timeseries) para ver un ejemplo de cómo crear este tipo de aplicación.

## <a name="next-steps"></a>Pasos siguientes
- Aprenda cómo la [coubicación](concepts-hyperscale-colocation.md) entre datos distribuidos ayuda a que las consultas se ejecuten rápidamente.
