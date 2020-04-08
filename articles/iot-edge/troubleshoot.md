---
title: 'Solución de problemas: Azure IoT Edge | Microsoft Docs'
description: Use este artículo para obtener información sobre las aptitudes de diagnóstico estándar para Azure IoT Edge, como la recuperación de los registros y el estado del componente, así como la resolución de problemas comunes.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/20/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 13eab175356ed1ec20caa3263ba00d0563384f0e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064390"
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Problemas habituales y soluciones para Azure IoT Edge

Si experimenta problemas al ejecutar Azure IoT Edge en el entorno, use este artículo como guía para solucionarlos.

## <a name="run-the-iotedge-check-command"></a>Ejecutar el comando 'check' de iotedge

El primer paso a la hora de solucionar problemas de IoT Edge debe ser usar el comando `check`, que ejecuta una serie de pruebas de configuración y conectividad para problemas comunes. El comando `check` está disponible en la [versión 1.0.7](https://github.com/Azure/azure-iotedge/releases/tag/1.0.7) y posteriores.

Puede ejecutar el comando `check` como se indica a continuación, o bien incluir la marca `--help` para ver una lista completa de opciones:

* En Linux:

  ```bash
  sudo iotedge check
  ```

* En Windows:

  ```powershell
  iotedge check
  ```

Los tipos de comprobaciones que ejecuta la herramienta se pueden clasificar como:

* Comprobaciones de configuración: examinan detalles que podrían evitar que los dispositivos de Edge se conectaran a la nube, incluidos problemas con *config.yaml* y el motor del contenedor.
* Comprobaciones de conexión: comprueban que el runtime de IoT Edge pueda acceder a los puertos en el dispositivo de host y que todos los componentes de IoT Edge puedan conectarse a IoT Hub.
* Comprobaciones de preparación de producción: buscan procedimientos recomendados de producción, como el estado de los certificados de la entidad de certificación (CA) del dispositivo y la configuración del archivo de registro de módulo.

Para obtener una lista completa de las comprobaciones de diagnóstico, vea [Built-in troubleshooting functionality](https://github.com/Azure/iotedge/blob/master/doc/troubleshoot-checks.md) (Funcionalidad de solución de problemas integrada).

## <a name="gather-debug-information-with-iotedge-support-bundle-command"></a>Recopilación de información de depuración con el comando "support-bundle"

Cuando necesite recopilar registros de un dispositivo IoT Edge, la manera más cómoda es usar el comando `support-bundle`. Este comando recopila de forma predeterminada registros de módulo, de IoT Edge Security Manager y del motor de contenedor, salidas JSON "iotedge check" y otra información de depuración de utilidad y, luego, lo comprime todo en un solo archivo para poder compartirlo fácilmente. El comando `support-bundle` está disponible en la [versión 1.0.9](https://github.com/Azure/azure-iotedge/releases/tag/1.0.9) y posteriores.

Ejecute el comando `support-bundle` con la marca `--since` para especificar hasta qué momento en el pasado deben remontarse los registros. Por ejemplo, con `6h` se obtendrán registros de las últimas 6 horas, con `6d`, de los últimos 6 días, con `6m`, de los últimos 6 minutos, etc. Incluya la marca `--help` para ver una lista completa de opciones.


* En Linux:

  ```bash
  sudo iotedge support-bundle --since 6h
  ```

* En Windows:

  ```powershell
  iotedge support-bundle --since 6h
  ```

> [!WARNING]
> La salida del comando `support-bundle` puede contener nombres de hosts, de dispositivos y de módulos, información registrada por los módulos, etc. Tenga esto en cuenta si comparte la salida en un foro público.

## <a name="standard-diagnostic-steps"></a>Pasos de diagnóstico estándar

Si detecta un problema, puede obtener más información sobre el estado del dispositivo de IoT Edge mediante la revisión de los registros de contenedor y de los mensajes que se envían hacia el dispositivo y desde este. Use los comandos y las herramientas de esta sección para recopilar información.

### <a name="check-the-status-of-the-iot-edge-security-manager-and-its-logs"></a>Comprobación del estado del administrador de seguridad de IoT Edge y sus registros

En Linux:

* Para ver el estado del administrador de seguridad de IoT Edge:

   ```bash
   sudo systemctl status iotedge
   ```

* Para ver los registros del administrador de seguridad de IoT Edge:

    ```bash
    sudo journalctl -u iotedge -f
    ```

* Para ver registros más detallados del administrador de seguridad de IoT Edge:

  * Edite la configuración del demonio de iotedge:

      ```bash
      sudo systemctl edit iotedge.service
      ```

  * Actualice las líneas siguientes:

      ```bash
      [Service]
      Environment=IOTEDGE_LOG=edgelet=debug
      ```

  * Reinicie el demonio de seguridad de IoT Edge:

      ```bash
      sudo systemctl cat iotedge.service
      sudo systemctl daemon-reload
      sudo systemctl restart iotedge
      ```

En Windows:

* Para ver el estado del administrador de seguridad de IoT Edge:

   ```powershell
   Get-Service iotedge
   ```

* Para ver los registros del administrador de seguridad de IoT Edge:

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
   ```

### <a name="if-the-iot-edge-security-manager-is-not-running-verify-your-yaml-configuration-file"></a>Comprobación del archivo de configuración de YAML si no se está ejecutando el administrador de seguridad de IoT Edge

> [!WARNING]
> Los archivos de YAML no pueden contener tabulaciones como sangría. Utilice en su lugar dos espacios. Los elementos de nivel superior no deben tener espacios iniciales.

En Linux:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

En Windows:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

### <a name="check-container-logs-for-issues"></a>Comprobación de los registros del contenedor para detectar problemas

Cuando el demonio de seguridad de IoT Edge se esté ejecutando, examine los registros de los contenedores para detectar problemas. Empiece por los contenedores implementados y, luego, examine los contenedores que componen el runtime de IoT Edge: edgeAgent y edgeHub. Los registros del agente de IoT Edge normalmente proporcionan información sobre el ciclo de vida de cada contenedor. Los registros del centro de IoT Edge proporcionan información sobre mensajería y enrutamiento.

   ```cmd
   iotedge logs <container name>
   ```

### <a name="view-the-messages-going-through-the-iot-edge-hub"></a>Visualización de mensajes que se envían a través del centro de IoT Edge

Puede ver los mensajes que se envían a través del centro de IoT Edge y recopilar la información que se encuentra en los registros detallados procedentes de los contenedores del runtime. Para activar los registros detallados en estos contenedores, establezca `RuntimeLogLevel` en el archivo de configuración yaml. Para abrir el archivo:

En Linux:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

En Windows:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

De forma predeterminada, el elemento `agent` tendrá un aspecto similar al del ejemplo siguiente:

   ```yaml
   agent:
     name: edgeAgent
     type: docker
     env: {}
     config:
       image: mcr.microsoft.com/azureiotedge-agent:1.0
       auth: {}
   ```

Reemplace `env: {}` por:

   ```yaml
   env:
     RuntimeLogLevel: debug
   ```

   > [!WARNING]
   > Los archivos de YAML no pueden contener tabulaciones como sangría. Utilice en su lugar dos espacios. Los elementos de nivel superior no pueden tener espacios en blanco iniciales.

Guarde el archivo y reinicie el administrador de seguridad de IoT Edge.

También puede comprobar los mensajes que se envían entre los dispositivos de IoT Hub e IoT Edge. Vea estos mensajes mediante la extensión de [Azure IoT Hub para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit). Para más información, consulte [Handy tool when you develop with Azure IoT](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/) (Herramienta práctica para desarrollar con Azure IoT).

### <a name="restart-containers"></a>Reinicio de los contenedores

Después de investigar los registros y mensajes para obtener información, puede intentar reiniciar los contenedores:

```cmd
iotedge restart <container name>
```

Reinicie los contenedores del entorno de ejecución de IoT Edge:

```cmd
iotedge restart edgeAgent && iotedge restart edgeHub
```

### <a name="restart-the-iot-edge-security-manager"></a>Reinicio del administrador de seguridad de IoT Edge

Si el problema persiste, puede probar a reiniciar el administrador de seguridad de IoT Edge.

En Linux:

   ```cmd
   sudo systemctl restart iotedge
   ```

En Windows:

   ```powershell
   Stop-Service iotedge -NoWait
   sleep 5
   Start-Service iotedge
   ```

## <a name="iot-edge-agent-stops-after-about-a-minute"></a>El agente de IoT Edge se detiene después de aproximadamente un minuto

El módulo edgeAgent se inicia y se ejecuta correctamente durante un minuto aproximadamente y luego se detiene. Los registros indican que el agente de IoT Edge intenta conectarse a IoT Hub a través de AMQP y después intenta conectarse mediante AMQP a través de WebSocket. Cuando se produce un error, se cierra el agente de IoT Edge.

Registros de ejemplo de edgeAgent:

```output
2017-11-28 18:46:19 [INF] - Starting module management agent.
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5)
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP...
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket...
```

**Causa principal**

Una configuración de redes en la red del host evita que el agente de IoT Edge alcance la red. El agente intentará conectarse en primer lugar a través de AMQP (puerto 5671). Si se produce un error en la conexión, lo intentará mediante los protocolos WebSocket (puerto 443).

El entorno de ejecución de IoT Edge configura una red para cada uno de los módulos en los que se comunica. En Linux, esta red es una red de puente. En Windows, se usa NAT. Este problema es más frecuente en los dispositivos de Windows que usan contenedores de Windows que usan la red NAT.

**Resolución**

Asegúrese de que hay una ruta a Internet para las direcciones IP asignadas a esta red de puente o NAT. A veces, una configuración de VPN en el host invalida la red de IoT Edge.

## <a name="iot-edge-hub-fails-to-start"></a>No se puede iniciar el centro de IoT Edge

El módulo edgeHub no se inicia e imprime el siguiente mensaje en los registros:

```output
One or more errors occurred. 
(Docker API responded with status code=InternalServerError, response=
{\"message\":\"driver failed programming external connectivity on endpoint edgeHub (6a82e5e994bab5187939049684fb64efe07606d2bb8a4cc5655b2a9bad5f8c80): 
Error starting userland proxy: Bind for 0.0.0.0:443 failed: port is already allocated\"}\n)
```

**Causa principal**

Algún otro proceso del equipo host se ha enlazado con el puerto 443. El centro de IoT Edge asigna los puertos 5671 y 443 para su uso en escenarios de puerta de enlace. Se producirá un error en la asignación de puertos si hay algún otro proceso enlazado con este puerto.

**Resolución**

Busque y detenga el proceso que está utilizando el puerto 443. Este proceso suele ser un servidor web.

## <a name="iot-edge-agent-cant-access-a-modules-image-403"></a>El agente de IoT Edge no puede acceder a la imagen de un módulo (403)

No se puede ejecutar un contenedor y los registros de edgeAgent muestran un error 403.

**Causa principal**

El agente de IoT Edge no tiene permisos para acceder a la imagen de un módulo.

**Resolución**

Asegúrese de que las credenciales del registro se hayan especificado correctamente en el manifiesto de implementación.

## <a name="iot-edge-security-daemon-fails-with-an-invalid-hostname"></a>Error de nombre de host no válido del demonio de seguridad de IoT Edge

El comando `sudo journalctl -u iotedge` produce un error e imprime el mensaje siguiente:

```output
Error parsing user input data: invalid hostname. Hostname cannot be empty or greater than 64 characters
```

**Causa principal**

El tiempo de ejecución de IoT Edge solo puede admitir los nombres de host que tienen menos de 64 caracteres. Las máquinas físicas no suelen tener nombres de host largos, pero el problema es más común en una máquina virtual. Los nombres de host generados automáticamente para las máquinas virtuales Windows hospedadas en Azure, en particular, suelen ser largos.

**Resolución**

Cuando vea este error, puede resolverlo configurando el nombre DNS de la máquina virtual y, a continuación, estableciendo el nombre DNS como nombre de host en el comando de configuración.

1. En Azure Portal, navegue a la hoja de introducción de la máquina virtual.
2. Seleccione **configurar** en el nombre DNS. Si la máquina virtual ya tiene configurado un nombre DNS, no es necesario configurar uno nuevo.

   ![Configuración del nombre DNS de la máquina virtual](./media/troubleshoot/configure-dns.png)

3. Proporcione un valor para **Etiqueta de nombre DNS** y seleccione **Guardar**.
4. Copie el nuevo nombre DNS, que debe tener el formato **\<etiquetanombreDNS\>.\< ubicaciónvm\>.cloudapp.azure.com**.
5. Dentro de la máquina virtual, use el comando siguiente para configurar el tiempo de ejecución de IoT Edge con el nombre DNS:

   * En Linux:

      ```bash
      sudo nano /etc/iotedge/config.yaml
      ```

   * En Windows:

      ```cmd
      notepad C:\ProgramData\iotedge\config.yaml
      ```

## <a name="stability-issues-on-resource-constrained-devices"></a>Problemas de estabilidad en dispositivos con recursos limitados

Es posible que encuentre problemas de estabilidad en dispositivos restringidos como Raspberry Pi, especialmente cuando se utiliza como puerta de enlace. Los síntomas incluyen excepciones de memoria insuficiente en el módulo del concentrador perimetral, los dispositivos de nivel inferior no pueden conectarse o el dispositivo deja de enviar mensajes de telemetría después de unas horas.

**Causa principal**

El centro de IoT Edge, que forma parte del runtime de IoT Edge, está optimizado para el rendimiento de manera predeterminada e intenta asignar grandes fragmentos de memoria. Esta optimización no es ideal para dispositivos con perímetro limitado y puede causar problemas de estabilidad.

**Resolución**

En el centro de IoT Edge, establezca una variable de entorno **OptimizeForPerformance** en **false**. Hay dos maneras de establecer variables de entorno:

En Azure Portal:

En el IoT Hub, seleccione el dispositivo de IoT Edge y, en la página Detalles del dispositivo, seleccione **Establecer módulos** > **Configuración de tiempo de ejecución**. Cree una variable de entorno para el módulo Centro de Edge denominada *OptimizeForPerformance* establecida en *false*.

![OptimizeForPerformance establecido en false](./media/troubleshoot/optimizeforperformance-false.png)

**OR**

En el manifiesto de implementación:

```json
  "edgeHub": {
    "type": "docker",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
      "createOptions": <snipped>
    },
    "env": {
      "OptimizeForPerformance": {
          "value": "false"
      }
    },
```

## <a name="cant-get-the-iot-edge-daemon-logs-on-windows"></a>No se obtienen los registros del demonio de IoT Edge en Windows

Si aparece EventLogException al usar`Get-WinEvent` en Windows, compruebe las entradas del registro.

**Causa principal**

El comando de PowerShell `Get-WinEvent` se basa en que esté presente una entrada del Registro para encontrar los registros por un elemento `ProviderName` concreto.

**Resolución**

Establezca una entrada del registro para el demonio de IoT Edge. Cree un archivo **iotedge.reg** con el siguiente contenido e impórtelo en el Registro de Windows haciendo doble clic en él o mediante el comando `reg import iotedge.reg`:

```reg
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
"CustomSource"=dword:00000001
"EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
"TypesSupported"=dword:00000007
```

## <a name="iot-edge-module-fails-to-send-a-message-to-the-edgehub-with-404-error"></a>El módulo de IoT Edge no puede enviar un mensaje a edgeHub y se produce el error 404

El módulo de IoT Edge personalizado no puede enviar un mensaje a edgeHub y se produce el error 404 `Module not found`. El demonio de IoT Edge imprime el mensaje siguiente en los registros:

```output
Error: Time:Thu Jun  4 19:44:58 2018 File:/usr/sdk/src/c/provisioning_client/adapters/hsm_client_http_edge.c Func:on_edge_hsm_http_recv Line:364 executing HTTP request fails, status=404, response_buffer={"message":"Module not found"}u, 04 )
```

**Causa principal**

El demonio de IoT Edge aplica la identificación de proceso para todos los módulos que se conectan a edgeHub por motivos de seguridad. Comprueba que todos los mensajes enviados por un módulo proceden del identificador de proceso principal del módulo. Si un módulo envía un mensaje desde un identificador de proceso diferente del que se estableció inicialmente, se rechazará el mensaje y se generará el mensaje de error 404.

**Resolución**

A partir de la versión 1.0.7, todos los procesos de módulo tienen autorización para conectarse. Si no es posible actualizar a la versión 1.0.7, realice los pasos siguientes. Para más información, consulte el [registro de cambios de la versión v1.1.0.7](https://github.com/Azure/iotedge/blob/master/CHANGELOG.md#iotedged-1).

Asegúrese de que el módulo de IoT Edge personalizado siempre usa el mismo identificador de proceso para enviar los mensajes a edgeHub. Por ejemplo, asegúrese de usar `ENTRYPOINT` en lugar del comando `CMD` en el archivo de Docker, ya que `CMD` da lugar a un identificador de proceso para el módulo y a otro identificador de proceso para el comando bash que ejecuta el programa principal, mientras que `ENTRYPOINT` da lugar a un identificador de proceso único.

## <a name="firewall-and-port-configuration-rules-for-iot-edge-deployment"></a>Reglas de configuración de puertos y firewall para la implementación de IoT Edge

Azure IoT Edge permite la comunicación desde un servidor local a la nube de Azure mediante protocolos de IoT Hub compatibles; vea [Elección de un protocolo de comunicación](../iot-hub/iot-hub-devguide-protocols.md). Para mejorar la seguridad, los canales de comunicación entre Azure IoT Edge y Azure IoT Hub siempre están configurados para que sea la salida. Esta configuración se basa en el [patrón de comunicación asistida de servicios](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/), que minimiza la superficie de ataque que una entidad malintencionada puede explorar. La comunicación entrante solo es necesaria para escenarios específicos donde Azure IoT Hub necesita insertar mensajes en el dispositivo de Azure IoT Edge. Los mensajes de nube a dispositivo están protegidos mediante canales TLS seguros y pueden protegerse aún más mediante los certificados X.509 y los módulos de dispositivos TPM. El Administrador de seguridad de Azure IoT Edge rige cómo se puede establecer esta comunicación; consulte [Administrador de seguridad de IoT Edge](../iot-edge/iot-edge-security-manager.md).

Aunque IoT Edge permite una mejor configuración para proteger el entorno de ejecución de Azure IoT Edge y los módulos implementados, todavía depende de la configuración de la máquina y la red subyacentes. Por lo tanto, es fundamental garantizar que existan reglas adecuadas de red y firewall configuradas para una comunicación segura entre Edge y la nube. La siguiente tabla se puede usar como guía al configurar reglas de firewall para los servidores subyacentes donde se hospeda el runtime de Azure IoT Edge:

|Protocolo|Port|Entrante|Saliente|Guía|
|--|--|--|--|--|
|MQTT|8883|BLOQUEADO (valor predeterminado)|BLOQUEADO (valor predeterminado)|<ul> <li>Configure el valor de Saliente (de salida) para que sea Abierto cuando se usa MQTT como protocolo de comunicación.<li>1883 para MQTT no es compatible con IoT Edge. <li>Se deben bloquear las conexiones entrantes (de entrada).</ul>|
|AMQP|5671|BLOQUEADO (valor predeterminado)|ABIERTO (valor predeterminado)|<ul> <li>Protocolo de comunicación predeterminado de IoT Edge. <li> Debe configurarse para ser Abierto si Azure IoT Edge no está configurado para otros protocolos compatibles o AMQP es el protocolo de comunicación que se desea.<li>5672 para AMQP no es compatible con IoT Edge.<li>Bloquee este puerto cuando Azure IoT Edge use un protocolo compatible de IoT Hub diferente.<li>Se deben bloquear las conexiones entrantes (de entrada).</ul></ul>|
|HTTPS|443|BLOQUEADO (valor predeterminado)|ABIERTO (valor predeterminado)|<ul> <li>Configure el valor de las conexiones salientes (de salida) para que sea abierto en 443 para el aprovisionamiento de IoT Edge. Esta configuración es necesaria cuando se usen scripts manuales o Azure IoT Device Provisioning Service (DPS). <li>La conexión entrante (de entrada) solo debe ser Abierta en escenarios concretos: <ul> <li>  Si tiene una puerta de enlace transparente con dispositivos de hoja que puedan enviar solicitudes de método. En este caso, no es necesario que el puerto 443 esté abierto a las redes externas para conectarse a IOT Hub ni proporcionar servicios de IoTHub mediante Azure IoT Edge. Por lo tanto, la regla de entrada podría limitarse solo a los puertos entrantes (de entrada) abiertos desde la red interna. <li> En el caso de escenarios de cliente a dispositivo (C2D).</ul><li>80 para HTTP no es compatible con IoT Edge.<li>Si no se pueden configurar los protocolos que no sean HTTP (por ejemplo, AMQP o MQTT) en la empresa; los mensajes pueden enviarse a través de WebSockets. En ese caso, el puerto 443 se utilizará para la comunicación de WebSocket.</ul>|

## <a name="edge-agent-module-continually-reports-empty-config-file-and-no-modules-start-on-the-device"></a>El módulo Agente de Edge continuamente notifica "archivo de configuración vacío" y no se inicia ningún módulo en el dispositivo

El dispositivo tiene problemas para iniciar los módulos definidos en la implementación. Solo edgeAgent se está ejecutando, pero continuamente notifica "archivo de configuración vacío..."

**Causa principal**

De forma predeterminada, IoT Edge inicia módulos en su propia red de contenedores aislada. El dispositivo pueda tener problemas con la resolución de nombres DNS dentro de esta red privada.

**Resolución**

**Opción 1: establecer el servidor DNS en la configuración del motor de contenedor**

Especifique el servidor DNS del entorno en la configuración del motor de contenedor que se va a aplicar a todos los módulos de contenedor iniciados por el motor. Cree un archivo denominado `daemon.json` en el que se especifique el servidor DNS que se va a usar. Por ejemplo:

```json
{
    "dns": ["1.1.1.1"]
}
```

El ejemplo anterior establece el servidor DNS en un servicio DNS accesible públicamente. Si el dispositivo de Edge no puede acceder a esta dirección IP desde su entorno, reemplácela por una dirección de servidor DNS que sea accesible.

Coloque `daemon.json` en la ubicación correcta para la plataforma:

| Plataforma | Location |
| --------- | -------- |
| Linux | `/etc/docker` |
| Host de Windows con contenedores de Windows | `C:\ProgramData\iotedge-moby\config` |

Si la ubicación ya contiene el archivo `daemon.json`, agréguele la clave **dns** y guarde el archivo.

Reinicie el motor de contenedor para que las actualizaciones se apliquen.

| Plataforma | Get-Help |
| --------- | -------- |
| Linux | `sudo systemctl restart docker` |
| Windows (Administrador de Powershell) | `Restart-Service iotedge-moby -Force` |

**Opción 2: establecer el servidor DNS en la implementación de IoT Edge por módulo**

Puede establecer el servidor DNS para el elemento *createOptions* de cada módulo en la implementación de IoT Edge. Por ejemplo:

```json
"createOptions": {
  "HostConfig": {
    "Dns": [
      "x.x.x.x"
    ]
  }
}
```

Asegúrese de establecer esta configuración también para los módulos *edgeAgent* y *edgeHub*.

## <a name="next-steps"></a>Pasos siguientes

¿Cree que encontró un error en la plataforma de IoT Edge? [Envíe un problema](https://github.com/Azure/iotedge/issues) para que podamos seguir mejorando.

Si tiene más preguntas, cree una [solicitud de soporte técnico](https://portal.azure.com/#create/Microsoft.Support) para obtener ayuda.
