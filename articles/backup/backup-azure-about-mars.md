---
title: Acerca del agente de MARS
description: Obtenga información sobre cómo el agente de MARS admite los escenarios de copia de seguridad
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: d2cc8e32152f6930c9c250e2811668cc2c924616
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78673286"
---
# <a name="about-the-microsoft-azure-recovery-services-mars-agent"></a>Acerca del agente de Microsoft Azure Recovery Services (MARS)

En este artículo se describe de qué manera el servicio Azure Backup usa el agente de Microsoft Azure Recovery Services (MARS) para crear copias de seguridad y restaurar archivos y carpetas, así como el volumen o el estado del sistema desde un equipo local en Azure.

El agente de MARS admite los escenarios de copia de seguridad siguientes:

![Escenarios de copia de seguridad de MARS](./media/backup-try-azure-backup-in-10-mins/backup-scenarios.png)

- **Archivos y carpetas**: proteja selectivamente archivos y carpetas de Windows.
- **Nivel de volumen**: proteja todo el volumen de Windows de la máquina.
- **Nivel del sistema**: proteja todo el estado del sistema Windows.

El agente de MARS admite los siguientes escenarios de restauración:

![Escenarios de recuperación de MARS](./media/backup-try-azure-backup-in-10-mins/restore-scenarios.png)

- **Mismo servidor**: el servidor en el que se creó originalmente la copia de seguridad.
  - **Archivos y carpetas**: elija los archivos y carpetas individuales que quiere restaurar.
  - **Nivel de volumen**: elija el volumen y el punto de recuperación que quiere restaurar, y restáurelo en la misma ubicación o en una ubicación alternativa de la misma máquina.  Cree una copia de los archivos existentes, sobrescriba los archivos existentes u omita la recuperación de los archivos existentes.
  - **Nivel del sistema**: elija el estado del sistema y el punto de recuperación que quiere restaurar en una ubicación especificada de la misma máquina.

- **Servidor alternativo**: un servidor distinto del que se usó para realizar la copia de seguridad.
  - **Archivos y carpetas**: elija los archivos y carpetas individuales cuyo punto de recuperación quiere restaurar en un equipo de destino.
  - **Nivel de volumen**: elija el volumen y el punto de recuperación que quiere restaurar en otra ubicación. Cree una copia de los archivos existentes, sobrescriba los archivos existentes u omita la recuperación de los archivos existentes.
  - **Nivel del sistema**: elija el estado del sistema y el punto de recuperación que quiere restaurar como archivo de estado del sistema en una máquina alternativa.

## <a name="backup-process"></a>Proceso de copia de seguridad

1. En Azure Portal, cree un [almacén de Recovery Services](install-mars-agent.md#create-a-recovery-services-vault) y elija los archivos, las carpetas y el estado del sistema de entre los objetivos de copia de seguridad.
2. [Descargue las credenciales del almacén de Recovery Services y el instalador del agente](https://docs.microsoft.com/azure/backup/install-mars-agent#download-the-mars-agent) en una máquina local.

    Para proteger la máquina local con la opción de copia de seguridad, seleccione los archivos, carpetas y estado del sistema y, después, descargue el agente de MARS.

3. Preparación de la infraestructura:

    a. Ejecute el instalador para [instalar el agente](https://docs.microsoft.com/azure/backup/install-mars-agent#install-and-register-the-agent).

    b. Use las credenciales del almacén que descargó para registrar la máquina en el almacén de Recovery Services.
4. En la consola de agente del cliente, [configure la copia de seguridad](https://docs.microsoft.com/azure/backup/backup-windows-with-mars-agent#create-a-backup-policy). Especifique la directiva de retención de los datos de la copia de seguridad para iniciar la protección.

![Diagrama del agente de Azure Backup](./media/backup-try-azure-backup-in-10-mins/backup-process.png)

### <a name="additional-scenarios"></a>Otros escenarios

- **Hacer una copia de seguridad de archivos y carpetas específicos dentro de la VM de Azure**: el método principal para hacer copias de seguridad de máquinas virtuales (VM) de Azure es mediante una extensión de Azure Backup en la VM. La extensión realiza una copia de seguridad de toda la máquina virtual. Si quiere hacer una copia de seguridad de archivos y carpetas específicos dentro de una VM, puede instalar el agente de MARS en las máquinas virtuales de Azure. Para obtener más información, consulte [Arquitectura: copia de seguridad integrada de máquina virtual de Azure](https://docs.microsoft.com/azure/backup/backup-architecture#architecture-built-in-azure-vm-backup).

- **Propagación sin conexión**: las copias de seguridad completas iniciales de los datos en Azure normalmente transfieren grandes cantidades de datos y requieren más ancho de banda de red. Las copias de seguridad posteriores solo transfieren los datos Delta, o incrementales. Azure Backup permite comprimir las copias de seguridad iniciales. A través del proceso de *propagación sin conexión*, Azure Backup puede usar discos para cargar los datos comprimidos iniciales de la copia de seguridad sin conexión en Azure. Para obtener más información, consulte [Copia de seguridad sin conexión de Azure Backup con Azure Data Box](offline-backup-azure-data-box.md).

## <a name="next-steps"></a>Pasos siguientes

[Matriz de compatibilidad del agente de MARS](https://docs.microsoft.com/azure/backup/backup-support-matrix-mars-agent)

[Preguntas más frecuentes del agente de MARS](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq)
