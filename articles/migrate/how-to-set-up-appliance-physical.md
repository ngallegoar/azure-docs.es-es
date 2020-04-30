---
title: Configuración de un dispositivo de Azure Migrate para servidores físicos
description: Aprenda a configurar un dispositivo de Azure Migrate para la evaluación del servidor físico.
ms.service: azure-migrate
ms.topic: article
ms.date: 04/15/2020
ms.openlocfilehash: ddc70ee9430d3a767ce01191824c150a4dbd5e6f
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81538280"
---
# <a name="set-up-an-appliance-for-physical-servers"></a>Configuración de un dispositivo para servidores físicos

En este artículo se describe cómo configurar el dispositivo de Azure Migrate si está evaluando los servidores físicos con Azure Migrate: Server Assessment.

El dispositivo de Azure Migrate es un dispositivo ligero que Azure Migrate Server Assessment usa para lo siguiente:

- Descubrir servidores locales.
- Enviar metadatos y datos de rendimiento de los servidores detectados a Azure Migrate Server Assessment.

[Más información](migrate-appliance.md) sobre el dispositivo de Azure Migrate.


## <a name="appliance-deployment-steps"></a>Pasos de implementación del dispositivo

Para configurar el dispositivo:
- Descargue un archivo comprimido con el script del instalador de Azure Migrate desde Azure Portal.
- Extraiga el contenido del archivo comprimido. Inicie la consola de PowerShell con privilegios administrativos.
- Ejecute el script de PowerShell para iniciar la aplicación web del dispositivo.
- Configurar el dispositivo por primera vez y registrarlo en el proyecto de Azure Migrate.

## <a name="download-the-installer-script"></a>Descarga del script del instalador

Descargue el archivo comprimido del dispositivo.

1. En **Objetivos de migración** > **Servidores** > **Azure Migrate: Server Assessment**, haga clic en **Detectar**.
2. En **Detectar máquinas** >  **¿Las máquinas están virtualizadas?** , haga clic en **No virtualizado/Otro**.
3. Haga clic en **Descargar** para descargar el archivo comprimido.

    ![Descargar máquina virtual](./media/tutorial-assess-physical/download-appliance.png)


### <a name="verify-security"></a>Comprobación de la seguridad

Compruebe que el archivo comprimido es seguro, antes de implementarlo.

1. En la máquina en la que descargó el archivo, abra una ventana de comandos de administrador.
2. Ejecute el siguiente comando para generar el código hash para el archivo ZIP:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Ejemplo de uso para la nube pública: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller.zip SHA256 ```
    - Ejemplo de uso para la nube gubernamental: ```  C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller-Server-USGov.zip MD5 ```
3.  Verifique los valores hash:
 
    - Para la nube pública (para la versión más reciente del dispositivo):

        **Algoritmo** | **Valor del código hash**
          --- | ---
          MD5 | 1e92ede3e87c03bd148e56a708cdd33f
          SHA256 | a3fa78edc8ff8aff9ab5ae66be1b64e66de7b9f475b6542beef114b20bfdac3c

    - Para Azure Government (para la versión más reciente del dispositivo):

        **Algoritmo** | **Valor del código hash**
          --- | ---
          MD5 | f81c155fc4a1409901caea948713913f


## <a name="run-the-azure-migrate-installer-script"></a>Ejecución del script del instalador de Azure Migrate
El script del instalador hace lo siguiente:

- Instala los agentes y una aplicación web para la detección y evaluación de los servidores físicos.
- Instala los roles de Windows, incluido el servicio de activación de Windows, IIS y PowerShell ISE.
- Descarga e instala un módulo de reescritura de IIS. [Más información](https://www.microsoft.com/download/details.aspx?id=7435).
- Actualiza una clave del registro (HKLM) con detalles de configuración persistentes para Azure Migrate.
- Crea los siguientes archivos en la ruta de acceso:
    - **Archivos de configuración**:%Programdata%\Microsoft Azure\Config
    - **Archivos de registro**:%Programdata%\Microsoft Azure\Logs

Ejecute el script como se indica a continuación:

1. Extraiga el archivo comprimido en la carpeta del servidor que hospedará el dispositivo.  No ejecute el script en una máquina en un dispositivo de Azure Migrate existente.
2. Inicie PowerShell en el servidor anterior con privilegios administrativos (elevados).
3. Cambie el directorio de PowerShell a la carpeta en la que se ha extraído el contenido del archivo comprimido descargado.
4. Ejecute el script denominado **AzureMigrateInstaller.ps1** ejecutando el comando siguiente:

    - Para la nube pública: ``` PS C:\Users\administrator\Desktop\AzureMigrateInstaller> AzureMigrateInstaller.ps1 ```
    - Para Azure Government: ``` PS C:\Users\Administrators\Desktop\AzureMigrateInstaller-Server-USGov>AzureMigrateInstaller.ps1 ```

    El script iniciará la aplicación web del dispositivo cuando finalice correctamente.

En caso de que surja algún problema, puede acceder a los registros de script en C:\ProgramData\Microsoft Azure\Logs\AzureMigrateScenarioInstaller_<em>Timestamp</em>.log para solucionarlo.



### <a name="verify-appliance-access-to-azure"></a>Comprobación de que el dispositivo puede acceder a Azure

Asegúrese de que la VM del dispositivo pueda conectarse a las direcciones URL de Azure para las nubes [públicas](migrate-appliance.md#public-cloud-urls) y [gubernamentales](migrate-appliance.md#government-cloud-urls).

## <a name="configure-the-appliance"></a>Configuración del dispositivo

Configure el dispositivo por primera vez.

1. Abra un explorador en cualquier equipo que pueda conectarse a la máquina virtual y abra la dirección URL de la aplicación web del dispositivo: **https://*nombre o dirección IP del dispositivo*: 44368**.

   Como alternativa, puede abrir la aplicación desde el escritorio, para lo que debe hacer clic en el acceso directo de la aplicación.
2. En la aplicación web > **Set up prerequisites** (Configurar los requisitos previos ), realice las siguientes operaciones:
    - **License** (Licencia): Acepte los términos de licencia y lea la información de terceros.
    - **Connectivity** (Conectividad): la aplicación comprueba que la máquina virtual tiene acceso a Internet. Si la máquina virtual usa un proxy:
        - Haga clic en **Configuración de proxy** y especifique el puerto de escucha y la dirección del proxy con los formatos http://ProxyIPAddress o http://ProxyFQDN.
        - Especifique las credenciales si el proxy requiere autenticación.
        - Solo se admite un proxy HTTP.
    - **Time sync** (Sincronización de hora): Se comprueba la hora. Para que la detección funcione correctamente, la hora del dispositivo debe estar sincronizada con la hora de Internet.
    - **Instalación de actualizaciones**: Azure Migrate Server Assessment comprueba que el dispositivo tiene instaladas las actualizaciones más recientes.

### <a name="register-the-appliance-with-azure-migrate"></a>Registro del dispositivo en Azure Migrate

1. Haga clic en **Iniciar sesión**. Si no aparece, asegúrese de que ha deshabilitado el bloqueador de elementos emergentes en el explorador.
2. En la pestaña nueva, inicie sesión con sus credenciales de Azure.
    - Inicie sesión con su nombre de usuario y contraseña.
    - No se admite el inicio de sesión con un PIN.
3. Después de iniciar sesión correctamente, vuelva a la aplicación web.
4. Seleccione la suscripción en la que se creó el proyecto de Azure Migrate. Seleccione el proyecto.
5. Escriba un nombre para el dispositivo. Este nombre debe ser alfanumérico y no puede tener más de 14 caracteres.
6. Haga clic en **Registrar**.


## <a name="start-continuous-discovery"></a>Inicio de detección continua

Conéctese desde el dispositivo a los servidores físicos e inicie la detección.

1. Haga clic en **Agregar credenciales** para especificar las credenciales de la cuenta que el dispositivo utilizará para detectar los servidores.  
2. Especifique el **sistema operativo**, el nombre descriptivo de las credenciales, el nombre de usuario y la contraseña. A continuación, haga clic en **Agregar**.
Puede agregar un conjunto de credenciales para los servidores Windows y Linux.
4. Haga clic en **Agregar servidor** y especifique los detalles del servidor: FQDN/dirección IP y nombre descriptivo de las credenciales (una entrada por fila) para conectarse al servidor.
3. Haga clic en **Validar**. Después de la validación, se muestra la lista de servidores que se pueden detectar.
    - Si se produce un error de validación para un servidor, mantenga el puntero del mouse sobre el icono en la columna **Estado** para revisar el error. Corrija los problemas y vuelva a validar.
    - Para quitar un servidor, seleccione > **Eliminar**.
4. Después de la validación, haga clic en **Guardar e iniciar la detección** para iniciar el proceso de detección.

De esta forma comienza la detección. Los metadatos de las VM detectadas tardan unos 15 minutos en aparecer en Azure Portal.

## <a name="verify-servers-in-the-portal"></a>Comprobación de los servidores en el portal

Una vez finalizada la detección, puede verificar que los servidores aparezcan en el portal.

1. Abra el panel de Azure Migrate.
2. En la página **Azure Migrate - Servidores** > **Azure Migrate: Server Assessment**, haga clic en el icono que muestra el número de **servidores detectados**.


## <a name="next-steps"></a>Pasos siguientes

Pruebe la [evaluación de los servidores físicos](tutorial-assess-physical.md) con Azure Migrate Server Assessment.
