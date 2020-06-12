---
title: Referencias de la CLI de Azure para Azure Data Share
description: Página de aterrizaje de referencias de la CLI de Azure para Azure Data Share
services: data-share
author: dbradish-microsoft
manager: barbkess
ms.service: data-share
ms.devlang: azurecli
ms.topic: reference
ms.date: 05/27/2020
ms.author: dbradish
ms.openlocfilehash: 404022b13f44174e4b647f0430a58fac5eeb81df
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/02/2020
ms.locfileid: "84302647"
---
# <a name="azure-cli-for-azure-data-share"></a>CLI de Azure para Azure Data Share

La Interfaz de la línea de comandos de Azure ([CLI de Azure](/cli/azure/what-is-azure-cli)) es un conjunto de comandos que se usa para crear y administrar recursos de Azure.  Está disponible en muchos servicios de Azure, incluido Azure Data Share.  Hay más de 65 comandos diferentes para Data Share.  Estos comandos le permiten trabajar de forma eficaz con el servicio desde una línea de comandos.

## <a name="references-for-data-share"></a>Referencias para Data Share

Todos los comandos de la CLI de Azure para Azure Data Share son actualmente extensiones para la CLI de Azure.  Las extensiones proporcionan acceso a comandos experimentales y en versión preliminar.  Obtenga más información sobre las referencias de extensión en el artículo [Uso de extensiones con la CLI de Azure](/cli/azure/azure-cli-extensions-overview).

|Referencia de la CLI de Azure |Descripción
|-|-|-|
| [az datashare](/cli/azure/ext/datashare/datashare) | Todos los comandos para administrar Azure Data Share.
| [az datashare account](/cli/azure/ext/datashare/datashare/account) | Comandos para administrar cuentas de Azure Data Share.
| [az datashare consumer](/cli/azure/ext/datashare/datashare/consumer) | Comandos para que los consumidores administren Azure Data Share.
| [az datashare dataset](/cli/azure/ext/datashare/datashare/dataset) | Comandos para que los proveedores administren conjuntos de datos de Azure Data Share.
| [az datashare invitation](/cli/azure/ext/datashare/datashare/invitation) | Comandos para que los consumidores administren las invitaciones de Azure Data Share.
| [az datashare provider-share-subscription](/cli/azure/ext/datashare/datashare/provider-share-subscription) | Comandos para que los proveedores administren suscripciones de Azure Data Share.
| [az datashare synchronization](/cli/azure/ext/datashare/datashare/synchronization)  | Comandos para administrar la sincronización de Azure Data Share.
| [az datashare synchronization-setting](/cli/azure/ext/datashare/datashare/synchronization-setting)  | Comandos para que los proveedores administren la configuración de sincronización de Azure Data Share.

## <a name="reference-examples"></a>Ejemplos de referencia

Se proporcionan ejemplos con cada referencia de la CLI de Azure. Aunque también puede completar estas tareas a través de Azure Portal, el uso de la CLI de Azure requiere una sola línea de comandos.  A continuación se muestran algunos bloques de código que le ayudarán a hacerse una idea de lo fácil que es usar la CLI de Azure.

Para trabajar con Azure Data Share, primero necesitará un grupo de recursos.  Los grupos de recursos de Azure se pueden crear y administrar fácilmente con la CLI de Azure.  

```azurecli
#create a resource group
az group create -location westus -name MyResourceGroup
```

```azurecli
#get a list of resource groups for a subscription
az group list --subscription MySubscription --output table
```

Es tan sencillo como crear una cuenta de Data Share.

```azurecli
#create a data share account
az datashare account create --location "West US 2" --tags tag1=Red tag2=White --name MyAccount --resource-group MyResourceGroup
```

## <a name="see-also"></a>Consulte también

* Consulte la [introducción a la CLI de Azure](/cli/azure/get-started-with-azure-cli) para obtener información sobre la instalación y el inicio de sesión.

* Descubra referencias [principales](/cli/azure/reference-index) y de [extensión](/cli/azure/azure-cli-extensions-list) adicionales en la documentación de la CLI de Azure.
