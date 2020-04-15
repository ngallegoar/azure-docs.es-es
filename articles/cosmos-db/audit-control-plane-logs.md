---
title: Auditoría de operaciones de plano de control de Azure Cosmos DB
description: Aprenda a auditar las operaciones del plano de control, como agregar una región, actualizar el rendimiento, conmutar por error regiones o agregar una red virtual, entre otras, en Azure Cosmos DB
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: sngun
ms.openlocfilehash: 64ad8e6b1101d8486268c857b3a7752e1801f52c
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2020
ms.locfileid: "80420271"
---
# <a name="how-to-audit-azure-cosmos-db-control-plane-operations"></a>Auditoría de operaciones de plano de control de Azure Cosmos DB

Las operaciones del plano de control incluyen cambios en la cuenta o el contenedor de Azure Cosmos. Por ejemplo, crear una cuenta de Azure Cosmos, agregar una región, actualizar el rendimiento, conmutar por error regiones o agregar una red virtual son algunas de las operaciones del plano de control. En este artículo se explica cómo auditar las operaciones de plano de control en Azure Cosmos DB.

## <a name="disable-key-based-metadata-write-access"></a>Deshabilitar el acceso de escritura de metadatos basado en claves
 
Antes de auditar las operaciones de plano de control en Azure Cosmos DB, deshabilite el acceso de escritura de los metadatos basado en claves en su cuenta. Al deshabilitar el acceso de escritura de metadatos basado en claves, los clientes que se conecten a la cuenta de Azure Cosmos a través de claves de cuenta no podrán acceder a la cuenta. Para deshabilitar el acceso de escritura, defina la propiedad `disableKeyBasedMetadataWriteAccess` como true. Una vez definida esta propiedad, solo pueden aplicar cambios en los recursos los usuarios con las credenciales y el rol de control de acceso basado en rol (RBAC) adecuados. Para obtener más información sobre cómo establecer esta propiedad, consulte el artículo [Evitar cambios de SDK](role-based-access-control.md#preventing-changes-from-cosmos-sdk).

 Tenga en cuenta los siguientes puntos al desactivar el acceso de escritura de metadatos:

* Evalúe y asegúrese de que las aplicaciones no realizan llamadas de metadatos que cambian los recursos anteriores (por ejemplo, crear una colección, actualizar el rendimiento, etc.) mediante el SDK o las claves de cuenta.

* Actualmente, Azure Portal usa claves de cuenta para las operaciones de metadatos y, por lo tanto, estas operaciones se bloquearán. También puede usar las implementaciones de la CLI de Azure, SDK o plantillas de Resource Manager para realizar estas operaciones.

## <a name="enable-diagnostic-logs-for-control-plane-operations"></a>Habilitar registros de diagnóstico para las operaciones del plano de control

Puede habilitar los registros de diagnóstico para las operaciones del plano de control mediante Azure Portal. Siga estos pasos para habilitar el registro en las operaciones del plano de control:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y desplácese hasta la cuenta de Azure Cosmos.

1. Abra el panel de **configuración de diagnóstico** y proporcione un **Nombre** para crear los registros.

1. Seleccione **ControlPlaneRequests** para el tipo de registro y seleccione la opción **Enviar a Log Analytics**.

También puede almacenar los registros en una cuenta de almacenamiento o transmitir a un centro eventos. En este artículo se muestra cómo enviar registros a Log Analytics y, a continuación, consultarlos. Después de habilitarlos, los registros de diagnóstico tardan unos minutos en estar disponibles. Se puede realizar un seguimiento de todas las operaciones del plano de control realizadas después de ese punto. La captura de pantalla siguiente muestra cómo habilitar los registros del plano de control:

![Habilitar el registro de solicitudes del plano de control](./media/audit-control-plane-logs/enable-control-plane-requests-logs.png)

## <a name="view-the-control-plane-operations"></a>Ver las operaciones del plano de control

Después de activar el registro, siga estos pasos para realizar un seguimiento de las operaciones de una cuenta específica:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Abra la pestaña **Monitor** desde la navegación izquierda y seleccione el panel **Registros**. Se abre una interfaz de usuario donde se pueden ejecutar fácilmente consultas con esa cuenta específica en el ámbito. Ejecute la siguiente consulta para ver los registros del plano de control:

   ```kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="ControlPlaneRequests"
   | where TimeGenerated >= ago(1h)
   ```

Las capturas de pantalla siguientes capturan los registros cuando se agrega una red virtual a una cuenta de Azure Cosmos:

![Registros de plano de control cuando se agrega una red virtual](./media/audit-control-plane-logs/add-ip-filter-logs.png)

Las capturas de pantalla siguientes capturan los registros cuando se actualiza el rendimiento de una tabla Cassandra:

![Registros de plano de control cuando se actualiza el rendimiento](./media/audit-control-plane-logs/throughput-update-logs.png)

## <a name="identify-the-identity-associated-to-a-specific-operation"></a>Identificar la identidad asociada con una operación específica

Si quiere depurar más, puedes identificar una operación específica en el **Registro de actividad** mediante el identificador de actividad o la marca de tiempo de la operación. La marca de tiempo se usa para algunos clientes de Resource Manager cuando el identificador de actividad no se pasa explícitamente. El registro de actividad proporciona detalles sobre la identidad con la que se inició la operación. En la captura de pantalla siguiente se muestra cómo usar el identificador de la actividad y buscar las operaciones asociadas con él en el registro de actividad:

![Usar el identificador de actividad y buscar las operaciones](./media/audit-control-plane-logs/find-operations-with-activity-id.png)

## <a name="next-steps"></a>Pasos siguientes

* [Exploración de Azure Monitor para Azure Cosmos DB](../azure-monitor/insights/cosmosdb-insights-overview.md?toc=/azure/cosmos-db/toc.json&bc=/azure/cosmos-db/breadcrumb/toc.json)
* [Supervisión y depuración con métricas de Azure Cosmos DB](use-metrics.md)