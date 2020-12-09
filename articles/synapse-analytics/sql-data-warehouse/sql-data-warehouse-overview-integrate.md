---
title: Creación de soluciones integradas
description: Herramientas de soluciones y asociados que se integran en un grupo de SQL dedicado (anteriormente SQL DW) en Azure Synapse Analytics.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 2afc274bf7c040eca6a83abbab24c41767f16482
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96453670"
---
# <a name="integrate-other-services-with-a-dedicated-sql-pool-formerly-sql-dw-in-azure-synapse-analytics"></a>Integre otros servicios en un grupo de SQL dedicado (anteriormente SQL DW) en Azure Synapse Analytics.

La funcionalidad de grupo de SQL dedicado (anteriormente SQL DW) en Azure Synapse Analytics permite a los usuarios la integración en muchos de los demás servicios de Azure. Un grupo de SQL dedicado puede emplear varios servicios adicionales, algunos de los cuales incluyen:

* Power BI
* Azure Data Factory
* Azure Machine Learning
* Azure Stream Analytics

Para obtener más información sobre los servicios de integración en Azure, revise el artículo [Asociados de integración](sql-data-warehouse-partner-data-integration.md).

## <a name="power-bi"></a>Power BI

La integración de Power BI le permite combinar la capacidad de procesamiento de un almacenamiento de datos con los informes dinámicos y la visualización de Power BI. La integración de Power BI actualmente incluye:

* **Direct Connect**: Una conexión más avanzada con aplicación de lógica en un almacenamiento de datos aprovisionado mediante un grupo de SQL dedicado (anteriormente SQL DW). La aplicación proporciona un análisis más rápido a mayor escala.
* **Open in Power BI**: El botón "Open in Power BI" (Abrir en Power BI) pasa la información de la instancia a Power BI para lograr una conexión más fluida.

Para más información, consulte [Integración con Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md) o la [documentación de Power BI](https://powerbi.microsoft.com/blog/exploring-azure-sql-data-warehouse-with-power-bi/).

## <a name="azure-data-factory"></a>Azure Data Factory

Azure Data Factory ofrece a los usuarios una plataforma administrada para crear canalizaciones complejas de extracción y carga. La integración de un grupo de SQL dedicado (anteriormente SQL DW) en Azure Data Factory incluye lo siguiente:

* **Procedimientos almacenados**: supervise la ejecución de procedimientos almacenados.
* **Copia**: Use ADF para mover datos a un grupo de SQL dedicado (anteriormente SQL DW). Esta operación puede utilizar el mecanismo estándar de movimiento de datos de ADF o PolyBase en segundo plano.

Para más información, consulte el tema sobre la [integración con Azure Data Factory](../../data-factory/load-azure-sql-data-warehouse.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

## <a name="azure-machine-learning"></a>Azure Machine Learning

Azure Machine Learning es un servicio de análisis totalmente administrado que permite crear modelos complejos aprovechando un amplio conjunto de herramientas de predicción. El grupo de SQL dedicado (anteriormente SQL DW) se admite como origen y destino para estos modelos y tiene la siguiente funcionalidad:

* **Lectura de datos**: Produce modelos a escala mediante T-SQL en un grupo de SQL dedicado (anteriormente SQL DW).
* **Escritura de datos**: Confirma los cambios de cualquier modelo en un grupo de SQL dedicado (anteriormente SQL DW).

Para más información, consulte el tema sobre la [integración con Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md).

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

Azure Stream Analytics es una infraestructura compleja y totalmente administrada para el procesamiento y consumo de datos de eventos generados por el Centro de eventos de Azure.  La integración en un grupo de SQL dedicado (anteriormente SQL DW) permite que los datos de streaming se procesen y almacenen junto con los datos relacionales para permitir un análisis más profundo y avanzado de los datos.  

* **Resultado del trabajo:** Envía los resultados de los trabajos de Stream Analytics directamente a un grupo de SQL dedicado (anteriormente SQL DW).

Para más información, consulte el tema sobre la [integración con Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md).
