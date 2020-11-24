---
title: Actualizar un comando desde un punto de conexión web
titleSuffix: Azure Cognitive Services
description: Actualización de un comando desde un punto de conexión web
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/20/2020
ms.author: encorona
ms.openlocfilehash: 4432843ac93002bc92068db191706352234d76e6
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94572663"
---
# <a name="update-a-command-from-a-web-endpoint"></a>Actualizar un comando desde un punto de conexión web

Si la aplicación cliente necesita actualizar el estado de un comando en curso sin entrada de voz, puede usar una llamada a un punto de conexión web para actualizar el comando.

En este artículo, aprenderá a actualizar un comando en curso desde un punto de conexión web.

## <a name="prerequisites"></a>Requisitos previos
> [!div class = "checklist"]
> * Una [aplicación de Comandos personalizados creada](quickstart-custom-commands-application.md) previamente.

## <a name="create-an-azure-function"></a>Creación de una Función de Azure 

En este ejemplo, se necesitará una [función de Azure](https://docs.microsoft.com/azure/azure-functions/) desencadenada por HTTP que admita la entrada siguiente (o un subconjunto de esta entrada).

```JSON
{
  "conversationId": "SomeConversationId",
  "currentCommand": {
    "name": "SomeCommandName",
    "parameters": {
      "SomeParameterName": "SomeParameterValue",
      "SomeOtherParameterName": "SomeOtherParameterValue"
    }
  },
  "currentGlobalParameters": {
      "SomeGlobalParameterName": "SomeGlobalParameterValue",
      "SomeOtherGlobalParameterName": "SomeOtherGlobalParameterValue"
  }
}
```

Revisemos los atributos clave de esta entrada.

| Atributo | Explicación |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **conversationId** | "conversationId" es el identificador único de la conversación; tenga en cuenta que este identificador se puede generar desde la aplicación cliente. |
| **currentCommand** | "currentCommand" es el comando activo actualmente en la conversación. |
| **name** | "name" es el nombre del comando y "parameters" es una asignación con los valores actuales de los parámetros. |
| **currentGlobalParameters** | "currentGlobalParameters" es también una asignación como "parameters", pero se usa para los parámetros globales. |

La salida de la función de Azure debe admitir el formato siguiente.

```JSON
{
  "updatedCommand": {
    "name": "SomeCommandName",
    "updatedParameters": {
      "SomeParameterName": "SomeParameterValue"
    },
    "cancel": false
  },
  "updatedGlobalParameters": {
    "SomeGlobalParameterName": "SomeGlobalParameterValue"
  }
}
```

Es posible que reconozca este formato, ya que es el mismo que se usa al [actualizar un comando desde el cliente](./how-to-custom-commands-update-command-from-client.md). 

Ahora, cree una función de Azure basada en NodeJS y copie y pegue este código.

```nodejs
module.exports = async function (context, req) {
    context.log(req.body);
    context.res = {
        body: {
            updatedCommand: {
                name: "IncrementCounter",
                updatedParameters: {
                    Counter: req.body.currentCommand.parameters.Counter + 1
                }
            }
        }
    };
}
```

Cuando se llame a esta función de Azure desde Comandos personalizados, se enviarán los valores actuales de la conversación y se devolverán los parámetros que se desean actualizar o si se desea cancelar el comando actual.

## <a name="update-the-existing-custom-commands-app"></a>Actualización de la aplicación de Comandos personalizados existente

Ahora vamos a enlazar la función de Azure con la aplicación de Comandos personalizados existente.

1. Agregue un nuevo comando llamado IncrementCounter.
1. Agregue solo una frase de ejemplo con el valor "increment".
1. Agregue un nuevo parámetro llamado Counter (el mismo nombre que se especificó en la función de Azure anterior) de tipo Number con un valor predeterminado de 0.
1. Agregue un nuevo punto de conexión web denominado IncrementEndpoint con la dirección URL de la función de Azure y con la opción de actualizaciones remotas habilitada.
    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-commands/set-web-endpoint-with-remote-updates.png" alt-text="Establecer el punto de conexión web con actualizaciones remotas":::
1. Cree una nueva regla de interacción denominada "IncrementRule" y agregue una acción de llamada al punto de conexión web.
    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-commands/increment-rule-web-endpoint.png" alt-text="Regla de incremento":::
1. En la configuración de la acción, seleccione IncrementEndpoint, configure On success (En caso de éxito) en Send speech response (Enviar respuesta de voz) con el valor de Counter y On failure (En caso de error) con un mensaje de error.
    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-commands/set-increment-counter-call-endpoint.png" alt-text="Establecer el incremento del contador de llamada al punto de conexión":::
1. Establezca el estado posterior a la ejecución de la regla en Wait for user's input (Esperar la entrada del usuario).

## <a name="test-it"></a>Pruébelo

1. Guarde y entrene la aplicación.
1. Haga clic en Test (Probar).
1. Envíe algunas veces "increment" (que es la frase de ejemplo para el comando IncrementCounter).
    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-commands/increment-counter-example.png" alt-text="Ejemplo de incremento de contador":::

Observe cómo el valor del parámetro Counter se incrementa cada vez por la función de Azure.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Habilite un proceso de CI/CD para su aplicación de Comandos personalizados](./how-to-custom-commands-deploy-cicd.md)