---
title: Soluciones de supervisión como destino en Azure Monitor | Microsoft Docs
description: La selección de destino de las soluciones de supervisión permite limitar las soluciones de supervisión a un conjunto específico de agentes.  En este artículo se describe cómo crear una configuración de ámbito y aplicarla a una solución.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 04/27/2017
ms.openlocfilehash: 5cecf24f4ba086feba5ab87b5752fd665c540dff
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86498685"
---
# <a name="targeting-monitoring-solutions-in-azure-monitor-preview"></a>Soluciones de supervisión como destino en Azure Monitor (versión preliminar)
Cuando se agrega una solución de supervisión a la suscripción, se implementa automáticamente de forma predeterminada en todos los agentes de Windows y Linux conectados al área de trabajo de Log Analytics.  Quizá quiera administrar los costos y limitar la cantidad de datos recopilados para una solución mediante la limitación a un conjunto determinado de agentes.  En este artículo se describe cómo usar **Selección de destino de solución**, que es una característica que permite aplicar un ámbito a sus soluciones.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="how-to-target-a-solution"></a>Cómo seleccionar un destino para una solución
Hay tres pasos para seleccionar un destino para una solución, tal como se describe en las secciones siguientes. 


### <a name="1-create-a-computer-group"></a>1. Crear un grupo de equipos
Especifique los equipos que se van a incluir en un ámbito mediante la creación de un [grupo de equipos](../platform/computer-groups.md) en Azure Monitor.  El grupo de equipos puede basarse en una consulta de registros o importarse desde otros orígenes, como los grupos de Active Directory o WSUS. Como [se describe a continuación](#solutions-and-agents-that-cant-be-targeted), solo los equipos que están conectados directamente a Azure Monitor se incluirán en el ámbito.

Una vez se haya creado el grupo de equipos en el área de trabajo, deberá incluirlo en una configuración de ámbito que se pueda aplicar a una o varias soluciones.
 
 
### <a name="2-create-a-scope-configuration"></a>2. Creación de una configuración de ámbito
 Una **configuración de ámbito** incluye uno o varios grupos de equipos y se puede aplicar a una o varias soluciones. 
 
 Cree una configuración de ámbito mediante el proceso siguiente.  

 1. En Azure Portal, vaya a las **áreas de trabajo de Log Analytics** y seleccione su área de trabajo.
 2. En las propiedades del área de trabajo en **Orígenes de datos del área de trabajo**, seleccione **Configuraciones de ámbito**.
 3. Haga clic en **Agregar** para crear una nueva configuración de ámbito.
 4. Escriba un **nombre** para la configuración de ámbito.
 5. Haga clic en **Seleccionar grupos de equipos**.
 6. Seleccione el grupo de equipos que ha creado y, opcionalmente, cualquier otro grupo que quiera agregar a la configuración.  Haga clic en **Seleccionar**.  
 6. Haga clic en **Aceptar** para crear la configuración del ámbito. 


### <a name="3-apply-the-scope-configuration-to-a-solution"></a>3. Aplique la configuración de ámbito a una solución.
Una vez que tenga una configuración de ámbito, puede aplicarla a una o varias soluciones.  Tenga en cuenta que aunque una sola configuración de ámbito se puede utilizar con varias soluciones, cada solución solo puede utilizar una configuración de ámbito.

Aplique una configuración de ámbito mediante el proceso siguiente.  

 1. En Azure Portal, vaya a las **áreas de trabajo de Log Analytics** y seleccione su área de trabajo.
 2. En las propiedades del área de trabajo seleccione **Soluciones**.
 3. Haga clic en la solución cuyo ámbito desea establecer.
 4. En las propiedades de la solución en **Orígenes de datos del área de trabajo**, seleccione **Selección de destino de solución**.  Si la opción no está disponible, [no es posible establecer el destino de esta solución](#solutions-and-agents-that-cant-be-targeted).
 5. Haga clic en **Agregar configuración de ámbito**.  Si ya ha aplicado una configuración a esta solución, esta opción no estará disponible.  Debe quitar la configuración existente antes de agregar otra.
 6. Haga clic en la configuración de ámbito que ha creado.
 7. Consulte el **estado** de la configuración para asegurarse de que es **Correcto**.  Si el estado indica un error, haga clic en los puntos suspensivos a la derecha de la configuración y seleccione **Editar configuración de ámbito**.

## <a name="solutions-and-agents-that-cant-be-targeted"></a>No se puede establecer el destino de soluciones y agentes.
A continuación se muestran los criterios para agentes y soluciones que no se pueden utilizar con la selección de destino de solución.

- La selección de destino de solución solo se aplica a soluciones que se implementan en agentes.
- La selección de destino de solución solo se aplica a soluciones proporcionadas por Microsoft.  No se aplica a las soluciones [creadas por usted o sus socios](./solutions.md).
- Solo puede filtrar los agentes que se conectan directamente a Azure Monitor.  Las soluciones se implementarán automáticamente en los agentes que forman parte de un grupo de administración de Operations Manager conectado o no están incluidos en una configuración de ámbito.

### <a name="exceptions"></a>Excepciones
La selección de destino de solución no se puede utilizar con las soluciones siguientes, incluso si cumplen con los criterios establecidos.

- Evaluación de estado del agente

## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre las soluciones de supervisión, incluidas las soluciones que están disponibles para instalar en su entorno, en [Add Azure Log Analytics monitoring solutions to your workspace](solutions.md) (Adición de soluciones de supervisión de Azure Log Analytics al área de trabajo).
- Para más información sobre la creación de grupos de equipos, consulte [Computer groups in Azure Monitor log queries](../platform/computer-groups.md) (Grupos de equipos en búsquedas de registros de Azure Monitor).
