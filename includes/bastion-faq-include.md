---
title: archivo de inclusión
description: archivo de inclusión
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 03/25/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 57a764b62fcda333f042794e176c24c8e6cc5526
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "80374045"
---
### <a name="which-regions-are-available"></a><a name="regions"></a>¿En qué regiones está disponible?

[!INCLUDE [region](bastion-regions-include.md)]

### <a name="do-i-need-a-public-ip-on-my-virtual-machine"></a><a name="publicip"></a>¿Necesito una dirección IP pública en mi máquina virtual?

Si va a conectar una máquina virtual mediante Azure Bastion, NO necesitará ninguna dirección IP pública en la máquina virtual de Azure a la que se va a conectar. El servicio Bastion abrirá la sesión o conexión RDP/SSH a la máquina virtual a través de la dirección IP privada de su máquina virtual dentro de su red virtual.

### <a name="is-ipv6-supported"></a>¿Se admite IPv6?

Actualmente, IPv6 no se admite. Azure Bastion solo admite IPv4.

### <a name="do-i-need-an-rdp-or-ssh-client"></a><a name="rdpssh"></a>¿Necesito un cliente RDP o SSH?

No es necesario un cliente RDP o SSH para que el protocolo de escritorio remoto/Secure Shell accedan a su máquina virtual de Azure en Azure Portal. Use [Azure Portal](https://portal.azure.com) para que RDP/SSH acceda a su máquina virtual directamente en el explorador.

### <a name="does-azure-bastion-require-an-rds-cal-for-administrative-purposes-on-azure-hosted-vms"></a><a name="rdscal"></a>¿Requiere Azure Bastion una CAL de RDS con fines administrativos en máquinas virtuales hospedadas en Azure?
No, el acceso a las máquinas virtuales Windows Server con Azure Bastion no requiere una [CAL de RDS](https://www.microsoft.com/en-us/p/windows-server-remote-desktop-services-cal/dg7gmgf0dvsv?activetab=pivot:overviewtab) cuando se utiliza únicamente con fines administrativos.

### <a name="how-many-concurrent-rdp-and-ssh-sessions-does-each-azure-bastion-support"></a><a name="limits"></a>¿Cuántas sesiones RDP y SSH simultáneas admite cada instancia de Azure Bastion?
Tanto RDP como SSH son protocolos basados en el uso. El uso intensivo de las sesiones hará que el host bastión admita un número total de sesiones inferior. Las siguientes cifras presuponen unos flujos de trabajo normales rutinarios.

[!INCLUDE [limits](bastion-limits.md)]

### <a name="do-i-need-an-agent-running-in-the-azure-virtual-machine"></a><a name="agent"></a>¿Es necesario que un agente se ejecute en la máquina virtual de Azure?

No es preciso instalar un agente ni ningún otro software en el explorador ni en la máquina virtual de Azure. El servicio Bastion no utiliza agentes y no requiere software adicional para RDP/SSH.

### <a name="which-browsers-are-supported"></a><a name="browsers"></a>¿Qué exploradores se admiten?

Utilice el explorador Microsoft Edge o Google Chrome en Windows. Para Apple Mac, use Google Chrome. Microsoft Edge Chromium también es compatible tanto con Windows como con Mac.

### <a name="are-any-roles-required-to-access-a-virtual-machine"></a><a name="roles"></a>¿Se necesitan roles para acceder a una máquina virtual?

Para crear una conexión, se requieren los siguientes roles:

* Rol de lector en la máquina virtual
* Rol de lector en la tarjeta de interfaz de red con la dirección IP privada de la máquina virtual
* Rol de lector en el recurso de Azure Bastion

### <a name="what-is-the-pricing"></a><a name="pricingpage"></a>¿Cuáles son los precios?

Consulte la [página de precios](https://aka.ms/BastionHostPricing)para obtener más información.

### <a name="why-do-i-get-your-session-has-expired-error-message-before-the-bastion-session-starts"></a><a name="session"></a>¿Por qué obtengo el error "Su sesión ha expirado" antes de iniciar la sesión de Bastion?

Las sesiones deben iniciarse solo en Azure Portal. Inicie sesión en Azure Portal y vuelva a iniciar una sesión. Si va a la dirección URL directamente desde otra sesión del explorador o pestaña, este error es previsible. Ayuda a garantizar que la sesión sea más segura y que se solo se pueda acceder a la sesión desde Azure Portal.

### <a name="what-keyboard-layouts-are-supported-during-the-bastion-remote-session"></a><a name="keyboard"></a>¿Qué distribuciones de teclado se admiten durante la sesión remota Bastion?

Actualmente, Azure Bastion admite la distribución de teclado en-US-QWERTY dentro de la máquina virtual.  La compatibilidad con otras configuraciones regionales para la distribución del teclado está en curso.

### <a name="is-user-defined-routing-udr-supported-on-an-azure-bastion-subnet"></a><a name="udr"></a>¿Se admite el enrutamiento definido por el usuario (UDR) en las subredes de Azure Bastion?

No. UDR no es compatible con las subredes de Azure Bastion.
En los escenarios que incluyen Azure Bastion y Azure Firewall/Aplicación virtual de red (NVA) en la misma red virtual, no es preciso forzar el tráfico de una subred de Azure Bastion a Azure Firewall, ya que la comunicación entre Azure Bastion y las máquinas virtuales es privada. Para más información, consulte [Acceso a las máquinas virtuales que están detrás de Azure Firewall con Bastion](https://azure.microsoft.com/blog/accessing-virtual-machines-behind-azure-firewall-with-azure-bastion/).

### <a name="is-file-transfer-supported-with-azure-bastion-rdp-session"></a><a name="filetransfer"></a>¿Se admite la transferencia de archivos con la sesión RDP de Azure Bastion?

Estamos trabajando para agregar nuevas características. Actualmente, no se admite la transferencia de archivos, pero forma parte de nuestro mapa de ruta. No dude en compartir sus comentarios sobre las nuevas características en la [página de comentarios de Azure Bastion](https://feedback.azure.com/forums/217313-networking?category_id=367303).

### <a name="how-do-i-handle-deployment-failures"></a><a name="udr"></a>¿Cómo controlo los errores de implementación?

Revise los mensajes de error y [genere una solicitud de soporte técnico en Azure portal](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) según sea necesario. Los errores de implementación pueden estar motivados por [límites, cuotas y restricciones de la suscripción de Azure](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits). Más concretamente, los clientes pueden encontrar un límite en cuanto al número de direcciones IP públicas que se permiten por suscripción y que provoca un error en la implementación de Azure Bastion.
