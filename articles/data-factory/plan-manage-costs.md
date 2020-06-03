---
title: Planeamiento y administración de los costos de Azure Data Factory
description: En este artículo se describe cómo puede planear y administrar los costos de Azure Data Factory.
documentationcenter: ''
author: shirleywangmsft
ms.author: shwang
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/14/2020
ms.openlocfilehash: ca76563475dfbf8d35595c1de3cdee37f80e3ce1
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83690241"
---
# <a name="plan-and-manage-costs-for-azure-data-factory"></a>Planeamiento y administración de los costos de Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Azure Data Factory es un servicio de integración de datos elástico y sin servidor diseñado para el escalado en la nube.  Esto significa que no es necesario planear ningún proceso de tamaño fijo para las cargas máximas; en su lugar, se especifica la cantidad de recursos que se asignarán a petición por operación, lo que permite diseñar los procesos ETL de una manera mucho más escalable. Además, el plan de ADF se factura según el consumo, por lo que solo paga por lo que usa.

En este artículo se describe cómo puede planear y administrar los costos de Azure Data Factory.

*   En primer lugar, al principio del proyecto ETL, puede realizar una prueba de concepto y usar una combinación de consumo por canalización y calculadora de precios para calcular los costos.
*   Una vez que haya implementado las canalizaciones en la fase de producción, use las características de administración de costos para configurar presupuestos y supervisar los costos. También puede revisar los costos previstos e identificar las tendencias de gasto.
*   Además, puede ver el consumo de cada canalización y la información de consumo de cada actividad para comprender qué canalizaciones y actividades son más costosas e identificar a los candidatos para la reducción de costos.

## <a name="estimate-costs-using-pipeline-and-activity-run-consumption-and-pricing-calculator"></a>Cotización de costos mediante el consumo de las ejecuciones de canalizaciones y actividad y la calculadora de precios

Puede usar la [calculadora de precios de ADF](https://azure.microsoft.com/pricing/calculator/?service=data-factory) para obtener una cotización del costo por ejecutar la carga de trabajo ETL en Azure Data Factory.  Para usar la calculadora, debe especificar detalles como el número de ejecuciones de actividad, el número de horas por unidad de integración de datos, el tipo de proceso que se usará para Data Flow, el número de núcleos, el recuento de instancias y la duración de la ejecución, entre otros.

Una de las preguntas más frecuentes sobre la calculadora de precios es qué valores se deben usar como entradas.  Durante la fase de prueba de concepto, puede realizar ejecuciones de prueba con conjuntos de datos de ejemplo para comprender el consumo de varios medidores de ADF.  Después, en función del consumo para el conjunto de datos de ejemplo, puede realizar una proyección del consumo de todo el conjunto de datos y de la programación de la operacionalización.

> [!NOTE]
> Los precios usados en los ejemplos siguientes son hipotéticos y no pretenden reflejar precios reales.

Por ejemplo, supongamos que necesita trasladar 1 TB de datos diariamente de AWS S3 a Azure Data Lake Gen2.  Puede realizar una prueba de concepto en la que se mueven 100 GB de datos para medir el rendimiento de la ingesta de datos y comprender el consumo de facturación correspondiente.

Este es un ejemplo sobre los detalles de ejecución de una actividad de copia (su consumo real variará en función de la forma de su conjunto de datos específico, las velocidades de red, los límites de salida de la cuenta de S3, los límites de entrada en ADLS Gen2 y otros factores).

![Ejecución de copia en S3](media/plan-manage-costs/s3-copy-run-details.png)

Gracias a la [supervisión del consumo de en el nivel de ejecución de canalización](#monitor-consumption-at-pipeline-run-level), puede ver las cantidades de consumo de las mediciones de movimiento de datos correspondientes:

![Consumo de la canalización de copia en S3](media/plan-manage-costs/s3-copy-pipeline-consumption.png)

Por lo tanto, el número total de horas por DIU necesarias para mover 1 TB al día durante todo el mes es:

1,2667 (horas por DIU) * (1 TB/100 GB) * 30 (días en un mes) = 380 DIU por hora

Ahora puede introducir 30 ejecuciones de actividad y 380 horas por DIU a la calculadora de precios de ADF para obtener una estimación de la factura mensual:

![Calculadora de precios de una copia de S3](media/plan-manage-costs/s3-copy-pricing-calculator.png)

## <a name="use-budgets-and-cost-alerts"></a>Uso de presupuestos y alertas de costos

Puede crear [presupuestos](https://docs.microsoft.com/azure/cost-management/tutorial-acm-create-budgets) para administrar los costos y crear alertas que envíen notificaciones automáticamente a las partes interesadas sobre anomalías en los gastos y riesgos de gastos adicionales.  Las alertas se basan en el gasto comparado con los umbrales de presupuesto y costo.  Podrá crear un presupuesto en el nivel de suscripción o en una granularidad más baja al agregar filtros adicionales, como el identificador de recurso y el nombre del medidor.  Sin embargo, no puede crear presupuestos para las canalizaciones individuales dentro de una factoría.

## <a name="monitor-costs-at-factory-level"></a>Supervisión de costos en el nivel de factoría

Cuando comience a usar Azure Data Factory, podrá ver los costos en los que se incurre en el panel [Análisis de costos](https://docs.microsoft.com/azure/cost-management/quick-acm-cost-analysis) de Azure Portal.

1. Para ver el [análisis de costos](https://docs.microsoft.com/azure/cost-management/quick-acm-cost-analysis), abra la ventana **Administración de costos y facturación**, seleccione **Administración de costos** en el menú y, a continuación, seleccione **Abrir análisis de costos**.
2. La vista predeterminada muestra los costos acumulados del mes en curso.  Puede cambiar a un intervalo de tiempo diferente y a una granularidad diferente; por ejemplo, diaria o mensual.
3. Para restringir los costos de un único servicio, como Azure Data Factory, seleccione **Agregar filtro** y, a continuación, seleccione **Nombre del servicio**.  A continuación, elija **Azure Data Factory v2** de la lista.
4. Puede agregar filtros adicionales para analizar los costos de una instancia de factoría específica y una granularidad de medidor de ADF específica.

   ![Análisis de costos](media/plan-manage-costs/cost-analysis.png)

## <a name="monitor-consumption-at-pipeline-run-level"></a>Supervisión del consumo en el nivel de ejecución de canalización

En función de los tipos de actividades que tenga en la canalización, la cantidad de datos que se mueven y transforman, y la complejidad de la transformación, la ejecución de una canalización activará distintos medidores de facturación en Azure Data Factory.

Puede ver la cantidad de consumo de medidores distintos para ejecuciones de canalización individuales en la experiencia de usuario de Azure Data Factory. Para abrir la experiencia de supervisión, seleccione el icono **Supervisar y administrar** de la hoja de la factoría de datos de [Azure Portal](https://portal.azure.com/). Si ya está en ADF UX, haga clic en el icono **Supervisar** de la barra lateral izquierda. La vista de supervisión predeterminada es una lista de ejecuciones de la canalización.

Al hacer clic en el botón **Consumo** situado junto al nombre de la canalización, se mostrará una ventana emergente con el consumo de la ejecución de canalización agregada de todas las actividades de la canalización.

![Consumo de la ejecución de canalización](media/plan-manage-costs/pipeline-run-consumption.png)

![Detalles de consumo de la canalización](media/plan-manage-costs/pipeline-consumption-details.png)

La vista del consumo de la ejecución de canalización muestra el la cantidad de cada medidor de ADF que consumió la ejecución de canalización específica, pero no muestra el precio real que se cobrará, ya que la cantidad facturada depende del tipo de cuenta de Azure y del tipo de moneda que se usará.  Para ver la lista completa de tipos de cuenta compatibles, consulte [Understand Cost Management data](https://docs.microsoft.com/azure/cost-management-billing/costs/understand-cost-mgt-data) (Información sobre los datos de Cost Management).

## <a name="monitor-consumption-at-activity-run-level"></a>Supervisión del consumo en el nivel de ejecución de actividad
Una vez que comprenda el consumo agregado en el nivel de ejecución de la canalización, existen escenarios en los que es necesario profundizar e identificar cuál es la actividad más costosa de la canalización.

Para ver el consumo en el nivel de la ejecución de la actividad, vaya a la opción **Crear y supervisar** de la interfaz de usuario de su factoría de datos. Desde la pestaña **Supervisión** en la que se muestra una lista de ejecuciones de canalización, haga clic en el vínculo del **nombre de canalización** para acceder a la lista de ejecuciones de actividad en la ejecución de canalización.  Haga clic en el botón **Salida** situado junto al nombre de la actividad y busque la propiedad **billableDuration** en la salida JSON:

El siguiente es un ejemplo de la ejecución de una actividad de copia:

![Salida de copia](media/plan-manage-costs/copy-output.png)

Y este es un ejemplo de la ejecución de una actividad de flujo de datos de asignación:

![Salida de flujo de datos](media/plan-manage-costs/dataflow-output.png)

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos para obtener información sobre cómo funcionan los precios en Azure Data Factory:

- [Página de precios de Azure Data Factory](https://azure.microsoft.com/pricing/details/data-factory/ssis/)
- [Información sobre Azure Data Factory a través de ejemplos](https://docs.microsoft.com/azure/data-factory/pricing-concepts)
- [Calculadora de precios de Azure Data Factory](https://azure.microsoft.com/pricing/calculator/?service=data-factory)
