---
title: Introducción a los clústeres independientes de Service Fabric
description: Los clústeres de Service Fabric se ejecutan en Windows Server y Linux, lo que significa que podrá implementar y hospedar aplicaciones de Service Fabric en cualquier lugar donde sea posible ejecutar Windows Server y Linux.
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: a3162a3d02510ce8efab6c5bcac0f1fdd2b2539b
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94684005"
---
# <a name="overview-of-service-fabric-standalone-clusters"></a>Introducción a los clústeres de Service Fabric independientes

Un clúster de Service Fabric es un conjunto de máquinas físicas o virtuales conectadas a la red, en las que se implementan y administran los microservicios. Una máquina física o virtual que forma parte de un clúster se denomina nodo del clúster. Los clústeres pueden escalarse a miles de nodos. Si agrega nuevos nodos al clúster, Service Fabric reequilibra las réplicas e instancias de la partición del servicio en el número aumentado de nodos. El rendimiento general de la aplicación mejora y se reduce la contención para el acceso a la memoria. Si los nodos del clúster no se usan de forma eficaz, puede reducir su número de nodos. Service Fabric vuelve a reequilibrar las réplicas e instancias de la partición en el número reducido de nodos para aprovechar mejor el hardware de cada nodo.

Un tipo de nodo define el tamaño, el número y las propiedades de un conjunto de nodos en el clúster. Cada tipo de nodo se puede escalar o reducir verticalmente de forma independiente. Cada uno tiene diferentes conjuntos de puertos abiertos y puede tener distintas métricas de capacidad. Los tipos de nodo se utilizan para definir los roles para un conjunto de nodos de clúster, por ejemplo, "front-end" o "back-end". El clúster puede tener más de un tipo de nodo, pero el tipo de nodo principal debe tener al menos cinco máquinas virtuales para los clústeres de producción (o al menos tres máquinas virtuales en clústeres de prueba). [Los servicios del sistema de Service Fabric](service-fabric-technical-overview.md#system-services) se colocan en los dos del tipo de nodo principal.

El proceso de creación de un clúster local de Service Fabric es similar al proceso de creación de un clúster en cualquier nube que elija donde cuente con un conjunto de máquinas virtuales. Los pasos iniciales para aprovisionar las máquinas virtuales se regirán por el proveedor de la nube o el entorno local que use. Cuando tenga un conjunto de máquinas virtuales con la conectividad de red habilitada entre ellas, los pasos para configurar el paquete de Service Fabric, editar la configuración del clúster y ejecutar la creación del clúster y los scripts de administración son idénticos. Esto garantiza que sus conocimientos y su experiencia con respecto al uso y la administración de los clústeres de Service Fabric se puedan aplicar si decide cambiar a un nuevo entorno de hospedaje.

## <a name="cluster-security"></a>Seguridad de clúster

Un clúster de Service Fabric es un recurso que usted posee.  Tiene la responsabilidad de proteger los clústeres para impedir que usuarios no autorizados se conecten a ellos. Proteger el clúster es especialmente importante si en él se ejecutan cargas de trabajo de producción.

> [!NOTE]
> La autenticación de Windows se basa en Kerberos. NTLM no se admite como tipo de autenticación.
>
> Siempre que sea posible, use la autenticación de certificados X.509 para los clústeres de Service Fabric.

### <a name="node-to-node-security"></a>Seguridad de nodo a nodo

La seguridad de nodo a nodo protege la comunicación entre las máquinas virtuales o los equipos de un clúster. Este escenario de seguridad garantiza que solo los equipos autorizados a unirse al clúster pueden participar en el hospedaje de aplicaciones y servicios en el clúster. Service Fabric usa certificados X.509 para proteger un clúster y proporcionar características de seguridad de las aplicaciones.  Se necesita un certificado de clúster para proteger el tráfico de clúster y proporcionar autenticación de servidor y clúster.  Los certificados autofirmados se pueden usar para los clústeres de prueba, pero para proteger los de producción se debe utilizar un certificado de una entidad de certificación de confianza.

También se puede habilitar la seguridad de Windows para un clúster independiente de Windows. Si tiene Windows Server 2012 R2 y Windows Active Directory, se recomienda que utilice la seguridad de Windows con las cuentas de servicio administradas de grupo. De lo contrario, use la seguridad de Windows con cuentas de Windows.

Para más información, lea [Seguridad de nodo a nodo](service-fabric-cluster-security.md#node-to-node-security)

### <a name="client-to-node-security"></a>Seguridad de cliente a nodo

La seguridad de cliente a nodo autentica los clientes y ayuda a proteger la comunicación entre un cliente y los nodos individuales del clúster. Este tipo de seguridad ayuda a garantizar que solo los usuarios autorizados accedan al clúster y a las aplicaciones implementadas en él. Los clientes se identifican de forma exclusiva mediante sus credenciales de seguridad del certificado X.509. Puede utilizarse cualquier número de certificados de cliente opcionales para autenticar a los clientes administradores o usuarios con el clúster.

Además de los certificados de cliente, Azure Active Directory también puede configurarse para autenticar a los clientes con el clúster.

Para más información, lea [Seguridad de cliente a nodo](service-fabric-cluster-security.md#client-to-node-security)

### <a name="service-fabric-role-based-access-control"></a>Control de acceso basado en rol de Service Fabric
Service Fabric también admite el control de acceso para limitar este a determinadas operaciones de clúster para los diferentes grupos de usuarios. Esto ayuda a que el clúster esté más protegido. Se admiten dos tipos de control de acceso para los clientes que se conectan a un clúster: Rol de administrador y rol de usuario.  

Para más información, vea [Control de acceso basado en rol de Service Fabric](service-fabric-cluster-security.md#service-fabric-role-based-access-control).

## <a name="scaling"></a>Ampliación

Las necesidades de las aplicaciones cambian a lo largo del tiempo. Puede necesitar aumentar los recursos del clúster para cumplir con aumentos del tráfico de red o de la carga de trabajo de la aplicación o reducir los recursos del clúster cuando la demanda baja. Después de crear un clúster de Service Fabric, puede escalar el clúster horizontalmente (cambiar el número de nodos) o verticalmente (cambiar los recursos de los nodos). Puede escalar el clúster en cualquier momento, incluso con cargas de trabajo en ejecución en el clúster. Según se escala el clúster, las aplicaciones se escalan automáticamente.

Para más información, lea [Escalado de clústeres independientes](service-fabric-cluster-scaling-standalone.md).

## <a name="upgrading"></a>Actualizando

Un clúster independiente es un recurso que es totalmente de su propiedad. Por tanto, usted es el responsable de la aplicación de revisiones del sistema operativo subyacente y de iniciar las actualizaciones de Service Fabric. Puede configurar el clúster para recibir las actualizaciones automáticas del entorno de ejecución cuando Microsoft publique una nueva versión, o bien seleccionar la versión compatible que desee del entorno de ejecución. Además de las actualizaciones de Service Fabric, también puede aplicar revisiones al sistema operativo y actualizar los valores de configuración del clúster, como los certificados o los puertos de aplicación. 

Para más información, lea [Actualización de clústeres independientes](service-fabric-cluster-upgrade-standalone.md).

## <a name="supported-operating-systems"></a>Sistemas operativos admitidos

Podrá crear clústeres en VM o equipos que ejecuten estos sistemas operativos (aún no se admite Linux):

* Windows Server 2012 R2
* Windows Server 2016 
* Windows Server 2019

## <a name="next-steps"></a>Pasos siguientes

Obtenga información más detallada sobre [protección](service-fabric-cluster-security.md), [escalado](service-fabric-cluster-scaling-standalone.md) y [actualización](service-fabric-cluster-upgrade-standalone.md) de los clústeres independientes.

Más información sobre las [opciones de soporte técnico de Service Fabric](service-fabric-support.md).
