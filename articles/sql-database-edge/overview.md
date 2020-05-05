---
title: ¿Qué es Azure SQL Database Edge? | Microsoft Docs
description: Obtenga información acerca de Azure SQL Database Edge
keywords: Introducción a SQL Database Edge, qué es SQL Database Edge, información general de SQL Database Edge
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 03/24/2020
ms.openlocfilehash: d5c48b6036065f6182912c21c144cab80fc3cfbf
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "80246711"
---
# <a name="what-is-azure-sql-database-edge-preview"></a>¿Qué es la versión preliminar de Azure SQL Database Edge?

La versión preliminar de Azure SQL Database Edge es un motor de base de datos relacional optimizado orientado a implementaciones de IoT y IoT Edge. Proporciona funcionalidades para crear una capa de procesamiento y almacenamiento de datos de alto rendimiento para las aplicaciones y soluciones de IoT. Azure SQL Database Edge proporciona funcionalidades para transmitir, procesar y analizar datos relacionales y no relacionales como JSON, gráficos y datos de series temporales, lo que hace que sea la opción adecuada para una variedad de aplicaciones de IoT modernas.

Azure SQL Database Edge se basa en las versiones más recientes del [Motor de base de datos de Microsoft SQL Server](/sql/sql-server/sql-server-technical-documentation?toc=/azure/sql-database-edge/toc.json), que proporciona capacidades de rendimiento, seguridad y procesamiento de consultas líderes en el sector. Dado que Azure SQL Database Edge se basa en el mismo motor que SQL Server y Azure SQL Database, proporciona la misma área expuesta de programación de T-SQL, la cual hace que el desarrollo de aplicaciones o soluciones sea más fácil y rápido, y al mismo tiempo hace que la portabilidad de las aplicaciones entre los dispositivos IoT Edge, los centros de datos y la nube sea más directa.

## <a name="deployment-models"></a>Modelos de implementación

Azure SQL Database Edge está disponible en Azure Marketplace y se puede implementar como un módulo para [Azure IoT Edge](../iot-edge/about-iot-edge.md). Para obtener más información, consulte [Implementación de Azure SQL Database Edge](deploy-portal.md).<br>

![Diagrama de información general de SQL Database Edge](media/overview/overview.png)

## <a name="editions-of-sql-database-edge"></a>Ediciones de SQL Database Edge

SQL Database Edge está disponible en tres ediciones o planes de software diferentes. Estas ediciones tienen conjuntos de características idénticos y solo difieren en cuanto a sus derechos de uso y la cantidad de CPU o memoria que admiten.

   |**Plan**  |**Descripción**  |
   |---------|---------|
   |Desarrollador de Azure SQL Database Edge  |  SKU solo de desarrollo; cada contenedor de SQL Database está limitado a hasta 4 núcleos y 32 GB de memoria  |
   |Azure SQL Database Edge    |  SKU de producción; cada contenedor de SQL Database perimetral está limitado a hasta 8 núcleos y 64 GB de memoria. |

## <a name="pricing-and-availability"></a>Precios y disponibilidad

Actualmente, Azure SQL Database se encuentra en versión preliminar. Para obtener más información sobre los precios y la disponibilidad, consulte [Azure SQL Database Edge](https://azure.microsoft.com/services/sql-database-edge/).

> [!IMPORTANT]
> Para comprender las diferencias en las características de Azure SQL Database Edge y SQL Server, así como las que existen entre distintas opciones de Azure SQL Database Edge, consulte las [características de base de datos de SQL Database Edge](https://azure.microsoft.com/services/sql-database-edge/).

## <a name="streaming-capabilities"></a>Capacidades de streaming  

Azure SQL Database Edge ofrece funcionalidades de streaming integradas para el análisis en tiempo real y el procesamiento de eventos complejos. La funcionalidad de streaming se crea con las mismas construcciones que [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) y proporciona funcionalidades similares a [Azure Stream Analytics en IoT Edge](../stream-analytics/stream-analytics-edge.md).

El motor de streaming de Azure SQL Database Edge está diseñado para ofrecer baja latencia, resistencia, uso eficiente del ancho de banda y cumplimiento.

## <a name="machine-learning-and-artificial-intelligence-capabilities"></a>Funcionalidades de aprendizaje automático e inteligencia artificial

Azure SQL Database Edge proporciona funcionalidades de análisis y aprendizaje automático integradas mediante la integración del entorno de ejecución de formato abierto de ONNX (Open Neuronal Network Exchange), que permite el intercambio de modelos de red neuronal y de aprendizaje profundo entre diferentes marcos. Para obtener más información sobre ONNX, vaya [aquí](https://onnx.ai/). El entorno de ejecución de ONNX proporciona la flexibilidad para desarrollar modelos en el lenguaje o las herramientas de su elección, que luego se pueden convertir al formato ONNX para su ejecución en SQL Database Edge. Para obtener más información, consulte [Aprendizaje automático e inteligencia artificial con ONNX en SQL Database Edge](onnx-overview.md).

## <a name="working-with-azure-sql-database-edge"></a>Uso de Azure SQL Database Edge

Azure SQL Database Edge facilita el desarrollo y el mantenimiento de aplicaciones y aumenta su productividad. Los usuarios pueden usar todas las herramientas y los conocimientos que ya conocen para crear excelentes aplicaciones y soluciones para sus necesidades de IoT Edge. El usuario puede desarrollar en SQL Database Edge mediante herramientas como las siguientes:

- [Azure Portal](https://portal.azure.com/): una aplicación web para administrar todos los servicios de Azure.
- [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms/): una aplicación cliente gratuita que se puede descargar para administrar cualquier infraestructura de SQL, desde SQL Server hasta SQL Database.
- [SQL Server Data Tools en Visual Studio](/sql/ssdt/download-sql-server-data-tools-ssdt/): una aplicación cliente gratuita que se puede descargar para desarrollar bases de datos relacionales de SQL Server, bases de datos de SQL, paquetes de Integration Services, modelos de datos de Analysis Services e informes de Reporting Services.
- [Azure Data Studio](/sql/azure-data-studio/what-is/): una herramienta multiplataforma gratuita de base de datos que se puede descargar, ideal para profesionales de datos que utilizan la familia Microsoft de plataformas de datos locales y en nube para Windows, macOS y Linux.
- [Visual Studio Code](https://code.visualstudio.com/docs): editor de código abierto gratuito que se puede descargar para Windows, macOS y Linux. Admite extensiones, como la [extensión mssql](https://aka.ms/mssql-marketplace) para consultar Microsoft SQL Server, Azure SQL Database y SQL Azure Data Warehouse.


## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre los precios y los detalles relacionados con la disponibilidad, consulte [Azure SQL Database Edge](https://azure.microsoft.com/services/sql-database-edge/).
- Solicite que habiliten Azure SQL Database Edge para su suscripción.
- Para comenzar, consulte los siguientes artículos:
  - [Implementación de SQL Database Edge mediante Azure Portal](deploy-portal.md)
  - [Aprendizaje automático e inteligencia artificial con SQL Database Edge](onnx-overview.md)
