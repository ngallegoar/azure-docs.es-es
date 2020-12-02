---
author: vladvino
ms.service: api-management
ms.topic: include
ms.date: 11/09/2018
ms.author: vlvinogr
ms.openlocfilehash: 2bfa356deeede1c16bd5a464ea7081132a67faf6
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96183838"
---
## <a name="append-other-apis"></a>Anexión de otras API

Una API puede constar de varias API expuestas por diferentes servicios, entre las que se incluyen la especificación OpenAPI, una API de SOAP, la característica API Apps de Azure App Service, Azure Function App, Azure Logic Apps y Azure Service Fabric.

![Importación de una API](./media/api-management-append-apis/import.png)

Para anexar una API diferente a la API existente, complete los pasos siguientes. Al importar otra API, las operaciones se anexan a la API actual.

1. Vaya a la instancia de Azure API Management en Azure Portal.
2. Seleccione **API** del menú de la izquierda.
3. Haga clic en **...** junto a la API a la que desea anexar otra API.
4. Seleccione **Importar** en el menú desplegable.
5. Seleccione un servicio desde el que se va a importar una API.