---
title: Uso de Azure Policy para aplicar configuraciones de clúster a escala (versión preliminar)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Uso de Azure Policy para aplicar configuraciones de clúster a escala
keywords: Kubernetes, Arc, Azure, K8s, contenedores
ms.openlocfilehash: c017e9422733069ffd93f6dff72ecb884da057c4
ms.sourcegitcommit: a9784a3fd208f19c8814fe22da9e70fcf1da9c93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2020
ms.locfileid: "83779962"
---
# <a name="use-azure-policy-to-apply-cluster-configurations-at-scale-preview"></a>Uso de Azure Policy para aplicar configuraciones de clúster a escala (versión preliminar)

## <a name="overview"></a>Información general

Use Azure Policy para exigir que cada recurso `Microsoft.Kubernetes/connectedclusters` o `Microsoft.ContainerService/managedClusters` habilitado para operaciones de Git tenga aplicado un elemento `Microsoft.KubernetesConfiguration/sourceControlConfigurations` específico.  Para usar Azure Policy, debe seleccionar una definición de directiva existente y crear una asignación de directiva.  Al crear la asignación de directiva, establezca el ámbito de la asignación: será un grupo de recursos o una suscripción de Azure.  También puede establecer los parámetros para el elemento `sourceControlConfiguration` que se creará.  Una vez que se ha creado la asignación, el motor de directivas identificará todos los recursos `connectedCluster` o `managedCluster` que se encuentren dentro del ámbito y aplicará `sourceControlConfiguration` a cada uno de ellos.

Si usa varios repositorios de Git como orígenes de verdad para cada clúster (por ejemplo, un repositorio para el operador de clúster o TI central, y otros para los equipos de la aplicación), puede habilitarlo mediante varias asignaciones de directiva, cada una configurada para utilizar un repositorio de Git diferente.

## <a name="create-a-custom-policy-definition"></a>Creación de una definición de directiva personalizada

1. En Azure Portal, vaya a Directiva y, en la sección **Creación** de la barra lateral, seleccione **Definiciones**.
2. Seleccione **+ Definición de directiva**.
3. Establezca **Ubicación de definición** en la suscripción o el grupo de administración.  Esto determinará el ámbito más amplio en el que se puede usar la definición de directiva.
4. Asigne a la directiva un **Nombre** y una **Descripción**.
5. En Categoría, elija **Crear nuevo** y escriba *Clúster de Kubernetes: Azure Arc*.
6. En el cuadro de edición **Regla de directivas**, copie y pegue el contenido de este [ejemplo de definición de directiva](https://raw.githubusercontent.com/Azure/arc-k8s-demo/master/policy/Ensure-GitOps-configuration-for-Kubernetes-cluster.json).
7. Seleccione **Guardar**.

Este paso para crear una definición de directiva personalizada no será necesario una vez que se haya completado el trabajo para convertirla en una directiva integrada.

## <a name="create-a-policy-assignment"></a>Creación de una asignación de directiva

1. En Azure Portal, vaya a Directiva y, en la sección **Creación** de la barra lateral, seleccione **Definiciones**.
2. Busque la definición que acaba de crear y selecciónela.
3. En las acciones de página, seleccione **Asignar**.
4. Establezca el **Ámbito** en el grupo de administración, la suscripción o el grupo de recursos al que se aplicará la asignación de directiva.
5. Si quiere excluir recursos del ámbito de la directiva, establezca **Exclusiones**.
6. Asigne un **Nombre** y una **Descripción** a la asignación de directiva que pueda usar para identificarla con facilidad.
7. Asegúrese de que **Cumplimiento de directivas** esté establecido en *Habilitado*.
8. Seleccione **Next** (Siguiente).
9. Establezca los valores de parámetro que se usarán durante la creación de `sourceControlConfiguration`.
10. Seleccione **Next** (Siguiente).
11. Habilite **Crear una tarea de corrección**.
12. Asegúrese de que **Crear una identidad administrada** está activada y de que la identidad tendrá permisos de **Colaborador**.  Vea [esta documentación](https://docs.microsoft.com/azure/governance/policy/assign-policy-portal) y [el comentario de esta documentación](https://docs.microsoft.com/azure/governance/policy/how-to/remediate-resources) para obtener más información sobre los permisos que necesita.
13. Seleccione **Revisar + crear**.

Después de crear la asignación de directiva, para cualquier recurso `connectedCluster` nuevo (o recurso `managedCluster` con los agentes de GitOps instalados) que se encuentra dentro del ámbito de la asignación, se aplicará `sourceControlConfiguration`.  Para los clústeres existentes, tendrá que ejecutar manualmente una tarea de corrección.  Normalmente, se tarda entre 10 y 20 minutos para que la asignación de la directiva surta efecto.

## <a name="verify-a-policy-assignment"></a>Comprobación de una asignación de directiva

1. En Azure Portal, vaya a uno de los recursos `connectedCluster` y, en la sección **Configuración** de la barra lateral, seleccione **Directivas**. (La experiencia de usuario para el clúster administrado de AKS todavía no se ha implementado, pero se hará en el futuro).
2. En la lista, debería ver la asignación de directiva que ha creado antes y el valor de **Estado de cumplimiento** debe ser *Conforme*.
3. En la sección **Configuración** de la barra lateral, seleccione **Configuraciones**.
4. En la lista, debería ver el elemento `sourceControlConfiguration` creado por la asignación de directiva.
5. Use **kubectl** para interrogar al clúster: debería ver el espacio de nombres y los artefactos creados por `sourceControlConfiguration`.
6. En cinco minutos, debería ver en el clúster los artefactos que se describen en los manifiestos del repositorio de Git configurado.

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de Azure Monitor para contenedores con clústeres de Kubernetes habilitados para Arc](./deploy-azure-monitor-for-containers.md)
