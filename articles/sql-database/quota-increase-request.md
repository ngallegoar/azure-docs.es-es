---
title: Solicitar un aumento de cuota
description: En esta página se describe cómo crear una solicitud de soporte técnico para aumentar las cuotas de las bases de datos únicas, los servidores y las instancias administradas de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.topic: conceptual
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: sstein
ms.date: 02/04/2020
ms.openlocfilehash: ff2be6972bb4e8af266d0aa8a56d1879bc1b8b78
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77586162"
---
# <a name="request-quota-increases-for-azure-sql-database"></a>Solicitud de aumentos de cuota para Azure SQL Database

En este artículo se explica cómo solicitar un aumento de cuota para las bases de datos únicas, servidores e instancias administradas de Azure SQL Database. También se explica cómo habilitar el acceso de suscripciones a una región.

## <a name="create-a-new-support-request"></a><a id="newquota"></a> Creación de una solicitud de soporte técnico

Use los pasos siguientes para crear una nueva solicitud de soporte técnico desde Azure Portal para SQL Database.

1. En el menú de [Azure Portal](https://portal.azure.com), seleccione **Ayuda y soporte técnico**.

   ![El vínculo de Ayuda y soporte técnico](./media/quota-increase-request/help-plus-support.png)

1. Seleccione **Ayuda y soporte técnico** y **Nueva solicitud de soporte técnico**.

    ![Creación de una solicitud de soporte técnico](./media/quota-increase-request/new-support-request.png)

1. En **Tipo de problema**, seleccione **Límites de servicio y suscripción (cuotas)** .

   ![Selección de un tipo de problema](./media/quota-increase-request/select-quota-issue-type.png)

1. En **Suscripción**, seleccione la suscripción cuya cuota quiere aumentar.

   ![Selección de una suscripción para el aumento de cuota](./media/quota-increase-request/select-subscription-support-request.png)

1. En **Tipo de cuota**, seleccione uno de los siguientes tipos de cuota:

   - **SQL Database** para las cuotas de bases de datos únicas y grupos elásticos.
   - **Instancia administrada de SQL Database** para las instancias administradas.

   Después, seleccione **Next: Soluciones >>** .

   ![Selección de un tipo de cuota](./media/quota-increase-request/select-quota-type.png)

1. En la ventana **Detalles**, seleccione **Proporcionar detalles** para especificar información adicional.

   ![El vínculo "Proporcionar detalles"](./media/quota-increase-request/provide-details-link.png)

Cuando hace clic en **Proporcionar detalles** aparece la ventana **Detalles de la cuota** que le permite agregar información adicional. En las secciones siguientes se describen las distintas opciones de tipos de cuota de **SQL Database** e **Instancia administrada de SQL Database**.

## <a name="sql-database-quota-types"></a><a id="sqldbquota"></a> Tipos de cuota de SQL Database

En las secciones siguientes se describen tres opciones de aumento de cuota para los tipos de cuota de **SQL Database**:

- Unidades de transacción de base de datos (DTU) por servidor
- Servidores por suscripción
- Habilitar el acceso de suscripciones a una región

### <a name="database-transaction-units-dtus-per-server"></a>Unidades de transacción de base de datos (DTU) por servidor

Siga estos pasos para solicitar un aumento de las DTU por servidor.

1. Seleccione el tipo de cuota **Unidades de transacción de base de datos (DTU) por servidor**.

1. En la lista **Recurso**, seleccione el recurso de destino.

1. En el campo **Nueva cuota**, escriba el nuevo límite de DTU que solicita.

   ![Detalles de la cuota de DTU](./media/quota-increase-request/quota-details-dtus.png)

Para más información, consulte [Límites de recursos para bases de datos únicas que usan el modelo de compra de DTU](sql-database-dtu-resource-limits-single-databases.md) y [Límites de recursos para grupos elásticos que utilizan el modelo de compra de DTU](sql-database-dtu-resource-limits-elastic-pools.md).

### <a name="servers-per-subscription"></a>Servidores por suscripción

Siga estos pasos para solicitar un aumento en el número de servidores por suscripción.

1. Seleccione el tipo de cuota **Servidores por suscripción**.

1. En la lista **Ubicación**, seleccione la región de Azure que se va a usar. La cuota es por suscripción en cada región.

1. En el campo **Nueva cuota**, escriba la solicitud del número máximo de servidores de esa región.

   ![Detalles de la cuota de servidores](./media/quota-increase-request/quota-details-servers.png)

Para más información, consulte [Límites y regulación de recursos de SQL Database](sql-database-resource-limits-database-server.md).

### <a name="enable-subscription-access-to-a-region"></a><a id="other"></a> Habilitar el acceso de suscripciones a una región

Algunos tipos de oferta no están disponibles en todas las regiones. Puede ver un error como el siguiente:

`This location is not available for subscription`

Si su suscripción necesita acceso en una región determinada, use la opción **Otra solicitud de cuota** para solicitar acceso. En la solicitud, especifique los detalles de la oferta y la SKU que desea habilitar para la región. Para explorar las opciones de ofertas y SKU, consulte [Precios de Azure SQL Database](https://azure.microsoft.com/pricing/details/sql-database/single/).

![Otros detalles de cuota](./media/quota-increase-request/quota-details-whitelisting.png)

## <a name="managed-instance-quota-type"></a><a id="sqlmiquota"></a> Tipo de cuota de instancia administrada

Para el tipo de cuota **Instancia administrada de SQL Server**, siga estos pasos:

1. En la lista **Región**, seleccione la región de Azure de destino.

1. Especifique los nuevos límites que quiera solicitar para **Subred** y **vCore**.

   ![Información adicional sobre la cuota para Instancia administrada](./media/quota-increase-request/quota-details-managed-instance.png)

Para más información, consulte [Introducción a los límites de recursos de instancia administrada de Azure SQL Database](sql-database-managed-instance-resource-limits.md).

## <a name="submit-your-request"></a>Enviar la solicitud

El paso final consiste en rellenar los detalles restantes de la solicitud de cuota de SQL Database. Después, seleccione **Next: Revisar y crear>>** y, después de revisar los detalles de la solicitud, haga clic en **Crear** para enviar la solicitud.

## <a name="next-steps"></a>Pasos siguientes

Una vez que se envía la solicitud, esta será revisada. Recibirá una respuesta en función de la información que haya proporcionado en el formulario.

Para más información sobre otros límites de Azure, consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../azure-resource-manager/management/azure-subscription-service-limits.md).
