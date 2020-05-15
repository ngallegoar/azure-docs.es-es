---
title: Instalación de Visual Studio 2019
description: Instalación de Visual Studio y SQL Server Development Tools (SSDT) para SQL de Synapse
services: synapse-analytics
ms.custom: vs-azure, azure-synapse
ms.workload: azure-vs
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 05/11/2020
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 9f36fb952b21b058fb50dc567f714e8bdb665d6c
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83200313"
---
# <a name="getting-started-with-visual-studio-2019"></a>Introducción a Visual Studio 2019

SQL Server Data Tools (SSDT) para Visual Studio **2019** es una herramienta única que permite hacer lo siguiente:

- Conectarse a aplicaciones, realizar consultas y desarrollarlas
- Aprovechar un explorador de objetos para examinar de forma visual todos los objetos del modelo de datos, lo que incluye las tablas, las vistas, los procedimientos almacenados, etc.
- Generar scripts de lenguaje de definición de datos (DDL) de T-SQL para los objetos
- Desarrollar el almacenamiento de datos mediante un enfoque basado en el estado con proyectos de base de datos de SSDT
- Integrar el proyecto de base de datos con sistemas de control de código fuente como Git con repositorios de Azure Repos
- Configuración de canalizaciones de integración e implementación continuas con servidores de automatización como Azure DevOps

## <a name="install-visual-studio-2019"></a>Instalación de Visual Studio 2019

Consulte [Descargar Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) para descargar e instalar Visual Studio **16.3 o posteriores**. Durante la instalación, seleccione la carga de trabajo de almacenamiento de datos y procesamiento. La instalación independiente de SSDT ya no es necesaria en Visual Studio 2019.

## <a name="unsupported-features-in-ssdt"></a>Características no admitidas en SSDT

En ocasiones, es posible que las versiones de actualización de características de SQL de Synapse no incluyan compatibilidad con SSDT. Actualmente no se admiten las siguientes características:


- [Administración de cargas de trabajo](sql-data-warehouse-workload-management.md): grupos y clasificadores de cargas de trabajo.
- [Seguridad de nivel de fila](/sql/relational-databases/security/row-level-security?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
  - Envíe una [incidencia de soporte técnico o vote](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/39040057-ssdt-row-level-security) para obtener la característica admitida.
- [Enmascaramiento de datos dinámicos](/sql/relational-databases/security/dynamic-data-masking?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest#defining-a-dynamic-data-mask)
   - Envíe una [incidencia de soporte técnico o vote](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/39040048-ssdt-support-dynamic-data-masking) para obtener la característica admitida.

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene la versión más reciente de SSDT, ya puede [conectarse](sql-data-warehouse-query-visual-studio.md) al grupo de SQL.
