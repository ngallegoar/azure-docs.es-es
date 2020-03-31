---
title: Uso del emulador de Azurite para el desarrollo local de Azure Storage
description: El emulador de código abierto Azurite (versión preliminar) proporciona un entorno local gratuito para probar las aplicaciones de Azure Storage.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/31/2019
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.openlocfilehash: 5e1fce0852a4e820d7ee0af626ce3fddf6773750
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76029929"
---
# <a name="use-the-azurite-emulator-for-local-azure-storage-development-and-testing-preview"></a>Uso del emulador de Azurite para desarrollo y pruebas locales de Azure Storage (versión preliminar)

El emulador de código abierto Azurite versión 3.2 (versión preliminar) proporciona un entorno local gratuito para probar las aplicaciones de almacenamiento de colas y blobs de Azure. Cuando esté satisfecho con el funcionamiento de la aplicación a nivel local, puede empezar a usarla en una cuenta de almacenamiento de Azure Storage en la nube. El emulador proporciona compatibilidad multiplataforma en Windows, Linux y MacOS. Azurite v3 admite las API implementadas por Azure Blob service.

Azurite es la plataforma del emulador de almacenamiento futura. Azurite reemplaza al [emulador de Azure Storage](storage-use-emulator.md). Azurite se seguirá actualizando para admitir las versiones más recientes de las API de Azure Storage.

Hay varias maneras de instalar y ejecutar Azurite en el sistema local:

  1. [Instalación y ejecución de la extensión de Visual Studio Code para Azurite](#install-and-run-the-azurite-visual-studio-code-extension)
  1. [Instalación y ejecución de Azurite con NPM](#install-and-run-azurite-by-using-npm)
  1. [Instalación y ejecución de la imagen de Docker de Azurite](#install-and-run-the-azurite-docker-image)
  1. [Clonación, compilación y ejecución de Azurite desde el repositorio de GitHub](#clone-build-and-run-azurite-from-the-github-repository)

## <a name="install-and-run-the-azurite-visual-studio-code-extension"></a>Instalación y ejecución de la extensión de Visual Studio Code para Azurite

En Visual Studio Code, seleccione el panel **EXTENSIONES** y busque *Azurite* en **EXTENSIONES: MARKETPLACE**.

![Marketplace de extensiones de Visual Studio Code](media/storage-use-azurite/azurite-vs-code-extension.png)

También puede ir a [Marketplace de Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=Azurite.azurite) en el explorador. Seleccione el botón **Instalar** para abrir Visual Studio Code y vaya directamente a la página de la extensión de Azurite.

Puede iniciar o cerrar rápidamente Azurite haciendo clic en **[Azurite Blob service]** o **[Azurite Queue Service]** en la barra de estado de VS Code o ejecutando los siguientes comandos en la paleta de comandos de VS Code. Para abrir la paleta de comandos presione **F1** en Visual Studio Code.

La extensión admite los siguientes comandos de Visual Studio Code:

   * **Azurite: Start** (Azurite: Iniciar). Inicia todos los servicios de Azurite
   * **Azurite: Close**(Azurite: Cerrar). Cierra todos los servicios de Azurite
   * **Azurite: Clean** (Azurite: Limpiar). Restablece todos los datos de persistencia de los servicios de Azurite
   * **Azurite: Iniciar Blob Service**. Inicia Blob Service
   * **Azurite: Cerrar Blob Service**. Cierra Blob Service
   * **Azurite: Limpiar Blob Service**. Limpia Blob Service
   * **Azurite: Iniciar Queue Service**. Inicia Queue Service
   * **Azurite: Cerrar Queue Service**. Cierra Queue Service
   * **Azurite: Limpiar Queue Service**. Limpia Queue Service

Para configurar Azurite en Visual Studio Code, seleccione el panel de extensiones. Seleccione el icono de **Administración** (engranaje) de **Azurite**. Seleccione **Configure Extension Settings** (Configurar los valores de extensión).

![Configuración de los valores de extensión de Azurite](media/storage-use-azurite/azurite-configure-extension-settings.png)

Se admite los siguientes valores de configuración:

   * **Azurite: Blob Host** (Azurite: host de blob). El punto de conexión de escucha de Blob service. El valor predeterminado es 127.0.0.1.
   * **Azurite: Blob Port** (Azurite: puerto de blob). El puerto de escucha de Blob service. El puerto predeterminado es 10000.
   * **Azurite: Debug** (Azurite: depurar). Salida del registro de depuración al canal de Azurite. El valor predeterminado es **false**.
   * **Azurite: Location** (Ubicación de Azurite). La ruta de acceso de la ubicación del área de trabajo. El valor predeterminado es la carpeta de trabajo de Visual Studio Code.
   * **Azurite: Host de cola**. El punto de conexión de escucha del Queue service. El valor predeterminado es 127.0.0.1.
   * **Azurite: Puerto de cola**. El puerto de escucha del Queue service. El puerto predeterminado es 10001.
   * **Azurite: Silent** (Azurite: modo silencioso). El modo silencioso deshabilita el registro de acceso. El valor predeterminado es **false**.

## <a name="install-and-run-azurite-by-using-npm"></a>Instalación y ejecución de Azurite con NPM

Este método de instalación requiere que tenga instalada [la versión 8.0 de Node.js o una posterior](https://nodejs.org). **npm** es la herramienta de administración de paquetes incluida con cada instalación de Node.js. Después de instalar Node.js, ejecute el siguiente comando de **npm** para instalar Azurite.

```console
npm install -g azurite
```

Después de instalar Azurite, consulte [Ejecución de Azurite desde una línea de comandos](#run-azurite-from-a-command-line).

## <a name="install-and-run-the-azurite-docker-image"></a>Instalación y ejecución de la imagen de Docker de Azurite

Use [DockerHub](https://hub.docker.com/) para extraer la [imagen de Azurite más reciente](https://hub.docker.com/_/microsoft-azure-storage-azurite) con el siguiente comando:

```console
docker pull mcr.microsoft.com/azure-storage/azurite
```

**Ejecución de la imagen de Docker de Azurite**:

El siguiente comando ejecuta la imagen de Docker de Azurite. El parámetro `-p 10000:10000` redirige las solicitudes del puerto 10000 de la máquina host a la instancia de Docker.

```console
docker run -p 10000:10000 -p 10001:10001 mcr.microsoft.com/azure-storage/azurite
```

**Especifique la ubicación del área de trabajo**:

En el ejemplo siguiente, el `-v c:/azurite:/data`parámetro especifica *c:/azurite* como la ubicación de los datos persistentes de Azurite. El directorio *c:/Azurite*debe crearse antes de ejecutar el comando Docker.

```console
docker run -p 10000:10000 -p 10001:10001 -v c:/azurite:/data mcr.microsoft.com/azure-storage/azurite
```

**Ejecutar solo Blob service**

```console
docker run -p 10000:10000 mcr.microsoft.com/azure-storage/azurite
    azurite-blob --blobHost 0.0.0.0 --blobPort 10000
```

**Establecimiento de todos los parámetros de Azurite**:

Este ejemplo muestra cómo establecer todos los parámetros de línea de comandos. Todos los parámetros siguientes deben colocarse en una sola línea de comandos.

```console
docker run -p 8888:8888
           -p 9999:9999
           -v c:/azurite:/workspace mcr.microsoft.com/azure-storage/azurite azurite
           -l /workspace
           -d /workspace/debug.log
           --blobPort 8888
           --blobHost 0.0.0.0
           --queuePort 9999
           --queueHost 0.0.0.0
```

Consulte [Opciones de línea de comandos](#command-line-options) para más información sobre la configuración de Azurite al inicio.

## <a name="clone-build-and-run-azurite-from-the-github-repository"></a>Clonación, compilación y ejecución de Azurite desde el repositorio de GitHub

Este método de instalación requiere que tenga instalado [Git](https://git-scm.com/). Clone el [repositorio de GitHub](https://github.com/azure/azurite) para el proyecto de Azurite con el siguiente comando de la consola.

```console
git clone https://github.com/Azure/Azurite.git
```

Después de clonar el código fuente, ejecute los siguientes comandos desde la raíz del repositorio clonado para compilar e instalar Azurite.

```console
npm install
npm run build
npm install -g
```

Después de instalar y compilar Azurite, consulte [Ejecución de Azurite desde una línea de comandos](#run-azurite-from-a-command-line).

## <a name="run-azurite-from-a-command-line"></a>Ejecución de Azurite desde una línea de comandos

> [!NOTE]
> No se puede ejecutar Azurite desde la línea de comandos si solo se instaló la extensión Visual Studio Code. Utilice la paleta de comandos de Visual Studio Code. Para más información, consulte [Instalación y ejecución de la extensión de Visual Studio Code para Azurite](#install-and-run-the-azurite-visual-studio-code-extension).

Para empezar inmediatamente con la línea de comandos, cree un directorio denominado **c:\azurite**y, luego, inicie Azurite emitiendo el siguiente comando:

```console
azurite --silent --location c:\azurite --debug c:\azurite\debug.log
```

Este comando indica a Azurite que almacene todos los datos en un directorio determinado, **c:\azurite**. Si se omite la opción  **--location**, se usará el directorio de trabajo actual.

## <a name="command-line-options"></a>Opciones de línea de comandos

En esta sección se detallan los modificadores de línea de comandos disponibles al iniciar Azurite. Todos los modificadores de línea de comandos son opcionales.

```console
C:\Azurite> azurite [--blobHost <IP address>] [--blobPort <port address>] 
    [-d | --debug <log file path>] [-l | --location <workspace path>]
    [--queueHost <IP address>] [--queuePort <port address>]
    [-s | --silent] [-h | --help]
```

El **-d** es un acceso directo para **- debug**, **-l** el interruptor es acceso directo para **- location**, **- s** es un acceso directo para **- silent**, y **-h** es un acceso directo para **- help**.

### <a name="blob-listening-host"></a>Host de escucha de Blob

**Opcional** de forma predeterminada, Azurite escuchará a 127.0.0.1 como el servidor local. Use el modificador **--blobHost** para establecer la dirección de acuerdo a sus requisitos.

Acepte solicitudes solo en la máquina local:

```console
azurite --blobHost 127.0.0.1
```

Permita las solicitudes remotas:

```console
azurite --blobHost 0.0.0.0
```

> [!CAUTION]
> Permitir las solicitudes remotas puede hacer que el sistema sea vulnerable a ataques externos.

### <a name="blob-listening-port-configuration"></a>Configuración del puerto de escucha de Blob

**Opcional** de forma predeterminada, Azurite escuchará al Blob service en el puerto 10000. Use el modificador **--blobPort** para especificar el puerto de escucha que necesita.

> [!NOTE]
> Después de utilizar un puerto personalizado, tiene que actualizar la cadena de conexión o la configuración correspondiente en los SDK o las herramientas de Azure Storage.

Personalice el puerto de escucha de Blob service:

```console
azurite --blobPort 8888
```

Permitir al sistema seleccionar automáticamente un puerto disponible:

```console
azurite --blobPort 0
```

El puerto en uso se muestra durante el inicio de Azurite.

### <a name="queue-listening-host"></a>Host de escucha de Queue

**Opcional** de forma predeterminada, Azurite escuchará a 127.0.0.1 como el servidor local. Use el modificador **--colaHost** para establecer la dirección de acuerdo a sus requisitos.

Acepte solicitudes solo en la máquina local:

```console
azurite --queueHost 127.0.0.1
```

Permita las solicitudes remotas:

```console
azurite --queueHost 0.0.0.0
```

> [!CAUTION]
> Permitir las solicitudes remotas puede hacer que el sistema sea vulnerable a ataques externos.

### <a name="queue-listening-port-configuration"></a>Configuración del puerto de escucha de Queue

**Opcional** de forma predeterminada, Azurite escuchará al Queue service en el puerto 10001. Use el modificador **--queuePort** para especificar el puerto de escucha que necesita.

> [!NOTE]
> Después de utilizar un puerto personalizado, tiene que actualizar la cadena de conexión o la configuración correspondiente en los SDK o las herramientas de Azure Storage.

Personalice el puerto de escucha de Queue service:

```console
azurite --queuePort 8888
```

Permitir al sistema seleccionar automáticamente un puerto disponible:

```console
azurite --queuePort 0
```

El puerto en uso se muestra durante el inicio de Azurite.

### <a name="workspace-path"></a>Ruta de acceso del área de trabajo

**Opcional** Azurite almacena los datos en el disco local durante la ejecución. Use el modificador **--location** para especificar una ruta de acceso como ubicación del área de trabajo. De forma predeterminada, se usará el directorio de trabajo del proceso actual.

```console
azurite --location c:\azurite
```

```console
azurite -l c:\azurite
```

### <a name="access-log"></a>Registro de acceso

**Opcional** De forma predeterminada, el registro de acceso se muestra en la ventana de la consola. Deshabilite la visualización del registro de acceso con el modificador **--silent**.

```console
azurite --silent
```

```console
azurite -s
```

### <a name="debug-log"></a>Registro de depuración

**Opcional** El registro de depuración incluye información detallada sobre cada solicitud y seguimiento de la pila de excepciones. Habilite el registro de depuración proporcionando una ruta de acceso local válida de archivo al modificador **--debug**.

```console
azurite --debug path/debug.log
```

```console
azurite -d path/debug.log
```

### <a name="loose-mode"></a>Modo flexible

**Opcional** De forma predeterminada, Azurite aplica el modo strict para bloquear parámetros y encabezados de solicitud no compatibles. Deshabilite el modo strict mediante el modificador **--loose**.

```console
azurite --loose
```

Anote el modificador de acceso directo "L" mayúscula:

```console
azurite -L
```

## <a name="authorization-for-tools-and-sdks"></a>Autorización para herramientas y SDK

Conéctese a Azurite desde los SDK o las herramientas de Azure Storage, como el [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/), mediante cualquier estrategia de autenticación. Es necesaria la autenticación. Azurite admite la autorización con claves compartidas y firmas de acceso compartido (SAS). Azurite también admite el acceso anónimo a contenedores públicos.

### <a name="well-known-storage-account-and-key"></a>Cuenta de almacenamiento y clave conocidas

Puede usar el nombre de cuenta y clave siguientes con Azurite. Se trata de la misma cuenta y clave conocidas que usa el emulador de Azure Storage heredado.

* Nombre de cuenta: `devstoreaccount1`
* Clave de cuenta: `Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==`

> [!NOTE]
> Además de la autenticación SharedKey, Azurite admite la autenticación SAS de cuenta y servicio. El acceso anónimo también está disponible cuando un contenedor se establece para permitir el acceso público.

### <a name="connection-string"></a>Cadena de conexión

La manera más fácil de conectarse a Azurite desde su aplicación consiste en configurar, dentro del archivo de configuración de la aplicación, una cadena de conexión que haga referencia al acceso directo *UseDevelopmentStorage=true*. Este es un ejemplo de una cadena de conexión un archivo *app.config*:

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

Para obtener más información sobre las cadenas de conexión, consulte [Configuración de las cadenas de conexión de Azure Storage](storage-configure-connection-string.md).

### <a name="custom-storage-accounts-and-keys"></a>Cuentas y claves de almacenamiento personalizadas

Azurite admite las claves y los nombres de cuentas de almacenamiento personalizados estableciendo la variable de entorno `AZURITE_ACCOUNTS` en el siguiente formato: `account1:key1[:key2];account2:key1[:key2];...`.

Por ejemplo, use una cuenta de almacenamiento personalizada que tenga una clave:

```cmd
set AZURITE_ACCOUNTS="account1:key1"
```

O bien, use varias cuentas de almacenamiento con dos claves cada una:

```cmd
set AZURITE_ACCOUNTS="account1:key1:key2;account2:key1:key2"
```

Azurite actualiza las claves y los nombres de cuentas personalizados de la variable de entorno cada minuto de forma predeterminada. Con esta característica, puede girar de forma dinámica la clave de cuenta, o bien agregar nuevas cuentas de almacenamiento sin reiniciar Azurite.

> [!NOTE]
> La cuenta de almacenamiento `devstoreaccount1` predeterminada se deshabilita cuando establece cuentas de almacenamiento personalizadas.

> [!NOTE]
> Actualice la cadena de conexión en consecuencia al usar claves y nombres de cuentas personalizados.

> [!NOTE]
> Use la palabra clave `export` para establecer variables de entorno en un entorno Linux y utilice `set` en Windows.

### <a name="storage-explorer"></a>Explorador de Storage

En el Explorador de Azure Storage, conéctese a Azurite haciendo clic en el icono **Agregar cuenta**, luego seleccione **Vincular un emulador local** y haga clic en **Conectar**.

## <a name="differences-between-azurite-and-azure-storage"></a>Diferencias entre Azurite y Azure Storage

Existen diferencias funcionales entre una instancia local de Azurite y una cuenta de Azure Storage en la nube.

### <a name="endpoint-and-connection-url"></a>Punto de conexión y URL de conexión

Los puntos de conexión de servicio para Azurite son diferentes de los de una cuenta de Azure Storage. El equipo local no realiza la resolución del nombre de dominio, lo que requiere que los puntos de conexión de Azurite sean direcciones locales.

Al direccionar un recurso en una cuenta de Azure Storage, el nombre de la cuenta forma parte del nombre de host del URI. El recurso que se va a direccionar es parte de la ruta de acceso del URI:

`<http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>`

El siguiente URI es una dirección válida para un blob en una cuenta de Azure Storage:

`https://myaccount.blob.core.windows.net/mycontainer/myblob.txt`

Dado que el equipo local no realiza la resolución de nombres de dominio, el nombre de la cuenta forma parte de la ruta de acceso del URI en lugar del nombre de host. Use el siguiente formato de URI para un recurso en Azurite:

`http://<local-machine-address>:<port>/<account-name>/<resource-path>`

La siguiente dirección se puede usar para obtener acceso a un blob en Azurite:

`http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt`

### <a name="scaling-and-performance"></a>Escalado y rendimiento

Azurite no es un servicio de almacenamiento escalable y no es compatible con un gran número de clientes simultáneos. No hay garantía de rendimiento. Azurite se ha pensado para su uso con fines de desarrollo y pruebas.

### <a name="error-handling"></a>Control de errores

Azurite se alinea con la lógica de control de errores de Azure Storage, pero existen diferencias. Por ejemplo, los mensajes de error pueden ser diferentes, mientras que los códigos de estado de error están alineados.

### <a name="ra-grs"></a>RA-GRS

Azurite admite la replicación con redundancia geográfica con acceso de lectura (RA-GRS). Para los recursos de almacenamiento puede obtener acceso a la ubicación de la cuenta secundaria si anexa **secondary** al nombre de la cuenta. Por ejemplo, la siguiente dirección se puede usar para obtener acceso a un blob usando la cuenta secundaria de solo lectura en Azurite:

`http://127.0.0.1:10000/devstoreaccount1-secondary/mycontainer/myblob.txt`

## <a name="azurite-is-open-source"></a>Azurite es de código abierto

Las contribuciones y las sugerencias para Azurite son bienvenidas. Vaya a la página del [proyecto GitHub](https://github.com/Azure/Azurite/projects) o de [incidencias de Github](https://github.com/Azure/Azurite/issues) para ver los hitos y los elementos de trabajo de los que estamos haciendo un seguimiento para futuras características y correcciones de errores. También se realiza un seguimiento de los elementos de trabajo detallados en GitHub.

## <a name="next-steps"></a>Pasos siguientes

* [Uso del emulador de Azure Storage para el desarrollo y pruebas](storage-use-emulator.md) documenta el antiguo emulador de almacenamiento de Azure que Azurite sustituye.
* [Configuración de las cadenas de conexión de Azure Storage](storage-configure-connection-string.md) explica cómo ensamblar una cadena de conexión de Azure Storage que sea válida.
