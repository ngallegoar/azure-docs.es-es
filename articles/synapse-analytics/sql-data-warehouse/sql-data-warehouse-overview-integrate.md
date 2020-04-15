---
title: Creación de soluciones integradas
description: Herramientas de soluciones y asociados que se integran con un grupo de SQL de Synapse.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 2f6e091b6e0285bea5fef9e4d0be40faec936c6b
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633143"
---
# <a name="integrate-other-services-with-a-sql-analytics-data-warehouse"></a>Integración de otros servicios con un almacenamiento de datos de SQL Analytics

La funcionalidad SQL Analytics de Azure Synapse Analytics permite a los usuarios la integración con muchos de los otros servicios de Azure. Con SQL Analytics puede crear un almacenamiento de datos mediante su recurso de grupo de SQL, que pueden utilizar después varios servicios, entre los que se incluyen:

* Power BI
* Azure Data Factory
* Azure Machine Learning
* Azure Stream Analytics

Para más información sobre los servicios de integración en Azure, consulte el artículo [Asociados de integración](sql-data-warehouse-partner-data-integration.md).

## <a name="power-bi"></a>Power BI

La integración de Power BI le permite combinar la capacidad de procesamiento de un almacenamiento de datos con los informes dinámicos y la visualización de Power BI. La integración de Power BI actualmente incluye:

* **Direct Connect**: una conexión más avanzada con aplicación de lógica en un almacenamiento de datos aprovisionado mediante un grupo de SQL. La aplicación proporciona un análisis más rápido a mayor escala.
* **Open in Power BI**: El botón "Open in Power BI" (Abrir en Power BI) pasa la información de la instancia a Power BI para lograr una conexión más fluida.

Para más información, consulte [Integración con Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md) o la [documentación de Power BI](https://powerbi.microsoft.com/blog/exploring-azure-sql-data-warehouse-with-power-bi/).

## <a name="azure-data-factory"></a>Azure Data Factory

Azure Data Factory ofrece a los usuarios una plataforma administrada para crear canalizaciones complejas de extracción y carga. La integración de un grupo de SQL con Azure Data Factory incluye lo siguiente:

* **Procedimientos almacenados**: supervise la ejecución de procedimientos almacenados.
* **Copia**: use ADF para trasladar datos a un grupo de SQL. Esta operación puede utilizar el mecanismo estándar de movimiento de datos de ADF o PolyBase en segundo plano.

Para más información, consulte el tema sobre la [integración con Azure Data Factory](../../data-factory/load-azure-sql-data-warehouse.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

## <a name="azure-machine-learning"></a>Azure Machine Learning

Azure Machine Learning es un servicio de análisis totalmente administrado que permite crear modelos complejos aprovechando un amplio conjunto de herramientas de predicción. El grupo de SQL se admite como origen y destino para estos modelos y tiene la siguiente funcionalidad:

* **Lectura de datos**: produce modelos a escala mediante T-SQL en un grupo de SQL.
* **Escritura de datos**: confirma los cambios de cualquier modelo en un grupo de SQL.

Para más información, consulte el tema sobre la [integración con Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md).

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

Azure Stream Analytics es una infraestructura compleja y totalmente administrada para el procesamiento y consumo de datos de eventos generados por el Centro de eventos de Azure.  La integración con un grupo de SQL permite que los datos de transmisión se procesen y almacenen junto con los datos relacionales para permitir un análisis más profundo y avanzado de los datos.  

* **Resultado del trabajo:** envía los resultados de los trabajos de Stream Analytics directamente a un grupo de SQL.

Para más información, consulte el tema sobre la [integración con Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md).
