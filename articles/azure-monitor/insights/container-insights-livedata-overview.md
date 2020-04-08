---
title: Ver datos en directo (versión preliminar) con Azure Monitor para contenedores | Microsoft Docs
description: En este artículo se describe la vista en tiempo real de los registros de Kubernetes, eventos y métricas de pod sin usar kubectl en Azure Monitor para contenedores.
ms.topic: conceptual
ms.date: 10/15/2019
ms.openlocfilehash: 9e7c7a7b7bf276b3451cee1d289b8b07ac0f40ba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79216560"
---
# <a name="how-to-view-kubernetes-logs-events-and-pod-metrics-in-real-time"></a>Cómo ver los registros de Kubernetes, eventos y métricas de pods en tiempo real

Azure Monitor para contenedores incluye la característica de datos en directo (versión preliminar), que es una característica de diagnóstico avanzada que le permite el acceso directo a los registros de contenedor de Azure Kubernetes Service (AKS) (stdout/errorestándar), eventos y métricas de pods. Expone acceso directo a `kubectl logs -c`, eventos `kubectl get` y `kubectl top pods`. En este panel se muestra los registros, los eventos y las métricas generados por el motor de contenedores para ayudar a solucionar problemas en tiempo real.

En este artículo se proporciona una introducción detallada y le ayuda a entender cómo usar esta característica. 

>[!NOTE]
>No se admiten los clústeres de AKS habilitados como [clústeres privados](https://azure.microsoft.com/updates/aks-private-cluster/) con esta característica. Esta característica se basa en el acceso directo a la API de Kubernetes a través de un servidor proxy desde el explorador. La habilitación de la seguridad de red para bloquear la API de Kubernetes desde este proxy bloqueará este tráfico. 

>[!NOTE]
>Esta característica está disponible en todas las regiones de Azure, incluida Azure China. Actualmente no está disponible en Azure US Gov

Para obtener ayuda para configurar o solucionar problemas de la característica de datos en directo (versión preliminar), revise la [guía de configuración](container-insights-livedata-setup.md). Esta característica accede directamente a la API Kubernetes y se puede encontrar información adicional sobre el modelo de autenticación [aquí](https://kubernetes.io/docs/concepts/overview/kubernetes-api/). 

## <a name="live-data-preview-functionality-overview"></a>Información general sobre la funcionalidad de datos en directo (versión preliminar)

### <a name="search"></a>Search

![Ejemplo de filtro del panel de la consola de datos en directo](./media/container-insights-livedata-overview/livedata-pane-filter-example.png)

La característica de datos en directo (versión preliminar) incluye la funcionalidad de búsqueda. En el campo **Búsqueda**, puede filtrar los resultados escribiendo una palabra clave o término, los resultados coincidentes se resaltan para permitir una revisión rápida. Durante la visualización de eventos, puede limitar además los resultados mediante la cápsula **Filtro** que se encuentra a la derecha de la barra de búsqueda. Dependiendo de qué recurso haya seleccionado, la cápsula muestra un pod, un espacio de nombres o un clúster para elegir.  

### <a name="scroll-lock-and-pause"></a>Desplácese, bloquee y pause 

Para suspender el desplazamiento automático y controlar el comportamiento del panel, de forma que pueda desplazarse manualmente por lo nuevos datos leídos, haga clic en la opción **Desplazar**. Para volver a habilitar el desplazamiento automático, vuelva a hacer clic en la opción **Desplazar**. También puede pausar la recuperación de datos de registros o eventos si hace clic en la opción **Pausar**; cuando esté listo para reanudar, simplemente haga clic en **Reproducir**.  

![Vista activa de la pausa en el panel de la consola de datos en directo](./media/container-insights-livedata-overview/livedata-pane-scroll-pause-example.png)

>[!IMPORTANT]
>Se recomienda suspender o pausar el desplazamiento automático solo durante un breve período de tiempo mientras se soluciona un problema. Estas solicitudes pueden afectar la disponibilidad y limitación de la API de Kubernetes en el clúster. 

>[!IMPORTANT]
>Durante el funcionamiento de esta característica, no se almacenan datos de forma permanente. Toda la información capturada durante la sesión se elimina al cerrar el explorador o salir de ella. Los datos solo permanecen presentes para su visualización en el período de cinco minutos de la característica de métricas; también se eliminan las métricas con una antigüedad superior a cinco minutos. El búfer de datos en directo (vista preliminar) se consulta dentro de los límites de uso de memoria razonables (debe ser más específico aquí, ¿qué es razonable?). 

## <a name="view-logs"></a>Ver registros

Puede ver los datos de registro en tiempo real cuando se generan con el motor de contenedores en la vista **Nodos**, **Controladores** y **Contenedores**. Para comprobar los datos de registro, haga lo siguiente.

1. En el Azure Portal, vaya al grupo de recursos de clúster de AKS y elija el recurso de AKS.

2. En el panel del clúster de AKS, en **Supervisión** a la izquierda, elija **Insights**. 

3. Elija la pestaña **Nodos**, **Controladores** o **Contenedores**.

4. Elija un objeto de la cuadrícula de rendimiento y, en el panel de propiedades que se encuentra en el lado derecho, seleccione la opción **Ver datos en directo (vista preliminar)** . Si el clúster de AKS se configura con inicio de sesión único mediante Azure AD, deberá autenticarse en el primer uso durante esa sesión del explorador. Seleccione la cuenta y complete la autenticación con Azure.  

    >[!NOTE]
    >Al ver los datos desde el área de trabajo de Log Analytics seleccionando la opción **Vista en Analytics** en el panel Propiedades, los resultados de la búsqueda de registros podrían mostrar **nodos**, **conjuntos de demonio** **conjuntos de réplicas**, **trabajos**, **trabajos de Cron**, **pods** y **contenedores** que ya no existen. También se producirá un error al intentar buscar registros de un contenedor que no está disponible en `kubectl`. Revise la característica [Vista en Analytics](container-insights-log-search.md#search-logs-to-analyze-data) para más información sobre cómo ver registros históricos, eventos y métricas.  

Después de autenticarse correctamente, el panel de consola de datos en directo (vista preliminar) aparecerá debajo de la cuadrícula de datos de rendimiento, donde puede ver los datos de registro en un flujo continuo. Si el indicador de estado de captura muestra una marca de verificación verde, que se encuentra en el extremo derecho del panel, significa que pueden recuperar los datos y comienzan a transmitirse a la consola.  

![Node properties pane view data option](./media/container-insights-livedata-overview/node-properties-pane.png)  

El título del panel muestra el nombre del pod con el que se agrupa el contenedor.

## <a name="view-events"></a>Ver eventos

Puede ver los datos de eventos en tiempo real a medida que los genera el motor de contenedor desde **Nodos**, **Controladores**, **Contenedores** e **Implementaciones (vista preliminar)** cuando un contenedor, pod, nodo, ReplicaSet, DaemonSet, Job, CronJob o implementación está seleccionado. Para ver los eventos, haga lo siguiente.

1. En el Azure Portal, vaya al grupo de recursos de clúster de AKS y elija el recurso de AKS.

2. En el panel del clúster de AKS, en **Supervisión** a la izquierda, elija **Insights**. 

3. Elija la pestaña **Nodos**, **Controladores**, **Contenedores** o **Implementaciones (versión preliminar)** .

4. Elija un objeto de la cuadrícula de rendimiento y, en el panel de propiedades que se encuentra en el lado derecho, seleccione la opción **Ver datos en directo (vista preliminar)** . Si el clúster de AKS se configura con inicio de sesión único mediante Azure AD, deberá autenticarse en el primer uso durante esa sesión del explorador. Seleccione la cuenta y complete la autenticación con Azure.  

    >[!NOTE]
    >Al ver los datos desde el área de trabajo de Log Analytics seleccionando la opción **Vista en Analytics** en el panel Propiedades, los resultados de la búsqueda de registros podrían mostrar **nodos**, **conjuntos de demonio** **conjuntos de réplicas**, **trabajos**, **trabajos de Cron**, **pods** y **contenedores** que ya no existen. También se producirá un error al intentar buscar registros de un contenedor que no está disponible en `kubectl`. Revise la característica [Vista en Analytics](container-insights-log-search.md#search-logs-to-analyze-data) para más información sobre cómo ver registros históricos, eventos y métricas.  

Después de autenticarse correctamente, el panel de consola de datos en directo (vista preliminar) aparecerá debajo de la cuadrícula de datos de rendimiento. Si el indicador de estado de captura muestra una marca de verificación verde, que se encuentra en el extremo derecho del panel, significa que pueden recuperar los datos y comienzan a transmitirse a la consola. 
    
Si el objeto que seleccionó era un contenedor, elija la opción **Eventos** en el panel. Si ha seleccionado un nodo, pod o controlador, la visualización de eventos se selecciona automáticamente. 

![Eventos de visualización del panel de propiedades del controlador](./media/container-insights-livedata-overview/controller-properties-live-event.png)  

El título del panel muestra el nombre del pod con el que se agrupa el contenedor.

### <a name="filter-events"></a>Filtrado de eventos 

Durante la visualización de eventos, puede limitar además los resultados mediante la cápsula **Filtro** que se encuentra a la derecha de la barra de búsqueda. Dependiendo de qué recurso haya seleccionado, la cápsula muestra un pod, un espacio de nombres o un clúster para elegir.  

## <a name="view-metrics"></a>Visualización de métricas 

Puede ver los datos de la métrica en tiempo real cuando se generan con el motor de contenedores en la vista **Nodos** o **Controladores** solo cuando un **pod** está seleccionado. Para ver las métricas, haga lo siguiente.

1. En el Azure Portal, vaya al grupo de recursos de clúster de AKS y elija el recurso de AKS.

2. En el panel del clúster de AKS, en **Supervisión** a la izquierda, elija **Insights**. 

3. Elija la pestaña **Nodos** o **Controladores**.

4. Elija un objeto **pod** de la cuadrícula de rendimiento y, en el panel de propiedades que se encuentra en el lado derecho, elija la opción **Ver datos en directo (vista preliminar)** . Si el clúster de AKS se configura con inicio de sesión único mediante Azure AD, deberá autenticarse en el primer uso durante esa sesión del explorador. Seleccione la cuenta y complete la autenticación con Azure.  

    >[!NOTE]
    >Al ver los datos desde el área de trabajo de Log Analytics seleccionando la opción **Vista en Analytics** en el panel Propiedades, los resultados de la búsqueda de registros podrían mostrar **nodos**, **conjuntos de demonio** **conjuntos de réplicas**, **trabajos**, **trabajos de Cron**, **pods** y **contenedores** que ya no existen. También se producirá un error al intentar buscar registros de un contenedor que no está disponible en `kubectl`. Revise la característica [Vista en Analytics](container-insights-log-search.md#search-logs-to-analyze-data) para más información sobre cómo ver registros históricos, eventos y métricas.  

Después de autenticarse correctamente, el panel de consola de datos en directo (vista preliminar) aparecerá debajo de la cuadrícula de datos de rendimiento. Los datos de métricas se recuperan y comienzan a transmitirse a la consola para su presentación en los dos gráficos. El título del panel muestra el nombre del pod con el que se agrupa el contenedor.

![Ver ejemplo de métricas pod](./media/container-insights-livedata-overview/pod-properties-live-metrics.png)  

## <a name="next-steps"></a>Pasos siguientes

- Para continuar aprendiendo a usar Azure Monitor y supervisar otros aspectos de su clúster de AKS, consulte [Comprender el rendimiento del clúster de AKS con Azure Monitor para contenedores](container-insights-analyze.md).

- Vea [ejemplos de consultas de registro](container-insights-log-search.md#search-logs-to-analyze-data) para ver ejemplos y consultas predefinidas para crear alertas, visualizaciones o realizar análisis adicionales de los clústeres.
