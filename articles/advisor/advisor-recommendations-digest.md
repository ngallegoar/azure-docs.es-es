---
title: Resumen de recomendaciones para Azure Advisor
description: Obtenga un resumen periódico de las recomendaciones activas
ms.topic: article
ms.date: 03/16/2020
ms.author: sagupt
ms.openlocfilehash: 98f1bfd42a486e005cd1e777a83f5b615a7c8304
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79503920"
---
# <a name="configure-periodic-summary-for-recommendations"></a>Configuración del resumen periódico de recomendaciones

Los **resúmenes de recomendaciones** de Advisor ofrecen una manera fácil y proactiva de mantenerse al día con las recomendaciones activas en distintas categorías. La característica permite configurar notificaciones periódicas para el resumen de todas las recomendaciones activas en distintas categorías. Puede elegir el canal que quiera para las notificaciones, (como el correo electrónico, SMS u otros) mediante grupos de acciones. En este artículo se muestra cómo configurar un **resumen** para las recomendaciones de Advisor.


## <a name="setting-up-your-recommendation-digest"></a>Configuración del resumen de recomendaciones 

La experiencia de creación del **resumen de recomendaciones** ayuda a configurar el resumen. Puede seleccionar los siguientes parámetros para las configuraciones:
1. Categoría: Tenemos categorías de recomendaciones tales como costo, alta disponibilidad, rendimiento y excelencia operativa. La funcionalidad todavía no está disponible para las recomendaciones de seguridad.
2. Frecuencia de resúmenes: La frecuencia de las notificaciones de resumen puede ser semanal, quincenal y mensual.
3. Grupo de acciones: Puede seleccionar un grupo de acciones existente o crear uno nuevo. Para más información sobre los grupos de acciones, consulte el artículo [Creación y administración de grupos de acciones](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups).
4. Idioma del resumen
5. Nombre del resumen de recomendaciones: Puede usar una cadena descriptiva para supervisar y realizar un seguimiento de los resúmenes con mayor facilidad.

## <a name="steps-to-create-recommendation-digest-in-azure-portal"></a>Pasos para crear el resumen de recomendaciones en Azure Portal

Estos son los pasos para crear un **resumen de recomendaciones:**
* **Paso 1:** En Azure Portal, vaya a **Advisor** y en la sección **Supervisión**, seleccione **Recommendation digest** (Resumen de recomendaciones). 

   ![Punto de entrada del resumen de recomendaciones](./media/digest-0.png)

* **Paso 2:** Seleccione **New recommendation digest** (Nuevo resumen de recomendaciones) en la barra superior como se indica a continuación:

   ![Creación del resumen de recomendaciones](./media/digest-5.png)

* **Paso 3:** En la sección **ámbito**, seleccione la **suscripción** para el resumen.

   ![Proporcionar los datos del resumen de recomendaciones](./media/digest-1.png)

* **Paso 4:** En la sección **condición**, seleccione las opciones de configuración como **categoría**, **frecuencia** e **idioma**.

   ![Proporcionar las condiciones del resumen de recomendaciones](./media/digest-2.png)

* **Paso 5:** En la sección **grupo de acciones**, seleccione el **grupo de acciones** del resumen. Puede obtener más información aquí: [Creación y administración de grupos de acciones](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups)

   ![Proporcionar el grupo de acciones del resumen de recomendaciones](./media/digest-3.png)

* **Paso 6:** En esta última sección para los **detalles de resumen**, puede asignar un nombre y un estado al resumen de recomendación. Presione **Create recommendation digest** (Crear resumen de recomendaciones) para completar la configuración.
   ![Creación del resumen de recomendación completa](./media/digest-4.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las recomendaciones de Advisor, consulte:
* [Introducción a Azure Advisor](advisor-overview.md)
* [Introducción a Advisor](advisor-get-started.md)
* [Recomendaciones sobre el costo de Advisor](advisor-cost-recommendations.md)
* [Recomendaciones sobre rendimiento de Advisor](advisor-performance-recommendations.md)
* [Recomendaciones sobre seguridad de Advisor](advisor-security-recommendations.md)
* [Recomendaciones de excelencia operativa de Advisor](advisor-operational-excellence-recommendations.md)
* [API REST de Advisor](https://docs.microsoft.com/rest/api/advisor/)
