---
title: Solución de problemas de Live Video Analytics on IoT Edge - Azure
description: En este artículo se describen los pasos de solución de problemas para Live Video Analytics on IoT Edge.
author: IngridAtMicrosoft
ms.topic: how-to
ms.author: inhenkel
ms.date: 05/24/2020
ms.openlocfilehash: dd55050521a1791a11f220cd5617d9df2fa2d160
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86045590"
---
# <a name="troubleshoot-live-video-analytics-on-iot-edge"></a>Solución de problemas de Live Video Analytics on IoT Edge

En este artículo se describen los pasos de solución de problemas para Live Video Analytics on IoT Edge.

## <a name="troubleshoot-deployment-issues"></a>Solución de problemas de implementación

### <a name="diagnostics"></a>Diagnóstico

Como parte de la implementación de Live Video Analytics, configurará recursos de Azure como IoT Hub y el dispositivo IoT Edge. Como primer paso para diagnosticar problemas, siga estas instrucciones para asegurarse de que el borde esté bien configurado.

1. [Ejecución del comando "check"](https://docs.microsoft.com/azure/iot-edge/troubleshoot#run-the-check-command)
1. [Comprobación de la versión de IoT Edge](https://docs.microsoft.com/azure/iot-edge/troubleshoot#check-your-iot-edge-version)
1. [Comprobación del estado del administrador de seguridad de IoT Edge y sus registros](https://docs.microsoft.com/azure/iot-edge/troubleshoot#check-the-status-of-the-iot-edge-security-manager-and-its-logs)
1. [Visualización de mensajes que se envían a través del centro de IoT Edge](https://docs.microsoft.com/azure/iot-edge/troubleshoot#view-the-messages-going-through-the-iot-edge-hub)
1. [Reinicio de los contenedores](https://docs.microsoft.com/azure/iot-edge/troubleshoot#restart-containers)
1. [Comprobación de las reglas de configuración de los puertos y el firewall](https://docs.microsoft.com/azure/iot-edge/troubleshoot#check-your-firewall-and-port-configuration-rules)

### <a name="pre-deployment-issues"></a>Problemas anteriores a la implementación

Si la infraestructura perimetral está bien, puede comprobar si existen problemas con el archivo de manifiesto de implementación. Para implementar el módulo Live Video Analytics on IoT Edge en el dispositivo perimetral junto con cualquier otro módulo de IoT, usará un manifiesto de implementación que contenga el centro perimetral, el agente perimetral y otros módulos con sus propiedades. Si el código JSON no tiene un formato correcto, puede que reciba un error como el siguiente: 

```
az iot edge set-modules --hub-name <iot-hub-name> --device-id lva-sample-device --content <path-to-deployment_manifest.json>
```

No se pudo analizar el código JSON del archivo: "<deployment manifest.json>" para el argumento "content" con la excepción: "Datos adicionales: línea 101, columna 1 (char 5325)"

Si se produce este error, se recomienda comprobar en el archivo JSON si faltan corchetes o existe algún otro problema con la estructura del archivo. Puede usar un cliente como [Notepad++ con el complemento del visor de JSON](https://riptutorial.com/notepadplusplus/example/18201/json-viewer) o una herramienta en línea como https://jsonformatter.curiousconcept.com/ para validar la estructura de archivos.

### <a name="deployment--diagnose-with-media-graph-direct-methods"></a>Implementación: diagnóstico con los métodos directos de grafo multimedia 

Una vez implementado correctamente el módulo Live Video Analytics on IoT Edge en el dispositivo perimetral, puede crear y ejecutar el grafo multimedia invocando [métodos directos](direct-methods.md). Puede usar el portal para ejecutar el diagnóstico del grafo multimedia mediante métodos directos:

1. Desde el portal, vaya a la instancia de IoT Hub conectada al dispositivo perimetral.
    1. En la hoja de IoT Hub, busque Administración automática de dispositivos->IoT Edge.
    1. Al hacer clic en IoT Edge, debería abrirse una lista de dispositivos perimetrales. Elija el dispositivo que quiera diagnosticar.
         
        ![Dispositivos perimetrales](./media/troubleshoot-how-to/lva-sample-device.png)
    1. Compruebe si el código de respuesta es 200-OK. Existen otros códigos de respuesta para el [entorno de ejecución de IoT Edge](https://docs.microsoft.com/azure/iot-edge/iot-edge-runtime), como los siguientes:
        1. 400 - La configuración de implementación tiene un formato incorrecto o no es válida.
        1. 417 - El dispositivo no tiene un conjunto de configuración de implementación.
        1. 412 - La versión del esquema de la configuración de implementación no es válida.
        1. 406 - El dispositivo de IoT Edge está sin conexión o no envía informes de estado.
        1. 500 - Error en el entorno en tiempo de ejecución de Azure IoT Edge.
    1. Al hacer clic en el dispositivo, también se mostrará la lista de módulos de IoT Edge esperados que se han implementado y su estado.
    1. Si las columnas "Especificado en la implementación" y "Notificado por el dispositivo" indican "Sí", puede invocar métodos directos en el módulo Live Video Analytics on IoT Edge. Haga clic en el módulo y le dirigirá a una pantalla en la que puede comprobar las propiedades deseadas y notificadas, así como invocar métodos directos. 
        1. La comprobación de las propiedades notificadas y deseadas le ayuda a entender si las propiedades del módulo se han sincronizado con la implementación. Si no lo han hecho, puede reiniciar el borde. 
        1. Use la guía de [métodos directos](direct-methods.md) para invocar algunos métodos, especialmente los sencillos, como GraphTopologyList. En la guía también se especifican las cargas de solicitud y respuesta esperadas, así como los códigos de error. Si los métodos directos sencillos funcionan correctamente, puede estar seguro de que el módulo perimetral Live Video Analytics está operativo.
        
        ![Método directo](./media/troubleshoot-how-to/direct-method.png) 
1. Si recibe un código de estado 501, compruebe si el nombre del método directo es preciso. Si el nombre del método y la carga útil de la solicitud son precisos, debería obtener resultados junto con el código 200 de ejecución correcta. Si la carga de la solicitud es incorrecta, obtendrá un estado 400 y una carga de respuesta que indica el código de error, así como un mensaje que debería resultar útil para diagnosticar el problema con la llamada de método directo. 

### <a name="post-deployment--diagnose-logs-for-issues-during-run"></a>Después de la implementación: diagnóstico de registros para problemas durante la ejecución 

Los registros de contenedor para el módulo perimetral deben tener información<!--<todo:add link to diagnostics doc>--> de diagnóstico que ayude a depurar los problemas en el tiempo de ejecución del módulo. No obstante, puede [comprobar si hay problemas en los registros de contenedores](https://docs.microsoft.com/azure/iot-edge/troubleshoot#check-container-logs-for-issues) y el diagnóstico automático. Si se han realizado todas las comprobaciones anteriores y sigue teniendo problemas, recopile los registros del dispositivo IoT Edge [con el comando "support bundle"](https://docs.microsoft.com/azure/iot-edge/troubleshoot#gather-debug-information-with-support-bundle-command) para que el equipo de Azure los pueda analizar en profundidad. Puede [ponerse en contacto](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) con nosotros para obtener soporte técnico y enviar los registros recopilados.

## <a name="common-error-resolutions"></a>Resoluciones de errores comunes

Live Video Analytics se implementa como módulo de IoT Edge en el dispositivo perimetral y funciona en colaboración con el agente de IoT Edge y los módulos del centro. Algunos de los errores comunes que se encontrará con la implementación de Live Video Analytics se deben a los problemas con la infraestructura de IoT subyacente. Algunos errores comunes que pueden tener el centro y el agente de IoT Edge son:

1. [El agente de IoT Edge se detiene después de, aproximadamente, un minuto](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#iot-edge-agent-stops-after-about-a-minute).
1. [El agente de IoT Edge no puede acceder a la imagen de un módulo (403)](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#iot-edge-agent-cant-access-a-modules-image-403).
1. [El módulo Agente de Edge notifica "archivo de configuración vacío" y no se inicia ningún módulo en el dispositivo](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#edge-agent-module-reports-empty-config-file-and-no-modules-start-on-the-device).
1. [No se puede iniciar el centro de IoT Edge](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#iot-edge-hub-fails-to-start).
1. [Error de nombre de host no válido del demonio de seguridad de IoT Edge](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#iot-edge-security-daemon-fails-with-an-invalid-hostname).
1. [Live Video Analytics o cualquier otro módulo de IoT Edge personalizado no puede enviar un mensaje al centro de IoT Edge con el error 404](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#iot-edge-module-fails-to-send-a-message-to-edgehub-with-404-error).
1. [El módulo de IoT Edge se implementa correctamente y, a continuación, desaparece del dispositivo](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#iot-edge-module-deploys-successfully-then-disappears-from-device).

### <a name="edge-set-up-script-issues"></a>Problemas de scripts de configuración de Edge

Como parte de la documentación, hemos proporcionado un [script de configuración](https://github.com/Azure/live-video-analytics/tree/master/edge/setup) para implementar los recursos perimetrales y en la nube para empezar a usar el borde de Live Video Analytics. En esta sección, hemos capturado errores que podría encontrarse con el script y cómo depurarlos.

El script se ejecuta parcialmente mediante la creación de algunos recursos, pero devuelve un error con el siguiente mensaje:

```
registering device...

Unable to load extension 'eventgrid: unrecognized kwargs: ['min_profile']'. Use --debug for more information.
The command failed with an unexpected error. Here is the traceback:

No module named 'azure.mgmt.iothub.iot_hub_client'
Traceback (most recent call last):
File "/opt/az/lib/python3.6/site-packages/knack/cli.py", line 215, in invoke
  cmd_result = self.invocation.execute(args)
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/commands/__init__.py", line 631, in execute
  raise ex
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/commands/__init__.py", line 695, in _run_jobs_serially
  results.append(self._run_job(expanded_arg, cmd_copy))
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/commands/__init__.py", line 688, in _run_job
  six.reraise(*sys.exc_info())
File "/opt/az/lib/python3.6/site-packages/six.py", line 693, in reraise
  raise value
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/commands/__init__.py", line 665, in _run_job
  result = cmd_copy(params)
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/commands/__init__.py", line 324, in __call__
  return self.handler(*args, **kwargs)
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/__init__.py", line 574, in default_command_handler
  return op(**command_args)
File "/home/.azure/cliextensions/azure-cli-iot-ext/azext_iot/operations/hub.py", line 75, in iot_device_list
  result = iot_query(cmd, query, hub_name, top, resource_group_name, login=login)
File "/home/.azure/cliextensions/azure-cli-iot-ext/azext_iot/operations/hub.py", line 45, in iot_query
  target = get_iot_hub_connection_string(cmd, hub_name, resource_group_name, login=login)
File "/home/.azure/cliextensions/azure-cli-iot-ext/azext_iot/common/_azure.py", line 112, in get_iot_hub_connection_string
  client = iot_hub_service_factory(cmd.cli_ctx)
File "/home/.azure/cliextensions/azure-cli-iot-ext/azext_iot/_factory.py", line 28, in iot_hub_service_factory
  from azure.mgmt.iothub.iot_hub_client import IotHubClient
ModuleNotFoundError: No module named 'azure.mgmt.iothub.iot_hub_client'
```
    
Para corregir este problema:

1. Ejecute el siguiente comando:

    ```
    az --version
    ```
1. Asegúrese de que tiene instaladas las siguientes extensiones. En el momento en que se escribía esta guía, la versión de las extensiones era la siguiente:

    | Extensión | Versión |
    |---|---|
    |azure-cli   |      2.5.1*|
    |command-modules-nspkg         |   2.0.3|
    |core  |    2.5.1*|
    |nspkg    | 3.0.4|
    |telemetry| 1.0.4|
    |Extensiones:    ||
    |storage-preview          |     0.2.10|
    |azure-cli-iot-ext          |    0.8.9|
    |eventgrid| 0.4.9|
    |azure-iot                       | 0.9.2|
1. Si alguna de las extensiones es anterior a los números de versión anteriores, actualícela a la versión más reciente con el comando:

    ```
    az extension update --name <Extension name>
    ```

    Por ejemplo: `az extension update --name azure-iot`

### <a name="sample-app-issues"></a>Problemas con la aplicación de muestra

Como parte de nuestra versión, hemos proporcionado cierto código de ejemplo en .NET para impulsar el arranque de la comunidad de desarrolladores. En esta sección, hemos capturado errores que podría encontrarse al ejecutar el código de ejemplo y cómo depurarlos.

1. Program.cs devuelve el siguiente error en la invocación del método directo:

    ```
    Unhandled exception. Microsoft.Azure.Devices.Common.Exceptions.UnauthorizedException: {"Message":"{\"errorCode\":401002,\"trackingId\":\"b1da85801b2e4faf951a2291a2c467c3-G:32-TimeStamp:04/06/2020 17:15:11\",\"message\":\"Unauthorized\",\"timestampUtc\":\"2020-04-06T17:15:11.6990676Z\"}","ExceptionMessage":""}
    
        at Microsoft.Azure.Devices.HttpClientHelper.ExecuteAsync(HttpClient httpClient, HttpMethod httpMethod, Uri requestUri, Func`3 modifyRequestMessageAsync, Func`2 isMappedToException, Func`3 processResponseMessageAsync, IDictionary`2 errorMappingOverrides, CancellationToken cancellationToken)
    
        at Microsoft.Azure.Devices.HttpClientHelper.ExecuteAsync(HttpMethod httpMethod, Uri requestUri, Func`3 modifyRequestMessageAsync, Func`3 processResponseMessageAsync, IDictionary`2 errorMappingOverrides, CancellationToken cancellationToken)
        
        at Microsoft.Azure.Devices.HttpClientHelper.PostAsync[T,T2](Uri requestUri, T entity, TimeSpan operationTimeout, IDictionary`2 errorMappingOverrides, IDictionary`2 customHeaders, CancellationToken cancellationToken)…
    ```

    1. Asegúrese de que dispone de las [herramientas de IoT de Azure](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) instaladas en el entorno de VS Code y la conexión con el programa de instalación de IoT Hub. (Presione Ctrl + Mayús + P y, a continuación, elija Select IoT Hub method para conectarse a su suscripción e IoT Hub).
1. Compruebe si puede invocar un método directo en el módulo perimetral mediante VS Code (por ejemplo, llame a GraphToplogyList con la siguiente carga { "@apiVersion": "1.0"}). Debería obtener la siguiente respuesta. 

    ```
    {
      "status": 200,
      "payload": {
        "values": [
          {…
    …}
          ]
        }
    }
    ```

    ![Visual Studio Code](./media/troubleshoot-how-to/visual-studio-code1.png)
1. Si se produce un error en el paso anterior, pruebe lo siguiente:
    1. Vaya al símbolo del sistema del dispositivo perimetral y escriba.
    
    ```
    sudo systemctl restart iotedge
    ```

    Se reiniciará el dispositivo perimetral y todos los módulos. Espere unos minutos y ejecute lo siguiente para confirmar que los módulos se están ejecutando antes de volver a intentar usar DirectMethod.

    ```
    sudo iotedge list
    ```
    1. Si también se produce un error, pruebe a arrancar la VM o la máquina.
    1. Si se produce un error en todo, ejecute lo siguiente para obtener un archivo ZIP con todos los [registros relevantes](https://docs.microsoft.com/azure/iot-edge/troubleshoot#gather-debug-information-with-support-bundle-command) que se adjuntarán a la [incidencia de soporte técnico](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

    ```
    sudo iotedge support-bundle --since 2h
    ```
1. Si recibe un código 400 de respuesta de error, compruebe que la carga de invocación del método tenga un formato correcto según la guía [método directo](direct-methods.md).
1. Si obtiene el código de estado 200, indica que el centro funciona bien y que la implementación del módulo es correcta y responde. El siguiente paso consiste en comprobar si las configuraciones de la aplicación son precisas. La configuración de la aplicación consta de los siguientes campos en el archivo appsettings.json. Compruebe que los valores de deviceId y moduleId sean precisos. Una manera sencilla de comprobarlo es mediante la extensión de Azure IoT Hub de VSCode. Los valores del archivo appsettings.json y la sección de IoT Hub deben coincidir.
    
    ```
    {
        "IoThubConnectionString" : 
        "deviceId" : 
        "moduleId" : 
    }
    ```

    ![IOT HUB](./media/troubleshoot-how-to/iot-hub.png)

1. Por último, asegúrese de que, en appsettings.json, ha proporcionado la cadena de conexión de IoT Hub y no la cadena de conexión del dispositivo IoT Hub, ya que sus [formatos](https://devblogs.microsoft.com/iotdev/understand-different-connection-strings-in-azure-iot-hub/) son diferentes.

### <a name="live-video-analytics-working-with-external-modules"></a>Live Video Analytics trabajando con módulos externos

Live Video Analytics, mediante el procesador de extensiones HTTP, puede ampliar el grafo multimedia para enviar y recibir datos de otros módulos de IoT Edge por HTTP con REST.  Como [ejemplo específico](https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/httpExtension), el grafo multimedia puede enviar fotogramas de vídeo como imágenes a un módulo de inferencia externo, como Yolo v3, y recibir resultados de análisis basados en JSON. En esta topología, el destino final de los eventos es, principalmente, IoT Hub. En situaciones en las que no vea los eventos de inferencia en el centro, compruebe lo siguiente:

1. Compruebe si el centro en el que publica el grafo multimedia es el mismo que está examinando. A veces, al crear varias implementaciones, termina con muchos centros y podría estar comprobando el centro incorrecto para los eventos.
1. Compruebe mediante VS Code si el módulo externo está implementado y en ejecución. En la imagen de ejemplo que se muestra aquí, rtspsim y cv son módulos de IoT Edge que se ejecutan fuera del módulo lvaEdge.

    ![IOT HUB](./media/troubleshoot-how-to/iot-hub.png)
1. Compruebe que está enviando eventos al punto de conexión de URL correcto. El contenedor de IA externo expone una dirección URL y un puerto a través del cual recibe y devuelve los datos de las solicitudes POST. Esta dirección URL se especifica como un punto de conexión: una propiedad URL para el procesador de extensión HTTP. Como se aprecia en la [dirección URL de la topología](https://github.com/Azure/live-video-analytics/blob/master/MediaGraph/topologies/httpExtension/topology.json), se establece en el parámetro URL de inferencia. Asegúrese de que el valor predeterminado del parámetro (http://yolov3/score) o el valor pasado es preciso y pruebe si funciona con cURL.  
    1. Por ejemplo, el contenedor de yolo v3 se ejecuta en la máquina local y la dirección IP del contenedor es 172.17.0.3 (use docker inspect para buscar la dirección IP).

    ```
    curl -X POST http://172.17.0.3/score -H "Content-Type: image/jpeg" --data-binary @<fullpath to jpg>
    ```

    Resultado que se devuelve:

    ```
    {"inferences": [{"type": "entity", "entity": {"tag": {"value": "car", "confidence": 0.8668569922447205}, "box": {"l": 0.3853073438008626, "t": 0.6063712999658677, "w": 0.04174524943033854, "h": 0.02989496027381675}}}]}
    ```

1. Si está ejecutando una o varias instancias de un grafo que usa el procesador de extensiones HTTP, debe tener un filtro de velocidad de fotogramas antes de cada procesador de extensión HTTP para administrar los fotogramas por segundo (FPS) de la fuente de vídeo. En determinadas situaciones en las que la CPU o la memoria de la máquina perimetral tienen un uso muy elevado, puede perder ciertos eventos de inferencia. Para solucionar este problema, establezca un valor bajo para la propiedad maximumFps en el filtro de velocidad de fotogramas. Puede establecerlo en 0,5 ("maximumFps": 0.5) en cada instancia del grafo y volver a ejecutar para obtener los eventos de inferencia del centro.
    1. Como alternativa, puede obtener una máquina perimetral más eficaz con una mayor CPU y memoria.
    
### <a name="multiple-direct-methods-in-parallel--timeout-failure"></a>Varios métodos directos en paralelo: error de tiempo de espera 

Live Video Analytics on IoT Edge proporciona un modelo de programación basado en métodos directos que permite configurar varias topologías y varias instancias de grafos. Como parte de la configuración de la topología y el grafo, se invocarán varias llamadas a métodos directos en el módulo perimetral. Si invoca estas llamadas a métodos múltiples, especialmente las que inician y detienen los grafos, en paralelo, puede experimentar algunos errores de tiempo de espera, como los siguientes. 

Assembly Initialization method Microsoft.Media.LiveVideoAnalytics.Test.Feature.Edge.AssemblyInitializer.InitializeAssemblyAsync threw exception. Microsoft.Azure.Devices.Common.Exceptions.IotHubException: Microsoft.Azure.Devices.Common.Exceptions.IotHubException:<br/> `{"Message":"{\"errorCode\":504101,\"trackingId\":\"55b1d7845498428593c2738d94442607-G:32-TimeStamp:05/15/2020 20:43:10-G:10-TimeStamp:05/15/2020 20:43:10\",\"message\":\"Timed out waiting for the response from device.\",\"info\":{},\"timestampUtc\":\"2020-05-15T20:43:10.3899553Z\"}","ExceptionMessage":""}. Aborting test execution. `

Es recomendable no llamar a métodos directos en paralelo, sino de manera secuencial, es decir, realizar una llamada al método directo cuando finalice la anterior.

### <a name="collecting-logs-for-submitting-a-support-ticket"></a>Recopilación de registros para enviar una incidencia de soporte técnico

Cuando los pasos de solución de problemas autoguiados no resuelven los problemas, debe ir a Azure Portal y [abrir una incidencia de soporte técnico](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request).

Siga los pasos siguientes para recopilar los registros pertinentes que se deben agregar a la incidencia. Podrá cargar los archivos de registro en la pestaña **Detalles** de la solicitud de soporte técnico.

### <a name="support-bundle"></a>Support-bundle

Cuando tenga que recopilar registros de un dispositivo IoT Edge, la manera más sencilla es usar el comando `support-bundle`. Este comando recopila:

- Registros del módulo
- Administrador de seguridad de IoT Edge y registros del motor de contenedor
- Salida JSON iotedge check
- Información de depuración útil

#### <a name="use-the-iot-edge-security-manager"></a>Uso del administrador de seguridad de IoT Edge
 
El administrador de seguridad de IoT Edge es responsable de operaciones como la inicialización del sistema de IoT Edge en los dispositivos de inicio y aprovisionamiento. Si IoT Edge no se inicia, los registros del administrador de seguridad pueden proporcionar información útil. Para ver registros más detallados del administrador de seguridad de IoT Edge:

1. Edite la configuración del demonio de IoT Edge en el dispositivo IoT Edge:

    ```
    sudo systemctl edit iotedge.service
    ```

1. Actualice las líneas siguientes:

    ```
    [Service]
    Environment=IOTEDGE_LOG=edgelet=debug
    ```

1. Reinicie el demonio de seguridad de IoT Edge ejecutando estos comandos:

    ```
    sudo systemctl cat iotedge.service
    sudo systemctl daemon-reload
    sudo systemctl restart iotedge
    ```

1. Ejecute el comando `support-bundle` con la marca --since para especificar hasta qué momento en el pasado deben remontarse los registros. Por ejemplo, 2 h obtendrá registros desde las últimas dos horas. Puede cambiar el valor de esta marca para incluir los registros de un período diferente.

    ```
    sudo iotedge support-bundle --since 2h
    ```

### <a name="lva-debug-logs"></a>Registros de depuración de LVA

Siga estos pasos para configurar LVA en el módulo de IoT Edge para generar registros de depuración:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a IoT Hub.
1. Seleccione **IoT Edge** en el menú.
1. Haga clic en el identificador del dispositivo de destino en la lista de dispositivos.
1. Haga clic en el vínculo **Establecer módulos** del menú superior.

  ![establecer módulos azure portal](media/troubleshoot-how-to/set-modules.png)

5. En la sección Módulos de IoT Edge, busque y haga clic en **lvaEdge**.
1. Haga clic en **Opciones de creación del contenedor**.
1. En la sección Enlaces, agregue el comando siguiente:

    `/var/local/mediaservices/logs:/var/lib/azuremediaservices/logs`

    esto enlaza las carpetas de registros entre el dispositivo perimetral y el contenedor.

1. Haga clic en el botón **Actualizar**.
1. Haga clic en el botón **Revisar y crear** de la parte inferior de la página. Una validación sencilla se llevará a cabo y publicará un mensaje de validación correcta bajo un banner verde.
1. Haga clic en el botón **Crear**.
1. A continuación, actualice la **Identidad de módulo gemela** para apuntar el parámetro DebugLogsDirectory a fin de seleccionar el directorio en el que se recopilarán los registros:
    1. Seleccione **lvaEdge** en la tabla **Módulos**.
    1. Haga clic en el vínculo **Identidad de módulo gemela**. Lo encontrará en la parte superior de la página. De este modo se abrirá un panel editable.
    1. Agregue el siguiente par clave-valor en la **clave que desee**:

        `"DebugLogsDirectory": "/var/lib/azuremediaservices/logs"`

    1. Haga clic en **Guardar**.

1. Reproduzca el problema.
1. Conéctese a la máquina virtual desde la página IoT Hub del portal.
1. Vaya a la carpeta `/var/local/mediaservices/logs` y comprima el contenido binario de esta carpeta y compártalo con nosotros (estos archivos de registro no están diseñados para el autodiagnóstico, sino para que la ingeniería de Azure analice sus problemas).

1. La recopilación de registros se puede detener estableciendo ese valor en **Identidad de módulo gemela** en *NULL*. Vuelva a la página **Identidad de módulo gemela** y actualice los siguientes parámetros como:

    `"DebugLogsDirectory": ""`

## <a name="next-steps"></a>Pasos siguientes

[Tutorial: Grabación de vídeo basada en eventos en la nube y reproducción desde la nube](event-based-video-recording-tutorial.md)
