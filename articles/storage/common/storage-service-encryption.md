---
title: Cifrado de Azure Storage para datos en reposo
description: Azure Storage protege los datos cifrándolos automáticamente antes de guardarlos en la nube. Puede confiar en las claves administradas por Microsoft para el cifrado de los datos de la cuenta de almacenamiento, o puede administrar el cifrado con sus propias claves.
services: storage
author: tamram
ms.service: storage
ms.date: 04/10/2020
ms.topic: conceptual
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: c737ccf83dae0cc4b198b9cd708a55b988e6593b
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457950"
---
# <a name="azure-storage-encryption-for-data-at-rest"></a>Cifrado de Azure Storage para datos en reposo

Azure Storage cifra automáticamente los datos al guardarlos en la nube. El cifrado de Azure Storage protege los datos y le ayuda a satisfacer los requisitos de cumplimiento normativo y seguridad de la organización.

## <a name="about-azure-storage-encryption"></a>Acerca del cifrado de Azure Storage

Los datos de Azure Storage se cifran y descifran de forma transparente mediante el [cifrado AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) de 256 bits, uno de los cifrados de bloques más sólidos que hay disponibles, y son compatibles con FIPS 140-2. El cifrado de Azure Storage es similar al cifrado de BitLocker en Windows.

El cifrado de Azure Storage está habilitado para todas las cuentas de almacenamiento, incluidas las cuentas de almacenamiento clásicas y las de Resource Manager. El cifrado de Azure Storage no se puede deshabilitar. Como los datos están protegidos de forma predeterminada, no es necesario modificar el código o las aplicaciones para aprovechar el cifrado de Azure Storage.

Los datos de una cuenta de almacenamiento se cifran independientemente de su nivel de rendimiento (Estándar o Premium), del nivel de acceso (frecuente o esporádico) o del modelo de implementación (Azure Resource Manager o clásico). También se cifran todos los blobs del nivel de archivo. Todas las opciones de redundancia de Azure Storage admiten el cifrado, y todos los datos de las regiones primaria y secundaria se cifran cuando la replicación geográfica está habilitada. Se cifran todos los recursos de Azure Storage, incluidos los blobs, los discos, los archivos, las colas y las tablas. También se cifran todos los metadatos de objetos. No hay ningún costo adicional para el cifrado de Azure Storage.

Todos los blobs en bloques, blobs en anexos o blobs en páginas que se escribieron en Azure Storage después del 20 de octubre de 2017 está cifrados. Los blobs creados antes de esta fecha se siguen cifrando mediante un proceso en segundo plano. Para forzar el cifrado de un blob creado antes del 20 de octubre de 2017, puede volver a escribir el blob. Para más información sobre cómo comprobar el estado de cifrado de un blob, consulte [Comprobación del estado de cifrado de un blob](../blobs/storage-blob-encryption-status.md).

Para más información sobre de los módulos criptográficos subyacentes al cifrado de Azure Storage, vea [API de criptografía: última generación](https://docs.microsoft.com/windows/desktop/seccng/cng-portal).

## <a name="about-encryption-key-management"></a>Información sobre la administración de claves de cifrado

Los datos de una cuenta de almacenamiento nueva se cifran con claves administradas por Microsoft. Puede confiar en las claves administradas por Microsoft para el cifrado de los datos, o puede administrar el cifrado con sus propias claves. Si opta por administrar el cifrado con sus propias claves, tiene dos opciones:

- Puede especificar una *clave administrada por el cliente* con Azure Key Vault que se usará para cifrar y descifrar datos en el almacenamiento de blobs y en Azure Files.<sup>1,2</sup> Para más información sobre las claves administradas por el cliente, consulte [Uso de claves administradas por el cliente con Azure Key Vault para administrar el cifrado de Azure Storage](encryption-customer-managed-keys.md).
- En las operaciones de almacenamiento de blobs, puede especificar una *clave proporcionada por el cliente*. Un cliente que realiza una solicitud de lectura o escritura en el almacenamiento de blobs puede incluir una clave de cifrado en la solicitud para tener un control detallado sobre el cifrado y el descifrado de los datos de blob. Para más información sobre las claves proporcionadas por el cliente, consulte [Proporcionar una clave de cifrado en una solicitud de Blob Storage (versión preliminar)](encryption-customer-provided-keys.md).

En la tabla siguiente se comparan las opciones de administración de claves para el cifrado de Azure Storage.

|                                        |    Claves administradas por Microsoft                             |    Claves administradas por el cliente                                                                                                                        |    Claves proporcionadas por el cliente                                                          |
|----------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|    Operaciones de cifrado y descifrado    |    Azure                                              |    Azure                                                                                                                                        |    Azure                                                                         |
|    Servicios de Azure Storage admitidos    |    All                                                |    Blob Storage, Azure Files<sup>1,2</sup>                                                                                                               |    Blob Storage                                                                  |
|    Almacenamiento de claves                         |    Almacén de claves de Microsoft    |    Azure Key Vault                                                                                                                              |    Propio almacén de claves del cliente                                                                 |
|    Responsabilidad de la rotación de claves         |    Microsoft                                          |    Customer                                                                                                                                     |    Customer                                                                      |
|    Control de claves                          |    Microsoft                                     |    Customer                                                                                                                    |    Customer                                                                 |

<sup>1</sup> Para obtener información sobre cómo crear una cuenta que admita el uso de claves administradas por el cliente con Queue Storage, consulte [Creación de una cuenta que admita las claves administradas por el cliente para colas](account-encryption-key-create.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).<br />
<sup>2</sup> Para obtener información sobre cómo crear una cuenta que admita el uso de claves administradas por el cliente con Table Storage, consulte [Creación de una cuenta que admita las claves administradas por el cliente para tablas](account-encryption-key-create.md?toc=%2fazure%2fstorage%2ftables%2ftoc.json).

## <a name="next-steps"></a>Pasos siguientes

- [¿Qué es Azure Key Vault?](../../key-vault/general/overview.md)
- [Configurar las claves administradas por el cliente para el cifrado de Azure Storage desde Azure Portal](storage-encryption-keys-portal.md)
- [Configurar las claves administradas por el cliente para el cifrado de Azure Storage desde PowerShell](storage-encryption-keys-powershell.md)
- [Configuración de las claves administradas por el cliente para el cifrado de Azure Storage desde la CLI de Azure](storage-encryption-keys-cli.md)
