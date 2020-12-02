---
title: 'Inicio rápido: Creación y administración de tokens de acceso'
titleSuffix: An Azure Communication Services quickstart
description: Aprenda a administrar identidades y tokens de acceso mediante la biblioteca cliente de administración de Azure Communication Services.
author: tomaschladek
manager: nmurav
services: azure-communication-services
ms.author: tchladek
ms.date: 08/20/2020
ms.topic: quickstart
ms.service: azure-communication-services
zone_pivot_groups: acs-js-csharp-java-python
ms.openlocfilehash: 246b8041e7e12ea1a1391cb4690d02892bc31b63
ms.sourcegitcommit: 4295037553d1e407edeb719a3699f0567ebf4293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96325321"
---
# <a name="quickstart-create-and-manage-access-tokens"></a>Inicio rápido: Creación y administración de tokens de acceso

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

Para empezar a usar Azure Communication Services, utilice la biblioteca cliente de administración de Communication Services. Permite crear identidades y administrar los tokens de acceso. La identidad representa la entidad de la aplicación en Azure Communication Services (por ejemplo, usuario o dispositivo). Los tokens de acceso permiten que las aplicaciones cliente de chat y llamada se autentiquen directamente en Azure Communication Services. Se recomienda generar tokens de acceso en un servicio del lado servidor. Los tokens de acceso se usan para inicializar las bibliotecas cliente de Communication Services en los dispositivos cliente.

Los precios que se muestran en las imágenes de este tutorial son ejemplos solamente.

::: zone pivot="programming-language-csharp"
[!INCLUDE [.NET](./includes/user-access-token-net.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [JavaScript](./includes/user-access-token-js.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Python](./includes/user-access-token-python.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Java](./includes/user-access-token-java.md)]
::: zone-end

La salida de la aplicación describe cada acción que se completa:
<!---cSpell:disable --->
```console
Azure Communication Services - Access Tokens Quickstart

Created an identity: 8:acs:4ccc92c8-9815-4422-bddc-ceea181dc774_00000006-19e0-2727-80f5-8b3a0d003502

Issued an access token with 'voip' scope that expires at Fri Nov 27 2020 16:47:05 GMT-0800 (Pacific Standard Time):
<token signature here>

Successfully revoked all access tokens for identity with ID: 8:acs:4ccc92c8-9815-4422-bddc-ceea181dc774_00000006-19e0-2727-80f5-8b3a0d003502

Deleted the identity with ID: 8:acs:4ccc92c8-9815-4422-bddc-ceea181dc774_00000006-19e0-2727-80f5-8b3a0d003502
```
<!---cSpell:enable --->

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y quitar una suscripción a Communication Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él. Obtenga más información sobre la [limpieza de recursos](./create-communication-resource.md#clean-up-resources).


## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido ha aprendido a:

> [!div class="checklist"]
> * Administrar identidades
> * Emitir tokens de acceso
> * Usar la biblioteca cliente de administración de Communication Services


> [!div class="nextstepaction"]
> [Adición de la llamada de voz a una aplicación](./voice-video-calling/getting-started-with-calling.md)

Puede que también le interese:

 - [Información sobre la autenticación](../concepts/authentication.md)
 - [Incorporación de chat a una aplicación](./chat/get-started.md)
 - [Información sobre la arquitectura de cliente y servidor](../concepts/client-and-server-architecture.md)
 