---
title: Guía de inicio rápido de bases de datos únicas
description: Aprenda a empezar a trabajar rápidamente con las bases de datos únicas en Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
ms.date: 07/29/2019
ms.openlocfilehash: 6070b53e5f906bc378402d98275b8f798f57b505
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "78674430"
---
# <a name="getting-started-with-single-databases-in-azure-sql-database"></a>Introducción a las bases de datos únicas en Azure SQL Database

[Una base de datos única](sql-database-single-index.yml) es una base de datos como servicio PaaS que está totalmente administrada (DbaaS) y que resulta el motor de almacenamiento ideal para las aplicaciones modernas creadas en la nube. En esta sección, aprenderá a crear y configurar rápidamente una base de datos única en SQL Database.

## <a name="quickstart-overview"></a>Introducción al artículo de inicio rápido

En esta sección, verá información general de los artículos disponibles que le ayudarán a empezar a trabajar rápidamente con las bases de datos únicas. Los siguientes inicios rápidos le permiten crear rápidamente una base de datos única, configurar una regla de firewall del servidor de bases de datos y, a continuación, importar una base de datos en la nueva base de datos única mediante un archivo `.bacpac`:

- [Creación de una base de datos única mediante Azure Portal](sql-database-single-database-get-started.md).
- Después de crear la base de datos, necesitaría [proteger la base de datos mediante la configuración de reglas de firewall](sql-database-server-level-firewall-rule.md).
- Si tiene una base de datos en SQL Server que desea migrar a Azure, debe instalar [Data Migration Assistant (DMA)](https://www.microsoft.com/download/details.aspx?id=53595), que analizará las bases de datos de SQL Server y detectará cualquier problema que pudiera bloquear la migración a la opción de implementación de la base de datos única. Si no se encuentra ninguno, puede exportar la base de datos como archivo `.bacpac` e [importarlo mediante Azure Portal o SqlPackage](sql-database-import.md).

## <a name="automating-management-operations"></a>Automatización de las operaciones de administración

Puede usar PowerShell o la CLI de Azure para crear, configurar y escalar la base de datos.

- [Creación y configuración de una base de datos única con PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) o la [CLI de Azure](scripts/sql-database-create-and-configure-database-cli.md)
- [Actualización de una base de datos única y escalado de recursos mediante PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md) o la [CLI de Azure](scripts/sql-database-monitor-and-scale-database-cli.md)

## <a name="migrating-to-a-single-database-with-minimal-downtime"></a>Migración a una base de datos única con tiempo de inactividad mínimo

Estos inicios rápidos le permiten crear o importar rápidamente la base de datos a Azure mediante un archivo `.bacpac`. Los archivos `.bacpac` y `.dacpac` están diseñados para mover rápidamente las bases de datos entre las distintas versiones de SQL Server y las opciones de implementación de Azure SQL Database, o para implementar la integración continua en la canalización de DevOps. No obstante, este método no está diseñado para la migración de las bases de datos de producción con un tiempo de inactividad mínimo, ya que tendría que dejar de agregar nuevos datos, esperar a que se completara la exportación de la base de datos de origen a un archivo `.bacpac` y, finalmente, esperar a que terminara la importación en Azure SQL Database. Todo este tiempo de espera genera un tiempo de inactividad de la aplicación, especialmente en el caso de grandes bases de datos. Para mover la base de datos de producción, probablemente necesite una manera mejor de realizar la migración que garantice un tiempo de inactividad mínimo. Para ello, use [Data Migration Service (DMS)](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql?toc=/azure/sql-database/toc.json) para migrar la base de datos con el menor tiempo de inactividad posible. DMS lo consigue mediante la inserción de forma incremental de los cambios realizados en la base de datos de origen en la base de datos única que se restaura. De este modo, puede cambiar rápidamente la aplicación de la base de datos de origen a la de destino con tiempo de inactividad mínimo.

## <a name="hands-on-learning-modules"></a>Módulos de aprendizaje práctico

El siguiente módulo de Microsoft Learn le ayuda a aprender de forma gratuita sobre Azure SQL Database.

- [Aprovisionamiento de una base de datos de Azure SQL para almacenar datos de aplicación](https://docs.microsoft.com/learn/modules/provision-azure-sql-db/)
- [Desarrollo y configuración de una aplicación de ASP.NET que consulta una instancia de Azure SQL Database](https://docs.microsoft.com/learn/modules/develop-app-that-queries-azure-sql/)
- [Protección de las bases de datos de Azure SQL](https://docs.microsoft.com/learn/modules/secure-your-azure-sql-database/)

## <a name="next-steps"></a>Pasos siguientes

- Buscar una [lista detallada de las características admitidas en Azure SQL Database](sql-database-features.md).
- Aprender a [proteger mejor la base de datos](sql-database-security-tutorial.md).
- Para conocer procedimientos más avanzados, consulte [Cómo usar una base de datos única en Azure SQL Database](sql-database-howto-single-database.md).
- Buscar más scripts de ejemplo escritos en [PowerShell](sql-database-powershell-samples.md) y la [CLI de Azure](sql-database-cli-samples.md).
- Conozca más información sobre la [API de administración](sql-database-single-databases-manage.md) que puede usar para configurar las bases de datos.
- [Identificación de la SKU de instancia administrada o SQL Database de Azure adecuada para la base de datos local](/sql/dma/dma-sku-recommend-sql-db/).
