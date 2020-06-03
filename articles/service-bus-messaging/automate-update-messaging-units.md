---
title: 'Azure Service Bus: actualización automática de las unidades de mensajería'
description: En este artículo se muestra cómo puede usar un runbook de Azure Automation para actualizar automáticamente las unidades de mensajería de un espacio de nombres de Service Bus.
services: service-bus-messaging
ms.service: service-bus-messaging
documentationcenter: ''
author: spelluru
ms.topic: how-to
ms.date: 05/14/2020
ms.author: spelluru
ms.openlocfilehash: 5ece7beaea709c9b1e52cf2130484663da0aa4ac
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83663103"
---
# <a name="automatically-update-messaging-units-of-an-azure-service-bus-namespace"></a>Actualización automática de las unidades de mensajería de un espacio de nombres de Azure Service Bus 
En este artículo se muestra cómo puede actualizar automáticamente las [unidades de mensajería](service-bus-premium-messaging.md) de un espacio de nombres de Service Bus basado en el uso de recursos (CPU o memoria). 

En el ejemplo de este artículo se muestra cómo aumentar las unidades de mensajería para un espacio de nombres de Service Bus cuando el uso de CPU del espacio de nombres supera el 75 %. Los pasos generales son los siguientes:

1. Cree un runbook de Azure Automation con un script de PowerShell que escale verticalmente (aumente) las unidades de mensajería para un espacio de nombres de Service Bus. 
2. Cree una alerta de uso de CPU en el espacio de nombres de Service Bus que invoque el script de PowerShell cuando el uso de CPU del espacio de nombres supere el 75 %. 

> [!IMPORTANT]
> Este artículo se aplica solo al nivel **Premium** de Azure Service Bus. 


## <a name="create-a-service-bus-namespace"></a>Creación de un espacio de nombres de Service Bus
Cree un espacio de nombres de Service Bus del nivel Premier. Siga los pasos del artículo [Creación de un espacio de nombres en Azure Portal](service-bus-quickstart-portal.md#create-a-namespace-in-the-azure-portal) para crear el espacio de nombres. 

## <a name="create-an-azure-automation-account"></a>Creación de una cuenta de Azure Automation
Cree una cuenta de Azure Automation siguiendo las instrucciones del artículo [Creación de una cuenta de Azure Automation](../automation/automation-quickstart-create-account.md). 

## <a name="import-azservice-module-from-gallery"></a>Importación del módulo Az.Service de la galería
Importe los módulos `Az.Accounts` y `Az.ServiceBus` de la galería a la cuenta de Automation. El módulo `Az.ServiceBus` depende del módulo `Az.Accounts`, por lo que debe instalarse primero. 

Para obtener instrucciones paso a paso, consulte [Importación de un módulo desde la galería de módulos](../automation/automation-runbook-gallery.md#import-a-module-from-the-module-gallery-with-the-azure-portal).

## <a name="create-and-publish-a-powershell-runbook"></a>Creación y publicación de un runbook de PowerShell

1. Para crear un runbook de PowerShell, siga las instrucciones del artículo [Creación de un runbook de PowerShell](../automation/automation-quickstart-create-runbook.md). 

    Este es un script de PowerShell de ejemplo que puede usar para aumentar las unidades de mensajería de un espacio de nombres de Service Bus. Este script de PowerShell en un runbook de Automation aumenta las unidades de mensajería de 1 a 2, de 2 a 4 o de 4 a 8. Los valores permitidos para esta propiedad son 1, 2, 4 y 8. Puede crear otro runbook para reducir las unidades de mensajería.

    Los parámetros **namespaceName** y **resourceGroupName** se usan para probar el script por separado del escenario de alertas. 
    
    El parámetro **WebHookData** es para que la alerta pase información como el nombre del grupo de recursos, el nombre del recurso, etc. en tiempo de ejecución. 

    ```powershell
    [OutputType("PSAzureOperationResponse")]
    param
    (
        [Parameter (Mandatory=$false)]
        [object] $WebhookData,
    
        [Parameter (Mandatory = $false)]
        [String] $namespaceName,
    
        [Parameter (Mandatory = $false)]
        [String] $resourceGroupName
    )
    
    
    if ($WebhookData)
    {
        # Get the data object from WebhookData
        $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)
    
        # Get the alert schema ID
        $schemaId = $WebhookBody.schemaId

        # If it's a metric alert
        if ($schemaId -eq "AzureMonitorMetricAlert") {

            # Get the resource group name from the alert context
            $resourceGroupName = $AlertContext.resourceGroupName
            
            # Get the namespace name from the alert context
            $namespaceName = $AlertContext.resourceName
        }
    }
    
    # Connect to Azure account
    $connection = Get-AutomationConnection -Name AzureRunAsConnection
    
    while(!($connectionResult) -And ($logonAttempt -le 10))
    {
        $LogonAttempt++
        # Logging in to Azure...
        $connectionResult =    Connect-AzAccount `
                                    -ServicePrincipal `
                                    -Tenant $connection.TenantID `
                                    -ApplicationId $connection.ApplicationID `
                                    -CertificateThumbprint $connection.CertificateThumbprint
    
        Start-Sleep -Seconds 30
    }
    
    # Get the current capacity (number of messaging units) of the namespace
    $sbusns=Get-AzServiceBusNamespace `
        -Name $namespaceName `
        -ResourceGroupName $resourceGroupName
    
    $currentCapacity = $sbusns.Sku.Capacity
    
    # Increase the capacity
    # Capacity can be one of these values: 1, 2, 4, 8
    if ($currentCapacity -eq 1) {
        $newMU = 2
    }
    elseif ($currentCapacity -eq 2) {
        $newMU = 4
    }
    elseif ($currentCapacity -eq 4) {
        $newMU = 8    
    }
    else {
    
    }
    
    # Update the capacity of the namespace
    Set-AzServiceBusNamespace `
            -Location eastus `
            -SkuName Premium `
            -Name $namespaceName `
            -SkuCapacity $newMU `
            -ResourceGroupName $resourceGroupName

    ```
2. [Pruebe el libro ](../automation/manage-runbooks.md#test-a-runbook); para ello, especifique los valores de los parámetros **namespaceName** y **resourceGroupName**. Confirme que las unidades de mensajería del espacio de nombres estén actualizadas. 
3. Después de realizar la prueba correctamente, [publique el libro](..//automation/manage-runbooks.md#publish-a-runbook) de modo que esté disponible para agregarlo como una acción para una alerta en el espacio de nombres más adelante. 

## <a name="create-an-alert-on-the-namespace-to-trigger-the-runbook"></a>Creación de una alerta en el espacio de nombres para desencadenar el runbook
Consulte el artículo [Uso de una alerta para desencadenar un runbook de Azure Automation](../automation/automation-create-alert-triggered-runbook.md) para configurar una alerta en el espacio de nombres de Service Bus y desencadenar el runbook de Automation que creó. Por ejemplo, puede crear una alerta sobre la métrica del **uso de CPU por espacio de nombres** o del **uso del tamaño de memoria por espacio de nombres**, y agregar una acción para desencadenar el runbook de Automation que creó. Para más información sobre estas métricas, consulte [Métricas de uso de recursos](service-bus-metrics-azure-monitor.md#resource-usage-metrics).

El siguiente procedimiento muestra cómo crear una alerta que desencadene el runbook de Automation cuando el **uso de CPU** del espacio de nombres supere el **75 %** .

1. En la página **Espacio de nombres de Service Bus** del espacio de nombres, seleccione **Alertas** en el menú de la izquierda y, a continuación, seleccione **+ Nueva regla de alerta** en la barra de herramientas. 
    
    ![Página Alertas: botón Nueva regla de alerta](./media/automate-update-messaging-units/alerts-page.png)
2. En la página **Crear regla de alerta**, haga clic en **Seleccionar condición**. 

    ![Página Crear regla de alerta: Seleccionar condición](./media/automate-update-messaging-units/alert-rule-select-condition.png) 
3. En la página **Configurar lógica de señal**, seleccione **CPU** para la señal. 

    ![Configuración de la lógica de señal: seleccionar CPU](./media/automate-update-messaging-units/configure-signal-logic.png)
4. Escriba un **valor de umbral** (en este ejemplo, es **75 %** ) y seleccione **Listo**. 

    ![Configuración de la señal de CPU](./media/automate-update-messaging-units/cpu-signal-configuration.png)
5. Ahora, en la **página Crear alerta**, haga clic en **Seleccionar grupo de acciones**.
    
    ![Selección del grupo de acciones](./media/automate-update-messaging-units/select-action-group-button.png)
6. Seleccione el botón **Crear grupo de acciones** en la barra de herramientas. 

    ![Creación de un botón de grupo de acciones](./media/automate-update-messaging-units/create-action-group-button.png)
7. En la página **Agregar grupo de acciones**, realice los pasos siguientes:
    1. Escriba un **nombre** para el grupo de acciones. 
    2. Escriba un **nombre corto** para el grupo de acciones.
    3. Seleccione la **suscripción** en la que desee crear este grupo de acciones.
    4. Seleccione el **grupo de recursos**. 
    5. En la sección **Acciones**, escriba un **nombre para la acción** y seleccione **Runbook de Automation** en **Tipo de acción**. 

        ![Página Agregar grupo de acciones](./media/automate-update-messaging-units/add-action-group-page.png)
8. En la página **Configurar Runbook**, siga estos pasos:
    1. En **Origen de Runbook**, seleccione **Usuario**. 
    2. En **Suscripción**, seleccione su **suscripción** de Azure que contenga la cuenta de Automation. 
    3. En la página **Cuenta de Automation**, seleccione su **cuenta de Automation**.
    4. En **Runbook**, seleccione su runbook. 
    5. Seleccione **Aceptar** en la página **Configurar Runbook**. 
        ![Configurar Runbook](./media/automate-update-messaging-units/configure-runbook.png)
9. Seleccione **Aceptar** en la página **Agregar grupo de acciones**. 
5. Ahora, en la página **Crear regla de alerta**, escriba un **nombre para la regla** y, a continuación, seleccione **Crear regla de alerta**. 
    ![Crear regla de alerta](./media/automate-update-messaging-units/create-alert-rule.png)

    > [!NOTE]
    > Ahora, cuando el uso de CPU del espacio de nombres supera el 75 %, la alerta desencadena el runbook de Automation, que aumenta las unidades de mensajería del espacio de nombres de Service Bus. Del mismo modo, puede crear una alerta para otro runbook de Automation, lo que reduce las unidades de mensajería si el uso de CPU del espacio de nombres desciende por debajo de 25 %. 

## <a name="next-steps"></a>Pasos siguientes
Para obtener información acerca de las unidades de mensajería, consulte la [mensajería Prémium](service-bus-premium-messaging.md)