---
title: Preguntas más frecuentes sobre Azure Kubernetes Service (AKS)
description: Encuentre respuestas a algunas de las preguntas comunes sobre Azure Kubernetes Service (AKS).
ms.topic: conceptual
ms.date: 10/02/2019
ms.openlocfilehash: c4bb4328af5df7f729967c7b249847b2ab098770
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79497755"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Preguntas más frecuentes sobre Azure Kubernetes Service (AKS)

En este artículo se abordan las preguntas más frecuentes sobre Azure Kubernetes Service (AKS).

## <a name="which-azure-regions-currently-provide-aks"></a>¿Qué regiones de Azure proporcionan actualmente AKS?

Para obtener una lista completa de las regiones disponibles, consulte [AKS regions and availability][aks-regions] (Regiones y disponibilidad de AKS).

## <a name="does-aks-support-node-autoscaling"></a>¿AKS admite el escalado automático de los nodos?

Sí, la capacidad para escalar automáticamente los nodos de agente horizontalmente en AKS está disponible actualmente en versión preliminar. Consulte el artículo [Escalado automático de un clúster para satisfacer las necesidades de aplicación en AKS][aks-cluster-autoscaler]. El escalado automático de AKS se basa en el [escalador automático de Kubernetes][auto-scaler].

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>¿Puedo implementar AKS en mi red virtual existente?

Sí, puede implementar un clúster de AKS en una red virtual existente mediante la [característica de redes avanzadas][aks-advanced-networking].

## <a name="can-i-limit-who-has-access-to-the-kubernetes-api-server"></a>¿Puedo limitar quién tiene acceso al servidor de API de Kubernetes?

Sí, puede limitar el acceso al servidor de API de Kubernetes mediante los [intervalos IP autorizados del servidor de API][api-server-authorized-ip-ranges].

## <a name="can-i-make-the-kubernetes-api-server-accessible-only-within-my-virtual-network"></a>¿Puedo restringir el servidor de API de Kubernetes para que solo sea accesible en mi red virtual?

En este momento no es posible, pero está planeado. Puede realizar un seguimiento del progreso en el [repositorio de GitHub de AKS][private-clusters-github-issue].

## <a name="can-i-have-different-vm-sizes-in-a-single-cluster"></a>¿Puedo tener diferentes tamaños de máquina virtual en un único clúster?

Sí, puede usar diferentes tamaños de máquinas virtuales en el clúster de AKS mediante la creación de [varios grupos de nodos][multi-node-pools].

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>¿Se aplican las actualizaciones de seguridad a los nodos de agente de AKS?

Azure aplica automáticamente revisiones de seguridad a los nodos de Linux del clúster siguiendo una programación nocturna. Sin embargo, es responsabilidad suya asegurarse de que los nodos de Linux se reinician según sea necesario. Cuenta con varias opciones para reiniciar los nodos:

- Manualmente, mediante Azure Portal o la CLI de Azure.
- Mediante la actualización del clúster de AKS. Las actualizaciones del clúster [acordonan y purgan los nodos][cordon-drain] automáticamente, y luego ponen un nuevo nodo en línea con la imagen de Ubuntu más reciente y una nueva versión de revisión o una versión secundaria de Kubernetes. Para más información, consulte [Actualización de un clúster de Azure Kubernetes Service (AKS)][aks-upgrade].
- Mediante [Kured](https://github.com/weaveworks/kured), un demonio de reinicio de código abierto para Kubernetes. Kured se ejecuta como un elemento [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) y supervisa en todos los nodos la presencia de un archivo que indique que hace falta un reinicio. En el clúster, los reinicios del sistema operativo se administran con el mismo [proceso de acordonar y purgar][cordon-drain] que una actualización de clúster.

Para obtener más información sobre el uso de Kured, consulte [Apply security and kernel updates to nodes in AKS][node-updates-kured] (Aplicación de actualizaciones de kernel y de seguridad en los nodos de AKS).

### <a name="windows-server-nodes"></a>Nodos de Windows Server

Para los nodos de Windows Server (actualmente en versión preliminar en AKS), Windows Update no ejecuta ni aplica las actualizaciones más recientes de manera automática. En una programación normal del ciclo de versiones de Windows Update y su proceso de validación propio, debe realizar una actualización en el clúster y los grupos de nodos de Windows Server en el clúster de AKS. Este proceso de actualización crea nodos que ejecutan la imagen y las revisiones más recientes de Windows Server y elimina los nodos anteriores. Para obtener más información sobre este proceso, consulte [Actualización de un grupo de nodos en AKS][nodepool-upgrade].

## <a name="why-are-two-resource-groups-created-with-aks"></a>¿Por qué se crean dos grupos de recursos con AKS?

AKS se basa en diversos recursos de infraestructura de Azure, como los conjuntos de escalado de máquinas virtuales, las redes virtuales y los discos administrados. Esto le permite utilizar muchas de las funcionalidades básicas de la plataforma de Azure en el entorno de Kubernetes administrado que proporciona AKS. Por ejemplo, la mayoría de los tipos de máquinas virtuales de Azure se pueden usar directamente con AKS, y Azure Reservations se puede usar para recibir descuentos automáticamente en esos recursos.

Para habilitar esta arquitectura, cada implementación de AKS abarca dos grupos de recursos:

1. Debe cree el primer grupo de recursos. Este grupo solo contiene el recurso del servicio de Kubernetes. El proveedor de recursos de AKS crea automáticamente el segundo grupo de recursos durante la implementación. Un ejemplo del segundo grupo de recursos es *MC_myResourceGroup_myAKSCluster_eastus*. Para obtener información sobre cómo especificar el nombre de este segundo grupo de recursos, consulte la sección siguiente.
1. El segundo grupo de recursos, conocido como el *grupo de recursos del nodo*, contiene todos los recursos de infraestructura asociados con el clúster. Estos recursos incluyen las máquinas virtuales de nodos de Kubernetes, las redes virtuales y el almacenamiento. De forma predeterminada, el grupo de recursos del nodo tiene un nombre como *MC_myResourceGroup_myAKSCluster_eastus*. AKS elimina automáticamente el recurso del nodo cada vez que se elimina el clúster, por lo que solo se debe usar para los recursos que comparten el ciclo de vida del clúster.

## <a name="can-i-provide-my-own-name-for-the-aks-node-resource-group"></a>¿Puedo proporcionar mi propio nombre para el grupo de recursos del nodo de AKS?

Sí. De forma predeterminada, AKS asignará el nombre *MC_resourcegroupname_clustername_location* al grupo de recursos del nodo, pero también puede proporcionar su propio nombre.

Para especificar un nombre de su elección para el grupo de recursos, instale la versión de la extensión de la CLI de Azure [aks-preview][aks-preview-cli]*0.3.2* o una posterior. Cuando cree un clúster de AKS mediante el comando [az aks create][az-aks-create], use el parámetro *--node-resource-group* y especifique un nombre para el grupo de recursos. Si [usa una plantilla de Azure Resource Manager][aks-rm-template] para implementar un clúster de AKS, puede definir el nombre del grupo de recursos mediante la propiedad *nodeResourceGroup*.

* El proveedor de recursos de Azure crea automáticamente el grupo de recursos secundario en su propia suscripción.
* Solo puede especificar un nombre personalizado para el grupo de recursos cuando cree el clúster.

Mientras trabaje con el grupo de recursos del nodo, tenga en cuenta que no puede realizar lo siguiente:

* Especificar un grupo de recursos existente para el grupo de recursos del nodo.
* Especificar otra suscripción para el grupo de recursos del nodo.
* Cambiar el nombre del grupo de recursos del nodo una vez se haya creado el clúster.
* Especificar los nombres de los recursos administrados del grupo de recursos del nodo.
* Modificar o eliminar las etiquetas de los recursos administrados del grupo de recursos del nodo. (Puede ver la información adicional en la sección siguiente).

## <a name="can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-node-resource-group"></a>¿Puedo modificar etiquetas y otras propiedades de los recursos de AKS en el grupo de recursos del nodo?

Si modifica o elimina etiquetas creadas por Azure y otras propiedades de recursos en el grupo de recursos del nodo, es posible que obtenga resultados inesperados, como errores de escalado y actualización. AKS le permite crear y modificar las etiquetas personalizadas. Es posible que quiera crear o modificar etiquetas personalizadas, por ejemplo, para asignar un centro de costos o una unidad de negocio. Si modifica los recursos del grupo de recursos del nodo en el clúster de AKS, interrumpirá el objetivo de nivel de servicio (SLO). Para obtener más información, consulte [¿AKS ofrece un contrato de nivel de servicio?](#does-aks-offer-a-service-level-agreement)

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-admission-controllers-be-added-or-removed"></a>¿Qué controladores de admisión de Kubernetes admite AKS? ¿Se pueden agregar o eliminar los controladores de admisión?

AKS admite los siguientes [controladores de admisión][admission-controllers]:

- *NamespaceLifecycle*
- *LimitRanger*
- *ServiceAccount*
- *DefaultStorageClass*
- *DefaultTolerationSeconds*
- *MutatingAdmissionWebhook*
- *ValidatingAdmissionWebhook*
- *ResourceQuota*

Actualmente, no puede modificar la lista de controladores de admisión en AKS.

## <a name="is-azure-key-vault-integrated-with-aks"></a>¿Azure Key Vault se integra con AKS?

AKS no está integrado de forma nativa con Azure Key Vault en estos momentos. Sin embargo, el [proyecto FlexVolume de Azure Key Vault para Kubernetes][keyvault-flexvolume] permite realizar una integración directa desde pods de Kubernetes a los secretos de Key Vault.

## <a name="can-i-run-windows-server-containers-on-aks"></a>¿Puedo ejecutar contenedores de Windows Server en AKS?

Sí, los contenedores de Windows Server están disponibles en versión preliminar. Para ejecutar los contenedores de Windows Server en AKS, cree un grupo de nodos que ejecute Windows Server como sistema operativo invitado. Los contenedores de Windows Server solo pueden usar Windows Server 2019. Para empezar, vea [Creación de un clúster de AKS con un grupo de nodos de Windows Server][aks-windows-cli].

La compatibilidad de Windows Server con el grupo de nodos incluye algunas limitaciones que forman parte de la versión de Windows Server ascendente en el proyecto de Kubernetes. Para obtener más información sobre estas limitaciones, consulte [Windows Server containers in AKS limitations][aks-windows-limitations] (Limitaciones de los contenedores de Windows Server en AKS).

## <a name="does-aks-offer-a-service-level-agreement"></a>¿AKS ofrece un Acuerdo de Nivel de Servicio?

En un Acuerdo de Nivel de Servicio (SLA), el proveedor acuerda reembolsar al cliente el costo del servicio si no se pudo cumplir el nivel de servicio publicado. Dado que AKS es gratuito, no hay que reembolsar ningún costo, por lo que AKS no tiene ningún SLA formal. Sin embargo, AKS busca mantener una disponibilidad del 99,5 % como mínimo para el servidor de API de Kubernetes.

Es importante reconocer la diferencia entre la disponibilidad del servicio AKS, que hace referencia al tiempo de actividad del plano de control de Kubernetes, y la disponibilidad de la carga de trabajo específica que se ejecuta en Azure Virtual Machines. Aunque es posible que el plano de control no esté disponible si no está listo, las cargas de trabajo de clúster que se ejecutan en máquinas virtuales de Azure pueden seguir funcionando. Dado que las máquinas virtuales de Azure son recursos de pago, están respaldadas por un contrato de nivel de servicio financiero. Lea [aquí para obtener más detalles](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/) sobre el contrato de nivel de servicio de máquina virtual de Azure y cómo aumentar esa disponibilidad con características como [Availability Zones][availability-zones].

## <a name="why-cant-i-set-maxpods-below-30"></a>¿Por qué no puedo establecer el argumento maxPods por debajo de 30?

En AKS, puede establecer el valor `maxPods` al crear el clúster mediante el uso de las plantillas de la CLI de Azure y Azure Resource Manager. Sin embargo, Kubenet y la CNI de Azure requieren un *valor mínimo* (validado en el momento de la creación):

| Redes | Mínima | Máxima |
| -- | :--: | :--: |
| CNI de Azure | 30 | 250 |
| Kubenet | 30 | 110 |

Dado que AKS es un servicio administrado, debe implementar y administrar los complementos y los pods como parte del clúster. En el pasado, los usuarios podían definir un valor de `maxPods` menor que el valor que requerían los pods administrados para poder ejecutarse (por ejemplo, 30). Ahora, AKS calcula el número mínimo de pods mediante el uso de esta fórmula: ((maxPods o (maxPods * vm_count)) > mínimo de pods de complemento administrados.

Los usuarios no pueden reemplazar la validación de `maxPods` mínima.

## <a name="can-i-apply-azure-reservation-discounts-to-my-aks-agent-nodes"></a>¿Puedo aplicar descuentos de las reservas de Azure en mis nodos de agente de AKS?

Los nodos de agente de AKS se facturan como máquinas virtuales de Azure estándares, por lo que, si ha comprado [reservas Azure][reservation-discounts] para el tamaño de máquina virtual que está usando en AKS, los descuentos se aplican automáticamente.

## <a name="can-i-movemigrate-my-cluster-between-azure-tenants"></a>¿Puedo mover o migrar mi clúster entre inquilinos de Azure?

El comando `az aks update-credentials` se puede usar para mover un clúster de AKS entre inquilinos de Azure. Siga las instrucciones de [Elija actualizar o crear una entidad de servicio](https://docs.microsoft.com/azure/aks/update-credentials) y luego [actualice el clúster de AKS con las nuevas credenciales](https://docs.microsoft.com/azure/aks/update-credentials#update-aks-cluster-with-new-service-principal-credentials).

## <a name="can-i-movemigrate-my-cluster-between-subscriptions"></a>¿Puedo mover o migrar mi clúster entre suscripciones?

Actualmente no se admite el movimiento de clústeres entre suscripciones.

## <a name="can-i-move-my-aks-clusters-from-the-current-azure-subscription-to-another"></a>¿Puedo trasladar los clústeres de AKS de la suscripción actual de Azure a otra? 

No se admite el traslado de clústeres de AKS y de los recursos asociados entre suscripciones de Azure.

## <a name="why-is-my-cluster-delete-taking-so-long"></a>¿Por qué tarda tanto la eliminación del clúster? 

La mayoría de los clústeres se eliminan en el momento de la solicitud del usuario; en algunos casos, especialmente en aquellos en los que los clientes traen su propio grupo de recursos o se están realizando tareas entre grupos de recursos, la eliminación puede tardar más tiempo o producir un error. Si tiene un problema con las eliminaciones, compruebe que no tiene bloqueos en el grupo de recursos, que los recursos situados fuera del grupo están desasociados de este, etc.

## <a name="if-i-have-pod--deployments-in-state-nodelost-or-unknown-can-i-still-upgrade-my-cluster"></a>Si tengo un pod o implementaciones con el estado "NodeLost" (Nodo perdido) o "Unknown" (Desconocido), ¿puedo actualizar el clúster?

Se puede, pero AKS no lo recomienda. Lo ideal es realizar las actualizaciones si el estado del clúster es conocido o correcto.

## <a name="if-i-have-a-cluster-with-one-or-more-nodes-in-an-unhealthy-state-or-shut-down-can-i-perform-an-upgrade"></a>Si tengo un clúster con uno o varios nodos en un estado incorrecto o apagado, ¿puedo realizar una actualización?

No, elimine o quite los nodos con un estado erróneo antes de actualizar.

## <a name="i-ran-a-cluster-delete-but-see-the-error-errno-11001-getaddrinfo-failed"></a>Ejecuté una eliminación de clúster, pero me aparece el error `[Errno 11001] getaddrinfo failed` 

Normalmente, esto se debe a usuarios que tienen uno o varios grupos de seguridad de red (NSG) todavía en uso y asociados al clúster.  Quítelos e intente de nuevo la eliminación.

## <a name="i-ran-an-upgrade-but-now-my-pods-are-in-crash-loops-and-readiness-probes-fail"></a>Ejecuté una actualización, pero ahora mis pods están en bucles de bloqueo y se producen errores en los sondeos de preparación.

Confirme que la entidad de servicio no ha expirado.  Consulte: [Entidad de servicio de AKS](https://docs.microsoft.com/azure/aks/kubernetes-service-principal) y [Credenciales de actualización de AKS](https://docs.microsoft.com/azure/aks/update-credentials).

## <a name="my-cluster-was-working-but-suddenly-can-not-provision-loadbalancers-mount-pvcs-etc"></a>Mi clúster estaba funcionando, pero, de repente, no puede aprovisionar LoadBalancers, montar PVC, etc. 

Confirme que la entidad de servicio no ha expirado.  Consulte: [Entidad de servicio de AKS](https://docs.microsoft.com/azure/aks/kubernetes-service-principal) y [Credenciales de actualización de AKS](https://docs.microsoft.com/azure/aks/update-credentials).

## <a name="can-i-use-the-virtual-machine-scale-set-apis-to-scale-manually"></a>¿Puedo usar las API del conjunto de escalado de máquinas virtuales para escalar manualmente?

No, no se admiten las operaciones de escalado mediante el uso de las API del conjunto de escalado de máquinas virtuales. Use las API de AKS (`az aks scale`).

## <a name="can-i-use-virtual-machine-scale-sets-to-manually-scale-to-0-nodes"></a>¿Puedo usar conjuntos de escalado de máquinas virtuales para escalar manualmente a 0 nodos?

No, no se admiten las operaciones de escalado mediante el uso de las API del conjunto de escalado de máquinas virtuales.

## <a name="can-i-stop-or-de-allocate-all-my-vms"></a>¿Puedo detener o cancelar la asignación de todas las máquinas virtuales?

Aunque AKS tiene mecanismos de resistencia para admitir este tipo de configuración y recuperarse de él, no es una configuración recomendada.

## <a name="can-i-use-custom-vm-extensions"></a>¿Puedo usar extensiones de máquina virtual personalizadas?

No, AKS es un servicio administrado y no se admite la manipulación de los recursos de IaaS. Para instalar componentes personalizados, etc. Aproveche las API y los mecanismos de Kubernetes. Por ejemplo, aproveche DaemonSets para instalar los componentes necesarios.

<!-- LINKS - internal -->

[aks-upgrade]: ./upgrade-cluster.md
[aks-cluster-autoscale]: ./autoscaler.md
[aks-advanced-networking]: ./configure-azure-cni.md
[aks-rbac-aad]: ./azure-ad-integration.md
[node-updates-kured]: node-updates-kured.md
[aks-preview-cli]: /cli/azure/ext/aks-preview/aks
[az-aks-create]: /cli/azure/aks#az-aks-create
[aks-rm-template]: /azure/templates/microsoft.containerservice/2019-06-01/managedclusters
[aks-cluster-autoscaler]: cluster-autoscaler.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[aks-windows-cli]: windows-container-cli.md
[aks-windows-limitations]: windows-node-limitations.md
[reservation-discounts]:../cost-management-billing/reservations/save-compute-costs-reservations.md
[api-server-authorized-ip-ranges]: ./api-server-authorized-ip-ranges.md
[multi-node-pools]: ./use-multiple-node-pools.md
[availability-zones]: ./availability-zones.md

<!-- LINKS - external -->
[aks-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[auto-scaler]: https://github.com/kubernetes/autoscaler
[cordon-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
[keyvault-flexvolume]: https://github.com/Azure/kubernetes-keyvault-flexvol
[private-clusters-github-issue]: https://github.com/Azure/AKS/issues/948
