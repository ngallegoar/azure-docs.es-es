---
title: Uso de mensajes para la integración con Azure Blockchain Workbench
description: Introducción al uso de mensajes para integrar la versión preliminar de Azure Blockchain Workbench con otros sistemas.
ms.date: 09/05/2019
ms.topic: article
ms.reviewer: brendal
ms.openlocfilehash: 14bd0f84bc9490d95d3dbe0b9f122882f0d2059d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74324508"
---
# <a name="azure-blockchain-workbench-messaging-integration"></a>Integración de los mensajes de Azure Blockchain Workbench

Además de proporcionar una API REST, Azure Blockchain Workbench proporciona integración basada en mensajería. Workbench publica eventos centrados en el libro de contabilidad a través de Azure Event Grid, lo que permite a los consumidores de nivel final ingerir datos o realizar acciones en función de estos eventos. Para aquellos clientes que precisan de mensajería de confianza, Azure Blockchain Workbench también entrega mensajes a un punto de conexión de Azure Service Bus.

## <a name="input-apis"></a>API de entrada

Si quiere iniciar las transacciones desde sistemas externos para crear usuarios, crear contratos y actualizar contratos, puede usar las API de entrada de mensajería para realizar transacciones en un libro de contabilidad. Consulte los [ejemplos de integración de mensajería](https://aka.ms/blockchain-workbench-integration-sample) para obtener ejemplos que muestren las API de entrada.

Las siguientes son las API de entrada actualmente disponibles.

### <a name="create-user"></a>Crear usuario

Crea un nuevo usuario.

La solicitud requiere los siguientes campos:

| **Nombre**             | **Descripción**                                      |
|----------------------|------------------------------------------------------|
| requestId            | GUID del cliente proporcionado                                |
| firstName            | Nombre del usuario                              |
| lastName             | Apellidos del usuario                               |
| emailAddress         | Dirección de correo electrónico del usuario                           |
| externalId           | Identificador del objeto de Azure AD del usuario                      |
| connectionId         | Identificador único de la conexión de cadena de bloques |
| messageSchemaVersion | Versión del esquema de mensajería                            |
| messageName          | **CreateUserRequest**                               |

Ejemplo:

``` json
{
    "requestId": "e2264523-6147-41fc-bbbb-edba8e44562d",
    "firstName": "Ali",
    "lastName": "Alio",
    "emailAddress": "aa@contoso.com",
    "externalId": "6a9b7f65-ffff-442f-b3b8-58a35abd1bcd",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateUserRequest"
}
```

Blockchain Workbench devuelve una respuesta con los siguientes campos:

| **Nombre**              | **Descripción**                                                                                                             |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------|
| requestId             | GUID del cliente proporcionado |
| userId                | Identificador del usuario que se ha creado |
| userChainIdentifier   | Dirección del usuario que se ha creado en la red de la cadena de bloques. En Ethereum, es la dirección del usuario **en la cadena**. |
| connectionId          | Identificador único de la conexión de cadena de bloques|
| messageSchemaVersion  | Versión del esquema de mensajería |
| messageName           | **CreateUserUpdate** |
| status                | Estado de la solicitud de creación de usuario.  Si se crea correctamente, el valor es **Success**. En caso de error, el valor es **Failure**.     |
| additionalInformation | Proporciona información adicional en función del estado. |

Ejemplo de una respuesta correcta de **creación de usuario** desde Blockchain Workbench:

``` json
{ 
    "requestId": "e2264523-6147-41fc-bb59-edba8e44562d", 
    "userId": 15, 
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398", 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateUserUpdate", 
    "status": "Success", 
    "additionalInformation": { } 
} 
```

Si la solicitud no se realizó correctamente, se incluyen detalles sobre el error en la información adicional.

``` json
{
    "requestId": "e2264523-6147-41fc-bb59-edba8e44562d", 
    "userId": 15, 
    "userChainIdentifier": null, 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateUserUpdate", 
    "status": "Failure", 
    "additionalInformation": { 
        "errorCode": 4000, 
        "errorMessage": "User cannot be provisioned on connection." 
    }
}
```

### <a name="create-contract"></a>Crear contrato

Crea un nuevo contrato.

La solicitud requiere los siguientes campos:

| **Nombre**             | **Descripción**                                                                                                           |
|----------------------|---------------------------------------------------------------------------------------------------------------------------|
| requestId            | GUID del cliente proporcionado |
| userChainIdentifier  | Dirección del usuario que se ha creado en la red de la cadena de bloques. En Ethereum, es la dirección del usuario **en la cadena**. |
| applicationName      | Nombre de la aplicación |
| version              | Versión del tipo de aplicación. Requerido si tiene varias versiones de la aplicación habilitada. En caso contrario, la versión es opcional. Para obtener más información sobre las versiones de las aplicaciones, consulte [Control de versiones de una aplicación de Azure Blockchain Workbench](version-app.md). |
| workflowName         | Nombre del flujo de trabajo |
| parámetros           | Entrada de parámetros para la creación de contratos |
| connectionId         | Identificador único de la conexión de cadena de bloques |
| messageSchemaVersion | Versión del esquema de mensajería |
| messageName          | **CreateContractRequest** |

Ejemplo:

``` json
{ 
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211", 
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398", 
    "applicationName": "AssetTransfer",
    "version": "1.0",
    "workflowName": "AssetTransfer", 
    "parameters": [ 
        { 
            "name": "description", 
            "value": "a 1969 dodge charger" 
        }, 
        { 
            "name": "price", 
            "value": "12345" 
        } 
    ], 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateContractRequest" 
}
```

Blockchain Workbench devuelve una respuesta con los siguientes campos:

| **Nombre**                 | **Descripción**                                                                   |
|--------------------------|-----------------------------------------------------------------------------------|
| requestId                | GUID del cliente proporcionado                                                             |
| contractId               | Identificador único del contrato en Azure Blockchain Workbench |
| contractLedgerIdentifier | Dirección del contrato en el libro de contabilidad                                            |
| connectionId             | Identificador único de la conexión de cadena de bloques                               |
| messageSchemaVersion     | Versión del esquema de mensajería                                                         |
| messageName              | **CreateContractUpdate**                                                      |
| status                   | Estado de la solicitud de creación de contrato.  Valores posibles: **Enviado**, **Confirmado** y **Error**.  |
| additionalInformation    | Proporciona información adicional en función del estado.                              |

Ejemplo de una respuesta de **crear contrato** enviada desde Blockchain Workbench:

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Submitted",
    "additionalInformation": { }
}
```

Ejemplo de una respuesta de **crear contrato** confirmada desde Blockchain Workbench:

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Committed",
    "additionalInformation": { }
}
```

Si la solicitud no se realizó correctamente, se incluyen detalles sobre el error en la información adicional.

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": null,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Failure",
    "additionalInformation": {
        "errorCode": 4000,
        "errorMessage": "Contract cannot be provisioned on connection."
    }
}
```

### <a name="create-contract-action"></a>Crear acción de contrato

Crea una nueva acción de contrato.

La solicitud requiere los siguientes campos:

| **Nombre**                 | **Descripción**                                                                                                           |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------|
| requestId                | GUID del cliente proporcionado |
| userChainIdentifier      | Dirección del usuario que se ha creado en la red de la cadena de bloques. En Ethereum, es la dirección del usuario **en la cadena**. |
| contractLedgerIdentifier | Dirección del contrato en el libro de contabilidad |
| version                  | Versión del tipo de aplicación. Requerido si tiene varias versiones de la aplicación habilitada. En caso contrario, la versión es opcional. Para obtener más información sobre las versiones de las aplicaciones, consulte [Control de versiones de una aplicación de Azure Blockchain Workbench](version-app.md). |
| workflowFunctionName     | Nombre de la función de flujo de trabajo |
| parámetros               | Entrada de parámetros para la creación de contratos |
| connectionId             | Identificador único de la conexión de cadena de bloques |
| messageSchemaVersion     | Versión del esquema de mensajería |
| messageName              | **CreateContractActionRequest** |

Ejemplo:

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398",
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "version": "1.0",
    "workflowFunctionName": "modify",
    "parameters": [
        {
            "name": "description",
            "value": "a 1969 dodge charger"
        },
        {
            "name": "price",
            "value": "12345"
        }
    ],
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionRequest"
}
```

Blockchain Workbench devuelve una respuesta con los siguientes campos:

| **Nombre**              | **Descripción**                                                                   |
|-----------------------|-----------------------------------------------------------------------------------|
| requestId             | GUID del cliente proporcionado|
| contractId            | Identificador único del contrato en Azure Blockchain Workbench |
| connectionId          | Identificador único de la conexión de cadena de bloques |
| messageSchemaVersion  | Versión del esquema de mensajería |
| messageName           | **CreateContractActionUpdate** |
| status                | Estado de la solicitud de acción de contrato. Valores posibles: **Enviado**, **Confirmado** y **Error**.                         |
| additionalInformation | Proporciona información adicional en función del estado. |

Ejemplo de una respuesta de **crear acción de contrato** enviada desde Blockchain Workbench:

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Submitted",
    "additionalInformation": { }
}
```

Ejemplo de una respuesta de **crear acción de contrato** confirmada desde Blockchain Workbench:

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Committed",
    "additionalInformation": { }
}
```

Si la solicitud no se realizó correctamente, se incluyen detalles sobre el error en la información adicional.

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Failure",
    "additionalInformation": {
        "errorCode": 4000,
        "errorMessage": "Contract action cannot be provisioned on connection."
    }
}
```

### <a name="input-api-error-codes-and-messages"></a>Mensajes y códigos de error de la API de entrada

**Código de error 4000: Error de solicitud incorrecta**
- ConnectionId no válido
- Error de deserialización de CreateUserRequest
- Error de deserialización de CreateContractRequest
- Error de deserialización de CreateContractActionRequest
- La aplicación {identificada por el nombre de la aplicación} no existe.
- La aplicación {identificada por el nombre de la aplicación} no tiene ningún flujo de trabajo.
- UserChainIdentifier no existe.
- El contrato {identificado por el identificador del libro de contabilidad} no existe.
- El contrato {identificado por el identificador del libro de contabilidad} no tiene la función {nombre de la función del flujo de trabajo}.
- UserChainIdentifier no existe.

**Código de error 4090: Error de conflicto**
- El usuario ya existe.
- El contrato ya existe.
- La acción de contrato ya existe.

**Código de error 5000: Error interno del servidor**
- Mensajes de excepción

## <a name="event-notifications"></a>Notificaciones de eventos

Las notificaciones de eventos se pueden usar para notificar a los usuarios y a los sistemas de nivel final de eventos que se producen en Blockchain Workbench y la red de cadena de bloques a la que está conectada. Las notificaciones de eventos se pueden consumir directamente en código o se pueden utilizar con herramientas como Logic Apps y Flow para desencadenar el flujo de datos hacia los sistemas de nivel final.

Consulte la [Referencia de mensajes de notificación](#notification-message-reference) para más detalles sobre los distintos mensajes que se pueden recibir.

### <a name="consuming-event-grid-events-with-azure-functions"></a>Consumo de eventos de Event Grid con Azure Functions

Si un usuario desea usar Event Grid para recibir notificaciones sobre eventos que se producen en Blockchain Workbench, puede consumir eventos de Event Grid mediante el uso de Azure Functions.

1. Cree una **aplicación de función de Azure** en Azure Portal.
2. Cree una nueva función.
3. Busque la plantilla de Event Grid. Se muestra el código de la plantilla básica para leer el mensaje. Modifique el código según sea necesario.
4. Guarde la función. 
5. Seleccione la instancia de Event Grid desde el grupo de recursos de Blockchain Workbench.

### <a name="consuming-event-grid-events-with-logic-apps"></a>Consumo de eventos de Event Grid con Logic Apps

1. Cree una nueva **aplicación lógica de Azure** en Azure Portal.
2. Al abrir la aplicación de la lógica de Azure en el portal, se le pedirá seleccionar un desencadenador. Seleccione **Azure Event Grid: cuando se produce un evento de recurso**.
3. Cuando se muestra el diseñador de flujo de trabajo, se le pedirá que inicie sesión.
4. Seleccione la suscripción. Recurso como **Microsoft.EventGrid.Topics**. Seleccione el **Nombre de recurso** desde el nombre del recurso del grupo de recursos de Azure Blockchain Workbench.
5. Seleccione la instancia de Event Grid desde el grupo de recursos de Blockchain Workbench.

## <a name="using-service-bus-topics-for-notifications"></a>Uso de temas de Service Bus para notificaciones

Los temas de Service Bus se pueden utilizar para notificar a los usuarios acerca de los eventos que se producen en Blockchain Workbench. 

1. Vaya a Service Bus en el grupo de recursos de Workbench.
2. Seleccione **Temas**.
3. Seleccione **egress-topic**.
4. Cree una nueva suscripción a este tema. Obtenga una clave para la suscripción.
5. Cree un programa, que se suscribe a los eventos de esta suscripción.

### <a name="consuming-service-bus-messages-with-logic-apps"></a>Consumo de mensajes de Service Bus con Logic Apps

1. Cree una nueva **aplicación lógica de Azure** en Azure Portal.
2. Al abrir la aplicación de la lógica de Azure en el portal, se le pedirá seleccionar un desencadenador. Escriba **Service Bus** en el cuadro de búsqueda y seleccione el desencadenador adecuado para el tipo de interacción que desea tener con Service Bus. Por ejemplo, **Service Bus: cuando se recibe un mensaje en una suscripción de tema (autocompletar)** .
3. Cuando se muestra el diseñador de flujo de trabajo, especifique la información de conexión para Service Bus.
4. Seleccione la suscripción y especifique el tema **workbench-external**.
5. Desarrolle la lógica de la aplicación que utiliza el mensaje de este desencadenador.

## <a name="notification-message-reference"></a>Referencia de mensajes de notificación

En función del valor de **messageName**, los mensajes de notificación tienen uno de los siguientes tipos de mensaje.

### <a name="block-message"></a>Mensaje de bloque

Contiene información sobre bloques individuales. *BlockMessage* incluye una sección con información de nivel de bloque y una sección con información de la transacción.

| Nombre | Descripción |
|------|-------------|
| block | Contiene [información del bloque](#block-information) |
| transacciones | Contiene [información de la transacción](#transaction-information) de la colección para el bloque |
| connectionId | Identificador único de la conexión |
| messageSchemaVersion | Versión del esquema de mensajería |
| messageName | **BlockMessage** |
| additionalInformation | Información adicional proporcionada |

#### <a name="block-information"></a>Información de bloque

| Nombre              | Descripción |
|-------------------|-------------|
| blockId           | Identificador único del bloque en Azure Blockchain Workbench |
| blockNumber       | Identificador único de un bloque en el libro de contabilidad |
| blockHash         | Hash del bloque |
| previousBlockHash | El hash del bloque anterior |
| blockTimestamp    | Marca de tiempo del bloque |

#### <a name="transaction-information"></a>Información de la transacción

| Nombre               | Descripción |
|--------------------|-------------|
| transactionId      | Identificador único de la transacción en Azure Blockchain Workbench |
| transactionHash    | Hash de la transacción en el libro de contabilidad |
| desde               | Identificador único en el libro de contabilidad para el origen de la transacción |
| to                 | Identificador único en el libro de contabilidad para el destino de la transacción |
| provisioningStatus | Identifica el estado actual del proceso de aprovisionamiento de la transacción. Los valores posibles son: </br>0: la API ha creado la transacción en la base de datos</br>1: la transacción se ha enviado al libro de contabilidad</br>2: la transacción se ha confirmado correctamente en el libro de contabilidad</br>3 o 4: la transacción no se pudo confirmar en el libro de contabilidad</br>5: la transacción se confirmó correctamente en el libro de contabilidad |

Ejemplo de un *BlockMessage* de Blockchain Workbench:

``` json
{
    "block": {
        "blockId": 123,
        "blockNumber": 1738312,
        "blockHash": "0x03a39411e25e25b47d0ec6433b73b488554a4a5f6b1a253e0ac8a200d13fffff",
        "previousBlockHash": null,
        "blockTimestamp": "2018-10-09T23:35:58Z",
    },
    "transactions": [
        {
            "transactionId": 234,
            "transactionHash": "0xa4d9c95b581f299e41b8cc193dd742ef5a1d3a4ddf97bd11b80d123fec27ffff",
            "from": "0xd85e7262dd96f3b8a48a8aaf3dcdda90f60dffff",
            "to": null,
            "provisioningStatus": 1
        },
        {
            "transactionId": 235,
            "transactionHash": "0x5c1fddea83bf19d719e52a935ec8620437a0a6bdaa00ecb7c3d852cf92e1ffff",
            "from": "0xadd97e1e595916e29ea94fda894941574000ffff",
            "to": "0x9a8DDaCa9B7488683A4d62d0817E965E8f24ffff",
            "provisioningStatus": 2
        }
    ],
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "BlockMessage",
    "additionalInformation": {}
}
```

### <a name="contract-message"></a>Mensaje del contrato

Contiene información sobre un contrato. El mensaje incluye una sección con propiedades del contrato y una sección con información de la transacción. Todas las transacciones que han modificado el contrato para el bloque determinado se incluyen en la sección de la transacción.

| Nombre | Descripción |
|------|-------------|
| blockId | Identificador único del bloque en Azure Blockchain Workbench |
| blockHash | Hash del bloque |
| modifyingTransactions | [Las transacciones que modificaron](#modifying-transaction-information) el contrato |
| contractId | Identificador único del contrato en Azure Blockchain Workbench |
| contractLedgerIdentifier | Identificador único del contrato en el libro de contabilidad |
| contractProperties | [Propiedades del contrato](#contract-properties) |
| isNewContract | Indica si este contrato se creó recientemente o no. Los valores posibles son: true (este contrato se creó recientemente) o false (este contrato es la actualización de otro contrato). |
| connectionId | Identificador único de la conexión |
| messageSchemaVersion | Versión del esquema de mensajería |
| messageName | **ContractMessage** |
| additionalInformation | Información adicional proporcionada |

#### <a name="modifying-transaction-information"></a>Modificación de la información de la transacción

| Nombre               | Descripción |
|--------------------|-------------|
| transactionId | Identificador único de la transacción en Azure Blockchain Workbench |
| transactionHash | Hash de la transacción en el libro de contabilidad |
| desde | Identificador único en el libro de contabilidad para el origen de la transacción |
| to | Identificador único en el libro de contabilidad para el destino de la transacción |

#### <a name="contract-properties"></a>Propiedades del contrato

| Nombre               | Descripción |
|--------------------|-------------|
| workflowPropertyId | Identificador único de la propiedad del flujo de trabajo en Azure Blockchain Workbench |
| name | Nombre de la propiedad del flujo de trabajo |
| value | Valor de la propiedad del flujo de trabajo |

Ejemplo de un *ContractMessage* de Blockchain Workbench:

``` json
{
    "blockId": 123,
    "blockhash": "0x03a39411e25e25b47d0ec6433b73b488554a4a5f6b1a253e0ac8a200d13fffff",
    "modifyingTransactions": [
        {
            "transactionId": 234,
            "transactionHash": "0x5c1fddea83bf19d719e52a935ec8620437a0a6bdaa00ecb7c3d852cf92e1ffff",
            "from": "0xd85e7262dd96f3b8a48a8aaf3dcdda90f60dffff",
            "to": "0xf8559473b3c7197d59212b401f5a9f07ffff"
        },
        {
            "transactionId": 235,
            "transactionHash": "0xa4d9c95b581f299e41b8cc193dd742ef5a1d3a4ddf97bd11b80d123fec27ffff",
            "from": "0xd85e7262dd96f3b8a48a8aaf3dcdda90f60dffff",
            "to": "0xf8559473b3c7197d59212b401f5a9f07b429ffff"
        }
    ],
    "contractId": 111,
    "contractLedgerIdentifier": "0xf8559473b3c7197d59212b401f5a9f07b429ffff",
    "contractProperties": [
        {
            "workflowPropertyId": 1,
            "name": "State",
            "value": "0"
        },
        {
            "workflowPropertyId": 2,
            "name": "Description",
            "value": "1969 Dodge Charger"
        },
        {
            "workflowPropertyId": 3,
            "name": "AskingPrice",
            "value": "30000"
        },
        {
            "workflowPropertyId": 4,
            "name": "OfferPrice",
            "value": "0"
        },
        {
            "workflowPropertyId": 5,
            "name": "InstanceAppraiser",
            "value": "0x0000000000000000000000000000000000000000"
        },
        {
            "workflowPropertyId": 6,
            "name": "InstanceBuyer",
            "value": "0x0000000000000000000000000000000000000000"
        },
        {
            "workflowPropertyId": 7,
            "name": "InstanceInspector",
            "value": "0x0000000000000000000000000000000000000000"
        },
        {
            "workflowPropertyId": 8,
            "name": "InstanceOwner",
            "value": "0x9a8DDaCa9B7488683A4d62d0817E965E8f24ffff"
        },
        {
            "workflowPropertyId": 9,
            "name": "ClosingDayOptions",
            "value": "[21,48,69]"
        }
    ],
    "isNewContract": false,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "ContractMessage",
    "additionalInformation": {}
}
```

### <a name="event-message-contract-function-invocation"></a>Mensaje de evento: Invocación de la función de contrato

Contiene información cuando se invoca una función de contrato, por ejemplo, el nombre de función, la entradas de parámetros y el llamador de la función.

| Nombre | Descripción |
|------|-------------|
| eventName                   | **ContractFunctionInvocation** |
| caller                      | [Información del autor de la llamada](#caller-information) |
| contractId                  | Identificador único del contrato en Azure Blockchain Workbench |
| contractLedgerIdentifier    | Identificador único del contrato en el libro de contabilidad |
| functionName                | Nombre de la función. |
| parámetros                  | [Información del parámetro](#parameter-information) |
| transaction                 | Información de la transacción |
| inTransactionSequenceNumber | El número de secuencia de la transacción en el bloque |
| connectionId                | Identificador único de la conexión |
| messageSchemaVersion        | Versión del esquema de mensajería |
| messageName                 | **EventMessage** |
| additionalInformation       | Información adicional proporcionada |

#### <a name="caller-information"></a>Información del autor de la llamada

| Nombre | Descripción |
|------|-------------|
| type | Tipo del autor de la llamada, como un usuario o un contrato |
| id | Identificador único del autor de la llamada en Azure Blockchain Workbench |
| ledgerIdentifier | Identificador único del autor de la llamada en el libro de contabilidad |

#### <a name="parameter-information"></a>Información del parámetro

| Nombre | Descripción |
|------|-------------|
| name | Nombre de parámetro |
| value | Valor del parámetro |

#### <a name="event-message-transaction-information"></a>Información de la transacción del mensaje de eventos

| Nombre               | Descripción |
|--------------------|-------------|
| transactionId      | Identificador único de la transacción en Azure Blockchain Workbench |
| transactionHash    | Hash de la transacción en el libro de contabilidad |
| desde               | Identificador único en el libro de contabilidad para el origen de la transacción |
| to                 | Identificador único en el libro de contabilidad para el destino de la transacción |

Ejemplo de *EventMessage ContractFunctionInvocation* de Blockchain Workbench:

``` json
{
    "eventName": "ContractFunctionInvocation",
    "caller": {
        "type": "User",
        "id": 21,
        "ledgerIdentifier": "0xd85e7262dd96f3b8a48a8aaf3dcdda90f60ffff"
    },
    "contractId": 34,
    "contractLedgerIdentifier": "0xf8559473b3c7197d59212b401f5a9f07b429ffff",
    "functionName": "Modify",
    "parameters": [
        {
            "name": "description",
            "value": "a new description"
        },
        {
            "name": "price",
            "value": "4567"
        }
    ],
    "transaction": {
        "transactionId": 234,
        "transactionHash": "0x5c1fddea83bf19d719e52a935ec8620437a0a6bdaa00ecb7c3d852cf92e1ffff",
        "from": "0xd85e7262dd96f3b8a48a8aaf3dcdda90f60dffff",
        "to": "0xf8559473b3c7197d59212b401f5a9f07b429ffff"
    },
    "inTransactionSequenceNumber": 1,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "EventMessage",
    "additionalInformation": { }
}
```

### <a name="event-message-application-ingestion"></a>Mensaje de evento: Ingesta de aplicación

Contiene información de cuando una aplicación se carga en Workbench, como el nombre y la versión de la aplicación cargada.

| Nombre | Descripción |
|------|-------------|
| eventName | **ApplicationIngestion** |
| applicationId | Identificador único de la aplicación en Azure Blockchain Workbench |
| applicationName | Nombre de la aplicación |
| applicationDisplayName | Nombre para mostrar de la aplicación |
| applicationVersion | Versión de la aplicación |
| applicationDefinitionLocation | Dirección URL en la que se encuentra el archivo de configuración de la aplicación |
| contractCodes | Colección de [códigos de contrato](#contract-code-information) para la aplicación |
| applicationRoles | Colección de [códigos de aplicación](#application-role-information) para la aplicación |
| applicationWorkflows | Colección de [flujos de trabajo de aplicación](#application-workflow-information) para la aplicación |
| connectionId | Identificador único de la conexión |
| messageSchemaVersion | Versión del esquema de mensajería |
| messageName | **EventMessage** |
| additionalInformation | La información adicional proporcionada aquí incluye los estados de flujo de trabajo de la aplicación y la información de transición. |

#### <a name="contract-code-information"></a>Información de código de contrato

| Nombre | Descripción |
|------|-------------|
| id | Identificador único del archivo de código de contrato en Azure Blockchain Workbench |
| ledgerId | Identificador único del libro de contabilidad en Azure Blockchain Workbench |
| ubicación | Dirección URL donde se encuentra el archivo de código de contrato |

#### <a name="application-role-information"></a>Información de rol de la aplicación

| Nombre | Descripción |
|------|-------------|
| id | Identificador único del rol de la aplicación en Azure Blockchain Workbench |
| name | Nombre del rol de la aplicación |

#### <a name="application-workflow-information"></a>Información del flujo de trabajo de la aplicación

| Nombre | Descripción |
|------|-------------|
| id | Identificador único del flujo de trabajo de la aplicación en Azure Blockchain Workbench |
| name | Nombre del flujo de trabajo de la aplicación |
| DisplayName | Nombre para mostrar del flujo de trabajo de la aplicación |
| functions | Colección de [funciones para el flujo de trabajo de la aplicación](#workflow-function-information)|
| states | Colección de [estados para el flujo de trabajo de la aplicación](#workflow-state-information) |
| properties | [Información de propiedades del flujo de trabajo](#workflow-property-information) de la aplicación |

##### <a name="workflow-function-information"></a>Información de la función del flujo de trabajo

| Nombre | Descripción |
|------|-------------|
| id | Identificador único de la función del flujo de trabajo de la aplicación en Azure Blockchain Workbench |
| name | Nombre de función |
| parámetros | Parámetros de la función |

##### <a name="workflow-state-information"></a>Información de estado de flujo de trabajo

| Nombre | Descripción |
|------|-------------|
| name | Nombre del estado |
| DisplayName | Nombre para mostrar del estado |
| style | Estilo del estado (completado o error) |

##### <a name="workflow-property-information"></a>Información de la propiedad del flujo de trabajo

| Nombre | Descripción |
|------|-------------|
| id | Identificador único de la propiedad del flujo de trabajo de la aplicación en Azure Blockchain Workbench |
| name | Nombre de propiedad |
| type | Tipo de propiedad |

Ejemplo de *EventMessage ApplicationIngestion* de Blockchain Workbench:

``` json
{
    "eventName": "ApplicationIngestion",
    "applicationId": 31,
    "applicationName": "AssetTransfer",
    "applicationDisplayName": "Asset Transfer",
    "applicationVersion": "1.0",
    "applicationDefinitionLocation": "http://url",
    "contractCodes": [
        {
            "id": 23,
            "ledgerId": 1,
            "location": "http://url"
        }
    ],
    "applicationRoles": [
            {
                "id": 134,
                "name": "Buyer"
            },
            {
                "id": 135,
                "name": "Seller"
            }
       ],
    "applicationWorkflows": [
        {
            "id": 89,
            "name": "AssetTransfer",
            "displayName": "Asset Transfer",
            "functions": [
                {
                    "id": 912,
                    "name": "",
                    "parameters": [
                        {
                            "name": "description",
                            "type": {
                                "name": "string"
                             }
                        },
                        {
                            "name": "price",
                            "type": {
                                "name": "int"
                            }
                        }
                    ]
                },
                {
                    "id": 913,
                    "name": "modify",
                    "parameters": [
                        {
                            "name": "description",
                            "type": {
                                "name": "string"
                             }
                        },
                        {
                            "name": "price",
                            "type": {
                                "name": "int"
                            }
                        }
                    ]
                }
            ],
            "states": [ 
                 {
                      "name": "Created",
                      "displayName": "Created",
                      "style" : "Success"
                 },
                 {
                      "name": "Terminated",
                      "displayName": "Terminated",
                      "style" : "Failure"
                 }
            ],
            "properties": [
                {
                    "id": 879,
                    "name": "Description",
                    "type": {
                        "name": "string"
                     }
                },
                {
                    "id": 880,
                    "name": "Price",
                    "type": {
                        "name": "int"
                     }
                }
            ]
        }
    ],
    "connectionId": [ ],
    "messageSchemaVersion": "1.0.0",
    "messageName": "EventMessage",
    "additionalInformation":
        {
            "states" :
            [
                {
                    "Name": "BuyerAccepted",
                    "Transitions": [
                        {
                            "DisplayName": "Accept",
                            "AllowedRoles": [ ],
                            "AllowedInstanceRoles": [ "InstanceOwner" ],
                            "Function": "Accept",
                            "NextStates": [ "SellerAccepted" ]
                        }
                    ]
                }
            ]
        }
}
```

### <a name="event-message-role-assignment"></a>Mensaje de evento: Asignación de roles

Contiene información de cuando a un usuario se le asigna un rol en Workbench, como quién realizó la asignación del rol y el nombre del rol y la aplicación correspondiente.

| Nombre | Descripción |
|------|-------------|
| eventName | **RoleAssignment** |
| applicationId | Identificador único de la aplicación en Azure Blockchain Workbench |
| applicationName | Nombre de la aplicación |
| applicationDisplayName | Nombre para mostrar de la aplicación |
| applicationVersion | Versión de la aplicación |
| applicationRole        | Contiene información sobre el [rol de aplicación](#roleassignment-application-role) |
| assigner               | Información sobre el [asignador](#roleassignment-assigner) |
| assignee               | Información sobre el [asignado](#roleassignment-assignee) |
| connectionId           | Identificador único de la conexión |
| messageSchemaVersion   | Versión del esquema de mensajería |
| messageName            | **EventMessage** |
| additionalInformation  | Información adicional proporcionada |

#### <a name="roleassignment-application-role"></a>Rol de aplicación RoleAssignment

| Nombre | Descripción |
|------|-------------|
| id | Identificador único del rol de la aplicación en Azure Blockchain Workbench |
| name | Nombre del rol de la aplicación |

#### <a name="roleassignment-assigner"></a>Asignador de RoleAssignment

| Nombre | Descripción |
|------|-------------|
| id | Identificador único del usuario en Azure Blockchain Workbench |
| type | Tipo de asignador |
| chainIdentifier | Identificador único del usuario en el libro de contabilidad |

#### <a name="roleassignment-assignee"></a>Asignado de RoleAssignment

| Nombre | Descripción |
|------|-------------|
| id | Identificador único del usuario en Azure Blockchain Workbench |
| type | Tipo de asignado |
| chainIdentifier | Identificador único del usuario en el libro de contabilidad |

Ejemplo de *EventMessage RoleAssignment* de Blockchain Workbench:

``` json
{
    "eventName": "RoleAssignment",
    "applicationId": 31,
    "applicationName": "AssetTransfer",
    "applicationDisplayName": "Asset Transfer",
    "applicationVersion": "1.0",
    "applicationRole": {
        "id": 134,
        "name": "Buyer"
    },
    "assigner": {
        "id": 1,
        "type": null,
        "chainIdentifier": "0xeFFC7766d38aC862d79706c3C5CEEf089564ffff"
    },
    "assignee": {
        "id": 3,
        "type": null,
        "chainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f24ffff"
    },
    "connectionId": [ ],
    "messageSchemaVersion": "1.0.0",
    "messageName": "EventMessage",
    "additionalInformation": { }
}
```

## <a name="next-steps"></a>Pasos siguientes

- [Patrones de integración de contratos inteligentes](integration-patterns.md)
