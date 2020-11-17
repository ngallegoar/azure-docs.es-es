---
author: baanders
description: Archivo de inclusión para configurar la autenticación local para DefaultAzureCredential en los ejemplos de Azure Digital Twins, sin introducción
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: 3cb053f9673532ac19e2098678ec341f2f676486
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "94329485"
---
Con `DefaultAzureCredential`, el ejemplo buscará las credenciales en el entorno local; por ejemplo, un inicio de sesión de Azure en una [CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) local o en Visual Studio o Visual Studio Code. Por este motivo, debe *iniciar sesión en Azure localmente* mediante uno de estos mecanismos para configurar las credenciales del ejemplo.

Si usa Visual Studio o Visual Studio Code para ejecutar el ejemplo de código, asegúrese de que inicia sesión en ese editor con las mismas credenciales de Azure que quiere usar para acceder a la instancia de Azure Digital Twins.

Si no, puede [instalar la CLI de Azure local](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true), iniciar un símbolo del sistema en la máquina y ejecutar el comando `az login` para iniciar sesión en su cuenta de Azure. Después de iniciar sesión, cuando ejecute el código de ejemplo, se debería iniciar sesión automáticamente.