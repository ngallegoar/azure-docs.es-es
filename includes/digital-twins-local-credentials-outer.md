---
author: baanders
description: Archivo de inclusión para configurar la autenticación local de DefaultAzureCredential en los ejemplos de Azure Digital Twins, con introducción
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: 56c63ee13c5e42350a2b544074a0b0527377ca4c
ms.sourcegitcommit: d6a739ff99b2ba9f7705993cf23d4c668235719f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92494660"
---
### <a name="set-up-local-azure-credentials"></a>Configuración de credenciales locales de Azure

En este ejemplo se usa [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential?preserve-view=true&view=azure-dotnet) (parte de la biblioteca de `Azure.Identity`) para autenticar a los usuarios mediante la instancia de Azure Digital Twins cuando la ejecuta en la máquina local. Para más información sobre las distintas formas en que una aplicación cliente puede autenticarse mediante Azure Digital Twins, consulte [ *Escritura de código de autenticación de aplicación*](../articles/digital-twins/how-to-authenticate-client.md).

[!INCLUDE [Azure Digital Twins: local credentials prereq (inner)](digital-twins-local-credentials-inner.md)]