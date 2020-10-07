---
title: 'Supervisión de la actividad de migración: Azure Database Migration Service'
description: Aprenda a usar Azure Database Migration Service para supervisar la actividad de migración.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: how-to
ms.date: 02/20/2020
ms.openlocfilehash: dc9123735ef65bec89e99a14d25cba881be306fe
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91297477"
---
# <a name="monitor-migration-activity-using-the-azure-database-migration-service"></a>Supervisión de la actividad de migración con Azure Database Migration Service
En este artículo, aprenderá a supervisar el progreso de una migración tanto de nivel de base de datos como de tabla.

## <a name="monitor-at-the-database-level"></a>Supervisión de nivel de base de datos
Para supervisar la actividad de nivel de base de datos, consulte la hoja de nivel de base de datos:

![Hoja de nivel de base de datos](media/how-to-monitor-migration-activity/dms-database-level-blade.png)

> [!NOTE]
> Al seleccionar el hipervínculo de la base de datos, se mostrará la lista de tablas y el progreso de su migración.

En la tabla siguiente se enumeran los campos en la hoja de nivel de base de datos y se describen los distintos valores de estado asociados a cada uno.

<table id='overview' class='overview'>
  <thead>
    <tr>
      <th class="x-hidden-focus"><strong>Nombre del campo</strong></th>
      <th><strong>Subestado del campo</strong></th>
      <th><strong>Descripción</strong></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="3" class="ActivityStatus"><strong>Estado de la actividad</strong></td>
      <td>En ejecución</td>
      <td>La actividad de migración se está ejecutando.</td>
    </tr>
    <tr>
      <td>Correcto</td>
      <td>Se ha realizado la actividad de migración sin problemas.</td>
    </tr>
    <tr>
      <td>Error</td>
      <td>Error de migración. Seleccione el vínculo "Ver detalles del error" en los detalles de la migración para ver el mensaje de error completo.</td>
    </tr>
    <tr>
      <td rowspan="4" class="Status"><strong>Estado</strong></td>
      <td>Inicializando</td>
      <td>DMS está configurando la canalización de migración.</td>
    </tr>
    <tr>
      <td>En ejecución</td>
      <td>La canalización de DMS está ejecutándose y realizando la migración.</td>
    </tr>
    <tr>
      <td>Operación completada</td>
      <td>Migración completada.</td>
    </tr>
    <tr>
      <td>Con error</td>
      <td>Error de migración. Haga clic en los detalles de la migración para ver los errores de migración.</td>
    </tr>
    <tr>
      <td rowspan="5" class="migration-details"><strong>Detalles de la migración</strong></td>
      <td>Iniciando la canalización de migración</td>
      <td>DMS está configurando la canalización de migración.</td>
    </tr>
    <tr>
      <td>Carga completa de los datos en curso</td>
      <td>DMS está realizando la carga inicial.</td>
    </tr>
    <tr>
      <td>Listo para la migración total</td>
      <td>Una vez completada la carga inicial, DMS marcará la base de datos e indicará que está lista para la migración total. El usuario debe comprobar si los datos han alcanzado la sincronización continua.</td>
    </tr>
    <tr>
      <td>Todos los cambios aplicados</td>
      <td>La carga inicial y la sincronización continua se han completado. Este estado también se produce después de que la base de datos haya migrado totalmente de forma correcta.</td>
    </tr>
    <tr>
      <td>Ver detalles del error</td>
      <td>Haga clic en el vínculo para mostrar detalles del error.</td>
    </tr>
    <tr>
      <td rowspan="1" class="duration"><strong>Duration</strong></td>
      <td>N/D</td>
      <td>Tiempo total desde que se inicia la actividad de migración hasta que se completa o se produce un error de migración.</td>
    </tr>
     </tbody>
</table>

## <a name="monitor-at-table-level--quick-summary"></a>Supervisión en el nivel de tabla: resumen rápido
Para supervisar la actividad de nivel de tabla, consulte la hoja de nivel de tabla. La parte superior de la hoja muestra el número detallado de filas que han migrado en carga completa y las actualizaciones incrementales. 

La parte inferior de la hoja enumera las tablas y muestra un resumen rápido del progreso de la migración.

![Hoja de nivel de tabla: resumen rápido](media/how-to-monitor-migration-activity/dms-table-level-blade-summary.png)

En la tabla siguiente se describen los campos que se muestran en los detalles de nivel de tabla.

| Nombre del campo        | Descripción       |
| ------------- | ------------- |
| **Carga completa efectuada**      | Número de tablas que han finalizado la carga completa de datos. |
| **Carga completa en cola**      | Número de tablas que se están poniendo en cola para carga completa.      |
| **Carga completa en curso** | Número de tablas con error.      |
| **Actualizaciones incrementales**      | Número de actualizaciones de captura de datos modificados (CDC) en las filas, que se aplican al destino. |
| **Inserciones incrementales**      | Número de inserciones de CDC en las filas, que se aplican al destino.      |
| **Eliminaciones incrementales** | Número de eliminaciones de CDC en las filas, que se aplican al destino.      |
| **Cambios pendientes**      | Número de CDC en las filas, que aún están esperando a que se apliquen al destino. |
| **Cambios aplicados**      | Total de actualizaciones, inserciones y eliminaciones de CDC en las filas, que se aplican al destino.      |
| **Tablas en estado de error** | Número de tablas que se encuentran en estado de "error" durante la migración. Algunos ejemplos de tablas que generan error son cuando existen duplicados identificados en el destino o los datos no son compatibles al cargarlos en la tabla de destino.      |

## <a name="monitor-at-table-level--detailed-summary"></a>Supervisión en el nivel de tabla: resumen detallado
Hay dos pestañas que muestran el progreso de la migración en la carga completa y la sincronización de datos incrementales.
    
![Pestaña de la carga completa](media/how-to-monitor-migration-activity/dms-full-load-tab.png)

![Pestaña de sincronización de datos incrementales](media/how-to-monitor-migration-activity/dms-incremental-data-sync-tab.png)

En la tabla siguiente se describen los campos que se muestran en el progreso de la migración de nivel de tabla.

| Nombre del campo        | Descripción       |
| ------------- | ------------- |
| **Estado - Sincronizándose**      | Se está ejecutando la sincronización continua. |
| **Insertar**      | Número de inserciones de CDC en las filas, que se aplican al destino.      |
| **Actualizar** | Número de actualizaciones de CDC en las filas, que se aplican al destino.      |
| **Eliminar**      | Número de eliminaciones de CDC en las filas, que se aplican al destino. |
| **Total aplicado**      | Total de actualizaciones, inserciones y eliminaciones de CDC en las filas, que se aplican al destino. |
| **Errores de datos** | Número de errores de datos que se han producido en esta tabla. Algunos ejemplos de los errores son *511: Cannot create a row of size %d which is greater than the allowable maximum row size of %d, 8114: Error converting data type %ls to %ls.* (511: No se puede crear una fila de tamaño %d que sea mayor que el tamaño de fila máximo permitido de %d, 8114: Error al convertir el tipo de datos de %ls a %ls.)  El cliente debe consultar desde la tabla dms_apply_exceptions en el destino de Azure para ver los detalles del error.    |

> [!NOTE]
> Los valores de inserción, actualización, eliminación y total aplicado de CDC pueden disminuir cuando la base de datos migra totalmente o se reinicia la migración.

## <a name="next-steps"></a>Pasos siguientes
- Revise las directrices de migración en la [Guía de migración de bases de datos](https://datamigration.microsoft.com/).