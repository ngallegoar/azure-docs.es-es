---
title: Conectar datos de actividad de Azure a Azure Sentinel | Microsoft Docs
description: Transmita los registros de actividad de Azure a Azure Sentinel con tan solo un clic. El registro de actividad registra y muestra los eventos a nivel de suscripción en Azure.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 8c25baa8-b93b-41da-9e6c-15bb7b5c5511
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/22/2020
ms.author: yelevin
ms.openlocfilehash: 88f363406ec40dfa439a52ad351501d1c64dce95
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85564498"
---
# <a name="connect-data-from-azure-activity-log"></a>Conectar datos del registro de actividad de Azure

Se pueden transmitir registros desde el [registro de actividad de Azure](../azure-monitor/platform/platform-logs-overview.md) a Azure Sentinel con tan solo un clic. El registro de actividad es un registro de las suscripciones que graba y muestra los eventos de nivel de suscripción en Azure, desde los datos operativos de Azure Resource Manager hasta las actualizaciones de eventos de Service Health. Con el registro de actividades, se pueden determinar los interrogantes "qué, quién y cuándo" de las operaciones de escritura (PUT, POST, DELETE) que se realizan en los recursos de la suscripción. También puede obtener más información sobre el estado de la operación y otras propiedades pertinentes. El registro de actividad no incluye las operaciones de lectura (GET) ni las operaciones de los recursos que usan el modelo Clásico/"RDFE". 

## <a name="prerequisites"></a>Requisitos previos

- El usuario debe tener permisos de colaborador en el área de trabajo de Log Analytics.
- El usuario debe tener permisos de lector en cualquiera de las suscripciones cuyos registros quiere transmitir a Azure Sentinel.

## <a name="set-up-the-azure-activity-connector"></a>Configuración del conector de actividad de Azure

1. En el menú de navegación de Azure Sentinel, seleccione **Data connectors** (Conectores de datos). En la lista de conectores, haga clic en **Actividad de Azure** y, a continuación, en el botón **Open connector page** (Abrir página del conector) en la parte inferior derecha.

2. En la pestaña **Instrucciones**, haga clic en el vínculo **Configurar los registros de actividad de Azure >** .

3. En el panel **Registro de actividad de Azure**, seleccione las suscripciones cuyos registros quiere transmitir a Azure Sentinel. 

4. En el panel de suscripción que se abre a la derecha, haga clic en **Conectar**.

5. Si quiere usar el esquema correspondiente en Log Analytics para las alertas de actividad de Azure, escriba `AzureActivity` en la ventana de consulta.

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar el registro de actividad de Azure a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a detectar amenazas con Azure Sentinel mediante las reglas [integradas](tutorial-detect-threats-built-in.md) o [personalizadas](tutorial-detect-threats-custom.md).
