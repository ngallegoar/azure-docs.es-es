---
title: Virtual Networks
titleSuffix: Azure Cognitive Services
description: Configure la seguridad de red por niveles para sus recursos de Cognitive Services.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: dapine
ms.openlocfilehash: 0988c8154c63bb408493edf3243078e625c80d53
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371229"
---
# <a name="configure-azure-cognitive-services-virtual-networks"></a>Configuración de redes virtuales de Azure Cognitive Services

Azure Cognitive Services proporciona un modelo de seguridad por niveles. Este modelo le permite proteger las cuentas de Cognitive Services en un subconjunto específico de redes. Cuando se configuran las reglas de red, solo las aplicaciones que solicitan datos del conjunto especificado de redes pueden acceder a la cuenta. Puede limitar el acceso a sus recursos con el filtrado de solicitudes. Puede permitir solo las solicitudes procedentes de direcciones IP especificadas, intervalos de IP o una lista de subredes de instancias de [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Si está interesado en esta oferta, deberá [solicitar acceso a la versión preliminar](https://aka.ms/cog-svc-vnet-signup).

Una aplicación que accede a un recurso de Cognitive Services cuando las reglas de red están en vigor requiere autorización. La autorización es compatible con las credenciales de [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) (Azure AD) o con una clave de API válida.

> [!IMPORTANT]
> Al activar las reglas de firewall para su cuenta de Cognitive Services, se bloquean las solicitudes de datos entrantes de forma predeterminada. Para permitir las solicitudes, se debe cumplir una de las siguientes condiciones:
> * La solicitud debe originarse en un servicio que funcione en una instancia de Azure Virtual Network (VNet) en la lista de subredes permitidas de la cuenta de Cognitive Services de destino. El punto de conexión en las solicitudes originadas en la red virtual debe establecerse como el [subdominio personalizado](cognitive-services-custom-subdomains.md) de la cuenta de Cognitive Services.
> * O bien, la solicitud debe originarse en una lista de direcciones IP permitidas.
>
> Las solicitudes que bloquean incluyen aquellas de otros servicios de Azure, desde Azure Portal, desde los servicios de registro y de métricas, etc.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scenarios"></a>Escenarios

Para proteger el recurso de Cognitive Services, primero debe configurar una regla para denegar el acceso al tráfico desde todas las redes (incluido el tráfico de Internet) de forma predeterminada. A continuación, debe configurar las reglas que conceden acceso al tráfico desde redes virtuales específicas. Esta configuración le permite crear un límite de red seguro para las aplicaciones. También puede configurar reglas para conceder acceso al tráfico desde intervalos de direcciones IP de Internet públicos, lo que permite habilitar conexiones desde clientes locales o específicos de Internet.

Se aplican reglas de red en todos los protocolos de red para Azure Cognitive Services, incluidos REST y WebSocket. Para tener acceso a los datos mediante herramientas como las consolas de prueba de Azure, deben configurarse reglas de red explícitas. Puede aplicar reglas de red a recursos de Cognitive Services existentes o al crear nuevos recursos de Cognitive Services. Una vez que se apliquen las reglas de red, se aplicarán a todas las solicitudes.

## <a name="supported-regions-and-service-offerings"></a>Ofertas de servicio y regiones admitidas

La compatibilidad de la red virtual con los servicios de Cognitive Services que se enumeran a continuación se limita a las siguientes regiones de Azure: *EUAP de centro de EE. UU.* , *Centro-sur de EE. UU.* , *Este de EE. UU.* , *Oeste de EE. UU. 2*, *Norte de Europa*, *Norte de Sudáfrica*, *Oeste de Europa*y *Centro de la India*, *Este de Australia*, *Oeste de EE. UU.* y *US Gov Virginia*. Si la oferta de servicio no aparece aquí, significa que no es compatible con redes virtuales.

> [!div class="checklist"]
> * [Anomaly Detector](./anomaly-detector/index.yml)
> * [Computer Vision](./computer-vision/index.yml)
> * [Content Moderator](./content-moderator/index.yml)
> * [Custom Vision](./custom-vision-service/index.yml)
> * [Face](./face/index.yml)
> * [Form Recognizer](./form-recognizer/index.yml)
> * [LUIS](./luis/index.yml)
> * [Personalizer](./personalizer/index.yml)
> * [Text Analytics](./text-analytics/index.yml)
> * [QnA Maker](./qnamaker/index.yml)

La compatibilidad de la red virtual con los servicios de Cognitive Services que se enumeran a continuación se limita a las siguientes regiones de Azure: *EUAP de centro de EE. UU.* , *Centro-sur de EE. UU.* , *Este de EE. UU.* , *Oeste de EE. UU. 2*, *Global* y *US Gov Virginia*.
> [!div class="checklist"]
> * [Translator Text](./translator/index.yml)

## <a name="service-tags"></a>Etiquetas de servicio
Además de admitir los puntos de conexión de servicio de red virtual para los servicios anteriores, Cognitive Services también admite una etiqueta de servicio para la configuración de reglas de red de salida. Los servicios siguientes se incluyen en la etiqueta de servicio CognitiveServicesManagement.
> [!div class="checklist"]
> * [Anomaly Detector](./anomaly-detector/index.yml)
> * [Computer Vision](./computer-vision/index.yml)
> * [Content Moderator](./content-moderator/index.yml)
> * [Custom Vision](./custom-vision-service/index.yml)
> * [Face](./face/index.yml)
> * [Form Recognizer](./form-recognizer/index.yml)
> * [LUIS](./luis/index.yml)
> * [Personalizer](./personalizer/index.yml)
> * [Text Analytics](./text-analytics/index.yml)
> * [QnA Maker](./qnamaker/index.yml)
> * [Translator Text](./translator/index.yml)
> * [Servicio Voz](./speech-service/index.yml)

## <a name="change-the-default-network-access-rule"></a>Modificación de la regla de acceso de red predeterminada

De forma predeterminada, los recursos de Cognitive Services aceptan conexiones de clientes en cualquier red. Para limitar el acceso a redes seleccionadas, primero debe cambiar la acción predeterminada.

> [!WARNING]
> La realización de cambios en reglas de red puede afectar a la capacidad de las aplicaciones de conexión a Azure Cognitive Services. Si se establece la regla de red predeterminada en **denegar**, se bloquea el acceso a los datos, a no ser que se apliquen también las reglas de red específicas para **conceder** acceso. Asegúrese de conceder acceso a las redes permitidas con reglas de red antes de cambiar la regla predeterminada para denegar el acceso. Si permite enumerar las direcciones IP de su red local, asegúrese de agregar todas las direcciones IP públicas salientes posibles de dicha red.

### <a name="managing-default-network-access-rules"></a>Administración de las reglas de acceso de red predeterminadas

Puede administrar las reglas predeterminadas de acceso a redes para los recursos de Cognitive Services a través de Azure Portal, PowerShell o la CLI de Azure.

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

1. Vaya al recurso de Cognitive Services que desea proteger.

1. Seleccione el menú **ADMINISTRACIÓN DE RECURSOS** de administración de recursos denominado **Red virtual**.

   ![Opción Red virtual](media/vnet/virtual-network-blade.png)

1. Para denegar el acceso de forma predeterminada, elija permitir el acceso desde **Redes seleccionadas**. Solo con la opción **Redes seleccionadas**, sin las opciones **Redes virtuales** ni **Intervalos de direcciones** configuradas, todo el acceso se deniega de forma efectiva. Cuando se deniega todo el acceso, no se permiten solicitudes que intenten consumir el recurso de Cognitive Services. Azure Portal, Azure PowerShell o la CLI de Azure se pueden seguir usando para configurar el recurso de Cognitive Services.
1. Para permitir el tráfico desde todas las redes, elija permitir el acceso desde **Todas las redes**.

   ![Denegación de redes virtuales](media/vnet/virtual-network-deny.png)

1. Seleccione **Guardar** para aplicar los cambios.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

1. Instale [Azure PowerShell](/powershell/azure/install-az-ps) e [inicie sesión](/powershell/azure/authenticate-azureps), o bien seleccione **Probar**.

1. Visualice el estado de la regla predeterminada para el recurso de Cognitive Services.

    ```azurepowershell-interactive
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
    }
    (Get-AzCognitiveServicesAccountNetworkRuleSet @parameters).DefaultAction
    ```

1. Establezca la regla predeterminada para denegar el acceso de red de forma predeterminada.

    ```azurepowershell-interactive
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
        -DefaultAction Deny
    }
    Update-AzCognitiveServicesAccountNetworkRuleSet @parameters
    ```

1. Establezca la regla predeterminada para permitir el acceso de red de forma predeterminada.

    ```azurepowershell-interactive
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
        -DefaultAction Allow
    }
    Update-AzCognitiveServicesAccountNetworkRuleSet @parameters
    ```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

1. Instale la [CLI de Azure](/cli/azure/install-azure-cli) e [inicie sesión](/cli/azure/authenticate-azure-cli), o bien seleccione **Probar**.

1. Visualice el estado de la regla predeterminada para el recurso de Cognitive Services.

    ```azurecli-interactive
    az cognitiveservices account show \
        -g "myresourcegroup" -n "myaccount" \
        --query networkRuleSet.defaultAction
    ```

1. Establezca la regla predeterminada para denegar el acceso de red de forma predeterminada.

    ```azurecli-interactive
    az cognitiveservices account update \
        -g "myresourcegroup" -n "myaccount" \
        --default-action Deny
    ```

1. Establezca la regla predeterminada para permitir el acceso de red de forma predeterminada.

    ```azurecli-interactive
    az cognitiveservices account update \
        -g "myresourcegroup" -n "myaccount" \
        --default-action Allow
    ```

***

## <a name="grant-access-from-a-virtual-network"></a>Concesión de acceso desde una red virtual

Puede configurar recursos de Cognitive Services para permitir el acceso solo desde subredes específicas. Las subredes permitidas pueden pertenecer a una red virtual de la misma suscripción o de una suscripción diferente, incluidas las suscripciones que pertenecen a un inquilino de Azure Active Directory diferente.

Habilite un [punto de conexión de servicio](../virtual-network/virtual-network-service-endpoints-overview.md) para Azure Cognitive Services dentro de la red virtual. El punto de conexión de servicio enruta el tráfico desde la red virtual a través de una ruta de acceso óptima al servicio Azure Cognitive Services. Las identidades de la red virtual y la subred también se transmiten con cada solicitud. Luego, los administradores pueden configurar reglas de red para el recurso de Cognitive Services que permitan que se reciban solicitudes desde subredes específicas en una red virtual. Los clientes a los que se concedió acceso a través de estas reglas de red deben seguir cumpliendo los requisitos de autorización del recurso de Cognitive Services para acceder a los datos.

Cada recurso de Cognitive Services admite hasta 100 reglas de red virtual, que se pueden combinar con [reglas de red de IP](#grant-access-from-an-internet-ip-range).

### <a name="required-permissions"></a>Permisos necesarios

Para aplicar una regla de red virtual a un recurso de Cognitive Services, el usuario debe tener permisos apropiados para las subredes que se agregan. El permiso necesario es el rol *Colaborador* predeterminado, o bien el rol *Colaborador de Cognitive Services*. También se pueden agregar los permisos necesarios a las definiciones de roles personalizados.

El recurso de Cognitive Services y las redes virtuales a las que se concedió acceso pueden estar en distintas suscripciones, incluidas las suscripciones que forman parte de un inquilino de Azure AD diferente.

> [!NOTE]
> La configuración de reglas que conceden acceso a subredes en redes virtuales que forman parte de un inquilino de Azure Active Directory diferente solo se admiten actualmente a través de PowerShell, la CLI y las API REST. Estas reglas no se pueden configurar mediante Azure Portal, aunque se puedan ver en el portal.

### <a name="managing-virtual-network-rules"></a>Administración de reglas de red virtual

Puede administrar las reglas de red virtual para los recursos de Cognitive Services a través de Azure Portal, PowerShell o la CLI de Azure.

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

1. Vaya al recurso de Cognitive Services que desea proteger.

1. Seleccione el menú **ADMINISTRACIÓN DE RECURSOS** de administración de recursos denominado **Red virtual**.

1. Compruebe haber elegido permitir el acceso desde **Redes seleccionadas**.

1. Para conceder acceso a una red virtual con una regla de red existente, en **Redes virtuales**, seleccione  **Agregar red virtual existente**.

   ![Adición de una red virtual existente](media/vnet/virtual-network-add-existing.png)

1. Seleccione las opciones **Redes virtuales** y **Subredes** y, a continuación, haga clic en **Habilitar**.

   ![Adición de detalles de red virtual existente](media/vnet/virtual-network-add-existing-details.png)

1. Para crear una nueva red virtual y concederle acceso, seleccione **Agregar nueva red virtual**.

   ![Adición de una nueva red virtual](media/vnet/virtual-network-add-new.png)

1. Proporcione la información necesaria para crear la nueva red virtual y, luego, seleccione **Crear**.

   ![Creación de una red virtual](media/vnet/virtual-network-create.png)

    > [!NOTE]
    > Si un punto de conexión de servicio para Azure Cognitive Services no se ha configurado previamente para la red virtual y las subredes seleccionadas, se puede configurar como parte de esta operación.
    >
    > Actualmente, solo se muestran las redes virtuales que pertenecen al mismo inquilino de Azure Active Directory para su selección durante la creación de la regla. Para conceder acceso a una subred de una red virtual que pertenece a otro inquilino, use PowerShell, la CLI o la API REST.

1. Para quitar una regla de red virtual o subred, seleccione **...** para abrir el menú contextual de la red virtual o la subred y seleccione **Quitar**.

   ![Eliminación de una red virtual](media/vnet/virtual-network-remove.png)

1. Seleccione **Guardar** para aplicar los cambios.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

1. Instale [Azure PowerShell](/powershell/azure/install-az-ps) e [inicie sesión](/powershell/azure/authenticate-azureps), o bien seleccione **Probar**.

1. Enumere las reglas de red virtual.

    ```azurepowershell-interactive
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
    }
    (Get-AzCognitiveServicesAccountNetworkRuleSet @parameters).VirtualNetworkRules
    ```

1. Habilite el punto de conexión de servicio para Azure Cognitive Services en una red virtual y una subred existentes.

    ```azurepowershell-interactive
    Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" `
        -Name "myvnet" | Set-AzVirtualNetworkSubnetConfig -Name "mysubnet" `
        -AddressPrefix "10.0.0.0/24" `
        -ServiceEndpoint "Microsoft.CognitiveServices" | Set-AzVirtualNetwork
    ```

1. Agregue una regla de red para una red virtual y subred.

    ```azurepowershell-interactive
    $subParameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myvnet"
    }
    $subnet = Get-AzVirtualNetwork @subParameters | Get-AzVirtualNetworkSubnetConfig -Name "mysubnet"

    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
        -VirtualNetworkResourceId $subnet.Id
    }
    Add-AzCognitiveServicesAccountNetworkRule @parameters
    ```

    > [!TIP]
    > Para agregar una regla de red para una subred de una red virtual que pertenezca a otro inquilino de Azure AD, use un parámetro completo **VirtualNetworkResourceId** con el formato "/subscriptions/subscription-ID/resourceGroups/resourceGroup-Name/providers/Microsoft.Network/virtualNetworks/vNet-name/subnets/subnet-name".

1. Quite una regla de red para una red virtual y subred.

    ```azurepowershell-interactive
    $subParameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myvnet"
    }
    $subnet = Get-AzVirtualNetwork @subParameters | Get-AzVirtualNetworkSubnetConfig -Name "mysubnet"

    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
        -VirtualNetworkResourceId $subnet.Id
    }
    Remove-AzCognitiveServicesAccountNetworkRule @parameters
    ```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

1. Instale la [CLI de Azure](/cli/azure/install-azure-cli) e [inicie sesión](/cli/azure/authenticate-azure-cli), o bien seleccione **Probar**.

1. Enumere las reglas de red virtual.

    ```azurecli-interactive
    az cognitiveservices account network-rule list \
        -g "myresourcegroup" -n "myaccount" \
        --query virtualNetworkRules
    ```

1. Habilite el punto de conexión de servicio para Azure Cognitive Services en una red virtual y una subred existentes.

    ```azurecli-interactive
    az network vnet subnet update -g "myresourcegroup" -n "mysubnet" \
    --vnet-name "myvnet" --service-endpoints "Microsoft.CognitiveServices"
    ```

1. Agregue una regla de red para una red virtual y subred.

    ```azurecli-interactive
    $subnetid=(az network vnet subnet show \
        -g "myresourcegroup" -n "mysubnet" --vnet-name "myvnet" \
        --query id --output tsv)

    # Use the captured subnet identifier as an argument to the network rule addition
    az cognitiveservices account network-rule add \
        -g "myresourcegroup" -n "myaccount" \
        --subnet $subnetid
    ```

    > [!TIP]
    > Para agregar una regla para una subred de una red virtual que pertenezca a otro inquilino de Azure AD, use un identificador de subred completo con el formato "/subscriptions/subscription-ID/resourceGroups/resourceGroup-Name/providers/Microsoft.Network/virtualNetworks/vNet-name/subnets/subnet-name".
    > 
    > Puede usar el parámetro **subscription** para recuperar el identificador de subred de una red virtual que pertenezca a otro inquilino de Azure AD.

1. Quite una regla de red para una red virtual y subred.

    ```azurecli-interactive
    $subnetid=(az network vnet subnet show \
        -g "myresourcegroup" -n "mysubnet" --vnet-name "myvnet" \
        --query id --output tsv)

    # Use the captured subnet identifier as an argument to the network rule removal
    az cognitiveservices account network-rule remove \
        -g "myresourcegroup" -n "myaccount" \
        --subnet $subnetid
    ```
***

> [!IMPORTANT]
> Asegúrese de [establecer la regla predeterminada](#change-the-default-network-access-rule) en **denegar** o, de lo contrario, las reglas de red no tendrán ningún efecto.

## <a name="grant-access-from-an-internet-ip-range"></a>Concesión de acceso desde un intervalo IP de Internet

Puede configurar recursos de Cognitive Services para permitir el acceso desde intervalos específicos de direcciones IP de Internet públicas. Esta configuración concede acceso a servicios específicos y redes locales, y bloquea el tráfico de Internet general de forma eficaz.

Proporcione intervalos de direcciones de Internet permitidos mediante la [notación CIDR](https://tools.ietf.org/html/rfc4632) en el formulario `16.17.18.0/24` o como direcciones IP individuales como `16.17.18.19`.

   > [!Tip]
   > Los intervalos de dirección pequeños con tamaños de prefijos "/31" o "/32" no son compatibles. Estos intervalos se deberían configurar utilizando reglas de direcciones IP individuales.

Las reglas de red IP solo se permiten para direcciones IP de **Internet público**. No se permiten intervalos de direcciones IP reservados para redes privadas (tal y como se define en [RFC 1918](https://tools.ietf.org/html/rfc1918#section-3)) en las reglas de IP. Las redes privadas incluyen direcciones que comienzan por `10.*`, `172.16.*` - `172.31.*` y `192.168.*`.

   > [!NOTE]
   > Las reglas de red IP no tienen ningún efecto en las solicitudes que proceden de la misma región de Azure que el recurso de Cognitive Services. Use [reglas de red virtual](#grant-access-from-a-virtual-network) para permitir solicitudes de la misma región.

Solo se admiten direcciones IPV4 en este momento. Cada recurso de Cognitive Services admite hasta 100 reglas de red IP, que se pueden combinar con [reglas de red virtual](#grant-access-from-a-virtual-network).

### <a name="configuring-access-from-on-premises-networks"></a>Configuración del acceso desde redes locales

Para conceder acceso desde las redes locales al recurso de Cognitive Services con una regla de red IP, debe identificar las direcciones IP orientadas a Internet que usa su red. Para obtener ayuda, póngase en contacto con el administrador de red.

Si usa [ExpressRoute](../expressroute/expressroute-introduction.md) localmente para el emparejamiento público o el emparejamiento de Microsoft, deberá identificar las direcciones IP de NAT. Para el emparejamiento público, cada circuito de ExpressRoute usa dos direcciones IP de NAT de forma predeterminada. Cada una de estas se aplica al tráfico del servicio de Azure cuando el tráfico entra en la red troncal de Microsoft Azure. Para el emparejamiento de Microsoft, las direcciones IP de NAT que se utilizan las proporciona el cliente o el proveedor de servicios. Para permitir el acceso a los recursos de servicio, tiene que permitir estas direcciones IP públicas en la configuración del firewall de IP de recursos. Para encontrar las direcciones de IP de circuito de ExpressRoute de los pares públicos, [abra una incidencia de soporte técnico con ExpressRoute](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) a través de Azure Portal. Obtenga más información sobre [NAT para ExpressRoute para emparejamiento público y de Microsoft.](../expressroute/expressroute-nat.md#nat-requirements-for-azure-public-peering)

### <a name="managing-ip-network-rules"></a>Administración de reglas de red IP

Puede administrar las reglas de red IP para los recursos de Cognitive Services a través de Azure Portal, PowerShell o la CLI de Azure.

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

1. Vaya al recurso de Cognitive Services que desea proteger.

1. Seleccione el menú **ADMINISTRACIÓN DE RECURSOS** de administración de recursos denominado **Red virtual**.

1. Compruebe haber elegido permitir el acceso desde **Redes seleccionadas**.

1. Para conceder acceso al intervalo IP de Internet, escriba la dirección IP o el intervalo de direcciones (en [formato CIDR](https://tools.ietf.org/html/rfc4632)) en **Firewall** > **Intervalo de direcciones**. Solo se aceptan direcciones IP públicas válidas (no reservadas).

   ![Adición de un intervalo de IP](media/vnet/virtual-network-add-ip-range.png)

1. Para quitar una regla de red IP, haga clic en el icono de la Papelera <span class="docon docon-delete x-hidden-focus"></span> junto al intervalo de direcciones.

   ![Eliminación de intervalo de IP](media/vnet/virtual-network-delete-ip-range.png)

1. Seleccione **Guardar** para aplicar los cambios.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

1. Instale [Azure PowerShell](/powershell/azure/install-az-ps) e [inicie sesión](/powershell/azure/authenticate-azureps), o bien seleccione **Probar**.

1. Enumere las reglas de red IP.

    ```azurepowershell-interactive
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
    }
    (Get-AzCognitiveServicesAccountNetworkRuleSet @parameters).IPRules
    ```

1. Agregue una regla de red para una dirección IP individual.

    ```azurepowershell-interactive
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
        -IPAddressOrRange "16.17.18.19"
    }
    Add-AzCognitiveServicesAccountNetworkRule @parameters
    ```

1. Agregue una regla de red para un intervalo de direcciones IP.

    ```azurepowershell-interactive
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
        -IPAddressOrRange "16.17.18.0/24"
    }
    Add-AzCognitiveServicesAccountNetworkRule @parameters
    ```

1. Quite una regla de red para una dirección IP individual.

    ```azurepowershell-interactive
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
        -IPAddressOrRange "16.17.18.19"
    }
    Remove-AzCognitiveServicesAccountNetworkRule @parameters
    ```

1. Quite una regla de red para un intervalo de direcciones IP.

    ```azurepowershell-interactive
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
        -IPAddressOrRange "16.17.18.0/24"
    }
    Remove-AzCognitiveServicesAccountNetworkRule @parameters
    ```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

1. Instale la [CLI de Azure](/cli/azure/install-azure-cli) e [inicie sesión](/cli/azure/authenticate-azure-cli), o bien seleccione **Probar**.

1. Enumere las reglas de red IP.

    ```azurecli-interactive
    az cognitiveservices account network-rule list \
        -g "myresourcegroup" -n "myaccount" --query ipRules
    ```

1. Agregue una regla de red para una dirección IP individual.

    ```azurecli-interactive
    az cognitiveservices account network-rule add \
        -g "myresourcegroup" -n "myaccount" \
        --ip-address "16.17.18.19"
    ```

1. Agregue una regla de red para un intervalo de direcciones IP.

    ```azurecli-interactive
    az cognitiveservices account network-rule add \
        -g "myresourcegroup" -n "myaccount" \
        --ip-address "16.17.18.0/24"
    ```

1. Quite una regla de red para una dirección IP individual.

    ```azurecli-interactive
    az cognitiveservices account network-rule remove \
        -g "myresourcegroup" -n "myaccount" \
        --ip-address "16.17.18.19"
    ```

1. Quite una regla de red para un intervalo de direcciones IP.

    ```azurecli-interactive
    az cognitiveservices account network-rule remove \
        -g "myresourcegroup" -n "myaccount" \
        --ip-address "16.17.18.0/24"
    ```

***

> [!IMPORTANT]
> Asegúrese de [establecer la regla predeterminada](#change-the-default-network-access-rule) en **denegar** o, de lo contrario, las reglas de red no tendrán ningún efecto.

## <a name="next-steps"></a>Pasos siguientes

* Exploración de los distintos servicios de [Azure Cognitive Services](welcome.md).
* Más información sobre los [puntos de conexión de servicio de Azure Virtual Network](../virtual-network/virtual-network-service-endpoints-overview.md).