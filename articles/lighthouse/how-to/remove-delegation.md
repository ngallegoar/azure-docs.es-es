---
title: Quitar el acceso a una delegación
description: Obtenga información acerca de cómo quitar el acceso a los recursos que se han delegado a un proveedor de servicios para la administración de recursos delegada de Azure.
ms.date: 04/24/2020
ms.topic: conceptual
ms.openlocfilehash: d0db809eb057f8b4bb48bdf9dd127f4d488f0406
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82149211"
---
# <a name="remove-access-to-a-delegation"></a>Quitar el acceso a una delegación

Una vez delegada la suscripción o el grupo de recursos de un cliente a un proveedor de servicios para la [administración de recursos delegada de Azure](../concepts/azure-delegated-resource-management.md), la delegación se puede quitar si es necesario. Una vez que se quita una delegación, ya no se aplicará el acceso concedido previamente a los usuarios del inquilino del proveedor de servicios.

La eliminación de una delegación puede realizarla un usuario en el inquilino del cliente o el inquilino del proveedor de servicios, siempre y cuando el usuario tenga los permisos adecuados.

## <a name="customers"></a>Clientes

Los usuarios del inquilino del cliente que tienen el [rol integrado propietario](../../role-based-access-control/built-in-roles.md#owner) para una suscripción pueden quitar el acceso del proveedor de servicios a esa suscripción (o a los grupos de recursos de esa suscripción). Para ello, un usuario en el inquilino del cliente puede ir a la [página Proveedores de servicios](view-manage-service-providers.md#add-or-remove-service-provider-offers) de Azure Portal, buscar la oferta en la pantalla **Ofertas del proveedor de servicios** y seleccionar el icono de la papelera en la fila de esa oferta.

Tras confirmar la eliminación, ningún usuario en el inquilino del proveedor de servicios podrá acceder a los recursos que se hayan delegado previamente.

## <a name="service-providers"></a>Proveedores de servicios

Los usuarios de un inquilino de administración pueden quitar el acceso a los recursos delegados si se les ha concedido el [rol para eliminar la asignación de registros de servicios administrados](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) para los recursos del cliente. Si este rol no se ha asignado a ningún usuario del proveedor de servicios, la delegación solo la puede quitar un usuario del inquilino del cliente.

En el ejemplo siguiente se muestra una asignación que concede el **rol para eliminar la asignación del registro de servicios administrados** que se puede incluir en un archivo de parámetros durante el [proceso de incorporación](onboard-customer.md):

```json
    "authorizations": [ 
        { 
            "principalId": "cfa7496e-a619-4a14-a740-85c5ad2063bb", 
            "principalIdDisplayName": "MSP Operators", 
            "roleDefinitionId": "91c1777a-f3dc-4fae-b103-61d183457e46" 
        } 
    ] 
```

Este rol también se puede seleccionar en una **autorización** durante la [creación de una oferta de servicio administrado](../../marketplace/partner-center-portal/create-new-managed-service-offer.md#authorization) para publicar en Azure Marketplace.

Un usuario con este permiso puede quitar una delegación de una de las siguientes maneras.

### <a name="azure-portal"></a>Azure Portal

1. Vaya a la página [Mis clientes](view-manage-customers.md).
2. Seleccione **Delegaciones**.
3. Busque la delegación que desea quitar y seleccione el icono de la papelera que aparece en su fila.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# Sign in as a user from the managing tenant directory 

Login-AzAccount

# Select the subscription that is delegated - or contains the delegated resource group(s)

Select-AzSubscription -SubscriptionName "<subscriptionName>"

# Get the registration assignment

Get-AzManagedServicesAssignment -Scope "/subscriptions/{delegatedSubscriptionId}"

# Delete the registration assignment

Remove-AzManagedServicesAssignment -ResourceId "/subscriptions/{delegatedSubscriptionId}/providers/Microsoft.ManagedServices/registrationAssignments/{assignmentGuid}"
```

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

# Sign in as a user from the managing tenant directory

az login

# Select the subscription that is delegated – or contains the delegated resource group(s)

az account set -s <subscriptionId/name>

# List registration assignments

az managedservices assignment list

# Delete the registration assignment

az managedservices assignment delete --assignment <id or full resourceId>
```

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre la [administración de recursos delegados de Azure](../concepts/azure-delegated-resource-management.md).
- Puede [ver y administrar clientes](view-manage-customers.md) desde **Mis clientes**, en Azure Portal.
