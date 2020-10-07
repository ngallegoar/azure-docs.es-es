---
title: Registro de una conexión de la versión preliminar de Peering Service mediante la CLI de Azure
description: Aprenderá a registrar una conexión de Peering Service mediante la CLI de Azure.
services: peering-service
author: derekolo
ms.service: peering-service
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: Infrastructure-services
ms.date: 05/2/2020
ms.author: derekol
ms.openlocfilehash: ef573817927cf732da3426d802f8f26e2e9cd4ec
ms.sourcegitcommit: 4313e0d13714559d67d51770b2b9b92e4b0cc629
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2020
ms.locfileid: "91399030"
---
# <a name="register-a-peering-service-connection-by-using-the-azure-cli"></a>Registro de una conexión de Peering Service mediante la CLI de Azure

Azure Peering Service es un servicio de red que mejora la conectividad de los clientes a los servicios en la nube de Microsoft, como Microsoft 365, Dynamics 365, servicios de software como servicio (SaaS), Azure o cualquier otro servicio de Microsoft accesible a través de la red pública de Internet. En este artículo, aprenderá a registrar una conexión de Peering Service mediante la CLI de Azure.

Si no tiene una suscripción a Azure, [cree una cuenta](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) ahora.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este artículo es preciso la versión 2.0.28 o posterior de la CLI de Azure. Para encontrar la versión, ejecute `az --version`. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

## <a name="prerequisites"></a>Requisitos previos 

Debe tener lo siguiente:

### <a name="azure-account"></a>Cuenta de Azure

Debe tener una cuenta de Microsoft Azure válida y activa. Esta cuenta es necesaria para configurar la conexión de Peering Service. Peering Service es un recurso dentro de las suscripciones de Azure.

### <a name="connectivity-provider"></a>Proveedor de conectividad

Puede trabajar con un proveedor de servicios Internet o con un asociado de intercambio de Internet para obtener Peering Service con el fin de conectar la red a la red de Microsoft.

Asegúrese de que los proveedores de conectividad se asocien con Microsoft.

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Inicie sesión en la cuenta de Azure y seleccione su suscripción.

Para empezar la configuración, inicie sesión en la cuenta de Azure. Si usa la opción **Try It** de CloudShell, iniciará sesión de manera automática. Use los siguientes ejemplos para conectarse.

```azurecli-interactive
az login
```

Compruebe las suscripciones para la cuenta.

```azurecli-interactive
az account list
```

Seleccione la suscripción para la que desea registrar la conexión de Peering Service.

```azurecli-interactive
az account set --subscription "<subscription ID>"
```

Si todavía no tiene un grupo de recursos, debe crear uno antes de registrar la conexión de Peering Service. Puede crear uno si ejecuta el comando siguiente:

```azurecli-interactive
az group create -n MyResourceGroup -l "West US"
```

### <a name="2-register-your-subscription-with-the-resource-provider-and-feature-flag"></a>2. Registre la suscripción con el proveedor de recursos y la marca de característica.

Antes de continuar con los pasos para registrar la conexión de Peering Service mediante la CLI de Azure, registre la suscripción con el proveedor de recursos y la marca de característica mediante la CLI de Azure. Los comandos de la CLI de Azure se especifican aquí:

```azurecli-interactive

az feature register --namespace Microsoft.Peering --name AllowPeeringService

```

### <a name="3-register-the-peering-service-connection"></a>3. Registro de la conexión de Peering Service

Registre la conexión de Peering Service mediante el siguiente conjunto de comandos mediante la CLI de Azure. En este ejemplo se registra la conexión de Peering Service denominada myPeeringService.

```azurecli-interactive
az peering service create : Create peering service\
  --location -l \
  --name myPeeringService\
  --resource-group -g MyResourceGroup\
  --peering-service-location\
  --peering-service-provider\
  --tags
```

### <a name="4-register-the-prefix"></a>4. Registre el prefijo.

Registre el prefijo que proporciona el proveedor de conectividad mediante la ejecución de los comandos siguientes a través de la CLI de Azure. En este ejemplo se registra el prefijo denominado myPrefix.

```azurecli-interactive
az peering service prefix create \
  --name  myPrefix\
  --peering-service-name myPeeringService\
  --resource-group  -g myResourceGroup\
```

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre la conexión de Peering Service, consulte [Conexión de Peering Service](connection.md).
- Para más información sobre la telemetría de la conexión de Peering Service, consulte [Telemetría de la conexión de Peering Service](connection-telemetry.md).
- Para medir la telemetría, consulte [Medición de la telemetría de la conexión](measure-connection-telemetry.md).
- Para registrar la conexión mediante Azure PowerShell, consulte [Registro de una conexión de Peering Service: Azure PowerShell](powershell.md).
- Para registrar la conexión mediante Azure Portal, consulte [Registro de una conexión de Peering Service: Azure Portal](azure-portal.md).
