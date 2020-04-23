---
title: Principales diferencias de Machine Learning Services (versión preliminar)
description: En este tema se describen las principales diferencias entre Azure SQL Database Machine Learning Services (con R) y SQL Server Machine Learning Services.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: carlrab
manager: cgronlun
ms.date: 11/20/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 34ba75b6126024c9cd43d6fe474f7c1b62dd990f
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81453156"
---
# <a name="key-differences-between-machine-learning-services-in-azure-sql-database-preview-and-sql-server"></a>Principales diferencias entre Machine Learning Services de Azure SQL Database (versión preliminar) y SQL Server

La funcionalidad de Azure SQL Database Machine Learning Services (con R) en (versión preliminar) es similar a [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning). A continuación se muestran algunas diferencias clave.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="language-support"></a>Compatibilidad con idiomas

SQL Server es compatible con R y Python gracias al [marco de extensibilidad](https://docs.microsoft.com/sql/advanced-analytics/concepts/extensibility-framework). SQL Database no admite ambos lenguajes. Las principales diferencias son:

- R es el único lenguaje que se admite en SQL Database. En este momento no hay ninguna compatibilidad con Python.
- La versión de R es la 3.4.4.
- No es necesario configurar `external scripts enabled` mediante `sp_configure`.

## <a name="package-management"></a>Administración de paquetes

La instalación y administración de paquetes de R funciona de forma distinta para SQL Database y SQL Server. Las diferencias son:

- Los paquetes de R se instalan a través de [sqlmlutils](https://github.com/Microsoft/sqlmlutils) o [CREATE EXTERNAL LIBRARY](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql).
- Los paquetes no pueden realizar llamadas de red salientes. Esta limitación es similar a las [reglas predeterminadas de firewall de Machine Learning Services](https://docs.microsoft.com//sql/advanced-analytics/security/firewall-configuration) en SQL Server, pero no se pueden cambiar en SQL Database.
- No hay ninguna compatibilidad con los paquetes que dependen de los entornos de ejecución externos (por ejemplo, Java) o que necesitan tener acceso a las API del sistema operativo para instalarse o usarse.

## <a name="writing-to-a-temporary-table"></a>Escritura en una tabla temporal

Si usa RODBC en Azure SQL Database, después no puede escribir en una tabla temporal, ya sea que se haya creado dentro o fuera de la sesión de `sp_execute_external_script`. La solución alternativa es usar [RxOdbcData](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxodbcdata) y [rxDataStep](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdatastep) (con overwrite=FALSE y append="rows") para escribir en una tabla temporal global creada antes de la consulta `sp_execute_external_script`.

## <a name="resource-governance"></a>Regulación de recursos

No es posible limitar los recursos de R a través de [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) ni de grupos de recursos externos.

Durante la versión preliminar pública, los recursos de R se establecen en un máximo de 20 % de los recursos de SQL Database y dependen del nivel de servicio que se elige. Para obtener más información, consulte [Azure SQL Database purchasing models](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers) (Modelos de compra de Azure SQL Database).
### <a name="insufficient-memory-error"></a>Error de memoria insuficiente

Si no hay memoria suficiente disponible para R, recibirá un mensaje de error. Los mensajes comunes de error son:

- Error de comunicación con el runtime del script de "R" para el identificador de solicitud: *******. Compruebe los requisitos del runtime de "R".
- Error de script de "R" ocurrido durante la ejecución de "sp_execute_external_script" con HRESULT 0x80004004. … error de script externo: "… no se pudo asignar memoria (0 Mb) en la función "R_AllocStringBuffer" de C".
- Error de script externo: Error: no se puede asignar el vector de tamaño.

El uso de la memoria depende de cuánta se use en los scripts de R y del número de consultas paralelas que se ejecutan. Si recibe los errores anteriores, puede escalar la base de datos a un nivel de servicio superior para resolver esto.

## <a name="next-steps"></a>Pasos siguientes

- Consulte la información general, [Azure SQL Database Machine Learning Services con R (versión preliminar)](sql-database-machine-learning-services-overview.md).
- Para obtener información sobre cómo usar R para consultar Machine Learning Services de Azure SQL Database (versión preliminar), consulte la [Guía de inicio rápido](sql-database-connect-query-r.md).
- Para empezar a trabajar con algunos scripts de R sencillos, consulte [Creación y ejecución de scripts de R sencillos en Machine Learning Services de Azure SQL Database (versión preliminar)](sql-database-quickstart-r-create-script.md).
