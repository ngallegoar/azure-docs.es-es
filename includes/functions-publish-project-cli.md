---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/18/2020
ms.author: glenga
ms.openlocfilehash: c99cac6626cb40b8fd368e4fc1d8454bb2195521
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93424876"
---
## <a name="deploy-the-function-project-to-azure"></a>Implementación del proyecto de función en Azure

Después de haber creado correctamente su aplicación de funciones en Azure, estará listo para implementar el proyecto de funciones local mediante el comando [func azure functionapp publish](../articles/azure-functions/functions-run-local.md#project-file-deployment).  

En el ejemplo siguiente, reemplace `<APP_NAME>` por el nombre de su aplicación.

```console
func azure functionapp publish <APP_NAME>
```

El comando de publicación muestra un resultado similar a lo que se muestra a continuación (se ha truncado para que resulte más simple):

<pre>
...

Getting site publishing info...
Creating archive for current directory...
Performing remote build for functions project.

...

Deployment successful.
Remote build succeeded!
Syncing triggers...
Functions in msdocs-azurefunctions-qs:
    HttpExample - [httpTrigger]
        Invoke url: https://msdocs-azurefunctions-qs.azurewebsites.net/api/httpexample
</pre>
