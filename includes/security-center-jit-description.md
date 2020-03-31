---
author: memildin
ms.author: memildin
manager: rkarlin
ms.date: 02/24/2020
ms.topic: include
ms.openlocfilehash: c77849b2285283a34e6adf84dc3845a4076407af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77597954"
---
## <a name="attack-scenario"></a>Escenario de ataque

Normalmente, los ataques por fuerza bruta tienen como destino los puertos de administración para obtener acceso a una máquina virtual. Si un atacante tiene éxito, puede tomar el control de la máquina virtual y establecer un punto de apoyo en su entorno.

Una manera de reducir el riesgo de sufrir un ataque por fuerza bruta consiste en limitar el tiempo que está abierto un puerto. No es necesario que los puertos de administración estén abiertos en todo momento. Solo deben estar abiertos mientras se está conectado a la máquina virtual, por ejemplo, para realizar tareas de administración o mantenimiento. Cuando se habilita Just-In-Time, Security Center usa las reglas del [grupo de seguridad de red](../articles/virtual-network/security-overview.md#security-rules) (NSG) y Azure Firewall, que restringen el acceso a los puertos de administración para que no puedan ser objeto de ataques.

![Escenario de Just-in-time](../articles/security-center/media/security-center-just-in-time/just-in-time-scenario.png)

## <a name="how-does-jit-access-work"></a>¿Cómo funciona el acceso JIT?

Cuando se habilita Just-In-Time, Security Center bloquea el tráfico entrante a las máquinas virtuales de Azure mediante la creación de una regla de NSG. Se deben seleccionar los puertos de la máquina virtual para la que se bloqueará el tráfico entrante. Estos puertos los controla la solución Just-In-Time.

Cuando un usuario solicita acceso a una máquina virtual, Security Center comprueba que el usuario tenga permisos de [control de acceso basado en rol (RBAC)](../articles/role-based-access-control/role-assignments-portal.md) para la máquina virtual. Si la solicitud se aprueba, Security Center configura automáticamente los grupos de seguridad de red (NSG) y Azure Firewall para permitir el tráfico entrante a los puertos seleccionados y a las direcciones o rangos IP de origen durante el periodo especificado. Una vez transcurrido ese tiempo, Security Center restaura los NSG a su estado anterior. Pero las conexiones que ya están establecidas no se interrumpen.

 > [!NOTE]
 > Si se aprueba una solicitud de acceso JIT para una VM detrás de una instancia de Azure Firewall, Security Center cambia automáticamente las reglas de directiva de NSG y el firewall. Para la cantidad de tiempo que se especificó, las reglas permiten tráfico entrante a los puertos seleccionados y las direcciones o rangos IP de origen solicitados. Una vez transcurrido el tiempo, Security Center restaura las reglas del firewall y NSG a su estado anterior.


## <a name="permissions-needed-to-configure-and-use-jit"></a>Permisos necesarios para configurar y usar JIT

| Para permitir a los usuarios: | Permisos que se deben establecer|
| --- | --- |
| Configurar o editar una directiva JIT para una VM | *Asigne estas acciones al rol.*  <ul><li>En el ámbito de una suscripción o un grupo de recursos asociados a la máquina virtual:<br/> `Microsoft.Security/locations/jitNetworkAccessPolicies/write` </li><li> En el ámbito de una suscripción o grupo de recursos de una máquina virtual: <br/>`Microsoft.Compute/virtualMachines/write`</li></ul> | 
|Solicitud de acceso Just-In-Time a una máquina virtual | *Asigne estas acciones al usuario.*  <ul><li>En el ámbito de una suscripción o un grupo de recursos asociados a la máquina virtual:<br/>  `Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action` </li><li>En el ámbito de una suscripción o un grupo de recursos asociados a la máquina virtual:<br/>  `Microsoft.Security/locations/jitNetworkAccessPolicies/*/read` </li><li>  En el ámbito de una suscripción, un grupo de recursos o una máquina virtual:<br/> `Microsoft.Compute/virtualMachines/read` </li><li>  En el ámbito de una suscripción, un grupo de recursos o una máquina virtual:<br/> `Microsoft.Network/networkInterfaces/*/read` </li></ul>|