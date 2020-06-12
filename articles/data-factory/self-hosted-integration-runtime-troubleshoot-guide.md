---
title: Solución de problemas de un entorno de ejecución de integración autohospedado en Azure Data Factory
description: Conozca cómo solucionar problemas del entorno de ejecución de integración autohospedado en Azure Data Factory.
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 11/07/2019
ms.author: abnarain
ms.openlocfilehash: f27132eb21d245d0d26de910abba088ba3b8efde
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84170983"
---
# <a name="troubleshoot-self-hosted-integration-runtime"></a>Solución de problemas del entorno de ejecución de integración autohospedado

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se exploran los métodos comunes de solución de problemas del entorno de ejecución de integración autohospedado en Azure Data Factory.

## <a name="common-errors-and-resolutions"></a>Errores comunes y soluciones

### <a name="error-message-self-hosted-integration-runtime-cant-connect-to-cloud-service"></a>Mensaje de error: El entorno de ejecución de integración autohospedado no puede conectarse al servicio en la nube.

![Problema de conexión de IR autohospedado](media/self-hosted-integration-runtime-troubleshoot-guide/unable-to-connect-to-cloud-service.png)

#### <a name="cause"></a>Causa 

El entorno de ejecución de integración autohospedado no puede conectarse al servicio Data Factory (back-end). Este problema se debe normalmente a la configuración de red en el firewall.

#### <a name="resolution"></a>Solución

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

### <a name="error-message-self-hosted-integration-runtime-node-logical-shir-is-in-inactive-running-limited-state"></a>Mensaje de error: El nodo del entorno de ejecución de integración autohospedado/SHIR lógico está en estado inactivo/"en ejecución (limitado)"

#### <a name="cause"></a>Causa 

El nodo de tiempo de ejecución integrado autohospedado podría tener un estado **inactivo**, como se muestra en la siguiente captura de pantalla:

![Nodo de IR autohospedado inactivo](media/self-hosted-integration-runtime-troubleshoot-guide/inactive-self-hosted-ir-node.png)

Este comportamiento se produce cuando los nodos no pueden comunicarse entre sí.

#### <a name="resolution"></a>Solución

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


## <a name="next-steps"></a>Pasos siguientes

Para obtener más ayuda para solucionar problemas, pruebe los siguientes recursos:

*  [Blog de Data Factory](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Solicitud de características de Data Factory](https://feedback.azure.com/forums/270578-data-factory)
*  [Vídeos de Azure](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Página de preguntas y respuestas de Microsoft](https://docs.microsoft.com/answers/topics/azure-data-factory.html)
*  [Foro de Stack Overflow para Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Información de Twitter sobre Data Factory](https://twitter.com/hashtag/DataFactory)
*  [Guía de rendimiento de flujos de datos de asignación](concepts-data-flow-performance.md)
