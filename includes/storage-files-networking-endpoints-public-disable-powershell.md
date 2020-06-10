---
title: Archivo de inclusión
description: archivo de inclusión
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 6/2/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: a0a9bc29c3e20a025fb2c46a71c2f134c37bee04
ms.sourcegitcommit: 813f7126ed140a0dff7658553a80b266249d302f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2020
ms.locfileid: "84465113"
---
El siguiente comando de PowerShell denegará todo el tráfico que llega al punto de conexión público de la cuenta de almacenamiento. Tenga en cuenta que este comando tiene el parámetro `-Bypass` establecido en `AzureServices`. De esta forma, los servicios propios de confianza, como Azure File Sync, acceden a la cuenta de almacenamiento mediante el punto de conexión público.

```PowerShell
# This assumes $storageAccount is still defined from the beginning of this of this guide.
$storageAccount | Update-AzStorageAccountNetworkRuleSet `
        -DefaultAction Deny `
        -Bypass AzureServices `
        -WarningAction SilentlyContinue `
        -ErrorAction Stop | `
    Out-Null
```