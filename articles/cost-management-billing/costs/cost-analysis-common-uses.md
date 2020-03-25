---
title: Usos del análisis de costos comunes en Azure Cost Management
description: En este artículo se explica cómo puede obtener los resultados de las tareas comunes de análisis de costos en Azure Cost Management.
author: bandersmsft
ms.author: banders
ms.date: 02/21/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.reviewer: adwise
ms.openlocfilehash: 901f271ac401cb985e59c434b9e6d7f8db03889f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "79203107"
---
# <a name="common-cost-analysis-uses"></a>Usos del análisis de costos comunes

Los usuarios de Azure Cost Management suelen querer respuesta a preguntas que muchos otros formulan. En este artículo se explica cómo obtener los resultados de las tareas comunes de análisis de costos en Cost Management.

## <a name="view-cost-breakdown-by-azure-service"></a>Visualización del desglose de costos por servicio de Azure

La visualización de los costos por un servicio de Azure puede ayudarle a conocer mejor qué partes de su infraestructura son las más caras. Por ejemplo, los costos de proceso de la máquina virtual pueden ser pequeños. Aún así, es posible que se acumulen costos de red significativos debido a la cantidad de información que las máquinas virtuales emiten. Conocer los principales costos de los servicios de Azure es esencial para ajustar el uso del servicio según sea necesario.

1. En Azure Portal, vaya al análisis de los costos para su ámbito. Por ejemplo: **Administración de costos + facturación** > **Administración de costos** > **Análisis de costos**.
1. Seleccione **Costo por servicio** y, después, agrupe por **Nivel de servicio**.
1. Cambie la vista a **Tabla**.

![Visualización del desglose de costos por servicio de Azure](./media/cost-analysis-common-uses/breakdown-by-service.png)

## <a name="view-cost-breakdown-by-azure-resource"></a>Visualización del desglose de costos por recurso de Azure

Los servicios se compilan con los recursos de Azure. Revisar los costos en función de los recursos puede ayudarle a identificar rápidamente quiénes contribuyen al costo principal. Si un servicio tiene recursos que son demasiado caros, considere la posibilidad de realizar cambios para reducir los costos.

1. En Azure Portal, vaya al análisis de los costos para su ámbito. Por ejemplo: **Administración de costos + facturación** > **Administración de costos** > **Análisis de costos**.
1. Seleccione **Costo por recurso**.
1. Cambie la vista a **Tabla**.

![Visualización del desglose de costos por recurso de Azure](./media/cost-analysis-common-uses/cost-by-resource.png)

## <a name="view-cost-breakdown-by-selected-dimensions"></a>Visualización del desglose de costos por dimensiones seleccionadas

Las dimensiones permiten organizar los costos en función de los distintos valores de metadatos que se muestran en los cargos. Por ejemplo, podría agrupar los costos por ubicación.

1. En Azure Portal, vaya al análisis de los costos para su ámbito. Por ejemplo: **Administración de costos + facturación** > **Administración de costos** > **Análisis de costos**.
1. Seleccione el filtro **Agrupar por**.  
    ![Selección de un elemento Agrupar por](./media/cost-analysis-common-uses/group-by.png)
1. También puede guardar la vista para usarla después.
1. Haga clic en un gráfico circular debajo del gráfico para ver datos más detallados.  
    ![Visualización del desglose de costos por dimensiones seleccionadas](./media/cost-analysis-common-uses/drill-down.png)

## <a name="view-costs-per-day-or-by-month"></a>Ver costos por día o por mes

Examinar los costos diarios y mensuales puede ayudarle a comprender mejor si hay una hora de la semana o del año en la que los costos son mayores. Si tiene más tráfico de clientes en un período de vacaciones, ¿eso conlleva un aumento correspondiente en los costos de Azure? ¿Es el viernes un día más caro que el lunes?

1. En Azure Portal, vaya al análisis de los costos para su ámbito. Por ejemplo: **Administración de costos + facturación** > **Administración de costos** > **Análisis de costos**.
1. Establezca la **Granularidad** en **Mensual** o **Diaria**.

![Ver costos por día](./media/cost-analysis-common-uses/daily-granularity.png)

## <a name="view-costs-for-a-specific-tag"></a>Ver los costos de una etiqueta específica

Muchos usuarios de Azure aplican etiquetas a sus recursos, como un centro de costos o un entorno de desarrollo (producción y pruebas), para clasificar mejor los cargos. Las etiquetas aparecen como una dimensión en el análisis de costos. Puede usar la dimensión para obtener información detallada sobre las categorías de etiquetas personalizadas.

La compatibilidad con etiquetas se aplica al uso indicado *después* de que la etiqueta se aplicara al recurso. Las etiquetas no se aplican de forma retroactiva para los resúmenes de costos.

1. En Azure Portal, vaya al análisis de los costos para su ámbito. Por ejemplo: **Administración de costos + facturación** > **Administración de costos** > **Análisis de costos**.
1. Seleccione **Agrupar por** para la etiqueta.

![Ver los costos de una etiqueta específica](./media/cost-analysis-common-uses/tag.png)

## <a name="download-your-usage-details"></a>Descarga de los detalles de uso

El archivo de informe de detalles de uso, en formato CSV, proporciona un desglose de todos los cargos que se acumulan en una factura. Puede usar el informe para compararlo con la factura y comprenderla mejor. Cada cargo de la factura corresponde a los cargos desglosados en el informe de uso.

1. En Azure Portal, vaya a la pestaña **Usage and Charges** (Uso y cargos) para una cuenta de facturación o una suscripción. Por ejemplo: **Administración de costos + facturación** > **Facturación** > **Usage + charges** (Uso y cargos).
1. Seleccione el elemento de la línea del que desea descargar y, a continuación, haga clic en el símbolo de descarga.  
    ![Descarga del uso y los cargos](./media/cost-analysis-common-uses/download1.png)
1.  Seleccione el archivo de uso que se va a descargar.  
    ![Elección del archivo de uso que se va a descargar](./media/cost-analysis-common-uses/download2.png)

## <a name="view-monthly-ea-cost-breakdown"></a>Visualización del desglose mensual del costo de EA

La inscripción de EA acumula los costos de toda la organización. Entender cómo se acumulan los costos y se facturan a lo largo del tiempo ayuda a atraer a las partes interesadas adecuadas para garantizar que se administren de forma responsable.

Solo se muestran los costos de la inscripción activa. Si transfirió una inscripción (inactiva) a una nueva (activa), los costos de la inscripción anterior no se muestran en Cost Management.


1. En Azure Portal, vaya a **Administración de costos + facturación** > **Información general**.
1. Haga clic en **Desglose** para el mes en curso y vea la evolución del compromiso monetario.  
    ![Información general sobre costos de EA: desglose](./media/cost-analysis-common-uses/breakdown1.png)
1.  Haga clic en la pestaña **Usage and Charges** (Uso y cargos) y vea el desglose del mes anterior en el intervalo de tiempo seleccionado.  
    ![Pestaña Usage and charges (Uso y cargos)](./media/cost-analysis-common-uses/breakdown2.png)

## <a name="view-enrollment-monthly-cost-by-term"></a>Visualización del costo mensual de inscripción por trimestre

Use una vista gráfica de los costos mensuales de su inscripción para conocer las tendencias de costos y los importes facturados durante un período determinado.

1. En Azure Portal, vaya al análisis de los costos para su ámbito. Por ejemplo: **Administración de costos + facturación** > **Administración de costos** > **Análisis de costos**.
1. Seleccione su inscripción y establezca su trimestre.
1. Establezca una granularidad mensual y, a continuación, establezca la vista en **Columna (apilada)** .

Puede agrupar y filtrar los datos para un análisis más detallado.

![Costo de inscripción mensual por trimestre](./media/cost-analysis-common-uses/enrollment-term1.png)

## <a name="view-ea-enrollment-accumulated-costs"></a>Visualización de los costos acumulados de inscripción de EA

Vea los cargos acumulados netos a lo largo del tiempo para conocer los gastos generales de su organización durante un período determinado.

1. En Azure Portal, vaya al análisis de los costos para su ámbito. Por ejemplo: **Administración de costos + facturación** > **Administración de costos** > **Análisis de costos**.
1. Seleccione su inscripción y, a continuación, vea los costos acumulados actuales.

![Costos acumulados de inscripción](./media/cost-analysis-common-uses/cost-analysis-enrollment.png)

## <a name="next-steps"></a>Pasos siguientes
- Si aún no ha completado la primera guía rápida de Cost Management, léala en [Start analyzing costs](quick-acm-cost-analysis.md) (Comenzar a analizar los costos).
- Lea la [documentación sobre Cost Management](../index.yml).
