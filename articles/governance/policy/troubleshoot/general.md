---
title: Solución de errores comunes
description: Aprenda a solucionar problemas relacionados con la creación de definiciones de directivas, los diversos SDK y el complemento de Kubernetes.
ms.date: 10/30/2020
ms.topic: troubleshooting
ms.openlocfilehash: 74b622dd41fb28e845a35780e5d06588189ec029
ms.sourcegitcommit: 4b76c284eb3d2b81b103430371a10abb912a83f4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2020
ms.locfileid: "93146286"
---
# <a name="troubleshoot-errors-using-azure-policy"></a>Solución de problemas mediante Azure Policy

Es posible que se produzcan errores al crear definiciones de directivas, trabajar con SDK o configurar el complemento [Azure Policy para Kubernetes](../concepts/policy-for-kubernetes.md). En este artículo se describen diversos errores generales que pueden producirse y cómo resolverlos.

## <a name="finding-error-details"></a>Búsqueda de detalles del error

La ubicación de los detalles del error depende de la acción que provoca el error.

- Cuando trabaje con una directiva personalizada, pruébela en Azure Portal para obtener comentarios de linting o revisar los [datos de cumplimiento](../how-to/get-compliance-data.md) resultantes para ver cómo se evaluaron los recursos.
- Si trabaja con varios SDK, los SDK proporcionan detalles sobre por qué se produjo un error en la función.
- Si trabaja con el complemento para Kubernetes, empiece por [registrarse](../concepts/policy-for-kubernetes.md#logging) en el clúster.

## <a name="general-errors"></a>Errores generales

### <a name="scenario-alias-not-found"></a>Escenario: Alias no encontrado

#### <a name="issue"></a>Incidencia

Azure Policy usa [alias](../concepts/definition-structure.md#aliases) para asignarlos a las propiedades de Azure Resource Manager.

#### <a name="cause"></a>Causa

Se ha utilizado un alias incorrecto o inexistente en una definición de directiva.

#### <a name="resolution"></a>Solución

En primer lugar, compruebe que la propiedad de Resource Manager tiene un alias. Use la [extensión de Azure Policy para Visual Studio Code](../how-to/extension-for-vscode.md), [Azure Resource Graph](../../resource-graph/samples/starter.md#distinct-alias-values) o el SDK para buscar los alias disponibles. Si el alias de una propiedad de Resource Manager no existe, cree una incidencia de soporte técnico.

### <a name="scenario-evaluation-details-not-up-to-date"></a>Escenario: Detalles de evaluación no actualizados

#### <a name="issue"></a>Problema

Un recurso tiene el estado "No iniciado" o los detalles de cumplimiento no son actuales.

#### <a name="cause"></a>Causa

Cualquier nueva asignación de directiva o iniciativa tarda unos 30 minutos en aplicarse. Los recursos nuevos o actualizados dentro del ámbito de una asignación existente estarán disponibles unos 15 minutos más tarde. Un examen de cumplimiento estándar se produce cada 24 horas. Para más información, consulte los [desencadenadores de evaluación](../how-to/get-compliance-data.md#evaluation-triggers).

#### <a name="resolution"></a>Solución

En primer lugar, espere la cantidad de tiempo adecuada para que se complete una evaluación y que los resultados de cumplimiento estén disponibles en Azure Portal o el SDK. Para iniciar un nuevo examen de evaluación con Azure PowerShell o la API REST, consulte [Examen de evaluación a petición](../how-to/get-compliance-data.md#on-demand-evaluation-scan).

### <a name="scenario-compliance-not-as-expected"></a>Escenario: El cumplimiento no es el esperado

#### <a name="issue"></a>Incidencia

Un recurso no está en el estado de evaluación, ya sea _Compatible_ o _No compatible_ , que es lo que se espera de ese recurso.

#### <a name="cause"></a>Causa

El recurso no está en el ámbito correcto de la asignación de directiva o la definición de directiva no funciona según lo previsto.

#### <a name="resolution"></a>Resolución

Siga estos pasos para solucionar los problemas de la definición de la directiva:

1. En primer lugar, espere la cantidad de tiempo adecuada para que se complete una evaluación y que los resultados de cumplimiento estén disponibles en Azure Portal o el SDK. Para iniciar un nuevo examen de evaluación con Azure PowerShell o la API REST, consulte [Examen de evaluación a petición](../how-to/get-compliance-data.md#on-demand-evaluation-scan).
1. Compruebe que los parámetros de asignación y el ámbito de asignación estén configurados correctamente.
1. Compruebe el [modo de definición de directiva](../concepts/definition-structure.md#mode):
   - Modo "all" para todos los tipos de recursos.
   - Modo "indexed" si la definición de la directiva comprueba las etiquetas o la ubicación.
1. Compruebe que el ámbito del recurso no sea [excluido](../concepts/assignment-structure.md#excluded-scopes) ni [exento](../concepts/exemption-structure.md).
1. Si la compatibilidad de una asignación de directiva muestra `0/0` recursos, se determinó que ningún recurso era aplicable en el ámbito de la asignación. Compruebe la definición de la directiva y el ámbito de la asignación.
1. En el caso de un recurso no compatible que se esperaba que fuera compatible, consulte la [determinación de las causas de incumplimiento](../how-to/determine-non-compliance.md). La comparación de la definición con el valor de propiedad evaluado indica por qué un recurso no era compatible.
   - Si el **valor de destino** es incorrecto, revise la definición de la directiva.
   - Si el **valor actual** es incorrecto, valide la carga de recursos a través de `resources.azure.com`.
1. Consulte [Solución de problemas: La aplicación no es la esperada](#scenario-enforcement-not-as-expected) para conocer los otros problemas y soluciones comunes.

Si todavía tiene un problema con la definición de la directiva integrada duplicada y personalizada, o la definición personalizada, cree una incidencia de soporte técnico en **Creación de una directiva** para dirigir el problema correctamente.

### <a name="scenario-enforcement-not-as-expected"></a>Escenario: La aplicación no es la esperada

#### <a name="issue"></a>Problema

Un recurso en el que se espera que actúe Azure Policy no existe y no hay ninguna entrada en el [registro de actividad de Azure](../../../azure-monitor/platform/platform-logs-overview.md).

#### <a name="cause"></a>Causa

La asignación de directivas se ha configurado para un valor de [enforcementMode](../concepts/assignment-structure.md#enforcement-mode) de _Deshabilitado_. Aunque el modo de cumplimiento esté deshabilitado, el efecto de la directiva no se aplica y no hay ninguna entrada en el registro de actividad.

#### <a name="resolution"></a>Resolución

Siga estos pasos para solucionar los problemas de aplicación de la asignación de la directiva:

1. En primer lugar, espere la cantidad de tiempo adecuada para que se complete una evaluación y que los resultados de cumplimiento estén disponibles en Azure Portal o el SDK. Para iniciar un nuevo examen de evaluación con Azure PowerShell o la API REST, consulte [Examen de evaluación a petición](../how-to/get-compliance-data.md#on-demand-evaluation-scan).
1. Compruebe que los parámetros de asignación y el ámbito de asignación estén configurados correctamente y que **enforcementMode** esté _Habilitado_. 
1. Compruebe el [modo de definición de directiva](../concepts/definition-structure.md#mode):
   - Modo "all" para todos los tipos de recursos.
   - Modo "indexed" si la definición de la directiva comprueba las etiquetas o la ubicación.
1. Compruebe que el ámbito del recurso no sea [excluido](../concepts/assignment-structure.md#excluded-scopes) ni [exento](../concepts/exemption-structure.md).
1. Compruebe que la carga de recursos coincide con la lógica de la directiva. Esto puede realizarse mediante la [captura de un seguimiento HAR](../../../azure-portal/capture-browser-trace.md) o la revisión de las propiedades de la plantilla de Resource Manager.
1. Consulte [Solución de problemas: El cumplimiento no es el esperado](#scenario-compliance-not-as-expected) para conocer los otros problemas y soluciones comunes.

Si todavía tiene un problema con la definición de la directiva integrada duplicada y personalizada, o la definición personalizada, cree una incidencia de soporte técnico en **Creación de una directiva** para dirigir el problema correctamente.

### <a name="scenario-denied-by-azure-policy"></a>Escenario: Denegado por Azure Policy

#### <a name="issue"></a>Problema

Se ha denegado la creación o actualización de un recurso.

#### <a name="cause"></a>Causa

Una asignación de directivas en el ámbito en el que se encuentra el recurso nuevo o actualizado cumple los criterios de una definición de directiva con un efecto de [denegación](../concepts/effects.md#deny). Se impide que los recursos que reúnan estas condiciones se creen o actualicen.

#### <a name="resolution"></a>Solución

El mensaje de error de una asignación de directiva de denegación incluye los identificadores de definición de directiva y de asignación de directivas. Si se pierde la información de error en el mensaje, también está disponible en el [registro de actividad](../../../azure-monitor/platform/activity-log.md#view-the-activity-log). Use esta información para obtener más detalles para comprender las restricciones de recursos y ajustar las propiedades de los recursos de la solicitud para que coincidan con los valores permitidos.

## <a name="template-errors"></a>Errores de plantilla

### <a name="scenario-policy-supported-functions-processed-by-template"></a>Escenario: Funciones admitidas por la directiva que procesa una plantilla

#### <a name="issue"></a>Incidencia

Azure Policy admite varias funciones y funciones de plantilla de Azure Resource Manager (plantilla de ARM) que solo están disponibles en una definición de directiva. Resource Manager procesa estas funciones como parte de una implementación en lugar de como parte de una definición de directiva.

#### <a name="cause"></a>Causa

El uso de funciones admitidas, como `parameter()` o `resourceGroup()`, da como resultado el resultado procesado de la función en el momento de la implementación en lugar de mantener la función para que la definición de la directiva y el motor de Azure Policy la procesen.

#### <a name="resolution"></a>Solución

Para pasar una función para que forme parte de una definición de directiva, escape la cadena completa con `[` de forma que la propiedad se parezca a `[[resourceGroup().tags.myTag]`. El carácter de escape hace que Resource Manager trate el valor como una cadena al procesar la plantilla. Después, Azure Policy coloca la función en la definición de la directiva para que pueda ser dinámica según lo previsto. Para obtener más información, consulte [Sintaxis y expresiones de las plantillas de Azure Resource Manager](../../../azure-resource-manager/templates/template-expressions.md).

## <a name="add-on-for-kubernetes-installation-errors"></a>Complemento para errores de instalación de Kubernetes

### <a name="scenario-install-using-helm-chart-fails-on-password"></a>Escenario: Error en la instalación mediante el gráfico de Helm al especificar la contraseña

#### <a name="issue"></a>Problema

El comando `helm install azure-policy-addon` produce un error y muestra uno de los siguientes mensajes:

- `!: event not found`
- `Error: failed parsing --set data: key "<key>" has no value (cannot end with ,)`

#### <a name="cause"></a>Causa

La contraseña generada incluye una coma (`,`) que hace que el gráfico de Helm se divida.

#### <a name="resolution"></a>Solución

Use una barra diagonal inversa (`\`) como carácter de escape de la coma (`,`) cuando se ejecute `helm install azure-policy-addon`.

### <a name="scenario-install-using-helm-chart-fails-as-name-already-exists"></a>Escenario: Error en la instalación con el gráfico de Helm porque el nombre ya existe

#### <a name="issue"></a>Problema

El comando `helm install azure-policy-addon` produce un error y muestra uno de los siguientes mensajes:

- `Error: cannot re-use a name that is still in use`

#### <a name="cause"></a>Causa

El gráfico de Helm llamado `azure-policy-addon` ya se ha instalado completa o parcialmente.

#### <a name="resolution"></a>Solución

Siga las instrucciones para [quitar el complemento Azure Policy para Kubernetes](../concepts/policy-for-kubernetes.md#remove-the-add-on) y, a continuación, vuelva a ejecutar el comando `helm install azure-policy-addon`.

### <a name="scenario-azure-virtual-machine-user-assigned-identities-are-replaced-by-system-assigned-managed-identities"></a>Escenario: Las identidades asignadas por el usuario de una máquina virtual de Azure se sustituyen por identidades administradas asignadas por el sistema

#### <a name="issue"></a>Incidencia

Después de asignar las iniciativas de directiva de configuración de invitado a la configuración de auditoría dentro de las máquinas, las identidades administradas asignadas por el usuario que se asignaron a la máquina ya no se asignan. Solo se asigna una identidad administrada asignada por el sistema.

#### <a name="cause"></a>Causa

Las definiciones de directiva usadas anteriormente en las definiciones de DeployIfNotExists de la configuración de invitado garantizaban que una identidad asignada por el sistema estuviese asignada a la máquina, pero que también quitaba las asignaciones de identidad asignadas por el usuario.

#### <a name="resolution"></a>Resolución

Las definiciones que anteriormente provocaban este problema aparecen como \[En desuso\] y se reemplazaron por definiciones de directivas que administran los requisitos previos sin quitar la identidad administrada asignada por el usuario. Se requiere un paso manual. Elimine las asignaciones de directiva existentes marcadas como \[En desuso\] y reemplácelas por la iniciativa de directiva de requisitos previos y las definiciones de directiva actualizadas que tengan el mismo nombre que el original.

Para obtener una descripción detallada, consulte la siguiente entrada de blog:

[Cambio importante publicado para las directivas de auditoría de configuración de invitado](https://techcommunity.microsoft.com/t5/azure-governance-and-management/important-change-released-for-guest-configuration-audit-policies/ba-p/1655316)

## <a name="add-on-for-kubernetes-general-errors"></a>Complemento para errores generales de Kubernetes

### <a name="scenario-add-on-doesnt-work-with-aks-clusters-on-version-119-preview"></a>Escenario: El complemento no funciona con clústeres de AKS en la versión 1.19 (versión preliminar)

#### <a name="issue"></a>Problema

Los clústeres de la versión 1.19 devuelven este error a través del controlador de Gatekeeper y los pods de webhook de directiva:

```
2020/09/22 20:06:55 http: TLS handshake error from 10.244.1.14:44282: remote error: tls: bad certificate
```

#### <a name="cause"></a>Causa

Los clústeres de AKS en la versión 1.19 (versión preliminar) todavía no son compatibles con el complemento de Azure Policy.

#### <a name="resolution"></a>Solución

Evite el uso de Kubernetes 1.19 (versión preliminar) con el complemento de Azure Policy. El complemento se puede usar con cualquier versión disponible con carácter general, como 1.16, 1.17 o 1.18.

### <a name="scenario-add-on-is-unable-to-reach-the-azure-policy-service-endpoint-due-to-egress-restrictions"></a>Escenario: El complemento no puede tener acceso al punto de conexión de servicio de Azure Policy debido a restricciones de salida.

#### <a name="issue"></a>Problema

El complemento no puede tener acceso al punto de conexión de servicio de Azure Policy y devuelve uno de los siguientes errores:

- `failed to fetch token, service not reachable`
- `Error getting file "Get https://raw.githubusercontent.com/Azure/azure-policy/master/built-in-references/Kubernetes/container-allowed-images/template.yaml: dial tcp 151.101.228.133.443: connect: connection refused`

#### <a name="cause"></a>Causa

Este problema se produce cuando se bloquea una salida de clúster.

#### <a name="resolution"></a>Solución

Asegúrese de que los dominios y puertos de los siguientes artículos están abiertos:

- [Reglas de red de salida y FQDN necesarios para clústeres de AKS](../../../aks/limit-egress-traffic.md#required-outbound-network-rules-and-fqdns-for-aks-clusters)
- [Instalación del complemento de Azure Policy de Kubernetes habilitado para Azure Arc (versión preliminar)](../concepts/policy-for-kubernetes.md#install-azure-policy-add-on-for-azure-arc-enabled-kubernetes)

### <a name="scenario-add-on-is-unable-to-reach-the-azure-policy-service-endpoint-due-to-aad-pod-identity-configuration"></a>Escenario: El complemento no puede tener acceso al punto de conexión de servicio de Azure Policy debido a la configuración de aad-pod-identity.

#### <a name="issue"></a>Problema

El complemento no puede tener acceso al punto de conexión de servicio de Azure Policy y devuelve uno de los siguientes errores:

- `azure.BearerAuthorizer#WithAuthorization: Failed to refresh the Token for request to https://gov-prod-policy-data.trafficmanager.net/checkDataPolicyCompliance?api-version=2019-01-01-preview: StatusCode=404`
- `adal: Refresh request failed. Status Code = '404'. Response body: getting assigned identities for pod kube-system/azure-policy-8c785548f-r882p in CREATED state failed after 16 attempts, retry duration [5]s, error: <nil>`

#### <a name="cause"></a>Causa

Este error se produce cuando _add-pod-identity_ está instalado en el clúster y los pods de _kube-system_ no se excluyen en _aad-pod-identity_.

Los pods de Identidad administrada del nodo del componente _aad-pod-identity_ modifican las tablas de IP de los nodos para interceptar las llamadas que se realizan al punto de conexión de Azure Instance Metadata. Esta configuración implica que NMI intercepte toda solicitud realizada al punto de conexión de Metadata, incluso aunque el pod no utilice _aad-pod-identity_.
La CRD de **AzurePodIdentityException** se puede configurar para informar a _aad-pod-identity_ de que las solicitudes dirigidas al punto de conexión de Metadata que se originen en un pod que coincida con las etiquetas definidas en la CRD deban pasar por el servidor proxy sin que se procesen en NMI.

#### <a name="resolution"></a>Solución

Excluya los pods del sistema que tienen la etiqueta `kubernetes.azure.com/managedby: aks` del espacio de nombres _kube-system_ en _aad-pod-identity_ mediante la configuración de la CRD de **AzurePodIdentityException**.

Para más información, consulte este artículo acerca de [cómo deshabilitar AAD Pod Identity en una aplicación o pod específicos](https://azure.github.io/aad-pod-identity/docs/configure/application_exception).

Para configurar una excepción, vea este ejemplo:

```yaml
apiVersion: "aadpodidentity.k8s.io/v1"
kind: AzurePodIdentityException
metadata:
  name: mic-exception
  namespace: default
spec:
  podLabels:
    app: mic
    component: mic
---
apiVersion: "aadpodidentity.k8s.io/v1"
kind: AzurePodIdentityException
metadata:
  name: aks-addon-exception
  namespace: kube-system
spec:
  podLabels:
    kubernetes.azure.com/managedby: aks
```

### <a name="scenario-the-resource-provider-isnt-registered"></a>Escenario: El proveedor de recursos no está registrado.

#### <a name="issue"></a>Problema

El complemento puede tener acceso al extremo del servicio Azure Policy, pero ve el siguiente error:

```
The resource provider 'Microsoft.PolicyInsights' is not registered in subscription '{subId}'. See https://aka.ms/policy-register-subscription for how to register subscriptions.
```

#### <a name="cause"></a>Causa

El proveedor de recursos `Microsoft.PolicyInsights` no está registrado y se debe registrar para que el complemento obtenga las definiciones de directiva y devuelva los datos de cumplimiento.

#### <a name="resolution"></a>Solución

Registre el proveedor de recursos `Microsoft.PolicyInsights`. Para obtener instrucciones, consulte [Registro del proveedor de recursos](../../../azure-resource-manager/management/resource-providers-and-types.md#register-resource-provider).

### <a name="scenario-the-subscript-is-disabled"></a>Escenario: El subíndice está deshabilitado.

#### <a name="issue"></a>Problema

El complemento puede tener acceso al extremo del servicio Azure Policy, pero ve el siguiente error:

```
The subscription '{subId}' has been disabled for azure data-plane policy. Please contact support.
```

#### <a name="cause"></a>Causa

Este error significa que la suscripción se determinó como problemática y se agregó la marca de características `Microsoft.PolicyInsights/DataPlaneBlocked` para bloquear la suscripción.

#### <a name="resolution"></a>Solución

Póngase en contacto con el equipo de características en `azuredg@microsoft.com` para investigar y resolver este problema. 

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

- Obtenga respuestas de expertos a través de [Preguntas y respuestas de Microsoft](/answers/topics/azure-policy.html).
- Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
- Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
