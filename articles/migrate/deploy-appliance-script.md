---
title: Configuración de un dispositivo de Azure Migrate con un script
description: Aprenda a configurar un dispositivo de Azure Migrate con un script
ms.topic: article
ms.date: 04/16/2020
ms.openlocfilehash: 0c4d85909bbfa623b5ad8590e973250474d9d95a
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81676309"
---
# <a name="set-up-an-appliance-with-a-script"></a>Configuración de un dispositivo con un script

Siga este artículo para crear una [aplicación de Azure Migrate](deploy-appliance.md) para evaluar o migrar máquinas virtuales de VMware y máquinas virtuales de Hyper-V. Ejecute un script para crear una aplicación y compruebe que se puede conectar a Azure. 

Puede implementar la aplicación para máquinas virtuales de Hyper-V y VMware mediante un script o mediante una plantilla que se descarga de Azure Portal. El uso de un script es útil si no se puede crear una máquina virtual mediante la plantilla descargada.

- Para usar una plantilla, siga los tutoriales de [VMware](tutorial-prepare-vmware.md) o [Hyper-V](tutorial-prepare-hyper-v.md).
- Para configurar una aplicación para servidores físicos, solo puede usar un script. Siga [este artículo](how-to-set-up-appliance-physical.md).
- Para configurar una aplicación en una nube de Azure Government, siga [este artículo](deploy-appliance-script-government.md).

## <a name="prerequisites"></a>Prerrequisitos

El script configura el dispositivo de Azure Migrate en un equipo físico o máquina virtual existente.

- El equipo que hará de dispositivo debe ejecutar Windows Server 2016, con 32 GB de memoria, ocho CPU virtuales, cerca de 80 GB de almacenamiento en disco y un conmutador virtual externo. Requiere una dirección IP estática o dinámica y acceso a Internet.
- Antes de implementar el dispositivo, consulte los requisitos detallados del dispositivo para las [máquinas virtuales de VMware](migrate-appliance.md#appliance---vmware), las [máquinas virtuales de Hyper-V](migrate-appliance.md#appliance---hyper-v)y los [servidores físicos](migrate-appliance.md#appliance---physical).
- No ejecute el script en un dispositivo de Azure Migrate existente.

## <a name="set-up-the-appliance-for-vmware"></a>Instalación de la aplicación para VMware

Para instalar la aplicación para VMware, descargue un archivo comprimido de Azure Portal y extraiga el contenido. Ejecute el script de PowerShell para iniciar la aplicación web de la aplicación. Instale la aplicación y configúrela por primera vez. Después, registre la aplicación en el proyecto de Azure Migrate.

### <a name="download-the-script"></a>Descarga del script

1.  En **Objetivos de migración** > **Servidores** > **Azure Migrate: Server Assessment**, haga clic en **Detectar**.
2.  En **Detectar máquinas** >  **¿Las máquinas están virtualizadas?** , haga clic en **Yes, with VMware vSphere Hypervisor** (Sí, con VMware vSphere Hypervisor).
3.  Haga clic en **Descargar** para descargar el archivo comprimido. 


### <a name="verify-file-security"></a>Comprobación de la seguridad del archivo

Compruebe que el archivo comprimido es seguro, antes de implementarlo.

1. En la máquina en la que descargó el archivo, abra una ventana de comandos de administrador.
2. Ejecute el siguiente comando para generar el código hash para el archivo ZIP.
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Ejemplo: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller.zip SHA256```
3. Compruebe el valor hash generado. Para obtener la versión más reciente de la aplicación:

    **Algoritmo** | **Valor del código hash**
    --- | ---
    MD5 | 1e92ede3e87c03bd148e56a708cdd33f
    SHA256 | a3fa78edc8ff8aff9ab5ae66be1b64e66de7b9f475b6542beef114b20bfdac3c



### <a name="run-the-script"></a>Ejecute el script.

Funcionamiento del script:

- Instala agentes y una aplicación web.
- Instala roles de Windows, como el servicio de activación de Windows, IIS y PowerShell ISE.
- Descarga e instala un módulo de reescritura de IIS. [Más información](https://www.microsoft.com/download/details.aspx?id=7435).
- Actualiza una clave del registro (HKLM) con una configuración persistente para Azure Migrate.
- Crea archivos de registro y de configuración del siguiente modo:
    - **Archivos de configuración**: %ProgramData%\Microsoft Azure\Config
    - **Archivos de registro**: %ProgramData%\Microsoft Azure\Logs

Para ejecutar el script:

1. Extraiga el archivo comprimido en una carpeta del equipo que vaya a hospedar el dispositivo. No ejecute el script en una máquina en una aplicación de Azure Migrate existente.
2. Inicie PowerShell en el equipo con privilegios de administrador (elevados).
3. Cambie el directorio de PowerShell a la carpeta que contiene el contenido extraído del archivo comprimido descargado.
4. Ejecute el script **AzureMigrateInstaller.ps1** como se indica a continuación:

    ``` PS C:\Users\administrator\Desktop\AzureMigrateInstaller> AzureMigrateInstaller.ps1 -scenario VMware ```
   
5. Una vez que el script se haya ejecutado correctamente, inicia la aplicación web del dispositivo para que pueda configurarlo. Si detecta algún problema, consulte los registros del script en C:\Archivos de programa\Microsoft Azure\Logs\AzureMigrateScenarioInstaller_<em>Timestamp</em>.log.

### <a name="verify-access"></a>Comprobación del acceso

Asegúrese de que la aplicación puede conectarse a las direcciones URL de Azure para la nube [pública](migrate-appliance.md#public-cloud-urls).

## <a name="set-up-the-appliance-for-hyper-v"></a>Configure la aplicación para Hyper-V

Para configurar la aplicación para Hyper-V, descargue un archivo comprimido de Azure Portal y extraiga el contenido. Ejecute el script de PowerShell para iniciar la aplicación web de la aplicación. Instale la aplicación y configúrela por primera vez. Después, registre la aplicación en el proyecto de Azure Migrate.

### <a name="download-the-script"></a>Descarga del script

1.  En **Objetivos de migración** > **Servidores** > **Azure Migrate: Server Assessment**, haga clic en **Detectar**.
2.  En **Detectar máquinas** >  **¿Las máquinas están virtualizadas?** , seleccione **Sí, con Hyper-V**.
3.  Haga clic en **Descargar** para descargar el archivo comprimido. 


### <a name="verify-file-security"></a>Comprobación de la seguridad del archivo

Compruebe que el archivo comprimido es seguro, antes de implementarlo.

1. En la máquina en la que descargó el archivo, abra una ventana de comandos de administrador.
2. Ejecute el siguiente comando para generar el código hash para el archivo ZIP.
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Ejemplo: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller.zip SHA256```

3. Compruebe los valores hash generados. Para obtener la versión más reciente de la aplicación:

    **Algoritmo** | **Valor del código hash**
    --- | ---
    MD5 | 1e92ede3e87c03bd148e56a708cdd33f
    SHA256 | a3fa78edc8ff8aff9ab5ae66be1b64e66de7b9f475b6542beef114b20bfdac3c

### <a name="run-the-script"></a>Ejecute el script.

Funcionamiento del script:

- Instala agentes y una aplicación web.
- Instala roles de Windows, como el servicio de activación de Windows, IIS y PowerShell ISE.
- Descarga e instala un módulo de reescritura de IIS. [Más información](https://www.microsoft.com/download/details.aspx?id=7435).
- Actualiza una clave del registro (HKLM) con una configuración persistente para Azure Migrate.
- Crea archivos de registro y de configuración del siguiente modo:
    - **Archivos de configuración**: %ProgramData%\Microsoft Azure\Config
    - **Archivos de registro**: %ProgramData%\Microsoft Azure\Logs

Para ejecutar el script:

1. Extraiga el archivo comprimido en una carpeta del equipo que vaya a hospedar el dispositivo. No ejecute el script en una máquina en una aplicación de Azure Migrate existente.
2. Inicie PowerShell en el equipo con privilegios de administrador (elevados).
3. Cambie el directorio de PowerShell a la carpeta que contiene el contenido extraído del archivo comprimido descargado.
4. Ejecute el script **AzureMigrateInstaller.ps1** como se indica a continuación: ``` PS C:\Users\administrator\Desktop\AzureMigrateInstaller> AzureMigrateInstaller.ps1 -scenario Hyperv ```
   
5. Una vez que el script se haya ejecutado correctamente, inicia la aplicación web del dispositivo para que pueda configurarlo. Si detecta algún problema, consulte los registros del script en C:\Archivos de programa\Microsoft Azure\Logs\AzureMigrateScenarioInstaller_<em>Timestamp</em>.log.

### <a name="verify-access"></a>Comprobación del acceso

Asegúrese de que la aplicación puede conectarse a las direcciones URL de Azure para la nube [pública](migrate-appliance.md#public-cloud-urls).

## <a name="next-steps"></a>Pasos siguientes

Después de implementar la aplicación, es preciso que la configure y la registre en el proyecto de Azure Migrate.

- Configure el dispositivo para [VMware](how-to-set-up-appliance-vmware.md#configure-the-appliance).
- Configure el dispositivo para [Hyper-V](how-to-set-up-appliance-hyper-v.md#configure-the-appliance).
