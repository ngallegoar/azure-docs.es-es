---
title: Conexión con Facebook
description: Automatización de las tareas y los flujos de trabajo que administran la escala de tiempo y la página de Facebook mediante Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 11/07/2016
tags: connectors
ms.openlocfilehash: 803453291b5cab2c51fec6641f8b096f62336325
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75665809"
---
# <a name="manage-your-facebook-timeline-and-page-by-using-azure-logic-apps"></a>Administrar la escala de tiempo y la página de Facebook mediante Azure Logic Apps

Conéctese a Facebook y publique en una biografía, obtenga una fuente de página y mucho más. Con Facebook, puede hacer lo siguiente:

* Compilar el flujo de negocio en función de los datos que obtiene de Facebook. 
* Utilizar un desencadenador cuando se reciba un nuevo mensaje.
* Usar acciones para publicar en la biografía, obtener una fuente de página y mucho más. Estas acciones obtienen una respuesta y luego dejan el resultado a disposición de otras acciones. Por ejemplo, cuando haya un nuevo mensaje en su biografía, puede tomar ese mensaje e insertarlo en su fuente de Twitter. 

Puede empezar creando una aplicación lógica ahora. Para ello, consulte [Creación de una aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-facebook"></a>Creación de una conexión a Facebook

Al agregar este conector a las aplicaciones lógicas, debe autorizar a estas para que se conecten a su Facebook.

1. Inicie sesión en su cuenta de Facebook.

2. Seleccione **Authorize**(Autorizar) y permita que sus aplicaciones lógicas se conecten y utilicen su aplicación de Facebook. 

> [!INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]
> 

## <a name="connector-reference"></a>Referencia de conectores

Para obtener datos técnicos, como los desencadenadores, las acciones y los límites, tal como lo describe el archivo OpenAPI (antes Swagger) del conector, consulte la [página de referencia del conector](/connectors/facebook/).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)