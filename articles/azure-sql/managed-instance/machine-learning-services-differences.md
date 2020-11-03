---
title: Principales diferencias de Machine Learning Services (versión preliminar)
description: En este artículo se describen las principales diferencias entre Machine Learning Services de Azure SQL Managed Instance y Machine Learning Services de SQL Server.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.reviewer: sstein, davidph
manager: cgronlun
ms.date: 10/26/2020
ms.openlocfilehash: adf454ac697f8cabf4256ebfc5baa5d0d1c76264
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92782474"
---
# <a name="key-differences-between-machine-learning-services-in-azure-sql-managed-instance-and-sql-server"></a>Principales diferencias entre Machine Learning Services de Instancia administrada de Azure SQL y SQL Server

La funcionalidad de [Machine Learning Services de Instancia administrada de Azure SQL (versión preliminar)](machine-learning-services-overview.md) es prácticamente idéntica a la de [Machine Learning Services de SQL Server](/sql/advanced-analytics/what-is-sql-server-machine-learning). A continuación se indican algunas diferencias principales.

> [!IMPORTANT]
> Machine Learning Services de Instancia administrada de Azure SQL se encuentra actualmente en versión preliminar pública. Para registrarse, vea [Registro en la versión preliminar](machine-learning-services-overview.md#signup).

## <a name="preview-limitations"></a>Limitaciones de vista previa

Durante la versión preliminar, el servicio tiene las siguientes limitaciones:

- Las conexiones de bucle invertido no funcionan ( vea [Conexión de bucle invertido con SQL Server desde un script de Python o R](/sql/machine-learning/connect/loopback-connection)).
- Los grupos de recursos externos no se admiten.
- Solo se admiten Python y R. No se pueden agregar lenguajes externos como Java.
- No se admiten escenarios con la [interfaz de paso de mensajes](/message-passing-interface/microsoft-mpi) (MPI).

En el caso de una actualización de un Objetivo de nivel de servicio (SLO), actualice el SLO y cree una incidencia de soporte técnico para volver a habilitar los límites de recursos dedicados de R o Python.

## <a name="language-support"></a>Compatibilidad con idiomas

Machine Learning Services de Instancia administrada de SQL y SQL Server admiten el [marco de extensibilidad](/sql/advanced-analytics/concepts/extensibility-framework) de Python y R. Las principales diferencias son:

- Las versiones iniciales de Python y R son diferentes entre Machine Learning Services de Instancia administrada de SQL y SQL Server:

  | Sistema               | Python | R     |
  |----------------------|--------|-------|
  | Instancia administrada de SQL | 3.7.1  | 3.5.2 |
  | SQL Server           | 3.5.2  | 3.3.3 |

- No es necesario configurar `external scripts enabled` mediante `sp_configure`. Una vez que se haya [registrado](machine-learning-services-overview.md#signup) en la versión preliminar, el aprendizaje automático estará habilitado para Instancia administrada de Azure SQL.

## <a name="packages"></a>Paquetes

La administración de paquetes de Python y R funciona de forma diferente entre Instancia administrada de SQL y SQL Server. Las diferencias son:

- No hay ninguna compatibilidad con los paquetes que dependen de los entornos de ejecución externos (por ejemplo, Java) o que necesitan tener acceso a las API del sistema operativo para instalarse o usarse.
- Los paquetes pueden realizar llamadas de red salientes (un cambio respecto al inicio de la versión preliminar). Puede establecer las reglas de seguridad saliente adecuadas en el nivel de [grupo de seguridad de red](/azure/virtual-network/network-security-groups-overview) para habilitar las llamadas de red salientes.

Para obtener más información sobre la administración de paquetes de Python y R, vea:

- [Obtención de información de paquetes de Python](/sql/machine-learning/package-management/python-package-information?context=/azure/azure-sql/managed-instance/context/ml-context&view=azuresqldb-mi-current&preserve-view=true)
- [Obtención de información de paquetes de R](/sql/machine-learning/package-management/r-package-information?context=/azure/azure-sql/managed-instance/context/ml-context&view=azuresqldb-mi-current&preserve-view=true)

## <a name="resource-governance"></a>Regulación de recursos

No es posible limitar los recursos de R a través de [Resource Governor](/sql/relational-databases/resource-governor/resource-governor) ni de grupos de recursos externos.

Durante la versión preliminar pública, los recursos de R se establecen en un máximo del 20 % de los recursos de Instancia administrada de SQL y dependen del nivel de servicio que se elija. Para obtener más información, consulte [Azure SQL Database purchasing models](../database/purchasing-models.md) (Modelos de compra de Azure SQL Database).

### <a name="insufficient-memory-error"></a>Error de memoria insuficiente

Si no hay memoria suficiente disponible para R, recibirá un mensaje de error. Los mensajes comunes de error son:

- `Unable to communicate with the runtime for 'R' script for request id: *******. Please check the requirements of 'R' runtime`
- `'R' script error occurred during execution of 'sp_execute_external_script' with HRESULT 0x80004004. ...an external script error occurred: "..could not allocate memory (0 Mb) in C function 'R_AllocStringBuffer'"`
- `An external script error occurred: Error: cannot allocate vector of size.`

El uso de la memoria depende de cuánta se use en los scripts de R y del número de consultas paralelas que se ejecutan. Si recibe los errores anteriores, puede escalar la base de datos a un nivel de servicio superior para resolver esto.

## <a name="next-steps"></a>Pasos siguientes

- Vea la introducción, [Machine Learning Services de Instancia administrada de Azure SQL](machine-learning-services-overview.md).
- Para obtener información sobre cómo usar Python en Machine Learning Services, vea [Ejecución de scripts de Python](/sql/machine-learning/tutorials/quickstart-python-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=azuresqldb-mi-current&preserve-view=true).
- Para obtener información sobre cómo usar R en Machine Learning Services, vea [Ejecución de scripts de R](/sql/machine-learning/tutorials/quickstart-r-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=azuresqldb-mi-current&preserve-view=true).
