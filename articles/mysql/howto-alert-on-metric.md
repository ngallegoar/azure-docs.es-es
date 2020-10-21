---
title: Configuración de alertas de métricas mediante Azure Portal de Azure Database for MySQL
description: En este artículo se describe cómo configurar las alertas de métricas de Azure Database for MySQL, y obtener acceso a ellas, mediante Azure Portal.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 3/18/2020
ms.openlocfilehash: 3891d1730648fea3c480cd03921a76fe005c308f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91740061"
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-mysql"></a>Uso de Azure Portal para configurar alertas de métricas para Azure Database for MySQL 

En este artículo se explica cómo configurar alertas de Azure Database for MySQL mediante Azure Portal. Puede recibir una alerta basada en las métricas de supervisión para los servicios de Azure.

La alerta se desencadena cuando el valor de una métrica especificada cruza el umbral que se ha asignado. La alerta se desencadena tanto la primera vez que se cumple la condición como después, cuando dicha condición deja de cumplirse. 

Puede configurar una alerta para realizar las siguientes acciones cuando se desencadene:
* Enviar notificaciones de correo electrónico al administrador y los coadministradores del servicio.
* Enviar un correo electrónico a direcciones de correo electrónico adicionales que especifique.
* Llamar a un webhook

Puede obtener información sobre las reglas de alerta y configurarlas mediante:
* [Azure Portal](../azure-monitor/platform/alerts-metric.md#create-with-azure-portal)
* [CLI de Azure](../azure-monitor/platform/alerts-metric.md#with-azure-cli)
* [API de REST de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/metricalerts)

## <a name="create-an-alert-rule-on-a-metric-from-the-azure-portal"></a>Creación de una regla de alerta sobre una métrica desde Azure Portal
1. En [Azure Portal](https://portal.azure.com/), seleccione el servidor de Azure Database for MySQL que quiera supervisar.

2. En la sección **Supervisión** de la barra lateral, seleccione **Alertas**, tal y como se muestra a continuación:

   :::image type="content" source="./media/howto-alert-on-metric/2-alert-rules.png" alt-text="Selección de Reglas de alerta":::

3. Seleccione **Agregar alerta de métrica** (icono +).

4. Se abre la página **Crear regla**, tal y como se muestra a continuación. Rellene la información necesaria:

   :::image type="content" source="./media/howto-alert-on-metric/4-add-rule-form.png" alt-text="Selección de Reglas de alerta":::

5. En la sección **Condición**, seleccione **Agregar condición**.

6. Seleccione una métrica de la lista de señales sobre las que desea recibir alertas. En este ejemplo, seleccione "Porcentaje de almacenamiento".
   
   :::image type="content" source="./media/howto-alert-on-metric/6-configure-signal-logic.png" alt-text="Selección de Reglas de alerta":::

7. Configure la lógica de alerta incluida la **condición** (p. ej., "Mayor que") el **umbral** (p. ej., 85 %), la **agregación de tiempo**, el **período** de tiempo de la regla de métrica que debe transcurrir para que se desencadene la alerta (p. ej., "En los últimos 30 minutos") y la **frecuencia**.
   
   Seleccione **Listo** cuando haya terminado.

   :::image type="content" source="./media/howto-alert-on-metric/7-set-threshold-time.png" alt-text="Selección de Reglas de alerta" para seleccionar los propietarios, colaboradores y lectores de la suscripción que recibirán las notificaciones.
   
    También puede indicar un identificador URI válido en el campo **Webhook** si quiere llamarlo cuando se active la alerta.

    Cuando haya terminado, seleccione **Aceptar**.

    :::image type="content" source="./media/howto-alert-on-metric/10-action-group-type.png" alt-text="Selección de Reglas de alerta":::

11. Especifique el nombre de la regla de alertas, la descripción y la gravedad.

    :::image type="content" source="./media/howto-alert-on-metric/11-name-description-severity.png" alt-text="Selección de Reglas de alerta"::: 

12. Seleccione **Crear regla de alerta** para crear la alerta.

    En cuestión de minutos, se activa la alerta y se desencadena tal como se describió anteriormente.

## <a name="manage-your-alerts"></a>Administración de alertas
Una vez que haya creado una alerta, puede seleccionarla y realizar las acciones siguientes:

* Ver un gráfico que muestre el umbral de las métricas y los valores reales del día anterior en relación con esta alerta.
* **Editar** o **eliminar** la regla de alerta.
* **Deshabilitar** o **habilitar** la alerta, si quiere detener temporalmente o reanudar la recepción de notificaciones.


## <a name="next-steps"></a>Pasos siguientes
* Obtenga más información sobre cómo [configurar webhooks en las alertas](../azure-monitor/platform/alerts-webhooks.md).
* Obtenga [información general sobre la colección de métricas](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) para garantizar que el servicio está disponible y que responder adecuadamente.
