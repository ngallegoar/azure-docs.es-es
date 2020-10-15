---
title: Solución de problemas al usar el emulador de Azure Cosmos
description: Obtenga información sobre cómo solucionar problemas de servicio no disponible, certificados, cifrado y control de versiones mediante el emulador de Azure Cosmos.
ms.service: cosmos-db
ms.topic: troubleshooting
author: markjbrown
ms.author: mjbrown
ms.date: 09/17/2020
ms.custom: contperfq1
ms.openlocfilehash: af9122aaa0233fe5248f31ffe805e01a98831eae
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91447421"
---
# <a name="troubleshoot-issues-when-using-the-azure-cosmos-emulator"></a>Solución de problemas al usar el emulador de Azure Cosmos

El emulador de Azure Cosmos proporciona un entorno local que emula el servicio Azure Cosmos DB con fines de desarrollo. Use las sugerencias de este artículo como ayuda para solucionar los problemas que encuentre al instalar o usar el emulador de Azure Cosmos. 

Si se instala una nueva versión del emulador y se experimentan errores, asegúrese de restablecer los datos. Para restablecer los datos, haga clic con el botón derecho en el icono del emulador de Azure Cosmos de la bandeja del sistema y haga clic en Restablecer datos… Si no se solucionan los errores, puede desinstalar el emulador y las versiones anteriores de este (si se encuentran), quitar el directorio *C:\Archivos de programa\Azure Cosmos DB Emulator* y volver a instalar el emulador. Consulte [Desinstalación del emulador local](local-emulator.md#uninstall) para obtener instrucciones. Como alternativa, si no funciona el restablecimiento de datos, vaya a la ubicación `%LOCALAPPDATA%\CosmosDBEmulator` y elimine la carpeta.

## <a name="troubleshoot-corrupted-windows-performance-counters"></a>Solución de problemas de contadores de rendimiento de Windows dañados

* Si el emulador de Azure Cosmos se bloquea, recopile los archivos de volcado de memoria de la carpeta `%LOCALAPPDATA%\CrashDumps`, comprímalos y abra una incidencia de soporte técnico en [Azure Portal](https://portal.azure.com).

* Si experimenta bloqueos en `Microsoft.Azure.Cosmos.ComputeServiceStartupEntryPoint.exe`, podría ser un síntoma de donde los contadores de rendimiento tienen un estado dañado. Normalmente, ejecutar el siguiente comando desde un símbolo del sistema de administración corrige el problema:

  ```cmd
  lodctr /R
   ```

## <a name="troubleshoot-connectivity-issues"></a>Solución de problemas de conectividad

* Si se produce un problema de conectividad, [recopile los archivos de seguimiento](#trace-files), comprímalos y abra una incidencia de soporte técnico en [Azure Portal](https://portal.azure.com).

* Si recibe un mensaje de **Servicio no disponible**, es posible que el emulador no pueda inicializar la pila de red. Compruebe si tiene instalado el cliente de Pulse Secure o el cliente de Juniper Networks, ya que sus controladores de filtro de red podrían ser la causa del problema. Normalmente el problema se corrige al desinstalar los controladores del filtro de red de otros fabricantes. Como alternativa, puede iniciar el emulador con /DisableRIO, lo cual cambiará la comunicación de red del emulador a una Winsock normal. 

* Si encuentra problemas de conectividad **"Forbidden","message":"Request is being made with a forbidden encryption in transit protocol or cipher. Check account SSL/TLS minimum allowed protocol setting..."** ("Prohibido","mensaje":"La solicitud se realiza con un cifrado prohibido en el protocolo de tránsito o cifrado. Compruebe la configuración del protocolo SSL/TLS mínimo permitido de la cuenta...), esto puede deberse a cambios globales en el sistema operativo (por ejemplo, Insider Preview, compilación 20170) o a la configuración del explorador que habilita TLS 1.3 como valor predeterminado. Podría producirse un error similar al usar el SDK para ejecutar una solicitud en el emulador de Cosmos, como **Microsoft.Azure.Documents.DocumentClientException: Request is being made with a forbidden encryption in transit protocol or cipher. Check account SSL/TLS minimum allowed protocol setting** (La solicitud se realiza con un cifrado prohibido en el protocolo de tránsito o cifrado. Compruebe la configuración del protocolo SSL/TLS mínimo permitido de la cuenta). Esto es esperable en este momento, ya que el emulador de Cosmos solo acepta y funciona con el protocolo TLS 1.2. La solución recomendada es cambiar la configuración y usar de forma predeterminada TLS 1.2; por ejemplo, en el administrador de IIS, vaya a "Sitios"-> "Sitios web predeterminados" y busque "Enlaces de sitios" para el puerto 8081 y edítelo para deshabilitar TLS 1.3. Se puede realizar una operación similar para el explorador web a través de las opciones de "Configuración".

* Mientras se ejecuta el emulador, si el equipo entra en modo de suspensión o se ejecuta alguna actualización del sistema operativo, es posible que aparezca un mensaje del tipo **el servicio no está disponible en estos momentos**. Restablezca el emulador; para ello, haga clic con el botón derecho en el icono que aparece en la bandeja de notificaciones de Windows y seleccione **Reset Data** (Restablecer datos).

## <a name="collect-trace-files"></a><a id="trace-files"></a>Recopilación de archivos de seguimiento

Para recopilar los seguimientos de depuración, ejecute los siguientes comandos desde un símbolo del sistema administrativo:

1. Navegue hasta la ruta de acceso donde está instalado el emulador:

   ```bash
   cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"
   ```

1. Apague el emulador y vea la bandeja del sistema para asegurarse de que el programa se ha cerrado. Puede tardar hasta un minuto en completarse. También puede seleccionar **Salir** en la interfaz de usuario del emulador de Azure Cosmos.

   ```bash
   Microsoft.Azure.Cosmos.Emulator.exe /shutdown
   ```

1. Inicie el registro con el siguiente comando:

   ```bash
   Microsoft.Azure.Cosmos.Emulator.exe /startwprtraces
   ```

1. Inicie el emulador.

   ```bash
   Microsoft.Azure.Cosmos.Emulator.exe
   ```

1. Reproduzca el problema. Si el Explorador de datos no funciona, solo necesitará esperar a que el explorador se abra durante unos segundos para detectar el error.

1. Detenga el registro con el siguiente comando:

   ```bash
   Microsoft.Azure.Cosmos.Emulator.exe /stopwprtraces
   ```
   
1. Vaya a la ruta de acceso `%ProgramFiles%\Azure Cosmos DB Emulator` y busque el archivo *docdbemulator_000001.etl*.

1. Abra una incidencia de soporte técnico en [Azure Portal](https://portal.azure.com) e incluya el archivo .etl junto con los pasos de reproducción.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a depurar problemas con el emulador local. Ahora puede avanzar a los siguientes artículos:

* [Exportación de los certificados del emulador de Azure Cosmos para su uso con aplicaciones en Java, Python y Node.js](local-emulator-export-ssl-certificates.md)
* [Uso de parámetros de línea de comandos y comandos de PowerShell para controlar el emulador](emulator-command-line-parameters.md)
