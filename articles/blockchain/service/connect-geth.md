---
title: Uso de Geth para conectarse a Azure Blockchain Service
description: Conexión a una instancia de Geth en un nodo de transacción de Azure Blockchain Service
ms.date: 05/26/2020
ms.topic: quickstart
ms.reviewer: maheshna
ms.openlocfilehash: 2c5b6a56d96ac132949052e9879c8f61d41ffb06
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "83994796"
---
# <a name="quickstart-use-geth-to-attach-to-an-azure-blockchain-service-transaction-node"></a>Inicio rápido: Uso de Geth para conectarse a un nodo de transacción de Azure Blockchain Service

En este inicio rápido, se usa el cliente de Geth para conectarse a una instancia de Geth en un nodo de transacción de Azure Blockchain Service. Una vez conectado, use la consola de Geth para llamar a Ethereum JavaScript API.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

* Instalar [Geth](https://github.com/ethereum/go-ethereum/wiki/geth)
* Realizar el tutorial [Quickstart: Creación de un miembro de cadena de bloques mediante Azure Portal](create-member.md) o [Inicio rápido: Creación de un miembro de cadena de bloques de Azure Blockchain Service mediante la CLI de Azure](create-member-cli.md)

## <a name="get-geth-connection-string"></a>Obtención de una cadena de conexión de Geth

La cadena de conexión de Geth para un nodo de transacción de Azure Blockchain Service se puede obtener en Azure Portal.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Vaya al miembro de Azure Blockchain Service. Seleccione **Nodos de transacción** y el vínculo al nodo de transacción predeterminado.

    ![Selección del nodo de transacción predeterminado](./media/connect-geth/transaction-nodes.png)

1. Seleccione **Cadenas de conexión**.
1. Copie la cadena de conexión de **HTTPS (Access key 1)** (HTPPS [clave de acceso 1]). Necesita la cadena para la siguiente sección.

    ![Cadena de conexión](./media/connect-geth/connection-string.png)

## <a name="connect-to-geth"></a>Conexión a Geth

1. Abra el shell o un símbolo del sistema.
1. Use el subcomando attach de Geth para conectarse a la instancia de Geth en ejecución en el nodo de transacción. Pegue la cadena de conexión como un argumento para el subcomando attach. Por ejemplo:

    ``` bash
    geth attach <connection string>
    ```

1. Una vez conectado a la consola de Ethereum del nodo de transacción, puede llamar a Ethereum JavaScript API.

    Por ejemplo, use la siguiente API para averiguar el valor de chainId.

    ``` bash
    admin.nodeInfo.protocols.istanbul.config.chainId
    ```

    En este ejemplo, es 661.

    ![Opción de Azure Blockchain Service](./media/connect-geth/geth-attach.png)

1. Para desconectarse de la consola, escriba `exit`.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, usó el cliente de Geth para conectarse a una instancia de Geth en un nodo de transacción de Azure Blockchain Service. Pruebe el siguiente tutorial para usar Azure Blockchain Development Kit for Ethereum para crear, compilar, implementar y ejecutar una función de contrato inteligente mediante una transacción.

> [!div class="nextstepaction"]
> [Creación, compilación e implementación de contratos inteligentes en Azure Blockchain Service](send-transaction.md)
