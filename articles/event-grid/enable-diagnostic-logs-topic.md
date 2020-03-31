---
title: 'Azure Event Grid: Habilitación de los registros de diagnóstico de un tema'
description: En este artículo se proporcionan instrucciones paso a paso sobre cómo habilitar los registros de diagnóstico para un tema de Azure Event Grid.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: how-to
ms.date: 01/30/2020
ms.author: spelluru
ms.openlocfilehash: a32d26629bb9efed4a4f1f49eee2f0534e1873a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76960205"
---
#  <a name="diagnostic-logs-for-an-azure-event-grid-topic"></a>Registros de diagnóstico de un tema de Azure Event Grid
La configuración de diagnóstico permite a los usuarios de Event Grid capturar y ver los registros de error de publicación y entrega en una de las siguientes ubicaciones: una cuenta de almacenamiento de Azure, un centro de eventos o un área de trabajo de Log Analytics. En este artículo se proporcionan instrucciones paso a paso sobre cómo habilitar los registros de diagnóstico para un tema de Event Grid.

## <a name="prerequisites"></a>Prerequisites

- Un tema de Event Grid aprovisionado.
- Un destino aprovisionado para capturar registros de diagnóstico. Puede ser uno de los siguientes destinos:
    - Cuenta de almacenamiento de Azure
    - Centro de eventos
    - Área de trabajo de Log Analytics


## <a name="steps-for-enabling-diagnostic-logs-for-a-topic"></a>Pasos para habilitar los registros de diagnóstico para un tema

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Vaya al tema de Event Grid para el que desee habilitar la configuración del registro de diagnóstico. 
3. Seleccione **Configuración de diagnóstico** en **Supervisión**, en el menú de la izquierda.
4. En la página **Configuración de diagnóstico**, seleccione **Add New Diagnostic Setting** (Agregar nueva configuración de diagnóstico). 
    
    ![Botón Agregar configuración de diagnóstico](./media/enable-diagnostic-logs-topic/diagnostic-settings-add.png)
5. Especifique un **nombre** para la configuración de diagnóstico. 

    ![Configuración de diagnóstico: nombre](./media/enable-diagnostic-logs-topic/diagnostic-settings-name.png)     
6. Habilite uno o varios destinos de captura para los registros y, a continuación, configúrelos; para ello, seleccione un recurso de captura creado anteriormente. 
    - Si selecciona **Archive to a storage account** (Archivar en una cuenta de almacenamiento), seleccione **Cuenta de almacenamiento: configurar** y, a continuación, seleccione la cuenta de almacenamiento en la suscripción de Azure. 

        ![Archivo en una cuenta de almacenamiento de Azure](./media/enable-diagnostic-logs-topic/archive-storage.png)
    - Si selecciona **Stream to an event hub** (Transmitir a un centro de eventos), seleccione **Centro de eventos: Configurar** y, a continuación, seleccione el espacio de nombres de Event Hubs, el centro de eventos y la directiva de acceso. 
        ![Transmisión a un centro de eventos](./media/enable-diagnostic-logs-topic/archive-event-hub.png)
    - Si selecciona **Send to Log Analytics** (Enviar a Log Analytics), seleccione un área de trabajo de Log Analytics.
        ![Enviar a Log Analytics](./media/enable-diagnostic-logs-topic/send-log-analytics.png)
7. Seleccione las opciones **DeliveryFailures** y **PublishFailures** en la sección **Registro**. 
    ![Seleccione los errores](./media/enable-diagnostic-logs-topic/log-failures.png)
8. Seleccione **Guardar**. Seleccione **X** en la esquina derecha para cerrar la página. 
9. Ahora, vuelva a la página **Configuración de diagnóstico**, confirme que ve una nueva entrada en la tabla **Configuración de diagnóstico**. 
    ![Configuración de diagnóstico en la lista](./media/enable-diagnostic-logs-topic/diagnostic-setting-list.png)

     También puede habilitar la recopilación de todas las métricas del tema. 

## <a name="next-steps"></a>Pasos siguientes
Si necesita más ayuda, publique su problema en el [foro de Stack Overflow](https://stackoverflow.com/questions/tagged/azure-eventgrid) o abra una [incidencia de soporte técnico](https://azure.microsoft.com/support/options/). 
