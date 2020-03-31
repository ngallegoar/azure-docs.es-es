---
title: Supervisar runbooks de Azure Automation con alertas de métricas
description: En este artículo se indica cómo supervisar runbooks de Azure Automation en función de las métricas
services: automation
ms.date: 11/01/2018
ms.topic: article
ms.openlocfilehash: 9bd028157b33817898ef69f9e47cb8b5d9b8f381
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75367100"
---
# <a name="monitoring-runbooks-with-metric-alerts"></a>Supervisión de runbooks con alertas de métricas

En este artículo, aprenderá a crear alertas basadas en el estado de finalización de runbooks.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure en https://portal.azure.com

## <a name="create-alert"></a>Crear una alerta

Las alertas le permiten definir una condición para supervisar y una acción para realizar cuando se cumple esa condición.

En Azure Portal, vaya a la cuenta de Automation. En **Supervisión**, seleccione **Alertas** y haga clic en **+ Nueva regla de alertas**. El ámbito del destino ya está definido para la cuenta de Automation.

### <a name="configure-alert-criteria"></a>Configuración de los criterios de alerta

1. Haga clic en **+ Agregar criterios**. Seleccione **Métricas** para el **Tipo de señal** y elija **Total de trabajos** de la tabla.

2. La página **Configurar lógica de señal** es donde se define la lógica que desencadena la alerta. En el gráfico del historial se presentan dos dimensiones, **Nombre de Runbook** y **Estado**. Las dimensiones son propiedades diferentes de una métrica que puede usarse para filtrar los resultados. En **Nombre de Runbook**, seleccione el runbook sobre el que quiera enviar la alerta o déjelo en blanco enviar alertas de todos los runbooks. En **Estado**, seleccione un estado de la lista desplegable que quiera supervisar. Los valores de nombre y estado del runbook que aparecen en la lista desplegable son solo para los trabajos que se ejecutaron en la última semana.

   Si quiere enviar una alerta sobre un estado o un runbook que no se muestran en la lista desplegable, haga clic en **\+** junto a la dimensión. Esta acción abre un cuadro de diálogo que le permite escribir un valor personalizado que no se haya emitido recientemente para esa dimensión. Si escribe un valor que no existe para una propiedad, no se activará la alerta.

   > [!NOTE]
   > Si no aplica un nombre para la dimensión **RunbookName** y existen runbooks que cumplen los criterios de estado, incluidos runbooks ocultos del sistema, recibirá una alerta.

3. En **Lógica de alerta**, defina la condición y el umbral de la alerta. Debajo se muestra una vista previa de la condición definida.

4. En **Se evaluó basándose en**, seleccione el intervalo de tiempo para la consulta y la frecuencia con la que desea que se ejecute esa consulta. Por ejemplo, si elige **En los últimos 5 minutos** en **Período** y **Cada minuto** en **Frecuencia**, la alerta busca el número de runbooks que cumplen los criterios en los últimos 5 minutos. Esta consulta se ejecuta cada minuto y, una vez que los criterios de alerta que ha definido ya no se encuentran en un periodo de 5 minutos, la alerta se resuelve. Cuando termine, haga clic en **Listo**.

   ![Seleccionar un recurso para la alerta](./media/automation-alert-activity-log/configure-signal-logic.png)

### <a name="define-alert-details"></a>Definición de detalles de la alerta

1. En **2. Definir detalles de la alerta**, proporcione a la alerta un nombre descriptivo y una descripción. Establezca la **Gravedad** para que coincida con la condición de la alerta. Hay cinco niveles de gravedad que van de 0 a 5. Las alertas se tratan de igual manera, independientemente de la gravedad; puede hacer coincidir la gravedad con la lógica de negocios.

1. En la parte inferior de la sección, hay un botón que permite habilitar la regla cuando se completa. De forma predeterminada, las reglas están habilitadas en el momento de la creación. Si selecciona No, puede crear la alerta y se crea en un estado **Deshabilitado**. En la página **Reglas** de Azure Monitor, puede seleccionarla y hacer clic en **Habilitar** para habilitar la alerta cuando esté listo.

### <a name="define-the-action-to-take"></a>Definición de la acción que se realizará

1. En **3. Definir grupo de acciones**, haga clic en **+Nuevo grupo de acciones**. Un grupo de acciones es un conjunto de acciones que puede usar en varias alertas. Por ejemplo, notificaciones por correo electrónico, runbooks, webhooks y muchas más. Para más información sobre los grupos de acciones, consulte [Creación y administración de grupos de acciones](../azure-monitor/platform/action-groups.md).

1. En el cuadro **Nombre del grupo de acciones**, proporcione un nombre descriptivo y un nombre corto. El nombre corto se utiliza en lugar del nombre completo del grupo de acciones cuando se envían notificaciones mediante este grupo.

1. En la sección **Acciones**, de la opción **TIPO DE ACCIÓN**, seleccione **Email/SMS/Push/Voice** (Correo electrónico/SMS/Inserción/Voz).

1. En la página **Email/SMS/Push/Voice** (Correo electrónico/SMS/Inserción/Voz), proporciónele un nombre. Marque la casilla **Correo electrónico** y escriba una dirección de correo electrónico válida para usar.

   ![Configuración del grupo de acciones de correo electrónico](./media/automation-alert-activity-log/add-action-group.png)

1. Haga clic en **Aceptar** en la página **Email/SMS/Push/Voice** (Correo electrónico/SMS/Inserción/Voz)para cerrarla y haga clic en **Aceptar** para cerrar la página **Agregar grupo de acciones**. El nombre especificado en esta página se guarda como **NOMBRE DE ACCIÓN**.

1. Cuando haya terminado, haga clic en **Guardar**. Esta acción crea la regla que le avisa cuando un runbook se completa con un estado en particular.

> [!NOTE]
> Al agregar una dirección de correo electrónico a un grupo de acciones, se envía un correo electrónico de notificación que indica que la dirección se agregó a un grupo de acciones.

## <a name="notification"></a>Notification

Cuando se cumplen los criterios de la alerta, el grupo de acciones ejecuta la acción definida. En el ejemplo de este artículo, se envía un correo electrónico. La imagen siguiente es un ejemplo de un correo electrónico que recibirá después de que se desencadene la alerta:

![Alerta de correo electrónico](./media/automation-alert-activity-log/alert-email.png)

Una vez que la métrica ya no esté fuera del umbral definido, se desactivará la alerta y el grupo de acciones ejecutará la acción definida. Si se selecciona un tipo de acción de correo electrónico, se envía un correo electrónico de resolución que indica que esa acción se ha resuelto.

## <a name="next-steps"></a>Pasos siguientes

Continúe con el siguiente artículo para obtener información sobre otras formas mediante las cuales puede integrar las alertas en su cuenta de Automation.

> [!div class="nextstepaction"]
> [Usar una alerta para desencadenar un runbook de Azure Automation](automation-create-alert-triggered-runbook.md)
