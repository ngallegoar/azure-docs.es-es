---
title: 'Creación de una instancia de Connection Monitor: PowerShell'
titleSuffix: Azure Network Watcher
description: Aprenda a crear una instancia de Connection Monitor mediante PowerShell.
services: network-watcher
documentationcenter: na
author: vinigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/23/2020
ms.author: vinigam
ms.openlocfilehash: 1a554177bf7084b9a7f4c413dbe82271b3ab6b3a
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95545540"
---
# <a name="create-a-connection-monitor-using-powershell"></a>Creación de una instancia de Connection Monitor mediante PowerShell

Aprenda a crear una instancia de Connection Monitor para supervisar la comunicación entre los recursos mediante PowerShell.


## <a name="before-you-begin"></a>Antes de empezar 

En los monitores de conexión que crea en Connection Monitor, puede agregar máquinas locales y máquinas virtuales de Azure como orígenes. Estos monitores de conexión también pueden supervisar la conectividad con los puntos de conexión. Los puntos de conexión pueden estar en Azure o en cualquier otra dirección URL o IP.

Connection Monitor incluye estas entidades:

* **Recurso de monitor de conexión**: un recurso de Azure específico de la región. Todas las entidades siguientes son propiedades de un recurso de monitor de conexión.
* **Punto de conexión**: un origen o destino que participa en las comprobaciones de conectividad. Algunos ejemplos de puntos de conexión son las máquinas virtuales de Azure, los agentes locales, las direcciones URL y las direcciones IP.
* **Configuración de prueba**: la configuración de una prueba específica para el protocolo. En función del protocolo elegido, puede definir el puerto, los umbrales, la frecuencia de las pruebas y otros parámetros.
* **Grupo de prueba**: el grupo que contiene puntos de conexión de origen, puntos de conexión de destino y configuraciones de prueba. Un monitor de conexión puede contener más de un grupo de prueba.
* **Prueba**: la combinación de un punto de conexión de origen, un punto de conexión de destino y una configuración de prueba. Una prueba es el nivel más granular en el que están disponibles los datos de supervisión. Los datos de supervisión incluyen el porcentaje de comprobaciones con errores y el tiempo de ida y vuelta (RTT).

    ![Diagrama que muestra un monitor de conexión y donde se define la relación entre los grupos de prueba y las pruebas](./media/connection-monitor-2-preview/cm-tg-2.png)

## <a name="steps-to-create-with-powershell"></a>Pasos para crear con PowerShell

Use los comandos siguientes para crear un monitor de conexión mediante PowerShell.

```powershell

//Connect to your Azure account with the subscription
Connect-AzAccount
Select-AzSubscription -SubscriptionId <your-subscription>
//Select region
$nw = "NetworkWatcher_centraluseuap"
//Declare endpoints like Azure VM below. You can also give VNET,Subnet,Log Analytics workspace
$sourcevmid1 = New-AzNetworkWatcherConnectionMonitorEndpointObject -Name MyAzureVm -ResourceID /subscriptions/<your-subscription>/resourceGroups/<your resourceGroup>/providers/Microsoft.Compute/virtualMachines/<vm-name>
//Declare endpoints like URL, IPs
$bingEndpoint = New-AzNetworkWatcherConnectionMonitorEndpointObject -name Bing -Address www.bing.com # Destination URL
//Create test configuration.Choose Protocol and parametersSample configs below.

$IcmpProtocolConfiguration = New-AzNetworkWatcherConnectionMonitorProtocolConfigurationObject -IcmpProtocol
$TcpProtocolConfiguration = New-AzNetworkWatcherConnectionMonitorProtocolConfigurationObject -TcpProtocol -Port 80
$httpProtocolConfiguration = New-AzNetworkWatcherConnectionMonitorProtocolConfigurationObject -HttpProtocol -Port 443 -Method GET -RequestHeader @{Allow = "GET"} -ValidStatusCodeRange 2xx, 300-308 -PreferHTTPS
$httpTestConfiguration = New-AzNetworkWatcherConnectionMonitorTestConfigurationObject -Name http-tc -TestFrequencySec 60 -ProtocolConfiguration $httpProtocolConfiguration -SuccessThresholdChecksFailedPercent 20 -SuccessThresholdRoundTripTimeMs 30
$icmpTestConfiguration = New-AzNetworkWatcherConnectionMonitorTestConfigurationObject -Name icmp-tc -TestFrequencySec 30 -ProtocolConfiguration $icmpProtocolConfiguration -SuccessThresholdChecksFailedPercent 5 -SuccessThresholdRoundTripTimeMs 500
$tcpTestConfiguration = New-AzNetworkWatcherConnectionMonitorTestConfigurationObject -Name tcp-tc -TestFrequencySec 60 -ProtocolConfiguration $TcpProtocolConfiguration -SuccessThresholdChecksFailedPercent 20 -SuccessThresholdRoundTripTimeMs 30
//Create Test Group
$testGroup1 = New-AzNetworkWatcherConnectionMonitorTestGroupObject -Name testGroup1 -TestConfiguration $httpTestConfiguration, $tcpTestConfiguration, $icmpTestConfiguration -Source $sourcevmid1 -Destination $bingEndpoint,
$testname = "cmtest9"
//Create Connection Monitor
New-AzNetworkWatcherConnectionMonitor -NetworkWatcherName $nw -ResourceGroupName NetworkWatcherRG -Name $testname -TestGroup $testGroup1

```

## <a name="description-of-properties"></a>Descripción de las propiedades

* connectionMonitorName: nombre del recurso de Connection Monitor.

* SUB: identificador de la suscripción donde quiere crear el monitor de conexión.

* NW: identificador de recurso de Network Watcher en el que se creará el monitor de conexión. 

* location: región en la que se creará el monitor de conexión.

* Puntos de conexión
    * name: nombre único de cada punto de conexión.
    * resourceId: en el caso de los puntos de conexión de Azure, el id. de recurso hace referencia al id. de recurso de Azure Resource Manager para las máquinas virtuales. En el caso de los puntos de conexión que no son de Azure, el id. de recurso hace referencia al id. de recurso de Azure Resource Manager para el área de trabajo de Log Analytics vinculada a agentes que no son de Azure.
    * address: solo se aplica cuando no se especifica ningún identificador de recurso o si el identificador de recurso es el área de trabajo de Log Analytics. Si se usa con el identificador de recurso de Log Analytics, se refiere al FQDN del agente que se puede usar para la supervisión. Si se usa sin el identificador de recurso, puede ser la dirección URL o IP de cualquier punto de conexión público.
    * filter: en el caso de los puntos de conexión que no son de Azure, use este valor para seleccionar los agentes del área de trabajo de Log Analytics que se usarán para supervisar los recursos del monitor de conexión. Si no se establecen filtros, todos los agentes que pertenezcan al área de trabajo Log Analytics se pueden usar en la supervisión.
        * type: establezca el tipo como "dirección del agente".
        * address: establezca la dirección como el FQDN del agente local.

* Grupos de prueba
    * name: asigne un nombre al grupo de prueba.
    * testConfigurations: configuraciones de prueba basadas en los puntos de conexión de origen que se conectan a los puntos de conexión de destino.
    * sources: elija entre los puntos de conexión creados anteriormente. Los puntos de conexión de origen basados en Azure deben tener instalada la extensión de Azure Network Watcher y los que no se basan en Azure deben tener instalado el agente de Azure Log Analytics. Para instalar un agente para el origen, consulte [Instalación de los agentes de supervisión](./connection-monitor-overview.md#install-monitoring-agents).
    * destinations: elija entre los puntos de conexión creados anteriormente. Se puede supervisar la conectividad a las máquinas virtuales de Azure o a cualquier punto de conexión (una IP pública, una dirección URL o un FQDN) si se especifican como destinos. En un solo grupo de prueba, puede agregar máquinas virtuales de Azure, direcciones URL de Office 365, direcciones URL de Dynamics 365 y puntos de conexión personalizados.
    * disable: use este campo para deshabilitar la supervisión de todos los orígenes y destinos que especifica el grupo de prueba.

* Configuraciones de prueba
    * name: asigne un nombre a la configuración de prueba.
    * testFrequencySec: especifique la frecuencia con la que los orígenes harán ping a los destinos en el protocolo y el puerto especificados. Puede elegir 30 segundos, 1 minuto, 5 minutos, 15 minutos o 30 minutos. Los orígenes probarán la conectividad a los destinos según el valor que elija. Por ejemplo, si selecciona 30 segundos, los orígenes comprobarán la conectividad con el destino al menos una vez en un período de 30 segundos.
    * protocol: puede elegir TCP, ICMP, HTTP o HTTPS. Dependiendo del protocolo, puede realizar algunas configuraciones específicas del protocolo.
        * preferHTTPS: especifique si se usará HTTPS sobre HTTP.
        * port: especifique el puerto de destino de su elección.
        * disableTraceRoute: se aplica a los grupos de prueba cuyo protocolo es TCP o ICMP. Impide que los orígenes detecten la topología y el valor de RTT de salto a salto.
        * method: se aplica a las configuraciones de prueba cuyo protocolo es HTTP. Seleccione el método de solicitud HTTP, ya sea GET o POST.
        * path: especifique los parámetros de ruta de acceso para anexar a la dirección URL.
        * validStatusCodes: elija los códigos de estado aplicables. Si el código de respuesta no coincide con esta lista, obtendrá un mensaje de diagnóstico.
        * requestHeaders: especifique cadenas de encabezado de solicitud personalizadas que se pasarán al destino.
    * successThreshold: puede establecer los umbrales de estos parámetros de red:
        * checksFailedPercent: establezca el porcentaje de comprobaciones que pueden presentar errores cuando los orígenes comprueban la conectividad a los destinos mediante los criterios especificados. En el caso del protocolo TCP o ICMP, el porcentaje de comprobaciones con error puede ser igual al porcentaje de pérdida de paquetes. En el caso del protocolo HTTP, este campo representa el porcentaje de solicitudes HTTP que no recibieron respuesta.
        * roundTripTimeMs: establezca el RTT en milisegundos para indicar cuánto tiempo pueden tardar los orígenes en conectarse al destino a través de la configuración de prueba.

## <a name="scale-limits"></a>Límites de escalado

Los monitores de conexión tienen los límites de escala siguientes:

* Máximo de monitores de conexión por suscripción por región: 100
* Máximo de grupos de prueba por monitor de conexión: 20
* Máximo de orígenes y destinos por monitor de conexión: 100
* Máximo de configuraciones de prueba por monitor de conexión: 20

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [analizar los datos de supervisión y a definir alertas](./connection-monitor-overview.md#analyze-monitoring-data-and-set-alerts).
* Aprenda a [diagnosticar los problemas de la red](./connection-monitor-overview.md#diagnose-issues-in-your-network).