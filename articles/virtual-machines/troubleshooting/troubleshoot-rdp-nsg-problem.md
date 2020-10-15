---
title: No se puede conectar a VM de Azure porque el puerto RDP no está habilitado en NSG | Microsoft Docs
description: Obtenga información sobre cómo solucionar un problema por el que se produce un error de RDP debido a la configuración de NSG en Azure Portal | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: v-jesits
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/20/2018
ms.author: genli
ms.openlocfilehash: 878e2c233f2171c3c9a6fbd2a8d629d3f3987c3a
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91976732"
---
#  <a name="cannot-connect-remotely-to-a-vm-because-rdp-port-is-not-enabled-in-nsg"></a>No puede conectarse de forma remota a una VM porque el puerto RDP no está habilitado en NSG

En este artículo se explica cómo resolver un problema por el que no puede conectarse a una máquina virtual (VM) Windows de Azure porque el puerto del protocolo de escritorio remoto (RDP) no está habilitado en el grupo de seguridad de red (NSG).


## <a name="symptom"></a>Síntoma

No puede establecer una conexión RDP a una VM en Azure porque el puerto RDP no está abierto en el grupo de seguridad de red.

## <a name="solution"></a>Solución 

Cuando se crea una VM, todo el tráfico de Internet se bloquea de forma predeterminada. 

Para habilitar el puerto RDP en un NSG, siga estos pasos:
1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. En **Máquinas virtuales**, seleccione la VM que tiene el problema. 
3. En **Configuración**, seleccione **Redes**. 
4. En **Reglas de puerto de entrada**, compruebe si el puerto de RDP se estableció correctamente. El siguiente es un ejemplo de la configuración: 

    **Prioridad**: 300 </br>
    **Name**: Port_3389 </br>
    **Puerto (destino)** : 3389 </br>
    **Protocolo**: TCP </br>
    **Origen**: Any </br>
    **Destinos**: Any </br>
    **Acción**: Allow </br>

Si especifica la dirección IP de origen, esta opción permite el tráfico solo desde una dirección IP específica o intervalo de direcciones IP para conectarse a la VM. Asegúrese de que el equipo que use para iniciar la sesión de RDP esté dentro del intervalo.

Para más información sobre NSG, consulte [Grupos de seguridad de red](../../virtual-network/network-security-groups-overview.md).

> [!NOTE]
> El puerto 3389 de RDP se expone a Internet. Por lo tanto, se recomienda usar este puerto solo para pruebas. Para entornos de producción, se recomienda usar una VPN o conexión privada.

## <a name="next-steps"></a>Pasos siguientes

Si el puerto RDP ya está habilitado en NSG, consulte [Solución de problemas de un error general de RDP en una máquina virtual Windows en Azure](./troubleshoot-rdp-general-error.md).