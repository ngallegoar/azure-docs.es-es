---
title: Supervisión de mensajes B2B con Azure Monitor
description: Solución de problemas con mensajes AS2, X12 y EDIFACT mediante la configuración de registros de Azure Monitor y la recopilación de datos de diagnóstico para Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: divswa, logicappspm
ms.topic: article
ms.date: 01/30/2020
ms.openlocfilehash: 5baa4d4d968adb25b5520ca91149970f5c5578e9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86536280"
---
# <a name="set-up-azure-monitor-logs-and-collect-diagnostics-data-for-b2b-messages-in-azure-logic-apps"></a>Configuración de registros de Azure Monitor y recopilación de datos de diagnóstico para mensajes B2B en Azure Logic Apps

Después de configurar la comunicación B2B entre entidades en la cuenta de integración, los asociados pueden intercambiar mensajes mediante protocolos como AS2, X12 y EDIFACT. Para comprobar que esta comunicación funciona según lo previsto, puede configurar los [registros de Azure Monitor](../azure-monitor/platform/data-platform-logs.md) para la cuenta de integración. [Azure Monitor](../azure-monitor/overview.md) le ayuda a supervisar los entornos locales y en la nube para que pueda mantener más fácilmente su disponibilidad y rendimiento. Con registros de Azure Monitor puede registrar y almacenar datos sobre eventos y tiempo de ejecución, como los de desencadenamiento, ejecución y acción en un [área de trabajo de Log Analytics](../azure-monitor/platform/resource-logs.md#send-to-log-analytics-workspace). Para los mensajes, el registro también recopila la información siguiente:

* Número y estado de los mensajes
* Del estado de las confirmaciones
* Correlaciones entre mensajes y confirmaciones
* Descripción detallada de errores

Azure Monitor permite crear [consultas de registro](../azure-monitor/log-query/log-query-overview.md) que le ayudarán a encontrar y revisar esta información. También puede [usar estos datos de diagnóstico con otros servicios de Azure](../logic-apps/monitor-logic-apps-log-analytics.md#extend-data), como Azure Storage y Azure Event Hubs.

Para configurar el registro en la cuenta de integración, [instale la solución Logic Apps B2B](#install-b2b-solution) en Azure Portal. Esta solución proporciona información agregada para los eventos de los mensajes B2B. Luego, para habilitar el registro y la creación de consultas para esta información, configure los [registros de Azure Monitor](#set-up-resource-logs).

En este artículo se muestra cómo configurar el registro de Azure Monitor para la cuenta de integración.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Prerrequisitos

* Un área de trabajo de Log Analytics. Si no tiene un área de trabajo de Log Analytics, aprenda a [crear un área de trabajo de Log Analytics](../azure-monitor/learn/quick-create-workspace.md).

* Una aplicación lógica que se configura con el registro de Azure Monitor y que envía esa información a un área de trabajo de Log Analytics. Aprenda a [configurar registros de Azure Monitor para la aplicación lógica](../logic-apps/monitor-logic-apps.md).

* Una cuenta de integración vinculada a la aplicación lógica. Aprenda a [vincular la cuenta de integración a la aplicación lógica](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).

<a name="install-b2b-solution"></a>

## <a name="install-logic-apps-b2b-solution"></a>Instalación de una solución Logic Apps B2B

Para que los registros de Azure Monitor puedan realizar el seguimiento de los mensajes B2B de la aplicación lógica, agregue la solución **Logic Apps B2B** al área de trabajo de Log Analytics.

1. En el cuadro de búsqueda de [Azure Portal](https://portal.azure.com), escriba `log analytics workspaces` y seleccione **Áreas de trabajo de Log Analytics**.

   ![Selección de "Áreas de trabajo de Log Analytics"](./media/monitor-b2b-messages-log-analytics/find-select-log-analytics-workspaces.png)

1. En **Áreas de trabajo de Log Analytics**, seleccione su área de trabajo.

   ![Selección del área de trabajo de Log Analytics](./media/monitor-b2b-messages-log-analytics/select-log-analytics-workspace.png)

1. En el panel Información general, en **Introducción a Log Analytics** > **Configurar soluciones de supervisión**, elija **Ver soluciones**.

   ![Selección de "Ver soluciones" en el panel de información general](./media/monitor-b2b-messages-log-analytics/log-analytics-workspace.png)

1. En el panel de información general, seleccione **Agregar**.

   ![Incorporación de una solución nueva en el panel de información general](./media/monitor-b2b-messages-log-analytics/add-logic-apps-management-solution.png)

1. Una vez abierto **Marketplace**, en el cuadro de búsqueda, escriba `logic apps b2b` y seleccione **Logic Apps B2B**.

   ![Selección de "Logic Apps Management" en Marketplace](./media/monitor-b2b-messages-log-analytics/select-logic-apps-b2b-solution.png)

1. En el panel de descripción de la solución, seleccione **Crear**.

   ![Selección de "Crear" para agregar la solución "Logic Apps B2B"](./media/monitor-b2b-messages-log-analytics/create-logic-apps-b2b-solution.png)

1. Revise y confirme el área de trabajo de Log Analytics donde quiere instalar la solución y vuelva a seleccionar **Crear**.

   ![Selección de "Crear" para "Logic Apps B2B"](./media/monitor-b2b-messages-log-analytics/confirm-log-analytics-workspace.png)

   Una vez que Azure implemente la solución en el grupo de recursos de Azure que contiene el área de trabajo de Log Analytics, la solución aparecerá en el panel de resumen del área de trabajo. Una vez procesados los mensajes B2B, aparece en este panel el número de mensajes actualizado.

   ![Panel de resumen del área de trabajo](./media/monitor-b2b-messages-log-analytics/b2b-overview-messages-summary.png)

<a name="set-up-resource-logs"></a>

## <a name="set-up-azure-monitor-logs"></a>Configuración de los registros de Azure Monitor

El registro de Azure Monitor se puede habilitar directamente desde la cuenta de integración.

1. En [Azure Portal](https://portal.azure.com), busque y seleccione la cuenta de integración.

   ![Busque y seleccione su cuenta de integración](./media/monitor-b2b-messages-log-analytics/find-integration-account.png)

1. En el menú de la cuenta de integración, seleccione **Configuración de diagnóstico** en **Supervisión**. Seleccione **Agregar configuración de diagnóstico**.

   ![Selección de "Configuración de diagnóstico" en "Supervisión"](./media/monitor-b2b-messages-log-analytics/monitor-diagnostics-settings.png)

1. Para crear la configuración, siga estos pasos:

   1. Proporcione un nombre para la configuración.

   1. Seleccione **Enviar a Log Analytics**.

   1. En **Suscripción**, seleccione la suscripción de Azure que está asociada al área de trabajo de Log Analytics.

   1. En **Área de trabajo de Log Analytics**, seleccione el área de trabajo que quiere usar.

   1. En **registro**, seleccione la categoría **IntegrationAccountTrackingEvents**, que especifica la categoría de eventos que quiere registrar.

   1. Cuando finalice, seleccione **Guardar**.

   Por ejemplo: 

   ![Configuración de registros de Azure Monitor para recopilar datos de diagnóstico](./media/monitor-b2b-messages-log-analytics/send-diagnostics-data-log-analytics-workspace.png)

<a name="view-message-status"></a>

## <a name="view-message-status"></a>Visualización del estado de los mensajes

Una vez ejecutada la aplicación lógica, puede ver los datos y el estado de esos mensajes en el área de trabajo de Log Analytics.

1. En el cuadro de búsqueda de [Azure Portal](https://portal.azure.com), busque y abra el área de trabajo de Log Analytics.

1. En el menú del área de trabajo, seleccione **Resumen del área de trabajo** > **Logic Apps B2B**.

   ![Panel de resumen del área de trabajo](./media/monitor-b2b-messages-log-analytics/b2b-overview-messages-summary.png)

   > [!NOTE]
   > Si el icono de Logic Apps B2B no muestra los resultados inmediatamente después de una ejecución, pruebe a seleccionar **Actualizar** o espere un poco antes de volver a intentarlo.

   De forma predeterminada, el icono de **Logic Apps B2B** muestra datos basados en un solo día. Para cambiar el ámbito de datos a otro intervalo, seleccione el control de ámbito de la parte superior de la página:

   ![Cambiar intervalo](./media/monitor-b2b-messages-log-analytics/change-summary-interval.png)

1. Después de que aparezca el panel de estado de los mensajes, puede ver más detalles sobre un tipo de mensaje concreto, lo que muestra los datos basados en un solo día. Seleccione el icono de **AS2**, **X12** o **EDIFACT**.

   ![Visualización de los estados de los mensajes](./media/monitor-b2b-messages-log-analytics/workspace-summary-b2b-messages.png)

   Aparece una lista de mensajes para el icono seleccionado. Por ejemplo, este podría ser el aspecto de una lista de mensajes AS2:

   ![Estados y detalles de los mensajes AS2](./media/monitor-b2b-messages-log-analytics/as2-message-results-list.png)

   Para más información sobre las propiedades de cada tipo de mensaje, vea estas descripciones de las propiedades de los mensajes:

   * [Propiedades de mensajes AS2](#as2-message-properties)
   * [Propiedades de mensajes X12](#x12-message-properties)
   * [Propiedades de mensajes EDIFACT](#EDIFACT-message-properties)

<!--
1. To view or export the inputs and outputs for specific messages, select those messages, and select **Download**. When you're prompted, save the .zip file to your local computer, and then extract that file.

   The extracted folder includes a folder for each selected message. If you set up acknowledgements, the message folder also includes files with acknowledgement details. Each message folder has at least these files:
   
   * Human-readable files with the input payload and output payload details
   * Encoded files with the inputs and outputs

   For each message type, you can find the folder and file name formats here:

   * [AS2 folder and file name formats](#as2-folder-file-names)
   * [X12 folder and file name formats](#x12-folder-file-names)
   * [EDIFACT folder and file name formats](#edifact-folder-file-names)

   ![Download message files](./media/monitor-b2b-messages-log-analytics/download-messages.png)

1. To view all actions that have the same run ID, on the **Log Search** page, select a message from the message list.

   You can sort these actions by column, or search for specific results.

   * To search results with prebuilt queries, select **Favorites**.

   * Learn [how to build queries by adding filters](../logic-apps/create-monitoring-tracking-queries.md). Or learn more about [how to find data with log searches in Azure Monitor logs](../azure-monitor/log-query/log-query-overview.md).

   * To change query in the search box, update the query with the columns and values that you want to use as filters.
-->

<a name="message-list-property-descriptions"></a>

## <a name="property-descriptions-and-name-formats-for-as2-x12-and-edifact-messages"></a>Descripciones de propiedades y formatos de nombre de los mensajes AS2, X12 y EDIFACT

Estas son las descripciones de propiedades y los formatos de nombre de los archivos de mensajes descargados por tipo de mensaje.

<a name="as2-message-properties"></a>

### <a name="as2-message-property-descriptions"></a>Descripciones de propiedades de los mensajes AS2

Estas son las descripciones de propiedades de cada mensaje AS2.

| Propiedad | Descripción |
|----------|-------------|
| **Sender** | Asociado invitado especificado en **Configuración de recepción** o asociado del host especificado en **Configuración de envío** para un acuerdo de AS2 |
| **Receiver** | Asociado del host especificado en **Configuración de recepción** o asociado invitado especificado en **Configuración de envío** para un acuerdo de AS2 |
| **Aplicación lógica** | Aplicación lógica donde se configuran las acciones AS2 |
| **Estado** | Estado del mensaje AS2 <br>Correcto = recibido o enviado un mensaje AS2 válido. No se configura ninguna MDN. <br>Correcto = recibido o enviado un mensaje AS2 válido. Se configura y se recibe una MDN, o se envía. <br>Error = recibido un mensaje AS2 no válido. No se configura ninguna MDN. <br>Pendiente = recibido o enviado un mensaje AS2 válido. Se configura una MDN y se espera una MDN. |
| **ACK** | Estado del mensaje MDN <br>Aceptado = recibida o enviada una MDN positiva. <br>Pendiente = esperando a recibir o enviar una MDN. <br>Rechazado = recibida o enviada una MDN negativa. <br>No necesario = no configurada ninguna MDN en el acuerdo. |
| **Dirección** | Dirección del mensaje AS2 |
| **Tracking ID** | Identificador que correlaciona todos los desencadenadores y las acciones de una aplicación lógica |
| **Message ID** | Identificador del mensaje AS2 a partir de los encabezados de mensajes AS2 |
| **Timestamp** | Hora a la que la acción AS2 procesó el mensaje |
|||

<!--
<a name="as2-folder-file-names"></a>

### AS2 name formats for downloaded message files

Here are the name formats for each downloaded AS2 message folder and files.

| Folder or file | Name format |
|----------------|-------------|
| Message folder | [sender]\_[receiver]\_AS2\_[correlation-ID]\_[message-ID]\_[timestamp] |
| Input, output, and if set up, acknowledgement files | **Input payload**: [sender]\_[receiver]\_AS2\_[correlation-ID]\_input_payload.txt </p>**Output payload**: [sender]\_[receiver]\_AS2\_[correlation-ID]\_output\_payload.txt </p></p>**Inputs**: [sender]\_[receiver]\_AS2\_[correlation-ID]\_inputs.txt </p></p>**Outputs**: [sender]\_[receiver]\_AS2\_[correlation-ID]\_outputs.txt |
|||
-->

<a name="x12-message-properties"></a>

### <a name="x12-message-property-descriptions"></a>Descripciones de propiedades de los mensajes X12

Estas son las descripciones de propiedades de cada mensaje X12.

| Propiedad | Descripción |
|----------|-------------|
| **Sender** | Asociado invitado especificado en **Configuración de recepción** o asociado del host especificado en **Configuración de envío** para un acuerdo de X12 |
| **Receiver** | Asociado del host especificado en **Configuración de recepción** o asociado invitado especificado en **Configuración de envío** para un acuerdo de X12 |
| **Aplicación lógica** | Aplicación lógica donde se configuran las acciones X12 |
| **Estado** | Estado del mensaje X12 <br>Correcto = recibido o enviado un mensaje X12 válido. No se configura ninguna confirmación funcional. <br>Correcto = recibido o enviado un mensaje X12 válido. Se configura y se recibe una confirmación funcional, o se envía una confirmación funcional. <br>Error = recibido o enviado un mensaje X12 no válido. <br>Pendiente = recibido o enviado un mensaje X12 válido. Se configura una confirmación funcional y se espera una confirmación funcional. |
| **ACK** | Estado de confirmación funcional (997) <br>Aceptado = recibida o enviada una confirmación funcional positiva. <br>Rechazado = recibida o enviada una confirmación funcional negativa. <br>Pendiente = se espera una confirmación funcional, pero no se ha recibido. <br>Pendiente = generada una confirmación funcional, pero no se puede enviar al asociado. <br>No necesario = confirmación funcional no configurada. |
| **Dirección** | Dirección del mensaje X12 |
| **Tracking ID** | Identificador que correlaciona todos los desencadenadores y las acciones de una aplicación lógica |
| **Msg Type** | Tipo de mensaje X12 de EDI |
| **ICN** | Número de control de intercambio del mensaje X12 |
| **TSCN** | Número de control de conjuntos de transacciones del mensaje X12 |
| **Timestamp** | Hora a la que la acción X12 procesó el mensaje |
|||

<!--
<a name="x12-folder-file-names"></a>

### X12 name formats for downloaded message files

Here are the name formats for each downloaded X12 message folder and files.

| Folder or file | Name format |
|----------------|-------------|
| Message folder | [sender]\_[receiver]\_X12\_[interchange-control-number]\_[global-control-number]\_[transaction-set-control-number]\_[timestamp] |
| Input, output, and if set up, acknowledgement files | **Input payload**: [sender]\_[receiver]\_X12\_[interchange-control-number]\_input_payload.txt </p>**Output payload**: [sender]\_[receiver]\_X12\_[interchange-control-number]\_output\_payload.txt </p></p>**Inputs**: [sender]\_[receiver]\_X12\_[interchange-control-number]\_inputs.txt </p></p>**Outputs**: [sender]\_[receiver]\_X12\_[interchange-control-number]\_outputs.txt |
|||
-->

<a name="EDIFACT-message-properties"></a>

### <a name="edifact-message-property-descriptions"></a>Descripciones de propiedades de los mensajes EDIFACT

Estas son las descripciones de propiedades de cada mensaje EDIFACT.

| Propiedad | Descripción |
|----------|-------------|
| **Sender** | Asociado invitado especificado en **Configuración de recepción** o asociado del host especificado en **Configuración de envío** para un acuerdo de EDIFACT |
| **Receiver** | Asociado del host especificado en **Configuración de recepción** o asociado invitado especificado en **Configuración de envío** para un acuerdo de EDIFACT |
| **Aplicación lógica** | Aplicación lógica donde se configuran las acciones EDIFACT |
| **Estado** | Estado del mensaje EDIFACT <br>Correcto = recibido o enviado un mensaje EDIFACT válido. No se configura ninguna confirmación funcional. <br>Correcto = recibido o enviado un mensaje EDIFACT válido. Se configura y se recibe una confirmación funcional, o se envía una confirmación funcional. <br>Error = recibido o enviado un mensaje EDIFACT no válido <br>Pendiente = recibido o enviado un mensaje EDIFACT válido. Se configura una confirmación funcional y se espera una confirmación funcional. |
| **ACK** | Estado de confirmación funcional (CONTRL) <br>Aceptado = recibida o enviada una confirmación funcional positiva. <br>Rechazado = recibida o enviada una confirmación funcional negativa. <br>Pendiente = se espera una confirmación funcional, pero no se ha recibido. <br>Pendiente = generada una confirmación funcional, pero no se puede enviar al asociado. <br>No necesario = confirmación funcional no configurada. |
| **Dirección** | Dirección del mensaje EDIFACT |
| **Tracking ID** | Identificador que correlaciona todos los desencadenadores y las acciones de una aplicación lógica |
| **Msg Type** | Tipo del mensaje EDIFACT |
| **ICN** | Número de control de intercambio del mensaje EDIFACT |
| **TSCN** | Número de control de conjuntos de transacciones del mensaje EDIFACT |
| **Timestamp** | Hora a la que la acción EDIFACT procesó el mensaje |
|||

<!--
<a name="edifact-folder-file-names"></a>

### EDIFACT name formats for downloaded message files

Here are the name formats for each downloaded EDIFACT message folder and files.

| Folder or file | Name format |
|----------------|-------------|
| Message folder | [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_[global-control-number]\_[transaction-set-control-number]\_[timestamp] |
| Input, output, and if set up, acknowledgement files | **Input payload**: [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_input_payload.txt </p>**Output payload**: [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_output\_payload.txt </p></p>**Inputs**: [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_inputs.txt </p></p>**Outputs**: [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_outputs.txt |
|||
-->

## <a name="next-steps"></a>Pasos siguientes

* [Creación de consultas de supervisión y seguimiento](../logic-apps/create-monitoring-tracking-queries.md)
