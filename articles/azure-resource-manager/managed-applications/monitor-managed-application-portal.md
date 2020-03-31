---
title: Uso de Azure Portal para supervisar una aplicación administrada
description: Muestra cómo usar Azure Portal para supervisar la disponibilidad y las alertas de una aplicación administrada.
author: tfitzmac
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: afe78dd00ecebdc54b6d73c4c8324729e117d95b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75649596"
---
# <a name="monitor-a-deployed-instance-of-a-managed-application"></a>Supervisión de una instancia implementada de una aplicación administrada

Después de haber implementado una aplicación administrada en la suscripción de Azure, es posible que desee comprobar el estado de la aplicación. Este artículo muestra las opciones que hay en Azure Portal para comprobar el estado. Puede supervisar la disponibilidad de los recursos de la aplicación administrada. También puede configurar y ver las alertas.

## <a name="view-resource-health"></a>Visualizar el estado de los recursos

1. Seleccione la instancia de la aplicación administrada.

   ![Selección de la aplicación administrada](./media/monitor-managed-application-portal/select-managed-application.png)

1. Seleccione **Resource Health**.

   ![Seleccionar Resource Health](./media/monitor-managed-application-portal/select-resource-health.png)

1. Vea la disponibilidad de los recursos de la aplicación administrada.

   ![Visualizar el estado de los recursos](./media/monitor-managed-application-portal/view-health.png)

## <a name="view-alerts"></a>Visualización de alertas

1. Seleccione **Alertas**.

   ![Seleccionar Alertas](./media/monitor-managed-application-portal/select-alerts.png)

1. Si tiene reglas de alerta configuradas, verá información sobre las alertas que han surgido.

   ![Visualización de alertas](./media/monitor-managed-application-portal/view-alerts.png)

1. Para agregar reglas de alerta, seleccione **+ Nueva regla de alertas**.

   ![Crear una alerta](./media/monitor-managed-application-portal/create-new-alert.png)

Puede crear alertas para la instancia de aplicación administrada o los recursos de la aplicación administrada. Para más información sobre la creación de alertas, consulte [Información general sobre las alertas en Microsoft Azure](../../azure-monitor/platform/alerts-overview.md).

## <a name="next-steps"></a>Pasos siguientes

* Para ver ejemplos de aplicaciones administradas, consulte [Sample projects for Azure managed applications](sample-projects.md) (Ejemplos de proyectos para aplicaciones administradas de Azure).
* Para implementar una aplicación administrada, consulte [Implementación de la aplicación de catálogo de servicio mediante Azure Portal](deploy-service-catalog-quickstart.md).