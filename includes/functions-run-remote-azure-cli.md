---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/18/2020
ms.author: glenga
ms.openlocfilehash: d1931614356a313334d712713965346e843a403d
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93424926"
---
## <a name="invoke-the-function-on-azure"></a>Invocación de la función en Azure

Como la función usa un desencadenador HTTP, para invocarla es preciso realizar una solicitud HTTP a su dirección URL en el explorador o con una herramienta como CURL. 

# <a name="browser"></a>[Browser](#tab/browser)

Copie la **dirección URL de invocación** completa que se muestra en la salida del comando de publicación en una barra de direcciones del explorador, y anexe el parámetro de consulta `&name=Functions`. El explorador debe mostrar una salida similar a cuando ejecutó la función localmente.

![La salida de la función ejecutada en Azure en un explorador](./media/functions-run-remote-azure-cli/function-test-cloud-browser.png)

# <a name="curl"></a>[curl](#tab/curl)

Ejecute [`curl`](https://curl.haxx.se/) con la **dirección URL de invocación** , y anexe el parámetro `&name=Functions`. La salida del comando será el texto "Hello Functions".

![La salida de la función ejecutada en Azure en un mediante curl](./media/functions-run-remote-azure-cli/function-test-cloud-curl.png)

---
