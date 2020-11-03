---
author: baanders
description: 'archivo de inclusión para DefaultAzureCredential en ejemplos de Azure Digital Twins: nota'
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: 35996595158994308a78f4d1197dcdc8f00af618
ms.sourcegitcommit: d6a739ff99b2ba9f7705993cf23d4c668235719f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92494483"
---
>[!NOTE]
> En este ejemplo se usa [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential?preserve-view=true&view=azure-dotnet) (parte de la biblioteca `Azure.Identity`) para autenticar a los usuarios en la instancia de Azure Digital Twins cuando la ejecuta en la máquina local. Con este tipo de autenticación, el ejemplo buscará las credenciales de Azure en el entorno local, como un inicio de sesión de una instancia local de la [CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) o en Visual Studio y Visual Studio Code.
>
> Para más información sobre el uso de `DefaultAzureCredential` y otras opciones de autenticación, consulte [*Procedimiento: Escritura de código de autenticación de aplicación*](../articles/digital-twins/how-to-authenticate-client.md).