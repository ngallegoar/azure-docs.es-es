---
title: Conexión con Project Online desde Azure Logic Apps
description: Automatización de los flujos de trabajo que supervisan, crean y administran proyectos, tareas y recursos de Project Online con Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/24/2018
tags: connectors
ms.openlocfilehash: a3e90fa3e3f57c1575a7ab09f9ce6941c13adcd1
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83834873"
---
# <a name="manage-project-online-projects-tasks-and-resources-by-using-azure-logic-apps"></a>Administración de proyectos, tareas y recursos de Project Online con Azure Logic Apps

Con Azure Logic Apps y el conector Project Online, puede crear tareas automatizadas y flujos de trabajo para sus proyectos, tareas y recursos en Project Online mediante Office 365. Los flujos de trabajo pueden realizar estas y otras acciones, como son:

* Supervisar cuándo se crean nuevos proyectos, tareas o recursos. O bien, supervisar cuándo se publican nuevos proyectos.
* Crear nuevos proyectos, tareas o recursos.
* Enumerar los proyectos o tareas actuales.
* Extraer del repositorio proyectos, insertarlos en el repositorio y publicarlos.

Project Online le ayuda a planear, priorizar y administrar proyectos e inversiones de carteras de proyectos, desde prácticamente cualquier lugar en casi todos los dispositivos gracias a sus eficaces funcionalidades de administración de proyectos. Puede usar desencadenadores de Project Online que obtengan respuestas de Project Online y hagan que la salida esté disponible para otras acciones. Puede utilizar acciones en las aplicaciones lógicas para realizar diversas tareas en Project Online. Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/). 

* Project Online, disponible mediante una [cuenta de Office 365](https://www.office.com/). 

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* La aplicación lógica donde quiera acceder a los datos de Project Online. Para comenzar con un desencadenador de Project Online, [cree una aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md). Para usar acciones de Project Online, inicie la aplicación lógica con otro desencadenador, por ejemplo, el de **periodicidad**.

## <a name="connect-to-project-online"></a>Conexión con Project Online

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y abra la aplicación lógica en el diseñador de aplicaciones lógicas, si aún no lo ha hecho.

1. Elija una ruta de acceso: 

   * Para las aplicaciones lógicas en blanco, en el cuadro de búsqueda, escriba "Project Online" como filtro. 
   En la lista de desencadenadores, seleccione el que desee. 

     O bien

   * Para las aplicaciones lógicas existentes, en el paso donde desea agregar una acción, elija **Nuevo paso**. En el cuadro de búsqueda, escriba "Project Online" como filtro. En la lista de acciones, seleccione la que desee.

1. Si se le pide que inicie sesión en Project Online, hágalo ahora.

   Sus credenciales autorizan a la aplicación lógica para crear una conexión con Project Online y acceder a sus datos.

1. Proporcione los detalles necesarios para el desencadenador o la acción seleccionados y continúe con la compilación del flujo de trabajo de la aplicación lógica.

## <a name="connector-reference"></a>Referencia de conectores

Para obtener detalles técnicos acerca de desencadenadores, acciones y límites, que se describen en la descripción de OpenAPI (antes Swagger) del conector, consulte la [página de referencia](/connectors/projectonline/) del conector.

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite la [Página de preguntas y respuestas de Microsoft sobre Azure Logic Apps](https://docs.microsoft.com/answers/topics/azure-logic-apps.html).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)