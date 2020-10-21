---
title: Registro automático del proveedor de recursos de VM con SQL
description: Obtenga información sobre cómo habilitar la característica de registro automático para registrar automáticamente todas las VM con SQL Server pasadas y futuras en el proveedor de recursos de VM de SQL mediante Azure Portal.
author: MashaMSFT
ms.author: mathoma
tags: azure-service-management
ms.service: virtual-machines-sql
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 09/21/2020
ms.openlocfilehash: b986832e5febbb2a0f88b65213f9acf0dd4c5ab5
ms.sourcegitcommit: 83610f637914f09d2a87b98ae7a6ae92122a02f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91996893"
---
# <a name="automatic-registration-with-sql-vm-resource-provider"></a>Registro automático del proveedor de recursos de VM con SQL
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Habilite la característica de registro automático en Azure Portal para registrar automáticamente todas las VM con SQL Server en Azure pasadas y futuras en el proveedor de recursos de VM con SQL en modo ligero.

En este artículo se enseña a habilitar la característica de registro automático. Como alternativa, puede [registrar una única VM](sql-vm-resource-provider-register.md)o [registrar las VM de forma masiva](sql-vm-resource-provider-bulk-register.md) en el proveedor de recursos de VM de SQL. 

## <a name="overview"></a>Introducción

El [proveedor de recursos de VM de SQL](sql-vm-resource-provider-register.md#overview) le permite administrar la VM con SQL Server desde Azure Portal. Además, el proveedor de recursos habilita un conjunto sólido de características, incluida la [aplicación automática de revisiones](automated-patching.md), la [copia de seguridad automática](automated-backup.md) y las funcionalidades de supervisión y administración. También se desbloquean las [licencias](licensing-model-azure-hybrid-benefit-ahb-change.md) y la flexibilidad de [ediciones](change-sql-server-edition.md). Anteriormente, estas características solo estaban disponibles para las imágenes de máquina virtual con SQL Server implementadas desde Azure Marketplace. 

La característica de registro automático permite a los clientes registrar automáticamente todas las VM con SQL Server pasadas y futuras en su suscripción de Azure en el proveedor de recursos de VM con SQL. Esto difiere del registro manual, que solo se centra en las VM con SQL Server actuales. 

El registro automático permite registrar las VM con SQL Server en modo ligero. Aún debe [actualizar manualmente al modo de administración completo](sql-vm-resource-provider-register.md#upgrade-to-full) para aprovechar todo el conjunto de características. 

## <a name="prerequisites"></a>Requisitos previos

Para registrar una máquina virtual con SQL Server con el proveedor de recursos, necesita lo siguiente: 

- Una [suscripción de Azure](https://azure.microsoft.com/free/).
- Un modelo de recursos de Azure [máquina virtual Windows ](../../../virtual-machines/windows/quick-create-portal.md) con [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads) implementado en la nube pública o de Azure Government. 


## <a name="enable"></a>Habilitar

Para habilitar el registro automático de las VM con SQL Server en Azure Portal, siga los pasos que se indican a continuación:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Vaya a la página del recurso [**Máquinas virtuales SQL**](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.SqlVirtualMachine%2FSqlVirtualMachines). 
1. Seleccione **Automatic SQL Server VM registration** (Registro automático de VM con SQL Server) para abrir la página **Automatic registration** (Registro automático). 

   :::image type="content" source="media/sql-vm-resource-provider-automatic-registration/automatic-registration.png" alt-text="Seleccione Registro automático de VM con SQL Server para abrir la página Registro automático.":::

1. Elija la suscripción en el menú desplegable. 
1. Lea los términos y, si está de acuerdo, seleccione **Acepto**. 
1. Seleccione **Registrarse** para habilitar la característica y registrar automáticamente todas las VM con SQL Server actuales y futuras en el proveedor de recursos de VM con SQL. Esto no reiniciará el servicio SQL Server en ninguna de las VM. 

## <a name="disable"></a>Deshabilitar

Use la [CLI de Azure](/cli/azure/install-azure-cli) o [Azure PowerShell](/powershell/azure/install-az-ps) para deshabilitar la característica de registro automático. Cuando la característica de registro automático está deshabilitada, las VM con SQL Server agregadas a la suscripción deben registrarse manualmente en el proveedor de recursos de VM con SQL. No se anulará el registro de las VM con SQL Server existentes que ya se hayan registrado.



# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para deshabilitar el registro automático mediante la CLI de Azure, ejecute el siguiente comando: 

```azurecli-interactive
az feature unregister --namespace Microsoft.SqlVirtualMachine --name BulkRegistration
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Para deshabilitar el registro automático mediante Azure PowerShell, ejecute el siguiente comando: 

```powershell-interactive
Unregister-AzProviderFeature -FeatureName BulkRegistration -ProviderNamespace Microsoft.SqlVirtualMachine
```

---

## <a name="enable-for-multiple-subscriptions"></a>Habilitación de varias suscripciones

Puede habilitar la característica de registro automático para varias suscripciones de Azure mediante PowerShell. 

Para ello, siga estos pasos:

1. Guarde [este script](https://github.com/microsoft/tigertoolbox/blob/master/AzureSQLVM/RegisterSubscriptionsToSqlVmAutomaticRegistration.ps1) en un archivo de `.ps1`, como `EnableBySubscription.ps1`. 
1. Navegue hasta la ubicación donde guardó el script mediante un símbolo del sistema administrativo o una ventana de PowerShell. 
1. Conéctese a Azure (`az login`).
1. Ejecute el script, pasando SubscriptionIds como parámetros, como:   
   `.\EnableBySubscription.ps1 -SubscriptionList SubscriptionId1,SubscriptionId2`

   Por ejemplo: 

   ```console
   .\EnableBySubscription.ps1 -SubscriptionList a1a1a-aa11-11aa-a1a1-a11a111a1,b2b2b2-bb22-22bb-b2b2-b2b2b2bb
   ```

Los errores de registro incorrecto se almacenan en `RegistrationErrors.csv`, que se encuentra en el mismo directorio donde guardó y dese el que ejecutó el script `.ps1`. 

## <a name="next-steps"></a>Pasos siguientes

Actualice el modo de administración a [completo](sql-vm-resource-provider-register.md#upgrade-to-full) para aprovechar todo el conjunto de características que proporciona el proveedor de recursos de VM con SQL. 
