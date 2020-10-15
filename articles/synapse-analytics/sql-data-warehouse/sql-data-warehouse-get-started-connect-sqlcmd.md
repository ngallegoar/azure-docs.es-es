---
title: Conexión con sqlcmd
description: Use la utilidad de línea de comandos sqlcmd para conectarse a un grupo de SQL de Synapse y consultarlo.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 2e76f48466d084d448cceea0490e44041e7d062e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85212146"
---
# <a name="connect-to-synapse-sql-pool-with-sqlcmd"></a>Conexión al grupo de SQL de Synapse con sqlcmd

> [!div class="op_single_selector"]
>
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md)
> * [SSMS](sql-data-warehouse-query-ssms.md)

Use la utilidad de línea de comandos [sqlcmd][sqlcmd] para conectarse a un grupo de SQL y realizar consultas en él.  

## <a name="1-connect"></a>1. Conectar

Para empezar a trabajar con [sqlcmd][sqlcmd], abra el símbolo del sistema y escriba **sqlcmd** seguido de la cadena de conexión de la base de datos del grupo de SQL. La cadena de conexión requiere los siguientes parámetros:

* **Server (-S):** servidor con el formato `<`Nombre de servidor`>`.database.windows.net
* **Database (-d):** nombre de base de datos.
* **Enable Quoted Identifiers (-I):** Los identificadores entre comillas tienen que estar habilitados para poder conectarse a una instancia del grupo de SQL.

Para utilizar la autenticación de SQL Server, debe agregar los parámetros de nombre de usuario y contraseña:

* **User (-U):** usuario del servidor con el formato `<`Usuario`>`
* **Password (-P):** contraseña asociada con el usuario.

Por ejemplo, la cadena de conexión podría ser similar a la siguiente:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Para usar la autenticación integrada de Azure Active Directory, debe agregar los parámetros de Azure Active Directory:

* **Autenticación de Azure Active Directory (-G):** use Azure Active Directory para la autenticación.

Por ejemplo, la cadena de conexión podría ser similar a la siguiente:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [!NOTE]
> Necesita [habilitar la autenticación de Azure Active Directory](sql-data-warehouse-authentication.md) para autenticarse con Active Directory.

## <a name="2-query"></a>2. Consultar

Después de la conexión, puede emitir cualquier instrucción Transact-SQL en la instancia.  En este ejemplo, las consultas se envían en modo interactivo.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Los siguientes ejemplos muestran cómo se pueden ejecutar las consultas en el modo por lotes con la opción -Q o mediante la canalización de su SQL a sqlcmd.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las opciones disponibles en sqlcmd, consulte la [documentación de sqlcmd](/sql/tools/sqlcmd-utility?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).
