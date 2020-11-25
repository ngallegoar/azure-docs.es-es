---
title: Requisitos de red adicionales para SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Requisitos de red adicionales para SAP HANA en Azure (instancias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 44e9712466734c0e42fd2bea05c5110cbff6924b
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94964803"
---
# <a name="additional-network-requirements-for-large-instances"></a>Requisitos de red adicionales para instancias grandes

Es posible que tenga requisitos de red adicionales como parte de la implementación de instancias grandes de SAP HANA en Azure.

## <a name="add-more-ip-addresses-or-subnets"></a>Incorporación de más direcciones IP o subredes

Use Azure Portal, PowerShell o la CLI de Azure al agregar más direcciones IP o subredes.

Agregue el nuevo intervalo de direcciones IP como un nuevo intervalo al espacio de direcciones de la red virtual en lugar de generar un nuevo intervalo agregado. Envíe este cambio a Microsoft. Esto le permite conectarse desde ese nuevo intervalo de direcciones IP a las unidades de instancias grandes de HANA del cliente. Puede abrir una solicitud de soporte técnico de Azure para obtener el nuevo espacio de direcciones de red virtual agregado. Después de recibir la confirmación, realice los pasos siguientes.

Para crear una subred adicional desde Azure Portal, consulte [Creación de una red virtual mediante Azure Portal](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network). Para crear una red virtual desde PowerShell, consulte [Creación de una red virtual mediante PowerShell](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network).

## <a name="add-virtual-networks"></a>Incorporación de redes virtuales

Después de conectarse inicialmente a una o varias redes virtuales de Azure, puede conectar otras adicionales que tengan acceso a SAP HANA en Azure (instancias grandes). En primer lugar, envíe una solicitud de soporte técnico de Azure. En esa solicitud, incluya la información específica para identificar la implementación de Azure concreta. Incluya también el intervalo o intervalos del espacio de direcciones IP del espacio de direcciones de red virtual de Azure. A continuación, SAP HANA en Microsoft Service Management proporciona la información necesaria para conectar las redes virtuales adicionales y Azure ExpressRoute. Para cada red virtual necesita una clave de autorización única a fin de conectar el circuito ExpressRoute a instancias grandes de HANA.

## <a name="increase-expressroute-circuit-bandwidth"></a>Aumento del ancho de banda del circuito ExpressRoute

Consulte con SAP HANA en Microsoft Service Management. Si se le recomienda aumentar el ancho de banda del circuito ExpressRoute de SAP HANA en Azure (instancias grandes), cree una solicitud de soporte técnico de Azure. Puede solicitar un aumento del ancho de banda de un solo circuito de hasta 10 Gbps. Recibirá una notificación cuando la operación se complete; no es necesaria ninguna acción adicional para habilitar esta velocidad superior en Azure.

## <a name="add-an-additional-expressroute-circuit"></a>Incorporación de un circuito ExpressRoute adicional

Consulte con SAP HANA en Microsoft Service Management. Si se le recomienda agregar un circuito ExpressRoute adicional, cree una solicitud de soporte técnico de Azure (incluida una solicitud para obtener información de autorización para conectarse al circuito nuevo). Antes de realizar la solicitud, debe definir el espacio de direcciones usado en las redes virtuales. A continuación, SAP HANA en Microsoft Service Management puede proporcionar autorización.

Una vez creado el circuito y terminada la configuración en SAP HANA en Microsoft Service Management, recibirá una notificación con la información que necesita para continuar. No puede conectar redes virtuales de Azure a este circuito adicional si ya están conectadas a otro circuito ExpressRoute de SAP HANA en Azure (instancias grandes) en la misma región de Azure.

## <a name="delete-a-subnet"></a>Eliminación de una subred

Para eliminar una subred de red virtual, puede usar Azure Portal, PowerShell o la CLI de Azure. Si el espacio de direcciones o el intervalo de direcciones IP de la red virtual de Azure fuesen intervalos agregados, no son necesarias acciones con Microsoft. (Sin embargo, tenga en cuenta que la red virtual todavía está propagando el espacio de direcciones de la ruta BGP que incluye la subred eliminada). Es posible que haya definido el espacio de direcciones o el intervalo de direcciones de la red virtual de Azure como varios intervalos de direcciones IP, de los cuales uno se asignó a la subred eliminada. Asegúrese de eliminar el espacio de direcciones de red virtual. A continuación, informe a SAP HANA en Microsoft Service Management para quitarlo de los intervalos con los que SAP HANA en Azure (instancias grandes) tiene permiso para comunicarse.

Para obtener más información, consulte [Eliminación de una subred](../../../virtual-network/virtual-network-manage-subnet.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#delete-a-subnet).

## <a name="delete-a-virtual-network"></a>Eliminar una red virtual

Para obtener más información, consulte [Eliminar una red virtual](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#delete-a-virtual-network).

SAP HANA en Microsoft Service Management quita las autorizaciones existentes en SAP HANA en el circuito ExpressRoute de SAP HANA en Azure (instancias grandes). También quita el espacio de direcciones o el intervalo de direcciones IP de la red virtual de Azure para la comunicación con instancias grandes de HANA.

Tras quitar la red virtual, abra una solicitud de soporte técnico de Azure para proporcionar el intervalo de espacios de direcciones IP o los intervalos que se van a quitar.

Para asegurarse de que se quite todo, elimine la conexión de ExpressRoute, la puerta de enlace de red virtual, la IP pública de puerta de enlace de red virtual y la red virtual.

## <a name="delete-an-expressroute-circuit"></a>Eliminación de un circuito ExpressRoute

Para quitar un circuito ExpressRoute adicional de SAP HANA en Azure (instancias grandes), abra una solicitud de soporte técnico de Azure en SAP HANA en Microsoft Service Management. Solicite que se elimine el circuito. En la suscripción de Azure, puede eliminar o mantener la red virtual, según sea necesario. Sin embargo, debe eliminar la conexión entre el circuito ExpressRoute de HANA de instancias grandes y la puerta de enlace de la red virtual vinculada.

## <a name="next-steps"></a>Pasos siguientes

- [Procedimiento para instalar y configurar SAP HANA en Azure (instancias grandes)](hana-installation.md)
