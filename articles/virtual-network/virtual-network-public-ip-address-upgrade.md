---
title: Actualización de direcciones IP públicas
titleSuffix: Azure Virtual Network
description: Actualice las direcciones IP públicas del nivel básico al nivel estándar.
services: virtual-network
documentationcenter: na
author: blehr
editor: ''
ms.assetid: bb71abaf-b2d9-4147-b607-38067a10caf6
ms.service: virtual-network
ms.subservice: ip-services
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/07/2020
ms.author: blehr
ms.custom: references_regions , devx-track-azurecli
ms.openlocfilehash: 0c248149694c2bf66b8c94e9c0a29a8f7da9f4e4
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94843747"
---
# <a name="upgrade-public-ip-addresses"></a>Actualización de direcciones IP públicas

Las direcciones IP públicas de Azure se crean con una SKU (de nivel básico o estándar) que determina los aspectos de su funcionalidad (como el método de asignación, el uso entre zonas de disponibilidad y los recursos a los que se pueden asociar). 

En este artículo se revisan los siguientes escenarios:
* Actualización de una dirección IP pública con una SKU de nivel básico a estándar (mediante el portal, PowerShell o la CLI)
* Migración de una dirección IP reservada de Azure clásico a una dirección IP pública con una SKU de nivel básico de Azure Resource Manager

## <a name="upgrade-public-ip-address-from-basic-to-standard-sku"></a>Actualización de la dirección IP pública con una SKU de nivel básico a estándar

>[!NOTE]
>La posibilidad de actualizar direcciones IP públicas de nivel básico a estándar no está disponible en todas las regiones.  Consulte [**Limitaciones**](#limitations) para más información.

Para actualizar una dirección IP pública, no debe estar asociada a ningún recurso (consulte [esta página](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address#view-modify-settings-for-or-delete-a-public-ip-address) para más información sobre cómo desasociar direcciones IP públicas).

>[!IMPORTANT]
>Las direcciones IP públicas actualizadas con una SKU de nivel básico a estándar continúan sin tener [zonas de disponibilidad](https://docs.microsoft.com/azure/availability-zones/az-overview?toc=/azure/virtual-network/toc.json#availability-zones).  Esto significa que no se pueden asociar a un recurso de Azure que tenga redundancia de zona o que esté vinculado a una zona especificada previamente en regiones en las que se ofrezca esta posibilidad.

---
# <a name="basic-to-standard---powershell"></a>[**Básico a estándar: PowerShell**](#tab/option-upgrade-powershell)

En el ejemplo siguiente se da por supuesto que se ha creado previamente una dirección IP pública con una SKU de nivel básico mediante el ejemplo proporcionado en [esta página](https://docs.microsoft.com/azure/virtual-network/create-public-ip-powershell?tabs=option-create-public-ip-basic) con una dirección IP pública básica **myBasicPublicIP** en **myResourceGroup**.

Para actualizar la dirección IP, simplemente ejecute los comandos siguientes con PowerShell.  Tenga en cuenta que, si la dirección IP ya está asignada estáticamente, se puede omitir esa sección.

```azurepowershell-interactive
## Variables for the command ##
$rg = 'myResourceGroup'
$name = 'myBasicPublicIP'
$newsku = 'Standard'
$pubIP = Get-AzPublicIpAddress -name $name -ResourceGroupName $rg

## This section is only needed if the Basic IP is not already set to Static ##
$pubIP.PublicIpAllocationMethod = 'Static'
Set-AzPublicIpAddress -PublicIpAddress $pubIP

## This section is for conversion to Standard ##
$pubIP.Sku.Name = $newsku
Set-AzPublicIpAddress -PublicIpAddress $pubIP
```

# <a name="basic-to-standard---cli"></a>[**Básica a estándar: CLI**](#tab/option-upgrade-cli)

En el ejemplo siguiente se da por supuesto que se ha creado previamente una dirección IP pública con una SKU de nivel básico mediante el ejemplo proporcionado en [esta página](https://docs.microsoft.com/azure/virtual-network/create-public-ip-cli?tabs=option-create-public-ip-basic) con una dirección IP pública básica **myBasicPublicIP** en **myResourceGroup**.

Para actualizar la dirección IP, simplemente ejecute los comandos siguientes mediante la CLI de Azure.  Tenga en cuenta que, si la dirección IP ya está asignada estáticamente, se puede omitir esa sección.

```azurecli-interactive
## Variables for the command ##
$rg = 'myResourceGroup'
$name = 'myBasicPublicIP'

## This section is only needed if the Basic IP is not already set to Static ##
az network public-ip update \
--resource-group $rg \
--name $name \
--allocation-method Static 

## This section is for conversion to Standard ##
az network public-ip update \
--resource-group $rg \
--name $name \
--sku Standard
```
---

## <a name="upgrade-migrate-a-classic-reserved-ip-to-a-static-public-ip"></a>Actualización (migración) de una dirección IP reservada clásica a una dirección IP pública estática

Para beneficiarse de las nuevas funcionalidades de Azure Resource Manager, puede migrar una dirección IP estática pública existente, llamada IP reservada, del modelo clásico al modelo moderno de Azure Resource Manager.  La dirección IP pública migrada será un tipo de SKU de nivel básico.


---

# <a name="reserved-to-basic---powershell"></a>[**Reservada a básica: PowerShell**](#tab/option-migrate-powershell)

En el ejemplo siguiente se da por supuesto que se ha creado previamente una IP reservada de Azure clásico **myReservedIP** en **myResourceGroup**. Otro requisito previo para la migración es asegurarse de que la suscripción de Azure Resource Manager se ha registrado. Este aspecto se trata de forma detallada en los pasos 3 y 4 de [esta página](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-ps).

Para migrar la IP reservada, ejecute los siguientes comandos con PowerShell.  Tenga en cuenta que, si la dirección IP no está asociada a ningún servicio (a continuación, se muestra el servicio denominado **myService**), ese paso se puede omitir.

```azurepowershell-interactive
## Variables for the command ##
$name = 'myReservedIP'

## This section is only needed if the Reserved IP is not already disassociated from any Cloud Services ##
$serviceName = 'myService'
Remove-AzureReservedIPAssociation -ReservedIPName $name -ServiceName $service

$validate = Move-AzureReservedIP -ReservedIPName $name -Validate
$validate.ValidationMessages
```
El comando anterior muestra las advertencias y errores que bloqueen la migración. Si la validación es correcta, puede continuar con los pasos siguientes para preparar y confirmar la migración:
```azurepowershell-interactive
Move-AzureReservedIP -ReservedIPName $name -Prepare
Move-AzureReservedIP -ReservedIPName $name -Commit
```
Se crea un grupo de recursos en Azure Resource Manager con el nombre de la dirección IP reservada migrada (en el ejemplo anterior, sería el grupo de recursos **myReservedIP-Migrated**).

# <a name="reserved-to-basic---cli"></a>[**Reservada a básica: CLI**](#tab/option-migrate-cli)

En el ejemplo siguiente se da por supuesto que se ha creado previamente una IP reservada de Azure clásico **myReservedIP** en **myResourceGroup**. Otro requisito previo para la migración es asegurarse de que la suscripción de Azure Resource Manager se ha registrado. Este aspecto se trata de forma detallada en los pasos 3 y 4 de [esta página](https://docs.microsoft.com/azure/virtual-machines/linux/migration-classic-resource-manager-cli).

Para migrar la dirección IP reservada, ejecute los siguientes comandos con la CLI de Azure.  Tenga en cuenta que, si la dirección IP no está asociada a ningún servicio (a continuación, se muestra el servicio denominado **myService** y la implementación **myDeployment**), ese paso se puede omitir.

```azurecli-interactive
## Variables for the command ##
$name = 'myReservedIP'

## This section is only needed if the Reserved IP is not already disassociated from any Cloud Services ##
$service = 'myService'
$deployment = 'myDeployment'
azure network reserved-ip disassociate $name $service $deployment

azure network reserved-ip validate-migration $name
```
El comando anterior muestra las advertencias y errores que bloqueen la migración. Si la validación es correcta, puede continuar con los pasos siguientes para preparar y confirmar la migración:
```azurecli-interactive
azure network reserved-ip prepare-migration $name
azure network reserved-ip commit-migration $name
```
Se crea un grupo de recursos en Azure Resource Manager con el nombre de la dirección IP reservada migrada (en el ejemplo anterior, sería el grupo de recursos **myReservedIP-Migrated**).

---

## <a name="limitations"></a>Limitaciones

* Actualmente, esta funcionalidad no está disponible en las siguientes regiones:<br>
US Gov - Virginia<br>
US DoD (este)<br>
US DoD (centro)<br>
Este de China<br>
Este de China 2<br>
Norte de China<br>
Norte de China 2

* Para actualizar una dirección IP pública básica, no se puede asociar con ningún recurso de Azure.  Examine [esta página](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address#view-modify-settings-for-or-delete-a-public-ip-address) para más información sobre cómo desasociar las direcciones IP públicas.  Del mismo modo, para migrar una dirección IP reservada, no se puede asociar con ningún servicio en la nube.  Revise [esta página](https://docs.microsoft.com/azure/virtual-network/remove-public-ip-address-vm) para más información sobre cómo desasociar direcciones IP reservadas.  
* Las direcciones IP públicas actualizadas de la SKU de nivel básico a estándar seguirán sin tener [zonas de disponibilidad](https://docs.microsoft.com/azure/availability-zones/az-overview?toc=/azure/virtual-network/toc.json#availability-zones) y, por lo tanto, no se pueden asociar a un recurso de Azure con redundancia de zona o zonal.  Tenga en cuenta que esto solo se aplica a las regiones que ofrecen zonas de disponibilidad.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre las [direcciones IP públicas](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) en Azure, incluida la diferencia entre los tipos de SKU, así como la [configuración de la dirección IP pública](virtual-network-public-ip-address.md#create-a-public-ip-address).
- Más información sobre cómo [actualizar los equilibradores de carga públicos de Azure de nivel básico a estándar](https://docs.microsoft.com/azure/load-balancer/upgrade-basic-standard).
- Aprenda sobre las [direcciones IP reservadas de Azure clásico](https://docs.microsoft.com/previous-versions/azure/virtual-network/virtual-networks-reserved-public-ip) y la [migración de recursos clásicos a Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview).
