---
title: Introducción al desarrollo de aplicaciones
description: Aprenda sobre las bibliotecas de conectividad disponibles y los procedimientos recomendados para las aplicaciones que se conectan a SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: genemi
ms.date: 11/14/2019
ms.openlocfilehash: 741906bbe9de68459b2e4a704a243fde4771b3a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80067323"
---
# <a name="sql-database-application-development-overview"></a>Introducción al desarrollo de aplicaciones en SQL Database

Este artículo le lleva por las consideraciones básicas que debe tener en cuenta un desarrollador al escribir código para conectarse a Azure SQL Database. Este artículo se aplica a todos los modelos de implementación de Azure SQL Database (base de datos única, grupos elásticos, instancias administradas).

> [!TIP]
> Si necesita configurar su base de datos de Azure SQL, consulte las guías de introducción para las [bases de datos únicas](sql-database-single-database-quickstart-guide.md) e [instancias administradas](sql-database-managed-instance-quickstart-guide.md).
>

## <a name="language-and-platform"></a>Plataforma y lenguaje

Puede usar distintas [plataformas y lenguajes de programación](sql-database-connect-query.md) para conectarse y consultar a Azure SQL Database. Puede encontrar [aplicaciones de ejemplo](https://azure.microsoft.com/resources/samples/?service=sql-database&sort=0) que puede usar para conectarse a Azure SQL Database.

Puede aprovechar herramientas de código abierto como [cheetah](https://github.com/wunderlist/cheetah), [sql-cli](https://www.npmjs.com/package/sql-cli) o [VS Code](https://code.visualstudio.com/). Además, Azure SQL Database funciona con herramientas de Microsoft como [Visual Studio](https://www.visualstudio.com/downloads/) y [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx). También puede usar Azure Portal, PowerShell y API REST, que le ayudan a conseguir una mayor productividad.

## <a name="authentication"></a>Authentication

El acceso a Azure SQL Database está protegido con inicios de sesión y firewalls. Azure SQL Database admite inicios de sesión y usuarios de SQL Server y de [autenticación de Azure Active Directory (AAD)](sql-database-aad-authentication.md). Los inicios de sesión de AAD solo están disponibles en la Instancia administrada. 

Obtenga más información sobre cómo [administrar el acceso e inicio de sesión a bases de datos](sql-database-manage-logins.md).

## <a name="connections"></a>Conexiones

En la lógica de conexión de cliente, reemplace el tiempo de espera predeterminado para que sea de 30 segundos. El valor predeterminado de 15 segundos es demasiado corto para conexiones que dependen de Internet.

Si usa un [grupo de conexiones](https://msdn.microsoft.com/library/8xx3tyca.aspx), asegúrese de cerrar la conexión en el momento en que el programa no la esté usando activamente y no esté preparándose para volver a usarla.

Evite las transacciones de larga ejecución, ya que cualquier error de conexión o de infraestructura puede revertir la transacción. Si es posible, divida la transacción en varias transacciones más pequeñas y use el [procesamiento por lotes para mejorar el rendimiento](sql-database-use-batching-to-improve-performance.md).

## <a name="resiliency"></a>Resistencia

Azure SQL Database es un servicio en la nube del que puede esperar errores transitorios que se producen en la infraestructura subyacente o en la comunicación entre las entidades de la nube. Aunque Azure SQL Database es resistente a los errores transitorios de infraestructura, estos pueden afectar a la conectividad. Cuando se produce un error transitorio al establecer una conexión con SQL Database, el código debe [reintentar la llamada](sql-database-connectivity-issues.md). Recomendamos que en la lógica de reintento se haga uso de la lógica de interrupción, de este modo no se sobrecargará la SQL Database con los reintentos de varios clientes a la vez. La lógica de reintento depende de los [códigos de error para las aplicaciones cliente de SQL Database](troubleshoot-connectivity-issues-microsoft-azure-sql-database.md).

Para más información sobre cómo prepararse para los eventos de mantenimiento planeado en su base de datos de Azure SQL, consulte [Planeación de los eventos de mantenimiento en Azure SQL Database](sql-database-planned-maintenance.md).

## <a name="network-considerations"></a>Consideraciones sobre la red

- En el equipo que hospeda el programa cliente, asegúrese de que el firewall permita la comunicación TCP saliente en el puerto 1433.  Más información: [Configuración de un firewall de Azure SQL Database mediante Azure Portal](sql-database-configure-firewall-settings.md).
- Si el programa cliente se conecta a SQL Database mientras el cliente se ejecuta en una máquina virtual (VM) de Azure, debe abrir determinados intervalos de puerto en la máquina virtual. Más información: [Puertos más allá de 1433 para ADO.NET 4.5 y SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).
- En ocasiones, las conexiones de cliente a Azure SQL Database omiten el proxy e interactúan directamente con la base de datos. Los puertos que no sean 1433 se convierten en puertos importantes. Para más información, consulte [Arquitectura de conectividad de Azure SQL Database](sql-database-connectivity-architecture.md) y [Puertos más allá de 1433 para ADO.NET 4.5 y SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).
- Para la configuración de red de una instancia administrada, consulte la [configuración de red para instancias administradas](sql-database-howto-managed-instance.md#network-configuration).

## <a name="next-steps"></a>Pasos siguientes

Explore todas las [funcionalidades de SQL Database](sql-database-technical-overview.md).
