---
title: 'Conceptos: supervisión y reparación de nubes privadas de Azure VMware Solution'
description: Obtenga información sobre cómo Azure VMware Solution supervisa y repara servidores VMware ESXi en una nube privada de Azure VMware Solution.
ms.topic: conceptual
ms.date: 09/16/2020
ms.openlocfilehash: bee08304cd14f2aeec6995203638b5a37d9a861c
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91340148"
---
# <a name="monitor-and-repair-azure-vmware-solution-private-clouds"></a>Supervisión y reparación de nubes privadas de Azure VMware Solution

Azure VMware Solution supervisa continuamente los servidores VMware ESXi en una nube privada de Azure VMware Solution. 

## <a name="what-azure-vmware-solution-monitors"></a>Qué supervisa Azure VMware Solution

Azure VMware Solution supervisa las siguientes condiciones de error en el host:  

- Estado del procesador 
- Estado de la memoria 
- Estado de conexión y energía 
- Estado del ventilador de hardware 
- Pérdida de conectividad de red 
- Estado de la placa del sistema de hardware 
- Errores en los discos de un host de vSAN 
- Voltaje de hardware 
- Estado de la temperatura de hardware 
- Estado de la energía de hardware 
- Estado de almacenamiento 
- Error de conexión 

> [!NOTE]
> Los administradores de inquilinos de Azure VMware Solution no deben editar ni eliminar las alarmas de VMware vCenter definidas anteriormente, ya que las administra el plano de control de Azure VMware Solution en vCenter. La funcionalidad de supervisión de Azure VMware Solution usa estas alarmas para desencadenar el proceso de corrección del host de Azure VMware Solution.

## <a name="azure-vmware-solution-host-remediation"></a>Corrección del host de Azure VMware Solution  

Cuando Azure VMware Solution detecta una degradación o un error en un nodo de Azure VMware Solution en la nube privada de un inquilino, desencadena el proceso de corrección del host. La corrección del host implica reemplazar el nodo defectuoso por un nuevo nodo en buen estado.  

El proceso de corrección del host se inicia agregando un nuevo nodo en buen estado en el clúster. A continuación, cuando sea posible, el host defectuoso se coloca en el modo de mantenimiento VMware vSphere. VMware vMotion se usa para trasladar las máquinas virtuales fuera del host defectuoso a otros servidores disponibles en el clúster, lo que puede permitir la migración en vivo de las cargas de trabajo sin tiempo de inactividad. En escenarios en los que el host defectuoso no se puede poner en modo de mantenimiento, se quita del clúster.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre las [actualizaciones de la nube privada de Azure VMware Solution](concepts-upgrades.md).  
