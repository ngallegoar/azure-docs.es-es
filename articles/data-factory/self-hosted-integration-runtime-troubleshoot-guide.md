---
title: Solución de problemas de un entorno de ejecución de integración autohospedado en Azure Data Factory
description: Conozca cómo solucionar problemas del entorno de ejecución de integración autohospedado en Azure Data Factory.
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 07/19/2020
ms.author: abnarain
ms.openlocfilehash: 521756081db938e749849e6f3630dbd60700d24f
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "87023893"
---
# <a name="troubleshoot-self-hosted-integration-runtime"></a>Solución de problemas del entorno de ejecución de integración autohospedado

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se exploran los métodos comunes de solución de problemas del entorno de ejecución de integración autohospedado en Azure Data Factory.

## <a name="gather-self-hosted-integration-runtime-logs-from-azure-data-factory"></a>Recopilación de registros del entorno de ejecución de integración autohospedado en Azure Data Factory

En el caso de las actividades con error que se ejecutan en IR autohospedado o IR compartido, Azure Data Factory admite la visualización y carga de registros de errores. Puede seguir los pasos a continuación para obtener el identificador del informe de errores y, a continuación, escribir el identificador del informe para buscar problemas conocidos relacionados.

1. Vaya a la página **Ejecuciones de actividad**.

1. En la columna **ERROR**, haga clic en el botón siguiente.

    ![Página Ejecuciones de actividad](media/self-hosted-integration-runtime-troubleshoot-guide/activity-runs-page.png)

1. Verá registros relacionados para la ejecución de actividad con errores. Haga clic en el botón **Enviar registros** para obtener más ayuda.

    ![Enviar registros](media/self-hosted-integration-runtime-troubleshoot-guide/send-logs.png)

1. Puede elegir los registros que desea enviar. Par *IR autohospedado*, puede cargar los registros relacionados con la actividad con errores o todos los registros del nodo de IR autohospedado. Para *IR compartido*, solo puede cargar los registros relacionados con la actividad con errores.

    ![Elección de registros](media/self-hosted-integration-runtime-troubleshoot-guide/choose-logs.png)

1. Cuando se carguen los registros, conserve un registro del identificador del informe si necesita ayuda adicional para resolver el problema.

    ![Carga de registros](media/self-hosted-integration-runtime-troubleshoot-guide/upload-logs.png)

> [!NOTE]
> Las solicitudes de visualización y carga de registros se ejecutarán en todas las instancias de IR autohospedado en línea. Asegúrese de que todas las instancias de IR autohospedado estén en línea en caso de que falten registros. 


## <a name="common-errors-and-resolutions"></a>Errores comunes y soluciones

### <a name="error-message"></a>Mensaje de error: 

`Self-hosted integration runtime can't connect to cloud service`

![Problema de conexión de IR autohospedado](media/self-hosted-integration-runtime-troubleshoot-guide/unable-to-connect-to-cloud-service.png)

#### <a name="cause"></a>Causa 

El entorno de ejecución de integración autohospedado no puede conectarse al servicio Data Factory (back-end). Este problema se debe normalmente a la configuración de red en el firewall.

#### <a name="resolution"></a>Resolución

1. Compruebe si el servicio de entorno de ejecución de integración está en ejecución.
    
   ![Estado de ejecución del servicio IR autohospedado](media/self-hosted-integration-runtime-troubleshoot-guide/integration-runtime-service-running-status.png)
    
1. Si el servicio se está ejecutando, vaya al paso 3.

1. Si no hay ningún proxy configurado en el entorno de ejecución de integración autohospedado (la configuración predeterminada), ejecute el siguiente comando de PowerShell en la máquina donde está instalado el entorno de ejecución de integración autohospedado:

    ```powershell
    (New-Object System.Net.WebClient).DownloadString("https://wu2.frontend.clouddatahub.net/")
    ```
        
   > [!NOTE]     
   > La dirección URL del servicio puede variar en función de la ubicación de Data Factory. Puede encontrar la dirección URL del servicio en **ADF UI** > **Connections** > **Integration runtimes** > **Edit Self-hosted IR** > **Nodes** > **View Service URLs** (Interfaz de usuario de ADF > Conexiones > Entornos de ejecución de integración > Editar IR autohospedado > Nodos > Ver direcciones URL del servicio).
            
    La respuesta esperada es la siguiente:
            
    ![Respuesta del comando de PowerShell](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)
            
1. Si no recibe la respuesta esperada, use uno de los métodos siguientes, según corresponda a su situación:
            
    * Si recibe el mensaje "No se pudo resolver el nombre remoto", hay un problema con el sistema de nombres de dominio (DNS). Póngase en contacto con su equipo de redes para solucionar este problema.
    * Si recibe el error "El certificado SSL/TLS no es de confianza", compruebe si el certificado de https://wu2.frontend.clouddatahub.net/ es de confianza en la máquina y luego instale el certificado público mediante el Administrador de certificados. Esta acción debería mitigar el problema.
    * Vaya **Windows** > **Visor de eventos (registros)**  > **Registros de aplicaciones y servicios** > **Integration Runtime** y compruebe si hay errores causados por DNS, una regla de firewall o la configuración de red de la empresa. (Si encuentra este tipo de error, fuerce el cierre de la conexión). Dado que cada compañía tiene una configuración de red personalizada, póngase en contacto con su equipo de red para solucionar estos problemas.

1. Si el proxy se ha configurado en el entorno de ejecución de integración autohospedado, compruebe si el servidor proxy puede acceder al punto de conexión de servicio. Para ver un comando de ejemplo, consulte [PowerShell, solicitudes web y servidores proxy](https://stackoverflow.com/questions/571429/powershell-web-requests-and-proxies).    
                
    ```powershell
    $user = $env:username
    $webproxy = (get-itemproperty 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet
    Settings').ProxyServer
    $pwd = Read-Host "Password?" -assecurestring
    $proxy = new-object System.Net.WebProxy
    $proxy.Address = $webproxy
    $account = new-object System.Net.NetworkCredential($user,[Runtime.InteropServices.Marshal]::PtrToStringAuto([Runtime.InteropServices.Marshal]::SecureStringToBSTR($pwd)), "")
    $proxy.credentials = $account
    $url = "https://wu2.frontend.clouddatahub.net/"
    $wc = new-object system.net.WebClient
    $wc.proxy = $proxy
    $webpage = $wc.DownloadData($url)
    $string = [System.Text.Encoding]::ASCII.GetString($webpage)
    $string
    ```

La respuesta esperada es la siguiente:
            
![Respuesta del comando de PowerShell 2](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)

> [!NOTE] 
> Consideraciones del proxy:
> *    Compruebe si el servidor proxy debe colocarse en la lista de destinatarios seguros. Si es así, asegúrese de que [estos dominios](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations#firewall-requirements-for-on-premisesprivate-network) estén en la lista de destinatarios seguros.
> *    Compruebe que el certificado TLS/SSL de "wu2.frontend.clouddatahub.net/" es de confianza en el servidor proxy.
> *    Si usa la autenticación de Active Directory en el proxy, cambie la cuenta de servicio por la cuenta de usuario que pueda acceder al proxy como "Integration Runtime Service".

### <a name="error-message"></a>Mensaje de error: 
`Self-hosted integration runtime node/ logical SHIR is in Inactive/ "Running (Limited)" state`

#### <a name="cause"></a>Causa 

El nodo de tiempo de ejecución integrado autohospedado podría tener un estado **inactivo**, como se muestra en la siguiente captura de pantalla:

![Nodo de IR autohospedado inactivo](media/self-hosted-integration-runtime-troubleshoot-guide/inactive-self-hosted-ir-node.png)

Este comportamiento se produce cuando los nodos no pueden comunicarse entre sí.

#### <a name="resolution"></a>Resolución

1. Inicie sesión en la máquina virtual hospedada en el nodo. En **Registros de aplicaciones y servicios** > **Integration Runtime**, abra el visor de eventos y filtre todos los registros de errores.

1. Compruebe si el registro de errores contiene el error siguiente: 
    
    ```
    System.ServiceModel.EndpointNotFoundException: Could not connect to net.tcp://xxxxxxx.bwld.com:8060/ExternalService.svc/WorkerManager. The connection attempt lasted for a time span of 00:00:00.9940994. TCP error code 10061: No connection could be made because the target machine actively refused it 10.2.4.10:8060. 
    System.Net.Sockets.SocketException: No connection could be made because the target machine actively refused it. 
    10.2.4.10:8060
    at System.Net.Sockets.Socket.DoConnect(EndPoint endPointSnapshot, SocketAddress socketAddress)
    at System.Net.Sockets.Socket.Connect(EndPoint remoteEP)
    at System.ServiceModel.Channels.SocketConnectionInitiator.Connect(Uri uri, TimeSpan timeout)
    ```
       
1. Si ve este error, ejecute lo siguiente en una línea de comandos: 

   ```
   telnet 10.2.4.10 8060
   ```
   
1. Si aparece el siguiente error, póngase en contacto con el departamento de TI para obtener ayuda para solucionar este problema. Una vez que pueda comunicarse con Telnet, póngase en contacto con Soporte técnico de Microsoft si todavía tiene problemas con el estado del nodo del entorno de ejecución integrador.
        
   ![Error de la línea de comandos](media/self-hosted-integration-runtime-troubleshoot-guide/command-line-error.png)
        
1. Compruebe si el registro de error contiene lo siguiente:

    ```
    Error log: Cannot connect to worker manager: net.tcp://xxxxxx:8060/ExternalService.svc/ No DNS entries exist for host azranlcir01r1. No such host is known Exception detail: System.ServiceModel.EndpointNotFoundException: No DNS entries exist for host xxxxx. ---> System.Net.Sockets.SocketException: No such host is known at System.Net.Dns.GetAddrInfo(String name) at System.Net.Dns.InternalGetHostByName(String hostName, Boolean includeIPv6) at System.Net.Dns.GetHostEntry(String hostNameOrAddress) at System.ServiceModel.Channels.DnsCache.Resolve(Uri uri) --- End of inner exception stack trace --- Server stack trace: at System.ServiceModel.Channels.DnsCache.Resolve(Uri uri)
    ```
    
1. Para solucionar este problema, pruebe uno de los métodos siguientes o ambos:
    - Colocar todos los nodos en el mismo dominio
    - Agregar la asignación de IP a host en todos los archivos de hosts de la máquina virtual hospedada


## <a name="troubleshoot-connectivity-issue"></a>Solución del problema de conectividad

### <a name="troubleshoot-connectivity-issue-between-self-hosted-ir-and-data-factory-or-self-hosted-ir-and-data-sourcesink"></a>Solución del problema de conectividad entre el IR autohospedado y Data Factory, o bien el IR autohospedado y el origen de datos o receptor

Para solucionar el problema de conectividad de red, debe saber cómo [recopilar el seguimiento de red](#how-to-collect-netmon-trace), entender cómo usarlo y [analizar el seguimiento de netmon](#how-to-analyze-netmon-trace) antes de aplicar las herramientas de Netmon en casos reales desde el IR autohospedado.

En ocasiones, cuando se solucionan problemas de conectividad como el siguiente entre el IR autohospedado y Data Factory: 

![Error de la solicitud HTTP](media/self-hosted-integration-runtime-troubleshoot-guide/http-request-error.png)

O bien entre el IR autohospedado y el origen de datos o el receptor, encontrará los errores siguientes:

**Mensaje de error:** 
`Copy failed with error:Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Cannot connect to SQL Server: ‘IP address’`

**Mensaje de error:** 
`One or more errors occurred. An error occurred while sending the request. The underlying connection was closed: An unexpected error occurred on a receive. Unable to read data from the transport connection: An existing connection was forcibly closed by the remote host. An existing connection was forcibly closed by the remote host Activity ID.`

**Resolución:** Al encontrar los problemas anteriores, consulte las instrucciones siguientes para solucionarlos:

Profundice en el análisis con el seguimiento de netmon.
- En primer lugar, puede establecer el filtro para ver cualquier restablecimiento del servidor en el lado cliente. En el ejemplo siguiente, puede ver que el lado servidor es el servidor de Data Factory.

    ![Servidor de Data Factory](media/self-hosted-integration-runtime-troubleshoot-guide/data-factory-server.png)

- Al obtener el paquete de restablecimiento, puede encontrar la conversación si sigue el TCP.

    ![Búsqueda de la conversación](media/self-hosted-integration-runtime-troubleshoot-guide/find-conversation.png)

- Después, puede obtener la conversación entre el cliente y el servidor de Data Factory si quita el filtro.

    ![Obtención de la conversación](media/self-hosted-integration-runtime-troubleshoot-guide/get-conversation.png)
- En función del seguimiento de netmon recopilado, se puede saber que el total de TTL (TimeToLive) es 64. Según los **valores predeterminados de TTL y límite de salto** mencionados en [este artículo](https://packetpushers.net/ip-time-to-live-and-hop-limit-basics/) (como se extrae a continuación), se aprecia que es el sistema Linux el que restablece el paquete y provoca la desconexión.

    Los valores predeterminados de TTL y límite de salto varían entre distintos sistemas operativos; estos son los de algunos de ellos:
    - Kernel de Linux 2.4 (2001 aproximadamente): 255 para TCP, UDP e ICMP
    - Kernel de Linux 4.10 (2015): 64 para TCP, UDP e ICMP
    - Windows XP (2001): 128 para TCP, UDP e ICMP
    - Windows 10 (2015): 128 para TCP, UDP e ICMP
    - Windows Server 2008: 128 para TCP, UDP e ICMP
    - Windows Server 2019 (2018): 128 para TCP, UDP e ICMP
    - macOS (2001): 64 para TCP, UDP e ICMP

    ![TTL 61](media/self-hosted-integration-runtime-troubleshoot-guide/ttl-61.png)
    
    Pero en el ejemplo anterior se muestra como 61 en lugar de 64, porque cuando el paquete de red alcanza el destino, debe pasar por varios saltos, como enrutadores o dispositivos de red. El número de enrutadores o dispositivos de red se deduce en el valor TTL final.
    En este caso, se ve que es posible que el restablecimiento se haya enviado desde el sistema Linux con TTL 64.

- Es necesario comprobar el cuarto salto desde IR autohospedado para confirmar de dónde puede provenir el dispositivo de restablecimiento.
 
    *Paquete de red del sistema Linux A con TTL 64 -> B TTL 64 menos 1 = 63 -> C TTL 63 menos 1 = 62 -> TTL 62 menos 1 = 61 IR autohospedado*

- En una situación idónea, el valor TTL será 128, lo que significa que el sistema Windows ejecuta la instancia de Data Factory. Como se muestra en el ejemplo siguiente, *128 – 107 = 21 saltos*, lo que significa que se han enviado 21 saltos para el paquete desde Data Factory a IR autohospedado durante el protocolo de enlace TCP 3.
 
    ![TTL 107](media/self-hosted-integration-runtime-troubleshoot-guide/ttl-107.png)

    Por tanto, debe comunicarle al equipo de red que compruebe cuál es el cuarto salto desde el IR autohospedado. Si es el firewall como sistema Linux, compruebe en los registros por qué el dispositivo restablece el paquete después del protocolo de enlace TCP 3. Pero si no está seguro de dónde investigar, intente obtener el seguimiento de netmon conjunto del IR autohospedado y el firewall durante el período problemático para determinar qué dispositivo ha podido restablecer este paquete y provocar la desconexión. En este caso, también debe indicarle al equipo de red que avance.

### <a name="how-to-collect-netmon-trace"></a>Procedimiento para recopilar el seguimiento de netmon

1.  Descargue las herramientas de Netmon desde [este sitio web](https://www.microsoft.com/en-sg/download/details.aspx?id=4865) e instálelas en el equipo del servidor (en cualquier servidor que tenga el problema) y en el cliente (por ejemplo el IR autohospedado).

2.  Cree una carpeta, por ejemplo, en la ruta de acceso siguiente: *D:\netmon*. Asegúrese de que tiene espacio suficiente para guardar el registro.

3.  Capture la información de puerto y dirección IP. 
    1. Inicie un símbolo del sistema CMD.
    2. Seleccione Ejecutar como administrador y ejecute el comando siguiente:
       
        ```
        Ipconfig /all >D:\netmon\IP.txt
        netstat -abno > D:\netmon\ServerNetstat.txt
        ```

4.  Capture el seguimiento de netmon (paquete de red).
    1. Inicie un símbolo del sistema CMD.
    2. Seleccione Ejecutar como administrador y ejecute el comando siguiente:
        
        ```
        cd C:\Program Files\Microsoft Network Monitor 3
        ```
    3. Puede usar tres comandos diferentes para capturar la página de red:
        - Opción A: comando de archivo RoundRobin (capturará un solo archivo y sobrescribirá los registros antiguos).

            ```
            nmcap /network * /capture /file D:\netmon\ServerConnection.cap:200M
            ```         
        - Opción B: comando de archivo encadenado (creará un archivo si se alcanzan los 200 MB).
        
            ```
            nmcap /network * /capture /file D:\netmon\ServerConnection.chn:200M
            ```          
        - Opción C: comando de archivo programado.

            ```
            nmcap /network * /capture /StartWhen /Time 10:30:00 AM 10/28/2011 /StopWhen /Time 11:30:00 AM 10/28/2011 /file D:\netmon\ServerConnection.chn:200M
            ```  

5.  Presione **Ctrl+C** para detener la captura del seguimiento de Netmon.
 
> [!NOTE]
> Si solo puede recopilar el seguimiento de netmon en el equipo cliente, obtenga la dirección IP del servidor para ayudarle a analizar el seguimiento.

### <a name="how-to-analyze-netmon-trace"></a>Procedimiento para analizar el seguimiento de netmon

Al intentar telnet **8.8.8.8 888** con el seguimiento de netmon recopilado anterior, debería ver el seguimiento siguiente:

![seguimiento de netmon 1](media/self-hosted-integration-runtime-troubleshoot-guide/netmon-trace-1.png)

![seguimiento de netmon 2](media/self-hosted-integration-runtime-troubleshoot-guide/netmon-trace-2.png)
 

Esto significa que no puede establecer una conexión TCP con el lado servidor **8.8.8.8** en función del puerto **888**, por lo que verá dos paquetes **SynReTransmit** adicionales. Como el origen **SELF-HOST2** no ha podido establecer la conexión con **8.8.8.8** en el primer paquete, seguirá intentándolo.

> [!TIP]
> - Puede hacer clic en **Cargar filtro** -> **Filtro estándar** -> **Direcciones** -> **Direcciones IPv4**.
> - Escriba **Dirección IPv4 = = 8.8.8.8** como filtro y haga clic en **Aplicar**. Después, solo verá la comunicación desde el equipo local al destino **8.8.8.8**.

![direcciones de filtro 1](media/self-hosted-integration-runtime-troubleshoot-guide/filter-addresses-1.png)
        
![direcciones de filtro 2](media/self-hosted-integration-runtime-troubleshoot-guide/filter-addresses-2.png)

En el ejemplo siguiente se muestra el aspecto que tendría un escenario correcto. 

- Si Telnet **8.8.8.8 53** funciona correctamente sin ningún problema, puede ver el protocolo de enlace TCP 3 y, después, la sesión finaliza con el protocolo de enlace TCP 4.

    ![Escenario de ejemplo correcto 1](media/self-hosted-integration-runtime-troubleshoot-guide/good-scenario-1.png)
     
    ![Escenario de ejemplo correcto 2](media/self-hosted-integration-runtime-troubleshoot-guide/good-scenario-2.png)

- En función del protocolo de enlace TCP 3 anterior, puede ver el flujo de trabajo siguiente:

    ![Flujo de trabajo del protocolo de enlace TCP 3](media/self-hosted-integration-runtime-troubleshoot-guide/tcp-3-handshake-workflow.png)
 
- El protocolo de enlace TCP 4 para finalizar la sesión y su flujo de trabajo se mostrarán de esta manera:

    ![Protocolo de enlace TCP 4](media/self-hosted-integration-runtime-troubleshoot-guide/tcp-4-handshake.png)

    ![Flujo de trabajo del protocolo de enlace TCP 4](media/self-hosted-integration-runtime-troubleshoot-guide/tcp-4-handshake-workflow.png) 


## <a name="next-steps"></a>Pasos siguientes

Para obtener más ayuda para solucionar problemas, pruebe los siguientes recursos:

*  [Blog de Data Factory](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Solicitud de características de Data Factory](https://feedback.azure.com/forums/270578-data-factory)
*  [Vídeos de Azure](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Página de preguntas y respuestas de Microsoft](https://docs.microsoft.com/answers/topics/azure-data-factory.html)
*  [Foro de Stack Overflow para Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Información de Twitter sobre Data Factory](https://twitter.com/hashtag/DataFactory)
*  [Guía de rendimiento de flujos de datos de asignación](concepts-data-flow-performance.md)
