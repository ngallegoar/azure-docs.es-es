---
title: Habilitación de TLS segura con .NET
titleSuffix: Azure Storage
description: Obtenga información sobre cómo habilitar TLS 1.2 mediante la biblioteca cliente de .NET para Azure Storage.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/25/2018
ms.author: tamram
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: 81c9a8fe9513f1f8fc65ad64b34f0fb04383569b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75371809"
---
# <a name="enable-secure-tls-for-azure-storage-client"></a>Habilitación de la conexión TLS segura para el cliente de Azure Storage

Seguridad de la capa de transporte (TLS) y Capa de sockets seguros (SSL) son protocolos criptográficos que proporcionan la seguridad de las comunicaciones a través de una red de equipos. Se ha detectado que SSL 1.0, 2.0 y 3.0 son vulnerables. RFC los ha prohibido. TLS 1.0 no es seguro para el uso del cifrado de bloques (DES CBC y RC2 CBC) y del cifrado de flujos (RC4). El consejo de PCI ha sugerido la migración a versiones superiores de TLS. Para más información, puede consultar [Seguridad de la capa de transporte (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0).

Azure Storage ha dejado de usar SSL 3.0 desde 2015 y usa TLS 1.2 en puntos de conexión HTTP públicos. Sin embargo, TLS 1.0 y TLS 1.1 se siguen admitiendo gracias a la compatibilidad con versiones anteriores.

Para garantizar una conexión segura y compatible con Azure Storage, debe habilitar TLS 1.2 o una versión más reciente en el cliente antes de enviar solicitudes para trabajar con el servicio de Azure Storage.

## <a name="enable-tls-12-in-net-client"></a>Habilitación de TLS 1.2 en el cliente de .NET

Para que el cliente pueda negociar TLS 1.2, la versión del sistema operativo y la de .NET Framework tienen que ser compatibles con TLS 1.2. Conozca más información en [Compatibilidad con TLS 1.2](https://docs.microsoft.com/dotnet/framework/network-programming/tls#support-for-tls-12).

El ejemplo siguiente muestra cómo habilitar TLS 1.2 en el cliente de .NET.

```csharp

    static void EnableTls12()
    {
        // Enable TLS 1.2 before connecting to Azure Storage
        System.Net.ServicePointManager.SecurityProtocol = System.Net.SecurityProtocolType.Tls12;

        // Connect to Azure Storage
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName={yourstorageaccount};AccountKey={yourstorageaccountkey};EndpointSuffix=core.windows.net");
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        CloudBlobContainer container = blobClient.GetContainerReference("foo");
        container.CreateIfNotExists();
    }

```

## <a name="enable-tls-12-in-powershell-client"></a>Habilitación de TLS 1.2 en el cliente de PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)] 

El ejemplo siguiente muestra cómo habilitar TLS 1.2 en el cliente de PowerShell.

```powershell
# Enable TLS 1.2 before connecting to Azure Storage
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12;

$resourceGroup = "{YourResourceGroupName}"
$storageAccountName = "{YourStorageAccountNme}"
$prefix = "foo"

# Connect to Azure Storage
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroup -Name $storageAccountName
$ctx = $storageAccount.Context
$listOfContainers = Get-AzStorageContainer -Context $ctx -Prefix $prefix
$listOfContainers
```

## <a name="verify-tls-12-connection"></a>Comprobación de la conexión de TLS 1.2

Puede utilizar Fiddler para comprobar si se está utilizando TLS 1.2 realmente. Abra Fiddler para empezar a capturar el tráfico del cliente y, a continuación, ejecute el ejemplo anterior. Después, puede encontrar la versión TLS en la conexión que realiza el ejemplo.

La captura de pantalla siguiente es un ejemplo de la comprobación.

![captura de pantalla de comprobación de la versión de TLS en Fiddler](./media/storage-security-tls/storage-security-tls-verify-in-fiddler.png)

## <a name="see-also"></a>Consulte también

* [Seguridad de la capa de transporte (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0)
* [Cumplimiento del estándar PCI en TLS](https://blog.pcisecuritystandards.org/migrating-from-ssl-and-early-tls)
* [Habilitar TLS en el cliente de Java](https://www.java.com/en/configure_crypto.html)
