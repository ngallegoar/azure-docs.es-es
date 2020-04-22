---
title: Modelo de metadatos compartidos de Azure Synapse Analytics
description: Azure Synapse Analytics permite que los diferentes motores de cálculo de áreas de trabajo compartan bases de datos y tablas entre los grupos de Spark (versión preliminar), el motor de SQL a petición (versión preliminar) y los grupos de SQL.
services: synapse-analytics
author: MikeRys
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: mrys
ms.reviewer: jrasnick
ms.openlocfilehash: 3b26d516080961a482a3ba67f314e98ece4c9f24
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81420179"
---
# <a name="azure-synapse-analytics-shared-metadata"></a>Metadatos compartidos de Azure Synapse Analytics

Azure Synapse Analytics permite que los diferentes motores de cálculo de áreas de trabajo compartan bases de datos y tablas entre los grupos de Spark (versión preliminar), el motor de SQL a petición (versión preliminar) y los grupos de SQL.

[!INCLUDE [preview](../includes/note-preview.md)]



El uso compartido es compatible con el llamado modelo de almacenamiento de datos moderno y proporciona a los motores de SQL del área de trabajo acceso a las bases de datos y las tablas creadas con Spark. También permite que los motores de SQL creen sus propios objetos que no se comparten con los demás motores.

## <a name="support-the-modern-data-warehouse"></a>Compatibilidad con el almacenamiento de datos moderno

El modelo de metadatos compartidos admite el moderno patrón de almacenamiento de datos de la siguiente manera:

1. Los datos del lago de datos se preparan y estructuran de manera eficaz con Spark mediante el almacenamiento de los datos preparados en tablas respaldadas por Parquet (posiblemente particionadas) contenidas en posiblemente varias bases de datos.

2. Las bases de datos creadas por Spark y todas sus tablas se vuelven visibles en cualquiera de las instancias del grupo de Spark del área de trabajo de Azure Synapse y se pueden usar desde cualquiera de los trabajos de Spark. Esta funcionalidad está sujeta a los [permisos](#security-model-at-a-glance), dado que todos los grupos de Spark de un área de trabajo comparten el mismo almacén de metadatos del catálogo subyacente.

3. Las bases de datos creadas por Spark y sus tablas respaldadas por Parquet se vuelven visibles en el motor de SQL a petición del área de trabajo. Las [bases de datos](database.md) se crean automáticamente en los metadatos de SQL a petición, y las [tablas externas y administradas](table.md) creadas mediante un trabajo de Spark se hacen accesibles como tablas externas en los metadatos de SQL a petición en el esquema `dbo` de la base de datos correspondiente. <!--For more details, see [ADD LINK].-->

4. Si hay instancias del grupo de SQL en el área de trabajo que tienen habilitada la sincronización de metadatos, <!--[ADD LINK]--> o, si se crea una instancia de grupo de SQL con la sincronización de metadatos habilitada, las bases de datos creadas por Spark y sus tablas respaldadas por Parquet se asignan automáticamente a la base de datos del grupo de SQL, tal como se describe en [Base de datos compartida de Azure Synapse Analytics](database.md).

<!--[INSERT PICTURE]-->

<!--__Figure 1 -__ Supporting the Modern Data Warehouse Pattern with shared metadata-->

La sincronización de objetos se produce de forma asincrónica. Los objetos tendrán un ligero retraso de unos segundos hasta que aparezcan en el contexto de SQL. Una vez que aparecen, los motores de SQL que tienen acceso a ellos pueden consultarlos, pero no actualizarlos ni cambiarlos.

## <a name="which-metadata-objects-are-shared"></a>Qué objetos de metadatos se comparten

Spark permite crear bases de datos, tablas externas, tablas administradas y vistas. Dado que las vistas de Spark requieren que un motor de Spark procese la instrucción SQL de Spark definitoria y no se pueden procesar con un motor SQL, solo las bases de datos y las tablas externas y administradas que contiene que usan el formato de almacenamiento de Parquet se comparten con los motores SQL del área de trabajo. Las vistas de Spark solo se comparten entre las instancias de grupo de Spark.

## <a name="security-model-at-a-glance"></a>Modelo de seguridad de un vistazo

Las bases de datos y las tablas de Spark, junto con sus representaciones sincronizadas en los motores de SQL, están protegidas en el nivel de almacenamiento subyacente. Cuando la tabla se consulta mediante alguno de los motores que el emisor de consultas tiene derecho a usar, la entidad de seguridad del emisor de consultas se pasa a los archivos subyacentes. Los permisos se comprueban en el nivel del sistema de archivos.

Para más información, consulte [Base de datos compartida de Azure Synapse Analytics](database.md).

## <a name="change-maintenance"></a>Cambio del mantenimiento

Si se elimina o cambia un objeto de metadatos con Spark, los cambios se recogen y propagan al motor de SQL a petición y los grupos de SQL que tienen los objetos sincronizados. La sincronización es asincrónica y los cambios se reflejan en los motores SQL después de un corto retraso.

## <a name="next-steps"></a>Pasos siguientes

- [Más información sobre las bases de datos de metadatos compartidos de Azure Synapse Analytics](database.md)
- [Más información sobre las tablas de metadatos compartidos de Azure Synapse Analytics](table.md)

