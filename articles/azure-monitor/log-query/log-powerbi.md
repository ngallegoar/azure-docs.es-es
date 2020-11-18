---
title: Integración de Log Analytics con Power BI y Excel
description: Cómo enviar los resultados de Log Analytics a Power BI
ms.subservice: logs
ms.topic: conceptual
author: roygalMS
ms.author: roygal
ms.date: 11/03/2020
ms.openlocfilehash: a4e2faf87ac3dddf91e6945343a46b02df72db0a
ms.sourcegitcommit: 4bee52a3601b226cfc4e6eac71c1cb3b4b0eafe2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94507402"
---
# <a name="log-analytics-integration-with-power-bi"></a>Integración de Log Analytics con Power BI

Este artículo se centra en las maneras de insertar datos desde Log Analytics en Microsoft Power BI para crear informes y paneles más atractivos visualmente. 

## <a name="background"></a>Información previa 

Los registros de Azure Monitor son una plataforma que proporciona una solución de un extremo a otro para la ingesta de registros. [Log Analytics de Azure Monitor](../platform/data-platform.md#) es la interfaz para consultar estos registros. Para más información sobre toda la plataforma de datos de Azure Monitor, incluido Log Analytics, consulte [Plataforma de datos de Azure Monitor](../platform/data-platform.md). 

Microsoft Power BI es la plataforma de visualización de datos de Microsoft. Para más información sobre cómo empezar, consulte la [página principal de Power BI](https://powerbi.microsoft.com/). 


En general, puede usar las características gratuitas de Power BI para integrar y crear informes y paneles visualmente atractivos.

Las características más avanzadas pueden requerir la adquisición de una cuenta de Power BI Pro o Premium. Estas características son las siguientes: 
 - Compartir el trabajo 
 - Actualizaciones programadas
 - Aplicaciones de Power BI 
 - Flujos de datos y actualización incremental 

Para más información, consulte [Precios de Power BI](https://powerbi.microsoft.com/pricing/). 

## <a name="integrating-queries"></a>Integración de consultas  

Power BI usa el [lenguaje de consulta M](/powerquery-m/power-query-m-language-specification/) como lenguaje de consulta principal. 

Las consultas de Log Analytics se pueden exportar a M y utilizarse directamente en Power BI. Después de ejecutar una consulta correcta, seleccione **Exportar a Power BI (M Query)** en el botón **Exportar** de la barra de acciones superior de la interfaz de usuario de Log Analytics.

:::image type="content" source="media/log-powerbi-excel/export-query2.png" alt-text="Consulta de Log Analytics que muestra el desplegable del menú de opciones de exportación" border="true":::

Log Analytics crea un archivo .txt que contiene el código M que se puede usar directamente en Power BI.

## <a name="connecting-your-logs-to-a-dataset"></a>Conexión de los registros a un conjunto de datos 

Un conjunto de datos de Power BI es un origen de datos preparado para la creación de informes y la visualización. Para conectar una consulta de Log Analytics a un conjunto de datos, copie el código M exportado de Log Analytics en una consulta en blanco en Power BI. 

Para más información, consulte [Conjuntos de datos en el servicio Power BI](/power-bi/service-datasets-understand/). 

## <a name="collect-data-with-power-bi-dataflows"></a>Recopilación de datos con flujos de datos de Power BI 

Los flujos de datos de Power BI también le permiten recopilar y almacenar datos. Para más información, consulte el artículo sobre [flujos de datos de Power BI](/power-bi/service-dataflows-overview).

Un flujo de datos es un tipo de "ETL en la nube" diseñado para ayudarle a recopilar y preparar los datos. Un conjunto de datos es el "modelo" diseñado para ayudarle a conectar entidades diferentes y modelarlas según sus necesidades.

## <a name="incremental-refresh"></a>Actualización incremental 

Tanto los conjuntos de datos de Power BI como los flujos de datos de Power BI tienen una opción de actualización incremental. Los flujos de datos de Power BI y los conjuntos de datos de Power BI admiten esta característica, pero necesita Power BI Premium para usarla.  


La actualización incremental ejecuta consultas pequeñas y actualiza cantidades más pequeñas de datos por cada ejecución en lugar de volver a ingerir todos los datos de nuevo al ejecutar la consulta. Tiene la opción de guardar grandes cantidades de datos, pero agrega un nuevo incremento de los datos cada vez que se ejecuta la consulta. Este comportamiento es idóneo para informes de ejecución prolongada.

La actualización incremental de Power BI se basa en la existencia de un campo de tipo *datetime* archivado en el conjunto de datos. Antes de configurar la actualización incremental, asegúrese de que el conjunto de resultados de la consulta de Log Analytics incluye al menos un campo de tipo *datetime* archivado. 

Para más información y conocer cómo configurar la actualización incremental, consulte [Actualizaciones incrementales en Power BI](/power-bi/service-premium-incremental-refresh) y [Características prémium de flujos de datos](/power-bi/service-dataflows-incremental-refresh).

## <a name="reports-and-dashboards"></a>Informes y paneles

Después de enviar los datos a Power BI, puede seguir usando Power BI para crear informes y paneles.

Para más información, consulte [Creación y uso compartido de su primer informe de Power BI](/learn/modules/build-your-first-power-bi-report/).  

## <a name="excel-integration"></a>integración de Excel

Puede usar la misma integración de M utilizada en Power BI para integrar con una hoja de cálculo de Excel. Para más información, consulte [Importar datos de orígenes de datos externos (Power Query)](https://support.microsoft.com/office/import-data-from-external-data-sources-power-query-be4330b3-5356-486c-a168-b68e9e616f5a) y, a continuación, pegue la consulta M exportada desde Log Analytics.

Puede encontrar información adicional en [Integración de Log Analytics y Excel](log-excel.md).

## <a name="next-steps"></a>Pasos siguientes

[Introducción a las consultas de registro en Azure Monitor](log-query-overview.md)
