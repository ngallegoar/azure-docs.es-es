---
title: Habilitación de Azure Disk Encryption para VM Linux
description: En este artículo se proporcionan instrucciones sobre cómo habilitar Microsoft Azure Disk Encryption para VM Linux.
author: msmbaldwin
ms.service: virtual-machines-linux
ms.subservice: security
ms.topic: article
ms.author: mbaldwin
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: ff1b37c3053ffa91dcb432cd97a7dd6fd71dad1c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79226840"
---
# <a name="azure-disk-encryption-for-linux-vms"></a>Azure Disk Encryption para VM Linux 

Azure Disk Encryption ayuda a custodiar y proteger sus datos con el fin de satisfacer los compromisos de cumplimiento y seguridad de su organización. Usa la característica [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) de Linux para proporcionar cifrado de volumen tanto a los discos de datos como a los del sistema operativo de máquinas virtuales (VM) de Azure y se integra con [Azure Key Vault](../../key-vault/index.yml) para ayudarle a controlar y administrar las claves y los secretos del cifrado de disco. 

Si utiliza [Azure Security Center](../../security-center/index.yml), se le alertará si tiene VM que no estén cifradas. Estas alertas se muestran con gravedad alta y se recomienda cifrar estas máquinas virtuales.

![Alerta de cifrado de discos en Azure Security Center](media/disk-encryption/security-center-disk-encryption-fig1.png)

> [!WARNING]
> - Si ya ha usado Azure Disk Encryption con Azure AD para cifrar una VM, debe seguir usando esta opción para cifrar la VM. Para más información, consulte [Azure Disk Encryption con Azure AD (versión anterior)](disk-encryption-overview-aad.md). 
> - Algunas de las recomendaciones pueden provocar un aumento del uso de datos, de la red o de recursos de proceso, lo que incrementará los costes de las licencias o suscripciones. Para crear recursos en Azure en las regiones admitidas, debe tener una suscripción válida de Azure activa.
> - Actualmente, las máquinas virtuales de generación 2 no admiten Azure Disk Encryption. Vea [Compatibilidad para máquinas virtuales de generación 2 en Azure](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2) para obtener más información.

Para obtener información sobre los aspectos básicos de Azure Disk Encryption para Linux en unos minutos, consulte [Inicio rápido: Creación y cifrado de una VM Linux con la CLI de Azure](disk-encryption-cli-quickstart.md) o [Inicio rápido: Creación y cifrado de una VM Linux con Azure Powershell](disk-encryption-powershell-quickstart.md).

## <a name="supported-vms-and-operating-systems"></a>VM y sistemas operativos compatibles

### <a name="supported-vm-sizes"></a>Tamaños de máquinas virtuales que se admiten

Las VM Linux están disponibles en una [variedad de tamaños](sizes.md). Azure Disk Encryption no está disponible en [VM básicas o de serie A](https://azure.microsoft.com/pricing/details/virtual-machines/series/) ni en las máquinas virtuales que no cumplen estos requisitos mínimos de memoria:

| Máquina virtual | Requisito mínimo de memoria |
|--|--|
| VM de Linux cuando solo se cifran volúmenes de datos| 2 GB |
| VM de Linux cuando se cifran volúmenes de datos y del SO, y donde el uso del sistema de archivos raíz (/) es 4 GB o menos | 8 GB |
| VM de Linux cuando se cifran volúmenes de datos y del SO, y donde el uso del sistema de archivos raíz (/) es mayor que 4 GB | El uso del sistema de archivos raíz * 2. Por ejemplo, un uso de 16 GB del sistema de archivos raíz requiere al menos 32 GB de RAM |

Una vez completado el proceso de cifrado de disco del sistema operativo en las máquinas virtuales de Linux, la VM se puede configurar para que se ejecute con menos memoria. 

Azure Disk Encryption también está disponible para las VM con almacenamiento Premium. 

### <a name="supported-operating-systems"></a>Sistemas operativos admitidos

Azure Disk Encryption se admite en un subconjunto de las [distribuciones de Linux aprobadas por Azure](endorsed-distros.md), que es en sí mismo un subconjunto de todas las posibles distribuciones de servidores Linux.

![Diagrama de Venn de distribuciones de servidores Linux que admiten Azure Disk Encryption](./media/disk-encryption/ade-supported-distros.png)

Las distribuciones de servidores Linux que no están aprobadas por Azure no admiten Azure Disk Encryption; de las que están aprobadas, solo las siguientes distribuciones y versiones admiten Azure Disk Encryption:

| Distribución de Linux | Versión | Tipo de volumen admitido para el cifrado|
| --- | --- |--- |
| Ubuntu | 18,04| Sistema operativo y disco de datos |
| Ubuntu | 16.04| Sistema operativo y disco de datos |
| Ubuntu | 14.04.5</br>[con kernel optimizado para Azure 4.15 o posterior](disk-encryption-troubleshooting.md) | Sistema operativo y disco de datos |
| RHEL | 7,7 | Sistema operativo y disco de datos (véase la nota siguiente) |
| RHEL | 7.6 | Sistema operativo y disco de datos (véase la nota siguiente) |
| RHEL | 7.5 | Sistema operativo y disco de datos (véase la nota siguiente) |
| RHEL | 7.4 | Sistema operativo y disco de datos (véase la nota siguiente) |
| RHEL | 7.3 | Sistema operativo y disco de datos (véase la nota siguiente) |
| RHEL | 7.2 | Sistema operativo y disco de datos (véase la nota siguiente) |
| RHEL | 6,8 | Disco de datos (véase la nota siguiente) |
| RHEL | 6.7 | Disco de datos (véase la nota siguiente) |
| CentOS | 7,7 | Sistema operativo y disco de datos |
| CentOS | 7.6 | Sistema operativo y disco de datos |
| CentOS | 7.5 | Sistema operativo y disco de datos |
| CentOS | 7.4 | Sistema operativo y disco de datos |
| CentOS | 7.3 | Sistema operativo y disco de datos |
| CentOS | 7.2n | Sistema operativo y disco de datos |
| CentOS | 6,8 | Disco de datos |
| openSUSE | 42.3 | Disco de datos |
| SLES | 12-SP4 | Disco de datos |
| SLES | 12-SP3 | Disco de datos |

> [!NOTE]
> La nueva implementación de Azure Disk Encryption se admite para el sistema operativo RHEL y el disco de datos para las imágenes de Pago por uso de RHEL7.  
>
> ADE también es compatible con las imágenes Gold de tipo Traiga su propia suscripción de RHEL, pero solo **después** de registrar la suscripción. Para más información, consulte [Imágenes Gold de tipo Traiga su propia suscripción (BYOS) de Red Hat Enterprise Linux en Azure](../workloads/redhat/byos.md#encrypt-red-hat-enterprise-linux-bring-your-own-subscription-gold-images).

## <a name="additional-vm-requirements"></a>Requisitos adicionales de VM

Azure Disk Encryption requiere que los módulos dm-crypt y vfat estén presentes en el sistema. Quitar o deshabilitar vfat desde la imagen predeterminada impedirá que el sistema lea el volumen clave y obtenga la clave necesaria para desbloquear los discos en los reinicios posteriores. Los pasos de protección del sistema que quitan el módulo vfat del sistema no se admiten en Azure Disk Encryption. 

Antes de habilitar el cifrado, los discos de datos que se van a cifrar deben aparecer correctamente en /etc/fstab. Use un nombre de dispositivo de bloqueo persistente para esta entrada, ya que los nombres de dispositivo con el formato "/ dev/sdX" no son confiables para asociarlos al mismo disco en los distintos reinicios, especialmente después de que se aplica el cifrado. Para obtener más información sobre este comportamiento, consulte: [Solución de problemas: se cambian los nombres de dispositivo de máquinas virtuales Linux](troubleshoot-device-names-problems.md)

Asegúrese de que el valor /etc/fstab está configurado correctamente para el montaje. Para configurar estos valores, ejecute el comando mount -a o reinicie la máquina virtual y desencadene el nuevo montaje de ese modo. Cuando haya finalizado, revise la salida del comando lsblk para comprobar que la unidad sigue montada. 
- Si el archivo/etc/fstab no monta la unidad correctamente antes de habilitar el cifrado, Azure Disk Encryption no podrá montarla correctamente.
- El proceso de Azure Disk Encryption sacará la información de montaje de/etc/fstab y la colocará en su propio archivo de configuración como parte del proceso de cifrado. No se alarme si ve que falta la entrada de/etc/fstab después de que se completa el cifrado de la unidad de datos.
- Antes de iniciar el cifrado, asegúrese de detener todos los servicios y procesos que se podrían estar escribiendo en los discos de datos montados y deshabilítelos de modo que no se reinicien automáticamente tras un reinicio. Estos podrían mantener los archivos abiertos en estas particiones, lo que impediría que el procedimiento de cifrado los vuelva a montar, provocando un error de cifrado. 
- Después del reinicio, el proceso de Azure Disk Encryption tardará un tiempo en montar los discos recién cifrados. No estarán disponibles inmediatamente después de un reinicio. El proceso necesita tiempo para iniciar, desbloquear y montar las unidades cifradas antes de que estén disponibles para que otros procesos tengan acceso a ellas. Este proceso puede tardar más de un minuto después del reinicio, dependiendo de las características del sistema.

Un ejemplo de comando que se puede usar para montar los discos de datos y crear las entradas necesarias de /etc/fstab se puede encontrar en el [script de la CLI de requisitos previos de Azure Disk Encryption](https://github.com/ejarvi/ade-cli-getting-started) (líneas 244 a 248) y el [script de PowerShell de requisitos previos de Azure Disk Encryption](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts). 

## <a name="networking-requirements"></a>Requisitos de red

Para habilitar la característica Azure Disk Encryption, las VM Linux deben cumplir los siguientes requisitos de configuración de puntos de conexión de red:
  - Para que un token se conecte al almacén de claves, la VM Linux debe poder conectarse a un punto de conexión de Azure Active Directory, \[login.microsoftonline.com\].
  - Para escribir las claves de cifrado en el almacén de claves, la VM Linux debe poder conectarse al punto de conexión del almacén de claves.
  - La VM Linux debe poder conectarse al punto de conexión de Azure Storage que hospeda el repositorio de extensiones de Azure y la cuenta de Azure Storage que hospeda los archivos VHD.
  -  Si su directiva de seguridad limita el acceso desde máquinas virtuales de Azure a Internet, puede resolver el URI anterior y configurar una regla concreta para permitir la conectividad de salida para las direcciones IP. Para más información, consulte [Azure Key Vault detrás de un firewall](../../key-vault/key-vault-access-behind-firewall.md).  

## <a name="encryption-key-storage-requirements"></a>Requisitos de almacenamiento de la clave de cifrado  

Azure Disk Encryption requiere Azure Key Vault para controlar y administrar las claves y los secretos de cifrado de discos. El almacén de claves y las máquinas virtuales deben residir en la misma región y suscripción de Azure.

Para obtener más información, consulte [Creación y configuración de un almacén de claves para Azure Disk Encryption](disk-encryption-key-vault.md).

## <a name="terminology"></a>Terminología
En la siguiente tabla se definen algunos de los términos comunes que se usan en la documentación de cifrado de disco de Azure:

| Terminología | Definición |
| --- | --- |
| Azure Key Vault | Key Vault es un servicio de administración de claves criptográficas basado en módulos de seguridad de hardware validados por el Estándar federal de procesamiento de información (FIPS). Estos estándares ayudan a proteger las claves criptográficas y los secretos confidenciales. Para obtener más información, consulte la documentación de [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) y [Creación y configuración de un almacén de claves para Azure Disk Encryption](disk-encryption-key-vault.md). |
| Azure CLI | [La CLI de Azure](/cli/azure/install-azure-cli) está optimizada para administrar recursos de Azure desde la línea de comandos.|
| DM-Crypt |[DM-Crypt](https://gitlab.com/cryptsetup/cryptsetup/wikis/DMCrypt) es el subsistema de cifrado transparente de disco basado en Linux que se usa para habilitar el cifrado de disco en las VM Linux. |
| Clave de cifrado de claves (KEK) | La clave asimétrica (RSA 2048) que puede usar para proteger o encapsular el secreto. Puede proporcionar una clave protegida mediante módulos de seguridad de hardware (HSM) o una clave protegida mediante software. Para obtener más información, consulte la documentación de [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) y [Creación y configuración de un almacén de claves para Azure Disk Encryption](disk-encryption-key-vault.md). |
| Cmdlets de PowerShell | Para más información, consulte [Cmdlets de Azure PowerShell](/powershell/azure/overview). |


## <a name="next-steps"></a>Pasos siguientes

- [Inicio rápido: Creación y cifrado de una VM Linux con la CLI de Azure](disk-encryption-cli-quickstart.md)
- [Inicio rápido: Creación y cifrado de una VM Linux con Azure PowerShell](disk-encryption-powershell-quickstart.md)
- [Escenarios de Azure Disk Encryption en VM Linux](disk-encryption-linux.md)
- [Script de la CLI de requisitos previos de Azure Disk Encryption](https://github.com/ejarvi/ade-cli-getting-started)
- [Script de PowerShell de requisitos previos de Azure Disk Encryption](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts)
- [Creación y configuración de un almacén de claves para Azure Disk Encryption](disk-encryption-key-vault.md)


