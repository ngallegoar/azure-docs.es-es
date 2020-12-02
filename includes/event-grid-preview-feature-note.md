---
title: archivo de inclusión
description: archivo de inclusión
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 11/06/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 644669ea27938e385e11f3b1911a23ab30829a95
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96027840"
---
Esta característica se encuentra en su versión preliminar. Para usarlo, debe instalar una extensión o módulo de la versión preliminar.

### <a name="install-extension-for-azure-cli"></a>Instalación de la extensión para la CLI de Azure

Para la CLI de Azure, necesita la [extensión de Event Grid](/cli/azure/azure-cli-extensions-list).

En [CloudShell](../articles/cloud-shell/quickstart.md):

* Si ha instalado la extensión anteriormente, actualícela `az extension update -n eventgrid`
* Si no ha instalado la extensión anteriormente, instálela `az extension add -n eventgrid`

Para una instalación local:

1. [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). Asegúrese de que tiene la última versión. Para ello, debe comprobarlo con `az --version`.
1. Desinstale las versiones anteriores de la extensión `az extension remove -n eventgrid`.
1. Instale la extensión `eventgrid` con `az extension add -n eventgrid`

### <a name="install-module-for-powershell"></a>Instalación del módulo para PowerShell

Para PowerShell, necesita el [módulo AzureRM.EventGrid](https://www.powershellgallery.com/packages/AzureRM.EventGrid/0.4.1-preview).

En [CloudShell](../articles/cloud-shell/quickstart-powershell.md):

* Instale el módulo `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`.

Para una instalación local:

1. Abra una consola de PowerShell como administrador
1. Instale el módulo `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`.

Si el parámetro `-AllowPrerelease` no está disponible, siga estos pasos:

1. Ejecute `Install-Module PowerShellGet -Force`:
1. Ejecute `Update-Module PowerShellGet`:
1. Cierre la consola de PowerShell.
1. Reinicie PowerShell como administrador.
1. Instale el módulo `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`.