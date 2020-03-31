---
title: Migración de aplicaciones lógicas entre suscripciones, grupos de recursos o regiones
description: Migración de aplicaciones lógicas o cuentas de integración a otras suscripciones de Azure, grupos de recursos o ubicaciones (regiones)
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 07/31/2019
ms.openlocfilehash: f5944accb185f1311c811cf65a8ea8348fd569db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77605610"
---
# <a name="move-logic-app-resources-to-other-azure-subscriptions-resource-groups-or-regions"></a>Traslado de recursos de aplicaciones lógicas a otras suscripciones, grupos de recursos o regiones de Azure

Para migrar la aplicación lógica o los recursos relacionados a otra suscripción, grupo de recursos o región de Azure, tiene varias formas de completar estas tareas, como Azure Portal, Azure PowerShell, CLI de Azure y API REST. Antes de trasladar recursos, revise estas consideraciones: 

* Solo puede trasladar [tipos de recursos específicos de la aplicación lógica](../azure-resource-manager/management/move-support-resources.md#microsoftlogic) entre las suscripciones o los grupos de recursos de Azure.

* Compruebe los [límites](../logic-apps/logic-apps-limits-and-config.md) en el número de recursos de la aplicación lógica que puede tener en su suscripción de Azure y en cada región de Azure. Estos límites afectan a si puede trasladar tipos de recursos específicos cuando la región permanece invariable en las suscripciones o en los grupos de recursos. Por ejemplo, solo puede tener una cuenta de integración de nivel Gratis para cada región de Azure en cada suscripción de Azure.

* Después de migrar las aplicaciones lógicas entre las suscripciones, los grupos de recursos o las regiones, debe volver a crear o a autorizar las conexiones que requieran autenticación abierta (OAuth).

* Cada vez que traslada recursos, Azure crea identificadores de recursos nuevos. Por tanto, asegúrese de usar los nuevos identificadores en su lugar y actualizar los scripts o las herramientas asociados a los recursos que ha movido.

## <a name="prerequisites"></a>Prerrequisitos

* La misma suscripción de Azure que se usó para crear la aplicación lógica o la cuenta de integración que desea trasladar

* Permisos de propietario de recursos para trasladar y configurar los recursos que desee. Más información sobre el [control de acceso basado en rol (RBAC)](../role-based-access-control/built-in-roles.md#owner).

<a name="move-subscription"></a>

## <a name="move-resources-between-subscriptions"></a>Migración de recursos entre suscripciones

Para trasladar un recurso, como una aplicación lógica o una cuenta de integración, a otra suscripción de Azure, puede usar Azure Portal, Azure PowerShell, CLI de Azure o la API REST. Estos pasos detallan Azure Portal, que puede usar cuando la región del recurso se mantiene igual. Para otros pasos y preparación general, consulte [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](../azure-resource-manager/management/move-resource-group-and-subscription.md).

1. En [Azure Portal](https://portal.azure.com), busque y seleccione el recurso de aplicación lógica que desea trasladar.

1. En la página **Información general** del recurso, junto a **Suscripción**, seleccione el vínculo **cambiar**.

1. En la página **Mover recursos**, seleccione el recurso de aplicación lógica y los recursos relacionados que desee trasladar.

1. En la lista **Suscripción**, seleccione la suscripción de destino.

1. En la lista **Grupo de recursos**, seleccione el grupo de recursos de destino. O bien, para crear otro grupo de recursos, seleccione **Crear un nuevo grupo**.

1. Para confirmar que comprende que los scripts o las herramientas asociados a los recursos que se han movido no funcionarán hasta que los actualice con los nuevos identificadores de recurso, seleccione el cuadro de confirmación y después seleccione **Aceptar**.

<a name="move-resource-group"></a>

## <a name="move-resources-between-resource-groups"></a>Traslado de recursos entre grupos de recursos

Para trasladar un recurso, como una aplicación lógica o una cuenta de integración, a otro grupo de recursos de Azure, puede usar Azure Portal, Azure PowerShell, CLI de Azure o la API REST. Estos pasos detallan Azure Portal, que puede usar cuando la región del recurso se mantiene igual. Para otros pasos y preparación general, consulte [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](../azure-resource-manager/management/move-resource-group-and-subscription.md).

Antes de trasladar realmente los recursos entre grupos, puede comprobar si puede trasladar correctamente el recurso a otro grupo. Para más información, consulte [Validación del traslado](../azure-resource-manager/management/move-resource-group-and-subscription.md#validate-move).

1. En [Azure Portal](https://portal.azure.com), busque y seleccione el recurso de aplicación lógica que desea trasladar.

1. En la página **Información general** del recurso, junto a **Grupo de recursos**, seleccione el vínculo **cambiar**.

1. En la página **Mover recursos**, seleccione el recurso de aplicación lógica y los recursos relacionados que desee trasladar.

1. En la lista **Grupo de recursos**, seleccione el grupo de recursos de destino. O bien, para crear otro grupo de recursos, seleccione **Crear un nuevo grupo**.

1. Para confirmar que comprende que los scripts o las herramientas asociados a los recursos que se han movido no funcionarán hasta que los actualice con los nuevos identificadores de recurso, seleccione el cuadro de confirmación y después seleccione **Aceptar**.

<a name="move-location"></a>

## <a name="move-resources-between-regions"></a>Traslado de recursos entre regiones

Si quiere trasladar una aplicación lógica a otra región, las opciones dependen de la forma en que se ha creado la aplicación lógica. En función de la opción que elija, debe volver a crear o a autorizar las conexiones de la aplicación lógica.

* En Azure Portal, vuelva a crear la aplicación lógica en la nueva región y vuelva a configurar la configuración del flujo de trabajo. Para ahorrar tiempo, puede copiar la definición del flujo de trabajo subyacente y las conexiones desde la aplicación de origen a la aplicación de destino. Para ver el "código" detrás de una aplicación lógica, en la barra de herramientas del diseñador de aplicación lógica, seleccione **Vista Código**.

* Con Visual Studio y las Herramientas de Azure Logic Apps para Visual Studio, puede [abrir y descargar la aplicación lógica](../logic-apps/manage-logic-apps-with-visual-studio.md) desde Azure Portal como una [plantilla de Azure Resource Manager](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md). Esta plantilla está lista principalmente para la implementación e incluye las definiciones de recursos para la aplicación lógica, incluido el mismo flujo de trabajo y las conexiones. La plantilla también declara los parámetros de los valores que se van a usar en la implementación. De este modo, puede cambiar más fácilmente dónde y cómo implementar la aplicación lógica, en función de sus necesidades. Para especificar la ubicación y otra información necesaria para la implementación, puede usar un archivo de parámetros independiente.

* Si ha creado e implementado la aplicación lógica mediante herramientas de integración continua (CI) y entrega continua (CD), como Azure Pipelines en Azure DevOps, puede implementar la aplicación en otra región mediante esas herramientas.

Para obtener más información sobre las plantillas de implementación de Logic Apps, consulte estos temas:

* [Información general: Automatización de la implementación para Azure Logic Apps mediante plantillas de Azure Resource Manager](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)
* [Búsqueda, apertura y descarga de la aplicación lógica desde Azure Portal en Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md)
* [Creación de plantillas de Azure Resource Manager para Azure Logic Apps](../logic-apps/logic-apps-create-azure-resource-manager-templates.md)
* [Implementación de plantillas de Azure Resource Manager para Azure Logic Apps](../logic-apps/logic-apps-deploy-azure-resource-manager-templates.md)

### <a name="related-resources"></a>Recursos relacionados

Algunos recursos de Azure, como los recursos de puerta de enlace de datos locales en Azure, pueden existir en una región distinta de las aplicaciones lógicas que usan esos recursos. Sin embargo, otros recursos de Azure, como las cuentas de integración vinculadas, deben existir en la misma región que las aplicaciones lógicas. En función del escenario, asegúrese de que las aplicaciones lógicas pueden acceder a los recursos que las aplicaciones esperan que existan en la misma región.

Por ejemplo, para vincular una aplicación lógica a una cuenta de integración, ambos recursos deben existir en la misma región. En escenarios como la recuperación ante desastres, normalmente se quieren cuentas de integración que tengan la misma configuración y artefactos. En otros escenarios, es posible que necesite cuentas de integración con diferentes configuraciones y artefactos.

Los conectores personalizados de Azure Logic Apps están visibles para los autores y usuarios de los conectores que tienen la misma suscripción de Azure y el mismo inquilino de Azure Active Directory. Estos conectores están disponibles en la misma región donde se implementan las aplicaciones lógicas. Para más información, consulte [Uso compartido de conectores personalizados en su organización](https://docs.microsoft.com/connectors/custom-connectors/share).

La plantilla que se obtiene de Visual Studio solo incluye las definiciones de recursos para la aplicación lógica y sus conexiones. Por lo tanto, si la aplicación lógica usa otros recursos, por ejemplo, una cuenta de integración y artefactos B2B, como asociados, acuerdos y esquemas, debe exportar la plantilla de la cuenta de integración mediante Azure Portal. Esta plantilla incluye las definiciones de recursos para la cuenta de integración y los artefactos. Sin embargo, la plantilla no está completamente parametrizada. Por lo tanto, debe parametrizar manualmente los valores que desea usar para la implementación.

### <a name="export-templates-for-integration-accounts"></a>Exportación de plantillas para las cuentas de integración

1. En [Azure Portal](https://portal.azure.com), busque y abra la cuenta de integración.

1. En el menú de la cuenta de integración, en **Configuración**, seleccione **Exportar plantilla**.

1. En la barra de herramientas, seleccione **Descargar** y guarde la plantilla.

1. Abra y edite la plantilla para parametrizar los valores necesarios para la implementación.

## <a name="next-steps"></a>Pasos siguientes

[Traslado de los recursos de Azure a nuevos grupos de recursos o suscripciones](../azure-resource-manager/management/move-resource-group-and-subscription.md)
