---
title: Configuración de un dispositivo de Azure Migrate con un script
description: Aprenda a configurar un dispositivo de Azure Migrate con un script
ms.topic: article
ms.date: 03/23/2020
ms.openlocfilehash: bf8d7148f685d4ac6a5f33603020a0503b0c62e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80337370"
---
# <a name="set-up-an-appliance-with-a-script"></a>Configuración de un dispositivo con un script

En este artículo se describe cómo configurar el [dispositivo de Azure Migrate](deploy-appliance.md) con un script de instalador de PowerShell.

El script proporciona:
- Una alternativa a la configuración del dispositivo mediante una plantilla de OVA, para la evaluación y la migración sin agente de máquinas virtuales de VMware.
- Una alternativa a la configuración del dispositivo mediante una plantilla de VHD, para la evaluación y la migración de máquinas virtuales de Hyper-V.
- En cuanto a la evaluación de servidores físicos (o de máquinas virtuales que quiera migrar como servidores físicos), el script es el único método disponible para configurar el dispositivo.

## <a name="prerequisites"></a>Prerrequisitos

El script configura el dispositivo de Azure Migrate en un equipo físico o máquina virtual existente.

- El equipo que hará de dispositivo debe ejecutar Windows Server 2016, con 32 GB de memoria, ocho CPU virtuales, cerca de 80 GB de almacenamiento en disco y un conmutador virtual externo. Requiere una dirección IP estática o dinámica y acceso a Internet.
- Antes de implementar el dispositivo, consulte los requisitos detallados del dispositivo para las [máquinas virtuales de VMware](migrate-appliance.md#appliance---vmware), las [máquinas virtuales de Hyper-V](migrate-appliance.md#appliance---hyper-v)y los [servidores físicos](migrate-appliance.md#appliance---physical).
- No ejecute el script en un dispositivo de Azure Migrate existente.


## <a name="download-the-script"></a>Descarga del script

1. Busque el equipo o máquina virtual que hará de dispositivo de Azure Migrate.
2. En el equipo, haga lo siguiente:

    - Para usar el dispositivo con máquinas virtuales de VMware o de Hyper-V, [descargue](https://go.microsoft.com/fwlink/?linkid=2105112) la carpeta comprimida que contiene el script del instalador y los archivos MSI.
    - Para usar el dispositivo con servidores físicos, descargue el script desde el portal de Azure Migrate, como se describe en este [tutorial](tutorial-assess-physical.md#set-up-the-appliance).

## <a name="verify-file-security"></a>Comprobación de la seguridad del archivo

Compruebe que el archivo comprimido es seguro, antes de implementarlo.

1. En la máquina en la que descargó el archivo, abra una ventana de comandos de administrador.
2. Ejecute el siguiente comando para generar el código hash para el archivo ZIP.
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Ejemplo de uso: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller.zip SHA256```

3. Compruebe que los valores hash generados coinciden con esta configuración (para la versión más reciente del dispositivo):

    **Algoritmo** | **Valor del código hash**
      --- | ---
      MD5 | 1e92ede3e87c03bd148e56a708cdd33f
      SHA256 | a3fa78edc8ff8aff9ab5ae66be1b64e66de7b9f475b6542beef114b20bfdac3c

## <a name="run-the-script"></a>Ejecute el script.

Funcionamiento del script:

- Instala agentes y una aplicación web.
- Instala roles de Windows, como el servicio de activación de Windows, IIS y PowerShell ISE.
- Descarga e instala un módulo de reescritura de IIS. [Más información](https://www.microsoft.com/download/details.aspx?id=7435).
- Actualiza una clave del registro (HKLM) con una configuración persistente para Azure Migrate.
- Crea archivos de registro y de configuración del siguiente modo:
    - **Archivos de configuración**: %ProgramData%\Microsoft Azure\Config
    - **Archivos de registro**: %ProgramData%\Microsoft Azure\Logs

Para ejecutar el script:

1. Extraiga el archivo comprimido en una carpeta del equipo que vaya a hospedar el dispositivo.
2. Inicie PowerShell en el equipo con privilegios de administrador (elevados).
3. Cambie el directorio de PowerShell a la carpeta que contiene el contenido extraído del archivo comprimido descargado.
4. Ejecute el script **AzureMigrateInstaller.ps1** como se indica a continuación:

    - Para VMware: 
        ```
        PS C:\Users\administrator\Desktop\AzureMigrateInstaller> AzureMigrateInstaller.ps1 -scenario VMware
        ```
    - Para Hyper-V:
        ```
        PS C:\Users\administrator\Desktop\AzureMigrateInstaller> AzureMigrateInstaller.ps1 -scenario Hyperv
        ```
    - Para servidores físicos:
        ```
        PS C:\Users\administrator\Desktop\AzureMigrateInstaller> AzureMigrateInstaller.ps1
        ```      
5. Una vez que el script se haya ejecutado correctamente, inicia la aplicación web del dispositivo para que pueda configurarlo. Si detecta algún problema, puede ver los registros del script en C:\Archivos de programa\Microsoft Azure\Logs\AzureMigrateScenarioInstaller_<em>Timestamp</em>.log.

## <a name="next-steps"></a>Pasos siguientes

Una vez configurado el dispositivo con el script, siga estas instrucciones:

- Configure el dispositivo para [VMware](how-to-set-up-appliance-vmware.md#configure-the-appliance).
- Configure el dispositivo para [Hyper-V](how-to-set-up-appliance-hyper-v.md#configure-the-appliance).
- Configure el dispositivo para [servidores físicos](how-to-set-up-appliance-physical.md).