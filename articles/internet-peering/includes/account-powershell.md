---
title: archivo de inclusión
titleSuffix: Azure
description: archivo de inclusión
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 3a5f7157ef8f3645dd03ec93684238dd8bbc067e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773643"
---
Antes de comenzar la configuración, instale e importe los módulos necesarios. Necesitará privilegios de administrador para instalar los módulos en PowerShell.

1. Instalación e importación del módulo Az
    ```powershell
    Install-Module Az -AllowClobber
    Import-Module Az
    ```
1. Instalación e importación del módulo Az.Peering
    ```powershell
    Install-Module -Name Az.Peering -AllowClobber
    Import-Module Az.Peering
    ```
1. Compruebe que los módulos se importan correctamente con el comando siguiente.
    ```powershell
    Get-Module
    ```
1. Inicie sesión en la cuenta de Azure mediante el siguiente comando.
    ```powershell
    Connect-AzAccount
    ```
1. Compruebe las suscripciones de la cuenta y seleccione la suscripción en la que desea crear un emparejamiento.
    ```powershell
    Get-AzSubscription
    Select-AzSubscription -SubscriptionId "subscription-id"
    ```
1. Si todavía no tiene un grupo de recursos, debe crear uno antes de crear un emparejamiento. Para ello, ejecute el siguiente comando:

    ```powershell
    New-AzResourceGroup -Name "PeeringResourceGroup" -Location "Central US"
    ```
> [!IMPORTANT]
> Si aún no ha asociado el ASN y la suscripción, siga los pasos de [asociar ASN del mismo nivel](../howto-subscription-association-powershell.md). Esto es necesario para solicitar un emparejamiento.

> [!NOTE]
> La ubicación del grupo de recursos es independiente de la ubicación en la que decida configurar un emparejamiento.
&nbsp;
