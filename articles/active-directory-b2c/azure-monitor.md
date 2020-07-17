---
title: Supervisión de Azure AD B2C con Azure Monitor
titleSuffix: Azure AD B2C
description: Obtenga información acerca de cómo registrar eventos de Azure AD B2C con Azure Monitor mediante la administración de recursos delegados.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.author: mimart
ms.subservice: B2C
ms.date: 02/10/2020
ms.openlocfilehash: 3106e5a640ed66828558078e6986979ad7195450
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85386222"
---
# <a name="monitor-azure-ad-b2c-with-azure-monitor"></a>Supervisión de Azure AD B2C con Azure Monitor

Use Azure Monitor para enrutar los registros de inicio de sesión y [auditoría](view-audit-logs.md) de Azure Active Directory B2C (Azure AD B2C) a distintas soluciones de supervisión. Puede conservar los registros para su uso a largo plazo o integrarlos en herramientas de Administración de eventos e información de seguridad (SIEM) de terceros para sacar conclusiones sobre su entorno.

Puede enrutar los eventos de registro a:

* Una [cuenta de Azure Storage](../storage/blobs/storage-blobs-introduction.md).
* Un [área de trabajo de Log Analytics](../azure-monitor/platform/resource-logs-collect-workspace.md) (para analizar los datos, crear paneles y alertar sobre eventos específicos).
* Un [centro de eventos](../event-hubs/event-hubs-about.md) de Azure (e integrarlo en sus instancias de Splunk y Sumo Logic).

![Azure Monitor](./media/azure-monitor/azure-monitor-flow.png)

## <a name="prerequisites"></a>Requisitos previos

Para completar los pasos de este artículo, implementará una plantilla de Azure Resource Manager con el módulo Azure PowerShell.

* [Módulo Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps), versión 6.13.1 o posterior.

También puede usar [Azure Cloud Shell](https://shell.azure.com), que incluye la versión más reciente del módulo Azure PowerShell.

## <a name="delegated-resource-management"></a>Administración de recursos delegados

Azure AD B2C aprovecha la [supervisión de Azure Active Directory](../active-directory/reports-monitoring/overview-monitoring.md). Para habilitar *Configuración de diagnóstico* en Azure Active Directory dentro del inquilino de Azure AD B2C, use la [administración de recursos delegados](../lighthouse/concepts/azure-delegated-resource-management.md).

Autorice a un usuario en el directorio de Azure AD B2C (el **Proveedor de servicios**) para configurar la instancia de Azure Monitor en el inquilino que contiene su suscripción a Azure (el **Cliente**). Para crear la autorización, implemente una plantilla de [Azure Resource Manager](../azure-resource-manager/index.yml) en el inquilino de Azure AD que contiene la suscripción. Las secciones siguientes le guiarán por el proceso.

## <a name="create-or-choose-resource-group"></a>Crear o elegir un grupo de recursos

Este es el grupo de recursos que contiene la cuenta de almacenamiento de Azure de destino, el centro de eventos o el área de trabajo de Log Analytics para recibir datos de Azure Monitor. El nombre del grupo de recursos se especifica al implementar la plantilla de Azure Resource Manager.

[Cree un grupo de recursos](../azure-resource-manager/management/manage-resource-groups-portal.md#create-resource-groups) o elija uno existente en el inquilino de Azure Active Directory (Azure AD) que contiene su suscripción a Azure (*no* el directorio que contiene el inquilino de Azure AD B2C).

En este ejemplo se usa un grupo de recursos denominado *azure-ad-b2c-monitor* en la región *Centro de EE. UU*.

## <a name="delegate-resource-management"></a>Administración de recursos delegados

A continuación, recopile la siguiente información:

El **Id. de directorio** del directorio de Azure AD B2C (también conocido como Id. de inquilino).

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como usuario con rol de *Administrador de usuarios* (o superior).
1. Seleccione el icono **Directorio y suscripción** en la barra de herramientas del portal y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. Seleccione **Azure Active Directory** y luego **Propiedades**.
1. Registre el **ID. de directorio**.

El **Id. de objeto** del grupo o usuario de Azure AD B2C al que desea conceder el permiso de *Colaborador* para el grupo de recursos que creó anteriormente en el directorio que contiene la suscripción.

Para facilitar la administración, se recomienda usar *grupos* de usuarios de Azure AD para cada rol, lo que le permite agregar o quitar usuarios individuales en el grupo, en lugar de asignar permisos directamente a ese usuario. En este tutorial, agrega un usuario.

1. Con **Azure Active Directory** aún seleccionado en Azure Portal, seleccione **Usuarios** y, a continuación, seleccione un usuario.
1. Registre el **Id. de objeto** del usuario.

### <a name="create-an-azure-resource-manager-template"></a>Creación de una plantilla de Azure Resource Manager

Para incorporar el inquilino de Azure AD (el **Cliente**), cree una [plantilla de Azure Resource Manager](../lighthouse/how-to/onboard-customer.md) para la oferta con la información siguiente. Verá los valores `mspOfferName` y `mspOfferDescription` al ver los detalles de la oferta en la [página Proveedores de servicios](../lighthouse/how-to/view-manage-service-providers.md) de Azure Portal.

| Campo   | Definición |
|---------|------------|
| `mspOfferName`                     | Nombre que describe esta definición. Por ejemplo, *Azure AD B2C Managed Service*. Este valor se muestra al cliente como el título de la oferta. |
| `mspOfferDescription`              | Descripción breve de la oferta. Por ejemplo, *Enables Azure Monitor in Azure AD B2C*.|
| `rgName`                           | Nombre del grupo de recursos que creó anteriormente en el inquilino de Azure AD. Por ejemplo, *azure-ad-b2c-monitor*. |
| `managedByTenantId`                | **Id. de directorio** del inquilino de Azure AD B2C (también conocido como Id. de inquilino). |
| `authorizations.value.principalId` | **Id. de objeto** del grupo o usuario de B2C que tendrá acceso a los recursos de esta suscripción a Azure. Para este tutorial, especifique el id. de objeto del usuario que registró anteriormente. |

Descargue la plantilla de Azure Resource Manager y los archivos de parámetros:

- [rgDelegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.json)
- [rgDelegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.parameters.json)

A continuación, actualice el archivo de parámetros con los valores que registró anteriormente. En el siguiente fragmento de código JSON se muestra un ejemplo de archivo de parámetros de una plantilla de Azure Resource Manager. Para `authorizations.value.roleDefinitionId`, use el valor [rol integrado](../role-based-access-control/built-in-roles.md) para el *rol Colaborador*, `b24988ac-6180-42a0-ab88-20f7382dd24c`.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "mspOfferName": {
            "value": "Azure AD B2C Managed Services"
        },
        "mspOfferDescription": {
            "value": "Enables Azure Monitor in Azure AD B2C"
        },
        "rgName": {
            "value": "azure-ad-b2c-monitor"
        },
        "managedByTenantId": {
            "value": "<Replace with DIRECTORY ID of Azure AD B2C tenant (tenant ID)>"
        },
        "authorizations": {
            "value": [
                {
                    "principalId": "<Replace with user's OBJECT ID>",
                    "principalIdDisplayName": "Azure AD B2C tenant administrator",
                    "roleDefinitionId": "b24988ac-6180-42a0-ab88-20f7382dd24c"
                }
            ]
        }
    }
}
```

### <a name="deploy-the-azure-resource-manager-templates"></a>Implementación de las plantillas de Azure Resource Manager

Una vez que haya actualizado el archivo de parámetros, implemente la plantilla de Azure Resource Manager en el inquilino de Azure como implementación de nivel de suscripción. Dado que se trata de una implementación de nivel de suscripción, no se puede iniciar en Azure Portal. Puede utilizar el módulo Azure PowerShell o la CLI de Azure para la implementación. A continuación se muestra el método de Azure PowerShell.

Inicie sesión en el directorio que contiene la suscripción mediante [Connect-AzAccount](/powershell/azure/authenticate-azureps). Use la marca `-tenant` para forzar la autenticación en el directorio correcto.

```PowerShell
Connect-AzAccount -tenant contoso.onmicrosoft.com
```

Use el cmdlet [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription) para enumerar las suscripciones a las que puede tener acceso la cuenta actual en el inquilino de Azure AD. Registre el identificador de la suscripción que desea proyectar al inquilino de Azure AD B2C.

```PowerShell
Get-AzSubscription
```

A continuación, cambie a la suscripción que desea proyectar al inquilino de Azure AD B2C:

``` PowerShell
Select-AzSubscription <subscription ID>
```

Por último, implemente la plantilla de Azure Resource Manager y los archivos de parámetros que descargó y actualizó anteriormente. Reemplace los valores de `Location`, `TemplateFile`y `TemplateParameterFile` en consecuencia.

```PowerShell
New-AzDeployment -Name "AzureADB2C" `
                 -Location "centralus" `
                 -TemplateFile "C:\Users\azureuser\Documents\rgDelegatedResourceManagement.json" `
                 -TemplateParameterFile "C:\Users\azureuser\Documents\rgDelegatedResourceManagement.parameters.json" `
                 -Verbose
```

La implementación correcta de la plantilla genera un resultado similar al siguiente (el resultado se ha truncado por motivos de brevedad):

```Console
PS /usr/csuser/clouddrive> New-AzDeployment -Name "AzureADB2C" `
>>                  -Location "centralus" `
>>                  -TemplateFile "rgDelegatedResourceManagement.json" `
>>                  -TemplateParameterFile "rgDelegatedResourceManagement.parameters.json" `
>>                  -Verbose
WARNING: Breaking changes in the cmdlet 'New-AzDeployment' :
WARNING:  - The cmdlet 'New-AzSubscriptionDeployment' is replacing this cmdlet.


WARNING: NOTE : Go to https://aka.ms/azps-changewarnings for steps to suppress this breaking change warning, and other information on breaking changes in Azure PowerShell.
VERBOSE: 7:25:14 PM - Template is valid.
VERBOSE: 7:25:15 PM - Create template deployment 'AzureADB2C'
VERBOSE: 7:25:15 PM - Checking deployment status in 5 seconds
VERBOSE: 7:25:42 PM - Resource Microsoft.ManagedServices/registrationDefinitions '44444444-4444-4444-4444-444444444444' provisioning status is succeeded
VERBOSE: 7:25:48 PM - Checking deployment status in 5 seconds
VERBOSE: 7:25:53 PM - Resource Microsoft.Resources/deployments 'rgAssignment' provisioning status is running
VERBOSE: 7:25:53 PM - Checking deployment status in 5 seconds
VERBOSE: 7:25:59 PM - Resource Microsoft.ManagedServices/registrationAssignments '11111111-1111-1111-1111-111111111111' provisioning status is running
VERBOSE: 7:26:17 PM - Checking deployment status in 5 seconds
VERBOSE: 7:26:23 PM - Resource Microsoft.ManagedServices/registrationAssignments '11111111-1111-1111-1111-111111111111' provisioning status is succeeded
VERBOSE: 7:26:23 PM - Checking deployment status in 5 seconds
VERBOSE: 7:26:29 PM - Resource Microsoft.Resources/deployments 'rgAssignment' provisioning status is succeeded

DeploymentName          : AzureADB2C
Location                : centralus
ProvisioningState       : Succeeded
Timestamp               : 1/31/20 7:26:24 PM
Mode                    : Incremental
TemplateLink            :
Parameters              :
                          Name                   Type                       Value
                          =====================  =========================  ==========
                          mspOfferName           String                     Azure AD B2C Managed Services
                          mspOfferDescription    String                     Enables Azure Monitor in Azure AD B2C
...
```

Después de implementar la plantilla, la proyección de recursos puede tardar unos minutos en completarse. Es posible que tenga que esperar unos minutos (por lo general, no más de cinco) antes de pasar a la siguiente sección para seleccionar la suscripción.

## <a name="select-your-subscription"></a>Seleccione su suscripción.

Una vez que haya implementado la plantilla y haya esperado unos minutos a que se complete la proyección de recursos, asocie la suscripción al directorio de Azure AD B2C con los pasos siguientes.

1. **Cierre la sesión** de Azure Portal si está conectado actualmente. Esto y el paso siguiente se realizan para actualizar las credenciales en la sesión del portal.
1. Inicie sesión en [Azure Portal](https://portal.azure.com) con su cuenta de administrador de Azure AD B2C.
1. Seleccione el icono **Directorio y suscripción** en la barra de herramientas del portal.
1. Elija el directorio que contiene la suscripción.

    ![Cambiar de directorio](./media/azure-monitor/azure-monitor-portal-03-select-subscription.png)
1. Verifique haber seleccionado el directorio y la suscripción correctos. En este ejemplo, todos los directorios y suscripciones están seleccionados.

    ![Todos los directorios seleccionados en el filtro Directorio y suscripción](./media/azure-monitor/azure-monitor-portal-04-subscriptions-selected.png)

## <a name="configure-diagnostic-settings"></a>Configuración de valores de diagnóstico

La configuración de diagnóstico define dónde se deben enviar los registros y las métricas de un recurso. Los posibles destinos son:

- [Cuenta de Almacenamiento de Azure](../azure-monitor/platform/resource-logs-collect-storage.md)
- Soluciones de [Event Hubs](../azure-monitor/platform/resource-logs-stream-event-hubs.md).
- [Área de trabajo de Log Analytics](../azure-monitor/platform/resource-logs-collect-workspace.md)

Si aún no lo ha hecho, cree una instancia del tipo de destino elegido en el grupo de recursos que especificó en la [plantilla de Azure Resource Manager](#create-an-azure-resource-manager-template).

### <a name="create-diagnostic-settings"></a>Crear configuraciones de diagnóstico

Ya está listo para [crear configuraciones de diagnóstico](../active-directory/reports-monitoring/overview-monitoring.md) en Azure Portal.

Para establecer la configuración de supervisión de los registros de actividad de Azure AD B2C:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
1. Seleccione el icono **Directorio y suscripción** en la barra de herramientas del portal y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. Seleccione **Azure Active Directory**.
1. En **Supervisión**, seleccione **Configuración de diagnóstico**.
1. Si hay una configuración actual en el recurso, verá una lista de opciones ya configuradas. Puede seleccionar **Agregar configuración de diagnóstico** para agregar una nueva configuración o **Editar configuración** para modificar una existente. Cada configuración no puede tener más de uno de los tipos de destino.

    ![Panel de configuración de diagnóstico en Azure Portal](./media/azure-monitor/azure-monitor-portal-05-diagnostic-settings-pane-enabled.png)

1. Asigne un nombre a la configuración, si aún no lo tiene.
1. Marque la casilla de cada destino para enviar los registros. Haga clic en **Configurar** para especificar la configuración como se describe en la tabla siguiente.

    | Configuración | Descripción |
    |:---|:---|
    | Archivar en una cuenta de almacenamiento | Nombre de la cuenta de almacenamiento. |
    | Transmitir a un centro de eventos | Espacio de nombres donde se crea el centro de eventos (si es la primera vez que transmite registros de diagnóstico) o a donde se transmita (si ya hay recursos que estén transmitiendo esa categoría de registro a este espacio de nombres).
    | Enviar a Log Analytics | Nombre del área de trabajo. |

1. Seleccione **AuditLogs** y **SignInLogs**.
1. Seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo agregar y establecer la configuración de diagnóstico en Azure Monitor, consulte [Tutorial: Recopilación y análisis de registros de recurso desde un recurso de Azure](../azure-monitor/insights/monitor-azure-resource.md).

Para obtener información sobre la transmisión de registros de Azure AD a un centro de eventos, consulte [Tutorial: Transmisión de registros de Azure Active Directory a un centro de eventos de Azure](../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md).
