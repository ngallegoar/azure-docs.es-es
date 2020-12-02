---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 07/08/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: b26d4af29a92fb0f14c52e76a7eae1d0073a3aa0
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "96005348"
---
Este error se puede producir cuando el servicio Azure File Sync no es accesible desde el servidor. Puede solucionar este error siguiendo estos pasos:

1. Compruebe que el servicio de Windows `FileSyncSvc.exe` no está bloqueado por el firewall.
2. Compruebe que el puerto 443 está abierto para las conexiones salientes al servicio Azure File Sync. Para ello, puede usar el cmdlet `Test-NetConnection`. La dirección URL para el marcador de posición `<azure-file-sync-endpoint>` a continuación se puede encontrar en el documento [Configuración del proxy y el firewall de Azure File Sync](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#firewall). 

    ```powershell
    Test-NetConnection -ComputerName <azure-file-sync-endpoint> -Port 443
    ```

3. Asegúrese de establecer la configuración del proxy como estaba previsto. Esto puede hacerse con el cmdlet `Get-StorageSyncProxyConfiguration`. Puede encontrar más información sobre cómo configurar la configuración del proxy de Azure File Sync en [Configuración del proxy y el firewall de Azure File Sync](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#firewall).

    ```powershell
    $agentPath = "C:\Program Files\Azure\StorageSyncAgent"
    Import-Module "$agentPath\StorageSync.Management.ServerCmdlets.dll"
    Get-StorageSyncProxyConfiguration
    ```
4. Use el cmdlet Test-StorageSyncNetworkConnectivity para comprobar la conectividad de red con los puntos de conexión de servicio. Para más información, vea [Prueba de la conectividad de red con los puntos de conexión de servicio](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#test-network-connectivity-to-service-endpoints).    

5. Póngase en contacto con el administrador de red si necesita más ayuda para resolver problemas de conectividad de red.