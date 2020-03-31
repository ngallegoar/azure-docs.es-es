---
title: Conexión con Box
description: Automatice las tareas y los flujos de trabajo que crean y administran archivos en Box mediante Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 11/07/2016
tags: connectors
ms.openlocfilehash: c7f97ff33742eb545cbfbd7521ba135584851e5e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75666778"
---
# <a name="create-and-manage-files-in-box-by-using-azure-logic-apps"></a>Creación y administración de archivos en Box mediante Azure Logic Apps

En este artículo se muestra cómo puede crear y administrar archivos de Box dentro de una aplicación lógica con el conector de Box. De este modo, puede crear aplicaciones lógicas que automatizan tareas y flujos de trabajo para administrar los archivos y otras acciones, como por ejemplo:

* Compilar el flujo de negocio en función de los datos que obtiene de Box.

* Desencadenar tareas y flujos de trabajo automatizados cuando se crea o actualiza un archivo.

* Ejecutar una acción que copia o elimina un archivo.

  Si estas acciones obtienen una respuesta, dejan la salida a disposición de otras acciones. 
  Por ejemplo, cuando se modifique un archivo en Box, puede enviarlo por correo electrónico mediante Office 365.

## <a name="prerequisites"></a>Prerrequisitos

* Una [cuenta de Box](https://www.box.com/home)

* Suscripción a Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/). 

* La aplicación lógica desde donde quiere acceder a la cuenta de Box. Para iniciar la aplicación lógica con un desencadenador de Box, necesita una [aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Si no está familiarizado con las aplicaciones lógicas, vea [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="connector-reference"></a>Referencia de conectores

Para obtener datos técnicos, como los desencadenadores, las acciones y los límites, tal como lo describe el archivo OpenAPI (antes Swagger) del conector, consulte la [página de referencia del conector](/connectors/box/).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)