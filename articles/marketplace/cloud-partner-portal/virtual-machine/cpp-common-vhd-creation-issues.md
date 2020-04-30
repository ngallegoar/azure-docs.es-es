---
title: Problemas comunes durante la creación de discos duros virtuales (P+F) para Azure Marketplace
description: Preguntas más frecuentes sobre la creación de discos duros virtuales y problemas asociados.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: dsindona
ms.openlocfilehash: df219ad7428eed5283c7a30a232308dca84fd25f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82146985"
---
# <a name="common-issues-during-vhd-creation-faq"></a>Problemas comunes durante la creación de discos duros virtuales (P+F)

> [!IMPORTANT]
> A partir del 13 de abril de 2020, comenzaremos el traslado de la administración de las ofertas de máquinas virtuales de Azure al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Preguntas recuentes sobre la creación de discos duros virtuales](https://docs.microsoft.com/azure/marketplace/partner-center-portal/common-issues-during-vhd-creation) para administrar las ofertas migradas.

Las preguntas más frecuentes (P+F) siguientes abarcan problemas comunes detectados durante la creación del disco duro virtual (VHD) y la máquina virtual (VM) para las ofertas de máquina virtual. 

## <a name="how-do-you-create-a-vm-from-the-azure-portal-using-the-vhd-that-is-uploaded-to-premium-storage"></a>¿Cómo se crea una máquina virtual desde Azure Portal mediante el disco duro virtual que se carga en Premium Storage?

En la actualidad, Azure Marketplace no admite la creación de ofertas de máquina virtual a partir de imágenes que residen en el almacenamiento administrado o desde Azure Premium Storage.  Para más información sobre estas opciones de almacenamiento, consulte [Introducción a Azure Managed Disks](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview).


## <a name="can-you-use-generation-2-vms-for-offers"></a>¿Se pueden usar máquinas virtuales de generación 2 para las ofertas?

No, solo se admiten los discos duros virtuales de generación 1.  Pero en la actualidad estamos trabajando con el equipo de la plataforma de Microsoft Azure para investigar la compatibilidad con las máquinas virtuales de generación 2.  Para obtener más información sobre las diferencias, vea [Should I create a generation 1 or 2 virtual machine in Hyper-V?](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v) (¿Debería crear una máquina virtual de generación 1 o 2 en Hyper-V?).


## <a name="how-do-you-change-the-name-of-the-host"></a>¿Cómo se cambia el nombre del host?

No se puede.  Una vez creada la máquina virtual, los usuarios (incluidos los propietarios) no pueden actualizar el nombre del host.


## <a name="how-do-you-reset-the-remote-desktop-service-or-its-sign-in-password"></a>¿Cómo se restablece el servicio Escritorio remoto o su contraseña de inicio de sesión?

En los artículos siguientes se explica cómo realizar el restablecimiento de RDS para máquinas virtuales basadas en Windows y Linux:   

- [Restablecimiento del servicio Escritorio remoto o la contraseña de inicio de sesión en una máquina virtual Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-reset-rdp/)
- [Cómo restablecer una contraseña de máquina virtual Linux o clave SSH, corregir la configuración de SSH y comprobar la coherencia de disco con la extensión VMAccess](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)


## <a name="how-do-you-generate-new-ssh-certificates"></a>¿Cómo se generan certificados SSH nuevos?

La generación de certificados se explica en el artículo [Get shared access signature URI for your VM image](./cpp-get-sas-uri.md) (Obtención del URI de firma de acceso compartido para la imagen de máquina virtual) en la sección posterior [Create technical assets for a VM offer](./cpp-create-technical-assets.md) (Creación de recursos técnicos para una oferta de máquina virtual).


## <a name="how-do-you-configure-a-virtual-private-network-vpn-to-work-with-my-vms"></a>¿Cómo se configura una red privada virtual (VPN) para trabajar con las máquinas virtuales?

Si usa el modelo de implementación de Azure Resource Manager, tiene tres opciones comunes para configurar una VPN:
- [Creación de una instancia de VPN Gateway basada en rutas mediante Azure Portal](https://docs.microsoft.com/azure/vpn-gateway/create-routebased-vpn-gateway-portal)
- [Creación de una instancia de VPN Gateway basada en rutas mediante PowerShell](https://docs.microsoft.com/azure/vpn-gateway/create-routebased-vpn-gateway-powershell)
- [Creación de una instancia de VPN Gateway basada en rutas mediante la CLI](https://docs.microsoft.com/azure/vpn-gateway/create-routebased-vpn-gateway-cli)


## <a name="what-are-microsoft-support-policies-for-running-microsoft-server-software-on-azure-based-vms"></a>¿Cuáles son las directivas de soporte técnico de Microsoft para ejecutar software de servidor de Microsoft en máquinas virtuales basadas en Azure?

Estas directivas de soporte técnico se describen en el artículo [Soporte técnico del software de servidor de Microsoft para máquinas virtuales de Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).


## <a name="do-virtual-machines-have-unique-identifiers-associated-with-them"></a>¿Las máquinas virtuales tienen identificadores únicos asociados?

Sí, si se hospedan en Azure.  Azure asigna un identificador único, denominado identificador único de máquina virtual de Azure, a cada recurso de máquina virtual que se crea.  Para obtener más información, lea la entrada de blog [Azure Virtual Machine Unique ID](https://blogs.msdn.microsoft.com/wasimbloch/2016/10/20/azure-virtual-machine-unique-id/) (Identificador único de máquina virtual de Azure).  También puede obtener este identificador mediante programación a través de [List API](https://docs.microsoft.com/rest/api/compute/virtualmachines/list).


## <a name="in-a-vm-how-do-you-manage-the-custom-script-extension-in-the-startup-task"></a>En una máquina virtual, ¿cómo se puede administrar la extensión de script personalizada en la tarea de inicio?

En el artículo siguiente se detalla cómo usar la extensión de script personalizado mediante el módulo de Azure PowerShell y plantillas de Azure Resource Manager, y se detallan también los pasos para solucionar problemas en los sistemas Windows: [Extensión de la secuencia de comandos personalizada para Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/)


## <a name="are-32-bit-applications-or-services-supported-in-the-azure-marketplace"></a>¿Se admiten en Azure Marketplace las aplicaciones o los servicios de 32 bits?

En general, no.  Todos los sistemas operativos compatibles y los servicios estándar para las máquinas virtuales de Azure son de 64 bits.  Pero desde un punto de vista técnico, la mayoría de los sistemas operativos de 64 bits admite la ejecución de las versiones de 32 bits de las aplicaciones para la compatibilidad con versiones anteriores.  Pero el uso de aplicaciones de 32 bits como parte de la solución de máquina virtual no se admite y, por tanto, se *desaconseja encarecidamente*.  En su lugar, vuelva a compilar la aplicación como un proyecto de 64 bits.

Para más información, consulte los siguientes artículos.
- [Running 32-bit applications](https://docs.microsoft.com/windows/desktop/WinProg64/running-32-bit-applications) (Ejecución de aplicaciones de 32 bits)
- [Soporte para sistemas operativos de 32 bits en máquinas virtuales de Azure](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines)
- [Soporte técnico del software de servidor de Microsoft para máquinas virtuales de Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)


## <a name="every-time-i-try-to-create-an-image-from-my-vhds-i-get-the-error-vhd-is-already-registered-with-image-repository-as-the-resource-in-powershell-i-did-not-create-any-image-before-nor-did-i-find-any-image-with-this-name-in-azure-how-do-i-resolve-this-issue"></a>Cada vez que intento crear una imagen a partir de los discos duros virtuales, se produce el error `.VHD is already registered with image repository as the resource` en PowerShell. No he creado ninguna imagen antes ni he encontrado ninguna imagen con este nombre en Azure. ¿Cómo se resuelve este problema?

Este problema suele producirse si el usuario ha aprovisionado una máquina virtual desde un disco duro virtual que tiene un bloqueo.  Compruebe que no hay ninguna máquina virtual asignada desde este disco duro virtual y, después, vuelva a intentar la operación.  Si el problema persiste, abra una incidencia de soporte técnico, como se explica en [Soporte técnico para Cloud Partner Portal](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-support-for-cloud-partner-portal). 

