---
title: Información sobre Azure Policy para AKS Engine
description: Obtenga información sobre cómo Azure Policy usa CustomResourceDefinitions y Open Policy Agent desde Gatekeeper v3 para administrar clústeres con AKS Engine.
ms.date: 11/04/2019
ms.topic: conceptual
ms.openlocfilehash: c41a9d84dfe43e356e9a4a17af523a37209c2933
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75436426"
---
# <a name="understand-azure-policy-for-aks-engine"></a>Explicación de Azure Policy para AKS Engine

Azure Policy se integra con [AKS Engine](https://github.com/Azure/aks-engine/blob/master/docs/README.md), un sistema que proporciona herramientas prácticas para arrancar rápidamente un clúster de Kubernetes autoadministrado en Azure. Esta integración permite aplicar medidas de seguridad y cumplimiento a escala en los clústeres autoadministrados de AKS Engine de manera centralizada y coherente. Al extender el uso de [Open Policy Agent](https://www.openpolicyagent.org/) (OPA) [Gatekeeper](https://github.com/open-policy-agent/gatekeeper) v3 (beta), un _webhook de controlador de admisión_, Azure Policy le permite administrar e informar sobre el estado de cumplimiento de sus recursos de Azure y clústeres autoadministrados de AKS Engine desde un solo lugar.

> [!NOTE]
> Azure Policy para AKS Engine está en versión preliminar pública y no tiene ningún contrato de nivel de servicio. Gatekeeper v3 está en versión beta y cuenta con el respaldo de la comunidad de código abierto. El servicio solo admite definiciones de directivas integradas y un único clúster de AKS Engine para cada grupo de recursos configurado con una entidad de servicio.

> [!IMPORTANT]
> Para obtener soporte técnico para Azure Policy para AKS Engine, AKS Engine o Gatekeeper v3, cree una [nueva incidencia](https://github.com/Azure/aks-engine/issues/new/choose) en el repositorio de GitHub de AKS Engine.

## <a name="overview"></a>Información general

Para habilitar y usar Azure Policy para AKS Engine con el clúster de Kubernetes autoadministrado en Azure, realice las siguientes acciones:

- [Requisitos previos](#prerequisites)
- [Instale el complemento de Azure Policy](#installing-the-add-on)
- [Asigne una definición de directiva para AKS Engine](#built-in-policies)
- [Espere la validación](#validation-and-reporting-frequency)

## <a name="prerequisites"></a>Prerrequisitos

Antes de instalar el complemento de Azure Policy o habilitar cualquiera de las características del servicio, su suscripción debe tener habilitados el proveedor de recursos **Microsoft.PolicyInsights** y crear una asignación de roles para la entidad de servicio del clúster. 

1. Para habilitar el proveedor de recursos, siga los pasos descritos en [Tipos y proveedores de recursos](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal) o ejecute el comando de PowerShell o la CLI de Azure:

   - Azure CLI

     ```azurecli-interactive
     # Log in first with az login if you're not using Cloud Shell
     
     # Provider register: Register the Azure Policy provider
     az provider register --namespace 'Microsoft.PolicyInsights'
     ```

   - Azure PowerShell
   
     ```azurepowershell-interactive
     # Log in first with Connect-AzAccount if you're not using Cloud Shell
   
     # Provider register: Register the Azure Policy provider
     Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
     ```

1. Creación de una asignación de roles para la entidad de servicio del clúster

   - Si no conoce el identificador de la aplicación de la entidad de servicio del clúster, búsquelo con el siguiente comando.

     ```bash
     # Get the kube-apiserver pod name
     kubectl get pods -n kube-system
   
     # Find the aadClientID value
     kubectl exec <kube-apiserver pod name> -n kube-system cat /etc/kubernetes/azure.json
     ```

   - Consigne la asignación de roles "Policy Insights Data Writer (Preview)" al identificador de la aplicación de la entidad de servicio del clúster (valor _aadClientID_ del paso anterior) con la CLI de Azure. Reemplace `<subscriptionId>` por el identificador de su suscripción y `<aks engine cluster resource group>` por el grupo de recursos en el que se encuentra el clúster de Kubernetes autoadministrado de AKS Engine.

     ```azurecli-interactive
     az role assignment create --assignee <cluster service principal app ID> --scope "/subscriptions/<subscriptionId>/resourceGroups/<aks engine cluster resource group>" --role "Policy Insights Data Writer (Preview)"
     ```

## <a name="azure-policy-add-on"></a>Complemento de Azure Policy

El _complemento de Azure Policy_ para Kubernetes conecta el servicio de Azure Policy al controlador de admisión de Gatekeeper. El complemento, que se instala en el espacio de nombres _kube-system_, ejecuta las siguientes funciones:

- Comprueba con Azure Policy las asignaciones al clúster de AKS Engine.
- Descarga e instala los detalles de la directiva, las plantillas de restricción y las restricciones.
- Ejecuta una comprobación de cumplimiento de examen completo en el clúster de AKS Engine.
- Realiza informes de auditoría y cumplimiento de los detalles de Azure Policy.

### <a name="installing-the-add-on"></a>Instalación del complemento

Una vez completados los requisitos previos, se puede instalar el complemento de Azure Policy. La instalación puede realizarse durante la creación o el ciclo de actualización de una instancia de AKS Engine o como una acción independiente en un clúster existente.

- Instalación durante la creación o el ciclo de actualización

  Para habilitar el complemento Azure Policy durante la creación de un nuevo clúster autoadministrado o como una actualización de un clúster existente, incluya la definición del clúster de la propiedad **addons** para AKS Engine.

  ```json
  "addons": [{
      "name": "azure-policy",
      "enabled": true,
      "config": {
          "auditInterval": "30",
          "constraintViolationsLimit": "20"
      }
  }]
  ```

  Para obtener más información al respecto, consulte la guía externa de [definición del clúster de AKS Engine](https://github.com/Azure/aks-engine/blob/master/docs/topics/clusterdefinitions.md).

- Instalación en un clúster existente con gráficos de Helm

  Siga estos pasos para preparar el clúster e instalar el complemento:

  1. Instale Gatekeeper en el espacio de nombres _gatekeeper-system_.

     ```bash
     kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/master/deploy/gatekeeper.yaml
     ```
  1. Agregue la etiqueta _control-plane_ a _kube-system_. Esta etiqueta excluye la auditoría de pods y servicios de _kube-system_ de Gatekeeper y el complemento de Azure Policy.

     ```bash
     kubectl label namespaces kube-system control-plane=controller-manager
     ```

  1. Sincronice los datos de Kubernetes (espacio de nombres, pod, entrada, servicio) con OPA.

     ```bash
     kubectl apply -f https://raw.githubusercontent.com/Azure/azure-policy/master/built-in-references/Kubernetes/gatekeeper-opa-sync.yaml
     ```

     Para obtener más información, consulte el tema de [OPA sobre la replicación de datos](https://github.com/open-policy-agent/gatekeeper#replicating-data).

  1. Agregue el repositorio Azure Policy a Helm.

     ```bash
     helm repo add azure-policy https://raw.githubusercontent.com/Azure/azure-policy/master/extensions/policy-addon-kubernetes/helm-charts
     ```

     Para obtener más información, consulte la [guía de inicio rápido del gráfico de Helm](https://helm.sh/docs/using_helm/#quickstart-guide).

  1. Instale el complemento con un gráfico de Helm. Reemplace `<subscriptionId>` por el identificador de su suscripción y `<aks engine cluster resource group>` por el grupo de recursos en el que se encuentra el clúster de Kubernetes autoadministrado de AKS Engine.

     ```bash
     helm install azure-policy/azure-policy-addon-aks-engine --name azure-policy-addon --set azurepolicy.env.resourceid="/subscriptions/<subscriptionId>/resourceGroups/<aks engine cluster resource group>"
     ```

     Para obtener más información sobre lo que instala el gráfico de Helm de complemento, consulte la [definición del gráfico de Helm de complemento de Azure Policy](https://github.com/Azure/azure-policy/tree/master/extensions/policy-addon-kubernetes/helm-charts) en GitHub.

     > [!NOTE]
     > Debido a la relación entre Azure Policy complemento y el identificador del grupo de recursos, Azure Policy solo admite un clúster de AKS Engine para cada grupo de recursos.

Para comprobar que la instalación del complemento se ha realizado correctamente y que el pod _azure-policy_ se está ejecutando, ejecute el siguiente comando:

```bash
kubectl get pods -n kube-system
```

### <a name="validation-and-reporting-frequency"></a>Validación y frecuencia de los informes

El complemento se registra con Azure Policy para detectar cambios en las asignaciones de directivas cada 5 minutos. Durante este ciclo de actualización, el complemento comprueba si hay cambios. Estos cambios desencadenan la creación, actualización o eliminación de restricciones y plantillas de restricciones.

> [!NOTE]
> Aunque es posible que un _administrador de clústeres_ tenga permiso para realizar cambios en las restricciones y las plantillas de restricciones, no se recomienda ni se admite para realizar cambios en las restricciones ni en las plantillas de restricciones creadas por Azure Policy. Cualquier cambio manual realizado se pierde durante el ciclo de actualización.

Cada 5 minutos, el complemento solicita un examen completo del clúster. Después de recopilar los detalles del examen completo y las evaluaciones en tiempo real que realiza Gatekeeper sobre los intentos de cambios en el clúster, el complemento proporciona los resultados a Azure Policy para los agregue en los [detalles de cumplimiento](../how-to/get-compliance-data.md) como cualquier otra asignación de Azure Policy. Solo los resultados de las asignaciones de directivas activas se devuelven durante el ciclo de auditoría. Los resultados de la auditoría también pueden verse como infracciones enumeradas en el campo de estado de la restricción errónea.

## <a name="policy-language"></a>Idioma de directiva

La estructura del lenguaje de Azure Policy para administrar AKS Engine sigue el camino de las directivas existentes. El efecto _EnforceOPAConstraint_ se usa para administrar los clústeres de AKS Engine y recopila propiedades de _detalles_ específicas para trabajar con [OPA Constraint Framework](https://github.com/open-policy-agent/frameworks/tree/master/constraint) y Gatekeeper v3. Para obtener detalles y ejemplos, consulte el efecto [EnforceOPAConstraint](effects.md#enforceopaconstraint).

Como parte de las propiedades _details.constraintTemplate_ y _details.constraint_ de la definición de directiva, Azure Policy pasa los URI de estas [CustomResourceDefinitions](https://github.com/open-policy-agent/gatekeeper#constraint-templates) (CRD) al complemento. Rego es el lenguaje que OPA y el equipo selector admiten para validar una solicitud al clúster de Kubernetes. Al admitir un estándar existente para la administración de Kubernetes, Azure Policy le permite reutilizar las reglas existentes y vincularlas con Azure Policy para obtener una experiencia de informes de cumplimiento unificada en la nube. Para obtener más información, consulte [¿Qué es Rego?](https://www.openpolicyagent.org/docs/how-do-i-write-policies.html#what-is-rego)

## <a name="built-in-policies"></a>Directivas integradas

Para encontrar las directivas integradas para administrar el clúster de AKS Engine mediante Azure Portal, siga estos pasos:

1. Inicie el servicio Azure Policy en Azure Portal. Seleccione **All services** (Todos los servicios) en el panel izquierdo y busque y seleccione la opción **Policy** (Directiva).

1. En el panel izquierdo de la página de Azure Policy, seleccione **Definitions** (Definiciones).

1. En el cuadro de lista desplegable Categoría, use **Seleccionar todo** para borrar el filtro y seleccione **Kubernetes**.

1. Seleccione la definición de directiva y pulse el botón **Assign** (Asignar).

> [!NOTE]
> Al asignar la definición de Azure Policy para AKS Engine, el valor de **Ámbito** debe ser el grupo de recursos del clúster de AKS Engine.

Alternativamente, use la opción de inicio rápido [Asignar una directiva - Portal](../assign-policy-portal.md) para encontrar y asignar una directiva de AKS Engine. Busque una definición de directiva de AKS Engine en lugar del ejemplo "audit vms".

> [!IMPORTANT]
> Las directivas integradas en la categoría **Kubernetes** solo se usan con AKS Engine.

## <a name="logging"></a>Registro

### <a name="azure-policy-add-on-logs"></a>Registros del complemento de Azure Policy

Como controlador o contenedor de Kubernetes, el complemento de Azure Policy mantiene registros en el clúster de AKS Engine.

Para ver los registros del complemento de Azure Policy, use `kubectl`:

```bash
# Get the Azure Policy Add-on pod name
kubectl -n kube-system get pods -l app=azure-policy --output=name

# Get the logs for the add-on
kubectl logs <Azure Policy Add-on pod name> -n kube-system
```

### <a name="gatekeeper-logs"></a>Registros de Gatekeeper

El pod de Gatekeeper, _gatekeeper-controller-manager-0_, suele estar en el espacio de nombres `gatekeeper-system` o `kube-system`, pero puede estar en un espacio de nombres diferente en función de cómo se implemente.

Para ver los registros de Gatekeeper, use `kubectl`:

```bash
NAMESPACE=<namespace of gatekeeper>
kubectl logs gatekeeper-controller-manager-0 -n $NAMESPACE
```

Para obtener más información, consulte [Depuración de Gatekeeper](https://github.com/open-policy-agent/gatekeeper#debugging) en la documentación de OPA.

## <a name="remove-the-add-on"></a>Eliminar el complemento

Para quitar el complemento de Azure Policy y Gatekeeper del clúster de AKS Engine, use el método en línea con la manera en que se ha instalado el complemento:

- Si se ha instalado definiendo la propiedad **addons** en la definición del clúster para AKS Engine:

  Vuelva a implementar la definición del clúster en AKS Engine después de cambiar la propiedad **addons** de _azure-policy_ a false:


  ```json
  "addons": [{
      "name": "azure-policy",
      "enabled": false
  }]
  ```

- Si se ha instalado con gráficos de Helm:

  1. Quite las restricciones antiguas.

     Actualmente, el mecanismo de desinstalación solo quita el sistema Gatekeeper, no quita ningún recurso _ConstraintTemplate_, _Constraint_ ni _Config_ creado por el usuario, ni quita las definiciones _CRD_ que los acompañan.

     Cuando Gatekeeper se está ejecutando, se pueden quitar las restricciones no deseadas mediante las siguientes acciones:

     - Eliminar todas las instancias del recurso Constraint.
     - Eliminar el recurso _ConstraintTemplate_, que debe limpiar automáticamente la definición _CRD_.
     - Eliminar el recurso _Config_, con lo que se quitarán los finalizadores de los recursos sincronizados.

  1. Desinstale el complemento de Azure Policy.
  
     ```bash
     helm del --purge azure-policy-addon
     ```

  1. Desinstale Gatekeeper.
  
     ```bash
     kubectl delete -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/master/deploy/gatekeeper.yaml
     ```

## <a name="diagnostic-data-collected-by-azure-policy-add-on"></a>Datos de diagnóstico recopilados por el complemento de Azure Policy

El complemento de Azure Policy para Kubernetes recopila datos de diagnóstico de clúster limitados. Estos datos de diagnóstico son datos técnicos esenciales relacionados con el software y el rendimiento. Se utiliza para lo siguiente:

- Mantener el complemento de Azure Policy actualizado.
- Mantener la seguridad, la confiabilidad y el rendimiento del complemento de Azure Policy
- Mejorar el complemento de Azure Policy mediante el análisis agregado del uso del complemento

La información recopilada por el complemento no son datos personales. Actualmente se recopilan los detalles siguientes:

- Versión del agente del complemento de Azure Policy
- Tipo de clúster
- Región del clúster
- Grupo de recursos del clúster
- Identificador de recurso del clúster
- Identificador de suscripción del clúster
- Sistema operativo del clúster (ejemplo: Linux)
- Ciudad del clúster (ejemplo: Seattle)
- Estado o provincia del clúster (ejemplo: Washington)
- País o región del clúster (ejemplo: Estados Unidos)
- Excepciones o errores encontrados por el complemento de Azure Policy durante la instalación del agente en la evaluación de directiva
- Número de directivas de Gatekeeper no instaladas por el complemento de Azure Policy

## <a name="next-steps"></a>Pasos siguientes

- Puede consultar ejemplos en [Ejemplos de Azure Policy](../samples/index.md).
- Vea la [Estructura de definición de Azure Policy](definition-structure.md).
- Vea la [Descripción de los efectos de directivas](effects.md).
- Obtenga información acerca de cómo se pueden [crear directivas mediante programación](../how-to/programmatically-create.md).
- Obtenga información sobre cómo [obtener datos de cumplimiento](../how-to/get-compliance-data.md).
- Obtenga información sobre cómo [corregir recursos no compatibles](../how-to/remediate-resources.md).
- En [Organización de los recursos con grupos de administración de Azure](../../management-groups/overview.md), obtendrá información sobre lo que es un grupo de administración.