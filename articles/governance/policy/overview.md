---
title: Introducción a Azure Policy
description: Azure Policy es un servicio de Azure que se usa para crear, asignar y administrar las definiciones de directivas en el entorno de Azure.
ms.date: 11/25/2019
ms.topic: overview
ms.custom: fasttrack-edit
ms.openlocfilehash: e886f37a8d7f1395b5c831e81e600ecc6e2dd20f
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "79290353"
---
# <a name="what-is-azure-policy"></a>¿Qué es Azure Policy?

La gobernanza garantiza que cualquier organización puede alcanzar sus objetivos si usa TI de forma eficaz. Para satisfacer este requisito, aporta claridad entre los proyectos de TI y los objetivos empresariales.

¿Su empresa experimenta muchos problemas de TI que parecen no resolverse nunca? Una buena gobernanza de TI implica planear las iniciativas y establecer prioridades a nivel estratégico que faciliten la administración y eviten problemas. Es en esta necesidad estratégica donde Azure Policy entra en juego.

Azure Policy es un servicio de Azure que se usa para crear, asignar y administrar directivas. Dichas directivas aplican distintas reglas y efectos a los recursos, con el fin de que estos sigan siendo compatibles con los estándares corporativos y los acuerdos de nivel de servicio. Azure Policy satisface esta necesidad mediante la evaluación de los recursos que incumplen las directivas asignadas. Todos los datos que almacena Azure Policy se cifran en reposo.

Por ejemplo, puede haber una directiva que permita solo un tamaño de SKU concreto de las máquinas virtuales de un entorno. Una vez que se implementa esta directiva, se evalúa el cumplimiento de los recursos nuevos y existentes. Con el tipo correcto de directiva, se puede conseguir el cumplimiento de los recursos existentes. Más adelante se analizará con más detalles cómo crear e implementar directivas con Azure Policy.

> [!IMPORTANT]
> La evaluación de cumplimiento de Azure Policy se ofrece ahora para todas las asignaciones independientemente del nivel de precios. Si las asignaciones no muestran los datos de cumplimiento, asegúrese de que la suscripción está registrada con el proveedor de recursos Microsoft.PolicyInsights.

[!INCLUDE [azure-lighthouse-supported-service](../../../includes/azure-lighthouse-supported-service.md)]

## <a name="how-is-it-different-from-rbac"></a>¿En qué se diferencia de RBAC?

Hay algunas diferencias importantes entre Azure Policy y el control de acceso basado en rol (RBAC). RBAC se centra en las acciones del usuario en ámbitos diferentes. Se le podría agregar al rol de colaborador de un grupo de recursos, para que pueda realizar cambios en dicho grupo. Azure Policy se centra en las propiedades de los recursos durante la implementación y para todos los recursos existentes. Azure Policy controla las propiedades como los tipos y las ubicaciones de los recursos. A diferencia de RBAC, Azure Policy es un sistema que permite de manera predeterminada y niega explícitamente.

### <a name="rbac-permissions-in-azure-policy"></a>Permisos de RBAC en Azure Policy

Azure Policy dispone de varios permisos, conocidos como operaciones, en dos proveedores de recursos:

- [Microsoft.Authorization](../../role-based-access-control/resource-provider-operations.md#microsoftauthorization)
- [Microsoft.PolicyInsights](../../role-based-access-control/resource-provider-operations.md#microsoftpolicyinsights)

Muchos roles integrados conceden permiso a recursos de Azure Policy. El rol **Colaborador de directiva de recursos** incluye la mayoría de las operaciones de Azure Policy. El rol **Propietario** tiene derechos completos. Tanto el rol **Colaborador** como **Lector** pueden usar todas las operaciones de lectura de Azure Policy, pero el rol **Colaborador** también puede activar la corrección.

Si ninguno de los roles integrados tiene los permisos necesarios, cree un [rol personalizado](../../role-based-access-control/custom-roles.md).

## <a name="policy-definition"></a>Definición de directiva

El proceso de creación e implementación de una directiva en Azure Policy comienza con la creación de una definición de directiva. Cada definición de directiva tiene condiciones que regulan su aplicación. Además, tiene un efecto definido que se produce cuando se cumplen las condiciones.

En Azure Policy, se ofrecen varias directivas integradas que están disponibles de manera predeterminada. Por ejemplo:

- **SKU permitidas de cuentas de almacenamiento**: determina si una cuenta de almacenamiento que se va a implementar se engloba dentro de un conjunto de tamaños de SKU. Su efecto es denegar todas las cuentas de almacenamiento que no cumplen el conjunto de tamaños de SKU definidos.
- **Tipo de recurso permitido**: define los tipos de recursos que se pueden implementar. Su efecto es denegar todos los recursos que no forman parte de esta lista definida.
- **Ubicaciones permitidas**: restringe las ubicaciones disponibles para los nuevos recursos. Su efecto se utiliza para exigir los requisitos de cumplimiento de replicación geográfica.
- **SKU de máquina virtual permitidas**: especifica un conjunto de SKU de máquina virtual que se pueden implementar.
- **Agregar una etiqueta a los recursos**: aplica una etiqueta obligatoria y su valor predeterminado si la solicitud de implementación no la especifica.
- **Aplicar una etiqueta y su valor**: aplica una etiqueta obligatoria y su valor a un recurso.
- **Tipos de recursos no permitidos**: impide la implementación de una lista de tipos de recursos.

Para implementar estas definiciones de directiva (tanto las definiciones integradas como las personalizadas), será preciso asignarlas. Puede asignar cualquiera de estas directivas a través de Azure Portal, PowerShell o la CLI de Azure.

La evaluación de la directiva se realiza con distintas acciones, como la asignación de directivas o las actualizaciones de directiva. Para obtener una lista completa, vea [desencadenadores de evaluación de directivas](./how-to/get-compliance-data.md#evaluation-triggers).

Para obtener más información sobre las estructuras de las definiciones de directiva, consulte [Estructura de definición de directiva](./concepts/definition-structure.md).

## <a name="policy-assignment"></a>Asignación de directiva

Una asignación de directiva es una definición de directiva que se asignó para que ocurra dentro de un ámbito específico. Este ámbito puede ir desde un [grupo de administración](../management-groups/overview.md) a un recurso individual. El término *ámbito* hace referencia a todos los recursos, grupos de recursos, suscripciones o grupos de administración a los que se asigna la definición de directiva. Todos los recursos secundarios heredan las asignaciones de directivas. Este diseño conlleva que una directiva aplicada a un grupo de recursos también se aplique a los recursos de dicho grupo. Sin embargo, puede excluir un subámbito de la asignación de directiva.

Por ejemplo, en el ámbito de la suscripción, puede asignar una directiva que impida la creación de recursos de red. También podría excluir un grupo de recursos en la suscripción que está diseñado para la infraestructura de red. De esta forma, concede acceso a este grupo de recursos de red a los usuarios de confianza con la creación de recursos de red.

En otro ejemplo, es posible que desee asignar una directiva de lista de permitidos de tipos de recursos al nivel de grupo de administración. Y después asigne una directiva más permisiva (para permitir más tipos de recursos) en un grupo de administración secundario o incluso directamente en las suscripciones. Sin embargo, este ejemplo podría no funcionar porque la directiva es un sistema de denegación explícito. En su lugar, debe excluir el grupo de administración secundario o la suscripción de la asignación de directivas a nivel de grupo de administración. Después, asigne la directiva más permisiva en el grupo de administración secundario o en el nivel de suscripción. Si alguna directiva tiene como resultado la denegación de un recurso, la única manera de permitir el recurso es modificar la directiva de denegación.

Para más información sobre la configuración de las definiciones y asignaciones de directiva mediante el portal, vea [Create a policy assignment to identify non-compliant resources in your Azure environment](assign-policy-portal.md) (Creación de una asignación de directiva para identificar recursos no compatibles en el entorno de Azure). También hay pasos disponibles para [PowerShell](assign-policy-powershell.md) y la [CLI de Azure](assign-policy-azurecli.md).

## <a name="policy-parameters"></a>Parámetros de directiva

Los parámetros de directiva permiten simplificar la administración de directivas reduciendo el número de definiciones de directiva que debe crear. Cuando crea una definición de directiva, puede definir parámetros para que sea más genérica. Después, puede volver a usar esa definición de directiva para diferentes escenarios. Para ello, transmita distintos valores al asignar la definición de directiva. Por ejemplo, especifique un conjunto de ubicaciones para una suscripción.

Los parámetros se definen cuando se crea una definición de directiva. Cuando se define un parámetro, se le asigna un nombre y, de manera opcional, un valor. Por ejemplo, podría definir un parámetro para una directiva denominada *location*. Después, puede asignarle valores diferentes, como *EastUS* o *WestUS*, al asignar una directiva.

Para más información sobre los parámetros de directiva, vea [Estructura de definición: Parámetros](./concepts/definition-structure.md#parameters).

## <a name="initiative-definition"></a>Definición de iniciativa

Una definición de iniciativa es una colección de definiciones de directiva personalizadas para alcanzar un único objetivo general. Las definiciones de iniciativa simplifican la administración y asignación de las definiciones de directiva. Tal simplificación se realiza mediante la agrupación de un conjunto de directivas como un solo elemento. Por ejemplo, podría crear una iniciativa titulada **Habilitar la supervisión en Azure Security Center**, con el objetivo de supervisar todas las recomendaciones de seguridad disponibles en Azure Security Center.

> [!NOTE]
> El SDK, como CLI de Azure y Azure PowerShell, usa propiedades y parámetros denominados **PolicySet** para hacer referencia a las iniciativas.

En esta iniciativa, tendría definiciones de directiva como las siguientes:

- **Supervisar SQL Database sin cifrar en Security Center**, para supervisar servidores y bases de datos SQL sin cifrar.
- **Supervisar las vulnerabilidades del SO en Security Center**, para supervisar los servidores que no cumplen con la base de referencia configurada.
- **Supervisar Endpoint Protection omitido en Security Center**, para supervisar los servidores que no tienen instalado un agente de protección de los puntos de conexión.

## <a name="initiative-assignment"></a>Asignación de iniciativa

Al igual que una asignación de directiva, una asignación de iniciativa es una definición de iniciativa que se asigna a un ámbito específico. Las asignaciones de iniciativa disminuyen la necesidad de crear varias definiciones de iniciativa para cada ámbito. Este ámbito también puede abarcar desde un grupo de administración a un grupo de recursos.

Cada iniciativa se puede asignar a diferentes ámbitos. Una iniciativa se puede asignar tanto a **subscriptionA** como a **subscriptionB**.

## <a name="initiative-parameters"></a>Parámetros de iniciativa

Al igual que los parámetros de directiva, los parámetros de iniciativa permiten simplificar la administración de iniciativas mediante la reducción de la redundancia. Los parámetros de iniciativa son aquellos que las definiciones de directiva usan dentro de la iniciativa.

Por ejemplo, imagine un escenario en el que tiene la definición de una iniciativa (**initiativeC**), con las definiciones de directiva **policyA** y **policyB**, y que cada una de ellas espera un tipo diferente de parámetro:

| Directiva | Nombre del parámetro |tipo del parámetro  |Nota: |
|---|---|---|---|
| policyA | allowedLocations | array  |Este parámetro espera una lista de cadenas para un valor, porque el tipo de parámetro está definido como una matriz |
| policyB | allowedSingleLocation |string |Este parámetro espera una palabra para un valor, porque el tipo de parámetro se definió como una cadena |

En este escenario, tiene tres opciones en el momento de definir los parámetros de iniciativa para **initiativeC**:

- Use los parámetros de las definiciones de directiva dentro de esta iniciativa: en este ejemplo, *allowedLocations* y *allowedSingleLocation* se convierten en parámetros de iniciativa para **initiativeC**.
- Proporcione valores a los parámetros de las definiciones de directiva dentro de esta definición de iniciativa. En este ejemplo, puede proporcionar una lista de ubicaciones al **parámetro de policyA: allowedLocations** y al **parámetro de policyB: allowedSingleLocation**. También puede proporcionar valores cuando asigne esta iniciativa.
- Proporcione una lista de las opciones de *value* que se puede usar cuando se asigna esta iniciativa. Cuando asigna esta iniciativa, los parámetros inherentes de las definiciones de directiva dentro de la iniciativa solo pueden tener valores provenientes de esta lista.

Al crear las opciones de valor en una definición de iniciativa, no se puede proporcionar un valor diferente durante la asignación de la iniciativa, porque no forma parte de la lista.

## <a name="maximum-count-of-azure-policy-objects"></a>Número máximo de objetos de Azure Policy

[!INCLUDE [policy-limits](../../../includes/azure-policy-limits.md)]

## <a name="recommendations-for-managing-policies"></a>Recomendaciones para la administración de directivas

A continuación, se ofrecen algunos consejos e indicaciones que se deben tener en cuenta:

- Comience con un efecto de auditoría en lugar de un efecto de denegación para realizar un seguimiento del impacto de la definición de directiva en los recursos de su entorno. Si ya tiene scripts implementados para escalar automáticamente las aplicaciones, establecer un efecto de denegación puede dificultar las tareas de automatización que ya tiene implementadas.

- Considere las jerarquías organizativas al crear definiciones y asignaciones. Se recomienda crear definiciones a niveles más altos, como suscripción o grupo de administración. A continuación, cree la asignación en el siguiente nivel secundario. Si crea una definición en un grupo de administración, se puede limitar la asignación a una suscripción o un grupo de recursos dentro de ese grupo de administración.

- Se recomienda crear y asignar las definiciones de iniciativa incluso para una definición de directiva única.
  Por ejemplo, tiene la definición de directiva *policyDefA* y la crea en la definición de iniciativa *initiativeDefC*. Si crea otra definición de directiva más adelante para *policyDefB* con objetivos similares a *policyDefA*, puede agregarla a *initiativeDefC* y realizar un seguimiento conjunto.

- Una vez creada una asignación de iniciativa, las definiciones agregadas a la iniciativa también forman parte de esas asignaciones de iniciativas.

- Cuando se evalúa una asignación de iniciativa, también se evalúan todas las directivas incluidas en la iniciativa.
  Si necesita evaluar una directiva de forma individual, es mejor no incluirla en una iniciativa.

## <a name="video-overview"></a>Introducción en vídeo

La siguiente introducción de Azure Policy es de la compilación 2018. Para descargar diapositivas o el vídeo, visite [Govern your Azure environment through Azure Policy](https://channel9.msdn.com/events/Build/2018/THR2030) (Gobierno de un entorno de Azure mediante Azure Policy) en Channel 9.

> [!VIDEO https://www.youtube.com/embed/dxMaYF2GB7o]

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene información general acerca de Azure Policy y algunos de los conceptos clave, se recomienda seguir estos pasos:

- [Asignación de una definición de directiva con el portal](./assign-policy-portal.md).
- [Asignación de una definición de directiva con la CLI de Azure](./assign-policy-azurecli.md).
- [Asignación de una definición de directiva con PowerShell](./assign-policy-powershell.md).
