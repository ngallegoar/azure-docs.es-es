---
title: Preguntas más frecuentes
description: En este artículo se ofrecen respuestas a algunas de las preguntas más frecuentes sobre Azure VMware Solution.
ms.topic: conceptual
ms.date: 11/19/2020
ms.author: dikamath
ms.openlocfilehash: 0bc3e0ab5b3017c2d051113a57d79214ffce0836
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94967353"
---
# <a name="frequently-asked-questions-about-azure-vmware-solution"></a>Preguntas frecuentes sobre Azure VMware Solution

En este artículo se responden las preguntas más frecuentes sobre Azure VMware Solution.

## <a name="general"></a>General

#### <a name="what-is-azure-vmware-solution"></a>¿Qué es Azure VMware Solution?

A medida que las empresas buscan estrategias de modernización de la TI para mejorar la agilidad empresarial, reducir costos y acelerar la innovación, las plataformas de nube híbrida han surgido como los grandes facilitadores de la transformación digital de los clientes. Azure VMware Solution combina el Centro de datos definido por software (SDDC) de VMware con el ecosistema global de servicios en la nube de Microsoft Azure. Azure VMware Solution se administra para cumplir los requisitos de rendimiento, disponibilidad, seguridad y cumplimiento.

## <a name="azure-vmware-solution-service"></a>Servicio de Azure VMware Solution

#### <a name="where-is-azure-vmware-solution-available-today"></a>¿Dónde está disponible actualmente Azure VMware Solution?

El servicio se agrega continuamente a regiones nuevas, por lo que debe consultar la [información de disponibilidad de servicio más reciente](https://azure.microsoft.com/global-infrastructure/services/?products=azure-vmware) para más detalles. 

#### <a name="can-workloads-running-in-an-azure-vmware-solution-instance-consume-or-integrate-with-azure-services"></a>¿Pueden las cargas de trabajo que se ejecutan en una instancia de Azure VMware Solution consumir o integrarse en los servicios de Azure?

Todos los servicios de Azure estarán disponibles para los clientes de Azure VMware Solution. Las limitaciones de rendimiento y disponibilidad de los servicios específicos deberán abordarse para cada caso.

#### <a name="do-i-use-the-same-tools-that-i-use-now-to-manage-private-cloud-resources"></a>¿Utilizo las mismas herramientas que uso ahora para administrar recursos de nube privada?

Sí. Azure Portal se utiliza para la implementación y para varias operaciones de administración. vCenter y NSX Manager se utilizan para administrar los recursos de vSphere y NSX-T.

#### <a name="can-i-manage-a-private-cloud-with-my-on-premises-vcenter"></a>¿Puedo administrar una nube privada con mi instancia de vCenter local?

En un principio, Azure VMware Solution no admitirá una única experiencia de administración en entornos locales y de nube privada. Los clústeres de nube privada se administrarán con vCenter y NSX Manager de local a una nube privada.

#### <a name="can-i-use-vrealize-suite-running-on-premises"></a>¿Puedo usar vRealize Suite que se ejecute en el entorno local? 

Las integraciones y los casos de uso específicos se pueden evaluar en cada caso.

#### <a name="can-i-migrate-vsphere-vms-from-on-premises-environments-to-azure-vmware-solution-private-clouds"></a>¿Puedo migrar VM de vSphere de entornos locales a nubes privadas de Azure VMware Solution?

Sí. La migración de VM y vMotion se pueden usar para mover VM a una nube privada si se cumplen los [requisitos de vMotion](https://kb.vmware.com/s/article/2106952?lang=en_US&queryTerm=2106952) estándar de vCenter.

#### <a name="is-a-specific-version-of-vsphere-required-in-on-premises-environments"></a>¿Se requiere una versión específica de vSphere en entornos locales?

Todos los entornos de nube cuentan con VMware HCX, vSphere 5.5 o posterior en entornos locales para vMotion.

#### <a name="what-does-the-change-control-process-look-like"></a>¿Qué aspecto tiene el proceso de control de cambios?

Las actualizaciones realizadas en el propio servicio sigue el proceso estándar de administración de cambios de Microsoft Azure. Los clientes son responsables de las tareas de administración de las cargas de trabajo y de los procesos asociados de administración de cambios.

#### <a name="how-is-this-different-from-azure-vmware-solution-by-cloudsimple"></a>¿En qué se diferencia de Azure VMware Solution by CloudSimple?

Con la nueva solución Azure VMware Solution, Microsoft y VMware tienen una asociación directa con el proveedor de la nube. La solución nueva está diseñada, creada y respaldada completamente por Microsoft, y aprobada por VMware. Desde el punto de vista de la arquitectura, las soluciones son coherentes con la pila de tecnología de VMware que se ejecuta en una infraestructura de Azure dedicada.



#### <a name="can-azure-vmware-solution-vms-be-managed-by-vmrc"></a>¿VMRC puede administrar las VM de Azure VMware Solution?
Sí, siempre que el sistema en el que está instalado pueda acceder al servidor vCenter de la nube privada y use un servidor DNS público para poder resolver los nombres de host ESXi.

#### <a name="are-there-special-instructions-for-installing-and-using-vmrc-with-azure-vmware-solution-vms"></a>¿Existen instrucciones especiales para instalar y usar VMRC con las VM de Azure VMware Solution?
No. Siga las [instrucciones proporcionadas por VMware](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-89E7E8F0-DB2B-437F-8F70-BA34C505053F.html) para cumplir los requisitos previos de VM especificados en dichas instrucciones. 

#### <a name="is-vmware-hcx-supported-on-vpns"></a>¿Se admite VMware HCX en las VPN?
No, debido a los requisitos de ancho de banda y latencia.

#### <a name="can-azure-bastion-be-used-for-connecting-to-azure-vmware-solution-vms"></a>¿Se puede usar Azure Bastion para conectarse a las VM de Azure VMware Solution?
Azure Bastion es el servicio recomendado para conectarse al jumpbox a fin de evitar exponer Azure VMware Solution a Internet. No se puede usar Azure Bastion para conectarse a VM de Azure VMware Solution porque no son objetos de IaaS de Azure.

#### <a name="can-azure-load-balancer-internal-be-used-for-azure-vmware-solution-vms"></a>¿Puede Azure Load Balancer usarse internamente para las VM de Azure VMware Solution?
No. Azure Load Balancer interno solo admite las VM de IaaS de Azure. Azure Load Balancer no admite los grupos de back-end basados en IP; solo las VM de Azure o los objetos de conjuntos de escalado de máquinas virtuales en los que las VM de Azure VMware Solution no sean objetos de Azure.

#### <a name="can-an-existing-expressroute-gateway-be-used-to-connect-to-azure-vmware-solution"></a>¿Se puede usar una puerta de enlace de ExpressRoute existente para la conexión a Azure VMware Solution?
Puede usar una puerta de enlace de ExpressRoute existente para conectarse a Azure VMware Solution, siempre que no supere el límite de cuatro circuitos ExpressRoute por red virtual.  Sin embargo, para tener acceso a Azure VMware Solution desde el entorno local mediante ExpressRoute, debe tener Global Reach de ExpressRoute, ya que la puerta de enlace de ExpressRoute no proporciona enrutamiento transitivo entre sus circuitos conectados.

## <a name="compute-network-storage-and-backup"></a>Proceso, red, almacenamiento y copia de seguridad

#### <a name="is-there-more-than-one-type-of-host-available"></a>¿Hay más de un tipo de host disponible?

Hay solo un tipo de host disponible.

#### <a name="what-are-the-cpu-specifications-in-each-type-of-host"></a>¿Cuáles son las especificaciones de CPU en cada tipo de host?

Los servidores tienen dos CPU Intel de 18 núcleos de 2,3 GHz.

#### <a name="how-much-memory-is-in-each-host"></a>¿Cuánta memoria hay en cada host?

Los servidores tienen 576 GB de RAM.

#### <a name="what-is-the-storage-capacity-of-each-host"></a>¿Cuál es la capacidad de almacenamiento de cada host?

Cada host ESXi tiene dos grupos de discos vSAN con un nivel de capacidad de 15,2 TB y un nivel de caché NVMe de 3,2 TB (1,6 TB en cada grupo de discos).

#### <a name="how-much-network-bandwidth-is-available-in-each-esxi-host"></a>¿Cuánto ancho de banda de red hay disponible en cada host ESXi?

Cada host ESXi de Azure VMware Solution está configurado con cuatro adaptadores de red de 25 Gbps, dos adaptadores de red aprovisionados para el tráfico del sistema ESXi y otros dos aprovisionados para el tráfico de carga de trabajo. 

#### <a name="is-data-stored-on-the-vsan-datastores-encrypted-at-rest"></a>¿Se cifran en reposo los datos almacenados en los almacenes de datos vSAN?

Sí, todos los datos vSAN se cifran de forma predeterminada mediante claves que se almacenan en Azure Key Vault.

#### <a name="you-document-that-commvault-veritas-and-veeam-have-extended-their-backup-solutions-to-work-with-azure-vmware-solution-what-about-other-independent-software-vendors-isvs-backup-solutions"></a>Puede documentar que Commvault, Veritas y Veeam han ampliado sus soluciones de copia de seguridad para trabajar con Azure VMware Solution. ¿Qué ocurre con otras soluciones de copia de seguridad de fabricantes de software independientes (ISV)?

A nuestro entender, cualquier solución de copia de seguridad que use VMware VADP con el modo de transporte HotAdd debería funcionar desde el principio en Azure VMware Solution.

#### <a name="what-about-support-for-isv-backup-solutions"></a>¿Qué ocurre con la compatibilidad de las soluciones de copia de seguridad de ISV?

A medida que los clientes instalan y administran estas soluciones de copia de seguridad, pueden ponerse en contacto con el fabricante de software independiente correspondiente para obtener ayuda. 

#### <a name="what-is-the-correct-storage-policy-for-the-dedupe-setup"></a>¿Cuál es la directiva de almacenamiento correcta para la configuración de la desduplicación?

Use la directiva de almacenamiento *thin_provision* para la plantilla de VM.  El valor predeterminado es *thick_provision*.

#### <a name="are-the-snmp-infrastructure-logs-shared"></a>¿Los registros de infraestructura SNMP están compartidos?

No.

## <a name="hosts-clusters-and-private-clouds"></a>Hosts, clústeres y nubes privadas

#### <a name="is-the-underlying-infrastructure-shared"></a>¿Se comparte la infraestructura subyacente?

No, los hosts y clústeres de nube privada son dedicados y se borran de forma segura antes y después del uso.

#### <a name="what-are-the-minimum-and-the-maximum-number-of-hosts-per-cluster"></a>¿Cuál es el número mínimo y máximo de hosts por clúster?

Los clústeres se pueden escalar entre 3 y 16 hosts ESXi. Los clústeres de prueba están limitados a tres hosts.

#### <a name="can-i-scale-my-private-cloud-clusters"></a>¿Puedo escalar mis clústeres de nube privada?

Sí, los clústeres se escalan entre el número mínimo y máximo de hosts ESXi. Los clústeres de prueba están limitados a tres hosts.

#### <a name="what-are-trial-clusters"></a>¿Qué son los clústeres de prueba?

Los clústeres de prueba son tres clústeres de hosts que se usan para las evaluaciones de un mes de las nubes privadas de Azure VMware Solution.

#### <a name="can-i-use-high-end-hosts-for-trial-clusters"></a>¿Puedo usar hosts de alto nivel para los clústeres de prueba?

No. Los hosts ESXi de alto nivel están reservados para su uso en clústeres de producción.

## <a name="azure-vmware-solution-and-vmware-software"></a>Azure VMware Solution y software de VMware

#### <a name="what-versions-of-vmware-software-is-used-in-private-clouds"></a>¿Qué versiones del software de VMware se usan en las nubes privadas?

Las nubes privadas usan vSphere 6.7 U3, vSAN 6.7 U3, VMware HCX y NSX-T 2.5.  Para obtener más información, consulte los [requisitos de la versión de software de VMware](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-54E5293B-8707-4D29-BFE8-EE63539CC49B.html).

#### <a name="do-private-clouds-use-vmware-nsx"></a>¿Usan las nubes privadas VMware NSX?

Sí, NSX-T 2.5 se usa para las redes definidas por software en las nubes privadas de Azure VMware Solution.

#### <a name="can-i-use-vmware-nsx-v-in-a-private-cloud"></a>¿Puedo usar VMware NSX-V en una nube privada?

No. La única versión admitida de NSX es NSX-T.

#### <a name="is-nsx-required-in-on-premises-environments-or-networks-that-connect-to-a-private-cloud"></a>¿Se requiere NSX en entornos locales o redes que se conectan a una nube privada?

No, no es necesario usar NSX en el entorno local.

#### <a name="what-is-the-upgrade-and-update-schedule-for-vmware-software-in-a-private-cloud"></a>¿Cuál es el cronograma de actualización del software de VMware en una nube privada?

Las actualizaciones del conjunto de productos de software de la nube privada mantienen el software dentro de una de las versiones más recientes del conjunto de productos de software de VMware. Las versiones de software de la nube privada pueden diferir de las versiones más recientes de los componentes de software individuales (ESXi, NSX-T, vCenter, vSAN).

#### <a name="how-often-will-the-private-cloud-software-stack-be-updated"></a>¿Con qué frecuencia se actualizará la pila de software de nube privada?

El software de la nube privada se actualiza según un cronograma que sigue la publicación del conjunto de productos de software de VMware. Su nube privada no requiere tiempo de inactividad para las actualizaciones.

## <a name="connectivity"></a>Conectividad

#### <a name="what-network-ip-address-planning-is-required-to-incorporate-private-clouds-with-on-premises-environments"></a>¿Qué planeación de direcciones IP de red se necesita para incorporar nubes privadas en entornos locales?

Se requiere un espacio de direcciones de red privada /22 para implementar una nube privada de Azure VMware Solution. Este espacio de direcciones privadas no debe superponerse con otras redes virtuales de una suscripción ni con redes locales.
 
#### <a name="how-do-i-connect-from-on-premises-environments-to-an-azure-vmware-solution-private-cloud"></a>¿Cómo me conecto desde entornos locales a una nube privada de Azure VMware Solution?

Puede conectarse al servicio mediante uno de dos métodos: 

- Con una VM o puerta de enlace de aplicaciones implementadas en una red virtual de Azure emparejada a través de ExpressRoute a la nube privada.
- A través de Global Reach de ExpressRoute desde el centro de datos local a un circuito de Azure ExpressRoute.

#### <a name="how-do-i-connect-a-workload-vm-to-the-internet-or-an-azure-service-endpoint"></a>¿Cómo conecto una VM de cargas de trabajo a Internet o a un punto de conexión de servicio de Azure?

En Azure Portal, habilite la conectividad a Internet para una nube privada. Con NSX-T Manager, cree un enrutador NSX-T T1 y un conmutador lógico. A continuación, use vCenter para implementar una VM en el segmento de red definido por el conmutador lógico. Esa VM tendrá acceso de red a Internet y a los servicios de Azure.

#### <a name="do-i-need-to-restrict-access-from-the-internet-to-vms-on-logical-networks-in-a-private-cloud"></a>¿Es necesario restringir el acceso desde Internet a las VM en redes lógicas en una nube privada?

No. No se permite el tráfico de red entrante desde Internet directamente a nubes privadas.

#### <a name="do-i-need-to-restrict-internet-access-from-vms-on-logical-networks-to-the-internet"></a>¿Es necesario restringir el acceso a Internet desde VM en redes lógicas?

Sí. Deberá usar NSX-T Manager para crear un firewall que restrinja el acceso de las VM a Internet.


#### <a name="can-azure-vmware-solution-use-azure-virtual-wan-hosted-expressroute-gateways"></a>¿Puede Azure VMware Solution usar puertas de enlace de ExpressRoute hospedadas por Azure Virtual WAN?
Sí.

#### <a name="can-transit-connectivity-be-established-between-on-premises-and-azure-vmware-solution-through-azure-virtual-wan-over-expressroute-global-reach"></a>¿Se puede establecer la conectividad de tránsito entre el en el entorno local y Azure VMware Solution mediante Azure Virtual WAN a través de Global Reach de ExpressRoute?
Azure Virtual WAN no proporciona enrutamiento transitivo entre dos circuitos ExpressRoute conectados y una puerta de enlace de ExpressRoute WAN no virtual. El uso de Global Reach de ExpressRoute permite la conectividad entre el en el entorno local y Azure VMware Solution, pero pasa a través de la red global de Microsoft en lugar del centro de Virtual WAN.


## <a name="accounts-and-privileges"></a>Cuentas y privilegios

#### <a name="what-accounts-and-privileges-will-i-get-with-my-new-azure-vmware-solution-private-cloud"></a>¿Qué cuentas y privilegios obtendré con mi nueva nube privada de Azure VMware Solution?

Recibirá credenciales para un usuario cloudadmin en vCenter y acceso de administrador en NSX-T Manager. También hay un grupo CloudAdmin que se puede usar para incorporar Azure Active Directory. Para obtener más información, consulte [Conceptos de acceso y de identidad](concepts-identity.md).

#### <a name="can-have-administrator-access-to-esxi-hosts"></a>¿Puedo tener acceso de administrador a los hosts de ESXi?

No, el acceso de administrador a ESXi está restringido para cumplir con los requisitos de seguridad de la solución.

#### <a name="what-privileges-and-permissions-will-i-have-in-vcenter"></a>¿Qué privilegios y permisos tendré en vCenter?

Tendrá los privilegios del grupo CloudAdmin. Para obtener más información, consulte [Conceptos de acceso y de identidad](concepts-identity.md).

#### <a name="what-privileges-and-permissions-will-i-have-on-the-nsx-t-manager"></a>¿Qué privilegios y permisos tendré en NSX-T Manager?

Tendrá privilegios completos de administrador en NSX-T y podrá administrar el control de acceso basado en roles de vSphere, tal como lo haría con NSX-T Data Center en el entorno local. Para obtener más información, consulte [Conceptos de acceso y de identidad](concepts-identity.md).

> [!NOTE]
> Se crea y configura un enrutador T0 como parte de una implementación de nube privada. Cualquier modificación en el enrutador lógico o en las VM del nodo perimetral de NSX-T podría afectar a la conectividad a la nube privada.

## <a name="billing-and-support"></a>Facturación y soporte técnico

#### <a name="how-will-pricing-be-structured-for-azure-vmware-solution"></a>¿Cómo se estructurará el precio de Azure VMware Solution?

Para ver las preguntas generales sobre los precios, consulte la página de [precios](https://azure.microsoft.com/pricing/details/azure-vmware) de Azure VMware Solution. 

#### <a name="can-azure-vmware-solution-be-purchased-through-a-microsoft-csp"></a>¿Se puede adquirir Azure VMware Solution a través de un CSP de Microsoft?

Sí, los clientes pueden implementar Azure VMware Solution en una suscripción de Azure administrada por un CSP.

#### <a name="who-supports-azure-vmware-solution"></a>¿Quién proporciona el soporte técnico de Azure VMware Solution?

Microsoft ofrece el soporte técnico para Azure VMware Solution. Puede enviar una [solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). 

En el caso de las suscripciones administradas por CSP, el proveedor de soluciones proporciona el primer nivel de soporte técnico, como el CSP lo hace para otros servicios de Azure.

#### <a name="what-accounts-do-i-need-to-create-an-azure-vmware-solution-private-cloud"></a>¿Qué cuentas necesito para crear una nube privada de Azure VMware Solution?

Necesitará una cuenta de Azure en una suscripción a Azure.

#### <a name="are-red-hat-solutions-supported-on-azure-vmware-solution"></a>¿Las soluciones de Red Hat se admiten en Azure VMware Solution?

Microsoft y Red Hat comparten un equipo de soporte técnico integrado y coubicado que proporciona un punto de contacto unificado para los ecosistemas de Red Hat que se ejecutan en la plataforma Azure.  Al igual que otros servicios de la plataforma Azure que funcionan con Red Hat Enterprise Linux, Azure VMware Solution está bajo la protección del soporte integrado y Cloud Access. Asimismo, se admite la ejecución de Red Hat Enterprise Linux sobre Azure VMware Solution en Azure.

#### <a name="is-vmware-hcx-enterprise-available-and-if-so-how-much-does-it-cost"></a>¿VMware HCX Enterprise está disponible? Si es así, ¿cuánto cuesta?

VMware HCX Enterprise está disponible con Azure VMware Solution, como una función o servicio en *versión preliminar*. Aunque VMware HCX Enterprise para Azure VMware Solution esté en su versión preliminar, es una función o servicio gratuito y está sujeto a los términos y condiciones de los servicios de versión preliminar. Una vez que el servicio VMware HCX Enterprise esté disponible de forma general, recibirá un aviso de 30 días de que la facturación cambiará. Puede desactivar o no participar en el servicio.

#### <a name="how-do-i-request-a-host-quota-increase-for-azure-vmware-solution"></a>¿Cómo se solicita un aumento de la cuota de host para Azure VMware Solution?

En el caso de las suscripciones administradas por CSP, el cliente debe enviar la solicitud al asociado. Luego el equipo de asociados se comunica con Microsoft para aumentar la cuota de la suscripción. Para obtener más información, consulte el artículo [Habilitación del recurso de Azure VMware Solution](enable-azure-vmware-solution.md). 

En el caso de las suscripciones de EA, siga el procedimiento a continuación. En primer lugar, necesitará:

* Un [Contrato Enterprise (EA) de Azure](../cost-management-billing/manage/ea-portal-agreements.md) con Microsoft.
* Una cuenta de Azure con una suscripción a Azure.

Antes de poder crear el recurso de Azure VMware Solution, deberá enviar una incidencia de soporte técnico para que se asignen los hosts. Se tarda hasta cinco días laborables en confirmar y cumplir la solicitud. Si tiene una nube privada de Azure VMware Solution y quiere asignar más hosts, pasará por el mismo proceso.

1. En Azure Portal, en **Ayuda y soporte técnico**, cree una **[solicitud de soporte técnico](https://rc.portal.azure.com/#create/Microsoft.Support)** y proporcione la siguiente información para el vale:
   - **Tipo de problema**: Requisitos previos técnicos
   - **Subscription** (Suscripción): Seleccione su suscripción.
   - **Servicio:** Todos los servicios > Azure VMware Solution
   - **Recurso:** Pregunta general 
   - **Resumen:** Capacidad necesitada
   - **Tipo de problema**: problemas de administración de la capacidad.
   - **Subtipo de problema:** solicitud de cliente de capacidad o cuota de host adicional.

1. En la **descripción** de la incidencia de soporte técnico, en la pestaña **Detalles**, proporcione la siguiente información:

   - POC o producción 
   - Nombre de región
   - Número de hosts
   - Cualquier otro detalle

   >[!NOTE]
   >Azure VMware Solution recomienda como mínimo tres hosts para poner en marcha la nube privada y N+1 hosts para la redundancia. 

1. Seleccione **Revisar + crear** para enviar la solicitud.

   Un representante de soporte técnico tardará hasta cinco días hábiles en confirmar su solicitud.

   >[!IMPORTANT] 
   >Si ya tiene una instancia de Azure VMware Solution y solicita hosts adicionales, tenga en cuenta que necesitamos cinco días laborables para asignarlos. 

1. Antes de aprovisionar los hosts, asegúrese de registrar el proveedor de recursos **Microsoft.AVS** en Azure Portal.  

   ```azurecli-interactive
   az provider register -n Microsoft.AVS --subscription <your subscription ID>
   ```

   Para ver otras formas de registrar el proveedor de recursos, consulte [Tipos y proveedores de recursos de Azure](../azure-resource-manager/management/resource-providers-and-types.md). 

#### <a name="are-reserved-instances-available-for-purchasing-through-the-cloud-solution-provider-csp-program"></a>¿Hay instancias reservadas disponibles para la compra a través del programa Proveedor de soluciones en la nube (CSP)?

Sí. El CSP puede comprar instancias reservadas para sus clientes. Para obtener más información, consulte el artículo [Ahorro de costos con una instancia reservada](reserved-instance.md). 

#### <a name="does-azure-vmware-solution-offer-multi-tenancy-for-hosting-csp-partners"></a>¿Ofrece Azure VMware Solution varios inquilinos para hospedar asociados de CSP?

No. Actualmente, Azure VMware Solution no ofrece varios inquilinos.

#### <a name="will-traffic-between-on-premises-and-azure-vmware-solution-over-expressroute-incur-any-outbound-data-transfer-charge-in-the-metered-data-plan"></a>¿Incurre el tráfico entre el entorno local y Azure VMware Solution a través de ExpressRoute en un cargo de transferencia de datos de salida en el plan de datos según uso?

El tráfico del circuito ExpressRoute de Azure VMware Solution no se mide de ningún modo. El tráfico del circuito ExpressRoute que conecta el entorno local a Azure se cobra en función de los planes de precios de ExpressRoute.


## <a name="customer-communication"></a>Comunicación al cliente

#### <a name="how-can-i-receive-an-alert-when-azure-sends-service-health-notifications-to-my-azure-subscription"></a>¿Cómo puedo recibir una alerta cuando Azure envía notificaciones de estado del servicio a mi suscripción de Azure?

Las notificaciones de problemas de servicio, mantenimiento planificado, avisos de estado y avisos de seguridad se publican a través de **Service Health** en Azure Portal.  Puede realizar acciones oportunas al configurar las alertas del registro de actividad para estas notificaciones. Para obtener más información, consulte [Creación de alertas de Service Health mediante Azure Portal](../service-health/alerts-activity-log-service-notifications-portal.md#create-service-health-alert-using-azure-portal).

:::image type="content" source="media/service-health.png" alt-text="Captura de pantalla de notificaciones de Service Health":::



<!-- LINKS - external -->
[kb2106952]: https://kb.vmware.com/s/article/2106952?lang=en_US&queryTerm=21069522

<!-- LINKS - internal -->
[Access and Identity Concepts]: concepts-identity.md
