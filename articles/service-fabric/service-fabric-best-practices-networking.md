---
title: Procedimientos recomendados de red de Azure Service Fabric
description: Procedimientos recomendados y consideraciones de diseño para administrar la conectividad de red mediante Azure Service Fabric.
author: peterpogorski
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: de2a74ad2d61de18d2150b72be3251e5b5583f2e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75551801"
---
# <a name="networking"></a>Redes

Al crear y administrar clústeres de Azure Service Fabric, proporcionará conectividad de red para los nodos y aplicaciones. Los recursos de red incluyen intervalos de direcciones IP, redes virtuales, equilibradores de carga y grupos de seguridad de red. En este artículo, conocerá los procedimientos recomendados para estos recursos.

Revise los [patrones de redes de Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking) de Azure para aprender a crear clústeres que usen las siguientes características: red virtual o subred existentes, dirección IP pública estática, equilibrador de carga solo interno y equilibrador de carga solo externo.

## <a name="infrastructure-networking"></a>Redes de infraestructura
Maximice el rendimiento de su máquina virtual con redes aceleradas; para ello, declare la propiedad enableAcceleratedNetworking en la plantilla de Resource Manager. El siguiente fragmento de código es de un conjunto de escalado de máquinas virtuales, NetworkInterfaceConfigurations, que permite redes aceleradas:

```json
"networkInterfaceConfigurations": [
  {
    "name": "[concat(variables('nicName'), '-0')]",
    "properties": {
      "enableAcceleratedNetworking": true,
      "ipConfigurations": [
        {
        <snip>
        }
      ],
      "primary": true
    }
  }
]
```
El clúster de Service Fabric se puede aprovisionar en [Linux con redes aceleradas](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli) y [Windows con redes aceleradas](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell).

Las redes aceleradas se admiten para las SKU de la serie de máquina virtual de Azure: D/DSv2, D/DSv3, E/ESv3, F/FS, FSv2 y Ms/Mms. Las redes aceleradas se probaron correctamente con la SKU Standard_DS8_v3 el 23 de enero de 2019 en un clúster de Windows de Service Fabric y con Standard_DS12_v2 el 29 de enero de 2019 en un clúster de Linux de Service Fabric.

Para habilitar las redes aceleradas en un clúster de Service Fabric existente, primero debe [escalar un clúster de Service Fabric horizontalmente mediante la incorporación de un conjunto de escalado de máquinas virtuales](https://docs.microsoft.com/azure/service-fabric/virtual-machine-scale-set-scale-node-type-scale-out), para realizar lo siguiente:
1. Aprovisionar un elemento NodeType con las redes aceleradas habilitadas
2. Migrar los servicios y su estado al elemento NodeType aprovisionado con las redes aceleradas habilitadas

Para habilitar las redes aceleradas en un clúster existente, es necesario escalar horizontalmente la infraestructura dado que si se habilitan tal y como está, se produciría tiempo de inactividad puesto que todas las máquinas virtuales de un conjunto de disponibilidad se tendrían que [detener y desasignar antes de habilitarlas en una NIC existente](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli#enable-accelerated-networking-on-existing-vms).

## <a name="cluster-networking"></a>Redes de clúster

* Los clústeres de Service Fabric se pueden implementar en una red virtual existente mediante los pasos descritos en [Patrones de redes de Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking).

* Se recomiendan grupos de seguridad de red (NSG) para los tipos de nodo que restringen el tráfico entrante y saliente a su clúster. Asegúrese de que los puertos necesarios estén abiertos en el NSG. Por ejemplo: ![Reglas de NSG de Service Fabric][NSGSetup]

* El tipo de nodo principal, que contiene los servicios del sistema de Service Fabric, no debe exponerse a través del equilibrador de carga externo y puede exponerse mediante un [equilibrador de carga interno](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking#internal-only-load-balancer).

* Use una [dirección IP pública estática](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking#static-public-ip-address-1) para el clúster.

## <a name="application-networking"></a>Redes de aplicación

* Para ejecutar cargas de trabajo de contenedor de Windows, use el [modo de red abierto](https://docs.microsoft.com/azure/service-fabric/service-fabric-networking-modes#set-up-open-networking-mode) para facilitar la comunicación de servicio a servicio.

* Use un proxy inverso, como [Traefik](https://docs.traefik.io/v1.6/configuration/backends/servicefabric/) o el [proxy inverso de Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) para exponer los puertos de aplicación comunes, como 80 o 443.

* En el caso de los contenedores de Windows hospedados en máquinas que no tienen una conexión física que no pueden extraer los niveles de base del almacenamiento en la nube de Azure, anule el comportamiento del nivel externo con el uso de la marca [--allow-nondistributable-artifacts](https://docs.microsoft.com/virtualization/windowscontainers/about/faq#how-do-i-make-my-container-images-available-on-air-gapped-machines) en el demonio de Docker.

## <a name="next-steps"></a>Pasos siguientes

* Creación de un clúster en máquinas virtuales o equipos que ejecutan Windows Server: [Creación de un clúster independiente con Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Creación de un clúster en máquinas virtuales o equipos que ejecutan Linux: [Crear un clúster Linux](service-fabric-cluster-creation-via-portal.md)
* Más información sobre las [opciones de soporte técnico de Service Fabric](service-fabric-support.md)

[NSGSetup]: ./media/service-fabric-best-practices/service-fabric-nsg-rules.png
