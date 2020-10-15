---
title: Versiones de Azure Blockchain Service para libros de contabilidad, aplicación de revisiones y actualizaciones
description: Información general de las versiones admitidas de los libros de contabilidad en Azure Blockchain Service. Incluidas directivas para las actualizaciones y revisiones del sistema.
ms.date: 06/30/2020
ms.topic: conceptual
ms.reviewer: ravastra
ms.openlocfilehash: ea7c54e229178320329204a3199ab2b4c44058fe
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85807747"
---
# <a name="supported-azure-blockchain-service-ledger-versions"></a>Versiones que Azure Blockchain Service admite para los libros de contabilidad

Azure Blockchain Service usa el libro de contabilidad de [Quorum](https://www.goquorum.com/developers) basado en Ethereum y diseñado para el procesamiento de transacciones privadas dentro de un grupo de participantes conocidos, identificado como un consorcio de Azure Blockchain Service.

Actualmente, Azure Blockchain Service admite [Quorum versión 2.6.0](https://github.com/jpmorganchase/quorum/releases/tag/v2.6.0) y el [administrador de transacciones Tessera](https://github.com/jpmorganchase/tessera).

## <a name="managing-updates-and-upgrades"></a>Administración de actualizaciones

El control de versiones en Quorum se realiza con las versiones principal, secundaria y de revisión. Por ejemplo, si la versión de Quorum es la 2.0.1, el tipo de versión se clasificaría como sigue:

|Principal | Minor  | Revisión  |
| :--- | :----- | :----- |
| 2 | 0 | 1 | 

Azure Blockchain Service actualiza automáticamente las versiones de las revisiones de Quorum para los miembros existentes que se ejecutan dentro de los 30 días posteriores a que estén disponibles desde Quorum.

## <a name="availability-of-new-ledger-versions"></a>Disponibilidad de nuevas versiones del libro de contabilidad

Azure Blockchain Service proporciona las versiones principales y secundarias más recientes del libro de contabilidad de Quorum 60 días después de que el fabricante de Quorum las publique. Se proporcionan cuatro versiones secundarias, como máximo, para que los consorcios elijan cuándo aprovisionar un nuevo miembro y un consorcio. La actualización a una versión principal o secundaria no se admite actualmente. Por ejemplo, si va a ejecutar la versión 2.x, actualmente no se admite la actualización a la versión 3.x. Por ejemplo, si va a ejecutar la versión 2.2, actualmente no se admite la actualización a la versión 2.3.

## <a name="how-to-check-quorum-ledger-version"></a>Cómo comprobar la versión del libro de contabilidad de Quorum

Puede comprobar la versión de Quorum en el miembro de Azure Blockchain Service; para ello, conéctese a su nodo con geth o consulte los registros de diagnóstico.

### <a name="using-geth"></a>Uso de geth

Conéctese al miembro de Azure Blockchain Service mediante geth. Por ejemplo, `geth attach https://myblockchainmember.blockchain.azure.com:3200/<Access key>`.

Una vez que el nodo esté conectado, geth notifica la versión de Quorum de forma similar a la siguiente salida:

``` text
instance: Geth/v1.9.7-stable-9339be03(quorum-v2.6.0)/linux-amd64/go1.13.12
```

Para obtener más información sobre el uso de geth, consulte [Inicio rápido: Uso de Geth para conectarse a un nodo de transacción de Azure Blockchain Service](connect-geth.md).

### <a name="using-diagnostic-logs"></a>Uso de registros de diagnóstico

Si habilita los registros de diagnóstico, la versión de Quorum se mostrará para los nodos de transacción. Por ejemplo, el siguiente mensaje de registro informativo del nodo incluye la versión de Quorum.

``` text 
{"NodeName":"transaction-node","Message":"INFO [06-22|05:31:45.156] Starting peer-to-peer node instance=Geth/v1.9.7-stable-9339be03(quorum-v2.6.0)/linux-amd64/go1.13.12\n"}
{"NodeName":"transaction-node","Message":"[*] Starting Quorum node with QUORUM_VERSION=2.6.0, TESSERA_VERSION=0.10.5 and PRIVATE_CONFIG=/working-dir/c/tm.ipc\n"}
111
```

Para obtener más información sobre los registros de diagnóstico, consulte [Supervisión de Azure Blockchain Service a través de Azure Monitor](monitor-azure-blockchain-service.md#diagnostic-settings).

## <a name="how-to-check-genesis-file-content"></a>Comprobación del contenido del archivo genesis

Para comprobar el contenido del archivo genesis del nodo de Blockchain, puede usar la siguiente API de JavaScript de Ethereum:

``` bash
admin.nodeInfo.protocols
```
Puede llamar a la API mediante una consola de geth o una biblioteca de web3. Para obtener más información sobre el uso de geth, consulte [Inicio rápido: Uso de Geth para conectarse a un nodo de transacción de Azure Blockchain Service](connect-geth.md).

## <a name="next-steps"></a>Pasos siguientes

[Límites en Azure Blockchain Service](limits.md)
