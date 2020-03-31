---
title: Conexión a Wunderlist desde Azure Logic Apps
description: Automatice tareas y flujos de trabajo que supervisan y administran listas, tareas, recordatorios y más en la cuenta de Wunderlist mediante Azure Logic Apps.
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/25/2018
tags: connectors
ms.openlocfilehash: 5ac13595bd77238aaede5fa3bdc3a35ef69e8504
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789127"
---
# <a name="monitor-and-manage-wunderlist-by-using-azure-logic-apps"></a>Supervisión y administración de Wunderlist con Azure Logic Apps

Con Azure Logic Apps y el conector Wunderlist, puede crear tareas y flujos de trabajo automatizados que supervisen y administren listas de tareas, tareas, recordatorios y mucho más en su cuenta de Wunderlist, junto con otras acciones, por ejemplo:

* Supervisar cuándo se crean tareas, cuándo vencen las tareas o cuándo tienen lugar los recordatorios
* Crear y administrar listas, notas, tareas, subtareas y mucho más
* Configurar recordatorios
* Obtener listas, tareas, subtareas, recordatorios, archivos, notas, comentarios y más

[Wunderlist](https://www.wunderlist.com/) es un servicio que le ayuda a planear, administrar y finalizar sus proyectos, listas de tareas y tareas, en cualquier dispositivo y en cualquier parte. Puede usar desencadenadores que obtengan respuestas de su cuenta de Wunderlist y permitan que la salida esté disponible para otras acciones. Puede usar acciones que realizan tareas con su cuenta de Wunderlist. También puede hacer que otras acciones usen la salida de las acciones de Wunderlist. Por ejemplo, cuando vencen nuevas tareas, puede publicar mensajes con el conector Slack. Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/). 

* Las credenciales de usuario y la cuenta de Wunderlist

   Las credenciales autorizan a la aplicación lógica a crear una conexión con la cuenta de Wunderlist y acceder a ella.

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* La aplicación lógica desde donde quiere acceder a la cuenta de Yammer. Para comenzar con un desencadenador de Wunderlist, [cree una aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md). Para usar una acción de Wunderlist, inicie la aplicación lógica con otro desencadenador, por ejemplo, **Recurrence**.

## <a name="connect-to-wunderlist"></a>Conexión a Wunderlist

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y abra la aplicación lógica en el diseñador de aplicaciones lógicas, si aún no lo ha hecho.

1. Elija una ruta de acceso: 

   * Para las aplicaciones lógicas en blanco, en el cuadro de búsqueda, escriba "wunderlist" como filtro. 
   En la lista de desencadenadores, seleccione el que desee. 

     O bien

   * Para las aplicaciones lógicas existentes: 
   
     * En el último paso para agregar una acción, elija **Nuevo paso**. 

       O bien

     * Entre los pasos en los que desee agregar una acción, mueva el puntero sobre la flecha. 
     Elija el signo más ( **+** ) que aparece y seleccione **Agregar una acción**.
     
       En el cuadro de búsqueda, escriba "wunderlist" como filtro. 
       En la lista de acciones, seleccione la que desee.

1. Si se le pide que inicie sesión en Wunderlist, inicie sesión ahora para permitir el acceso.

1. Proporcione los detalles necesarios para el desencadenador o la acción seleccionados y continúe con la compilación del flujo de trabajo de la aplicación lógica.

## <a name="connector-reference"></a>Referencia de conectores

Para obtener detalles técnicos acerca de desencadenadores, acciones y límites, que se describen en la descripción de OpenAPI (antes Swagger) del conector, consulte la [página de referencia](/connectors/wunderlist/) del conector.

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)