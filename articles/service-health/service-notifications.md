---
title: Visualización de las notificaciones de mantenimiento del servicio mediante Azure Portal
description: Visualice las notificaciones de mantenimiento del servicio en Azure Portal. Las notificaciones de mantenimiento del servicio se publican mediante la infraestructura de Azure en el registro de actividad de Azure.
ms.topic: conceptual
ms.date: 6/27/2019
ms.openlocfilehash: 615d08b6a04aef9e8ef2033154da8ff8caeebe04
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2020
ms.locfileid: "90967769"
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Visualización de las notificaciones de mantenimiento del servicio mediante Azure Portal

Las notificaciones de mantenimiento del servicio se publican mediante la infraestructura de Azure en el [registro de actividad de Azure](../azure-monitor/platform/platform-logs-overview.md).  Las notificaciones contienen información acerca de los recursos en la suscripción. Debido al volumen posiblemente grande de la información almacenada en el registro de actividad, hay una interfaz de usuario independiente que facilita la visualización y la configuración de alertas en las notificaciones de mantenimiento del servicio. 

En función de la clase, las notificaciones de mantenimiento del servicio pueden ser meramente informativas o pueden requerir acciones.

Para obtener más información sobre las distintas clases de notificaciones de mantenimiento del servicio, vea [Propiedades de las notificaciones de Service Health](service-health-notifications-properties.md).

## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>Visualización de las notificaciones de mantenimiento del servicio en Azure Portal

1. En [Azure Portal](https://portal.azure.com), seleccione **Supervisión**.

    ![Captura de pantalla del menú de Azure Portal, con el Monitor seleccionado](./media/service-notifications/home-monitor.png)

    Azure Monitor se abre junto con toda la configuración de supervisión y los datos en una sola vista consolidada. Primero se abre la sección **Registro de actividades** .

1. Seleccione **Alertas**.

    ![Captura de pantalla del registro de actividad del Monitor, con Alertas seleccionado](./media/service-notifications/service-health-summary.png)

1. Seleccione **+Agregar alerta del registro de actividad** y configure una alerta para asegurarse de que recibe las futuras notificaciones del servicio. Para más información, consulte [Creación de alertas del registro de actividad en notificaciones del servicio](./alerts-activity-log-service-notifications-portal.md).

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre las [alertas del registro de actividad](../azure-monitor/platform/activity-log-alerts.md).
