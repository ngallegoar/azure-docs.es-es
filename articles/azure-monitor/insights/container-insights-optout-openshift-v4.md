---
title: Cómo detener la supervisión del clúster de Azure y Red Hat OpenShift v4 | Microsoft Docs
description: En este artículo se describe cómo puede detener la supervisión del clúster de Red Hat OpenShift en Azure y Red Hat OpenShift versión 4 con Azure Monitor para contenedores.
ms.topic: conceptual
ms.date: 04/24/2020
ms.openlocfilehash: 768c4db8d72778b555a4f343cf2e23b8fa861991
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82196965"
---
# <a name="how-to-stop-monitoring-your-azure-and-red-hat-openshift-v4-cluster"></a>Cómo detener la supervisión del clúster de Azure y Red Hat OpenShift v4

Después de habilitar la supervisión del clúster de Red Hat OpenShift en Azure y Red Hat OpenShift versión 4.x, puede detener la supervisión del clúster con Azure Monitor para contenedores si decide que ya no quiere continuar haciéndolo. En este artículo se muestra cómo hacerlo.  

## <a name="how-to-stop-monitoring-using-helm"></a>Cómo detener la supervisión con Helm

1. Para identificar primero la versión del gráfico de Helm de Azure Monitor para contenedores instalada en el clúster, ejecute el siguiente comando de Helm.

    ```
    helm list
    ```

    La salida será similar a la siguiente:

    ```
    NAME                            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                           APP VERSION
    azmon-containers-release-1      default         3               2020-04-21 15:27:24.1201959 -0700 PDT   deployed        azuremonitor-containers-2.7.0   7.0.0-1
    ```

    *azmon-containers-release-1* representa la versión del gráfico de Helm para Azure Monitor para contenedores.

2. Para eliminar la versión del gráfico, ejecute el siguiente comando de Helm.

    `helm delete <releaseName>`

    Ejemplo:

    `helm delete azmon-containers-release-1`

    Esto eliminará la versión del clúster. Puede comprobarlo mediante la ejecución del comando `helm list`:

    ```
    NAME                            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                           APP VERSION
    ```

El cambio de configuración puede tardar unos minutos en completarse. Dado que Helm realiza un seguimiento de las versiones incluso después de eliminarlas, es posible auditar el historial de un clúster e incluso recuperar una versión con `helm rollback`.

## <a name="next-steps"></a>Pasos siguientes

Si el área de trabajo de Log Analytics se creó solamente para admitir la supervisión del clúster y ya no es necesaria, tiene que eliminarla manualmente. Si no está familiarizado con la eliminación de un área de trabajo, consulte [Eliminación de un área de trabajo de Azure Log Analytics](../../log-analytics/log-analytics-manage-del-workspace.md).
