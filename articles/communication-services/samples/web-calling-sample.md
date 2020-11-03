---
title: 'Azure Communication Services: ejemplo de llamada web'
titleSuffix: An Azure Communication Services sample
description: Obtenga información sobre el ejemplo de llamada web de Azure Communication Services
author: chriswhilar
manager: mariusu-msft
services: azure-communication-services
ms.author: mariusu
ms.date: 10/15/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: e6fc3441fac5fe037e9a268d26012761d1fece70
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2020
ms.locfileid: "92463228"
---
# <a name="get-started-with-the-web-calling-sample"></a>Introducción al ejemplo de llamada web

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

> [!IMPORTANT]
> [Este ejemplo está disponible en GitHub](https://github.com/Azure-Samples/communication-services-web-calling-tutorial/).

En el **ejemplo de llamada web** de Azure Communication Services se muestra cómo se puede usar la biblioteca cliente web de llamadas de Communication Services para crear una experiencia de llamada con JavaScript.

En esta guía de inicio rápido de ejemplo, veremos cómo funciona el ejemplo antes de ejecutarlo en la máquina local. A continuación, implementaremos el ejemplo en Azure con sus propios recursos de Azure Communication Services.

## <a name="overview"></a>Información general

El ejemplo de la llamada web es una aplicación web que sirve como tutorial detallado de las distintas funcionalidades que proporciona la biblioteca cliente de llamadas web de Communication Services. 

Este ejemplo se ha creado para desarrolladores y hace que sea muy fácil empezar a trabajar con Communication Services. Su interfaz de usuario se divide en varias secciones, y cada una de ellas incluye un botón "Mostrar código" que permite copiar código directamente desde el explorador en su propia aplicación de Communication Services.

Cuando el [ejemplo de llamada web](https://github.com/Azure-Samples/communication-services-web-calling-tutorial) se esté ejecutando en la máquina, verá la página de aterrizaje siguiente:

:::image type="content" source="./media/web-calling-tutorial-page-1.png" alt-text="Tutorial de llamadas web 1" lightbox="./media/web-calling-tutorial-page-1.png":::

:::image type="content" source="./media/web-calling-tutorial-page-2.png" alt-text="Tutorial de llamadas web 2" lightbox="./media/web-calling-tutorial-page-2.png":::


## <a name="user-provisioning-and-sdk-initialization"></a>Aprovisionamiento de usuario e inicialización de SDK 

Para empezar a usar la demostración, escriba la cadena de conexión del [recurso de Communication Services](../quickstarts/create-communication-resource.md) en `config.json`. Se usará para aprovisionar un [token de acceso de usuario](../concepts/authentication.md) para que se pueda inicializar el SDK que realiza la llamada.

Escriba su propio identificador personal en la entrada Identidad del usuario. Si no se especifica nada, se generará una identidad de usuario aleatoria. 

Haga clic en "Provisioning user and initialize SDK" (Aprovisionamiento de usuario e inicialización del SDK) para inicializar el SDK mediante un token aprovisionado por el servicio de aprovisionamiento de tokens de back-end. Este servicio de back-end está en `/project/webpack.config.js`.

Haga clic en el botón "Show code" (Mostrar código) para ver el código de ejemplo que puede usar en su propia solución.

Una vez que se inicialice el SDK, debería ver lo siguiente:

:::image type="content" source="./media/user-provisioning.png" alt-text="Aprovisionamiento de usuarios" lightbox="./media/user-provisioning.png":::

Ya está listo para empezar a realizar llamadas mediante el recurso de Communication Services.

## <a name="placing-and-receiving-calls"></a>Realización y recepción de llamadas

El SDK de llamada web de Communication Services permite llamadas **1:1** , **1: N** y **grupales**.

En el caso de las llamadas salientes 1:1 o 1:N, puede especificar varias identidades de usuario de Communication Services a las que llamar mediante valores separados por comas. También puede especificar números de teléfono tradicionales (RTC) a los que llamar mediante valores separados por comas. 

Cuando llame a números de teléfono tradicionales, especifique su identificador alternativo de autor de llamada. Haga clic en el botón "Place call" (Realizar llamada) para realizar una llamada saliente:

:::image type="content" source="./media/place-a-call.png" alt-text="Realización de una llamada" lightbox="./media/place-a-call.png":::

Para unirse a una llamada grupal, escriba el GUID que identifica la llamada y haga clic en el botón "Join group" (Unirse al grupo):

:::image type="content" source="./media/join-a-group-call.png" alt-text="Unión a una llamada grupal" lightbox="./media/join-a-group-call.png":::

Haga clic en el botón "Show code" (Mostrar código) para ver el código de ejemplo para realizar llamadas, recibir llamadas y unirse a llamadas grupales.

Una llamada activa tiene el siguiente aspecto:

:::image type="content" source="./media/group-call.png" alt-text="Llamada grupal" lightbox="./media/group-call.png":::

En este ejemplo también se proporcionan fragmentos de código para las siguientes funcionalidades:

  - Hacer clic en el icono del vídeo para encender o apagar la cámara de vídeo
  - Hacer clic en el icono del micrófono para encender o apagar el micrófono
  - Hacer clic en el icono de reproducción para retener o eliminar la retención de la llamada
  - Hacer clic en el icono de la pantalla para empezar a compartir la pantalla o dejar de hacerlo
  - Hacer clic en el icono de persona para agregar un participante a la llamada
  - Hacer clic en "Remove participant" (Quitar participante) en la lista de participantes para quitar un participante concreto de la llamada


## <a name="next-steps"></a>Pasos siguientes

>[!div class="nextstepaction"] 
>[Descargue el ejemplo de GitHub](https://github.com/Azure-Samples/communication-services-web-calling-tutorial/).

Para más información, consulte los siguientes artículos.

- Familiarícese con [el uso de la biblioteca cliente de llamada](../quickstarts/voice-video-calling/calling-client-samples.md)
- Más información sobre [cómo funciona la llamada](../concepts/voice-video-calling/about-call-types.md)
- Examine la [documentación de referencia de la API](https://docs.microsoft.com/javascript/api/azure-communication-services/@azure/communication-calling/?view=azure-communication-services-js)

## <a name="additional-reading"></a>Lecturas adicionales

- [GitHub de Azure Communication](https://github.com/Azure/communication): encuentre más ejemplos e información en la página oficial de GitHub.
- [Redux](https://redux.js.org/): Administración de estado del lado cliente
- [FluentUI](https://aka.ms/fluent-ui): Biblioteca de interfaz de usuario con tecnología de Microsoft
- [React](https://reactjs.org/): Biblioteca para compilar interfaces de usuario
- [ASP.NET Core](https://docs.microsoft.com/aspnet/core/introduction-to-aspnet-core?view=aspnetcore-3.1&preserve-view=true): Marco para compilar aplicaciones web
