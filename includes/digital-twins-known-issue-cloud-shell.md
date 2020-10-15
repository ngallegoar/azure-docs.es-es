---
author: baanders
description: 'inclusión de un archivo para Azure Digital Twins: aparece un problema conocido con la autenticación de Cloud Shell'
ms.service: digital-twins
ms.topic: include
ms.date: 9/1/2020
ms.author: baanders
ms.openlocfilehash: bd3a192ed493a886da427cb6d89239a4eb8e4d17
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "89290111"
---
>[!NOTE]
>Actualmente hay un **problema conocido** en Cloud Shell que afecta a estos grupos de comandos: `az dt route`, `az dt model` y `az dt twin`.
>
>Para resolverlo, ejecute `az login` en Cloud Shell antes de ejecutar el comando, o bien use la [CLI local](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) en lugar de Cloud Shell. Para obtener más información, vea [*Solución de problemas: Problemas conocidos en Azure Digital Twins*](../articles/digital-twins/troubleshoot-known-issues.md#400-client-error-bad-request-in-cloud-shell).