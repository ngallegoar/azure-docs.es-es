---
title: Aplicación del cifrado de discos en Azure Security Center | Microsoft Docs
description: En este documento se muestra cómo implementar la recomendación **Aplicar cifrado de discos** de Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 6cc7824a-8d6b-4a5f-ab40-e3bbaebc4a91
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: memildin
ms.openlocfilehash: d0f96fe758966a435f8fb8e448e75cbb18b85122
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77604514"
---
# <a name="apply-disk-encryption-in-azure-security-center"></a>Aplicación del cifrado de discos en el Centro de seguridad de Azure
Azure Security Center recomienda que aplique el cifrado de discos si tiene discos de máquina virtual de Windows o Linux que no estén cifrados con Azure Disk Encryption. El Cifrado de discos permite cifrar los discos de máquina virtual IaaS de Windows y Linux.  Se recomienda cifrar tanto los volúmenes de datos como los del sistema operativo en la máquina virtual.

Disk Encryption usa la característica [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) estándar del sector de Windows y la característica [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) de Linux. Estas características proporcionan cifrado de datos y del sistema operativo para proteger sus datos y asumir las responsabilidades de cumplimiento y seguridad de la organización. La solución se integra con [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) para ayudarle a controlar y administrar las claves y los secretos de cifrado de discos en su suscripción de Key Vault, al mismo tiempo que garantiza que todos los datos de los discos de las máquinas virtuales se cifran en reposo en [Azure Storage](https://azure.microsoft.com/documentation/services/storage/).

> [!NOTE]
> El Azure Disk Encryption es compatible con los siguientes sistemas operativos de Windows Server: Windows Server 2008 R2, Windows Server 2012 y Windows Server 2012 R2. El cifrado de discos es compatible con los siguientes sistemas operativos de Linux Server: Ubuntu, CentOS, SUSE y SUSE Linux Enterprise Server (SLES).
>
>

## <a name="implement-the-recommendation"></a>Implementación de la recomendación
1. En la hoja **Recomendaciones**, seleccione **Aplicar cifrado de discos**.
2. En la hoja **Aplicar cifrado de discos**, verá una lista de las máquinas virtuales para las que se recomienda el cifrado de discos.
3. Siga las instrucciones para aplicar el cifrado a estas máquinas virtuales.

![][1]

Para cifrar Azure Virtual Machines que Security Center ha identificado que necesitan cifrado, recomendamos que siga los siguientes pasos:

* Instale y configure Azure PowerShell. Esto permite ejecutar los comandos de PowerShell para configurar los requisitos previos necesarios para cifrar Azure Virtual Machines.
* Obtenga y ejecute el script de Azure PowerShell de requisitos previos de Azure Disk Encryption.
* Cifre las máquinas virtuales.

[Cifrado de una máquina virtual IaaS Windows con Azure PowerShell](../virtual-machines/windows/disk-encryption-powershell-quickstart.md) le guiará en estos pasos. En este tema se supone que utiliza una máquina cliente Windows desde la que configura el cifrado de disco.

Existen varios enfoques que se pueden usar para Azure Virtual Machines. Si conoce bien Azure PowerShell o CLI de Azure, puede preferir el uso de métodos alternativos. Para más información sobre estos otros enfoques, consulte [Azure Disk Encryption](../security/fundamentals/encryption-overview.md).

## <a name="see-also"></a>Consulte también
En este documento, mostramos cómo implementar la recomendación "Aplicar el cifrado de discos" del Centro de seguridad. Para más información acerca del cifrado de discos, consulte lo siguiente:

* [Encryption and key management with Azure Key Vault](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (Cifrado y administración de claves con el Almacén de claves de Azure) (vídeo, 36 m y 39 s): aprenda a usar la administración del cifrado de discos para máquinas virtuales IaaS y el Almacén de claves de Azure para ayudar a proteger sus datos.
* [Cifrado de discos de Azure](../security/fundamentals/encryption-overview.md) (documento): aprenda a habilitar el cifrado de discos para máquinas virtuales de Windows y Linux.

Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md) : aprenda a configurar directivas de seguridad.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): obtenga información sobre cómo supervisar el mantenimiento de los recursos de Azure.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md) : obtenga información sobre cómo administrar y responder a alertas de seguridad.
* [Administración de recomendaciones de seguridad en Azure Security Center](security-center-recommendations.md): recomendaciones que le ayudan a proteger los recursos de Azure.
* [Blog de seguridad de Azure](https://blogs.msdn.com/b/azuresecurity/) : encuentre publicaciones de blog sobre el cumplimiento y la seguridad de Azure.

<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png
