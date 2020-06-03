---
title: Solicitud de aumento de cuota y obtención de soporte técnico
description: Creación de una solicitud de soporte técnico en Azure Portal para Azure Synapse Analytics Solicite aumentos de cuota o soporte técnico para solucionar un problema.
services: synapse-analytics
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 03/10/2020
author: kevinvngo
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: e0788f978fd25356b230a7923def6cbbea3dc305
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83835468"
---
# <a name="request-quota-increases-and-get-support-for-azure-synapse-analytics"></a>Solicitud de aumentos de cuota y obtención de soporte técnico para Azure Synapse Analytics

En este artículo se describe cómo enviar una incidencia de soporte técnico en Azure Portal para Azure Synapse Analytics. Este proceso le permite solicitar un aumento de la cuota o enviar una solicitud de soporte técnico al equipo de soporte técnico de ingeniería.

## <a name="create-a-support-ticket"></a>Creación de una incidencia de soporte técnico

Use los pasos siguientes para crear una nueva solicitud de soporte técnico desde Azure Portal para Azure Synapse Analytics.

1. En el menú de [Azure Portal](https://portal.azure.com), seleccione **Ayuda y soporte técnico**.

   ![El vínculo de Ayuda y soporte técnico](./media/sql-data-warehouse-get-started-create-support-ticket/help-plus-support.png)


1. Seleccione **Ayuda y soporte técnico** y **Nueva solicitud de soporte técnico**.

    ![Creación de una solicitud de soporte técnico](./media/sql-data-warehouse-get-started-create-support-ticket/new-support-request.png)

1. Revise el [Plan de soporte técnico de Azure](https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/).

   * El soporte técnico para **facturación, cuota y administración de suscripciones** está disponible en todos los niveles.
   * El soporte técnico para problemas del tipo **break-fix** se proporciona en los niveles [Desarrollador](https://azure.microsoft.com/support/plans/developer/), [Estándar](https://azure.microsoft.com/support/plans/standard/), [Professional Direct](https://azure.microsoft.com/support/plans/prodirect/) o [Premier](https://azure.microsoft.com/support/plans/premier/). Los problemas "break-fix" son aquellos que experimentan los clientes mientras usan Azure en los que hay una posibilidad razonable de que hayan sido causados por Microsoft.
   * El **aprendizaje para desarrolladores** y los **servicios de asesoramiento** están disponibles en los niveles de soporte técnico [Professional Direct](https://azure.microsoft.com/support/plans/prodirect/) y [Premier](https://azure.microsoft.com/support/plans/premier/).

   Si tiene un plan de soporte técnico Premier, también puede informar sobre problemas relacionados con Azure Synapse Analytics en el [portal Microsoft Premier Online](https://premier.microsoft.com/). Consulte [Planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/) para obtener más información sobre los diversos planes de soporte técnico, incluido su ámbito, los tiempos de respuesta, el precio, etc.  Si quiere ver las preguntas más frecuentes sobre el soporte técnico de Azure, consulte [Preguntas más frecuentes de soporte técnico de Azure](https://azure.microsoft.com/support/faq/).

1. En **Tipo de problema**, seleccione el tipo de problema apropiado. En caso de problemas de break-fix, seleccione **Técnico**. Para las solicitudes de aumento de cuota, seleccione **Límites de servicio y suscripción (cuotas)** .

   ![Selección de un tipo de problema](./media/sql-data-warehouse-get-started-create-support-ticket/select-quota-issue-type.png)  

   > [!NOTE]
   > El resto de este artículo se centra en las solicitudes de aumento de cuota. No obstante, también puede seleccionar **Técnico** aquí para solicitudes de soporte técnico para la resolución de problemas. Si selecciona **Técnico**, se le pedirá que proporcione un resumen y, a continuación, identifique un tipo de problema en **Seleccionar el tipo de problema**. Es posible que vea soluciones que le ayuden a resolver el problema. Si las soluciones presentadas no resuelven el problema, seleccione **Siguiente: Detalles** y rellene el formulario para enviar la incidencia de soporte técnico.

1. Para las solicitudes de aumento de cuota, seleccione **Azure Synapse Analytics** para el **Tipo de cuota**. Después, seleccione **Next: Soluciones >>** .

   ![Selección de un tipo de cuota](./media/sql-data-warehouse-get-started-create-support-ticket/select-quota-type.png)

1. En la ventana **Detalles**, seleccione **Proporcionar detalles** para especificar información adicional.

   ![El vínculo "Proporcionar detalles"](./media/sql-data-warehouse-get-started-create-support-ticket/provide-details-link.png)

## <a name="quota-request-types"></a>Tipos de solicitud de cuota

Cuando hace clic en **Proporcionar detalles** aparece la ventana **Detalles de la cuota** que le permite agregar información adicional. En las secciones siguientes se describen las diferentes solicitudes de cuota disponibles para Azure Synapse Analytics.

### <a name="data-warehouse-units-dwus-per-server"></a>Unidades de almacenamiento de datos (DWU) por servidor

Siga estos pasos para solicitar un aumento de las DWU por servidor.

1. Seleccione el tipo de cuota **Unidades de almacenamiento de datos (DWU) por servidor**.

1. En la lista **Recurso**, seleccione el recurso de destino.

1. En el campo **Solicitar cuota**, escriba el nuevo límite de DWU que solicita.

   ![Detalles de la cuota de DWU](./media/sql-data-warehouse-get-started-create-support-ticket/quota-details-dwus.png)

### <a name="servers-per-subscription"></a>Servidores por suscripción

Siga estos pasos para solicitar un aumento en el número de servidores por suscripción.

1. Seleccione el tipo de cuota **Servidores por suscripción**.

1. En la lista **Ubicación**, seleccione la región de Azure que se va a usar. La cuota es por suscripción en cada región.

1. En el campo **Nueva cuota**, escriba la solicitud del número máximo de servidores de esa región.

   ![Detalles de la cuota de servidores](./media/sql-data-warehouse-get-started-create-support-ticket/quota-details-servers.png)

### <a name="enable-subscription-access-to-a-region"></a>Habilitar el acceso de suscripciones a una región

Algunos tipos de oferta no están disponibles en todas las regiones. Puede ver un error como el siguiente:

`This location is not available for subscription`

Si su suscripción necesita acceso en una región determinada, use la opción **Otra solicitud de cuota** para solicitar acceso. En la solicitud, especifique los detalles de la oferta y la SKU que desea habilitar para la región. Para explorar las opciones de ofertas y SKU, consulte [Precios de Azure Synapse Analytics](https://azure.microsoft.com/pricing/details/synapse-analytics/).

![Otros detalles de cuota](./media/sql-data-warehouse-get-started-create-support-ticket/quota-details-whitelisting.png)

## <a name="submit-your-request"></a>Enviar la solicitud

El paso final consiste en rellenar los detalles restantes de la solicitud de soporte técnico de SQL Database. Después, seleccione **Next: Revisar y crear>>** y, después de revisar los detalles de la solicitud, haga clic en **Crear** para enviar la solicitud.

## <a name="monitor-a-support-ticket"></a>Supervisión de una incidencia de soporte técnico

Una vez que ha enviado la solicitud de asistencia, el equipo de soporte técnico de Azure se pondrá en contacto con usted. Para comprobar tanto el estado de la solicitud como los detalles de la misma, haga clic en **Todas las solicitudes de soporte técnico** en el panel.

![Comprobar estado](./media/sql-data-warehouse-get-started-create-support-ticket/monitor-ticket.png)

## <a name="other-resources"></a>Otros recursos

También puede conectar con la comunidad de Azure Synapse Analytics en [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-synapse+or+azure-sql-data-warehouse) o mediante la [página de preguntas y respuestas de Microsoft para Azure SQL Data Warehouse](https://docs.microsoft.com/answers/topics/azure-synapse-analytics.html).

