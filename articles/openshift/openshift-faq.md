---
title: Preguntas frecuentes sobre Red Hat OpenShift en Azure
description: Aquí tiene respuestas a las preguntas habituales sobre Red Hat OpenShift en Azure.
author: jimzim
ms.author: jzim
ms.service: container-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: f468cb294d79c44f92ef95437c0d88639a78b9a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77619503"
---
# <a name="azure-red-hat-openshift-faq"></a>Preguntas más frecuentes de Red Hat OpenShift en Azure

En este artículo se tratan las preguntas más frecuentes (P+F) sobre Red Hat OpenShift en Microsoft Azure.

## <a name="which-azure-regions-are-supported"></a>¿Qué regiones de Azure se admiten?

Vea [Recursos admitidos](supported-resources.md#azure-regions) para obtener una lista de las regiones globales que son compatibles con Red Hat OpenShift en Azure.

## <a name="can-i-deploy-a-cluster-into-an-existing-virtual-network"></a>¿Puedo implementar un clúster en una red virtual existente?

No. Pero puede conectar un clúster de Red Hat OpenShift en Azure en una red virtual existente mediante emparejamiento. Vea [Conexión de la red virtual del clúster a una red virtual existente](tutorial-create-cluster.md#optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network) para obtener más información.

## <a name="what-cluster-operations-are-available"></a>¿Qué operaciones de clúster están disponibles?

Solo puede escalar o reducir verticalmente el número de nodos de proceso. No se permite ninguna otra modificación al recurso `Microsoft.ContainerService/openShiftManagedClusters` después de la creación. El número máximo de nodos de proceso está limitado a 20.

## <a name="what-virtual-machine-sizes-can-i-use"></a>¿Qué tamaños de máquina virtual puedo usar?

Consulte los [tamaños de máquina virtual de Red Hat OpenShift en Azure](supported-resources.md#virtual-machine-sizes) para obtener una lista de los tamaños de máquina virtual que puede usar con un clúster de Red Hat OpenShift en Azure.

## <a name="is-data-on-my-cluster-encrypted"></a>¿Están cifrados los datos de mi clúster?

De forma predeterminada, hay cifrado en reposo. La plataforma de Azure Storage cifra automáticamente los datos antes de almacenarlos y los descifra antes de recuperarlos. Consulte [Cifrado del servicio Azure Storage para datos en reposo](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) para obtener más detalles.

## <a name="can-i-use-prometheusgrafana-to-monitor-my-applications"></a>¿Puedo usar Prometheus o Grafana para supervisar mis aplicaciones?

Sí, puede implementar Prometheus en su espacio de nombres y supervisar las aplicaciones en él.

## <a name="can-i-use-prometheusgrafana-to-monitor-metrics-related-to-cluster-health-and-capacity"></a>¿Puedo usar Prometheus o Grafana para supervisar las métricas relacionadas con la capacidad y el mantenimiento del clúster?

No, actualmente no.

## <a name="is-the-docker-registry-available-externally-so-i-can-use-tools-such-as-jenkins"></a>¿Está disponible externamente el registro de Docker para poder usar herramientas como Jenkins?

El registro de Docker está disponible desde `https://docker-registry.apps.<clustername>.<region>.azmosa.io/`, pero no se proporciona una gran garantía de durabilidad de almacenamiento. También puede usar [Azure Container Registry](https://azure.microsoft.com/services/container-registry/).

## <a name="is-cross-namespace-networking-supported"></a>¿Se admiten las redes entre espacios de nombres?

Los administradores de proyectos de cliente e individuales pueden personalizar las redes entre espacios de nombres (incluido denegarlas) por proyecto usando objetos `NetworkPolicy`.

## <a name="can-an-admin-manage-users-and-quotas"></a>¿Puede un administrador administrar usuarios y cuotas?

Sí. Un administrador de Red Hat OpenShift en Azure puede administrar usuarios y cuotas, además de acceder a todos los proyectos creados por los usuarios.

## <a name="can-i-restrict-a-cluster-to-only-certain-azure-ad-users"></a>¿Puedo restringir un clúster a solo determinados usuarios de Azure AD?

Sí. Puede restringir qué usuarios de Azure AD pueden iniciar sesión en un clúster configurando la aplicación de Azure AD. Para más información, consulte [Procedimientos para: Restricción de la aplicación a un conjunto de usuarios](https://docs.microsoft.com/azure/active-directory/develop/howto-restrict-your-app-to-a-set-of-users).

## <a name="can-i-restrict-users-from-creating-projects"></a>¿Puedo impedir que los usuarios creen proyectos?

Sí. Inicie sesión en el clúster como administrador de Red Hat OpenShift en Azure y ejecute este comando:

```
oc adm policy \
    remove-cluster-role-from-group self-provisioner \
    system:authenticated:oauth
```

Para más información, consulte la documentación de OpenShift en [deshabilitar el aprovisionamiento automático](https://docs.openshift.com/container-platform/3.11/admin_guide/managing_projects.html#disabling-self-provisioning).

## <a name="can-a-cluster-have-compute-nodes-across-multiple-azure-regions"></a>¿Un clúster puede tener nodos de proceso en varias regiones de Azure?

No. Todos los nodos de un clúster de Red Hat OpenShift en Azure deben partir de la misma región de Azure.

## <a name="are-master-and-infrastructure-nodes-abstracted-away-as-they-are-with-azure-kubernetes-service-aks"></a>¿Los nodos de infraestructura y maestros se abstraen como lo hacen con Azure Kubernetes Service (AKS)?

No. Todos los recursos, incluido el clúster maestro, se ejecutan en su suscripción de cliente. Estos tipos de recursos se colocan en un grupo de recursos de solo lectura.

## <a name="is-open-service-broker-for-azure-osba-supported"></a>¿Es compatible Open Service Broker para Azure (OSBA)?

Sí. Puede usar OSBA con Red Hat OpenShift en Azure. Vea [Open Service Broker para Azure](https://github.com/Azure/open-service-broker-azure#openshift-project-template) para obtener más información.

## <a name="i-am-trying-to-peer-into-a-virtual-network-in-a-different-subscription-but-getting-failed-to-get-vnet-cidr-error"></a>Estoy intentando mirar una red virtual en otra suscripción pero recibo el error `Failed to get vnet CIDR`.

En la suscripción que tiene la red virtual, asegúrese de registrar el proveedor `Microsoft.ContainerService` con `az provider register -n Microsoft.ContainerService --wait`. 

## <a name="what-is-the-azure-red-hat-openshift-aro-maintenance-process"></a>¿Cuál es el proceso de mantenimiento de Red Hat OpenShift en Azure (ARO)?

Hay tres tipos de mantenimiento para ARO: actualizaciones, copia de seguridad y restauración de datos de etcd y mantenimiento Iniciado por el proveedor de nube.

+ Las actualizaciones incluyen actualizaciones de software y CVE. La corrección de CVE se produce al inicio mediante la ejecución de `yum update` y proporciona una mitigación inmediata.  En paralelo, se crea una nueva compilación de las imágenes para la futura creación de clústeres.

+ La copia de seguridad y administración de los datos de etcd es un proceso automatizado que puede requerir tiempo de inactividad del clúster en función de la acción. Si la base de datos de etcd se va a restaurar a partir de una copia de seguridad, habrá tiempo de inactividad. La copia de seguridad se realizará cada hora y se conservarán las últimas 6 horas de copias de seguridad.

+ El mantenimiento iniciado por el proveedor de nube incluye la red, el almacenamiento y las interrupciones regionales. El mantenimiento depende del proveedor de nube y se basa en las actualizaciones proporcionadas por el proveedor.

## <a name="what-is-the-general-upgrade-process"></a>¿Cuál es el proceso de actualización general?

La ejecución de una actualización debe ser un proceso seguro y no debe interrumpir los servicios de clúster. El SRE puede desencadenar el proceso de actualización cuando hay nuevas versiones disponibles o los CVE están pendientes.

Las actualizaciones disponibles se prueban en un entorno provisional y, luego, se aplican a los clústeres de producción. Cuando se aplican, se agrega un nuevo nodo temporalmente y los nodos se actualizan de forma rotativa para que los pods mantengan los recuentos de réplica. Los procedimientos recomendados siguientes ayudan a garantizar un tiempo de inactividad mínimo.

Según la gravedad de la actualización pendiente, el proceso puede ser diferente; por ejemplo, las actualizaciones se podrían aplicar rápidamente para mitigar la exposición del servicio a un CVE. Una nueva imagen se creará de forma asincrónica, se probará y se implementará como una actualización del clúster. Aparte de eso, no hay ninguna diferencia entre emergencia y mantenimiento planeado. El mantenimiento planeado no está programado de antemano con el cliente.

Las notificaciones se pueden enviar por medio del ICM y del correo electrónico si se requiere la comunicación con el cliente.

## <a name="what-about-emergency-vs-planned-maintenance-windows"></a>¿Qué ocurre con las ventanas de mantenimiento planeado y emergencia?

No se distingue entre los dos tipos de mantenimiento. Nuestros equipos están disponibles de manera ininterrumpida y no usan las ventanas de mantenimiento "fuera de horas" programadas tradicionales.

## <a name="how-will-host-operating-system-and-openshift-software-be-updated"></a>¿Cómo se actualizará el sistema operativo y el software OpenShift?

El sistema operativo host y el software OpenShift se actualizan a través de nuestro proceso general de compilación de imágenes y actualización.

## <a name="whats-the-process-to-reboot-the-updated-node"></a>¿Cuál es el proceso de reinicio del nodo actualizado?

Este proceso debe administrarse como parte de una actualización.

## <a name="is-data-stored-in-etcd-encrypted-on-aro"></a>¿Los datos almacenados en etcd están cifrados en ARO?

No se cifran en el nivel de etcd. Actualmente no se admite la opción para activarla. OpenShift es compatible con esta característica, pero se requieren labores de ingeniería para crearla en la hoja de ruta. Los datos se cifran en el nivel de disco. Consulte [Encrypting Data at Datastore Layer](https://docs.openshift.com/container-platform/3.11/admin_guide/encrypting_data.html) (Cifrado de datos en la capa de almacén de datos) para más información.

## <a name="can-logs-of-underlying-vms-be-streamed-out-to-a-customer-log-analysis-system"></a>¿Se pueden transmitir los registros de máquinas virtuales subyacentes a un sistema de análisis de registros de cliente?

Syslog, los registros de Docker, journal y dmesg se administran mediante el servicio administrado y no se exponen a los clientes.

## <a name="how-can-a-customer-get-access-to-metrics-like-cpumemory-at-the-node-level-to-take-action-to-scale-debug-issues-etc-i-cannot-seem-to-run-kubectl-top-on-an-aro-cluster"></a>¿Cómo puede un cliente obtener acceso a métricas como CPU y memoria en el nivel de nodo para tomar medidas para escalar, depurar problemas, etc.? No puedo ejecutar `kubectl top` en un clúster de ARO.

Los clientes pueden acceder a las métricas de CPU o memoria en el nivel de nodo mediante el comando `oc adm top nodes` o `kubectl top nodes` con el rol de clúster customer-admin.  Los clientes también pueden acceder a las métricas de CPU o memoria de `pods` con el comando `oc adm top pods` o `kubectl top pods`.

## <a name="what-is-the-default-pod-scheduler-configuration-for-aro"></a>¿Cuál es la configuración predeterminada del programador de pods para ARO?

ARO usa el programador predeterminado que se incluye en OpenShift. Hay un par de mecanismos adicionales que no se admiten en ARO. Consulte la [documentación del programador predeterminado](https://docs.openshift.com/container-platform/3.11/admin_guide/scheduling/scheduler.html#generic-scheduler) y la [documentación del programador principal](https://github.com/openshift/openshift-azure/blob/master/pkg/startup/v6/data/master/etc/origin/master/scheduler.json) para más información.

La programación avanzada y personalizada no se admite actualmente. Consulte la [documentación de programación](https://docs.openshift.com/container-platform/3.11/admin_guide/scheduling/index.html) para más información.

## <a name="if-we-scale-up-the-deployment-how-do-azure-fault-domains-map-into-pod-placement-to-ensure-all-pods-for-a-service-do-not-get-knocked-out-by-a-failure-in-a-single-fault-domain"></a>Si escalamos verticalmente la implementación, ¿cómo se asignan los dominios de error de Azure en la colocación de los pods para tener la seguridad de que todos los pods de un servicio no se han visto afectados por un error en un único dominio de error?

De forma predeterminada, hay cinco dominios de error cuando se usan conjuntos de escalado de máquinas virtuales en Azure. Cada instancia de máquina virtual de un conjunto de escalado se colocará en uno de estos dominios de error. Esto garantiza que las aplicaciones implementadas en los nodos de proceso de un clúster se colocarán en distintos dominios de error.

Para más información, consulte [Elección del número correcto de dominios de error para el conjunto de escalado de máquinas virtuales](https://docs.microsoft.com//azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-fault-domains).

## <a name="is-there-a-way-to-manage-pod-placement"></a>¿Hay alguna manera de administrar la colocación de los pods?

Los clientes tienen la posibilidad de obtener nodos y ver etiquetas con el rol customer-admin.  De esta forma, podrán dirigirse a cualquier máquina virtual del conjunto de escalado.

Es necesario tener cuidado al usar etiquetas específicas:

- No se debe usar el nombre de host. El nombre de host se rota con frecuencia con las actualizaciones y es seguro que cambiará.

- Si el cliente tiene una solicitud de etiquetas específicas o una estrategia de implementación, se podría hacer, pero harían falta labores de ingeniería, y esto no se admite actualmente.

## <a name="what-is-the-maximum-number-of-pods-in-an-aro-cluster-what-is-the-maximum-number-of-pods-per-node-in-aro"></a>¿Cuál es el número máximo de pods en un clúster de ARO?  ¿Cuál es el número máximo de pods por nodo en ARO?

 Azure Red Hat OpenShift 3.11 tiene un límite de 50 pods por nodo y [ARO tiene un límite de 20 nodos de proceso](https://docs.microsoft.com/azure/openshift/openshift-faq#what-cluster-operations-are-available), por lo que el número máximo de pods admitidos en un clúster de ARO es de 50*20 = 1000.

## <a name="can-we-specify-ip-ranges-for-deployment-on-the-private-vnet-avoiding-clashes-with-other-corporate-vnets-once-peered"></a>¿Se pueden especificar intervalos IP para la implementación en la red virtual privada y evitar los conflictos con otras redes virtuales corporativas una vez emparejadas?

Red Hat OpenShift de Azure admite el emparejamiento de VNET y permite al cliente proporcionar una red virtual con la que emparejar y un CIDR de red virtual en el que funcionará la red OpenShift.

La red virtual creada por ARO se protegerá y no aceptará cambios de configuración. La red virtual que está emparejada está controlada por el cliente y reside en su suscripción.

## <a name="does-the-cluster-reside-in-a-customer-subscription"></a>¿El clúster se encuentra en una suscripción de cliente? 

La aplicación administrada de Azure reside en un grupo de recursos bloqueado con la suscripción del cliente. El cliente puede ver los objetos de ese grupo de recursos pero no modificarlos.

## <a name="is-the-sdn-module-configurable"></a>¿Se puede configurar el módulo de SDN?

SDN es openshift-ovs-networkpolicy y no se puede configurar.

## <a name="which-unix-rights-in-iaas-are-available-for-mastersinfraapp-nodes"></a>¿Qué derechos de UNIX (en IaaS) están disponibles para los nodos maestros, infraestructura o aplicaciones?

No es aplicable a esta oferta. El acceso al nodo está prohibido.

## <a name="which-ocp-rights-do-we-have-cluster-admin-project-admin"></a>¿Qué derechos de OCP tenemos? ¿Administrador del clúster? ¿Administrador de proyectos?

Para más información, consulte la [introducción a la administración de clústeres](https://docs.openshift.com/aro/admin_guide/index.html) de Red Hat OpenShift en Azure.

## <a name="which-kind-of-federation-with-ldap"></a>¿Qué tipo de federación hay con LDAP?

Esto se lograría con la integración de Azure AD. 

## <a name="is-there-any-element-in-aro-shared-with-other-customers-or-is-everything-independent"></a>¿Hay algún elemento en ARO compartido con otros clientes? ¿O todo es independiente?

Cada clúster de Red Hat OpenShift en Azure está dedicado a un cliente determinado y vive en la suscripción del cliente. 

## <a name="can-we-choose-any-persistent-storage-solution-like-ocs"></a>¿Podemos elegir cualquier solución de almacenamiento persistente, como OCS? 

Hay dos clases de almacenamiento disponibles para seleccionar: Azure Disk y Azure Files.

## <a name="how-is-a-cluster-updated-including-majors-and-minors-due-to-vulnerabilities"></a>¿Cómo se actualiza un clúster (incluidas las principales y secundarias debido a vulnerabilidades)?

Consulte [¿Cuál es el proceso de actualización general?](https://docs.microsoft.com/azure/openshift/openshift-faq#what-is-the-general-upgrade-process)

## <a name="what-azure-load-balancer-is-used-by-aro-is-it-standard-or-basic-and-is-it-configurable"></a>¿Qué equilibrador de carga de Azure usa ARO?  ¿Es Estándar o Básico y es configurable?

ARO usa Azure Load Balancer Estándar y no es configurable.

## <a name="can-aro-use-netapp-based-storage"></a>¿Puede ARO usar el almacenamiento basado en NetApp?

En este momento, las únicas opciones de almacenamiento admitidas son las clases Azure Disk y Azure Files. 


