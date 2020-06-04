---
title: Características admitidas de Azure SQL Edge (versión preliminar)
description: Información sobre los detalles de las características compatibles con Azure SQL Edge (versión preliminar)
keywords: introducción a SQL Edge, qué es SQL Edge, información general sobre SQL Edge
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: e3bac1cbb0e57f9be4a489207f2fe8b30b5d457e
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2020
ms.locfileid: "84233213"
---
# <a name="supported-features-of-azure-sql-edge-preview"></a>Características admitidas de Azure SQL Edge (versión preliminar) 

En este artículo se proporcionan los detalles de las características compatibles con Azure SQL Edge. Azure SQL Edge se basa en la versión del Motor de base de datos de Microsoft SQL Server en Linux y admite un subconjunto de características compatibles con SQL Server 2019 para Linux, además de algunas características que actualmente no se admiten o no están disponibles en SQL Server 2019 en Linux o en Windows. Para obtener una lista completa de las características admitidas en SQL Server en Linux, consulte [Ediciones y características admitidas de SQL Server 2019 en Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-editions-and-components-2019). Para conocer las ediciones y las características admitidas de SQL Server en Windows, consulte [Ediciones y características admitidas de SQL Server 2019 (15.x)](https://docs.microsoft.com/sql/sql-server/editions-and-components-of-sql-server-version-15).

> [!NOTE]
> Azure SQL Edge está actualmente en (versión preliminar) y NO debe usarse en entornos de producción. Microsoft puede recomendar la ejecución de Azure SQL Edge en entornos de producción después de la validación de la implementación y los escenarios de casos de uso.

## <a name="azure-sql-edge-editions"></a>Ediciones de Azure SQL Edge

Azure SQL Edge está disponible en dos ediciones o planes de software diferentes. Estas ediciones tienen conjuntos de características idénticos y solo difieren en cuanto a sus derechos de uso y la cantidad de CPU o memoria a la que pueden obtener acceso en el sistema del host.

   |**Plan**  |**Descripción**  |
   |---------|---------|
   |Desarrollador de Azure SQL Edge  |  SKU solo de desarrollo; cada contenedor de Azure SQL Edge está limitado a hasta 4 núcleos y 32 GB de memoria.  |
   |Azure SQL Edge    |  SKU de producción; cada contenedor de Azure SQL Edge está limitado a hasta 8 núcleos y 64 GB de memoria.  |

## <a name="operating-system"></a>Sistema operativo

Actualmente, los contenedores de Azure SQL Edge se basan en Ubuntu 16.04 y, como tal, solo se pueden ejecutar en hosts de Docker con Ubuntu 16.04 (recomendado) o 18.04. Azure SQL Edge también puede ejecutarse en otros hosts del sistema operativo, por ejemplo, otras distribuciones de Linux o en Windows (mediante Docker CE o Docker EE). Sin embargo, Microsoft no prueba exhaustivamente estas configuraciones.

Actualmente Azure SQL Edge solo se admite para la implementación a través de Azure IoT Edge. Para obtener más información sobre los sistemas admitidos para Azure IoT Edge, consulte [Sistemas compatibles con Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/support).

La configuración recomendada para ejecutar Azure SQL Edge en Windows consiste en configurar una VM de Ubuntu en el host de Windows y, a continuación, ejecutar SQL Edge dentro de la VM Linux.

## <a name="hardware-support"></a>Compatibilidad de hardware

Azure SQL Edge requiere un procesador de 64 bits, que puede ser de Intel, AMD o ARM, con un mínimo de un procesador y 1 GB de RAM en el host. Mientras que la superficie de memoria de inicio de Azure SQL Edge está próxima a 500 MB, se necesita memoria adicional para otros módulos de IoT Edge que se ejecutan en el dispositivo perimetral.

## <a name="azure-sql-edge-components"></a>Componentes de Azure SQL Edge

Azure SQL Edge solo admite el motor de base de datos y no incluye compatibilidad con otros componentes disponibles con SQL Server 2019 en Windows o con SQL Server 2019 en Linux. En concreto, Azure SQL Edge no admite componentes de SQL Server, como Analysis Services, Reporting Services, Integration Services, Master Data Services, Machine Learning Services (en base de datos) y Machine Learning Server (independiente).

## <a name="supported-features"></a>Características admitidas

Además de admitir un subconjunto de características de SQL Server en Linux, Azure SQL Edge incluye compatibilidad con las siguientes características nuevas. 

- Streaming de SQL, que se basa en el mismo motor que sustenta Azure Stream Analytics, proporciona funcionalidades de streaming de datos en tiempo real en Azure SQL Edge. 
- La nueva función T-SQL denominada Date_Bucket para el análisis de datos de serie temporal.
- Funcionalidades de Machine Learning a través del runtime de ONNX, incluido con el motor de SQL.

## <a name="sql-server-on-linux-features-not-supported-in-azure-sql-edge"></a>Características de SQL Server en Linux no admitidas en Azure SQL Edge

La lista siguiente incluye las características de SQL Server 2019 en Linux que no se admiten actualmente en Azure SQL Edge.

| Área | Característica o servicio no admitido |
|-----|-----|
| **Diseño de la base de datos** | OLTP en memoria y comandos DDL y funciones de Transact-SQL relacionados, vistas de catálogo y vistas de administración dinámica |
| &nbsp; | Tipo de datos de HierarchyID y comandos DDL y funciones de Transact-SQL relacionados, vistas de catálogo y vistas de administración dinámica |
| &nbsp; | Tipo de datos espaciales y comandos DDL y funciones de Transact-SQL relacionados, vistas de catálogo y vistas de administración dinámica |
| &nbsp; | Stretch DB y comandos DDL y funciones de Transact-SQL relacionados, vistas de catálogo y vistas de administración dinámica |
| &nbsp; | Busca e índices de texto completo y comandos DDL y funciones de Transact-SQL relacionados, vistas de catálogo y vistas de administración dinámica|
| &nbsp; | FileTable, FILESTREAM y comandos DDL y funciones de Transact-SQL relacionados, vistas de catálogo y vistas de administración dinámica|
| **Motor de base de datos** | Replicación. Sin embargo, Azure SQL Edge puede configurarse como un suscriptor de inserción de una topología de replicación. |
| &nbsp; | PolyBase. Sin embargo, Azure SQL Edge puede configurarse como destino para tablas externas en Polybase. |
| &nbsp; | Extensibilidad de lenguaje a través de Java y Spark |
| &nbsp; | Integración de Active Directory |
| &nbsp; | Instantáneas de base de datos |
| &nbsp; | Compatibilidad con memoria persistente |
| &nbsp; | Microsoft DTC (Coordinador de transacciones distribuidas) |
| &nbsp; | Resource Governor y regulación de recursos de E/S |
| &nbsp; | Buffer Pool Extension |
| &nbsp; | Consulta distribuida con conexiones de terceros |
| &nbsp; | Servidores vinculados |
| &nbsp; | Procedimientos extendidos almacenados del sistema (XP_CMDSHELL, etc.) |
| &nbsp; | Ensamblados de CLR y comandos DDL y funciones de Transact-SQL relacionados, vistas de catálogo y vistas de administración dinámica |
| &nbsp; | Funciones T-SQL dependientes de CLR, como ASSEMBLYPROPERTY, FORMAT, PARSE, TRY_PARSE |
| &nbsp; | Vistas de catálogo, funciones y cláusulas de consulta de fecha y hora dependientes de CLR |
| &nbsp; | Buffer Pool Extension |
| &nbsp; | Correo electrónico de base de datos |
| **Agente SQL Server** |  Subsistemas: CmdExec, PowerShell, Agente de lectura de cola, SSIS, SSAS, SSRS |
| &nbsp; | Alertas |
| &nbsp; | Copia de seguridad administrada |
| **Alta disponibilidad** | Grupos de disponibilidad AlwaysOn  |
| &nbsp; | Grupos de disponibilidad básica |
| &nbsp; | Instancias de clúster de conmutación por error de Always On |
| &nbsp; | Creación de reflejo de la base de datos |
| &nbsp; | Agregar memoria y CPU sin interrupción |
| **Seguridad** | Administración extensible de claves |
| &nbsp; | Integración con Active Directory|
| &nbsp; | Compatibilidad con enclaves seguros|
| **Servicios** | SQL Server Browser |
| &nbsp; | Machine Learning a través de R y Python |
| &nbsp; | StreamInsight |
| &nbsp; | Analysis Services |
| &nbsp; | Reporting Services |
| &nbsp; | Data Quality Services |
| &nbsp; | Master Data Services |
| &nbsp; | Distributed Replay |
| **Facilidad de uso** | Punto de control de la utilidad de SQL Server |

## <a name="next-steps"></a>Pasos siguientes

- [Implementación de Azure SQL Edge](deploy-portal.md)
- [Configuración de Azure SQL Edge](configure.md)
- [Conexión a una instancia de Azure SQL Edge mediante herramientas de cliente de SQL Server](connect.md)
- [Copia de seguridad y restauración de bases de datos en Azure SQL Edge](backup-restore.md)
