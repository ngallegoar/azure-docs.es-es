---
title: Procedimientos recomendados para usar Almacén de consultas en Azure Database for PostgreSQL con un único servidor
description: En este artículo se describen los procedimientos recomendados para usar Almacén de consultas en Azure Database for PostgreSQL con un único servidor.
author: sunilagarwal
ms.author: sunila
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: dd39b7ecd51902f5035b4cd17d59dea964d0c962
ms.sourcegitcommit: 19dce034650c654b656f44aab44de0c7a8bd7efe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2020
ms.locfileid: "91708839"
---
# <a name="best-practices-for-query-store"></a>Procedimientos recomendados para usar Almacén de consultas

**Se aplica a:** Azure Database for PostgreSQL: Versiones de servidor único 9.6, 10, 11

En este artículo se describen los procedimientos recomendados para usar Almacén de consultas en Azure Database for PostgreSQL.

## <a name="set-the-optimal-query-capture-mode"></a>Establecimiento del modo de captura de consultas óptimo
Permite que Almacén de consultas capture los datos realmente relevantes. 

|**pg_qs.query_capture_mode** | **Escenario**|
|---|---|
|_Todo_  |Analice la carga de trabajo exhaustivamente en todas las consultas, así como sus frecuencias de ejecución y otras estadísticas. Identifique nuevas consultas en la carga de trabajo. Detecte si las consultas ad-hoc se usan para identificar oportunidades de parametrización automática o manual. _All_ (Todas) aumenta el costo de consumo de recursos. |
|_Top_ (Principales)  |Centre la atención en las consultas principales; las emitidos por los clientes.
|_None_ |Ya ha capturado un conjunto de consultas y el período de tiempo que desea investigar, y también desea eliminar los objetos innecesarios que pueden introducir otras consultas. La opción _None_ (Ninguna) es adecuada para entornos de pruebas y evaluación comparativa. Además, _None_ (Ninguna) debe usarse con precaución, ya que podría perder la oportunidad de realizar un seguimiento de las consultas nuevas importantes y de optimizarlas. No se pueden recuperar datos en períodos de tiempo transcurridos. |

Almacén de consultas también incluye un almacén para estadísticas de espera. Hay una consulta de modo de captura adicional que rige las estadísticas de espera: **pgms_wait_sampling.query_capture_mode** se puede establecer en _None_ (Ninguna) o _All_ (Todo). 

> [!NOTE] 
> **pg_qs.query_capture_mode** reemplaza a **pgms_wait_sampling.query_capture_mode**. Si el valor de pg_qs.query_capture_mode es _None_ (Ninguna), la configuración de pgms_wait_sampling.query_capture_mode no tiene ningún efecto. 


## <a name="keep-the-data-you-need"></a>Conservación de los datos necesarios
El parámetro **pg_qs.retention_period_in_days** especifica en días el período de retención de datos para Almacén de consultas. Se eliminan los datos de consulta y las estadísticas anteriores. De forma predeterminada, Almacén de consultas está configurado para conservar los datos durante siete días. Evite mantener datos históricos que no va a usar. Aumente el valor si necesita mantener los datos más tiempo.


## <a name="set-the-frequency-of-wait-stats-sampling"></a>Establecimiento de la frecuencia de muestreo de estadísticas de espera 
El parámetro **pgms_wait_sampling.history_period** especifica la frecuencia (en milisegundos) de muestreo de los eventos de espera. Cuanto más corto sea el período, más frecuente será el muestreo. Se recupera más información, pero a costa de un mayor consumo de recursos. Aumente este período si el servidor está bajo carga o si no necesita la granularidad.


## <a name="get-quick-insights-into-query-store"></a>Obtención de información rápida en Almacén de consultas
Puede usar [Información de rendimiento de consultas](concepts-query-performance-insight.md) en Azure Portal para obtener información rápida sobre los datos en Almacén de consultas. Con el tiempo, las visualizaciones usan consultas de ejecución y eventos de espera más largos.

## <a name="next-steps"></a>Pasos siguientes
- Obtenga información sobre cómo obtener o establecer los parámetros mediante [Azure Portal](howto-configure-server-parameters-using-portal.md) o la [CLI de Azure](howto-configure-server-parameters-using-cli.md).