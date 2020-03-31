---
title: 'Uso del panel de Connected Factory: Azure | Microsoft Docs'
description: En este artículo se describe cómo usar las características del panel Connected Factory para supervisar y administrar dispositivos IoT industriales.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: dobett
ms.openlocfilehash: b53177d578768428665891704269e63bd8edb09e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73820175"
---
# <a name="use-features-in-the-connected-factory-solution-accelerator-dashboard"></a>Uso de las características del panel del acelerador de soluciones de Connected Factory

En la guía de inicio rápido sobre la [implementación de una solución basada en la nube para administrar dispositivos IoT industriales](quickstart-connected-factory-deploy.md) se mostró cómo navegar por el panel y cómo responder las alarmas. En esta guía de procedimientos se muestran algunas características adicionales del panel que puede usar para supervisar y administrar los dispositivos IoT industriales.

## <a name="apply-filters"></a>Aplicación de filtros

Puede filtrar la información que se muestra en el panel, ya sea en el panel **Factory Locations** (Ubicaciones de fábrica) o en el panel **Alarms** (Alarmas):

1. Haga clic en el icono del **embudo** para mostrar una lista de los filtros disponibles en el panel de ubicaciones de factorías o en el panel de alarmas.

1. Se muestra el panel de filtros:

    [![Filtros del acelerador de soluciones de Connected Factory](./media/iot-accelerators-connected-factory-dashboard/filterpanel-inline.png)](./media/iot-accelerators-connected-factory-dashboard/filterpanel-expanded.png#lightbox)

1. Elija el filtro que necesita y haga clic en **Aplicar**. También puede escribir texto libre en los campos de filtro.

1. A continuación, se aplica el filtro. El icono de embudo adicional indica que se aplica un filtro:

    [![Filtro aplicado del acelerador de soluciones de Connected Factory](./media/iot-accelerators-connected-factory-dashboard/filterapplied-inline.png)](./media/iot-accelerators-connected-factory-dashboard/filterapplied-expanded.png#lightbox)

    > [!NOTE]
    > Un filtro activo no afecta a los valores de OEE y KPI mostrados, solo se filtra el contenido de la lista.

1. Para borrar un filtro, haga clic en el embudo y haga clic en **Clear** (Borrar) en el panel del filtro.

## <a name="browse-an-opc-ua-server"></a>Examinar un servidor de OPC UA

Al implementar el acelerador de soluciones, se aprovisiona automáticamente un conjunto de servidores de agente de usuario de OPC simulados que se pueden examinar desde el panel. Los servidores simulados facilitan la experimentación con el acelerador de soluciones sin necesidad de implementar servidores reales.

1. Haga clic en el **icono del explorador** de la barra de navegación del panel:

    [![Explorador del servidor del acelerador de soluciones de Connected Factory](./media/iot-accelerators-connected-factory-dashboard/browser-inline.png)](./media/iot-accelerators-connected-factory-dashboard/browser-expanded.png#lightbox)

1. Elija uno de los servidores en la lista que muestra los servidores que se implementan en el acelerador de soluciones:

    [![Lista de servidores del acelerador de soluciones de Connected Factory](./media/iot-accelerators-connected-factory-dashboard/serverlist-inline.png)](./media/iot-accelerators-connected-factory-dashboard/serverlist-expanded.png#lightbox)

1. Haga clic en **Conectar**; aparece un cuadro de diálogo de seguridad. Para la simulación, es seguro hacer clic en **Continuar**.

1. Haga clic en cualquiera de los nodos del árbol de servidores para expandirlo. Los nodos que publican telemetría tienen una marca de verificación junto a ellos:

    [![Árbol del servidor del acelerador de soluciones de Connected Factory](./media/iot-accelerators-connected-factory-dashboard/servertree-inline.png)](./media/iot-accelerators-connected-factory-dashboard/servertree-expanded.png#lightbox)

1. Haga clic con el botón derecho para leer, escribir, publicar o llamar a ese nodo. Las acciones disponibles dependen de los permisos y los atributos del nodo. La opción de lectura muestra un panel de contexto que muestra el valor de un nodo específico. La opción de escritura muestra un panel de contexto donde puede escribir un nuevo valor. La opción de llamada muestra un nodo donde puede especificar los parámetros para la llamada.

## <a name="publish-a-node"></a>Publicación de un nodo

Cuando se examina un *servidor de OPC UA simulado*, también puede elegir publicar nuevos nodos. Puede analizar la telemetría de estos nodos en la solución. Estos *servidores de agente de usuario de OPC simulados* facilitan la experimentación de la solución preconfigurada sin necesidad de implementar dispositivos reales:

1. Vaya a un nodo en el árbol del explorador de servidores de OPC UA que desea publicar.

1. Haga clic con el botón derecho en el nodo. Haga clic en **Publicar**:

    [![Nodo de publicación del acelerador de soluciones de Connected Factory](./media/iot-accelerators-connected-factory-dashboard/publishnode-inline.png)](./media/iot-accelerators-connected-factory-dashboard/publishnode-expanded.png#lightbox)

1. Aparece un panel de contexto que le indica que la publicación se ha realizado correctamente. El nodo aparece en la vista de nivel de estación con una marca de verificación junto a él:

    [![Publicación correcta del acelerador de soluciones de Connected Factory](./media/iot-accelerators-connected-factory-dashboard/publishsuccess-inline.png)](./media/iot-accelerators-connected-factory-dashboard/publishsuccess-expanded.png#lightbox)

## <a name="command-and-control"></a>Comando y control

Fábrica conectada permite tanto dar órdenes a los dispositivos como controlarlos directamente desde la nube. Esta característica se puede usar para responder a las alarmas generadas por el dispositivo. Por ejemplo, podría enviar un comando al dispositivo para abrir una válvula de despresurización. Puede encontrar los comandos disponibles en el nodo **StationCommands** del árbol del explorador de servidores de OPC UA. En este escenario, se abre una válvula de despresurización en la estación de ensamblado de una línea de producción en Munich. Para usar la funcionalidad de comando y control, debe estar en el rol **Administrador** de la implementación del acelerador de soluciones:

1. Vaya al nodo **StationCommands** en el árbol del navegador del servidor del agente de usuario de OPC para la línea de producción 0 de la estación de ensamblado de Múnich.

1. Elija el comando que desea usar. Haga clic con el botón derecho en el nodo **OpenPressureReleaseValve**. Haga clic en **Call** (Llamar):

    [![Comando de llamada del acelerador de soluciones de Connected Factory](./media/iot-accelerators-connected-factory-dashboard/callcommand-inline.png)](./media/iot-accelerators-connected-factory-dashboard/callcommand-expanded.png#lightbox)

1. Aparece un panel de contexto que le informa del método al que va a llamar, y de cualquier detalle de los parámetros. Haga clic en **Call** (Llamar):

    [![Parámetros de llamada del acelerador de soluciones de Connected Factory](./media/iot-accelerators-connected-factory-dashboard/callpanel-inline.png)](./media/iot-accelerators-connected-factory-dashboard/callpanel-expanded.png#lightbox)

1. El panel de contexto se actualiza para informarle de que la llamada al método se realizó correctamente. Para comprobar que la llamada se realizó correctamente, lea el valor del nodo de presión que se ha actualizado como resultado de la llamada.

    [![Llamada correcta del acelerador de soluciones de Connected Factory](./media/iot-accelerators-connected-factory-dashboard/callsuccess-inline.png)](./media/iot-accelerators-connected-factory-dashboard/callsuccess-expanded.png#lightbox)

## <a name="behind-the-scenes"></a>Entre bambalinas

Al implementar un acelerador de soluciones, el proceso de implementación crea varios recursos en la suscripción de Azure seleccionada. Dichos recursos se pueden ver en Azure [Portal](https://portal.azure.com). El proceso de implementación crea un **grupo de recursos** cuyo nombre se basa en el nombre que eligió para el acelerador de soluciones:

[![Grupo de recursos del acelerador de soluciones de Connected Factory](./media/iot-accelerators-connected-factory-dashboard/resourcegroup-inline.png)](./media/iot-accelerators-connected-factory-dashboard/resourcegroup-expanded.png#lightbox)

Para ver la configuración de cada recurso, selecciónelo en la lista de recursos del grupo de recursos.

También puede ver el código fuente del acelerador de soluciones en el repositorio [azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory) de GitHub.

Cuando haya terminado, puede eliminar el acelerador de soluciones de la suscripción de Azure en el sitio [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators#dashboard). Este sitio le permite eliminar fácilmente todos los recursos que se aprovisionaron al crear el acelerador de soluciones.

> [!NOTE]
> Para asegurarse de eliminar todo lo relacionado con el acelerador de soluciones, elimínelo en el sitio [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators#dashboard). No elimine el grupo de recursos en el portal.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha implementado un acelerador de soluciones activo, puede continuar la introducción a los aceleradores de soluciones de IoT con la lectura de los artículos siguientes:

* [Configuración del acelerador de soluciones de factoría conectada](iot-accelerators-connected-factory-configure.md)
* [Permisos en el sitio azureiotsolutions.com](iot-accelerators-permissions.md)
