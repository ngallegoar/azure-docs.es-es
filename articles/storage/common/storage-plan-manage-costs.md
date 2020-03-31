---
title: Planeamiento y administración de los costos de Azure Storage
description: Obtenga información acerca de cómo planear y administrar los costos de Azure Storage mediante el análisis de costos en Azure Portal.
services: storage
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: normesta
ms.subservice: common
ms.custom: subject-cost-optimization
ms.openlocfilehash: aa0b789b31f50c8b1ccf5450700874a02ad4664c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78304363"
---
# <a name="plan-and-manage-costs-for-azure-storage"></a>Planeamiento y administración de los costos de Azure Storage

En este artículo se describe cómo puede planear y administrar los costos de Azure Storage. En primer lugar, use la calculadora de precios de Azure para ayudar a planear los costos de almacenamiento antes de agregar recursos. Después de comenzar a usar los recursos de Azure Storage, use las características de administración de costos para establecer presupuestos y supervisar los costos. También puede revisar los costos previstos y supervisar las tendencias de gasto para identificar las áreas en las que podría querer actuar.

Tenga en cuenta que los costos de Azure Storage son solo una parte de los costos mensuales de la factura de Azure. Aunque en este artículo se explica cómo planear y administrar los costos de Azure Storage, se le facturarán todos los servicios y recursos de Azure usados para su suscripción de Azure, incluidos los servicios de terceros. Una vez que esté familiarizado con la administración de los costos de Azure Storage, puede aplicar métodos similares para administrar los costos de todos los servicios de Azure que se usan en la suscripción.

## <a name="prerequisites"></a>Prerrequisitos

El análisis de costos es compatible con varios tipos de cuenta de Azure. Para ver la lista completa de tipos de cuenta compatibles, consulte [Understand Cost Management data](../../cost-management-billing/costs/understand-cost-mgt-data.md) (Información sobre los datos de Cost Management). Para ver los datos de costos, se necesita al menos acceso de lectura en la cuenta de Azure. Para más información acerca de cómo asignar acceso a los datos de Azure Cost Management, consulte [Asignación de acceso a los datos](../../cost-management-billing/costs/assign-access-acm-data.md).

## <a name="estimate-costs-before-creating-an-azure-storage-account"></a>Estimación de los costos antes de crear una cuenta de Azure Storage

Use la [calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/) para calcular los costos antes de crear y comenzar a transferir datos a una cuenta de Azure Storage.

1. En la página [Calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/), elija el icono **Cuentas de almacenamiento**.

2. Desplácese hacia abajo en la página y busque la sección **Cuentas de almacenamiento** de la estimación.

3. Elija las opciones de las listas desplegables. 

   A medida que se modifica el valor de estas listas desplegables, cambia el costo estimado. Esa estimación aparece en la esquina superior, así como en la parte inferior de la estimación. 
    
   ![Supervisión de costos con el panel Análisis de costos](media/storage-plan-manage-costs/price-calculator-storage-type.png)

   A medida que cambia el valor de la lista desplegable **Tipo**, también cambian otras opciones que aparecen en esta hoja de cálculo. Use los vínculos de la sección **Más información** para obtener más información sobre el significado de cada opción y cómo afectan al precio de las operaciones relacionadas con el almacenamiento. 

4. Modifique las demás opciones cómo afectan a la estimación.

## <a name="use-budgets-and-cost-alerts"></a>Uso de presupuestos y alertas de costos

Puede crear [presupuestos](../../cost-management-billing/costs/tutorial-acm-create-budgets.md) para administrar los costos y crear alertas que envíen notificaciones automáticamente a las partes interesadas sobre anomalías en los gastos y riesgos de gastos adicionales. Las alertas se basan en el gasto comparado con los umbrales de presupuesto y costo. Los presupuestos y las alertas se crean para las suscripciones y los grupos de recursos de Azure, por lo que son útiles como parte de una estrategia general de supervisión de costos. Sin embargo, pueden tener una funcionalidad limitada para administrar los costos de servicios individuales de Azure, como el costo de Azure Storage, porque están diseñados para realizar un seguimiento de los costos en un nivel más alto.

## <a name="monitor-costs"></a>Supervisión de costos

A medida que se usan recursos con Azure Storage, se incurre en costos. Los costos de la unidad de uso de recursos varían en función de intervalos de tiempo (segundos, minutos, horas y días) o en función del uso de unidades (bytes, megabytes, etc.). Tan pronto como empieza el uso de Azure Storage, se incurre en costos. Puede ver los costos en el panel [Análisis de costos](../../cost-management-billing/costs/quick-acm-cost-analysis.md) de Azure Portal.

Al usar el análisis de costos, puede ver los costos de Azure Storage en gráficos y tablas para diferentes intervalos de tiempo. Algunos ejemplos son: por día, mes actual y anterior y año. También puede ver los costos comparados con los presupuestos y los costos previstos. Cambiar a vistas más largas en el tiempo puede ayudarle a identificar las tendencias de gasto y ver dónde podría haber ocurrido un gasto excesivo. Si ha creado presupuestos, también podrá ver fácilmente dónde se han excedido.

Para ver los costos de Azure Storage en los análisis de costos:

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com).

2. Abra la ventana **Administración de costos y facturación**, seleccione **Administración de costos** en el menú y, a continuación, seleccione **Análisis de costos**. A continuación, puede cambiar el ámbito a una suscripción específica en la lista desplegable **Ámbito**.

   ![Supervisión de costos con el panel Análisis de costos](./media/storage-plan-manage-costs/cost-analysis-pane.png)

4. Para ver los costos de Azure Storage, seleccione **Agregar filtro** y, a continuación, seleccione **Nombre del servicio**. Después, elija **almacenamiento** en la lista. 

   Este es un ejemplo que muestra solo los costos de Azure Storage:

   ![Supervisión de costos de almacenamiento con el panel Análisis de costos](./media/storage-plan-manage-costs/cost-analysis-pane-storage.png)

En el ejemplo anterior, hemos visto el costo actual del servicio. También se muestran los costos por regiones de Azure (ubicaciones) y por grupo de recursos.  

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre la administración de costos con los [análisis de costos](../../cost-management-billing/costs/quick-acm-cost-analysis.md).

Consulte los siguientes artículos para obtener información sobre cómo funcionan los precios con Azure Storage:

- [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
- [Optimizar los costos de almacenamiento de blobs con capacidad reservada](../blobs/storage-blob-reserved-capacity.md)
