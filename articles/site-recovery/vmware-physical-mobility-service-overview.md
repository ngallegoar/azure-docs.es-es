---
title: Acerca de Mobility Service para la recuperación ante desastres de máquinas virtuales VMware y servidores físicos en Azure con Azure Site Recovery | Microsoft Docs
description: Aprenda acerca del agente de Mobility Service para la recuperación ante desastres de máquinas virtuales VMware y servidores físicos en Azure con el servicio Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: ramamill
ms.openlocfilehash: c5acc9637fe5afe8f7dd32d23fbdbb80373b4f61
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79228812"
---
# <a name="about-the-mobility-service-for-vmware-vms-and-physical-servers"></a>Acerca de Mobility Service para máquinas virtuales VMware y servidores físicos

Al configurar la recuperación ante desastres para máquinas virtuales VMware y servidores físicos con [Azure Site Recovery](site-recovery-overview.md), instala el servicio Site Recovery Mobility en cada máquina virtual VMware local y servidor físico.  Mobility Service captura las escrituras de datos en la máquina y las reenvía al servidor de procesos de Site Recovery. Puede implementar Mobility Service con los métodos siguientes:

- [Instalación de inserción](#push-installation): Site Recovery instala el agente de movilidad en el servidor al habilitarse la protección a través de Azure Portal.
- Instalación manual: puede instalar Mobility Service manualmente en cada máquina a través de la [interfaz de usuario](#install-mobility-agent-through-ui) o el [símbolo del sistema](#install-mobility-agent-through-command-prompt).
- [Implementación automatizada](vmware-azure-mobility-install-configuration-mgr.md): puede automatizar la instalación con herramientas de implementación de software como Configuration Manager.

> [!NOTE]
> El agente de movilidad usa aproximadamente entre un 6 y un 10 % de memoria en máquinas de origen para máquinas físicas o máquinas virtuales de VMware.

## <a name="anti-virus-on-replicated-machines"></a>Antivirus en máquinas replicadas

Si las máquinas que desea replicar tiene software antivirus activo en ejecución, asegúrese de excluir la carpeta de instalación del servicio de movilidad de las operaciones antivirus (*C:\ProgramData\ASR\agent*). Esto garantiza que la replicación funciona según lo previsto.

## <a name="push-installation"></a>Instalación de inserción

La instalación de inserción es una parte integral del trabajo "[Habilitar replicación](vmware-azure-enable-replication.md#enable-replication)" desencadenado en el portal. Después de elegir el conjunto de máquinas virtuales que desea para proteger y desencadenar "Habilitar replicación", el servidor de configuración inserta el agente de movilidad en los servidores, lo instala y completa su registro. Para completar esta operación con éxito, haga lo siguiente:

- Asegúrese de que se cumplen todos los [requisitos previos](vmware-azure-install-mobility-service.md).
- Asegúrese de que todas las configuraciones de servidores se incluyen en la [matriz de soporte de VMware para el escenario de recuperación ante desastres de Azure](vmware-physical-azure-support-matrix.md).

Se han descrito detalles del flujo de trabajo de instalación de inserción en las siguientes secciones.

### <a name="from-923-version-onwards"></a>A partir de la [versión 9.23](https://support.microsoft.com/en-in/help/4494485/update-rollup-35-for-azure-site-recovery)

Durante la instalación de inserción del agente de movilidad, se realizan los siguientes pasos:

1. Inserta el agente en la máquina de origen. Es posible que no se pueda copiar el agente en la máquina de origen debido a varios errores de entorno. Visite [nuestra guía](vmware-azure-troubleshoot-push-install.md) para solucionar los errores de instalación de inserción.
2. Tras copiarse correctamente el agente en el servidor, se realizan comprobaciones de requisitos previos en este. Si uno o varios de los [requisitos previos](vmware-physical-azure-support-matrix.md) no se cumplen, se producirá un error en la instalación. Si se cumplen todos los requisitos previos, se desencadenará la instalación.
3. El proveedor VSS de Azure Site Recovery está instalado en el servidor como parte de la instalación del agente de movilidad. Este proveedor se usa para generar puntos coherentes con la aplicación. Si se produce un error en la instalación de VSS, este paso se omitirá y la instalación del agente continuará.
4. Si la instalación del agente se realiza correctamente, pero se produce un error en la del proveedor VSS, el estado del trabajo se marca como "Advertencia". Esto no afecta a la generación de puntos de coherencia de bloqueos.

    a. Para generar puntos coherentes con la aplicación, consulte [nuestra guía](vmware-physical-manage-mobility-service.md#install-site-recovery-vss-provider-on-source-machine) para completar la instalación del proveedor VSS de Site Recovery manualmente. </br>
    b.  Si no desea que se generen puntos coherentes con la aplicación, [modifique la directiva de replicación](vmware-azure-set-up-replication.md#create-a-policy) para desactivarlos.

### <a name="before-922-versions"></a>Versiones anteriores a la 9.22

1. Inserta el agente en la máquina de origen. Es posible que no se pueda copiar el agente en la máquina de origen debido a varios errores de entorno. Visite [nuestra guía](vmware-azure-troubleshoot-push-install.md) para solucionar los errores de instalación de inserción.
2. Tras copiarse correctamente el agente en el servidor, se realizan comprobaciones de requisitos previos en este. Si uno o varios de los [requisitos previos](vmware-physical-azure-support-matrix.md) no se cumplen, se producirá un error en la instalación. Si se cumplen todos los requisitos previos, se desencadenará la instalación.
3. El proveedor VSS de Azure Site Recovery está instalado en el servidor como parte de la instalación del agente de movilidad. Este proveedor se usa para generar puntos coherentes con la aplicación. Si se produce un error en la instalación del proveedor VSS, fallará la instalación del agente. Para evitar errores en la instalación del agente de movilidad, use la [versión 9.23](https://support.microsoft.com/en-in/help/4494485/update-rollup-35-for-azure-site-recovery) o versiones posteriores para generar puntos coherentes de bloqueos e instalar el proveedor VSS manualmente.

## <a name="install-mobility-agent-through-ui"></a>Instalación del agente de movilidad a través de la interfaz de usuario

### <a name="prerequisite"></a>Requisito previo

- Asegúrese de que todas las configuraciones de servidores se incluyen en la [matriz de soporte de VMware para el escenario de recuperación ante desastres de Azure](vmware-physical-azure-support-matrix.md).
- [Busque el instalador](#locate-installer-files) en función del sistema operativo del servidor.

>[!IMPORTANT]
> Si va a replicar máquinas virtuales de IaaS de Azure desde una región de Azure a otra, no use este método. En su lugar, use el método de instalación basado en la línea de comandos.

1. Copie el archivo de instalación en el equipo y ejecútelo.
2. En **Opción de instalación**, seleccione **Instalar Mobility Service**.
3. Seleccione la ubicación de instalación > **Instalar**.

    ![Página Opción de instalación de Mobility Service](./media/vmware-physical-mobility-service-install-manual/mobility1.png)

4. Supervise la instalación en **Progreso de la instalación**. Una vez completada la instalación, seleccione **Continuar con la configuración** para registrar el servicio en el servidor de configuración.

    ![Página de registro de Mobility Service](./media/vmware-physical-mobility-service-install-manual/mobility3.png)

5. En **Configuration Server Details** (Detalles del servidor de configuración), especifique la dirección IP y la frase de contraseña que ha configurado.

    ![Página de registro de Mobility Service](./media/vmware-physical-mobility-service-install-manual/mobility4.png)

6. Seleccione **Registrar** para finalizar el registro.

    ![Página de registro final de Mobility Service](./media/vmware-physical-mobility-service-install-manual/mobility5.png)

## <a name="install-mobility-agent-through-command-prompt"></a>Instalación del agente de movilidad a través del símbolo del sistema

### <a name="prerequisite"></a>Requisito previo

- Asegúrese de que todas las configuraciones de servidores se incluyen en la [matriz de soporte de VMware para el escenario de recuperación ante desastres de Azure](vmware-physical-azure-support-matrix.md).
- [Busque el instalador](#locate-installer-files) en función del sistema operativo del servidor.

### <a name="on-a-windows-machine"></a>Una máquina Windows

- Copie el instalador en una carpeta local (por ejemplo, C:\Temp) del servidor que desea proteger.

    ```
    cd C:\Temp
    ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
    MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
    cd C:\Temp\Extracted
    ```

- Haga la instalación de la siguiente manera:

    ```
    UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
    ```

- Registre el agente en el servidor de configuración.

    ```
    cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
    UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
    ```

#### <a name="installation-settings"></a>Configuración de la instalación
**Configuración** | **Detalles**
--- | ---
Uso | UnifiedAgent.exe /Role \<MS/MT> /InstallLocation \<Install Location> /Platform “VmWare” /Silent
Registros de configuración | En %ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log.
/Role | Parámetro de instalación obligatorio. Especifica si se debe instalar Mobility Service (MS) o el destino maestro (MT).
/InstallLocation| Parámetro opcional. Especifique la ubicación (cualquier carpeta) de la instalación de Mobility Service.
/Platform | Mandatory. Especifica la plataforma en la que se instala Mobility Service. **VMware** para servidores físicos o máquinas virtuales Mware; **Azure** para máquinas virtuales de Azure.<br/><br/> Si está tratando máquinas virtuales de Azure como máquinas físicas, especifique **VMware**.
/Silent| Opcional. Especifica si se debe ejecutar el instalador en modo silencioso.

#### <a name="registration-settings"></a>Configuración de registro
**Configuración** | **Detalles**
--- | ---
Uso | UnifiedAgentConfigurator.exe  /CSEndPoint \<CSIP> /PassphraseFilePath \<PassphraseFilePath>
Registros de configuración del agente | En %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log.
/CSEndPoint | Parámetro obligatorio. Especifique la dirección IP del servidor de configuración. Use una dirección IP válida.
/PassphraseFilePath |  Mandatory. Ubicación de la frase de contraseña. Use cualquier ruta de acceso de archivo local o UNC válida.

### <a name="on-a-linux-machine"></a>En un equipo Linux

1. Copie el instalador en una carpeta local (por ejemplo, /tmp) del servidor que desea proteger. En un terminal, ejecute los siguientes comandos:

    ```
    cd /tmp ;
    tar -xvf Microsoft-ASR_UA*release.tar.gz
    ```

2. Haga la instalación de la siguiente manera:

    ```
    sudo ./install -d <Install Location> -r MS -v VmWare -q
    ```

3. Una vez completada la instalación, Mobility Service debe registrarse en el servidor de configuración. Ejecute el siguiente comando para registrar Mobility Service en el servidor de configuración:

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
    ```

#### <a name="installation-settings"></a>Configuración de la instalación
**Configuración** | **Detalles**
--- | ---
Uso | ./install -d \<Install Location> -r \<MS/MT> -v VmWare -q
-r | Parámetro de instalación obligatorio. Especifica si se debe instalar Mobility Service (MS) o el destino maestro (MT).
-d | Parámetro opcional. Especifique la ubicación de instalación de Mobility Service: /usr/local/ASR.
-v | Mandatory. Especifica la plataforma en la que se instala Mobility Service. **VMware** para servidores físicos o máquinas virtuales Mware; **Azure** para máquinas virtuales de Azure.
-q | Opcional. Especifica si se debe ejecutar el instalador en modo silencioso.

#### <a name="registration-settings"></a>Configuración de registro
**Configuración** | **Detalles**
--- | ---
Uso | cd /usr/local/ASR/Vx/bin<br/><br/> UnifiedAgentConfigurator.sh -i \<CSIP> -P \<PassphraseFilePath>
-i | Parámetro obligatorio. Especifique la dirección IP del servidor de configuración. Use una dirección IP válida.
-P |  Mandatory. Ruta de acceso completa al archivo en que se guarda la frase de contraseña. Uso de cualquier carpeta válida.

## <a name="azure-virtual-machine-agent"></a>Agente de máquina virtual de Azure

- **Máquinas virtuales Windows**: desde la versión 9.7.0.0 de Mobility Service, el instalador de Mobility Service instala el [agente de máquina virtual de Azure](../virtual-machines/extensions/features-windows.md#azure-vm-agent). De este forma se garantiza que, cuando un equipo conmuta por error a Azure, la máquina virtual de Azure cumple el requisito previo de instalación del agente para usar cualquier extensión de máquina virtual.
- **Máquinas virtuales Linux**: [WALinuxAgent](https://docs.microsoft.com/azure/virtual-machines/extensions/update-linux-agent) debe instalarse manualmente en la máquina virtual Azure después de la conmutación por error.

## <a name="locate-installer-files"></a>Búsqueda de archivos del instalador

Vaya a la carpeta %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository del servidor de configuración. Compruebe qué instalador necesita en función del sistema operativo. En la siguiente tabla se resumen los archivos del instalador para cada máquina virtual VMware y el sistema operativo del servidor físico. Puede revisar los [sistemas operativos admitidos](vmware-physical-azure-support-matrix.md#replicated-machines) antes de empezar.

**Archivo de instalador** | **Sistema operativo (solo de 64 bits)**
--- | ---
Microsoft-ASR\_UA\*Windows\*release.exe | Windows Server 2016; Windows Server 2012 R2; Windows Server 2012; Windows Server 2008 R2 SP1
Microsoft-ASR\_UA\*RHEL6-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 6.* </br> CentOS 6.*
Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 7.* </br> CentOS 7.*
Microsoft-ASR\_UA\*SLES12-64\*release.tar.gz | SUSE Linux Enterprise Server 12 SP1, SP2, SP3
Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3
Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP4
Microsoft-ASR\_UA\*OL6-64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5
Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz | Ubuntu Linux 14.04
Microsoft-ASR\_UA\*UBUNTU-16.04-64\*release.tar.gz | Servidor Ubuntu Linux 16.04 LTS
Microsoft-ASR_UA\*DEBIAN7-64\*release.tar.gz | Debian 7
Microsoft-ASR_UA\*DEBIAN8-64\*release.tar.gz | Debian 8

## <a name="next-steps"></a>Pasos siguientes

[Configurar la instalación de Mobility Service](vmware-azure-install-mobility-service.md).
