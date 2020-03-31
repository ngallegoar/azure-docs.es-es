---
title: Referencias de almacén de máquina virtual admitidas en Azure Policy
description: En este artículo se describen las SKU de máquina virtual admitidas (por publicador, oferta de imagen y SKU de imagen) que se admiten para las directivas de Azure integradas que proporciona Azure Backup.
ms.topic: conceptual
ms.date: 11/08/2019
ms.openlocfilehash: 1b6a94b0f57ecfea946654c6cae38ac335335e00
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74979941"
---
# <a name="supported-vm-skus-for-azure-policy"></a>Referencias de almacén de máquina virtual admitidas en Azure Policy

Azure Backup proporciona una directiva integrada (mediante Azure Policy) que se puede asignar a **todas las máquinas virtuales de Azure de una ubicación especificada dentro de una suscripción o un grupo de recursos**. Cuando esta directiva se asigna a un ámbito determinado, todas las máquinas virtuales nuevas creadas en ese ámbito se configuran automáticamente para que se cree su copia de seguridad en un **almacén existente en la misma ubicación y suscripción**. En la tabla siguiente se enumeran todas las SKU de máquina virtual admitidas por esta directiva.

### <a name="supported-vms"></a>**Máquinas virtuales admitidas**

**Nombre de la directiva:** Configuración de copias de seguridad de las máquinas virtuales de una ubicación en un almacén central existente en la misma ubicación

Editor de la imagen | Oferta de la imagen | SKU de la imagen
--- | --- | ---
Microsoft Windows Server | Windows Server | Windows Server 2008 R2 SP1 (2008-R2-SP1)
Microsoft Windows Server | Windows Server | [smalldisk] Windows Server 2008 R2 SP1 (2008-R2-SP1-smalldisk)
Microsoft Windows Server | Windows Server | Windows Server 2012 Datacenter (2012-Datacenter)
Microsoft Windows Server | Windows Server | [smalldisk] Windows Server 2012 Datacenter (2012-Datacenter-smalldisk)
Microsoft Windows Server | Windows Server | Windows Server 2012 R2 Datacenter (2012-R2-Datacenter)
Microsoft Windows Server | Windows Server | [smalldisk] Windows Server 2012 R2 Datacenter (2012-R2-Datacenter-smalldisk)
Microsoft Windows Server | Windows Server | Windows Server 2016 Datacenter (2016-Datacenter)
Microsoft Windows Server | Windows Server | Windows Server 2016 Datacenter: Server Core (2016-Datacenter-Server-Core)
Microsoft Windows Server | Windows Server | [smalldisk] Windows Server 2016 Datacenter: Server Core (2016-Datacenter-Server-Core-smalldisk)
Microsoft Windows Server | Windows Server | [smalldisk] Windows Server 2016 Datacenter (2016-Datacenter-smalldisk)
Microsoft Windows Server | Windows Server | Windows Server 2019 Datacenter Server Core con contenedores (2016-Datacenter-with-Containers)
Microsoft Windows Server | Windows Server | Windows Server 2016 Host de sesión de Escritorio remoto 2016 (2016-Datacenter-with-RDSH)
Microsoft Windows Server | Windows Server | Windows Server 2019 Datacenter (2019-Datacenter)
Microsoft Windows Server | Windows Server | Windows Server 2019 Datacenter: Server Core (2019-Datacenter-Core)
Microsoft Windows Server | Windows Server | [smalldisk] Windows Server 2019 Datacenter Server Core (2019-Datacenter-Core-smalldisk)
Microsoft Windows Server | Windows Server | Windows Server 2019 Datacenter Server Core con contenedores (2019-Datacenter-Core-with-Containers)
Microsoft Windows Server | Windows Server | [smalldisk] Windows Server 2019 Datacenter Server Core con contenedores (2019-Datacenter-Core-with-Containers-smalldisk)
Microsoft Windows Server | Windows Server | [smalldisk] Windows Server 2019 Datacenter (2019-Datacenter-smalldisk)
Microsoft Windows Server | Windows Server | Windows Server 2019 Datacenter con contenedores (2019-Datacenter-with-Containers)
Microsoft Windows Server | Windows Server | [smalldisk] Windows Server 2019 Datacenter con contenedores (2019-Datacenter-with-Containers-smalldisk)
Microsoft Windows Server | Windows Server | Windows Server 2019 Datacenter (zh-cn) (2019-Datacenter-zhcn)
Microsoft Windows Server | WindowsServerSemiAnnual | Datacenter-Core-1709-smalldisk
Microsoft Windows Server | WindowsServerSemiAnnual | Datacenter-Core-1709-with-Containers-smalldisk
Microsoft Windows Server | WindowsServerSemiAnnual | Datacenter-Core-1803-with-Containers-smalldisk
MicrosoftWindowsServerHPCPack | WindowsServerHPCPack | Todas las SKU de imagen
MicrosoftSQLServer | SQL2016SP1-WS2016 | Todas las SKU de imagen
MicrosoftSQLServer | SQL2016-WS2016  | Todas las SKU de imagen
MicrosoftSQLServer | SQL2016SP1-WS2016-BYOL | Todas las SKU de imagen
MicrosoftSQLServer | SQL2012SP3-WS2012R2 | Todas las SKU de imagen
MicrosoftSQLServer | SQL2016-WS2012R2 | Todas las SKU de imagen
MicrosoftSQLServer | SQL2014SP2-WS2012R2 | Todas las SKU de imagen
MicrosoftSQLServer | SQL2012SP3-WS2012R2-BYOL | Todas las SKU de imagen
MicrosoftSQLServer | SQL2014SP1-WS2012R2-BYOL | Todas las SKU de imagen
MicrosoftSQLServer | SQL2014SP2-WS2012R2-BYOL | Todas las SKU de imagen
MicrosoftSQLServer | SQL2016-WS2012R2-BYOL | Todas las SKU de imagen
MicrosoftRServer | MLServer-WS2016 | Todas las SKU de imagen
MicrosoftVisualStudio | VisualStudio | Todas las SKU de imagen
MicrosoftVisualStudio | Windows | Todas las SKU de imagen
MicrosoftDynamicsAX | Dynamics  | Pre-Req-AX7-Onebox-U8
microsoft-ads | windows-data-science-vm | Todas las SKU de imagen
MicrosoftWindowsDesktop | Windows-10 | Todas las SKU de imagen
RedHat | RHEL | 6.7, 6.8, 6.9, 6.10, 7.2, 7.3, 7.4, 7.5, 7.6, 7.7
RedHat | RHEL-SAP-HANA | 6.7, 7.2, 7.3
SUSE | SLES | 12.X
SUSE | SLES-HPC | 12.X
SUSE | SLES-HPC-Priority | 12.X
SUSE | SLES-SAP | 12.X
SUSE | SLES-SAP-BYOS | 12.X
SUSE | SLES-Priority | 12.X
SUSE | SLES-BYOS | 12.X
SUSE | SLES-SAPCAL | 12.X
SUSE | SLES-Standard | 12.X
Canonical | UbuntuServer | 14.04.0-LTS
Canonical | UbuntuServer | 14.04.1-LTS
Canonical | UbuntuServer | 14.04.2-LTS
Canonical | UbuntuServer | 14.04.3-LTS
Canonical | UbuntuServer | 14.04.4-LTS
Canonical | UbuntuServer | 14.04.5-DAILY-LTS
Canonical | UbuntuServer | 14.04.5-LTS
Canonical | UbuntuServer | 16.04-DAILY-LTS
Canonical | UbuntuServer | 16.04-LTS
Canonical | UbuntuServer | 16.04.0-LTS
Canonical | UbuntuServer | 18.04-DAILY-LTS
Canonical | UbuntuServer | 18.04-LTS
Oracle | Oracle-Linux | 6.8, 6.9, 6.10, 7.3, 7.4, 7.5, 7.6
OpenLogic | CentOS | 6.X, 7.X
OpenLogic | CentOS–LVM | 6.X, 7.X
OpenLogic | CentOS–SRIOV | 6.X, 7.X
cloudera | cloudera-centos-os | 7.X
