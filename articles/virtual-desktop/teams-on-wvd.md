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
ms.openlocfilehash: a860b005457c6e02187423a3ffbbc63fe7c758b1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82187535"
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

Microsoft Teams se puede usar en los entornos de Windows Virtual Desktop para aprovechar las características de chat y colaboración de Microsoft Teams. Windows Virtual Desktop no es compatible con Teams en las optimizaciones de audio y vídeo (AV) de VDI. No se admiten llamadas ni reuniones. Si las directivas de la organización lo permiten, puede realizar llamadas de audio y unirse a reuniones con audio, pero la calidad del audio sin optimizar en las llamadas variará en función de la configuración del host y puede que no sea confiable. Para más información, consulte [Consideraciones del rendimiento de Teams en VDI](https://docs.microsoft.com/microsoftteams/teams-for-vdi#teams-on-vdi-performance-considerations).

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
      > Si instala Teams con el valor MSI ALLUSER=1, se deshabilitarán las actualizaciones automáticas. La recomendación es que se asegure de actualizar Teams al menos una vez al mes. Para más información sobre la implementación de la aplicación de escritorio Teams, consulte [Implementación de la aplicación de escritorio de Teams en la máquina virtual](https://docs.microsoft.com/microsoftteams/teams-for-vdi#deploy-the-teams-desktop-app-to-the-vm)de la máquina virtual.

## <a name="customize-remote-desktop-protocol-properties-for-a-host-pool"></a>Personalización de las propiedades de Protocolo de escritorio remoto para un grupo de hosts
La personalización de las propiedades de Protocolo de escritorio remoto (RDP) de un grupo de hosts, como el uso de varios monitores, la habilitación del micrófono y la redirección de audio, permite ofrecer una experiencia óptima a los usuarios en función de sus necesidades. Las propiedades de RDP se pueden personalizar en Windows Virtual Desktop mediante el parámetro **- CustomRdpProperty** del cmdlet **conjunto RdsHostPool**.
En [Configuración admitida del archivo RDP](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/rdp-files?context=/azure/virtual-desktop/context/context) encontrará una lista completa de las propiedades admitidas y sus valores predeterminados.
