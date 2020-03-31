---
title: Supervisión del estado del clúster de Kubernetes con Azure Monitor para contenedores | Microsoft Docs
description: En este artículo se describe cómo puede ver y analizar el estado de los clústeres de AKS y que no es AKS con Azure Monitor para contenedores.
ms.topic: conceptual
ms.date: 12/01/2019
ms.openlocfilehash: f50ef13efca78bbb5285b99759b8111dc1915ad0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76843997"
---
# <a name="understand-kubernetes-cluster-health-with-azure-monitor-for-containers"></a>Descripción del estado del clúster de Kubernetes con Azure Monitor para contenedores

Con Azure Monitor para contenedores, supervisa y notifica el estado de mantenimiento de los componentes de infraestructura administrados y todos los nodos que se ejecutan en cualquier clúster de Kubernetes compatible con Azure Monitor para contenedores. Esta experiencia se extiende más allá del estado de mantenimiento del clúster calculado e incluido en la [vista de varios clústeres](container-insights-analyze.md#multi-cluster-view-from-azure-monitor), donde ahora se puede saber si uno o más nodos del clúster tienen restricciones de recursos, o si un nodo o pod no está disponible, lo que podría afectar a una aplicación en ejecución en el clúster basada en las métricas seleccionadas.

>[!NOTE]
>La característica Estado está en versión preliminar pública en este momento.
>

Para obtener información acerca de cómo habilitar Azure Monitor para contenedores, consulte el artículo sobre [incorporación de Azure Monitor para contenedores](container-insights-onboard.md).

>[!NOTE]
>Para admitir clústeres de AKS Engine, compruebe que cumplen con lo siguiente:
>- Está usando la versión más reciente del [cliente de HELM](https://helm.sh/docs/using_helm/).
>- La versión de agente en contenedor es *microsoft/oms:ciprod11012019*. Para actualizar el agente, consulte [Actualización del agente en un clúster de Kubernetes](container-insights-manage-agent.md#upgrade-agent-on-monitored-kubernetes-cluster).
>

## <a name="overview"></a>Información general

En Azure Monitor para contenedores, la característica Estado (versión preliminar) proporciona una supervisión de estado proactiva de su clúster de Kubernetes para ayudarle a identificar y diagnosticar problemas. Le ofrece la posibilidad de ver los problemas importantes detectados. Los monitores que evalúan el estado del clúster se ejecutan en el agente en contenedor del clúster, mientras que los datos de estado se escriben en la tabla de **KubeHealth** en el área de trabajo de Log Analytics. 

El estado del clúster de Kubernetes se basa en una serie de escenarios de supervisión organizados por los siguientes objetos y abstracciones de Kubernetes:

- Infraestructura de Kubernetes: proporciona un resumen del servidor de API de Kubernetes, ReplicaSets y DaemonSets que se ejecutan en los nodos implementados en el clúster mediante la evaluación del uso de la CPU y la memoria, así como la disponibilidad de los pods

    ![Vista de acumulación de estado de infraestructura de Kubernetes](./media/container-insights-health/health-view-kube-infra-01.png)

- Nodos: proporciona un resumen de los grupos de nodos y el estado de los nodos individuales en cada grupo, mediante la evaluación del uso de la CPU y la memoria, así como el estado de un nodo notificado por Kubernetes.

    ![Vista de la acumulación de estado de nodos](./media/container-insights-health/health-view-nodes-01.png)

Actualmente, solo se admite el estado de un kubelet virtual. El estado de mantenimiento del uso de CPU y memoria de los nodos kubelet virtuales se notifica como **Desconocido**, ya que no se recibe una señal de ellos.

Todos los monitores se muestran en un diseño jerárquico en el panel Jerarquía de estado, donde un monitor agregado que representa el objeto Kubernetes o la abstracción (es decir, la infraestructura o los nodos de Kubernetes), son el monitor de nivel superior que refleja el estado combinado de todos monitores secundarios dependientes. Los escenarios clave de supervisión que se usan para derivar el estado son:

* Evaluar el uso de CPU del nodo y del contenedor.
* Evaluar el uso de memoria del nodo y del contenedor.
* Estado de pods y nodos según el cálculo de su estado listo indicado por Kubernetes.

Los iconos que se usan para indicar el estado son los siguientes:

|Icono|Significado|  
|--------|-----------|  
|![El icono de marca de verificación verde indica un estado correcto](./media/container-insights-health/healthyicon.png)|Correcto, el estado es correcto (verde)|  
|![El triángulo amarillo y el signo de admiración son de advertencia](./media/container-insights-health/warningicon.png)|Advertencia (amarillo)|  
|![El botón rojo con una X blanca indica un estado crítico](./media/container-insights-health/criticalicon.png)|Crítico (rojo)|  
|![Icono gris](./media/container-insights-health/grayicon.png)|Desconocido (gris)|  

## <a name="monitor-configuration"></a>Configuración del monitor

Para comprender el comportamiento y la configuración de cada monitor compatible con la característica Estado de Azure Monitor para contenedores, consulte la [Guía de configuración de los monitores de estado](container-insights-health-monitors-config.md).

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com). 

## <a name="view-health-of-an-aks-or-non-aks-cluster"></a>Ver el estado de un clúster AKS o no AKS

El acceso a la característica Estado (versión preliminar) de Azure Monitor para contenedores está disponible directamente desde un clúster de AKS al seleccionar la opción **Conclusiones** del panel izquierdo de Azure Portal. En la sección **Insights**  (Conclusiones), seleccione **Contenedores**. 

Para ver el estado de un clúster que no es de AKS, que es un clúster de motor de AKS hospedado de forma local o en Azure Stack, seleccione **Azure Monitor** en el panel izquierdo de la Azure Portal. En la sección **Insights**  (Conclusiones), seleccione **Contenedores**.  En la página de varios clústeres, seleccione de la lista el clúster que no sea de AKS.

En Azure Monitor para contenedores, en la página de **Clúster**, seleccione **Estado**.

![Ejemplo del panel de estado del clúster](./media/container-insights-health/container-insights-health-page.png)

## <a name="review-cluster-health"></a>Comprobación del estado del clúster

Cuando se abre la página Estado, **Infraestructura de Kubernetes** está seleccionado de forma predeterminada en la cuadrícula **Aspecto del estado**.  En la cuadrícula se resume el estado de acumulación de estado actual de la infraestructura de Kubernetes y los nodos de clúster. Al seleccionar cualquier aspecto de estado, se actualizan los resultados en el panel de Jerarquía de estado (es decir, en el panel central) y se muestran todos los monitores secundarios en un diseño jerárquico, mostrando su estado de mantenimiento actual. Para ver más información acerca de cualquier monitor dependiente, puede seleccionar uno y se muestra automáticamente un panel de propiedades en el lado derecho de la página. 

![Panel de propiedades de estado del clúster](./media/container-insights-health/health-view-property-pane.png)

En el panel de propiedades, aprenderá lo siguiente:

- En la pestaña **Información general**, se muestra el estado actual del monitor seleccionado, cuándo se calculó por última vez el monitor y cuándo se produjo el último cambio de estado. En función del tipo de monitor seleccionado en la jerarquía, se muestra información adicional.

    Si selecciona un monitor agregado en el panel Jerarquía de estado, en la pestaña **Información general** en el panel de propiedades, se muestra un resumen del número total de monitores secundarios en la jerarquía y cuántos monitores agregados se encuentran en un estado crítico, de advertencia y correcto. 

    ![Pestaña Información general del panel de propiedades de estado para el monitor agregado](./media/container-insights-health/health-overview-aggregate-monitor.png)

    Si selecciona un monitor de unidad en el panel de Jerarquía de estado, también se muestra en **Último cambio de estado** los ejemplos anteriores calculados y comunicados por el agente en contenedor en las últimas cuatro horas. Esto se basa en el cálculo de los monitores de unidad para comparar varios valores consecutivos y determinar su estado. Por ejemplo, si seleccionó el monitor de unidad *Pod en estado listo*, se mostrarán los dos últimos ejemplos controlados por el parámetro *ConsecutiveSamplesForStateTransition*. Para más información, consulte la descripción de [monitores de unidad](container-insights-health-monitors-config.md#unit-monitors).
    
    ![Pestaña con la información general del panel de propiedades de estado](./media/container-insights-health/health-overview-unit-monitor.png)

    Si el tiempo indicado por **Último cambio de estado** es un día o más, es el resultado de que no haya cambios en el estado del monitor. Sin embargo, si la última muestra recibida para un monitor de unidad tiene más de cuatro horas de antigüedad, probablemente indica que el agente en contenedor no ha estado enviando datos. Si el agente sabe que existe un recurso concreto, por ejemplo un nodo, pero no ha recibido datos de los monitores de uso de memoria o CPU del nodo (por ejemplo), el estado de mantenimiento del monitor se establece como **Desconocido**.  

- En la pestaña **Configuración**, se muestra la configuración predeterminada de los parámetros de configuración (solo para monitores de unidad, no para monitores agregados) y sus valores.
- En la pestaña **Conocimientos**, hay información que explica el comportamiento del monitor y cómo se evalúa para determinar si la condición es incorrecta.

La supervisión de datos en esta página no se actualiza automáticamente y debe seleccionar **Actualizar** en la parte superior de la página para ver el estado de mantenimiento más reciente recibido del clúster.

## <a name="next-steps"></a>Pasos siguientes

En los [ejemplos de consultas de registro](container-insights-log-search.md#search-logs-to-analyze-data) encontrará consultas predefinidas y ejemplos que puede evaluar o personalizar para las alertas, visualización o análisis de los clústeres.
