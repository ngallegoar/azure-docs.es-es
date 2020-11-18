---
title: archivo de inclusión
description: archivo de inclusión
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 11/05/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 57b0bb9ab8ceb34021a38db0d0abf2c9c919e808
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2020
ms.locfileid: "94356659"
---
### <a name="can-i-still-deploy-multiple-bastion-hosts-across-peered-virtual-networks"></a>¿Puedo seguir implementando varios hosts bastión en redes virtuales emparejadas?

Sí. De forma predeterminada, un usuario ve el host bastión que se implementa en la misma red virtual en la que reside la máquina virtual. Sin embargo, en el menú **Conectar**, el usuario puede ver varios hosts bastión detectados en redes emparejadas. Pueden seleccionar el host bastión que prefieren usar para conectarse a la máquina virtual implementada en la red virtual.

### <a name="if-my-peered-vnets-are-deployed-in-different-subscriptions-will-connectivity-via-bastion-work"></a>Si las redes virtuales emparejadas se implementan en distintas suscripciones, ¿se realizará la conectividad mediante el trabajo de Bastion?

Sí, la conectividad a través de Bastion continuará funcionando para redes virtuales emparejadas en distintas suscripciones para un solo inquilino. No se admiten suscripciones en dos inquilinos diferentes. Para ver Bastion en el menú desplegable **Conectar**, el usuario debe seleccionar las suscripciones a las que tiene acceso en **Suscripción > Suscripción global**.

:::image type="content" source="./media/bastion-faq-peering-include/global-subscriptions.png" alt-text="Filtro de suscripciones globales" lightbox="./media/bastion-faq-peering-include/global-subscriptions.png":::

### <a name="i-have-access-to-the-peered-vnet-but-i-cant-see-the-vm-deployed-there"></a>Tengo acceso a la red virtual emparejada, pero no veo que la máquina virtual se haya implementado allí.

Asegúrese de que el usuario tenga acceso de **lectura** tanto a la máquina virtual como a la red virtual emparejada. Además, compruebe en IAM que el usuario tiene acceso de **lectura** a los siguientes recursos:

* Rol Lector en la máquina virtual.
* Rol Lector en la tarjeta de interfaz de red con la dirección IP privada de la máquina virtual.
* Rol Lector en el recurso de Azure Bastion.
* Rol Lector en la red virtual (no es necesario si no hay ninguna red virtual emparejada).

|Permisos|Descripción|Tipo de permiso|
|---|---| ---|
|Microsoft.Network/bastionHosts/read |Obtiene un host de tipo bastión.|Acción|
|Microsoft.Network/virtualNetworks/BastionHosts/action |Obtiene las referencias del host de tipo bastión en una red virtual.|Acción|
|Microsoft.Network/virtualNetworks/bastionHosts/default/action|Obtiene las referencias del host de tipo bastión en una red virtual.|Acción|
|Microsoft.Network/networkInterfaces/read|Obtiene una definición de interfaz de red.|Acción|
|Microsoft.Network/networkInterfaces/ipconfigurations/read|Obtiene una definición de configuración de dirección IP de la interfaz de red.|Acción|
|Microsoft.Network/virtualNetworks/read|Obtiene la definición de red virtual|Acción|
|Microsoft.Network/virtualNetworks/subnets/virtualMachines/read|Obtiene referencias a todas las máquinas virtuales de una subred de red virtual|Acción|
|Microsoft.Network/virtualNetworks/virtualMachines/read|Obtiene referencias a todas las máquinas virtuales de una red virtual|Acción|