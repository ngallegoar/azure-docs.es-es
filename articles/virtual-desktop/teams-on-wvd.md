---
title: 'Microsoft Teams en Windows Virtual Desktop: Azure'
description: Procedimiento para usar Microsoft Teams en Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 15a4c9b16b102310fd02f8db3a4fb93cff84882b
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2020
ms.locfileid: "81314066"
---
# <a name="use-microsoft-teams-on-windows-virtual-desktop"></a>Uso de Microsoft Teams en Windows Virtual Desktop

> Se aplica a: Windows 10, y Windows 10 IoT Enterprise

Los entornos virtualizados presentan un conjunto único de desafíos para las aplicaciones colaborativas como Microsoft Teams, como una mayor latencia, un uso elevado de la CPU de host y un rendimiento de audio y vídeo deficiente en general. Para obtener más información sobre el uso de Microsoft Teams en entornos de VDI, consulte [Teams para la infraestructura de escritorio virtualizado](https://docs.microsoft.com/microsoftteams/teams-for-vdi).

## <a name="prerequisites"></a>Prerrequisitos

Para poder usar Microsoft Teams en Windows Virtual Desktop, tendrá que hacer lo siguiente:

- Instale el [cliente de Escritorio de Windows](connect-windows-7-and-10.md) en un dispositivo con Windows 10 que cumpla los [requisitos de hardware](https://docs.microsoft.com/microsoftteams/hardware-requirements-for-the-teams-app) de Microsoft Teams.
- Conéctese a una máquina virtual (VM) de sesión múltiple de Windows 10 o Windows 10 Enterprise.
- [Prepare la red](https://docs.microsoft.com/microsoftteams/prepare-network) para Microsoft Teams.

## <a name="use-unoptimized-microsoft-teams"></a>Uso de Microsoft Teams sin optimizar

Puede usar Microsoft Teams sin optimizar en los entornos de Windows Virtual Desktop para aprovechar las características completas de chat y de colaboración de Microsoft Teams, así como las llamadas de audio. La calidad del audio en las llamadas variará en función de la configuración del host, ya que las llamadas sin optimizar usan más CPU del host.

### <a name="prepare-your-image-for-teams"></a>Preparación de la imagen para Teams

Para permitir la instalación por equipo de Teams, establezca la siguiente clave del Registro en el host:

```shell
  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Teams\IsWVDEnvironment]
  Type: REG_DWORD
  Value: 1
```

### <a name="install-microsoft-teams"></a>Instalación de Microsoft Teams

Puede implementar la aplicación de escritorio Teams mediante una instalación por máquina. Para instalar Microsoft Teams en el entorno de Windows Virtual Desktop:

1. Descargue el [paquete MSI de Teams](https://docs.microsoft.com/microsoftteams/teams-for-vdi#deploy-the-teams-desktop-app-to-the-vm) apropiado para el entorno. En un sistema operativo de 64 bits se recomienda usar el instalador de 64 bits.
2. Ejecute este comando para instalar el paquete MSI en la máquina virtual del host.

      ```shell
      msiexec /i <msi_name> /l*v < install_logfile_name> ALLUSER=1
      ```

      Esto instalará Teams en Archivos de programa o Archivos de programa (x86). La próxima vez que inicie sesión y ejecute Teams, la aplicación le solicitará sus credenciales.

      > [!NOTE]
      > En la actualidad, los usuarios y administradores no pueden deshabilitar el inicio automático para Teams durante el inicio de sesión.

      Para desinstalar el archivo MSI de la máquina virtual del host, ejecute este comando:

      ```shell
      msiexec /passive /x <msi_name> /l*v <uninstall_logfile_name>
      ```

      > [!NOTE]
      > Si instala Teams con el valor MSI ALLUSER=1, se deshabilitarán las actualizaciones automáticas. La recomendación es que se asegure de actualizar Teams al menos una vez al mes.
      
### <a name="customize-remote-desktop-protocol-properties-for-a-host-pool"></a>Personalización de las propiedades de Protocolo de escritorio remoto para un grupo de hosts
La personalización de las propiedades de Protocolo de escritorio remoto (RDP) de un grupo de hosts, como el uso de varios monitores, la habilitación del micrófono y la redirección de audio, permite ofrecer una experiencia óptima a los usuarios en función de sus necesidades. Las propiedades de RDP se pueden personalizar en Windows Virtual Desktop mediante el parámetro **- CustomRdpProperty** del cmdlet **conjunto RdsHostPool**.
En [Configuración admitida del archivo RDP](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/rdp-files?context=/azure/virtual-desktop/context/context) encontrará una lista completa de las propiedades admitidas y sus valores predeterminados.
