---
title: Introducción al desarrollo de Azure Blockchain Service
description: Introducción sobre el desarrollo de soluciones en Azure Blockchain Service.
ms.date: 03/26/2020
ms.topic: conceptual
ms.reviewer: janders
ms.openlocfilehash: 4a98e5ea025894303bc5c77bba0c6154a08315f2
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "80348380"
---
# <a name="azure-blockchain-service-development-overview"></a>Introducción al desarrollo de Azure Blockchain Service

Con Azure Blockchain Service, puede crear redes de cadenas de bloques de consorcio para habilitar escenarios empresariales, como seguimiento de recursos, token digital, fidelidad y recompensa, cadena de suministro financiero y procedencia. En las secciones siguientes se presentan el desarrollo de Azure Blockchain Service para implementar soluciones de cadena de bloques de empresa.

## <a name="connecting-to-azure-blockchain-service"></a>Conexión a Azure Blockchain Service

Hay distintos tipos de clientes de redes de cadenas de bloques, incluidos nodos completos, nodos de luz y clientes remotos. Azure Blockchain Service compila una red de cadenas de bloques que incluye nodos. Puede usar distintos clientes como puerta de enlace a Azure Blockchain Service para el desarrollo de cadenas de bloques. Azure Blockchain Service ofrece autenticación básica o la clave de acceso como punto de conexión de desarrollo. A continuación, se indican algunos clientes populares que puede usar para conectarse.

### <a name="visual-studio-code"></a>Visual Studio Code

Puede conectarse a los miembros de un consorcio mediante la extensión para Visual Studio Code del kit de desarrollo de Azure Blockchain. Una vez conectado a un consorcio, puede crear, compilar e implementar contratos inteligentes en un miembro del consorcio de Azure Blockchain Service.

Para desarrollar soluciones empresariales sofisticadas de cadenas de bloques, es necesario un marco de desarrollo para conectarse a diferentes redes de cadenas de bloques y administrar el ciclo de vida del contrato inteligente. La mayoría de los proyectos interactúan con al menos dos nodos de la cadena de bloques. Los desarrolladores usan una cadena de bloques local durante el desarrollo. Cuando la aplicación está lista para la prueba o lanzamiento, el desarrollador se implementa en una red de cadena de bloques. Por ejemplo, la principal red de Ethereum pública o Azure Blockchain Service. El kit de desarrollo de Azure Blockchain para la extensión de Ethereum en Visual Studio Code usa Truffle. Truffle es uno de los marcos de desarrollo de cadenas de bloques más populares para escribir, compilar, implementar y probar aplicaciones descentralizadas en las cadenas de bloques de Ethereum. Truffle también se puede considerar como un marco que intenta integrar sin problemas el desarrollo de contratos inteligentes y el desarrollo web tradicional.

Para más información, consulte [Inicio rápido: Uso de Visual Studio Code para conectarse a una red del consorcio de Azure Blockchain Service](connect-vscode.md).

### <a name="metamask"></a>MetaMask

MetaMask es una cartera basada en explorador (cliente remoto), cliente RPC y explorador de contrato básico. A diferencia de otras carteras de explorador, MetaMask inserta una instancia web3 en el contexto de JavaScript del explorador, actuando como un cliente RPC que se conecta a una variedad de cadenas de bloques de Ethereum (*mainnet*, *Ropsten testnet*, *Kovan testnet*, nodo de RPC local, etc.). Puede configurar una RPC personalizada fácilmente para conectarse a Azure Blockchain Service e iniciar el desarrollo de la cadena de bloques mediante Remix.

Para más información, consulte [Inicio rápido: Uso de MetaMask para conectarse a un contrato inteligente e implementarlo](connect-metamask.md)

### <a name="geth"></a>Geth

Geth es la interfaz de la línea de comandos para ejecutar un nodo de Ethereum completo implementado en Go. No es necesario ejecutar el nodo completo, pero puede iniciar su consola interactiva que proporciona un entorno en tiempo de ejecución de JavaScript que expone una API de JavaScript para interactuar con Azure Blockchain Service.

Para más información, consulte [Inicio rápido: Uso de Geth para conectarse a un nodo de transacción de Azure Blockchain Service](connect-geth.md).

## <a name="ethereum-quorum-private-transactions"></a>Transacciones privadas de Quorum de Ethereum

Quorum es un protocolo de libro de contabilidad distribuida basado en Ethereum con transacción, además de privacidad de contrato y nuevos mecanismos de consenso. Las mejoras claves realizadas en Go-Ethereum incluyen:

* **Privacidad**: Quorum admite transacciones privadas y contratos privados mediante la separación del estado público y privado y usa los intercambios de mensajes cifrados punto a punto para transferencias dirigidas de datos privados para los participantes de la red.
* **Otros mecanismos de consenso**: sin necesidad de consenso de prueba de trabajo o de prueba de participación en una red con permisos. Quorum ofrece varios mecanismos de consenso que están diseñados para cadenas de consorcio, como RAFT e IBFT.  Azure Blockchain Service usa el mecanismo de consenso IBFT.
* **Permisos del mismo nivel**: los permisos de nodo y del mismo nivel que usan contratos inteligentes garantizan que solo puedan unirse a la red entidades conocidas.
* **Mayor rendimiento**: Quorum ofrece un mayor rendimiento respecto a la interfaz Geth pública.

## <a name="block-explorers"></a>Exploradores de bloques

Los exploradores de bloques son los exploradores de cadenas de bloques en línea que muestran el contenido de bloques individuales, datos de direcciones de transacción y el historial. La información básica de bloques está disponible a través de Azure Monitor en Azure Blockchain Service. Sin embargo, si necesita información más detallada durante el desarrollo, los exploradores de bloques pueden ser útiles.  Los siguientes exploradores de bloques funcionan con Azure Blockchain Service:

* [Epirus Azure Blockchain Service Explorer](https://azuremarketplace.microsoft.com/marketplace/apps/blk-technologies.azure-blockchain-explorer-template?tab=Overview) de Web3 Labs
* [BlockScout](https://github.com/Azure-Samples/blockchain/blob/master/ledger/template/ethereum-on-azure/technology-samples/blockscout/README.md)

También puede crear su propio explorador de bloques con Blockchain Data Manager y Azure Cosmos DB. Consulte [Tutorial: Uso de Blockchain Data Manager para enviar datos a Azure Cosmos DB](data-manager-cosmosdb.md).

## <a name="tps-measurement"></a>Medición de TPS

Como las cadenas de bloques se usan en más escenarios empresariales, la velocidad de las transacciones por segundo (TPS) es importante para evitar cuellos de botella y deficiencias de sistema. Las altas tasas de transacción pueden ser difíciles de mantener dentro de una cadena de bloques descentralizada. Una medida precisa de TPS puede verse afectada por distintos factores, como el subproceso del servidor, el tamaño de la cola de transacciones, la latencia de red y la seguridad. Si necesita medir la velocidad de TPS durante el desarrollo, una herramienta de código abierto popular es [ChainHammer](https://github.com/drandreaskrueger/chainhammer).

## <a name="next-steps"></a>Pasos siguientes

Pruebe un inicio rápido que usa Azure Blockchain Development Kit for Ethereum para realizar la asociación con un consorcio en Azure Blockchain Service.

> [!div class="nextstepaction"]
> [Uso de Visual Studio Code para conectarse a Azure Blockchain Service](connect-vscode.md)