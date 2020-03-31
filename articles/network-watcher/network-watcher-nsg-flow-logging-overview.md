---
title: Introducción al registro de flujo para NSG
titleSuffix: Azure Network Watcher
description: En esta página se explica cómo usar la característica de registros de flujo de los grupos de seguridad de red de Azure Network Watcher.
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: fb4a55b9757748581e26f3d6594f9be2139658cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78228258"
---
# <a name="introduction-to-flow-logging-for-network-security-groups"></a>Introducción al registro de flujo de grupos de seguridad de red

Los registros de flujos de grupos de seguridad de red (NSG) son una característica de Network Watcher que permite consultar información acerca del tráfico IP de entrada y de salida en un grupo de seguridad de red. Los registros de flujo se escriben en formato JSON y muestran los flujos de entrada y salida en función de cada regla, la tarjeta de interfaz de red (NIC) a la que se aplica el flujo, información de 5-tupla sobre el flujo (dirección IP de origen o destino, puerto de origen o destino y protocolo), si se permitió o denegó el tráfico, y en la versión 2, información de rendimiento (bytes y paquetes).


![información general de los registros de flujo](./media/network-watcher-nsg-flow-logging-overview/figure1.png)

Aunque los registros de flujo tienen como destino los NSG, no se muestran como los demás registros. Los registros de flujo se almacenan solo dentro de una cuenta de almacenamiento y siguen la ruta de acceso del registro que se muestra en el ejemplo siguiente:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```
Puede analizar los registros de flujo y obtener información sobre el tráfico de red mediante el [análisis del tráfico](traffic-analytics.md).

Las mismas directivas de retención vistas en otros registros se aplican a los registros de flujo. Puede establecer la directiva de retención de registros de 1 a 365 días. Si no se establece una directiva de retención, los registros se mantendrán indefinidamente.

## <a name="log-file"></a>Archivo de registro

Los registros de flujo incluyen las siguientes propiedades:

* **time**: la hora en la que se registró el evento
* **systemId**: el identificador de recurso del grupo de seguridad de red
* **category**: la categoría del evento. La categoría es siempre **NetworkSecurityGroupFlowEvent**
* **resourceid**: el identificador del recurso del grupo de seguridad de red
* **operationName**: siempre es NetworkSecurityGroupFlowEvents
* **properties**: una recopilación de las propiedades del flujo
    * **Version**: número de versión del esquema de eventos del registro de flujos
    * **flows**: una recopilación de flujos. Esta propiedad tiene varias entradas para diferentes reglas
        * **rule**: regla para la que se muestran los flujos
            * **flows**: una recopilación de flujos
                * **mac**: la dirección MAC de la NIC de la máquina virtual en la que se recopiló el flujo
                * **flowTuples**: una cadena que contiene varias propiedades de la tupla de flujo en un formato separado por comas
                    * **Time Stamp**: este valor es la marca de tiempo de cuando se produjo el flujo en formato de época de UNIX
                    * **Source IP**: la IP de origen
                    * **Destination IP**: la IP de destino
                    * **Source Port**: el puerto de origen
                    * **Destination Port**: el puerto de destino
                    * **Protocol**: el protocolo del flujo. Los valores válidos son **T** para TCP y **U** para UDP
                    * **Traffic Flow**: la dirección del flujo de tráfico. Los valores válidos son **I** para el correo entrante y **O** para el saliente.
                    * **Decisión de tráfico**: indica si el tráfico se permitió o se denegó. Los valores válidos son **A** para permitido y **D** para denegado.
                    * **Estado de flujo, solo versión 2**: captura el estado del flujo. Los estados posibles son**B**: Begin (Comienzo), cuando se crea el flujo. No se proporcionan las estadísticas. **C**: Continuación de un flujo en curso. Las estadísticas se proporcionan a intervalos de cinco minutos. **E**: End (Final), cuando termina el flujo. Se proporcionan las estadísticas.
                    * **Paquetes: origen a destino, solo versión 2** El número total de paquetes TCP o UDP enviados desde el origen al destino desde la última actualización.
                    * **Bytes enviados: origen a destino, solo versión 2** El número total de bytes de paquetes TCP o UDP enviados desde el origen al destino desde la última actualización. Los bytes de paquete incluyen el encabezado y la carga del paquete.
                    * **Paquetes: destino a origen, solo versión 2** El número total de paquetes TCP o UDP enviados desde el destino al origen desde la última actualización.
                    * **Bytes enviados: destino a origen, solo versión 2** El número total de bytes de paquetes TCP y UDP enviados desde el destino al origen desde la última actualización. Los bytes de paquete incluyen el encabezado y la carga del paquete.

## <a name="nsg-flow-logs-version-2"></a>Versión 2 de los registros de flujo de NSG

La versión 2 de los registros presenta el estado de flujo. Puede configurar qué versión de los registros de flujo recibe. Para saber cómo habilitar los registros de flujo, consulte [Habilitación del registro de flujo de NSG](network-watcher-nsg-flow-logging-portal.md).

El estado de flujo *B* se registra cuando se inicia un flujo. El estado de flujo *C* y el estado de flujo *E* son estados que marcan la continuación de un flujo y la terminación de este, respectivamente. Los estados *C* y *E* contienen información sobre el ancho de banda del tráfico.

**Ejemplo**: tuplas de flujo de una conversación TCP entre 185.170.185.105:35370 y 10.2.0.4:23:

"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,A,B,,,," "1493695838,185.170.185.105,10.2.0.4,35370,23,T,I,A,C,1021,588096,8005,4610880" "1493696138,185.170.185.105,10.2.0.4,35370,23,T,I,A,E,52,29952,47,27072"

Para los estados de continuación *C* y finalización *E*, los recuentos de paquetes y bytes son recuentos agregados desde el momento del registro de la tupla de flujo anterior. Con referencia a la conversación de ejemplo anterior, el número total de paquetes transferidos es 1021+52+8005+47 = 9125. El número total de bytes transferidos es 588096+29952+4610880+27072 = 5256000.

El texto que sigue es un ejemplo de un registro de flujo. Como puede ver, hay varios registros que siguen la lista de propiedades que se describe en la sección anterior.

## <a name="nsg-flow-logging-considerations"></a>Consideraciones acerca del registro de flujo de NSG

**Consideraciones de la cuenta de almacenamiento**: 

- Ubicación: la cuenta de almacenamiento usada debe estar en la misma región que NSG.
- Rotación de claves autoadministrable: si se cambian o rotan las claves de acceso a su cuenta de almacenamiento, los registros de flujos de NSG dejarán de funcionar. Para solucionar este problema, se deben deshabilitar y volver a habilitar los registros de flujos de NSG.

**Habilitar el registro de flujo de NSG en todos los NSG asociados a un recurso**: En Azure, el registro de flujo en Azure se configura en el recurso de NSG. Un flujo solo se asociará a una regla de NSG. En escenarios en los que se utilizan varios NSG, se recomienda que los registros de flujo de NSG estén habilitados en todos los NSG aplicados a la interfaz de red o subred de un recurso para garantizar que todo el tráfico se registre. Para obtener más información, consulte [cómo se evalúa el tráfico](../virtual-network/security-overview.md#how-traffic-is-evaluated) en los grupos de seguridad de red.

**Costos del registro de flujo**: El registro de flujos de NSG se factura según el volumen de registros generados. Un volumen de tráfico elevado puede producir un volumen de registro de flujo elevado, con los costos asociados. Los precios del registro de flujos de NSG no incluyen los costos de almacenamiento subyacentes. El uso de la característica de directiva de retención con el registro de flujos de NSG implica que se incurre en costos de almacenamiento independientes durante prolongados períodos de tiempo. Si no necesita la característica de directiva de retención, se recomienda que establezca este valor en 0. Para obtener más información, consulte [precios de Network Watcher](https://azure.microsoft.com/pricing/details/network-watcher/) y [precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/) para obtener más detalles.

**Flujos entrantes registrados desde direcciones IP de Internet a VM sin direcciones IP públicas**: Las VM que no tienen una dirección IP pública asignada a través de una dirección IP pública asociada con la NIC como dirección IP pública de nivel de instancia, o que forman parte de un grupo de back-end de equilibrador de carga básico, usan [SNAT predeterminada](../load-balancer/load-balancer-outbound-connections.md#defaultsnat) y tiene una dirección IP asignada por Azure para facilitar la conectividad de salida. Como consecuencia, es posible que vea las entradas de registro de flujo para los flujos desde las direcciones IP de Internet, si el flujo está destinado a un puerto en el intervalo de puertos asignados para SNAT. Si bien Azure no permitirá estos flujos a la VM, el intento se registra y aparece en el registro de flujos de NSG de Network Watcher por diseño. Se recomienda que el tráfico entrante de Internet no deseado se bloquee explícitamente con NSG.

**Recuentos de bytes y paquetes incorrectos para los flujos sin estado**: [Los grupos de seguridad de red (NSG)](https://docs.microsoft.com/azure/virtual-network/security-overview) se implementan como [firewall con estado](https://en.wikipedia.org/wiki/Stateful_firewall?oldformat=true). Sin embargo, muchas reglas predeterminadas o internas que controlan el flujo de tráfico se implementan sin estado. Debido a las limitaciones de la plataforma, los recuentos de bytes y paquetes no se registran para los flujos sin estado (es decir, los flujos de tráfico pasan por reglas sin estado), solo se registran para flujos con estado. Por lo tanto, el número de bytes y paquetes que se indican en los registros de flujo de NSG (y Análisis de tráfico) podrían ser diferentes de los flujos reales. Está previsto que esta limitación se corrija en junio de 2020.

## <a name="sample-log-records"></a>Entradas de registro de ejemplo

El texto que sigue es un ejemplo de un registro de flujo. Como puede ver, hay varios registros que siguen la lista de propiedades que se describe en la sección anterior.


> [!NOTE]
> Los valores de la propiedad **flowTuples* son una lista separada por comas.
 
### <a name="version-1-nsg-flow-log-format-sample"></a>Muestra de formato de la versión 1 del registro de flujo de NSG
```json
{
    "records": [
        {
            "time": "2017-02-16T22:00:32.8950000Z",
            "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
            "category": "NetworkSecurityGroupFlowEvent",
            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
            "operationName": "NetworkSecurityGroupFlowEvents",
            "properties": {
                "Version": 1,
                "flows": [
                    {
                        "rule": "DefaultRule_DenyAllInBound",
                        "flows": [
                            {
                                "mac": "000D3AF8801A",
                                "flowTuples": [
                                    "1487282421,42.119.146.95,10.1.0.4,51529,5358,T,I,D"
                                ]
                            }
                        ]
                    },
                    {
                        "rule": "UserRule_default-allow-rdp",
                        "flows": [
                            {
                                "mac": "000D3AF8801A",
                                "flowTuples": [
                                    "1487282370,163.28.66.17,10.1.0.4,61771,3389,T,I,A",
                                    "1487282393,5.39.218.34,10.1.0.4,58596,3389,T,I,A",
                                    "1487282393,91.224.160.154,10.1.0.4,61540,3389,T,I,A",
                                    "1487282423,13.76.89.229,10.1.0.4,53163,3389,T,I,A"
                                ]
                            }
                        ]
                    }
                ]
            }
        },
        {
            "time": "2017-02-16T22:01:32.8960000Z",
            "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
            "category": "NetworkSecurityGroupFlowEvent",
            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
            "operationName": "NetworkSecurityGroupFlowEvents",
            "properties": {
                "Version": 1,
                "flows": [
                    {
                        "rule": "DefaultRule_DenyAllInBound",
                        "flows": [
                            {
                                "mac": "000D3AF8801A",
                                "flowTuples": [
                                    "1487282481,195.78.210.194,10.1.0.4,53,1732,U,I,D"
                                ]
                            }
                        ]
                    },
                    {
                        "rule": "UserRule_default-allow-rdp",
                        "flows": [
                            {
                                "mac": "000D3AF8801A",
                                "flowTuples": [
                                    "1487282435,61.129.251.68,10.1.0.4,57776,3389,T,I,A",
                                    "1487282454,84.25.174.170,10.1.0.4,59085,3389,T,I,A",
                                    "1487282477,77.68.9.50,10.1.0.4,65078,3389,T,I,A"
                                ]
                            }
                        ]
                    }
                ]
            }
        },
    "records":
    [
        
        {
             "time": "2017-02-16T22:00:32.8950000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282421,42.119.146.95,10.1.0.4,51529,5358,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282370,163.28.66.17,10.1.0.4,61771,3389,T,I,A","1487282393,5.39.218.34,10.1.0.4,58596,3389,T,I,A","1487282393,91.224.160.154,10.1.0.4,61540,3389,T,I,A","1487282423,13.76.89.229,10.1.0.4,53163,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:01:32.8960000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282481,195.78.210.194,10.1.0.4,53,1732,U,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282435,61.129.251.68,10.1.0.4,57776,3389,T,I,A","1487282454,84.25.174.170,10.1.0.4,59085,3389,T,I,A","1487282477,77.68.9.50,10.1.0.4,65078,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:02:32.9040000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282492,175.182.69.29,10.1.0.4,28918,5358,T,I,D","1487282505,71.6.216.55,10.1.0.4,8080,8080,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282512,91.224.160.154,10.1.0.4,59046,3389,T,I,A"]}]}]}
        }
        ,
        ...
```
### <a name="version-2-nsg-flow-log-format-sample"></a>Muestra de formato de la versión 2 del registro de flujo de NSG
```json
 {
    "records": [
        {
            "time": "2018-11-13T12:00:35.3899262Z",
            "systemId": "a0fca5ce-022c-47b1-9735-89943b42f2fa",
            "category": "NetworkSecurityGroupFlowEvent",
            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
            "operationName": "NetworkSecurityGroupFlowEvents",
            "properties": {
                "Version": 2,
                "flows": [
                    {
                        "rule": "DefaultRule_DenyAllInBound",
                        "flows": [
                            {
                                "mac": "000D3AF87856",
                                "flowTuples": [
                                    "1542110402,94.102.49.190,10.5.16.4,28746,443,U,I,D,B,,,,",
                                    "1542110424,176.119.4.10,10.5.16.4,56509,59336,T,I,D,B,,,,",
                                    "1542110432,167.99.86.8,10.5.16.4,48495,8088,T,I,D,B,,,,"
                                ]
                            }
                        ]
                    },
                    {
                        "rule": "DefaultRule_AllowInternetOutBound",
                        "flows": [
                            {
                                "mac": "000D3AF87856",
                                "flowTuples": [
                                    "1542110377,10.5.16.4,13.67.143.118,59831,443,T,O,A,B,,,,",
                                    "1542110379,10.5.16.4,13.67.143.117,59932,443,T,O,A,E,1,66,1,66",
                                    "1542110379,10.5.16.4,13.67.143.115,44931,443,T,O,A,C,30,16978,24,14008",
                                    "1542110406,10.5.16.4,40.71.12.225,59929,443,T,O,A,E,15,8489,12,7054"
                                ]
                            }
                        ]
                    }
                ]
            }
        },
        {
            "time": "2018-11-13T12:01:35.3918317Z",
            "systemId": "a0fca5ce-022c-47b1-9735-89943b42f2fa",
            "category": "NetworkSecurityGroupFlowEvent",
            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
            "operationName": "NetworkSecurityGroupFlowEvents",
            "properties": {
                "Version": 2,
                "flows": [
                    {
                        "rule": "DefaultRule_DenyAllInBound",
                        "flows": [
                            {
                                "mac": "000D3AF87856",
                                "flowTuples": [
                                    "1542110437,125.64.94.197,10.5.16.4,59752,18264,T,I,D,B,,,,",
                                    "1542110475,80.211.72.221,10.5.16.4,37433,8088,T,I,D,B,,,,",
                                    "1542110487,46.101.199.124,10.5.16.4,60577,8088,T,I,D,B,,,,",
                                    "1542110490,176.119.4.30,10.5.16.4,57067,52801,T,I,D,B,,,,"
                                ]
                            }
                        ]
                    }
                ]
            }
        },
        ...
```

## <a name="next-steps"></a>Pasos siguientes

- Para saber cómo habilitar los registros de flujo, consulte [Habilitación del registro de flujo de NSG](network-watcher-nsg-flow-logging-portal.md).
- Para obtener información sobre cómo leer los registros del flujo, consulte [Lectura de registros de flujo de NSG](network-watcher-read-nsg-flow-logs.md).
- Para saber más sobre el registro de NSG, consulte [Registros de Azure Monitor para grupos de seguridad de red (NSG)](../virtual-network/virtual-network-nsg-manage-log.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- Para determinar si el tráfico se permite o deniega en una máquina virtual, consulte [Diagnóstico de problemas al filtrar el tráfico de red de una máquina virtual](diagnose-vm-network-traffic-filtering-problem.md).
