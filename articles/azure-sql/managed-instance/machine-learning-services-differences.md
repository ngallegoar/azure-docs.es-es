---
title: Principales diferencias de Machine Learning Services (versión preliminar)
description: En este tema se describen las principales diferencias entre Machine Learning Services de Instancia administrada de Azure SQL y Machine Learning Services de SQL Server.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.reviewer: carlrab, davidph
manager: cgronlun
ms.date: 05/27/2020
ms.openlocfilehash: 02211a09df2d2744119ea8fbe6f0ecaa63dba020
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84031206"
---
# <a name="key-differences-between-machine-learning-services-in-azure-sql-managed-instance-and-sql-server"></a>Principales diferencias entre Machine Learning Services de Instancia administrada de Azure SQL y SQL Server

La funcionalidad de [Machine Learning Services de Instancia administrada de Azure SQL (versión preliminar)](machine-learning-services-overview.md) es prácticamente idéntica a la de [Machine Learning Services de SQL Server](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning). A continuación se indican algunas diferencias principales.

> [!IMPORTANT]
> Machine Learning Services de Instancia administrada de Azure SQL se encuentra actualmente en versión preliminar pública. Para registrarse, vea [Registro en la versión preliminar](machine-learning-services-overview.md#signup).

## <a name="preview-limitations"></a>Limitaciones de vista previa

Durante la versión preliminar, el servicio tiene las siguientes limitaciones:

- Las conexiones de bucle invertido no funcionan ( vea [Conexión de bucle invertido con SQL Server desde un script de Python o R](/sql/machine-learning/connect/loopback-connection)).
- Los grupos de recursos externos no se admiten.
- Solo se admiten Python y R. No se pueden agregar lenguajes externos como Java.
- No se admiten escenarios con la [interfaz de paso de mensajes](https://docs.microsoft.com/message-passing-interface/microsoft-mpi) (MPI).

En caso de actualización de un Objetivo de nivel de servicio (SLO), actualice el SLO y cree una incidencia de soporte técnico para volver a habilitar los límites de recursos dedicados de R o Python.

## <a name="language-support"></a>Compatibilidad con idiomas

Machine Learning Services de Instancia administrada de SQL y SQL Server admiten el [marco de extensibilidad](https://docs.microsoft.com/sql/advanced-analytics/concepts/extensibility-framework) de Python y R. Las principales diferencias son:

- Las versiones iniciales de Python y R son diferentes entre Machine Learning Services de Instancia administrada de SQL y SQL Server:

  |                      | Python | R     |
  |----------------------|--------|-------|
  | Instancia administrada de SQL | 3.7.1  | 3.5.2 |
  | SQL Server           | 3.5.2  | 3.3.3 |

- No es necesario configurar `external scripts enabled` mediante `sp_configure`. Una vez que se haya [registrado](machine-learning-services-overview.md#signup) en la versión preliminar, el aprendizaje automático está habilitado para la base de datos SQL.

## <a name="packages"></a>Paquetes

La administración de paquetes de Python y R funciona de forma diferente entre Instancia administrada de SQL y SQL Server. Las diferencias son:

- Los paquetes no pueden realizar llamadas de red salientes. Esta limitación es similar a las [reglas de firewall predeterminadas de Machine Learning Services](https://docs.microsoft.com//sql/advanced-analytics/security/firewall-configuration) de SQL Server, pero no se puede cambiar en Instancia administrada de SQL.
- No hay ninguna compatibilidad con los paquetes que dependen de los entornos de ejecución externos (por ejemplo, Java) o que necesitan tener acceso a las API del sistema operativo para instalarse o usarse.

Para obtener más información sobre la administración de paquetes de Python y R, vea:

- [Obtención de información de paquetes de Python](https://docs.microsoft.com/sql/machine-learning/package-management/python-package-information?context=azure/sql-database/context/ml-context&view=sql-server-ver15)
- [Obtención de información de paquetes de R](https://docs.microsoft.com/sql/machine-learning/package-management/r-package-information?context=azure/sql-database/context/ml-context&view=sql-server-ver15)

## <a name="resource-governance"></a>Regulación de recursos

No es posible limitar los recursos de R a través de [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) ni de grupos de recursos externos.

Durante la versión preliminar pública, los recursos de R se establecen en un máximo del 20 % de los recursos de Instancia administrada de SQL y dependen del nivel de servicio que se elija. Para obtener más información, consulte [Azure SQL Database purchasing models](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers) (Modelos de compra de Azure SQL Database).

### <a name="insufficient-memory-error"></a>Error de memoria insuficiente

Si no hay memoria suficiente disponible para R, recibirá un mensaje de error. Los mensajes comunes de error son:

- Error de comunicación con el runtime del script de "R" para el identificador de solicitud: *******. Compruebe los requisitos del runtime de "R".
- Error de script de "R" ocurrido durante la ejecución de "sp_execute_external_script" con HRESULT 0x80004004. … error de script externo: "… no se pudo asignar memoria (0 Mb) en la función "R_AllocStringBuffer" de C".
- Error de script externo: Error: no se puede asignar el vector de tamaño.

El uso de la memoria depende de cuánta se use en los scripts de R y del número de consultas paralelas que se ejecutan. Si recibe los errores anteriores, puede escalar la base de datos a un nivel de servicio superior para resolver esto.

## <a name="next-steps"></a>Pasos siguientes

- Vea la introducción, [Machine Learning Services de Instancia administrada de Azure SQL](machine-learning-services-overview.md).
- Para obtener información sobre cómo usar Python en Machine Learning Services, vea [Ejecución de scripts de Python](https://docs.microsoft.com/sql/machine-learning/tutorials/quickstart-python-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=sql-server-ver15).
- Para obtener información sobre cómo usar R en Machine Learning Services, vea [Ejecución de scripts de R](https://docs.microsoft.com/sql/machine-learning/tutorials/quickstart-r-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=sql-server-ver15).
