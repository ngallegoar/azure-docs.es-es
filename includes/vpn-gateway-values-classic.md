---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/08/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 11d2172d085fe9b47587f4084908f99d7b54437e
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92103244"
---
Cuando crea redes virtuales clásicas en Azure Portal, el nombre que ve no es el nombre completo que usa para PowerShell. Por ejemplo, una red virtual que pareciera tener el nombre **TestVNet1** en el portal podría tener un nombre mucho más largo en el archivo de configuración de red. En el caso de una red virtual del grupo de recursos "ClassicRG" el nombre sería algo así: **Group ClassicRG TestVNet1** . Cuando cree conexiones, es importante usar los valores que ve en el archivo de configuración de red.

En los pasos siguientes, se conectará a la cuenta de Azure y descargará y verá el archivo de configuración de red para obtener los valores requeridos para las conexiones.

1. Descargue e instale la versión más reciente de los cmdlets de PowerShell para Azure Service Management (SM). La mayoría de los usuarios tienen los módulos de Resource Manager instalados localmente, pero no tienen módulos para la administración de servicios. Los módulos de administración de servicios son heredados y deben instalarse por separado. Para obtener más información, consulte la [Instalación de los cmdlets de administración de servicios](/powershell/azure/servicemanagement/install-azure-ps).

1. Abra la consola de PowerShell con privilegios elevados y conéctela a su cuenta. Use los siguientes ejemplos para conectarse. Estos comandos se deben ejecutar localmente mediante el módulo de administración de servicios de PowerShell. Conéctese a su cuenta. Use el siguiente ejemplo para conectarse:

   ```powershell
   Add-AzureAccount
   ```
1. Compruebe las suscripciones para la cuenta.

   ```powershell
   Get-AzureSubscription
   ```
1. Si tiene varias suscripciones, seleccione la que quiera usar.

   ```powershell
   Select-AzureSubscription -SubscriptionId "Replace_with_your_subscription_ID"
   ```
1. Cree un directorio en el equipo. Por ejemplo, C:\AzureVNet
1. Exporte el archivo de configuración de red al directorio. En este ejemplo, se exporta el archivo de configuración de red a **C:\AzureNet** .

   ```powershell
   Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
   ```
1. Abra el archivo con un editor de texto y consulte los nombres de las redes virtuales y los sitios. Estos serán los nombres que usará cuando cree las conexiones.<br>Los nombres de las **redes virtuales** aparecen como **VirtualNetworkSite name =**<br>Los nombres de los **sitios** aparecen como **LocalNetworkSiteRef name =**