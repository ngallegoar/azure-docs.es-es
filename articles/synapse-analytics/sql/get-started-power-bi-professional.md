---
title: Conexión a Power BI Professional
description: En este tutorial, veremos los pasos para conectar Power BI Desktop a SQL a petición (versión preliminar).
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: tutorial
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 0ce8f3a447f1896ae6d96d343782f8cdb44d4c6f
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81425334"
---
# <a name="connect-to-synapse-sql-with-power-bi-professional"></a>Conexión a Synapse SQL con Power BI Professional

> [!div class="op_single_selector"]
>
> - [Azure Data Studio](get-started-azure-data-studio.md)
> - [Power BI](get-started-power-bi-professional.md)
> - [Visual Studio](../sql-data-warehouse/sql-data-warehouse-query-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
> - [sqlcmd](../sql/get-started-connect-sqlcmd.md)
> - [SSMS](get-started-ssms.md)

En este tutorial, veremos los pasos para conectar Power BI Desktop a SQL a petición (versión preliminar).

## <a name="prerequisites"></a>Prerrequisitos

Herramienta para emitir consultas:

- El cliente SQL que prefiera:

  - Azure Data Studio
  - SQL Server Management Studio

- Power BI Desktop instalado

Parámetros:

| Parámetro                                 | Descripción                                                   |
| ----------------------------------------- | ------------------------------------------------------------- |
| Dirección de un punto de conexión del servicio de SQL a petición.    | Se usará como nombre de servidor.                                   |
| Región de punto de conexión del servicio de SQL a petición.     | Se usará para determinar qué almacenamiento se utilizará en los ejemplos. |
| Nombre de usuario y contraseña para el acceso al punto de conexión. | Se usará para acceder al punto de conexión.                               |
| Base de datos que se va a usar para crear vistas.     | Esta base de datos se utilizará como punto de partida en los ejemplos.       |

## <a name="first-time-setup"></a>Primera configuración

Hay dos pasos antes de usar los ejemplos:

1. Crear la base de datos para las vistas
2. Crear las credenciales que usará SQL a petición para acceder a los archivos en el almacenamiento

### <a name="create-database"></a>Crear base de datos

Dado que va a usar usará el entorno de demostración, debe crear su propia base de datos para fines de demostración. La base de datos es necesaria para crear vistas en ella. Usará esta base de datos en algunas de las consultas de ejemplo de esta documentación.

> [!NOTE]
> Tenga en cuenta que las bases de datos se usan solo para los metadatos de las vistas, no para los datos reales.
>
> Anote el nombre de la base de datos que use; lo necesitará más adelante.

```sql
DROP DATABASE IF EXISTS demo;
```

### <a name="create-credentials"></a>Crear credenciales

Tenemos que crear una credencial para poder ejecutar consultas. El servicio de SQL a petición usará esta credencial para acceder a los archivos del almacenamiento.

> [!NOTE]
> Tenga en cuenta que debe crear credenciales para el acceso a la cuenta de almacenamiento. Aunque SQL a petición puede acceder a los almacenamientos desde distintas regiones, tener el almacenamiento y el área de trabajo de Azure Synapse en la misma región proporcionará una mejor experiencia de rendimiento.

**Fragmento de código sobre cómo crear credenciales para contenedores de datos de censo**, ejecute:

```sql
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer')
DROP CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer];
GO

-- Create credentials for Census Data container which resides in a azure open data storage account
-- There is no secret. We are using public storage account which doesn't need secret
CREATE CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]  
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = '';
GO
```

## <a name="creating-power-bi-desktop-report"></a>Creación de un informe de escritorio de Power BI

Abra la aplicación Power BI Desktop y seleccione la opción "Obtener datos".
![Abra la aplicación Power BI Desktop y seleccione Obtener datos.](./media/get-started-power-bi-professional/step-0-open-powerbi.png)

### <a name="step-1---select-data-source"></a>Paso 1: Selección del origen de datos

Seleccione "Azure" en el menú y, a continuación, "Azure SQL Database".
![Seleccione el origen de datos.](./media/get-started-power-bi-professional/step-1-select-data-source.png)

### <a name="step-2---select-database"></a>Paso 2: Selección de la base de datos

Escriba la dirección URL de la base de datos y el nombre de la base de datos donde resida la vista.
![Seleccione la base de datos en el punto de conexión.](./media/get-started-power-bi-professional/step-2-db.png)

## <a name="next-steps"></a>Pasos siguientes

Vaya a [Consulta de los archivos de almacenamiento](get-started-azure-data-studio.md) para aprender a conectarse a SQL a petición mediante Azure Data Studio.
 