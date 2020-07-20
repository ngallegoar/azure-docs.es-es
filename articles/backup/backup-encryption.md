---
title: Cifrado en Azure Backup
description: Conozca las características de cifrado de Azure Backup que le ayudan a proteger los datos de copia de seguridad y a satisfacer las necesidades de seguridad de su negocio.
ms.topic: conceptual
ms.date: 04/30/2020
ms.openlocfilehash: aafb9868dfb6a63ec9b6a3ae654b88b202a1a145
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86171829"
---
# <a name="encryption-in-azure-backup"></a>Cifrado en Azure Backup

Todos los datos de copia de seguridad se cifran automáticamente cuando se almacenan en la nube mediante el cifrado de Azure Storage, lo que le ayuda a cumplir los compromisos de seguridad y cumplimiento. Los datos en reposo se cifran mediante el cifrado AES de 256 bits, uno de los cifrados de bloques más seguros que hay disponibles y que es compatible con FIPS 140-2.

Además del cifrado en reposo, todos los datos de copia de seguridad en tránsito se transfieren a través de HTTPS y permanecen siempre en la red troncal de Azure.

Para más información, consulte [Cifrado de Azure Storage para datos en reposo](https://docs.microsoft.com/azure/storage/common/storage-service-encryption). Consulte las [preguntas más frecuentes sobre Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-backup-faq#encryption) para obtener respuesta a las dudas que pueda tener sobre el cifrado.

## <a name="encryption-of-backup-data-using-platform-managed-keys"></a>Cifrado de datos de copia de seguridad mediante claves administradas por la plataforma

De forma predeterminada, todos los datos se cifran mediante claves administradas por la plataforma. No es necesario que realice ninguna acción explícita por su parte para habilitar este cifrado y se aplica a todas las cargas de trabajo de las que se realiza una copia de seguridad en el almacén de Recovery Services.

## <a name="encryption-of-backup-data-using-customer-managed-keys"></a>Cifrado de datos de copia de seguridad mediante claves administradas por el cliente

Al realizar una copia de seguridad de Azure Virtual Machines, ahora puede cifrar los datos mediante las claves que posee y administra. Azure Backup le permite usar las claves RSA almacenadas en Azure Key Vault para cifrar las copias de seguridad. La clave de cifrado utilizada para cifrar las copias de seguridad puede ser diferente de la que se usa para el origen. Los datos se protegen mediante una clave de cifrado de datos (DEK) basada en AES 256, que, a su vez, está protegida con las claves del usuario. Esto le proporciona un control total sobre los datos y las claves. Para permitir el cifrado, se requiere que el almacén de Recovery Services tenga acceso a la clave de cifrado en Azure Key Vault. Puede deshabilitar la clave o revocar el acceso siempre que sea necesario. Sin embargo, debe habilitar el cifrado con las claves antes de intentar proteger los elementos en el almacén.

Obtenga más información acerca de cómo cifrar los datos de copia de seguridad mediante claves administradas por el cliente [aquí](encryption-at-rest-with-cmk.md).

## <a name="backup-of-managed-disk-vms-encrypted-using-customer-managed-keys"></a>Copia de seguridad de las máquinas virtuales de disco administrado cifradas con claves administradas por el cliente

Azure Backup también le permite realizar copias de seguridad de las máquinas virtuales de Azure que usan la clave para el [cifrado del servicio de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-service-encryption). La clave que se usa para cifrar los discos se almacena en Azure Key Vault y la administra el usuario. Storage Service Encryption (SSE) mediante claves administradas por el cliente difiere de Azure Disk Encryption, ya que ADE aprovecha BitLocker (para Windows) y DM-Crypt (para Linux) a fin de realizar el cifrado en invitado, y SSE cifra los datos en el servicio de almacenamiento, lo que permite usar cualquier sistema operativo o imagen para las máquinas virtuales. Para obtener más información, consulte el artículo sobre [cifrado de discos administrados con claves administradas por el cliente](https://docs.microsoft.com/azure/virtual-machines/windows/disk-encryption#customer-managed-keys).

## <a name="infrastructure-level-encryption-for-backup-data"></a>Cifrado de nivel de infraestructura para datos de copia de seguridad

Además de cifrar los datos en el almacén de Recovery Services mediante claves administradas por el cliente, también puede elegir tener una capa adicional de cifrado configurada en la infraestructura de almacenamiento. La plataforma administra el cifrado de la infraestructura y, junto con el cifrado en reposo mediante claves administradas por el cliente, permite el cifrado de dos niveles de los datos de copia de seguridad. Debe tenerse en cuenta que el cifrado de infraestructura solo se puede configurar si elige usar sus propias claves para el cifrado en reposo. El cifrado de infraestructura usa claves administradas por la plataforma para cifrar datos.

>[!NOTE]
>El cifrado de infraestructura se encuentra actualmente en versión preliminar limitada y solo está disponible en las regiones Este de EE. UU., Oeste de EE. UU. 2, Centro y Sur de EE. UU. y US Gov Arizona. Si desea usar la característica en cualquiera de estas regiones, rellene [este formulario](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0H3_nezt2RNkpBCUTbWEapUN0VHNEpJS0ZUWklUNVdJSTEzR0hIOVRMVC4u) y envíenos un correo electrónico a [AskAzureBackupTeam@microsoft.com](mailto:AskAzureBackupTeam@microsoft.com).

## <a name="backup-of-vms-encrypted-using-ade"></a>Copia de seguridad de máquinas virtuales cifradas mediante ADE

Con Azure Backup, también puede hacer una copia de seguridad de las máquinas virtuales de Azure que tienen el sistema operativo o los discos de datos cifrados mediante Azure Disk Encryption. ADE usa BitLocker para las VM Windows y DM-Crypt para las VM Linux a fin de realizar el cifrado de invitado. Para obtener instrucciones, consulte [Copia de seguridad y restauración de máquinas virtuales cifradas con Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption).

## <a name="next-steps"></a>Pasos siguientes

- [Copia de seguridad y restauración de máquinas virtuales de Azure cifradas](backup-azure-vms-encryption.md)
