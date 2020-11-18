---
title: De DB2 a SQL Database (guía de migración)
description: Siga esta guía para migrar las bases de datos de DB2 a Azure SQL Database.
ms.service: sql-database
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: mokabiru
ms.author: mokabiru
ms.reviewer: MashaMSFT
ms.date: 11/06/2020
ms.openlocfilehash: bc7db167ed1d1d8823e90bf422f17428a7ed4e48
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94496357"
---
# <a name="migration-guide-db2-to-sql-database"></a>Guía de migración: De DB2 a SQL Database
[!INCLUDE[appliesto-sqldb-sqlmi](../../includes/appliesto-sqldb.md)]

En esta guía se explica cómo migrar las bases de datos de DB2 a Azure SQL Database mediante SQL Server Migration Assistant para DB2. 

Para ver otros escenarios, consulte la [Guía de migración de bases de datos](https://datamigration.microsoft.com/).

## <a name="prerequisites"></a>Requisitos previos 

Para migrar la base de datos de DB2 a SQL Database, necesita:

- Comprobar que el entorno de origen es compatible.
- Descargar [SQL Server Migration Assistant (SSMA) para DB2](https://www.microsoft.com/download/details.aspx?id=54254).
- Un instancia de [Azure SQL Database](../../database/single-database-create-quickstart.md) de destino.


## <a name="pre-migration"></a>Antes de la migración

Una vez cumplidos los requisitos previos, estará a punto para detectar la topología de su entorno y evaluar la viabilidad de la migración. 

### <a name="assess-and-convert"></a>Evaluación y conversión

Cree una evaluación mediante SQL Server Migration Assistant (SSMA). 

Para crear una evaluación, siga estos pasos:

1. Abra SQL Server Migration Assistant (SSMA) para DB2. 
1. Seleccione **Archivo** y, luego, elija **Nuevo proyecto**. 
1. Facilite el nombre del proyecto y una ubicación para guardarlo y, luego, seleccione Azure SQL Database como destino de la migración en la lista desplegable. Seleccione **Aceptar**.  

   :::image type="content" source="media/db2-to-sql-database-guide/new-project.png" alt-text="Especifique los detalles del proyecto y seleccione Aceptar para guardarlos.":::


1. Escriba los valores de los detalles de la conexión de DB2 en el cuadro de diálogo **Conectarse a DB2**. 

   :::image type="content" source="media/db2-to-sql-database-guide/connect-to-db2.png" alt-text="Conexión a la instancia de DB2":::


1. Haga clic con el botón derecho en el esquema de DB2 que quiera migrar y, luego, elija **Crear informe**. Se generará un informe HTML. También puede elegir **Crear informe** en la barra de navegación después de seleccionar el esquema. 

   :::image type="content" source="media/db2-to-sql-database-guide/create-report.png" alt-text="Haga clic con el botón derecho en el esquema y elija Crear informe.":::

1. Revise el informe en HTML para comprender las estadísticas de conversión, y los errores o advertencias. También puede abrir el informe en Excel para obtener un inventario de objetos DB2 y conocer el esfuerzo necesario para realizar las conversiones de esquema. La ubicación predeterminada del informe es la carpeta de informes dentro de SSMAProjects.

   Por ejemplo: `drive:\<username>\Documents\SSMAProjects\MyDB2Migration\report\report_<date>`. 

   :::image type="content" source="media/db2-to-sql-database-guide/report.png" alt-text="Revise el informe para identificar los errores o advertencias.":::


### <a name="validate-data-types"></a>Validación de los tipos de datos

Valide las asignaciones predeterminadas de los tipos de datos y cámbielas según los requisitos, si es necesario. Para hacerlo, siga estos pasos: 

1. Seleccione **Herramientas** en el menú principal. 
1. Seleccione **Configuración del proyecto**. 
1. Seleccione la pestaña **Type mappings** (Asignaciones de tipos). 

   :::image type="content" source="media/db2-to-sql-database-guide/type-mapping.png" alt-text="Seleccione el esquema y la asignación de tipos.":::

1. Puede cambiar la asignación de tipos de cada tabla seleccionando la tabla en el **Explorador de metadatos de DB2**. 

### <a name="schema-conversion"></a>Conversión del esquema 

Para convertir el esquema, siga estos pasos:

1. (Opcional) Agregue consultas dinámicas o ad hoc a las instrucciones. Haga clic con el botón derecho en el nodo y elija **Add statements** (Agregar instrucciones). 
1. Seleccione **Conectar a Azure SQL Database**. 
    1. Escriba los detalles de la conexión para conectar la base de datos en Azure SQL Database. 
    1. Elija la instancia de SQL Database de destino en la lista desplegable. 
    1. Seleccione **Conectar**. 

   :::image type="content" source="media/db2-to-sql-database-guide/connect-to-sql-database.png" alt-text="Rellenar los detalles para conectarse al servidor lógico en Azure":::


1. Haga clic con el botón derecho en el esquema y elija **Convertir esquema**. También puede elegir **Convertir esquema** en la barra de navegación superior después de seleccionar el esquema. 

   :::image type="content" source="media/db2-to-sql-database-guide/convert-schema.png" alt-text="Haga clic con el botón derecho en el esquema y elija Convertir esquema.":::

1. Una vez finalizada la conversión, compare y revise la estructura del esquema para identificar posibles problemas y solucionarlos en función de las recomendaciones. 

   :::image type="content" source="media/db2-to-sql-database-guide/compare-review-schema-structure.png" alt-text="Compare y revise la estructura del esquema para identificar posibles problemas y solucionarlos en función de las recomendaciones.":::

1. Guarde el proyecto localmente para un ejercicio de corrección de esquema sin conexión. Seleccione **Guardar proyecto** en el menú **Archivo**. 


## <a name="migrate"></a>Migrar

Cuando haya terminado de evaluar las bases de datos y de resolver las discrepancias, el siguiente paso consiste en ejecutar el proceso de migración.

Para publicar el esquema y migrar los datos, siga estos pasos:

1. Publique el esquema: haga clic con el botón derecho en la base de datos desde el nodo **Bases de datos** del **Explorador de metadatos de Azure SQL Database** y elija **Sincronizar con base de datos**.

   :::image type="content" source="media/db2-to-sql-database-guide/synchronize-with-database.png" alt-text="Haga clic con el botón derecho en la base de datos y elija Sincronizar con base de datos.":::

1. Migre los datos: haga clic con el botón derecho en el **Explorador de metadatos de DB2** y elija **Migrar datos**. 

   :::image type="content" source="media/db2-to-sql-database-guide/migrate-data.png" alt-text="Haga clic con el botón derecho en el esquema y elija Migrar datos":::

1. Especifique los detalles de la conexión para las instancias de DB2 y Azure SQL Database. 
1. Vea el **Informe de migración de datos**. 

   :::image type="content" source="media/db2-to-sql-database-guide/data-migration-report.png" alt-text="Revise el informe de migración de datos.":::

1. Conéctese a la instancia de Azure SQL Database mediante SQL Server Management Studio, y revise los datos y el esquema para validar la migración. 

   :::image type="content" source="media/db2-to-sql-database-guide/compare-schema-in-ssms.png" alt-text="Compare el esquema en SSMS.":::

## <a name="post-migration"></a>Después de la migración 

Cuando haya completado correctamente la fase de migración, deberá realizar una serie de tareas posteriores para asegurarse de que todo funcione de la forma más fluida y eficaz posible.

### <a name="remediate-applications"></a>Corrección de las aplicaciones 

Cuando se hayan migrado los datos al entorno de destino, todas las aplicaciones que antes utilizaban el origen deben empezar a utilizar el destino. Lograr esto requerirá en algunos casos realizar cambios en las aplicaciones.


### <a name="perform-tests"></a>Realización de pruebas

El método de prueba para la migración de bases de datos consta de las siguientes actividades:

1. **Desarrollar pruebas de validación**: para probar la migración de bases de datos, debe utilizar consultas SQL. Debe crear las consultas de validación para que se ejecuten en las bases de datos de origen y destino. Las consultas de validación deben abarcar el ámbito definido.
1. **Configurar un entorno de prueba**: el entorno de prueba debe contener una copia de la base de datos de origen y la base de datos de destino. Asegúrese de aislar el entorno de prueba.
1. **Ejecutar pruebas de validación**: ejecute las pruebas de validación en el origen y el destino y, luego, analice los resultados.
1. **Ejecutar pruebas de rendimiento**: ejecute la prueba de rendimiento en el origen y el destino y, luego, analice y compare los resultados.

   > [!NOTE]
   > Para obtener ayuda sobre el desarrollo y la ejecución de pruebas de validación tras la migración, tenga en cuenta la solución de calidad de datos [QuerySurge](https://www.querysurge.com/company/partners/microsoft). 


## <a name="leverage-advanced-features"></a>Aprovechamiento de las características avanzadas 

Asegúrese de aprovechar las características avanzadas basadas en la nube que ofrece SQL Database, como las de [alta disponibilidad integrada](../../database/high-availability-sla.md), [detección de amenazas](../../database/advanced-data-security.md) y [supervisión y ajuste de la carga de trabajo](../../database/monitor-tune-overview.md). 


Algunas características de SQL Server solo están disponibles cuando el [nivel de compatibilidad de la base de datos](/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database) cambia al nivel de compatibilidad más reciente (150). 

## <a name="migration-assets"></a>Recursos de migración 

Para obtener más ayuda, consulte los siguientes recursos, que se desarrollaron como apoyo de la participación en un proyecto de migración en el mundo real:

|Recurso  |Descripción  |
|---------|---------|
|[Herramienta y modelo de evaluación de la carga de trabajo de datos](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool)| Esta herramienta proporciona las plataformas de destino de ajuste perfecto sugeridas, la preparación para la nube, y el nivel de corrección de la aplicación o base de datos para una carga de trabajo determinada. Ofrece un cálculo sencillo con un solo clic y una función de generación de informes que ayuda a acelerar las evaluaciones de grandes volúmenes, ya que proporciona un proceso de toma de decisiones de plataforma de destino uniforme y automatizado.|
|[Paquete de detección y evaluación de recursos de datos de DB2 zOS](https://github.com/Microsoft/DataMigrationTeam/tree/master/DB2%20zOS%20Data%20Assets%20Discovery%20and%20Assessment%20Package)|Después de ejecutar el script de SQL en una base de datos, puede exportar los resultados a un archivo en el sistema de archivos. Se admiten varios formatos de archivo, incluido *.csv, para que se puedan capturar los resultados en herramientas externas, como hojas de cálculo. Este método puede resultar útil si quiere compartir fácilmente los resultados con equipos que no tengan el área de trabajo instalada.|
|[Artefactos y scripts de inventario de IBM DB2 LUW](https://github.com/Microsoft/DataMigrationTeam/tree/master/IBM%20DB2%20LUW%20Inventory%20Scripts%20and%20Artifacts)|Este recurso incluye una consulta SQL que accede a las tablas del sistema IBM DB2 LUW, versión 11.1, y proporciona un recuento de objetos por esquema y tipo de objeto, una estimación aproximada de datos sin procesar en cada esquema y el tamaño de las tablas de cada esquema, cuyos resultados se almacenan en formato CSV.|
|[Escalado puro de DB2 LUW en Azure: guía de instalación](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/DB2%20PureScale%20on%20Azure.pdf)|Esta guía sirve como punto de partida para un plan de implementación de DB2. Aunque sus requisitos empresariales varíen, se aplica el mismo patrón básico. Este patrón de arquitectura también se puede utilizar para aplicaciones OLAP en Azure.|

Estos recursos se desarrollaron como parte del programa Ninja SQL de datos, que está patrocinado por el equipo de ingeniería de grupos de datos de Azure. El objetivo principal del programa Ninja SQL de datos es lograr y acelerar la modernización compleja, y competir por las oportunidades de migración de las plataformas de datos a la de Azure, de Microsoft. Si cree que su organización estaría interesada en participar en el programa Ninja SQL de datos, póngase en contacto con su equipo de cuentas y pídale que le envíe una nominación.



## <a name="next-steps"></a>Pasos siguientes

- Para obtener una matriz de los servicios y las herramientas de Microsoft y de otros fabricantes que están disponibles para ayudarlo en diversos escenarios de migración de datos y bases de datos, además de las tareas especializadas, consulte [Servicios y herramientas de migración de datos](../../../dms/dms-tools-matrix.md).

- Para más información acerca de Azure SQL Database, consulte:
   - [Información general sobre SQL Database](../../database/sql-database-paas-overview.md)
   - [Calculadora del costo total de propiedad de Azure](https://azure.microsoft.com/pricing/tco/calculator/) 


- Para obtener más información sobre el marco y el ciclo de adopción de las migraciones en la nube, consulte:
   -  [Cloud Adoption Framework para Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [Procedimientos recomendados para la gestión de los costos y los ajustes de tamaño de las cargas de trabajo migradas a Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 

- Para evaluar el nivel de acceso de la aplicación, consulte [Data Access Migration Toolkit (versión preliminar)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)
- Para obtener más información sobre cómo realizar pruebas A/B de nivel de acceso a datos, consulte [Asistente para experimentación con bases de datos](/sql/dea/database-experimentation-assistant-overview).
