---
author: baanders
description: 'inclusión de un archivo para Azure Digital Twins: aparece un problema conocido con la autenticación de Cloud Shell'
ms.service: digital-twins
ms.topic: include
ms.date: 9/1/2020
ms.author: baanders
ms.openlocfilehash: 574ca9d42565e897e49d6800e61bb2c33717891b
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92324941"
---
>[!NOTE]
>Actualmente hay un **problema conocido** en Cloud Shell que afecta a estos grupos de comandos al ejecutarse desde *https://shell.azure.com* : `az dt route`, `az dt model` y `az dt twin`.
>
>Para resolverlo, puede realizar una de las acciones siguientes:
> * Ejecute `az login` en Cloud Shell antes de ejecutar el comando.
> * Abra el panel de Cloud Shell en Azure Portal y complete desde ahí su trabajo de Cloud Shell.
>  :::image type="content" source="../articles/digital-twins/media/includes/portal-cloud-shell.png" alt-text="Vista de Azure Portal con el icono de &quot;Cloud Shell&quot; resaltado; y Cloud Shell aparece indicado en la parte inferior de la ventana del portal.":::
> * Use la [CLI local](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) en lugar de Cloud Shell.
>
>Para más información, consulte [*Solución de problemas: Problemas conocidos en Azure Digital Twins*](../articles/digital-twins/troubleshoot-known-issues.md#400-client-error-bad-request-in-cloud-shell).