---
title: Administración de costos de Azure con Automation
description: En este artículo se explica cómo administrar los costos de Azure con Automation.
author: bandersmsft
ms.author: banders
ms.date: 04/15/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.reviewer: adwise
ms.openlocfilehash: 0727f98b917944f3721c6c6758fde05c2efd8773
ms.sourcegitcommit: 0a5bb9622ee6a20d96db07cc6dd45d8e23d5554a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2020
ms.locfileid: "84449838"
---
# <a name="manage-costs-with-automation"></a>Administración de costos con Automation

Puede usar Automation de Cost Management para crear un conjunto personalizado de soluciones para recuperar y administrar los datos de costos. En este artículo se describen escenarios comunes de Automation de Cost Management y las opciones disponibles en función de su situación. Para el desarrollo mediante API, se presentan ejemplos de solicitudes de API comunes para ayudar a acelerar el proceso de desarrollo.

## <a name="automate-cost-data-retrieval-for-offline-analysis"></a>Automatización de la recuperación de datos de costos para el análisis sin conexión

Es posible que tenga que descargar los datos de costos de Azure para combinarlos con otros conjuntos de datos. O bien, puede que necesite integrar los datos de costos en sus propios sistemas. Hay distintas opciones disponibles en función de la cantidad de datos implicados. En todo caso, debe tener permisos de Cost Management en el ámbito adecuado para usar las API y las herramientas. Para más información, consulte [Asignar el acceso a los datos](https://docs.microsoft.com/azure/cost-management-billing/costs/assign-access-acm-data).

## <a name="suggestions-for-handling-large-datasets"></a>Sugerencias para administrar grandes conjuntos de datos

Si su organización tiene una gran presencia de Azure en muchos recursos o suscripciones, tendrá una gran cantidad de datos de detalles de uso. Por lo general, Excel no puede cargar estos archivos de gran tamaño. En esta situación, se recomiendan las siguientes opciones:

**Power BI**

Power BI se usa para ingerir y controlar grandes cantidades de datos. Si es un cliente de Contrato Enterprise, puede usar la aplicación de plantilla de Power BI para analizar los costos de la cuenta de facturación. El informe contiene las principales vistas que usan los clientes. Para más información, consulte [Análisis de los costos de Azure con la aplicación de la plantilla de Power BI](https://docs.microsoft.com/azure/cost-management-billing/costs/analyze-cost-data-azure-cost-management-power-bi-template-app).

**Conector de datos de Power BI**

Si desea analizar los datos diariamente, se recomienda usar el [conector de datos de Power BI](https://docs.microsoft.com/power-bi/connect-data/desktop-connect-azure-cost-management) para obtener datos para un análisis detallado. El conector mantendrá actualizados todos los informes que cree a medida que se acumulen más costos.

**Exportaciones de Cost Management**

Es posible que no sea necesario analizar los datos a diario. Si es así, considere la posibilidad de usar la característica [Exportaciones](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-export-acm-data) de Cost Management para programar exportaciones de datos a una cuenta de Azure Storage. Después, puede cargar los datos en Power BI según sea necesario o analizarlos en Excel si el archivo es lo suficientemente pequeño. Las exportaciones están disponibles en Azure Portal o puede configurar las exportaciones con [Exports API](https://docs.microsoft.com/rest/api/cost-management/exports).

**Usage Details API**:

Considere la posibilidad de usar [Usage Details API](https://docs.microsoft.com/rest/api/consumption/usageDetails) si tiene un conjunto de datos de costos reducido. Si tiene una gran cantidad de datos de costos, debe solicitar la cantidad más pequeña de datos de uso posible durante un período. Para ello, especifique un intervalo de tiempo pequeño o use un filtro en la solicitud. Por ejemplo, en un escenario en el que necesite tres años de datos de costos, la API funciona mejor si realiza varias llamadas para diferentes intervalos de tiempo en lugar de una sola llamada. Desde allí, puede cargar los datos en Excel para realizar más análisis.

## <a name="automate-retrieval-with-usage-details-api"></a>Automatización de la recuperación con Usage Details API

[Usage Details API](https://docs.microsoft.com/rest/api/consumption/usageDetails) proporciona una manera sencilla de obtener datos de costo sin procesar y sin agregar que se correspondan con su factura de Azure. La API es útil cuando su organización necesita una solución de recuperación de datos mediante programación. Considere la posibilidad de usar la API si quiere analizar conjuntos de datos de costos más reducidos. Sin embargo, debe usar otras soluciones identificadas previamente si tiene conjuntos de datos de mayor tamaño. Los datos de los detalles de uso se proporcionan por medidor y por día. Se usan al calcular la factura mensual. La versión de disponibilidad general (GA) de estas API es `2019-10-01`. Use `2019-04-01-preview` para acceder a la versión preliminar de las compras de reservas y Azure Marketplace con las API.

### <a name="usage-details-api-suggestions"></a>Sugerencias de Usage Details API

**Solicitar programación**

Se recomienda _no realizar más de una solicitud_ a Usage Details API por día. Para obtener más información sobre la frecuencia con que se actualizan los datos de costo y cómo se controla el redondeo, consulte [Descripción de los datos de Cost Management](https://docs.microsoft.com/azure/cost-management-billing/costs/understand-cost-mgt-data#rated-usage-data-refresh-schedule).

**Establecer ámbitos de alto nivel sin filtrado como destino**

Use la API para obtener todos los datos que necesite en el ámbito de nivel más alto disponible. Espere hasta que se ingieran todos los datos necesarios antes de realizar ningún filtrado, agrupación o análisis agregado. La API está optimizada específicamente para proporcionar grandes cantidades de datos de costos sin procesar no agregados. Para más información sobre los ámbitos disponibles en Cost Management, consulte [Descripción y uso de ámbitos](https://docs.microsoft.com/azure/cost-management-billing/costs/understand-work-scopes). Una vez que haya descargado los datos necesarios para un ámbito, use Excel para analizar más datos con filtros y tablas dinámicas.

## <a name="example-usage-details-api-requests"></a>Ejemplo de solicitudes de Usage Details API

Los clientes de Microsoft usan las siguientes solicitudes de ejemplo para abordar escenarios comunes con los que se puede encontrar.

### <a name="get-usage-details-for-a-scope-during-specific-date-range"></a>Obtención de detalles de uso de un ámbito durante un intervalo de fechas específico

Los datos que devuelve la solicitud se corresponden con la fecha en la que el sistema de facturación recibió el uso. Podría incluir los costos de varias facturas.

```http
GET https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?$filter=properties%2FusageStart%20ge%20'2020-02-01'%20and%20properties%2FusageEnd%20le%20'2020-02-29'&$top=1000&api-version=2019-10-01

```

### <a name="get-amortized-cost-details"></a>Obtención de detalles de costos amortizados

Si necesita los costos reales para mostrar las compras que se han acumulado, cambie *metric* a `ActualCost` en la solicitud siguiente. Para usar los costos amortizados y reales, debe usar la versión de `2019-04-01-preview`. La versión actual de la API funciona igual que la versión de `2019-10-01`, excepto en el nuevo atributo type/metric y los nombres de propiedad modificados. Si tiene un Contrato de cliente de Microsoft, los filtros son `startDate` y `endDate` en el ejemplo siguiente.

```http
GET https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?metric=AmortizedCost&$filter=properties/usageStart+ge+'2019-04-01'+AND+properties/usageEnd+le+'2019-04-30'&api-version=2019-04-01-preview
```

## <a name="retrieve-large-cost-datasets-recurringly-with-exports"></a>Recuperación recurrente de conjuntos de datos de costos de gran tamaño con Exportaciones

La característica Exportaciones es una solución para programar volcados de datos de costos de forma periódica. Es el método recomendado para recuperar datos de costos no agregados para organizaciones cuyos archivos de uso pueden ser demasiado grandes para llamar y descargar datos de manera confiable mediante Usage Details API. Los datos se colocan en una cuenta de Azure Storage de su elección. Desde allí, puede cargarlos en sus propios sistemas y analizarlos según necesiten sus equipos. Para configurar las exportaciones en Azure Portal, consulte [Exportar datos](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-export-acm-data).

## <a name="data-latency-and-rate-limits"></a>Latencia de datos y límites de frecuencia

Se recomienda llamar a las API no más de una vez al día. Los datos de Cost Management se actualizan cada cuatro horas a medida que se reciben nuevos datos de uso de los proveedores de recursos de Azure. Llamar con mayor frecuencia no proporcionará datos adicionales. En su lugar, creará una carga mayor. Para más información sobre la frecuencia con que cambian los datos y cómo se controla su latencia, consulte [Descripción de los datos de Cost Management](https://docs.microsoft.com/azure/cost-management-billing/costs/understand-cost-mgt-data).

### <a name="error-code-429---call-count-has-exceeded-rate-limits"></a>Código de error 429: el recuento de llamadas ha superado los límites de frecuencia

Para permitir una experiencia coherente para todos los suscriptores de Cost Management, las Cost Management API tienen una frecuencia limitada. Cuando se alcance este límite, recibirá el código de estado HTTP `429: Too many requests`. Los límites de rendimiento actuales para nuestras API son los siguientes:

- 30 llamadas por minuto: se aplica por ámbito, por usuario o por aplicación.
- 200 llamadas por minuto: se aplica por inquilino, por usuario o por aplicación.

## <a name="next-steps"></a>Pasos siguientes

- [Análisis de los costos de Azure con la aplicación de la plantilla de Power BI](https://docs.microsoft.com/azure/cost-management-billing/costs/analyze-cost-data-azure-cost-management-power-bi-template-app).
- [Creación y administración de datos exportados](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-export-acm-data) con Exportaciones.
- Obtenga más información sobre [Usage Details API](https://docs.microsoft.com/rest/api/consumption/usageDetails).