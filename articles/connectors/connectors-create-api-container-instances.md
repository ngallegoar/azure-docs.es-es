---
title: Implementación y administración de Azure Container Instances
description: Automatización de tareas y flujos de trabajo que crean y administran implementaciones de contenedores en Azure Container Instances mediante Azure Logic Apps
services: logic-apps, container-instances
ms.service: logic-apps
ms.suite: integration
author: dlepow
ms.author: danlep
ms.manager: gwallace
ms.reviewer: estfan, macolso
ms.topic: article
tags: connectors
ms.date: 01/14/2020
ms.openlocfilehash: ecb1049d64197f2a60438df7eedfb244907f7327
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "76046206"
---
# <a name="deploy-and-manage-azure-container-instances-by-using-azure-logic-apps"></a>Implementación y administración de Azure Container Instances mediante Azure Logic Apps

Con Azure Logic Apps y el conector de Azure Container Instances, puede configurar tareas y flujos de trabajo automatizados que implementan y administran [grupos de contenedores](../container-instances/container-instances-container-groups.md). El conector de Container Instances admite las siguientes acciones:

* Creación o eliminación de un grupo de contenedores
* Obtención de las propiedades de un grupo de contenedores
* Obtención de una lista de grupos de contenedores
* Obtención de los registros de una instancia de contenedor

Use estas acciones en las aplicaciones lógicas para tareas como la ejecución de una carga de trabajo de contenedor en respuesta a un desencadenador de Logic Apps. También puede hacer que otras acciones usen la salida de las acciones de Container Instances. 

Este conector ofrece únicamente las acciones, por lo que, para iniciar la aplicación lógica, use un desencadenador independiente, como por ejemplo **Recurrence** para ejecutar la carga de trabajo de un contenedor con regularidad. O bien, puede que tenga que desencadenar una implementación del grupo de contenedores después de un evento como la llegada de un correo electrónico de Outlook. 

Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Prerequisites

* Suscripción a Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/). 

* Conocimientos básicos sobre [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md) y [cómo crear y administrar instancias de contenedor](../container-instances/container-instances-quickstart.md)

* La aplicación lógica desde donde quiere acceder a las instancia de contenedor. Para usar una acción, inicie la aplicación lógica con otro desencadenador, por ejemplo, el desencadenador **Recurrence**.

## <a name="add-a-container-instance-action"></a>Adición de una acción de instancia de contenedor

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y abra la aplicación lógica en el diseñador de aplicaciones lógicas, si aún no lo ha hecho.

1. Elija una ruta de acceso: 

   * En el último paso para agregar una acción, elija **Nuevo paso**. 

     O bien

   * Entre los pasos en los que desee agregar una acción, mueva el puntero sobre la flecha. 
   Elija el signo más ( **+** ) que aparece y seleccione **Agregar una acción**.

1. En el cuadro de búsqueda, escriba "instancia de contenedor" como filtro. En la lista de acciones, seleccione la acción del conector de Azure Container Instances que desee.

1. Proporcione un nombre para la conexión. 

1. Proporcione los detalles necesarios para la acción seleccionada y continúe con la creación del flujo de trabajo de la aplicación lógica.

  Por ejemplo, seleccione **Crear un grupo de contenedores** y escriba las propiedades de un grupo de contenedores y una o varias instancias de contenedor en el grupo, tal como se muestra en la siguiente imagen (detalle parcial):

  ![Creación de un grupo de contenedores](./media/connectors-create-api-container-instances/logic-apps-aci-connector.png)

## <a name="connector-reference"></a>Referencia de conectores

Para obtener información técnica acerca de los desencadenadores, las acciones y los límites, que se detallan en la descripción de OpenAPI (antes Swagger) del conector, consulte la [página de referencia](/connectors/aci/) de los conectores o la [referencia de YAMLº](../container-instances/container-instances-reference-yaml.md) del grupo de contenedores.

## <a name="next-steps"></a>Pasos siguientes

* Vea una [aplicación lógica de ejemplo](https://github.com/Azure-Samples/aci-logicapps-integration) que ejecuta un contenedor en Azure Container Instances para analizar la opinión del correo electrónico o del texto de Twitter.

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)