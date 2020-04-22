---
title: Autoayuda sobre SQL a petición (versión preliminar)
description: Esta sección contiene información que puede ayudarle a solucionar problemas con SQL a petición (versión preliminar).
services: synapse analytics
author: vvasic-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: vvasic
ms.reviewer: jrasnick
ms.openlocfilehash: e2c262915c928cf487cb84aeb3423d67e7a96e97
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81421199"
---
# <a name="self-help-for-sql-on-demand-preview"></a>Autoayuda para SQL a petición (versión preliminar)

Este artículo contiene información sobre cómo solucionar los problemas más frecuentes con SQL a petición (versión preliminar) en Azure Synapse Analytics.

## <a name="sql-on-demand-is-grayed-out-in-synapse-studio"></a>SQL a petición atenuado en Synapse Studio

Si Synapse Studio no puede establecer la conexión a SQL a petición, aparecerá atenuado o con el estado "Sin conexión". Normalmente, este problema se da cuando se produce uno de los siguientes casos:

1) La red impide la comunicación con el back-end de Azure Synapse. El caso más frecuente es que el puerto 1443 está bloqueado. Para conseguir que SQL a petición funcione, desbloquee este puerto. Otros problemas podrían impedir que SQL a petición funcione también, [visite la guía de solución de problemas completa para más información](../troubleshoot/troubleshoot-synapse-studio.md).
2) No tiene permisos para iniciar sesión en SQL a petición. Para obtener acceso, uno de los administradores del área de trabajo de Azure Synapse debe agregarle al administrador de áreas de trabajo o al rol de administrador de SQL. [Visite guía completa sobre el control de acceso para más información](access-control.md).

## <a name="query-fails-because-file-cannot-be-opened"></a>Se produce un error en la consulta porque no se puede abrir el archivo

Si se produce un error en la consulta y se indica que no se puede abrir el archivo porque no existe o porque lo está usando otro proceso, y está seguro de que el archivo existe y que no lo usa ningún otro proceso, SQL a petición no podrá acceder al archivo. Este problema suele ocurrir porque su identidad de Azure Active Directory no tiene derechos de acceso al archivo. De forma predeterminada, SQL a petición intenta acceder al archivo mediante su identidad de Azure Active Directory. Para resolver este problema, debe tener los derechos adecuados para acceder al archivo. La manera más fácil es concederse el rol "Colaborador de datos de blobs de almacenamiento" en la cuenta de almacenamiento que está intentando consultar. [Visite la guía completa sobre el control de acceso de Azure Active Directory para el almacenamiento para más información](../../storage/common/storage-auth-aad-rbac-portal.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json). 

## <a name="query-fails-because-it-cannot-be-executed-due-to-current-resource-constraints"></a>Se produce un error en la consulta porque no se puede ejecutar debido a las restricciones de recursos actuales 

Si se produce un error en la consulta con el mensaje "no se puede ejecutar esta consulta debido a las restricciones de recursos actuales", significa que SQL a petición no puede ejecutarla en este momento debido a restricciones de recursos: 

- Asegúrese de que se usan los tipos de datos de tamaños razonables. Además, especifique el esquema para los archivos Parquet para las columnas de cadenas, ya que serán VARCHAR(8000) de forma predeterminada. 

- Si la consulta se dirige a archivos CSV, considere la posibilidad de [crear estadísticas](develop-tables-statistics.md#statistics-in-sql-on-demand-preview). 

- Visite los [procedimientos recomendados de rendimiento para SQL a petición](best-practices-sql-on-demand.md) para optimizar la consulta.  

## <a name="next-steps"></a>Pasos siguientes

Consulte los artículos siguientes para aprender a usar SQL a petición:

- [Consulta de archivos CSV](query-single-csv-file.md)

- [Consulta de carpetas y varios archivos CSV](query-folders-multiple-csv-files.md)

- [Consulta de archivos específicos](query-specific-files.md)

- [Consulta de archivos Parquet](query-parquet-files.md)

- [Consulta de tipos anidados de Parquet](query-parquet-nested-types.md)

- [Consulta de archivos JSON](query-json-files.md)

- [Creación y uso de vistas](create-use-views.md)

- [Creación y uso de tablas externas](create-use-external-tables.md)

- [Almacenamiento de los resultados de las consultas en Storage](create-external-table-as-select.md)
