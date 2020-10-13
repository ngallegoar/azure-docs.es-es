---
title: 'Conceptos: Actualizaciones y mejoras de la nube privada'
description: Conozca los procesos de actualización y las características principales de Azure VMware Solution.
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: 380e97eae559145a9ef5ed7b6e7bf14f18039eed
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91316807"
---
# <a name="azure-vmware-solution-private-cloud-updates-and-upgrades"></a>Actualizaciones y mejoras de la nube privada de Azure VMware Solution

## <a name="overview"></a>Introducción

Una de las principales ventajas de las nubes privadas de Azure VMware Solution es que la plataforma se mantiene automáticamente. El mantenimiento de la plataforma incluye actualizaciones automatizadas de un paquete de software validado de VMware, lo que ayuda a garantizar que se usa la versión más reciente del software validado de nube privada de Azure VMware Solution.

En concreto, una nube privada de Azure VMware Solution incluye:

- Nodos de servidor sin sistema operativo dedicados aprovisionados con el hipervisor de VMware ESXi 
- Servidor vCenter para administrar ESXi y vSAN 
- Redes definidas por software de NSX-T de VMware para máquinas virtuales de carga de trabajo de vSphere  
- Almacén de datos de vSAN de VMware para máquinas virtuales de carga de trabajo de vSphere  
- HCX de VMware para la movilidad de la carga de trabajo  

Además de estos componentes, una nube privada de Azure VMware Solution incluye recursos en el componente subyacente de Azure necesario para la conectividad y para operar la nube privada. Azure VMware Solution supervisa continuamente el estado de los componentes subyacentes y de VMware. Cuando Azure VMware Solution detecta un error, toma medidas para reparar los componentes que no funcionan. 

## <a name="what-components-get-updated"></a>¿Qué componentes se actualizan?   

Azure VMware Solution actualiza los siguientes componentes de VMware: 

- vCenter Server y ESXi se ejecutan en los nodos de servidor sin sistema operativo 
- vSAN 
- NSX-T 

Azure VMware Solution también actualiza el software del componente subyacente, como controladores, software de los conmutadores de red y firmware de los nodos sin sistema operativo. 

## <a name="types-of-updates"></a>Tipos de actualizaciones

Azure VMware Solution aplica los siguientes tipos de actualizaciones a componentes de VMware:

- Revisiones: Revisiones de seguridad y correcciones de errores publicadas por VMware. 
- Actualizaciones: Actualizaciones de versiones secundarias de uno o más varios de VMware. 
- Mejoras: Actualizaciones de versiones principales de uno o varios componentes de VMware.

Recibirá una notificación antes y después de que las revisiones se apliquen a las nubes privadas. También trabajaremos con usted para programar una ventana de mantenimiento antes de aplicar actualizaciones o mejoras a la nube privada. 

## <a name="vmware-appliance-backup"></a>Copia de seguridad de dispositivos de VMware 

Además de realizar actualizaciones, Azure VMware Solution hace una copia de seguridad de configuración de estos componentes de VMware:

- vCenter Server 
- NSX-T Manager 

Cuando se produzcan errores, Azure VMware Solution puede restaurarlos desde la copia de seguridad de la configuración. 

Para más información sobre las versiones de software de VMware, consulte el [artículo sobre los conceptos de nubes privadas y clústeres](concepts-private-clouds-clusters.md) y las [preguntas frecuentes](faq.md).

## <a name="next-steps"></a>Pasos siguientes

El siguiente paso consiste en [crear una nube privada](tutorial-create-private-cloud.md).

<!-- LINKS - external -->

<!-- LINKS - internal -->
