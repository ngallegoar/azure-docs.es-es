---
title: Preparación de los servidores de Windows Server 2003 para la migración con Azure Migrate
description: Obtenga información sobre cómo preparar de los servidores de Windows Server 2003 para la migración con Azure Migrate.
ms.topic: how-to
ms.date: 05/27/2020
ms.openlocfilehash: 33519764b138c7711e6c03a85aa33ec6f936a748
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84172130"
---
# <a name="prepare-windows-server-2003-machines-for-migration"></a>Preparación de las máquinas de Windows Server 2003 para la migración

En este artículo se describe cómo preparar las máquinas que ejecutan Windows Server 2003 para la migración a Azure. 

- Puede usar la migración sin agente para migrar [máquinas virtuales de Hyper-V](tutorial-migrate-hyper-v.md) y [máquinas virtuales de VMware](tutorial-migrate-vmware.md) a Azure.
- Para conectarse a las máquinas virtuales de Azure después de la migración, servicios de integración de Hyper-V debe estar instalado en la máquina virtual de Azure. Las máquinas con Windows Server 2003 no lo tienen instalado de forma predeterminada.
- No hay ningún vínculo de descarga directa para instalar los servicios de integración de Hyper-V, por lo que debe hacer lo siguiente:
    - En el caso de las máquinas virtuales de Hyper-V que no los tienen instalado, extraiga los archivos de instalación de los servicios de integración en una máquina que ejecute Windows Server 2012 R2/Windows Server 2012 con el rol de Hyper-V y, a continuación, copie el instalador en la máquina con Windows Server 2003. Los archivos de instalación no están disponibles en las máquinas que ejecutan Windows Server 2016.
    - En el caso de las máquinas virtuales de VMware, se crea una tarea de inicio que instala los servicios de integración cuando la máquina virtual de Azure se inicia después de la migración.


## <a name="install-on-hyper-v-vms"></a>Instalación en máquinas virtuales de Hyper-V

Antes de la migración, compruebe si los servicios de integración de Hyper-V están instalados y, a continuación, instálelos si es necesario.

1. Siga [estas instrucciones](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#turn-an-integration-service-on-or-off-using-hyper-v-manager) para comprobar si están instalados.
2. Si no están instalados, inicie sesión en una máquina que ejecute Windows Server 2012 R2/Windows Server 2012 con el rol de Hyper-V.
3. Navegue hasta el archivo de instalación en **C:\Windows\System32\vmguest.iso** y móntelo.
2. Copie la carpeta de instalación en la máquina con Windows Server 2003 e instale los servicios de integración.
4. Después de la instalación, puede dejar la configuración predeterminada en los servicios de integración. 

## <a name="install-on-vmware-vms"></a>Instalación en máquinas virtuales de VMware

1. Inicie sesión en una máquina que ejecute Windows Server 2012 R2/Windows Server 2012 con el rol de Hyper-V.
2. Navegue hasta el archivo de instalación en **C:\Windows\System32\vmguest.iso** y móntelo.
3. Copie la carpeta de instalación en la máquina virtual de VMware.
4. En la línea de comandos de la máquina virtual, ejecute ```gpedit.msc```.
5. Abra **Configuración del equipo** > **Configuración de Windows** > **Scripts (inicio o apagado)** .
6. En **Inicio** > **Agregar** > **Nombre del script**, escriba la dirección de setup.exe.
7. Después de la migración a Azure, el script se ejecuta la primera vez que se inicia la máquina virtual de Azure.
8. Reinicie manualmente la máquina virtual de Azure. Aparece un elemento emergente en los diagnósticos de arranque para indicar que es necesario un reinicio.
9. Después de ejecutar el script e instalar los servicios de integración de Hyper-V en la máquina virtual de Azure, puede quitar el script del inicio.
10. Después de la instalación, puede dejar la configuración predeterminada en los servicios de integración. 

## <a name="next-steps"></a>Pasos siguientes

- Revise los requisitos de migración para [VMWare](migrate-support-matrix-vmware-migration.md) y las máquinas virtuales de [Hyper-V](migrate-support-matrix-hyper-v-migration.md).
- Migre máquinas virtuales de [VMware](server-migrate-overview.md) y [Hyper-V](tutorial-migrate-hyper-v.md).
