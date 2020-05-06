---
title: Actualización del plan de tarifa de un tema o dominio de Azure Event Grid
description: En este artículo se describe cómo actualizar el plan de tarifa de un tema o dominio de Azure Event Grid (Básico a Premium, Premium a Básico) mediante Azure Portal, CLI de Azure y Azure PowerShell.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/22/2020
ms.author: spelluru
ms.openlocfilehash: caea8d515964510fce432eb0497e3af19ecc1369
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82101644"
---
# <a name="update-pricing-tier"></a>Actualización del plan de tarifa 
En este artículo se indica cómo actualizar el plan de tarifa de un tema o dominio de Azure Event Grid mediante Azure Portal, CLI de Azure y Azure PowerShell. 

## <a name="use-azure-portal"></a>Usar Azure Portal
En esta sección se muestra cómo cambiar el plan de tarifa de un tema o un dominio en Azure Portal. 

### <a name="overview-page"></a>Página de información general
Puede cambiar el plan de tarifa de un tema o un dominio en la página **Información general**. En el ejemplo siguiente se muestra cómo actualizar un tema del nivel Básico al nivel Premium. Los pasos para cambiar a una versión anterior desde el nivel Premium al Básico son similares.

1. En [Azure Portal](https://portal.azure.com), vaya a la página del tema o dominio. 
2. En la página **Información general**, seleccione el plan de tarifa actual (en el ejemplo siguiente, este es **Básico**).
    
    ![Selección del plan de tarifa actual](./media/update-tier/select-tier.png)
3. En la página **Plan de tarifa**, cambie el nivel y seleccione **Aceptar**. 

    ![Actualización del plan de tarifa](./media/update-tier/change-tier.png)
4. Compruebe el estado de la operación en la página **Notificaciones**.

    ![Estado de actualización](./media/update-tier/status.png)    
5. Confirme que ve el nivel actualizado en la página **Información general**. 

    ![Estado de actualización](./media/update-tier/changed-tier.png)

### <a name="networking-page"></a>Página de redes
Puede **actualizar** del nivel Básico al Premium en la página **Redes**. Sin embargo, no puede cambiar del nivel Premium al Básico en esta página. 

1. En [Azure Portal](https://portal.azure.com), vaya a la página del tema o dominio. 
2. En la página **Redes**, vaya a la pestaña **Conexiones de punto de conexión privado (versión preliminar)** . 
3. Si el plan de tarifa actual es **Básico**, verá el mensaje siguiente. Selecciónelo. 

    ![Página Actualización del nivel en conexiones de punto de conexión privado](./media/update-tier/private-endpoint-connections-page.png)
4. En la página **Actualización al plan de tarifa Premium**, seleccione **Sí**. 
    
    ![Confirmación de la actualización](./media/update-tier/message-private-endpoint-connection.png)
5. Compruebe el estado de la operación en la página **Notificaciones**.

    ![Estado de actualización](./media/update-tier/status.png)



## <a name="use-azure-cli"></a>Uso de CLI de Azure
En esta sección se muestra cómo usar comandos de la CLI de Azure para cambiar el plan de tarifa de un tema. Para actualizar el plan de tarifa de un dominio, use el comando `az eventgrid domain update` de manera similar.

### <a name="prerequisites"></a>Prerrequisitos
Actualice la extensión de Azure Event Grid para la CLI mediante el siguiente comando: 

```azurecli-interactive
az extension update -n eventgrid
```

Si la extensión no está instalada, ejecute el siguiente comando para instalarla: 

```azurecli-interactive
az extension add -n eventgrid
```

### <a name="upgrade-a-topic-from-basic-to-premium"></a>Actualización de un tema del nivel Básico al Premium

```azurecli-interactive
az eventgrid topic update \
    --resource-group $resourceGroupName \
    --name $topicName \
    --location $location \
    --sku "Premium" 
```

### <a name="downgrade-a-topic-from-premium-to-basic"></a>Cambio a una versión anterior de un tema del nivel Premium al Básico

```azurecli-interactive
az eventgrid topic update \
    --resource-group $resourceGroupName \
    --name $topicName \
    --location $location \
    --sku "Basic" 
```


## <a name="use-azure-powershell"></a>Uso de Azure PowerShell
En esta sección se muestra cómo utilizar comandos de PowerShell para cambiar el plan de tarifa de un tema o dominio. 

### <a name="prepare-token-and-headers-for-rest-api-calls"></a>Preparación de tokens y encabezados para las llamadas de la API REST 
Ejecute los siguientes comandos como requisitos previos para obtener un token de autenticación para usarlo con llamadas de la API REST y la autorización y otra información de encabezado. 

```azurepowershell-interactive
# replace <CLIENT ID> and <CLIENT SECRET>
$body = "grant_type=client_credentials&client_id=<CLIENT ID>&client_secret=<CLIENT SECRET>&resource=https://management.core.windows.net"

# get the authentication token. Replace <TENANT ID>
$Token = Invoke-RestMethod -Method Post `
    -Uri https://login.microsoftonline.com/<TENANT ID>/oauth2/token `
    -Body $body `
    -ContentType 'application/x-www-form-urlencoded'

# set authorization and content-type headers
$Headers = @{}
$Headers.Add("Authorization","$($Token.token_type) "+ " " + "$($Token.access_token)")
$Headers.Add("Content-Type","application/json")
```

### <a name="upgrade-a-topic-from-basic-to-premium"></a>Actualización de un tema del nivel Básico al Premium

```azurepowershell-interactive
$body = @{"sku"=@{"name"="premium"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

### <a name="downgrade-a-topic-from-premium-to-basic"></a>Cambio a una versión anterior de un tema del nivel Premium al Básico

```azurepowershell-interactive
$body = @{"sku"=@{"name"="basic"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

### <a name="upgrade-a-domain-from-basic-to-premium"></a>Actualización de un dominio del nivel Básico al Premium

```azurepowershell-interactive
$body = @{"sku"=@{"name"="premium"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/domains/<EVENT GRID DOMAIN NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

### <a name="downgrade-a-domain-from-premium-to-basic"></a>Cambio a una versión anterior de un dominio del nivel Premium al Básico

```azurepowershell-interactive
$body = @{"sku"=@{"name"="basic"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/domains/<EVENT GRID DOMAIN NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

## <a name="next-steps"></a>Pasos siguientes
En el caso de los temas y dominios del nivel Premium, puede configurar puntos de conexión privados para restringir el acceso solo desde redes virtuales seleccionadas. Para obtener instrucciones detalladas, consulte [Configuración de puntos de conexión privados](configure-private-endpoints.md).
