---
title: Referencia de la línea de comandos y PowerShell para el emulador de Azure Cosmos DB
description: Aprenda los parámetros de la línea de comandos para el emulador de Azure Cosmos DB, cómo controlar el emulador con PowerShell y cómo cambiar el número de contenedores que puede crear dentro del emulador.
ms.service: cosmos-db
ms.topic: how-to
author: markjbrown
ms.author: mjbrown
ms.date: 09/17/2020
ms.custom: contperfq1
ms.openlocfilehash: f8bcadf25ac8e001657f2be012f99ddb507e672d
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91445186"
---
# <a name="command-line-and-powershell-reference-for-azure-cosmos-db-emulator"></a>Referencia de la línea de comandos y PowerShell para el emulador de Azure Cosmos DB

El emulador de Azure Cosmos proporciona un entorno local que emula el servicio Azure Cosmos DB con fines de desarrollo local. Después de [instalar el emulador](local-emulator.md), puede controlarlo con la línea de comandos y los comandos de PowerShell. En este artículo se describe cómo usar los comandos de la línea de comandos y PowerShell para iniciar y detener el emulador, configurar sus opciones y realizar otras operaciones. Tiene que ejecutar los comandos desde la ubicación de instalación.

##  <a name="manage-the-emulator-with-command-line-syntax"></a><a id="command-line"></a>Administración del emulador con la sintaxis de línea de comandos

```cmd
Microsoft.Azure.Cosmos.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/EnableMongoDbEndpoint] [/?]
```

Para ver la lista de opciones, escriba `Microsoft.Azure.Cosmos.Emulator.exe /?` en el símbolo del sistema.

|**Opción** | **Descripción** | **Comando**| **Argumentos**|
|---|---|---|---|
|[Sin argumentos] | Inicia el emulador de Azure Cosmos con la configuración predeterminada. |Microsoft.Azure.Cosmos.Emulator.exe| |
|[Ayuda] |Muestra la lista de argumentos de la línea de comandos admitidos.|Microsoft.Azure.Cosmos.Emulator.exe /? | |
| GetStatus |Obtiene el estado del emulador de Azure Cosmos. El estado se indica mediante el código de salida: 1 = iniciado, 2 = en ejecución, 3 = detenido. Un código de salida negativo indica que se ha producido un error. Se produce ninguna otra salida. | Microsoft.Azure.Cosmos.Emulator.exe /GetStatus| |
| Shutdown| Cierra el emulador de Azure Cosmos.| Microsoft.Azure.Cosmos.Emulator.exe /Shutdown | |
|DataPath | Especifica la ruta de acceso en la que se almacenarán los archivos de datos. El valor predeterminado es %LocalAppdata%\CosmosDBEmulator. | Microsoft.Azure.Cosmos.Emulator.exe /DataPath=\<datapath\> | \<datapath\>: una ruta de acceso accesible |
|Port | Especifica el número de puerto que se utilizará para el emulador. El valor predeterminado es 8081. |Microsoft.Azure.Cosmos.Emulator.exe /Port=\<port\> | \<port\>: número de puerto sencillo |
| ComputePort | Especifica el número de puerto que se usará para el servicio Compute Interop Gateway. El puerto de sondeo del punto de conexión HTTP de la puerta de enlace se calcula como ComputePort + 79. Por lo tanto, ComputePort y ComputePort + 79 deben estar abiertos y disponibles. El valor predeterminado es 8900. | Microsoft.Azure.Cosmos.Emulator.exe /ComputePort=\<computeport\> | \<computeport\>: número de puerto sencillo |
| EnableMongoDbEndpoint=3.2 | Habilita la versión 3.2 de MongoDB API | Microsoft.Azure.Cosmos.Emulator.exe /EnableMongoDbEndpoint=3.2 | |
| EnableMongoDbEndpoint=3.6 | Habilita la versión 3.6 de MongoDB API | Microsoft.Azure.Cosmos.Emulator.exe /EnableMongoDbEndpoint=3.6 | |
| MongoPort | Especifica el número de puerto que se utilizará para la API de compatibilidad de MongoDB. El valor predeterminado es 10255. |Microsoft.Azure.Cosmos.Emulator.exe /MongoPort=\<mongoport\>|\<mongoport\>: número de puerto sencillo|
| EnableCassandraEndpoint | Habilita Cassandra API | Microsoft.Azure.Cosmos.Emulator.exe /EnableCassandraEndpoint | |
| CassandraPort | Especifica el número de puerto que se utilizará para el punto de conexión de Cassandra. El valor predeterminado es 10350. | Microsoft.Azure.Cosmos.Emulator.exe /CassandraPort=\<cassandraport\> | \<cassandraport\>: número de puerto sencillo |
| EnableGremlinEndpoint | Habilita Gremlin API | Microsoft.Azure.Cosmos.Emulator.exe /EnableGremlinEndpoint | |
| GremlinPort | Número de puerto que se usará para el punto de conexión de Gremlin. El valor predeterminado es 8901. | Microsoft.Azure.Cosmos.Emulator.exe /GremlinPort=\<port\> | \<port\>: número de puerto sencillo |
|EnableTableEndpoint | Habilita Azure Table API | Microsoft.Azure.Cosmos.Emulator.exe /EnableTableEndpoint | |
|TablePort | Número de puerto que se utilizará para el punto de conexión de Azure Table. El valor predeterminado es 8902. | Microsoft.Azure.Cosmos.Emulator.exe /TablePort=\<port\> | \<port\>: número de puerto sencillo|
| KeyFile | Lee la clave de autorización del archivo especificado. Usa la opción/GenKeyFile para generar un archivo de claves | Microsoft.Azure.Cosmos.Emulator.exe /KeyFile=\<file_name\> | \<file_name\>: Ruta de acceso al archivo |
| ResetDataPath | Quita todos los archivos repetidamente de la ruta de acceso especificada. Si no especifica una ruta de acceso, el valor predeterminado es %LOCALAPPDATA%\CosmosDbEmulator | Microsoft.Azure.Cosmos.Emulator.exe /ResetDataPath=\<path> | \<path\>: Ruta de acceso del archivo  |
| StartTraces  |  Comienza a recopilar los registros de seguimiento de la depuración mediante LOGMAN. | Microsoft.Azure.Cosmos.Emulator.exe /StartTraces | |
| StopTraces     | Deja de recopilar los registros de seguimiento de la depuración mediante LOGMAN. | Microsoft.Azure.Cosmos.Emulator.exe /StopTraces  | |
| StartWprTraces  |  Inicia la recopilación de registros de seguimiento de depuración con la herramienta Windows Performance Recorder. | Microsoft.Azure.Cosmos.Emulator.exe /StartWprTraces | |
| StopWprTraces     | Detiene la recopilación de registros de seguimiento de depuración con la herramienta Windows Performance Recorder. | Microsoft.Azure.Cosmos.Emulator.exe /StopWprTraces  | |
|FailOnSslCertificateNameMismatch | De forma predeterminada, el emulador vuelve a generar su certificado TLS/SSL autofirmado, si los SAN de certificado no incluyen el nombre de dominio del host del emulador, la dirección IPv4 local, "localhost" y "127.0.0.1". Con esta opción, el emulador producirá un error al inicio en su lugar. A continuación, debe usar la opción /GenCert para crear e instalar un nuevo certificado TLS/SSL autofirmado. | Microsoft.Azure.Cosmos.Emulator.exe /FailOnSslCertificateNameMismatch  | |
| GenCert | Genera e instala un nuevo certificado TLS/SSL autofirmado. Puede incluir una lista de nombres DNS adicionales separada por comas para acceder al emulador a través de la red. | Microsoft.Azure.Cosmos.Emulator.exe /GenCert=\<dns-names\> |\<dns-names\>: lista opcional de nombres dns adicionales separada por comas  |
| DirectPorts |Especifica los puertos que se usarán para la conectividad directa. Los valores predeterminados son 10251,10252,10253,10254. | Microsoft.Azure.Cosmos.Emulator.exe /DirectPorts:\<directports\> | \<directports\>: una lista delimitada por comas de 4 puertos |
| Clave |Clave de autorización para el emulador. La clave debe ser la codificación en base 64 de un vector de 64 bytes. | Microsoft.Azure.Cosmos.Emulator.exe /Key:\<key\> | \<key\>: Las claves deben ser la codificación en base 64 de un vector de 64 bytes.|
| EnableRateLimiting | Especifica que el comportamiento de limitación de velocidad de solicitudes está habilitado. |Microsoft.Azure.Cosmos.Emulator.exe /EnableRateLimiting | |
| DisableRateLimiting |Especifica que el comportamiento de limitación de velocidad de solicitudes está deshabilitado. |Microsoft.Azure.Cosmos.Emulator.exe /DisableRateLimiting | |
| NoUI | No muestra la interfaz de usuario del emulador. | Microsoft.Azure.Cosmos.Emulator.exe /NoUI | |
| NoExplorer | No muestra el Explorador de datos en el inicio. |Microsoft.Azure.Cosmos.Emulator.exe /NoExplorer | | 
| PartitionCount | Especifica el número máximo de los contenedores particionados. Consulte [Cambio del número de contenedores](#set-partitioncount) para más información. | Microsoft.Azure.Cosmos.Emulator.exe /PartitionCount=\<partitioncount\> | \<partitioncount\>: Número máximo de contenedores de una sola partición permitidos. El valor predeterminado es de 25. El máximo permitido es 250.|
| DefaultPartitionCount| Especifica el número predeterminado de particiones para un contenedor. | Microsoft.Azure.Cosmos.Emulator.exe /DefaultPartitionCount=\<defaultpartitioncount\> | \<defaultpartitioncount\> El valor predeterminado es 25.|
| AllowNetworkAccess | Permite acceder al emulador a través de una red. También debe pasar /Key=\<key_string\> o /KeyFile=\<file_name\> para habilitar el acceso de red. | Microsoft.Azure.Cosmos.Emulator.exe /AllowNetworkAccess /Key=\<key_string\> o Microsoft.Azure.Cosmos.Emulator.exe /AllowNetworkAccess /KeyFile=\<file_name\>| |
| NoFirewall | No ajuste las reglas de firewall cuando se utilice la opción /AllowNetworkAccess. |Microsoft.Azure.Cosmos.Emulator.exe /NoFirewall | |
| GenKeyFile | Genere una nueva clave de autorización y guárdela en el archivo especificado. La clave generada se puede utilizar con las opciones /Key o /KeyFile. | Microsoft.Azure.Cosmos.Emulator.exe /GenKeyFile=\<path to key file\> | |
| Coherencia | Defina el nivel de coherencia predeterminado de la cuenta. | Microsoft.Azure.Cosmos.Emulator.exe /Consistency=\<consistency\> | \<consistency\>: el valor debe ser de uno de los siguientes [niveles de consistencia](consistency-levels.md): Sesión, Strong, Eventual o BoundedStaleness. El valor predeterminado es Session. |
| ? | Se muestra el mensaje de ayuda.| | |

## <a name="manage-the-emulator-with-powershell"></a>Administración del emulador con PowerShell

El emulador incluye un módulo de PowerShell para iniciar, detener, desinstalar y recuperar el estado del servicio. Ejecute el siguiente cmdlet para utilizar el módulo de PowerShell:

```powershell
Import-Module "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules\Microsoft.Azure.CosmosDB.Emulator"
```

o bien coloque el directorio `PSModules` en su `PSModulesPath` e impórtelo como se muestra en el siguiente comando:

```powershell
$env:PSModulesPath += "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules"
Import-Module Microsoft.Azure.CosmosDB.Emulator
```

Este es un resumen de los comandos de control del emulador desde PowerShell:

### `Get-CosmosDbEmulatorStatus`

**Sintaxis**

`Get-CosmosDbEmulatorStatus`

**Comentarios:**

Devuelve uno de estos valores de ServiceControllerStatus: ServiceControllerStatus.StartPending, ServiceControllerStatus.Running o ServiceControllerStatus.Stopped.

### `Start-CosmosDbEmulator`

**Sintaxis**

`Start-CosmosDbEmulator [-DataPath <string>] [-DefaultPartitionCount <uint16>] [-DirectPort <uint16[]>] [-MongoPort <uint16>] [-NoUI] [-NoWait] [-PartitionCount <uint16>] [-Port <uint16>] [<CommonParameters>]`

**Comentarios:**

Inicia el emulador. De forma predeterminada, el comando espera hasta que el emulador está listo para aceptar solicitudes. Utilice la opción -NoWait, si desea que el cmdlet realice la devolución en cuanto inicie el emulador.

### `Stop-CosmosDbEmulator`

**Sintaxis**

 `Stop-CosmosDbEmulator [-NoWait]`

**Comentarios:**

Detiene el emulador. De forma predeterminada, este comando espera hasta que el emulador esté completamente apagado. Utilice la opción -NoWait, si desea que el cmdlet realice la devolución en cuanto el emulador comience el apagado.

### `Uninstall-CosmosDbEmulator`

**Sintaxis**

`Uninstall-CosmosDbEmulator [-RemoveData]`

**Comentarios:**

Desinstala el emulador y, opcionalmente, quita todo el contenido de $env: LOCALAPPDATA\CosmosDbEmulator.
El cmdlet garantiza que el emulador se detiene antes de desinstalarlo.

## <a name="change-the-number-of-default-containers"></a><a id="set-partitioncount"></a>Cambio del número de contenedores predeterminados

De forma predeterminada se pueden crear hasta 25 contenedores de tamaño fijo (solo compatibles con los SDK de Azure Cosmos DB) o 5 contenedores ilimitados con el emulador de Azure Cosmos. Al modificar el valor de **PartitionCount**, se pueden crear hasta 250 contenedores de tamaño fijo o 50 contenedores ilimitados, o cualquier combinación de los dos que no supere los 250 contenedores de tamaño fijo (un contenedor ilimitado = 5 contenedores de tamaño fijo). Sin embargo, no se recomienda configurar el emulador para que se ejecute con más de 200 contenedores de tamaño fijo. Esto se debe a la sobrecarga añadida a las operaciones de E/S del disco, que producen tiempos de expiración impredecibles al usar las API de punto de conexión.

Si intenta crear un contenedor después de que se haya excedido el recuento de particiones actual, el emulador generará una excepción ServiceUnavailable con el siguiente mensaje.

> Sorry, we are currently experiencing high demand in this region, and cannot fulfill your request at this time. We work continuously to bring more and more capacity online, and encourage you to try again.
> ActivityId: 12345678-1234-1234-1234-123456789abc (Lo sentimos, la demanda actual en esta región es alta y no podemos responder a su solicitud en este momento. Trabajamos constantemente para añadir capacidad en línea, inténtelo de nuevo. ActivityId: 12345678-1234-1234-1234-123456789abc)

Para cambiar el número de contenedores disponibles en el emulador de Azure Cosmos, ejecute los siguientes pasos:

1. Elimine todos los datos del emulador local de Azure Cosmos; para ello, haga clic con el botón derecho en el icono del **Emulador de Azure Cosmos DB** de la bandeja del sistema y haga clic en **Reset Data…** (Restablecer datos...).

1. Elimine todos los datos del emulador de la carpeta `%LOCALAPPDATA%\CosmosDBEmulator`.

1. Cierre todas las instancias abiertas; para ello, haga clic con el botón derecho en el icono del **Emulador de Azure Cosmos DB** de la bandeja del sistema y, luego, haga clic en **Salir**. Todas las instancias pueden tardar un minuto en salir.

1. Instale la versión más reciente del [Emulador de Azure Cosmos](https://aka.ms/cosmosdb-emulator).

1. Para iniciar el emulador con la marca PartitionCount, establezca un valor <= 250. Por ejemplo: `C:\Program Files\Azure Cosmos DB Emulator> Microsoft.Azure.Cosmos.Emulator.exe /PartitionCount=100`.
 
## <a name="next-steps"></a>Pasos siguientes

* [Exportación de los certificados del emulador de Azure Cosmos para su uso con aplicaciones en Java, Python y Node.js](local-emulator-export-ssl-certificates.md)
* [Depuración de problemas con el emulador](troubleshoot-local-emulator.md)
