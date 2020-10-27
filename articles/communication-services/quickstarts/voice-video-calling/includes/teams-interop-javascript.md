---
title: 'Inicio rápido: unirse a una reunión de Teams'
author: chpalm
ms.author: mikben
ms.date: 10/10/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: dd75e5e97e5dca898ba10e91528782861fb949ec
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92114587"
---
## <a name="prerequisites"></a>Requisitos previos

- Una [aplicación de llamadas de Communication Services](../getting-started-with-calling.md) operativa.
- Una [implementación de Teams](https://docs.microsoft.com/deployoffice/teams-install).

## <a name="enable-teams-interoperability"></a>Habilitación de la interoperabilidad de Teams

La característica de interoperabilidad de Teams se encuentra actualmente en versión preliminar privada. Para habilitar esta característica para el recurso de Communication Services, envíe un correo electrónico a [acsfeedback@microsoft.com](mailto:acsfeedback@microsoft.com) que incluya:

1. El identificador de la suscripción de Azure que contiene el recurso de Communication Services.
2. El identificador de inquilino de Teams. La forma más fácil de conseguirlo es [obtener y compartir un vínculo con la instancia de Teams](https://support.microsoft.com/office/create-a-link-or-a-code-for-joining-a-team-11b0de3b-9288-4cb4-bc49-795e7028296f#:~:text=Create%20a%20link%20If%20you%E2%80%99re%20a%20team%20owner%2C,link%20into%20any%20browser%20to%20join%20the%20team).

Para usar esta característica debe ser miembro de la organización propietaria de ambas entidades.

## <a name="add-the-teams-ui-controls"></a>Incorporación de los controles de la interfaz de usuario de Teams

Agregue un nuevo cuadro de texto y un botón en el código HTML. El cuadro de texto se utilizará para especificar el contexto de reunión de Teams y el botón se usará para la unión a la reunión especificada:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Communication Client - Calling Sample</title>
  </head>
  <body>
    <h4>Azure Communication Services</h4>
    <h1>Calling Quickstart</h1>
    <input 
      id="callee-id-input"
      type="text"
      placeholder="Who would you like to call?"
      style="margin-bottom:1em; width: 200px;"
    />
    <input 
      id="teams-id-input"
      type="text"
      placeholder="Teams meeting context"
      style="margin-bottom:1em; width: 300px;"
    />
    <div>
      <button id="call-button" type="button" disabled="true">
        Start Call
      </button>
      &nbsp;
      <button id="hang-up-button" type="button" disabled="true">
        Hang Up
      </button>
         <button id="meeting-button" type="button" disabled="false">
        Join Teams Meeting
      </button>
    </div>
    <script src="./bundle.js"></script>
  </body>
</html>
```

## <a name="enable-the-teams-ui-controls"></a>Habilitación de los controles de la interfaz de usuario de Teams

Ahora podemos enlazar el botón **Join Teams Meeting** (Unirse a la reunión en Teams) al código de unión a la reunión ofrecida por Teams:

```javascript
meetingButton.addEventListener("click", () => {
    
    // join with meeting link
    call = callAgent.join({meetingLink: 'MEETING_LINK'}, {});

     // join with meeting coordinates
     call = callAgent.join({
        threadId: 'CHAT_THREAD_ID',
        organizerId: 'ORGANIZER_ID',
        tenantId: 'TENANT_ID',
        messageId: 'MESSAGE_ID'
    }, {})
    
    // toggle button states
    hangUpButton.disabled = false;
    callButton.disabled = true;
    meetingButton.disabled = true;
});
```

## <a name="get-the-meeting-context"></a>Obtención del contexto de la reunión

El contexto de Teams se puede recuperar mediante las API de Graph. Esto se detalla en la [documentación de Graph](https://docs.microsoft.com/graph/api/onlinemeeting-createorget?view=graph-rest-beta&tabs=http).

También puede obtener la información de la reunión necesaria en la dirección URL **Join Meeting** (Unirse a la reunión) de la propia invitación de la reunión.

## <a name="run-the-code"></a>Ejecución del código

Los usuarios de WebPack pueden usar `webpack-dev-server` para compilar y ejecutar la aplicación. Ejecute el siguiente comando para agrupar el host de aplicación en un servidor web local:

```console
npx webpack-dev-server --entry ./client.js --output bundle.js --debug --devtool inline-source-map
```

Abra el explorador web y vaya a http://localhost:8080/. Verá lo siguiente:

:::image type="content" source="../media/javascript/calling-javascript-app.png" alt-text="Captura de pantalla de la aplicación JavaScript completada.":::

Inserte el contexto de Teams en el cuadro de texto y presione *Join Teams Meeting* (Unirse a la reunión de Teams) para unirse a la reunión de Teams desde dentro de la aplicación de Communication Services.

