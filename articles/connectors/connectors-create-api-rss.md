---
title: Conexión a fuentes RSS desde Azure Logic Apps
description: Automatice las tareas y los flujos de trabajo que supervisan y administran fuentes RSS mediante Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/24/2018
tags: connectors
ms.openlocfilehash: 1e95b7bfce3d57dfe579a5622c3c13fac2b6af68
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83834822"
---
# <a name="manage-rss-feeds-by-using-azure-logic-apps"></a>Administración de fuentes RSS mediante Azure Logic Apps

Con Azure Logic Apps y el conector RSS, puede crear tareas automatizadas y flujos de trabajo para cualquier fuente RSS, por ejemplo:

* Supervise cuando se publican los elementos de la fuente RSS.
* Enumere todos los elementos de la fuente RSS.

RSS (Rich Site Summary), también llamado Really Simple Syndication, es un formato popular para la redifusión web y se utiliza para publicar contenido actualizado con frecuencia, como entradas de blogs y titulares de noticias. Muchos editores de contenido ofrecen una fuente RSS para que los usuarios se suscriban a dicho contenido. 

Puede utilizar un desencadenador RSS que obtiene respuestas de una fuente RSS y hace que la salida esté disponible para otras acciones. Puede usar una acción de RSS en las aplicaciones lógicas para realizar una tarea con la fuente RSS. Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/). 

* La dirección URL de una fuente RSS

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* La aplicación lógica donde quiere acceder a la fuente RSS. Para comenzar con un desencadenador RSS, [cree una aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md). Para usar una acción de RSS, inicie la aplicación lógica con otro desencadenador, por ejemplo, el desencadenador de **periodicidad**.

## <a name="connect-to-an-rss-feed"></a>Conexión a una fuente RSS

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y abra la aplicación lógica en el diseñador de aplicaciones lógicas, si aún no lo ha hecho.

1. Elija una ruta de acceso: 

   * Para las aplicaciones lógicas en blanco, en el cuadro de búsqueda, escriba "rss" como filtro. En la lista de desencadenadores, seleccione el que desee. 

     O bien

   * Para las aplicaciones lógicas existentes, en el paso donde desea agregar una acción, elija **Nuevo paso**. En el cuadro de búsqueda, escriba "rss" como filtro. En la lista de acciones, seleccione la que desee.

1. Proporcione los detalles necesarios para el desencadenador o la acción seleccionados y continúe con la compilación del flujo de trabajo de la aplicación lógica.

## <a name="connector-reference"></a>Referencia de conectores

Para obtener detalles técnicos acerca de desencadenadores, acciones y límites, que se describen en la descripción de OpenAPI (antes Swagger) del conector, consulte la [página de referencia](/connectors/rss/) del conector.

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite la [página de preguntas y respuestas de Microsoft sobre Azure Logic Apps](https://docs.microsoft.com/answers/topics/azure-logic-apps.html).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)