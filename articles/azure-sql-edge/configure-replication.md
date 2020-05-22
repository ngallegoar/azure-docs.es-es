---
title: Configuración de la replicación en Azure SQL Edge (versión preliminar)
description: Información sobre la configuración de la replicación en Azure SQL Edge (versión preliminar)
keywords: ''
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: e2b37e0f3ccf5fcebe4723c05d644f2cbb7c1d56
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83593994"
---
# <a name="configure-replication-to-azure-sql-edge-preview"></a>Configuración de la replicación en Azure SQL Edge (versión preliminar) 

La instancia de Azure SQL Edge puede configurarse como el suscriptor de inserción para una replicación transaccional unidireccional o una replicación de instantáneas. La instancia de Azure SQL Edge no puede actuar como el publicador o el distribuidor para una configuración de replicación transaccional. La replicación de mezcla, la replicación P2P y la publicación de Oracle no son compatibles con Azure SQL Edge.

## <a name="supported-configurations"></a>**Configuraciones admitidas**:
  
- La instancia de Azure SQL Edge debe ser un suscriptor de inserción para un publicador.
- El publicador y el distribuidor pueden ser:
   - Una instancia de SQL Server en ejecución en el entorno local o una instancia de SQL Server en ejecución en una máquina virtual de Azure. Para obtener más información, consulte [Introducción a SQL Server en máquinas virtuales de Azure](https://azure.microsoft.com/documentation/articles/virtual-machines-sql-server-infrastructure-services/). Las instancias de SQL Server deben usar una versión superior a SQL Server 2016.
   - Una instancia de Instancia administrada de Azure SQL Database. Instancia administrada puede hospedar las bases de datos del publicador, distribuidor y suscriptor. Para más información, consulte [Replication with SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/replication-with-sql-database-managed-instance/) (Replicación con Instancia administrada de Azure SQL Database).

- La base de datos de distribución y los agentes de replicación no pueden colocarse en una instancia de Azure SQL Edge.  

> [!NOTE]
> Al intentar configurar la replicación con una versión que no es compatible, puede producirse el error número MSSQL_REPL20084 (El proceso no pudo conectarse al suscriptor) y MSSQL_REPL40532 (No se puede abrir el servidor \<nombre> solicitado por el inicio de sesión. Error de inicio de sesión).  

Para usar todas las características de Azure SQL Edge, debe usar las versiones más recientes de [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) y [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).  

## <a name="remarks"></a>Observaciones

- La replicación puede configurarse mediante el uso de [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) o a través de la ejecución de instrucciones Transact-SQL en el publicador mediante SQL Server Management Studio o [Azure Database Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio).
- La replicación solo puede usar inicios de sesión de autenticación de SQL Server para conectarse a una instancia de Azure SQL Edge.
- Las tablas replicadas deben tener una clave principal.
- Una sola publicación en SQL Server puede admitir los suscriptores de SQL Server (entorno local y SQL Server en una máquina virtual de Azure) y Azure SQL Edge.  
- La administración, la supervisión y la solución de problemas de la replicación deben realizarse desde SQL Server local.  
- Azure SQL Edge solo admite las suscripciones de inserción.  
- Solo se admite `@subscriber_type = 0` en **sp_addsubscription** para Azure SQL Edge.  
- Azure SQL Edge no admite replicaciones bidireccionales, inmediatas, actualizables o de punto a punto.
- Azure SQL Edge solo admite un subconjunto de características disponibles en SQL Server o Instancia administrada de Azure SQL Database, ya que un intento de replicar una base de datos (u objetos en la base de datos) que contengan una o varias características no admitidas producirá un error. Por ejemplo, si intenta replicar una base de datos que contiene objetos con tipos de datos espaciales, se producirá un error. Para más información sobre las características compatibles con Azure SQL Edge, consulte [Características admitidas de Azure SQL Edge](features.md).

## <a name="scenarios"></a>Escenarios  

### <a name="initializing-reference-data-on-an-edge-instance"></a>Inicialización de datos de referencia en una instancia de Edge

Un escenario común en el que la replicación puede ser útil es cuando es necesario inicializar la instancia de Edge con datos de referencia que cambian con el tiempo. Por ejemplo, la actualización de modelos de ML en la instancia de Edge después de que se hayan entrenado en una instancia de SQL Server local.

1. Cree una publicación de replicación transaccional en una base de datos de SQL Server local.  
2. En la instancia de SQL Server local, use el **Asistente para nueva suscripción** o las instrucciones Transact-SQL para crear una inserción en la suscripción de Azure SQL Edge.  
3. La base de datos replicada en Azure SQL Edge se puede inicializar con una instantánea generada por el agente de instantáneas y distribuida y entregada por el agente de distribución o mediante una copia de seguridad de la base de datos desde el publicador. Una vez más, si la copia de seguridad de la base de datos contiene objetos o características que no son compatibles con Azure SQL Edge, se producirá un error en la operación de restauración.

## <a name="limitations"></a>Limitaciones

Las siguientes opciones no se admiten para las suscripciones de Azure SQL Edge:

- Copiar asociaciones de grupos de archivos  
- Copiar esquemas de partición de tabla  
- Copiar esquemas de partición de índice  
- Copiar estadísticas definidas por el usuario  
- Copiar enlaces predeterminados  
- Copiar enlaces de reglas  
- Copiar índices de texto completo  
- Copiar XML XSD  
- Copiar índices XML  
- Copiar permisos  
- Copiar índices espaciales  
- Copiar índices filtrados  
- Copiar atributos de compresión de datos  
- Copiar atributos de columna dispersa  
- Copiar tipos de datos espaciales, FileStream o HierarchyId.
- Convertir HierarchyId en tipos de datos MAX  
- Convertir una instancia espacial en tipos de datos MAX  
- Copiar propiedades extendidas  
- Copiar permisos  

## <a name="examples"></a>Ejemplos

Cree una publicación y una suscripción de inserción. Para más información, consulte:
  
- [Create a Publication](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication) (Creación de una publicación)
- [Cree una suscripción de inserción](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/) mediante el nombre del servidor o la dirección IP de Azure SQL Edge como suscriptor (por ejemplo, **myEdgeinstance,1433**) y el nombre de la base de datos en la instancia de Azure SQL Edge como base de datos de destino (por ejemplo, **AdventureWorks**).  

## <a name="see-also"></a>Consulte también  

- [Create a Publication](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication) (Creación de una publicación)
- [Create a Push Subscription](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/) (Creación de una suscripción de inserción)
- [Tipos de replicación](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)
- [Supervisión (replicación)](https://docs.microsoft.com/sql/relational-databases/replication/monitor/monitoring-replication)
- [Inicializar una suscripción](https://docs.microsoft.com/sql/relational-databases/replication/initialize-a-subscription)  


