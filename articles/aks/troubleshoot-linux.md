---
title: Herramientas de rendimiento de Linux
titleSuffix: Azure Kubernetes Service
description: Obtenga información sobre cómo usar las herramientas de rendimiento de Linux y solucionar problemas comunes al usar Azure Kubernetes Service (AKS).
services: container-service
author: alexeldeib
ms.service: container-service
ms.topic: troubleshooting
ms.date: 02/10/2020
ms.author: aleldeib
ms.openlocfilehash: 74f65780594c7bc938ed6d59437473c4363e5848
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2020
ms.locfileid: "90982042"
---
# <a name="linux-performance-troubleshooting"></a>Solución de problemas de rendimiento en Linux

El agotamiento de recursos de las máquinas Linux es un problema habitual que puede manifestarse con muchos síntomas diferentes. Este documento contiene información general sobre las herramientas disponibles para ayudar a diagnosticar estos problemas.

Muchas de estas herramientas permiten establecer el intervalo en el que se va a generar la salida gradual. Por lo general, esta forma de salida hace que la detección de patrones resulte mucho más fácil. Si se acepta, la invocación de ejemplo incluirá `[interval]`.

Muchas de estas herramientas cuentan con un amplio historial y una gran variedad de opciones de configuración. En esta página, se incluye solo un subconjunto de invocaciones para poner de manifiesto problemas comunes. El origen de información prioritario siempre debe ser la documentación de referencia de cada herramienta concreta. Esa documentación será mucho más exhaustiva que la que pueda proporcionarse aquí.

## <a name="guidance"></a>Guía

Sea sistemático en su enfoque para investigar los problemas de rendimiento. Dos enfoques comunes son USE (uso, saturación, errores) y VED (velocidad, errores, duración). El método VED suele utilizarse en el contexto de los servicios que realizan una supervisión basada en solicitudes. El método USE suele emplearse para supervisar recursos. Supervise el uso, la saturación y los errores de todos los recursos de una máquina. Los cuatro tipos principales de recursos de cualquier máquina son la CPU, la memoria, el disco y la red. Un alto índice de uso, saturación o errores en cualquiera de estos recursos puede ser indicativo de que hay algún problema en el sistema. Cuando existe un problema, investigue la causa principal: ¿por qué es tan alta la latencia de entrada y salida del disco? ¿Los discos o la SKU de la máquina virtual tienen alguna limitación? ¿Qué procesos están escribiendo en los dispositivos y en qué archivos lo hacen?

Estos son algunos ejemplos de problemas e indicadores comunes para que pueda diagnosticarlos:
- Limitación de IOPS: utilice iostat para medir las IOPS de cada dispositivo. Asegúrese de que no haya ningún disco por encima de su límite y que la suma de todos los discos sea inferior al límite de la máquina virtual.
- Límite de ancho de banda: utilice iostat igual que con las IOPS, pero para medir el rendimiento de lectura y escritura. Asegúrese de que el rendimiento de cada dispositivo y el rendimiento general estén por debajo de los límites de ancho de banda.
- Agotamiento de SNAT: puede manifestarse mediante un elevado número de conexiones activas (de salida) de SAR. 
- Pérdida de paquetes: puede medirse mediante el proxy atendiendo al número de retransmisiones de TCP en comparación con el número enviado o recibido. Tanto `sar` como `netstat` pueden mostrar esta información.

## <a name="general"></a>General

Estas herramientas son de uso general y cubren información básica del sistema. Constituyen un buen punto de partida para una investigación más exhaustiva.

### <a name="uptime"></a>uptime

```
$ uptime
 19:32:33 up 17 days, 12:36,  0 users,  load average: 0.21, 0.77, 0.69
```

uptime indica el tiempo de actividad del sistema y las cargas medias en 1, 5 y 15 minutos. Estas cargas medias se corresponden a grosso modo con los subprocesos que realizan el trabajo o esperan a que se complete un trabajo ininterrumpido. En términos absolutos, estos números son difíciles de interpretar, pero con el transcurso del tiempo, pueden aportarnos información útil:

- media de 1 minuto > media de 5 minutos: significa que la carga está aumentando.
- media de 1 minuto < media de 5 minutos: significa que la carga está disminuyendo.

uptime también puede ayudar a esclarecer por qué no hay información disponible: el problema puede haberse resuelto solo o porque la máquina se reinició antes de que accediera el usuario.

Si las medias de carga son superiores al número de subprocesos de la CPU disponibles, podría indicar que hay un problema de rendimiento con una determinada carga de trabajo.

### <a name="dmesg"></a>dmesg

```
$ dmesg | tail 
$ dmesg --level=err | tail
```

dmesg vuelca la memoria del búfer del kernel. Los eventos como OOMKill agregan una entrada en el búfer del kernel. Encontrar un evento OOMKill u otros mensajes de agotamiento de recursos en los registros de dmesg es un indicador con bastante peso de que hay un problema.

### <a name="top"></a>top

```
$ top
Tasks: 249 total,   1 running, 158 sleeping,   0 stopped,   0 zombie
%Cpu(s):  2.2 us,  1.3 sy,  0.0 ni, 95.4 id,  1.0 wa,  0.0 hi,  0.2 si,  0.0 st
KiB Mem : 65949064 total, 43415136 free,  2349328 used, 20184600 buff/cache
KiB Swap:        0 total,        0 free,        0 used. 62739060 avail Mem

   PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
116004 root      20   0  144400  41124  27028 S  11.8  0.1 248:45.45 coredns
  4503 root      20   0 1677980 167964  89464 S   5.9  0.3   1326:25 kubelet
     1 root      20   0  120212   6404   4044 S   0.0  0.0  48:20.38 systemd
     ...
```

`top` proporciona información general sobre el estado actual del sistema. Los encabezados aportan cierta información útil, como:

- El estado de las tareas: en ejecución, en espera, detenidas.
- El uso de la CPU: en este caso, suele indicar el tiempo de inactividad.
- La memoria total, libre y utilizada por el sistema.

`top` puede pasar por alto los procesos de corta duración. Otras alternativas como `htop` y `atop` ofrecen interfaces similares y solucionan algunas de estas limitaciones.

## <a name="cpu"></a>CPU

Estas herramientas proporcionan información sobre el uso de la CPU. Esta información resulta especialmente útil con las salidas graduales, en las que resulta más fácil detectar patrones.

### <a name="mpstat"></a>mpstat

```
$ mpstat -P ALL [interval]
Linux 4.15.0-1064-azure (aks-main-10212767-vmss000001)  02/10/20        _x86_64_        (8 CPU)

19:49:03     CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
19:49:04     all    1.01    0.00    0.63    2.14    0.00    0.13    0.00    0.00    0.00   96.11
19:49:04       0    1.01    0.00    1.01   17.17    0.00    0.00    0.00    0.00    0.00   80.81
19:49:04       1    1.98    0.00    0.99    0.00    0.00    0.00    0.00    0.00    0.00   97.03
19:49:04       2    1.01    0.00    0.00    0.00    0.00    1.01    0.00    0.00    0.00   97.98
19:49:04       3    0.00    0.00    0.99    0.00    0.00    0.99    0.00    0.00    0.00   98.02
19:49:04       4    1.98    0.00    1.98    0.00    0.00    0.00    0.00    0.00    0.00   96.04
19:49:04       5    1.00    0.00    1.00    0.00    0.00    0.00    0.00    0.00    0.00   98.00
19:49:04       6    1.00    0.00    1.00    0.00    0.00    0.00    0.00    0.00    0.00   98.00
19:49:04       7    1.98    0.00    0.99    0.00    0.00    0.00    0.00    0.00    0.00   97.03
```

`mpstat` imprime información similar de la CPU al principio, pero desglosada por cada subproceso de la CPU. Ver todos los núcleos a la vez puede resultar útil para detectar si el uso de la CPU está desequilibrado; por ejemplo, cuando una aplicación con un solo subproceso utiliza un núcleo al 100 %. Este problema puede ser más difícil de detectar cuando se observa el conjunto global de todas las CPU del sistema.

### <a name="vmstat"></a>vmstat

```
$ vmstat [interval]
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 2  0      0 43300372 545716 19691456    0    0     3    50    3    3  2  1 95  1  0
```

`vmstat` proporciona información similar a `mpstat` y `top`, como el número de procesos de la CPU en espera (columna r), estadísticas de memoria y el porcentaje de CPU invertido en cada estado de trabajo.

## <a name="memory"></a>Memoria

La memoria es un recurso muy importante y sorprendentemente sencillo de utilizar para medir el rendimiento. Algunas herramientas pueden proporcionar información sobre la CPU y la memoria, como `vmstat`. Sin embargo, las herramientas como `free` son muy útiles para realizar tareas de depuración rápidamente.

### <a name="free"></a>libre

```
$ free -m
              total        used        free      shared  buff/cache   available
Mem:          64403        2338       42485           1       19579       61223
Swap:             0           0           0
```

`free` proporciona información básica sobre la memoria total, tanto la memoria utilizada como la memoria libre. `vmstat` puede ser más útil para realizar análisis básicos de memoria, ya que puede proporcionar la salida gradualmente.

## <a name="disk"></a>Disco

Las siguientes herramientas miden las IOPS del disco, las colas de espera y el rendimiento total. 

### <a name="iostat"></a>iostat

```
$ iostat -xy [interval] [count]
$ iostat -xy 1 1
Linux 4.15.0-1064-azure (aks-main-10212767-vmss000001)  02/10/20        _x86_64_        (8 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           3.42    0.00    2.92    1.90    0.00   91.76

Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
loop0             0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00
sdb               0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00
sda               0.00    56.00    0.00   65.00     0.00   504.00    15.51     0.01    3.02    0.00    3.02   0.12   0.80
scd0              0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00
```

`iostat` proporciona información detallada sobre el uso del disco. Esta invocación pasa `-x` para mostrar estadísticas detalladas, `-y` para omitir la salida inicial e imprimir las medias del sistema desde el arranque y `1 1` para especificar que queremos aplicar un intervalo de 1 segundo y terminar después de un bloque de salida. 

`iostat` muestra estadísticas muy útiles:

- `r/s` y `w/s` son lecturas y escrituras por segundo. La suma de estos valores conforman las IOPS.
- `rkB/s` y `wkB/s` son lecturas y escrituras por segundo en kilobytes. La suma de estos valores es el rendimiento.
- `await` es el tiempo medio de iowait en milisegundos para las solicitudes en cola.
- `avgqu-sz` es el tamaño medio de la cola durante el intervalo proporcionado.

En una máquina virtual de Azure:

- La suma de `r/s` y `w/s` en un dispositivo específico del bloque no debe superar los límites de la SKU de ese disco.
- La suma de `rkB/s` y `wkB/s` en un dispositivo específico del bloque no debe superar los límites de la SKU de ese disco.
- La suma de `r/s` y `w/s` de todos los dispositivos del bloque no puede superar los límites de la SKU de la máquina virtual.
- La suma de `rkB/s` y wkB/s de todos los dispositivos del bloque no puede superar los límites de la SKU de la máquina virtual.

Tenga en cuenta que el disco del sistema operativo cuenta como un disco administrado de la SKU más pequeña que corresponda a su capacidad. Por ejemplo, un disco del sistema operativo de 1024 GB se corresponderá con un disco P30. Los discos de sistema operativo efímeros y los discos temporales no tienen límites de disco individuales; solo se van afectados por los límites de toda la máquina virtual.

Los valores distintos de cero de awaitwait o avgqu-sz también son buenos indicadores de la contención de entrada y salida.

## <a name="network"></a>Red

Estas herramientas miden las estadísticas de red, como el rendimiento, los errores de transmisión y el uso. Si se realiza un análisis más profundo, pueden obtenerse estadísticas detalladas de TCP sobre la congestión y los paquetes descartados.

### <a name="sar"></a>sar

```
$ sar -n DEV [interval]
22:36:57        IFACE   rxpck/s   txpck/s    rxkB/s    txkB/s   rxcmp/s   txcmp/s  rxmcst/s   %ifutil
22:36:58      docker0      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
22:36:58    azv604be75d832      1.00      9.00      0.06      1.04      0.00      0.00      0.00      0.00
22:36:58       azure0     68.00     79.00     27.79     52.79      0.00      0.00      0.00      0.00
22:36:58    azv4a8e7704a5b    202.00    207.00     37.51     21.86      0.00      0.00      0.00      0.00
22:36:58    azve83c28f6d1c     21.00     30.00     24.12      4.11      0.00      0.00      0.00      0.00
22:36:58         eth0    314.00    321.00     70.87    163.28      0.00      0.00      0.00      0.00
22:36:58    azva3128390bff     12.00     20.00      1.14      2.29      0.00      0.00      0.00      0.00
22:36:58    azvf46c95ddea3     10.00     18.00     31.47      1.36      0.00      0.00      0.00      0.00
22:36:58       enP1s1     74.00    374.00     29.36    166.94      0.00      0.00      0.00      0.00
22:36:58           lo      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
22:36:58    azvdbf16b0b2fc      9.00     19.00      3.36      1.18      0.00      0.00      0.00      0.00
```

`sar` es una eficaz herramienta que permite realizar una gran variedad de análisis. Aunque en este ejemplo se usa para medir las estadísticas de red, es igualmente eficaz para medir el consumo de la CPU y la memoria. En este ejemplo, `sar` se invoca con la marca `-n` para especificar la palabra clave `DEV` (dispositivo de red), que muestra el rendimiento de la red en cada dispositivo.

- La suma de `rxKb/s` y `txKb/s` es el rendimiento total de un dispositivo dado. Si este valor supera el límite de la NIC de Azure aprovisionada, las cargas de trabajo de la máquina registrarán una mayor latencia de red.
- `%ifutil` mide el uso en un dispositivo dado. A medida que este valor se aproxime al 100 %, las cargas de trabajo experimentarán una mayor latencia de red.

```
$ sar -n TCP,ETCP [interval]
Linux 4.15.0-1064-azure (aks-main-10212767-vmss000001)  02/10/20        _x86_64_        (8 CPU)

22:50:08     active/s passive/s    iseg/s    oseg/s
22:50:09         2.00      0.00     19.00     24.00

22:50:08     atmptf/s  estres/s retrans/s isegerr/s   orsts/s
22:50:09         0.00      0.00      0.00      0.00      0.00

Average:     active/s passive/s    iseg/s    oseg/s
Average:         2.00      0.00     19.00     24.00

Average:     atmptf/s  estres/s retrans/s isegerr/s   orsts/s
Average:         0.00      0.00      0.00      0.00      0.00
```

Esta invocación de `sar` utiliza las palabras clave `TCP,ETCP` para examinar las conexiones TCP. La tercera columna de la última fila, "retrans", es el número de retransmisiones de TCP por segundo. Los elevados valores de este campo indican que la conexión de red no es fiable. En la primera y la tercera fila, "active" significa que se inició una conexión desde el dispositivo local, mientras que "remote" hace referencia a una conexión entrante.  Un problema frecuente en Azure es el agotamiento de puertos de SNAT, algo que `sar` puede ayudar a detectar. El agotamiento de puertos de SNAT se manifestaría como valores "active" elevados, ya que el problema se debe a un alto índice de conexiones TCP de salida que se inician localmente.

Como `sar` toma un intervalo, imprime la salida gradualmente y después imprime las filas finales de salida que contienen los resultados medios de la invocación.

### <a name="netstat"></a>netstat

```
$ netstat -s
Ip:
    71046295 total packets received
    78 forwarded
    0 incoming packets discarded
    71046066 incoming packets delivered
    83774622 requests sent out
    40 outgoing packets dropped
Icmp:
    103 ICMP messages received
    0 input ICMP message failed.
    ICMP input histogram:
        destination unreachable: 103
    412802 ICMP messages sent
    0 ICMP messages failed
    ICMP output histogram:
        destination unreachable: 412802
IcmpMsg:
        InType3: 103
        OutType3: 412802
Tcp:
    11487089 active connections openings
    592 passive connection openings
    1137 failed connection attempts
    404 connection resets received
    17 connections established
    70880911 segments received
    95242567 segments send out
    176658 segments retransmited
    3 bad segments received.
    163295 resets sent
Udp:
    164968 packets received
    84 packets to unknown port received.
    0 packet receive errors
    165082 packets sent
UdpLite:
TcpExt:
    5 resets received for embryonic SYN_RECV sockets
    1670559 TCP sockets finished time wait in fast timer
    95 packets rejects in established connections because of timestamp
    756870 delayed acks sent
    2236 delayed acks further delayed because of locked socket
    Quick ack mode was activated 479 times
    11983969 packet headers predicted
    25061447 acknowledgments not containing data payload received
    5596263 predicted acknowledgments
    19 times recovered from packet loss by selective acknowledgements
    Detected reordering 114 times using SACK
    Detected reordering 4 times using time stamp
    5 congestion windows fully recovered without slow start
    1 congestion windows partially recovered using Hoe heuristic
    5 congestion windows recovered without slow start by DSACK
    111 congestion windows recovered without slow start after partial ack
    73 fast retransmits
    26 retransmits in slow start
    311 other TCP timeouts
    TCPLossProbes: 198845
    TCPLossProbeRecovery: 147
    480 DSACKs sent for old packets
    175310 DSACKs received
    316 connections reset due to unexpected data
    272 connections reset due to early user close
    5 connections aborted due to timeout
    TCPDSACKIgnoredNoUndo: 8498
    TCPSpuriousRTOs: 1
    TCPSackShifted: 3
    TCPSackMerged: 9
    TCPSackShiftFallback: 177
    IPReversePathFilter: 4
    TCPRcvCoalesce: 1501457
    TCPOFOQueue: 9898
    TCPChallengeACK: 342
    TCPSYNChallenge: 3
    TCPSpuriousRtxHostQueues: 17
    TCPAutoCorking: 2315642
    TCPFromZeroWindowAdv: 483
    TCPToZeroWindowAdv: 483
    TCPWantZeroWindowAdv: 115
    TCPSynRetrans: 885
    TCPOrigDataSent: 51140171
    TCPHystartTrainDetect: 349
    TCPHystartTrainCwnd: 7045
    TCPHystartDelayDetect: 26
    TCPHystartDelayCwnd: 862
    TCPACKSkippedPAWS: 3
    TCPACKSkippedSeq: 4
    TCPKeepAlive: 62517
IpExt:
    InOctets: 36416951539
    OutOctets: 41520580596
    InNoECTPkts: 86631440
    InECT0Pkts: 14
```

`netstat` puede proporcionar una gran variedad de estadísticas de red (aquí se ha invocado con la salida de resumen). Hay muchos campos que pueden resultar útiles en función del problema. Uno de ellos está en la sección TCP y es "failed connection attempts" (intentos de conexión infructuosos). Esto puede ser un indicativo del agotamiento de los puertos de SNAT o de otros problemas con las conexiones salientes. Un elevado índice de segmentos retransmitidos (también en la sección TCP) podría indicar problemas con la entrega de paquetes. 
