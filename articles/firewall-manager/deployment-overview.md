---
title: Información general sobre la implementación de Azure Firewall Manager (versión preliminar)
description: Conozca los pasos de implementación de alto nivel necesarios para Azure Firewall Manager (versión preliminar)
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: overview
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: c3a94cea838609f65511a21ee2f64e8782a6adea
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "77443132"
---
# <a name="azure-firewall-manager-preview-deployment-overview"></a>Información general sobre la implementación de Azure Firewall Manager (versión preliminar)

[!INCLUDE [Preview](../../includes/firewall-manager-preview-notice.md)]

Hay más de una forma de implementar la versión preliminar de Azure Firewall Manager, pero se recomienda el siguiente proceso general.

## <a name="general-deployment-process"></a>Proceso de implementación general

### <a name="hub-virtual-networks"></a>Redes virtuales del centro de conectividad

1.  Cree una directiva de firewall

    - Creación de una nueva directiva
<br>*or*<br>
    - Derive una directiva base y personalización de una directiva local
<br>*or*<br>
    - Importe reglas de una instancia de Azure Firewall existente. Asegúrese de quitar las reglas NAT de las directivas que deben aplicarse en varios firewalls.
1. Creación de una arquitectura de concentrador y radio
   - Cree una red virtual de centro de conectividad mediante Azure Firewall Manager y empareje redes virtuales radiales con ella mediante el emparejamiento de redes virtuales
<br>*or*<br>
    - Cree una red virtual, agregue conexiones de red virtual y empareje redes virtuales radiales con ella mediante el emparejamiento de redes virtuales

3. Seleccione los proveedores de seguridad y asocie la directiva de firewall. Actualmente, Azure Firewall es el único proveedor admitido.

   - Esto se hace al crear una red virtual del centro de conectividad
<br>*or*<br>
    - Convierta una red virtual existente en una red virtual del centro de conectividad. También es posible convertir varias redes virtuales.

4. Configure rutas definidas por el usuario para enrutar el tráfico al firewall de la red virtual del centro de conectividad.


### <a name="secured-virtual-hubs"></a>Centros virtuales protegidos

1. Creación de una arquitectura de concentrador y radio

   - Cree un centro virtual protegido mediante Azure Firewall Manager y agregue conexiones de red virtual.<br>*or*<br>
   - Cree un centro de conectividad de Virtual WAN y agregue conexiones de red virtual.
2. Selección de proveedores de seguridad

   - Se realiza al crear un centro virtual protegido.<br>*or*<br>
   - Convierta un centro de conectividad de Virtual WAN en un centro virtual seguro.
3. Creación de una directiva de firewall y asociación con su concentrador

   - Solo se aplica si se usa Azure Firewall.
   - Las directivas de seguridad como servicio (SECaaS) de terceros se configuran a través de la experiencia de administración de asociados.
4. Configuración de los valores de la ruta para enrutar el tráfico a su centro protegido

   - Enrute fácilmente el tráfico al centro protegido para filtrar y registrar rutas definidas por el usuario (UDR) en redes virtuales de radios mediante la página de configuración de rutas del centro virtual protegido.

> [!NOTE]
> - No se puede tener más de un concentrador por red WAN virtual por región. Sin embargo, es posible agregar varias redes WAN virtuales en la región para lograrlo.
> - No se pueden tener espacios de direcciones IP superpuestos para los concentradores de una vWAN.
> - Las conexiones de red virtual del concentrador deben estar en la misma región que el concentrador.

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Protección de una red en la nube con la versión preliminar de Azure Firewall Manager en Azure Portal](secure-cloud-network.md)