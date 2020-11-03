---
author: baanders
description: Archivo de inclusión para configurar la autenticación local para DefaultAzureCredential en los ejemplos de Azure Digital Twins, sin introducción
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: 8bf0590b867d37a22706f679bfbeee2140935637
ms.sourcegitcommit: d6a739ff99b2ba9f7705993cf23d4c668235719f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92494664"
---
Con `DefaultAzureCredential`, el ejemplo buscará las credenciales en el entorno local. Por ejemplo, un inicio de sesión de Azure en una [CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) local o en Visual Studio/Visual Studio Code. Esto significa que debe **iniciar sesión en Azure localmente** a través de uno de estos mecanismos para configurar las credenciales del ejemplo.

Si usa Visual Studio o Visual Studio Code para ejecutar el ejemplo de código, asegúrese de que iniciar sesión en ese editor con las mismas credenciales de Azure que quiere usar para acceder a la instancia de Azure Digital Twins.

De lo contrario, puede [instalar la **CLI de Azure**](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) local, iniciar un símbolo del sistema en la máquina y ejecutar el comando `az login` para iniciar sesión en su cuenta de Azure. Después de esto, cuando ejecute el código de ejemplo, debería iniciarse sesión automáticamente.