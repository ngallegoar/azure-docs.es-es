---
title: Rendimiento de las bases de datos Oracle en volúmenes únicos de NetApp Files | Microsoft Docs
description: Se describen los resultados de las pruebas de rendimiento de un volumen de Azure NetApp Files en una base de datos Oracle.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/30/2020
ms.author: b-juche
ms.openlocfilehash: c6cdf2f6dada0aa4dea2f70f18237b7ee39e3ea1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91571247"
---
# <a name="oracle-database-performance-on-azure-netapp-files-single-volumes"></a>Rendimiento de Oracle Database en volúmenes individuales de Azure NetApp Files

En este artículo se tratan los temas siguientes sobre Oracle en la nube. Estos temas pueden ser de especial interés para los administradores de bases de datos, los arquitectos de la nube o los arquitectos de almacenamiento:   

* Al controlar una carga de trabajo de procesamiento de transacciones en línea (OLTP) (principalmente E/S aleatoria) o una carga de trabajo de procesamiento analítico en línea (OLAP) (principalmente E/S secuencial), ¿cómo es el rendimiento?   
* ¿Qué diferencia hay en el rendimiento entre el cliente NFS del kernel de Linux (kNFS) y el propio cliente Direct NFS de Oracle?
* En lo referente al ancho de banda, ¿es suficiente el rendimiento de un solo volumen de Azure NetApp Files?

## <a name="testing-environment-and-components"></a>Entorno de pruebas y componentes

En el diagrama siguiente se ilustra el entorno usado para las pruebas. Por motivos de coherencia y simplicidad, se usaron los cuadernos de estrategia de Ansible para implementar todos los elementos del banco de pruebas.

![Entorno de pruebas de Oracle](../media/azure-netapp-files/performance-oracle-test-environment.png)  

### <a name="virtual-machine-configuration"></a>Configuración de la máquina virtual

En las pruebas se usó la siguiente configuración de la máquina virtual:
* Sistema operativo:   
    RedHat Enterprise Linux 7.8 (wle-ora01)
* Tipos de instancia:   
    Se usaron dos modelos en las pruebas: D32s_v3 y D64s_v3
* Número de interfaces de red:   
    Una (1) colocado en la subred 3  
* Discos:   
    Los archivos binarios y el sistema operativo de Oracle se colocaron en un solo disco premium.

### <a name="azure-netapp-files-configuration"></a><a name="anf_config"></a>Configuración de Azure NetApp Files
En las pruebas se usó la siguiente configuración de Azure NetApp Files:   

* Tamaño del grupo de capacidad:  
    Se configuraron varios tamaños del grupo: 4 TiB, 8 TiB, 16 TiB, 32 TiB 
* Nivel de servicio:  
    Ultra (128 MiB/s de ancho de banda por 1 TiB de capacidad de volumen asignada)
* Volúmenes:  
    Se evaluaron una y dos pruebas de volumen

### <a name="workload-generator"></a>Generador de cargas de trabajo 

En las pruebas se usó la carga de trabajo generada por SLOB 2.5.4.2. SLOB (Silly Little Oracle Benchmark) es un generador de cargas de trabajo muy conocido en el espacio de Oracle diseñado para cargar y probar el subsistema de E/S con una carga de trabajo de E/S física almacenada en búfer de SGA.  

SLOB 2.5.4.2 no admite la base de datos conectable (PDB). Como tal, se ha agregado un cambio a los scripts `setup.sh` y `runit.sh` para permitir la compatibilidad con PDB.  

Las variables de SLOB usadas en las pruebas se describen en las secciones siguientes.

#### <a name="workload-80-select-20-update--random-io--slobconf-variables"></a>Carga de trabajo 80 % SELECT, 20% UPDATE | E/S aleatoria: variables `slob.conf`   

`UPDATE_PCT=20`   
`SCAN_PCT=0`   
`RUN_TIME=600`   
`WORK_LOOP=0`   
`SCALE=75G`   
`SCAN_TABLE_SZ=50G`   
`WORK_UNIT=64`   
`REDO_STRESS=LITE`   
`LOAD_PARALLEL_DEGREE=12`   

#### <a name="workload-100-select--sequential-io--slobconf-variables"></a>Carga de trabajo 100 % SELECT | E/S secuencial: variables `slob.conf`

`UPDATE_PCT=0`   
`SCAN_PCT=100`   
`RUN_TIME=600`   
`WORK_LOOP=0`   
`SCALE=75G`   
`SCAN_TABLE_SZ=50G`   
`WORK_UNIT=64`   
`REDO_STRESS=LITE`   
`LOAD_PARALLEL_DEGREE=12`   

### <a name="database"></a>Base de datos

La versión de Oracle utilizada para las pruebas es Oracle Database Enterprise Edition 19.3.0.0.

Los parámetros de Oracle son los siguientes:  
* `sga_max_size`: 4096M
* `sga_target`: 4096
* `db_writer_processes`: 12
* `awr_pdb_autoflush_enabled`: true
* `filesystemio_options`: SETALL
* `log_buffer`: 134217728

Se creó una PDB para la base de datos de SLOB.

En el siguiente diagrama se muestra el espacio de tablas denominado PERFIO con un tamaño de 600 GB (20 archivos de datos, 30 GB cada uno) creado para hospedar cuatro esquemas de usuario de SLOB. Cada esquema de usuario tenía un tamaño de 125 GB.

![Base de datos Oracle](../media/azure-netapp-files/performance-oracle-tablespace.png)  

## <a name="performance-metrics"></a>Métricas de rendimiento

El objetivo era notificar el rendimiento de E/S experimentado por la aplicación. Por lo tanto, en todos los diagramas de este artículo se usan las métricas notificadas por la base de datos Oracle mediante sus informes de repositorio automático de carga de trabajo (AWR). Las métricas utilizadas en los diagramas son las siguientes:   

* **Media de solicitudes de E/S por segundo**   
    Corresponde a la suma de la media de solicitudes por segundo de E/S de lectura y la media de solicitudes por segundo de E/S de escritura de la sección de perfil de carga.
* **Media de MB por segundo de E/S**   
    Corresponde a la suma de la media de MB por segundo de E/S de lectura y la media de MB por segundo de E/S de escritura de la sección de perfil de carga.
* **Latencia de lectura media**   
    Corresponde a la latencia media de la "lectura secuencial del archivo de base de datos" del evento de espera de Oracle en microsegundos.
* **Número de subprocesos/esquema**   
    Corresponde al número de subprocesos de SLOB por esquema de usuario.

## <a name="performance-measurement-results"></a>Resultados de la medida de rendimiento  

En esta sección se describen los resultados de la medida de rendimiento.

### <a name="linux-knfs-client-vs-oracle-direct-nfs"></a>Cliente kNFS de Linux frente a Oracle Direct NFS

Este escenario se ha estado ejecutando en una máquina virtual de Azure Standard_D32s_v3 (Intel E5-2673 V4 a 2,30 GHz). La carga de trabajo es 75 % SELECT y 25 % UPDATE, la mayor parte E/S aleatoria y con un objetivo del búfer de base de datos de ~7,5 %. 

Como se muestra en el diagrama siguiente, el cliente DNFS de Oracle proporcionó hasta 2,8 veces más rendimiento que el cliente kNFS de Linux normal:  

![Cliente kNFS de Linux comparado con Oracle Direct NFS](../media/azure-netapp-files/performance-oracle-kfns-compared-dnfs.png)  

En el diagrama siguiente se muestra la curva de latencia de las operaciones de lectura. En este contexto, el cuello de botella del cliente kNFS es la única conexión de socket TCP de NFS establecida entre el cliente y el servidor NFS (el volumen de Azure NetApp Files).  

![Latencia de la curva del cliente kNFS de Linux comparado con Oracle Direct NFS](../media/azure-netapp-files/performance-oracle-latency-curve.png)  

El cliente DNFS pudo enviar más solicitudes de E/S por segundo debido a su capacidad para crear cientos de conexiones de socket TCP; por lo tanto, aprovechó el paralelismo. Como se describe en [Configuración de Azure NetApp Files](#anf_config), cada TiB adicional de capacidad asignada permite 128 MiB/s adicionales de ancho de banda. DNFS alcanzó el límite de 1 GiB/s de rendimiento, que es el máximo impuesto por la selección de capacidad de 8 TiB. Con más capacidad, más rendimiento se habría controlado.

El rendimiento es solo uno de los aspectos que se deben tener en cuenta. Otro aspecto es la latencia, que afecta de primera mano en la experiencia del usuario. Tal y como se muestra en el diagrama siguiente, se pueden esperar aumentos de la latencia mucho más rápidos con kNFS que con DNFS. 

![Latencia de lectura del cliente kNFS de Linux comparado con Oracle Direct NFS](../media/azure-netapp-files/performance-oracle-read-latency.png)  

En los histogramas se proporciona una visión excelente de las latencias de base de datos. En el siguiente diagrama se proporciona una vista completa desde la perspectiva de la "lectura secuencial del archivo de base de datos" grabada, mientras se usa DNFS en el punto de datos de simultaneidad más alto (32 subprocesos/esquema). Como se muestra en el diagrama siguiente, el 47 % de todas las operaciones de lectura se situaron entre 512 y 1000 microsegundos, mientras que el 90 % de todas las operaciones de lectura se sirvieron con una latencia inferior a 2 ms.

![Histogramas del cliente kNFS de Linux comparado con Oracle NFS Direct](../media/azure-netapp-files/performance-oracle-histogram-read-latency.png)  

En conclusión, está claro que DNFS es una apuesta segura para mejorar el rendimiento de una instancia de base de datos Oracle en NFS.

### <a name="single-volume-performance-limits"></a>Límites de rendimiento de un solo volumen

En esta sección se describen los límites de rendimiento de un solo volumen con E/S aleatoria y E/S secuencial. 

#### <a name="random-io"></a>E/S aleatoria

DNFS puede consumir mucho más ancho de banda que el que proporciona una cuota de rendimiento de Azure NetApp Files de 8 TB. Al aumentar la capacidad del volumen de Azure NetApp Files a 16 TiB, que es un cambio instantáneo, la cantidad de ancho de banda del volumen aumentó el doble, de 1024 a 2048 MiB/s. 

En el diagrama siguiente se muestra una configuración de una carga de trabajo de selección del 80 % y de actualización del 20 %, y con una relación del 8 % en el objetivo del búfer de base de datos. SLOB pudo impulsar un solo volumen a 200 000 solicitudes de E/S NFS por segundo. Teniendo en cuenta que cada operación tiene un tamaño de 8 KiB, el sistema sometido a prueba fue capaz de proporcionar ~200 000 solicitudes de E/S por segundo o 1600 MiB/s.
 
![Rendimiento de Oracle DNFS](../media/azure-netapp-files/performance-oracle-dnfs-throughput.png)  

En el siguiente diagrama de la curva de latencia de lectura se muestra que, a medida que aumenta el rendimiento de lectura, la latencia aumenta suavemente por debajo de la línea de 1 ms y alcanza el ángulo de la curva en la media de 165 000 solicitudes por segundo de E/S de lectura con una latencia de lectura media de ~ 1,3 ms.  Este valor es un valor de latencia increíble para una tasa de E/S inalcanzable con casi ninguna otra tecnología de nube de Azure. 

![Curva de latencia de Oracle DNFS](../media/azure-netapp-files/performance-oracle-dnfs-latency-curve.png)  

#### <a name="sequential-io"></a>E/S secuencial  

Como se muestra en el diagrama siguiente, no toda la E/S es aleatoria por naturaleza; consideremos, por ejemplo, una copia de seguridad de RMAN o un recorrido de tabla completo, donde las cargas de trabajo requieren todo el ancho de banda que puedan obtener.  Con la misma configuración que se ha descrito anteriormente, pero con un cambio de tamaño del volumen a 32 TiB, en el diagrama siguiente se muestra que una única instancia de Oracle DB puede aumentar el rendimiento a 3900 MB/s, muy cerca de la cuota de rendimiento del volumen de Azure NetApp Files de 32 TB (128 MB/s * 32 = 4096 MB/s).

![E/S de Oracle DNFS](../media/azure-netapp-files/performance-oracle-dnfs-io.png)  

En resumen, Azure NetApp Files le ayuda a llevar las bases de datos de Oracle a la nube. Ofrece rendimiento cuando la base de datos lo demanda. Y, en cualquier momento, puede cambiar el tamaño de la cuota de volumen de forma dinámica y sin interrupciones.

## <a name="next-steps"></a>Pasos siguientes

- [Banco de pruebas de rendimiento de recomendaciones de pruebas para Azure NetApp Files](azure-netapp-files-performance-metrics-volumes.md)
- [Bancos de pruebas de rendimiento para Linux](performance-benchmarks-linux.md)