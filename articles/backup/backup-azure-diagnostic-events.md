---
title: Uso de la configuración de diagnósticos para almacenes de Recovery Services
description: Un artículo que describe cómo usar los eventos de diagnóstico antiguos y nuevos para Azure Backup
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: d10bedf3818559971eff12624152d0e797f6c3cc
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80672779"
---
# <a name="using-diagnostics-settings-for-recovery-services-vaults"></a>Uso de la configuración de diagnósticos para almacenes de Recovery Services

Azure Backup envía eventos de diagnóstico que se pueden recopilar y usar con fines de análisis, alertas e informes. 

Puede configurar los valores de diagnóstico para un almacén de Recovery Services a través de Azure Portal. Para ello, vaya al almacén y haga clic en el elemento de menú **Configuración de diagnóstico**. Al hacer clic en **+ Agregar configuración de diagnóstico**, puede enviar uno o varios eventos de diagnóstico a una cuenta de almacenamiento, un centro de eventos o un área de trabajo de Log Analytics (LA).

![Hoja de Configuración de diagnóstico](./media/backup-azure-diagnostics-events/diagnostics-settings-blade.png)

## <a name="diagnostics-events-available-for-azure-backup-users"></a>Eventos de diagnóstico disponibles para usuarios de Azure Backup

Azure Backup proporciona los siguientes eventos de diagnóstico, cada uno de los cuales proporciona datos detallados sobre un conjunto específico de artefactos relacionados con la copia de seguridad:

* CoreAzureBackup
* AddonAzureBackupAlerts
* AddonAzureBackupProtectedInstance
* AddonAzureBackupJobs
* AddonAzureBackupPolicy
* AddonAzureBackupStorage

[Modelo de datos para eventos de diagnóstico de Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-reports-data-model)

Los datos de estos eventos se pueden enviar a una cuenta de almacenamiento, al área de trabajo o a un centro de eventos. Si va a enviar estos datos a un área de trabajo de Log Analytics, debe seleccionar el botón de alternancia **Específico del recurso** alternancia en la pantalla **Configuración de diagnóstico** (consulte más información en las secciones siguientes).

## <a name="using-diagnostics-settings-with-log-analytics-la"></a>Uso de la configuración de diagnóstico con Log Analytics (LA)

Para coordinarse con el mapa de ruta de Log Analytics de Azure, Azure Backup ahora permite enviar datos de diagnóstico de almacén a las tablas de Log Analytics dedicadas para Backup. Estas se conocen como [tablas específicas de recursos](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-collect-workspace#resource-specific).

Para enviar los datos de diagnóstico del almacén a Log Analytics:

1.    Navegue hasta el almacén y haga clic en **Configuración de diagnóstico**. Haga clic en **+ Agregar configuración de diagnóstico**.
2.    Asigne un nombre a la configuración de diagnóstico.
3.    Marque la casilla **Enviar a Log Analytics** y seleccione un área de trabajo de Log Analytics.
4.    Seleccione **Específico del recurso** en el botón de alternancia y active los seis eventos siguientes: **CoreAzureBackup**, **AddonAzureBackupAlerts**, **AddonAzureBackupProtectedInstance**, **AddonAzureBackupJobs**, **AddonAzureBackupPolicy** y **AddonAzureBackupStorage**.
5.    Haga clic en **Guardar**.

![Modo específico del recurso](./media/backup-azure-diagnostics-events/resource-specific-blade.png)

Una vez que los datos se transmiten al área de trabajo de Log Analytics, se crean tablas dedicadas para cada uno de estos eventos en el área de trabajo. Puede consultar cualquiera de estas tablas directamente y también realizar combinaciones o uniones entre estas tablas si es necesario.

> [!IMPORTANT]
> Los seis eventos anteriores (es decir, CoreAzureBackup, AddonAzureBackupAlerts, AddonAzureBackupProtectedInstance, AddonAzureBackupJobs, AddonAzureBackupPolicy y AddonAzureBackupStorage), **solo** se admiten en el modo específico del recurso en [Informes de Backup](https://docs.microsoft.com/azure/backup/configure-reports). **Tenga en cuenta que, si intenta enviar datos de estos seis eventos en el modo de Azure Diagnostics, no se podrá ver ningún dato en Informes de Backup.**

## <a name="legacy-event"></a>Evento heredado

Tradicionalmente, todos los datos de diagnóstico relacionados con la copia de seguridad de un almacén se incluyen en un solo evento llamado "AzureBackupReport". En esencia, los seis eventos descritos anteriormente son un desglose de todos los datos contenidos en AzureBackupReport. 

Actualmente, se sigue admitiendo el evento AzureBackupReport debido a la compatibilidad con versiones anteriores en los casos en los que los usuarios tienen consultas personalizadas existentes relacionadas con este evento. Por ejemplo, alertas de registro personalizadas, visualizaciones personalizadas, etc. Sin embargo, **se recomienda cambiar a los [nuevos eventos](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events#diagnostics-events-available-for-azure-backup-users) tan pronto como sea posible**, ya que facilita el trabajo con los datos en las consultas de registro, proporciona mejor capacidad de detección de esquemas y su estructura, y mejora el rendimiento en la latencia de ingesta y los tiempos de consulta. 

**El evento heredado en modo de Azure Diagnostics pasará a estar finalmente en desuso, de ahí que la elección de nuevos eventos pueda ayudarle a evitar migraciones complejas en una fecha posterior.** Nuestra [solución de informes](https://docs.microsoft.com/azure/backup/configure-reports) que aprovecha Log Analytics también dejará de respaldar los datos del evento heredado.

### <a name="steps-to-move-to-new-diagnostics-settings-to-log-analytics-workspace"></a>Pasos para pasar a la nueva configuración de diagnóstico (al área de trabajo de Log Analytics)

1. Identifique qué almacenes envían datos a las áreas de trabajo de Log Analytics mediante el evento heredado, y las suscripciones a las que pertenecen. Ejecute las siguientes áreas de trabajo para identificar estos almacenes y suscripciones:

    ````Kusto
    let RangeStart = startofday(ago(3d));
    let VaultUnderAzureDiagnostics = (){
        AzureDiagnostics
        | where TimeGenerated >= RangeStart | where Category == "AzureBackupReport" and OperationName == "Vault" and SchemaVersion_s == "V2"
        | summarize arg_max(TimeGenerated, *) by ResourceId    
        | project ResourceId, Category};
    let VaultUnderResourceSpecific = (){
        CoreAzureBackup
        | where TimeGenerated >= RangeStart | where OperationName == "Vault" 
        | summarize arg_max(TimeGenerated, *) by ResourceId
        | project ResourceId, Category};
        // Some Workspaces will not have AzureDiagnostics Table, hence you need to use isFuzzy
    let CombinedVaultTable = (){
        CombinedTable | union isfuzzy = true 
        (VaultUnderAzureDiagnostics() ),
        (VaultUnderResourceSpecific() )
        | distinct ResourceId, Category};
    CombinedVaultTable | where Category == "AzureBackupReport"
    | join kind = leftanti ( 
    CombinedVaultTable | where Category == "CoreAzureBackup"
    ) on ResourceId
    | parse ResourceId with * "SUBSCRIPTIONS/" SubscriptionId:string "/RESOURCEGROUPS" * "MICROSOFT.RECOVERYSERVICES/VAULTS/" VaultName:string
    | project ResourceId, SubscriptionId, VaultName
    ````

2. Use la [instancia integrada de Azure Policy](https://docs.microsoft.com/azure/backup/azure-policy-configure-diagnostics) de Azure Backup para agregar una nueva configuración de diagnóstico para todos los almacenes del ámbito especificado. Esta directiva agrega una nueva configuración de diagnóstico a esos almacenes que no tienen una opción de diagnóstico (o) que solo tienen una configuración de diagnóstico heredada. Esta directiva se puede asignar a una suscripción entera o a un grupo de recursos cada vez. Tenga en cuenta que necesitará acceso de "propietario" a cada suscripción para la que se asigna la directiva.

Puede elegir tener una configuración de diagnóstico independiente para AzureBackupReport y los seis eventos nuevos, hasta que haya migrado todas las consultas personalizadas para usar los datos de las nuevas tablas. La imagen siguiente muestra el ejemplo de un almacén que tiene dos configuraciones de diagnóstico. La primera configuración, denominada **Setting1**, envía datos del evento AzureBackupReport a un área de trabajo de Log Analytics en modo AzureDiagnostics. La segunda configuración, denominada **Setting2**, envía datos de los seis eventos de Azure Backup nuevos a un área de trabajo de Log Analytics en el modo específico del recurso.

![Dos configuraciones](./media/backup-azure-diagnostics-events/two-settings-example.png)

> [!IMPORTANT]
> El evento AzureBackupReport **solo** se admite en el modo de Azure Diagnostics. **Tenga en cuenta que si intenta enviar datos de este evento en el modo específico del recurso, no se transmitirán datos al área de trabajo de Log Analytics.**

> [!NOTE]
> El botón de alternancia entre los modos Azure Diagnostics y específico del recurso solo aparece si el usuario marca la casilla **Enviar a Log Analytics**. Para enviar datos a una cuenta de almacenamiento o un centro de eventos, un usuario puede simplemente seleccionar el destino necesario y comprobar los eventos deseados, sin realizar entradas adicionales. De nuevo, se recomienda no elegir el evento heredado AzureBackupReport de ahora en adelante.

## <a name="users-sending-azure-site-recovery-events-to-log-analytics-la"></a>Usuarios que envían eventos de Azure Site Recovery a Log Analytics (LA)

Los eventos de Azure Backup y Azure Site Recovery se envían desde el mismo almacén de Recovery Services. Como Azure Site Recovery actualmente no está incorporado en las tablas específicas del recurso, se dirigirá a los usuarios que quieran enviar eventos de Azure Site Recovery a Log Analytics para que usen el modo de Azure Diagnostics **únicamente** (consulte la imagen siguiente). **La elección del modo específico del recurso para los eventos de Azure Site Recovery impedirá que los datos necesarios se envíen al área de trabajo de Log Analytics**.

![Eventos de Site Recovery](./media/backup-azure-diagnostics-events/site-recovery-settings.png)

Para resumir los detalles anteriores:

* Si ya configuró los diagnósticos de Log Analytics con Azure Diagnostics y además escribió consultas personalizadas, mantenga **intacta** esa configuración hasta que migre las consultas para que usen los datos de los nuevos eventos.
* Si también quiere incorporarse a las tablas nuevas (tal como se recomienda), cree una **nueva** configuración de diagnóstico, elija **Específica del recurso** y seleccione los seis eventos nuevos como se especificó anteriormente.
* Si actualmente está enviando eventos de Azure Site Recovery a Log Analytics, **no** elija el modo específico del recurso para estos eventos. De lo contrario, los datos de estos eventos no se transmitirán al área de trabajo de Log Analytics. En su lugar, cree una **Configuración de diagnóstico adicional**, seleccione **Azure Diagnostics** y elija los eventos de Azure Site Recovery pertinentes.

La imagen siguiente muestra el ejemplo de un usuario que tiene tres configuraciones de diagnóstico para un almacén. La primera configuración, denominada **Setting1**, envía datos del evento AzureBackupReport a un área de trabajo de Log Analytics en modo AzureDiagnostics. La segunda configuración, denominada **Setting2**, envía datos de los seis eventos de Azure Backup nuevos a un área de trabajo de Log Analytics en el modo específico del recurso. La tercera configuración, denominada **Setting3**, envía datos de los eventos de Azure Site Recovery a un área de trabajo de Log Analytics en modo de Azure Diagnostics.

![Tres configuraciones](./media/backup-azure-diagnostics-events/three-settings-example.png)

## <a name="next-steps"></a>Pasos siguientes

[Obtenga información sobre el modelo de datos de Log Analytics para eventos de diagnóstico](https://docs.microsoft.com/azure/backup/backup-azure-reports-data-model)
