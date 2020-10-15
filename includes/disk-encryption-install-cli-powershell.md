---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: msmbaldwin
ms.service: virtual-machines
ms.topic: include
ms.date: 10/06/2019
ms.author: mbaldwin
ms.custom: include file
ms.openlocfilehash: 05794a046fdcb15a91145a75717a6a454d15a8da
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "72511521"
---
Azure Disk Encryption se puede habilitar y administrar mediante la [CLI de Azure](/cli/azure) y [Azure PowerShell](/powershell/azure/new-azureps-module-az). Para ello, es preciso instalar las herramientas localmente y conectarse a la suscripción de Azure.

### <a name="azure-cli"></a>Azure CLI

La [CLI de Azure 2.0](/cli/azure) es una herramienta de línea de comandos para administrar recursos de Azure. La CLI está diseñada para flexibilizar los datos de consulta, admitir operaciones de larga duración (como los procesos sin bloqueo) y facilitar la realización de scripts. Para instalarla localmente, siga los pasos que puede encontrar en [Instalación de la CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest).

Para [iniciar sesión en su cuenta de Azure con la CLI de Azure](/cli/azure/authenticate-azure-cli), use el comando [az login](/cli/azure/reference-index?view=azure-cli-latest#az-login).

```azurecli
az login
```

Si quiere seleccionar un inquilino con el que iniciar sesión, use:
    
```azurecli
az login --tenant <tenant>
```

Si tiene varias suscripciones y quiere especificar una en concreto, use [az account list](/cli/azure/account#az-account-list) para obtener la lista de suscripciones y [az account set](/cli/azure/account#az-account-set) para especificar cuál.
     
```azurecli
az account list
az account set --subscription "<subscription name or ID>"
```

Para más información, consulte [Service Fabric y CLI de Azure 2.0](/cli/azure/get-started-with-azure-cli). 

### <a name="azure-powershell"></a>Azure PowerShell
El [módulo az de Azure PowerShell](/powershell/azure/new-azureps-module-az) ofrece un conjunto de cmdlets que usan el modelo de [Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview) para administrar los recursos de Azure. Se puede usar en el explorador con [Azure Cloud Shell](/azure/cloud-shell/overview), o bien se puede instalar en una máquina local, para lo que hay que seguir las instrucciones de [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps). 

Si ya lo tiene instalado localmente, asegúrese de que usa la versión más reciente de la versión del SDK de Azure PowerShell para configurar Azure Disk Encryption. Descargue la versión más reciente de [Azure PowerShell](https://github.com/Azure/azure-powershell/releases).

Para [iniciar sesión en una cuenta de Azure con Azure PowerShell](/powershell/azure/authenticate-azureps?view=azps-2.5.0), utilice el cmdlet [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0).

```powershell
Connect-AzAccount
```

Si tiene varias suscripciones y desea especificar una de ellas, use el cmdlet [Get-AzSubscription](/powershell/module/Az.Accounts/Get-AzSubscription) para enumerarlas y, después, el cmdlet [Set-AzContext](/powershell/module/az.accounts/set-azcontext?view=azps-2.5.0):

```powershell
Set-AzContext -Subscription -Subscription <SubscriptionId>
```

La ejecución del cmdlet [Get-AzContext](/powershell/module/Az.Accounts/Get-AzContext) verificará que se ha seleccionado la suscripción correcta.

Para confirmar que están instalados los cmdlets de Azure Disk Encryption, use el cmdlet [Get-command](/powershell/module/microsoft.powershell.core/get-command?view=powershell-6):
     
```powershell
Get-command *diskencryption*
```
Para más información, consulte [Introducción a los cmdlets de Azure PowerShell](/powershell/azure/get-started-azureps). 