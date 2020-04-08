---
title: Más información sobre Azure Policy para Azure Kubernetes Service
description: Obtenga información sobre cómo Azure Policy usa Rego y el agente de directivas público para administrar clústeres en Azure Kubernetes Service.
ms.date: 03/27/2020
ms.topic: conceptual
ms.openlocfilehash: d77c5cf94a8239f4617e563961cbe1cc40e48fe0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "80372660"
---
# <a name="understand-azure-policy-for-azure-kubernetes-service"></a>Cómo usar Azure Policy para Azure Kubernetes Service

La integración de Azure Policy con [Azure Kubernetes Service](../../../aks/intro-kubernetes.md) (AKS) permite a los usuarios aplicar medidas de protección a escala para clústeres de un modo centralizado y coherente.
Al extender el uso de [Gatekeeper](https://github.com/open-policy-agent/gatekeeper) v3, un _webhook de controlador de admisión_ para [Open Policy Agent](https://www.openpolicyagent.org/) (OPA), Azure Policy le permite administrar e informar sobre el estado de cumplimiento de sus recursos de Azure y clústeres de AKS desde un solo lugar.

> [!IMPORTANT]
> Azure Policy para AKS está en versión preliminar y solo admite definiciones de directivas integradas. Las directivas integradas se encuentran en la categoría **Kubernetes**. El efecto **EnforceRegoPolicy** y las directivas de la categoría **Kubernetes Service** relacionadas van a quedar _en desuso_. En su lugar, use el efecto actualizado [EnforceOPAConstraint](./effects.md#enforceopaconstraint).

> [!WARNING]
> Esta característica aún no está disponible en todas las regiones. Para obtener un estado en el lanzamiento, vea [Problemas de AKS: cambio importante para el complemento de directivas](https://github.com/Azure/AKS/issues/1529).

## <a name="overview"></a>Información general

Para habilitar y usar la Azure Policy para AKS con su clúster de AKS, realice las siguientes acciones:

- [Participar en las características de la versión preliminar](#opt-in-for-preview)
- [Instale el complemento de Azure Policy](#installation-steps)
- [Asigne una definición de directiva para AKS](#built-in-policies)
- [Espere la validación](#validation-and-reporting-frequency)

## <a name="opt-in-for-preview"></a>Participar en la versión preliminar

Antes de instalar el complemento de Azure Policy o habilitar cualquiera de las características del servicio, su suscripción debe tener habilitados el proveedor de recursos **Microsoft.ContainerService** y el proveedor de recursos **Microsoft.PolicyInsights**; a continuación, debe obtener la aprobación para unirse a la versión preliminar. Para unirse a la versión preliminar, siga estos pasos en Azure Portal o con la CLI de Azure:

- Azure Portal:

  1. Registre los proveedores de recursos **Microsoft.ContainerService** y **Microsoft.PolicyInsights**. Para conocer los pasos, consulte [Proveedores de recursos y sus tipos](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal).

  1. Inicie el servicio Azure Policy en Azure Portal. Para ello, haga clic en **Todos los servicios** y, a continuación, busque y seleccione **Directiva**.

     ![Búsqueda de la directiva en todos los servicios](../media/rego-for-aks/search-policy.png)

  1. Seleccione **Join Preview** (Unirse a la versión preliminar) en el lado izquierdo de la página Azure Policy.

     ![Unirse a la directiva de la versión preliminar de AKS](../media/rego-for-aks/join-aks-preview.png)

  1. Seleccione la fila de la suscripción que quiera agregar a la versión preliminar.

  1. Seleccione el botón **Opt-in** (Participar) en la parte superior de la lista de suscripciones.

- CLI de Azure:

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  # Provider register: Register the Azure Kubernetes Service provider
  az provider register --namespace Microsoft.ContainerService

  # Provider register: Register the Azure Policy provider
  az provider register --namespace Microsoft.PolicyInsights

  # Feature register: enables installing the add-on
  az feature register --namespace Microsoft.ContainerService --name AKS-AzurePolicyAutoApprove
  
  # Use the following to confirm the feature has registered
  az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKS-AzurePolicyAutoApprove')].{Name:name,State:properties.state}"
  
  # Once the above shows 'Registered' run the following to propagate the update
  az provider register -n Microsoft.ContainerService
  
  ```

## <a name="azure-policy-add-on"></a>Complemento de Azure Policy

El _complemento de Azure Policy_ para Kubernetes conecta el servicio de Azure Policy al controlador de admisión de Gatekeeper. El complemento, que se instala en el espacio de nombres _kube-system_, ejecuta las siguientes funciones:

- Comprueba con el servicio Azure Policy las asignaciones al clúster.
- Implementa directivas en el clúster como [plantilla de restricción](https://github.com/open-policy-agent/gatekeeper#constraint-templates) y recursos personalizados de [restricción](https://github.com/open-policy-agent/gatekeeper#constraints).
- Realiza informes de auditoría y cumplimiento y los devuelve al servicio Azure Policy.

### <a name="installing-the-add-on"></a>Instalación del complemento

#### <a name="prerequisites"></a>Prerrequisitos

Antes de instalar el complemento en su clúster de AKS, debe instalar la extensión de la versión preliminar. Este paso se realiza con la CLI de Azure:

1. Si las directivas de Gatekeeper v2 están instaladas, quite el complemento con el botón **Deshabilitar** en el clúster de AKS en la página **Directivas (versión preliminar)** .

1. Es preciso que esté instalada y configurada la versión 2.0.62 de la CLI de Azure, o cualquier otra versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

1. Se debe usar la versión del clúster de AKS _1.14_ o posterior. Use el siguiente script para validar la versión del clúster de AKS:

   ```azurecli-interactive
   # Log in first with az login if you're not using Cloud Shell

   # Look for the value in kubernetesVersion
   az aks list
   ```

1. Instale la versión _0.4.0_ de la extensión de la versión preliminar de la CLI de Azure para AKS, `aks-preview`:

   ```azurecli-interactive
   # Log in first with az login if you're not using Cloud Shell

   # Install/update the preview extension
   az extension add --name aks-preview

   # Validate the version of the preview extension
   az extension show --name aks-preview --query [version]
   ```

   > [!NOTE]
   > Si ha instalado anteriormente la extensión _aks-preview_, instale todas las actualizaciones con el comando `az extension update --name aks-preview`.

#### <a name="installation-steps"></a>Pasos de instalación

Una vez completados los requisitos previos, instale el complemento de Azure Policy en el clúster de AKS que quiera administrar.

- Portal de Azure

  1. Inicie el servicio AKS en Azure Portal haciendo clic en **Todos los servicios**; a continuación, busque y seleccione **Servicios de Kubernetes**.

  1. Seleccione uno de sus clústeres de AKS.

  1. Seleccione **Policies (preview)** (Directivas [versión preliminar]) en el lado izquierdo de la página de servicio de Kubernetes.

     ![Directivas del clúster de AKS](../media/rego-for-aks/policies-preview-from-aks-cluster.png)

  1. En la página principal, seleccione el botón **Enable add-on** (Habilitar complemento).

     ![Habilitar la directiva de Azure para el complemento de AKS](../media/rego-for-aks/enable-policy-add-on.png)

     > [!NOTE]
     > Si el botón **Enable add-on** (Habilitar complemento) está en gris, es que la suscripción aún no se ha agregado a la versión preliminar. Consulte [Opt-in for preview](#opt-in-for-preview) (Participar en la versión preliminar) para ver los pasos necesarios. Si hay un botón **Deshabilitar** disponible, significa que Gatekeeper v2 está instalado aún y, por tanto, es necesario quitarlo.

- Azure CLI

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  az aks enable-addons --addons azure-policy --name MyAKSCluster --resource-group MyResourceGroup
  ```

### <a name="validation-and-reporting-frequency"></a>Validación y frecuencia de los informes

El complemento se registra con el servicio Azure Policy para detectar cambios en las asignaciones de directivas cada 15 minutos.
Durante este ciclo de actualización, el complemento comprueba si hay cambios. Estos cambios desencadenan la creación, actualización o eliminación de restricciones y plantillas de restricciones.

> [!NOTE]
> Aunque es posible que un administrador de clústeres tenga permiso para crear y actualizar las plantillas de restricciones y los recursos de restricciones, estos no son escenarios admitidos, ya que se sobrescribirán las actualizaciones manuales.

Cada 15 minutos, el complemento solicita un examen completo del clúster. Después de recopilar los detalles del examen completo y las evaluaciones en tiempo real que realiza Gatekeeper sobre los intentos de cambios en el clúster, el complemento proporciona los resultados al servicio Azure Policy para su inclusión en los [detalles de cumplimiento](../how-to/get-compliance-data.md#portal) como cualquier otra asignación de Azure Policy. Solo los resultados de las asignaciones de directivas activas se devuelven durante el ciclo de auditoría. Los resultados de la auditoría también pueden verse como [infracciones](https://github.com/open-policy-agent/gatekeeper#audit) enumeradas en el campo de estado de la restricción errónea.

## <a name="policy-language"></a>Idioma de directiva

La estructura del lenguaje de Azure Policy para administrar Kubernetes sigue el camino de las directivas existentes. El efecto _EnforceOPAConstraint_ se usa para administrar los clústeres de Kubernetes y recopila propiedades de detalles específicas para trabajar con [OPA Constraint Framework](https://github.com/open-policy-agent/frameworks/tree/master/constraint) y Gatekeeper v3. Para obtener detalles y ejemplos, consulte el efecto [EnforceOPAConstraint](./effects.md#enforceopaconstraint).
  
Como parte de las propiedades _details.constraintTemplate_ y _details.constraint_ de la definición de directiva, Azure Policy pasa los URI de estas [CustomResourceDefinitions](https://github.com/open-policy-agent/gatekeeper#constraint-templates) (CRD) al complemento. Rego es el lenguaje que OPA y el equipo selector admiten para validar una solicitud al clúster de Kubernetes. Al admitir un estándar existente para la administración de Kubernetes, Azure Policy le permite reutilizar las reglas existentes y vincularlas con Azure Policy para obtener una experiencia de informes de cumplimiento unificada en la nube. Para obtener más información, consulte [¿Qué es Rego?](https://www.openpolicyagent.org/docs/latest/policy-language/#what-is-rego)

## <a name="built-in-policies"></a>Directivas integradas

Para encontrar las directivas integradas para administrar el clúster mediante Azure Portal, siga estos pasos:

1. Inicie el servicio Azure Policy en Azure Portal. Seleccione Todos los servicios en el panel izquierdo y busque y seleccione **Directiva**.

1. En el panel izquierdo de la página de Azure Policy, seleccione **Definitions** (Definiciones).

1. En el cuadro de lista desplegable Categoría, use Seleccionar todo para borrar el filtro y seleccione **Kubernetes**.

1. Seleccione la definición de directiva y pulse el botón **Assign** (Asignar).

1. Establezca el **Ámbito** en el grupo de administración, la suscripción o el grupo de recursos del clúster de Kubernetes al que se aplicará la asignación de directiva.

   > [!NOTE]
   > Al asignar Azure Policy para la definición de AKS, el **Ámbito** debe incluir el recurso de clúster de AKS.

1. Asigne un **Nombre** y una **Descripción** a la asignación de directiva que pueda usar para identificarla con facilidad.

1. Establezca [Cumplimiento de directivas](./assignment-structure.md#enforcement-mode) en uno de los valores siguientes.

   - **Habilitado**: aplicar la directiva en el clúster. Se deniegan las solicitudes de admisión de Kubernetes con infracciones.
   
   - **Deshabilitado**: no aplicar la directiva en el clúster. No se deniegan las solicitudes de admisión de Kubernetes con infracciones. Los resultados de la evaluación de cumplimiento siguen estando disponibles. Al implementar nuevas directivas para ejecutar clústeres, la opción _Deshabilitado_ resulta útil para probar las directivas, ya que las solicitudes de admisión con infracciones no se deniegan.

1. Seleccione **Next** (Siguiente).

1. Establecer **Valores de parámetros**
   
   - Para excluir los espacios de nombres de Kubernetes de la evaluación de directivas, especifique la lista de espacios de nombres en el parámetro **Exclusiones de los espacios de nombres**. Se recomienda excluir: _kube-system_.

1. Seleccione **Revisar + crear**.

Alternativamente, use la opción de inicio rápido [Assign a policy - Portal](../assign-policy-portal.md) (Asignar una directiva - Portal) para encontrar y asignar una directiva de AKS. Busque una definición de directiva de Kubernetes en lugar del ejemplo "audit vms".

> [!IMPORTANT]
> Las directivas integradas en la categoría **Kubernetes** solo se usan con AKS. Para obtener una lista de las directivas integradas, vea [Ejemplos de Kubernetes](../samples/built-in-policies.md#kubernetes).

## <a name="logging"></a>Registro

### <a name="azure-policy-add-on-logs"></a>Registros del complemento de Azure Policy

Como controlador o contenedor de Kubernetes, el complemento de Azure Policy y Gatekeeper mantienen registros en el clúster de AKS. Los registros se exponen en la página **Insights** (Detalles) del clúster AKS. Para obtener más información, consulte [Comprender el rendimiento del clúster de AKS con Azure Monitor para contenedores](../../../azure-monitor/insights/container-insights-analyze.md).

## <a name="remove-the-add-on"></a>Eliminar el complemento

Para eliminar el complemento de Azure Policy del clúster de AKS, use Azure Portal o la CLI de Azure:

- Portal de Azure

  1. Inicie el servicio AKS en Azure Portal haciendo clic en **Todos los servicios**; a continuación, busque y seleccione **Servicios de Kubernetes**.

  1. Seleccione el clúster de AKS en el que quiera deshabilitar el complemento de Azure Policy.

  1. Seleccione **Policies (preview)** (Directivas [versión preliminar]) en el lado izquierdo de la página de servicio de Kubernetes.

     ![Directivas del clúster de AKS](../media/rego-for-aks/policies-preview-from-aks-cluster.png)

  1. En la página principal, pulse **Disable add-on** (Deshabilitar complemento).

     ![Deshabilitar Azure Policy para el complemento de AKS](../media/rego-for-aks/disable-policy-add-on.png)

- Azure CLI

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  az aks disable-addons --addons azure-policy --name MyAKSCluster --resource-group MyResourceGroup
  ```

## <a name="diagnostic-data-collected-by-azure-policy-add-on"></a>Datos de diagnóstico recopilados por el complemento de Azure Policy

El complemento de Azure Policy para Kubernetes recopila datos de diagnóstico de clúster limitados. Estos datos de diagnóstico son datos técnicos esenciales relacionados con el software y el rendimiento. Se utiliza para lo siguiente:

- Mantener actualizado el complemento de Azure Policy
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
