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
ms.openlocfilehash: e7b696ba052b2aca9e14628327c07275845607ad
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94540593"
---
# <a name="register-a-peering-service-connection-by-using-the-azure-cli"></a>Registro de una conexión de Peering Service mediante la CLI de Azure

Azure Peering Service es un servicio de red que mejora la conectividad de los clientes a los servicios en la nube de Microsoft, como Microsoft 365, Dynamics 365, servicios de software como servicio (SaaS), Azure o cualquier otro servicio de Microsoft accesible a través de la red pública de Internet. En este artículo, aprenderá a registrar una conexión de Peering Service mediante la CLI de Azure.

- This article requires version 2.0.28 or later of the Azure CLI. Ejecute [az version](/cli/azure/reference-index#az_version) para buscar cuál es la versión y las bibliotecas dependientes que están instaladas. Para realizar la actualización a la versión más reciente, ejecute [az upgrade](/cli/azure/reference-index#az_upgrade).

## <a name="prerequisites"></a>Requisitos previos 

Debe tener lo siguiente:

### <a name="azure-account"></a>Cuenta de Azure

Debe tener una cuenta de Microsoft Azure válida y activa. Esta cuenta es necesaria para configurar la conexión de Peering Service. Peering Service es un recurso dentro de las suscripciones de Azure.

### <a name="connectivity-provider"></a>Proveedor de conectividad

Puede trabajar con un proveedor de servicios Internet o con un asociado de intercambio de Internet para obtener Peering Service con el fin de conectar la red a la red de Microsoft.

Asegúrese de que los proveedores de conectividad se asocien con Microsoft.

[!INCLUDE [azure-cli-prepare-your-environment-h3.md](../../includes/azure-cli-prepare-your-environment-h3.md)]

- This article requires version 2.0.28 or later of the Azure CLI. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

### <a name="1-select-your-subscription"></a>Seleccione su suscripción.

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
