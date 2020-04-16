---
title: Procedimientos recomendados de carga de datos para el grupo de SQL de Synapse
description: Recomendaciones y optimizaciones de rendimiento para cargar datos mediante el grupo de SQL de Synapse.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/04/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: e170a789727fb0de36705895245cc638d30ee3d7
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80745497"
---
# <a name="best-practices-for-loading-data-using-synapse-sql-pool"></a>Procedimientos recomendados para cargar datos mediante el grupo de SQL de Synapse

En este artículo, conocerá las recomendaciones y las optimizaciones de rendimiento para cargar datos mediante el grupo de SQL.

## <a name="preparing-data-in-azure-storage"></a>Preparación de datos en Azure Storage

Para minimizar la latencia, reubique la capa de almacenamiento y el grupo de SQL.

Al exportar datos en formato de archivo ORC, pueden producirse errores de falta de memoria de Java con las columnas de texto grandes. Para resolver este problema, exporte solo un subconjunto de las columnas.

PolyBase no puede cargar filas que tengan más de un millón de bytes de datos. Cuando ponga datos en los archivos de texto de Azure Blob Storage o Azure Data Lake Store, estos tienen que tener menos de un millón de bytes de datos. Esta limitación de datos es cierta independientemente del esquema de tabla definido.

Todos los formatos de archivo tienen diferentes características de rendimiento. Para lograr la carga más rápida, use archivos de texto delimitados comprimidos. La diferencia entre el rendimiento de UTF-8 y UTF-16 es mínima.

Dividir archivos comprimidos grandes en archivos comprimidos más pequeños.

## <a name="running-loads-with-enough-compute"></a>Ejecución de cargas con suficientes recursos de proceso

Para una velocidad de carga más rápida, ejecute solo una carga de trabajo de cada vez. Si no es factible, ejecute un número mínimo de cargas al mismo tiempo. Si espera un trabajo de carga grande, considere la posibilidad de escalar verticalmente el grupo de SQL antes de la carga.

Para ejecutar cargas con recursos de proceso adecuados, cree usuarios de carga designados para ejecutar cargas. Asigne cada usuario de carga a un grupo de cargas de trabajo o una clase de recurso específicos. Para ejecutar una carga, inicie sesión como uno de los usuarios de carga y, a continuación, ejecute la carga. La carga se ejecuta con la clase de recurso del usuario.  

> [!NOTE]
> Este método es más sencillo que intentar cambiar la clase de recursos de un usuario para que se ajuste a la necesidad actual de clase de recurso.

### <a name="example-of-creating-a-loading-user"></a>Ejemplo de creación de un usuario de carga

En este ejemplo se crea un usuario de carga para la clase de recurso staticrc20. El primer paso consiste en **conectarse al servidor principal** y crear un inicio de sesión.

```sql
   -- Connect to master
   CREATE LOGIN LoaderRC20 WITH PASSWORD = 'a123STRONGpassword!';
```

Conéctese al grupo de SQL y cree un usuario. El código siguiente da por supuesto que está conectado a la base de datos llamada mySampleDataWarehouse. Muestra cómo crear un usuario llamado LoaderRC20 y concederle el permiso de control de usuario en una base de datos. A continuación, agrega el usuario como miembro del rol de base de datos staticrc20.  

```sql
   -- Connect to the database
   CREATE USER LoaderRC20 FOR LOGIN LoaderRC20;
   GRANT CONTROL ON DATABASE::[mySampleDataWarehouse] to LoaderRC20;
   EXEC sp_addrolemember 'staticrc20', 'LoaderRC20';
```

Para ejecutar una carga con recursos de las clases de recursos staticRC20, inicie sesión como LoaderRC20 y ejecute la carga.

Ejecute cargas en clases de recursos estáticas en lugar de dinámicas. El uso de clases de recursos estáticas garantiza los mismos recursos independientemente de las [unidades de almacenamiento de datos](what-is-a-data-warehouse-unit-dwu-cdwu.md). Si utiliza una clase de recursos dinámica, los recursos varían según el nivel de servicio.

Para las clases dinámicas, un nivel de servicio inferior significa que probablemente necesita usar una clase de recursos más grande para el usuario de carga.

## <a name="allowing-multiple-users-to-load"></a>Posibilidad de que varios usuarios realicen cargas

A menudo, es necesario que varios usuarios puedan cargar datos en un grupo de SQL. La carga con [CREATE TABLE AS SELECT (Transact-SQL)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) requiere permisos de CONTROL en la base de datos.  El permiso CONTROL ofrece control de acceso a todos los esquemas.

Probablemente no desee que todos los usuarios de carga tengan control de acceso en todos los esquemas. Para limitar los permisos, use la instrucción DENY CONTROL.

Por ejemplo: tenemos los esquemas de base de datos schema_A para el departamento A, schema_B para el departamento B y los usuarios de PolyBase user_A y user_B con cargas en los departamentos A y B respectivamente. A ambos se les ha concedido permiso de CONTROL sobre la base de datos. Ahora, los creadores de los esquemas A y B bloquean dichos esquemas utilizando DENY:

```sql
   DENY CONTROL ON SCHEMA :: schema_A TO user_B;
   DENY CONTROL ON SCHEMA :: schema_B TO user_A;
```

User_A y user_B estarán ahora bloqueados en el esquema de los otros departamentos.

## <a name="loading-to-a-staging-table"></a>Carga en una tabla de almacenamiento provisional

Para lograr la velocidad de carga más rápida para mover datos a una tabla del grupo de SQL, cargue los datos en una tabla de almacenamiento provisional.  Definir la tabla de almacenamiento provisional como un montón y usar round robin para la opción de distribución.

Tenga en cuenta que la carga suele ser un proceso de dos pasos en el que primero se realiza la carga en una tabla de almacenamiento provisional y, luego, se insertan los datos en una tabla del grupo de SQL de producción. Si la tabla de producción utiliza una distribución hash, el tiempo total para cargar e insertar puede ser más rápido si define la tabla de almacenamiento provisional con la distribución hash.

Cargar la tabla de almacenamiento provisional lleva más tiempo, pero el segundo paso de insertar las filas en la tabla de producción no incurre en movimiento de datos a través de las distribuciones.

## <a name="loading-to-a-columnstore-index"></a>Carga en un índice de almacén de columnas

Los índices de almacén de columnas necesitan mucha memoria para comprimir los datos en grupos de filas de alta calidad. Para una mejor compresión y eficacia del índice, el índice de almacén de columnas tiene que comprimir el máximo de 1.048.576 filas en cada grupo de filas.

Si no hay memoria suficiente, es posible que el índice de almacén de columnas no pueda lograr las tasas de compresión máximas. Este escenario afecta, a su vez, al rendimiento de las consultas. Para un análisis detallado, consulte [Maximización de la calidad del grupo de filas del almacén de columnas](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

- Para asegurarse de que el usuario de carga tenga suficiente memoria para lograr la tasa de compresión máxima, utilice usuarios de carga que sean miembros de una clase de recursos mediana o grande.
- Cargue filas suficientes para rellenar completamente los nuevos grupos de filas. Durante una carga masiva, cada 1.048.576 filas se comprime directamente en el almacén de columnas como un grupo de filas completo. Las cargas con menos de 102.400 filas envían las filas al almacén delta, donde se mantienen las filas en un índice de árbol b.

> [!NOTE]
> Si carga muy pocas filas, puede que pasen todas al almacén delta y que no se compriman inmediatamente con el formato de almacén de columnas.

## <a name="increase-batch-size-when-using-sqlbulkcopy-api-or-bcp"></a>Aumento del tamaño de lote al usar SqLBulkCopy API o bcp

La carga con PolyBase proporcionará el mayor rendimiento con el grupo de SQL. Si no puede usar PolyBase para la carga y debe usar [SqLBulkCopy API](/dotnet/api/system.data.sqlclient.sqlbulkcopy?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) o [bcp](/sql/tools/bcp-utility?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), considere la posibilidad de aumentar el tamaño del lote para mejorar el rendimiento.

> [!TIP]
> Un tamaño de lote entre 100 000 filas y 1 millón de filas es la línea de base recomendada para determinar la capacidad de tamaño de lote óptima.

## <a name="handling-loading-failures"></a>Control de errores de carga

Una carga que utiliza una tabla externa puede producir el error *"Consulta anulada: se alcanzó el umbral de rechazo máximo al leer desde un origen externo"* . Este mensaje indica que sus datos externos contienen registros con modificaciones.

Se considera que un registro de datos tiene modificaciones si cumple una de las condiciones siguientes:

- Los tipos de datos y el número de columnas no coinciden con las definiciones de columna de la tabla externa.
- Los datos no se ajustan al formato de archivo externo especificado.

Para corregir estos registros, asegúrese de que la tabla externa y las definiciones de formato de archivo externos son correctas y que los datos externos se ajustan a estas definiciones.

Si un subconjunto de registros de datos externos contiene modificaciones, puede elegir rechazar estos registros para sus consultas mediante las opciones de rechazo en [CREATE EXTERNAL TABLE (Transact-SQL)](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="inserting-data-into-a-production-table"></a>Inserción de datos en una tabla de producción

Una tabla pequeña se puede cargar una sola vez con una [instrucción INSERT](/sql/t-sql/statements/insert-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), o incluso una recarga periódica de una búsqueda puede funcionar bien con una instrucción del tipo `INSERT INTO MyLookup VALUES (1, 'Type 1')`.  Sin embargo las inserciones simples no son tan eficaces como para realizar una carga masiva.

Si se tienen miles de inserciones simples a lo largo del día, agrúpelas por lotes para que pueda cargarlas masivamente.  Desarrolle los procesos para anexar las inserciones sencillas a un archivo y, a continuación, cree otro proceso que cargue el archivo de forma periódica.

## <a name="creating-statistics-after-the-load"></a>Creación de estadísticas después de la carga

Para mejorar el rendimiento de las consultas, es importante crear estadísticas de todas las columnas de todas las tablas después de la primera carga, o bien después de que se realicen cambios importantes en los datos. La creación de estadísticas se puede realizar manualmente o bien puede habilitar [AUTO_CREATE_STATISTICS](sql-data-warehouse-tables-statistics.md#automatic-creation-of-statistic).

Para ver una explicación detallada de las estadísticas, consulte [Estadísticas](sql-data-warehouse-tables-statistics.md). En el ejemplo siguiente se muestra cómo crear manualmente las estadísticas de cinco columnas de la tabla Customer_Speed.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="rotate-storage-keys"></a>Rotación de claves de almacenamiento

Es un buen procedimiento de seguridad cambiar la clave de acceso al almacenamiento de blobs de forma regular. Tiene dos claves de almacenamiento para la cuenta de Blob Storage, lo que le permite la transición de las claves.

Para rotar las cuentas de Azure Storage:

Para cada cuenta de almacenamiento cuya clave haya cambiado, ejecute [ALTER DATABASE SCOPED CREDENTIAL](/sql/t-sql/statements/alter-database-scoped-credential-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

Ejemplo:

Se crea la clave original

```sql
CREATE DATABASE SCOPED CREDENTIAL my_credential WITH IDENTITY = 'my_identity', SECRET = 'key1'
```

Se cambia de la clave 1 a la clave 2.

```sql
ALTER DATABASE SCOPED CREDENTIAL my_credential WITH IDENTITY = 'my_identity', SECRET = 'key2'
```

No es necesario cambiar nada más en los orígenes de datos externos subyacentes.

## <a name="next-steps"></a>Pasos siguientes

- Para más información acerca de PolyBase y del diseño de un proceso de extracción, carga y transformación (ETL), consulte [Design ELT for SQL Data Warehouse](design-elt-data-loading.md) (Diseño de ELT para SQL Data Warehouse).
- Si desea un tutorial sobre carga, consulte [Uso de PolyBase para cargar de datos de Azure Blob Storage en Azure SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md).
- Para supervisar las cargas de datos, consulte [Supervisión de la carga de trabajo mediante DMV](sql-data-warehouse-manage-monitor.md).
