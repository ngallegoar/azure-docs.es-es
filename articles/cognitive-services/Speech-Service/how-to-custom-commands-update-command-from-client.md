---
title: Actualización de un comando desde la aplicación cliente
titleSuffix: Azure Cognitive Services
description: Actualización de un comando desde la aplicación cliente
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/20/2020
ms.author: encorona
ms.openlocfilehash: 1bffb09d0f49bbd0059e8a528d67bfe215f0650d
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94654354"
---
# <a name="update-a-command-from-the-client"></a>Actualización de un comando desde el cliente

En este artículo, aprenderá a actualizar un comando en curso desde una aplicación cliente.

## <a name="prerequisites"></a>Requisitos previos
> [!div class = "checklist"]
> * Una [aplicación de Comandos personalizados creada](quickstart-custom-commands-application.md) previamente.

## <a name="update-the-state-of-a-command"></a>Actualización del estado de un comando

Si la aplicación cliente necesita actualizar el estado de un comando en curso sin entrada de voz, puede enviar un evento para actualizar el comando.

Para ilustrar este escenario de actualización del estado de un comando en curso (TurnOnOff), podemos enviar la siguiente actividad de evento. 

```json
{
  "type": "event",
  "name": "RemoteUpdate",
  "value": {
    "updatedCommand": {
      "name": "TurnOnOff",
      "updatedParameters": {
        "OnOff": "on"
      },
      "cancel": false
    },
    "updatedGlobalParameters": {},
    "processTurn": true
  }
}
```

Revisemos los atributos clave de esta actividad.

| Atributo | Explicación |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **type** | La actividad es de tipo "event" y el nombre del evento debe ser "RemoteUpdate". |
| **value** | El atributo "value" contiene los atributos necesarios para actualizar el comando actual. |
| **updatedCommand** | El atributo "updatedCommand" contiene el nombre del comando; "updatedParameters" es una asignación con el nombre de los parámetros y sus valores actualizados. |
| **cancel** | Si es necesario cancelar el comando en curso, establezca el atributo "cancel" en true. |
| **updatedGlobalParameters** | El atributo "updatedGlobalParameters" también es una asignación, como "updatedParameters", pero se usa para los parámetros globales. |
| **processTurn** | Si el turno debe procesarse una vez enviada la actividad, establezca el atributo "processTurn" en true. |

Puede probar este escenario en el portal de Comandos personalizados.

1. Abra la aplicación de Comandos personalizados que creó anteriormente. 
1. Haga clic en Train (Entrenar) y después en Test (Probar).
1. Envíe "turn".
1. Abra el panel lateral y haga clic en Activity editor (Editor de actividades).
1. Escriba y envíe el evento RemoteCommand especificado en la sección anterior.
    > [!div class="mx-imgBorder"]
    > ![Enviar el comando remoto](media/custom-commands/send-remote-command-activity.png)

Observe cómo el valor del parámetro "OnOff" se estableció en "on" mediante una actividad realizada desde el cliente en lugar de usar voz o texto.

## <a name="update-the-catalog-of-the-parameter-for-a-command"></a>Actualización del catálogo del parámetro para un comando

Al configurar la lista de opciones válidas para un parámetro, los valores del parámetro se definen globalmente para la aplicación. 

En nuestro ejemplo, el parámetro SubjectDevice tendrá una lista fija de valores admitidos, independientemente de la conversación.

En caso de que necesite agregar nuevas entradas al catálogo del parámetro por conversación, puede enviar la siguiente actividad.

```json
{
  "type": "event",
  "name": "RemoteUpdate",
  "value": {
    "catalogUpdate": {
      "commandParameterCatalogs": {
        "TurnOnOff": [
          {
            "name": "SubjectDevice",
            "values": {
              "stereo": [
                "cd player"
              ]
            }
          }
        ]
      }
    },
    "processTurn": false
  }
}
```
Con esta actividad, agregamos una entrada para "stereo" al catálogo del parámetro "SubjectDevice" en el comando "TurnOnOff".

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/custom-commands/update-catalog-with-remote-activity.png" alt-text="Actualizar el catálogo":::

Tenga en cuenta un par de cuestiones.
1. Solo necesita enviar esta actividad una vez (idealmente, justo después de iniciar una conexión).
1. Después de enviar esta actividad, debe esperar a que el evento ParameterCatalogsUpdated se envíe de vuelta al cliente.

## <a name="add-additional-context-from-the-client-application"></a>Incorporación de contexto adicional desde la aplicación cliente

Puede establecer contexto adicional desde la aplicación cliente por conversación, contexto que se puede usar posteriormente en la aplicación de Comandos personalizados. 

Por ejemplo, supongamos que desea enviar el identificador y el nombre del dispositivo conectado a la aplicación de Comandos personalizados.

Para probar este escenario, vamos a crear un nuevo comando en la aplicación actual.
1. Cree un nuevo comando llamado GetDeviceInfo.
1. Agregue una frase de ejemplo con "get device info".
1. En la regla de finalización "Done" (Listo), agregue una acción de envío de respuesta de voz que contenga los atributos de clientContext.
    > ![Envío de respuesta de voz con contexto](media/custom-commands/send-speech-response-context.png)
1. Guarde, entrene y pruebe la aplicación.
1. Una vez en la ventana de pruebas, envíe una actividad para actualizar el contexto de cliente.
    > ```json
    >{
    >   "type": "event",
    >   "name": "RemoteUpdate",
    >   "value": {
    >     "clientContext": {
    >       "deviceId": "12345",
    >       "deviceName": "My device"
    >     },
    >     "processTurn": false
    >   }
    >}
    > ```
1. Envíe el texto "get device info".
    > ![Envío de actividad de contexto de cliente](media/custom-commands/send-client-context-activity.png)

Tenga en cuenta algunas cuestiones.
1. Solo necesita enviar esta actividad una vez (idealmente, justo después de iniciar una conexión).
1. Puede usar objetos complejos para clientContext.
1. Puede usar clientContext en las respuestas de voz, para enviar actividades y al llamar a puntos de conexión web.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Actualizar un comando desde un punto de conexión web](./how-to-custom-commands-update-command-from-web-endpoint.md)
