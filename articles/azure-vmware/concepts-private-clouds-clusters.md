---
title: Conceptos sobre nubes privadas y clústeres
description: Obtenga información sobre las funcionalidades clave de los centros de datos definidos por software de Azure VMware Solution y los clústeres de vSphere.
ms.topic: conceptual
ms.date: 10/27/2020
ms.openlocfilehash: daa712a722ca3252a49e4f54e0cc9e42de4ffc73
ms.sourcegitcommit: 2a8a53e5438596f99537f7279619258e9ecb357a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "94337328"
---
#  <a name="azure-vmware-solution-private-cloud-and-cluster-concepts"></a>Conceptos de nubes privadas y clústeres de Azure VMware Solution

Azure VMware Solution ofrece nubes privadas basadas en VMware en Azure. Las nubes privadas contienen clústeres que se han creado con hosts dedicados sin sistema operativo de Azure. Se implementan y administran mediante Azure Portal, la CLI o PowerShell.  Los clústeres aprovisionados en las nubes privadas incluyen el software VMware vSphere, vCenter, vSAN y NSX. Las implementaciones de software y hardware en nubes privadas de Azure VMware Solution están totalmente integradas y automatizadas en Azure.

Existe una relación lógica entre las suscripciones a Azure, las nubes privadas de Azure VMware Solution, los clústeres de vSAN y los hosts. El diagrama muestra una suscripción a Azure única con dos nubes privadas que representan el entorno de desarrollo y producción.  En cada una de esas nubes privadas hay dos clústeres. 

En este artículo se describen todos estos conceptos.

![Imagen de dos nubes privadas en la suscripción de un cliente](./media/hosts-clusters-private-clouds-final.png)

>[!NOTE]
>Dadas las reducidas necesidades potenciales de un entorno de desarrollo, se usan clústeres más pequeños con hosts de menor capacidad. 

## <a name="private-clouds"></a>Nubes privadas

Las nubes privadas contienen clústeres vSAN que se han creado con hosts dedicados sin sistema operativo de Azure. Cada nube privada puede tener varios clústeres administrados por el mismo servidor vCenter y NSX-T Manager. Puede implementar y administrar nubes privadas en el portal, la CLI o PowerShell. 

Al igual que con otros recursos, las nubes privadas se instalan y administran desde una suscripción a Azure. El número de nubes privadas dentro de una suscripción es escalable. Inicialmente, hay un límite de una nube privada por suscripción.

## <a name="clusters"></a>Clústeres
De manera predeterminada, hay un clúster vSAN para cada nube privada que se crea. Puede agregar, eliminar y escalar clústeres mediante Azure Portal o a través de la API.  Todos los clústeres tienen un tamaño predeterminado de tres hosts y se pueden escalar hasta 16 hosts.  Los hosts que se usan en un clúster debe ser del mismo tipo de host.

Los clústeres de prueba están disponibles para su evaluación y están limitados a tres hosts. Hay un solo clúster de prueba por cada nube privada. Puede escalar un clúster de prueba en un solo host durante el período de evaluación.

Se usa vSphere y NSX-T Manager para administrar la mayoría de los otros aspectos de la configuración o del funcionamiento de los clústeres. Todo el almacenamiento local de cada host de un clúster está bajo el control de vSAN.

## <a name="hosts"></a>Hosts

Los clústeres de nube privada de Azure VMware Solution usan nodos de infraestructura hiperconvergidos sin sistema operativo. En la tabla siguiente se muestran las capacidades de RAM, CPU y disco del host. 

| Tipo de host              |             CPU             |   RAM (GB)   |  Nivel de caché NVMe de vSAN (TB, sin procesar)  |  Nivel de capacidad SSD de vSAN (TB, sin procesar)  |
| :---                   |            :---:            |    :---:     |               :---:              |                :---:               |
| High-End (HE)          |  Intel doble de 18 núcleos, 2,3 GHz  |     576      |                3.2               |                15,20               |

Los hosts que se usan para crear o escalar clústeres provienen de un grupo de hosts aislado. Esos hosts han superado las pruebas de hardware, y se han eliminado todos los datos de manera segura. 

## <a name="vmware-software-versions"></a>Versiones de software de VMware

Las versiones actuales del software de VMware que se usan en los clústeres de nube privada de Azure VMware Solution son:

| Software              |    Versión   |
| :---                  |     :---:    |
| VCSA/vSphere/ESXi |    6.7 U3    | 
| ESXi                  |    6.7 U3    | 
| vSAN                  |    6.7 U3    |
| NSX-T                 |      2.5     |

En el caso de cualquier clúster nuevo en una nube privada, la versión del software coincide con la que se está ejecutando actualmente. Para cualquier nube privada nueva en una suscripción, se instala la versión más reciente de la pila de software.

Podrá encontrar las directivas y procesos de actualización generales del software de la plataforma Azure VMware Solution descritos en el artículo sobre [conceptos de actualización](concepts-upgrades.md).

## <a name="host-maintenance-and-lifecycle-management"></a>Mantenimiento y administración del ciclo de vida de hosts

El mantenimiento y la administración del ciclo de vida no afectan la capacidad ni el rendimiento de los clústeres de la nube privada.  Entre los ejemplos de mantenimiento automatizado de los hosts se incluyen las actualizaciones de firmware y la reparación o sustitución de hardware.

Microsoft es responsable de la administración del ciclo de vida de los dispositivos NSX-T, como NSX-T Manager y NSX-T Edge. También se encarga de la configuración de la red de arranque, como la creación de la puerta de enlace de nivel 0 y la habilitación del enrutamiento vertical de arriba abajo. Usted es responsable de la configuración de NSX-T SDN. Por ejemplo, segmentos de red, reglas de firewall distribuidas, puertas de enlace de nivel 1 y equilibradores de carga.

> [!IMPORTANT]
> No modifique la configuración de las puertas de enlace de NSX-T Edge o de nivel 0, ya que esto puede dar lugar a una pérdida de servicio.

## <a name="backup-and-restoration"></a>Copia de seguridad y restauración

Las configuraciones de vCenter y NSX-T de la nube privada se encuentran en una programación de copia de seguridad.  Las copias de seguridad se conservan durante tres días. Si tiene que realizar una restauración a partir de una copia de seguridad, abra una [solicitud de soporte técnico](https://rc.portal.azure.com/#create/Microsoft.Support) en Azure Portal para solicitar la restauración.

## <a name="next-steps"></a>Pasos siguientes

El siguiente paso es aprender los [conceptos de redes e interconectividad](concepts-networking.md).

<!-- LINKS - internal -->

<!-- LINKS - external-->
[VCSA versions]: https://kb.vmware.com/s/article/2143838
[ESXi versions]: https://kb.vmware.com/s/article/2143832
[vSAN versions]: https://kb.vmware.com/s/article/2150753

