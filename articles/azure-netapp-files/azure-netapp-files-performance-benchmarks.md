---
title: Resultados de las pruebas comparativas de rendimiento para Azure NetApp Files | Microsoft Docs
description: Describe los resultados del banco de pruebas de rendimiento para Azure NetApp Files al nivel de volumen.
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
ms.date: 08/07/2019
ms.author: b-juche
ms.openlocfilehash: 1d6b43110046f26d8c8070b19587366588eee7b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "68881747"
---
# <a name="performance-benchmark-test-results-for-azure-netapp-files"></a>Banco de pruebas de rendimiento de resultados de pruebas para Azure NetApp Files

Este artículo describe los resultados del banco de pruebas de rendimiento para Azure NetApp Files al nivel de volumen. 

## <a name="sample-application-used-for-the-tests"></a>Aplicación de ejemplo usada para las pruebas

Se ejecutaron pruebas de rendimiento con una aplicación de ejemplo con Azure NetApp Files. La aplicación tiene las siguientes características: 

* Una aplicación basada en Linux creada para la nube
* Puede escalar linealmente con máquinas virtuales que se agreguen para aumentar la capacidad de proceso según sea necesario
* Requiere una accesibilidad rápida al lago de datos
* Tiene patrones de E/S que a veces son aleatorios y a veces secuenciales 
    * Un patrón aleatorio requiere una latencia baja para grandes cantidades de E/S. 
    * Un patrón secuencial requiere una cantidad importante de ancho de banda. 

## <a name="about-the-workload-generator"></a>Acerca del generador de carga de trabajo

Los resultados se obtienen de los archivos de resumen de Vdbench. [Vdbench](https://www.oracle.com/technetwork/server-storage/vdbench-downloads-1901681.html) es una utilidad de línea de comandos que genera cargas de trabajo de E/S de disco para validar el rendimiento del almacenamiento. La configuración de cliente-servidor utilizada es escalable.  Incluye un sola máquina virtual mixta de maestro y cliente, y 14 máquinas virtuales dedicadas al cliente.

## <a name="about-the-tests"></a>Acerca de las pruebas

Las pruebas se diseñaron para identificar los límites que podría tener la aplicación de ejemplo y el tiempo de respuesta que asciende hasta los límites.  

Las pruebas siguientes se ejecutaron: 

* Lectura aleatoria del 100 % de 8-KiB
* Escritura aleatoria del 100 % de 8-KiB
* Lectura secuencial del 100 % de 64-KiB
* Escritura secuencial del 100 % de 64-KiB
* Lectura secuencial del 50 % de 64-KiB, escritura secuencial del 50 % de 64-KiB
* Lectura aleatoria del 50 % de 8-KiB, escritura aleatoria del 50 % de 8-KiB

## <a name="bandwidth"></a>Ancho de banda

Azure NetApp Files ofrece tres [niveles de servicio](azure-netapp-files-service-levels.md). Cada nivel de servicio ofrece una cantidad de ancho de banda por cada TiB de capacidad aprovisionada (cuota de volumen) diferente. El límite de ancho de banda para un volumen se aprovisiona en función de la combinación del nivel de servicio y de la cuota de volumen. El límite de ancho de banda es solo un factor para determinar la cantidad real de rendimiento que se alcanzará.  

Actualmente, MiB 4500 es el mayor rendimiento que se ha logrado en una carga de trabajo en un único volumen en las pruebas.  Con el nivel de servicio Premium, una cuota de volumen de 70,31 TiB aprovisionará suficiente ancho de banda para obtener este rendimiento de acuerdo al siguiente cálculo: 

![Fórmula de ancho de banda](../media/azure-netapp-files/azure-netapp-files-bandwidth-formula.png)

![Cuota y nivel de servicio](../media/azure-netapp-files/azure-netapp-files-quota-service-level.png)

## <a name="throughput-intensive-workloads"></a>Cargas de trabajo de rendimiento intensivo

La prueba de rendimiento ha usado Vdbench y una combinación de máquinas virtuales de almacenamiento 12xD32s V3. El volumen de ejemplo en la prueba ha conseguido las cifras de rendimiento siguientes:

![Prueba de rendimiento](../media/azure-netapp-files/azure-netapp-files-throughput-test.png)

## <a name="io-intensive-workloads"></a>Cargas de trabajo de E/S intensivas

La prueba de E/S ha usado Vdbench y una combinación de máquinas virtuales de almacenamiento 12xD32s V3. El volumen de ejemplo en la prueba logró las siguientes cifras en E/S:

![Prueba de E/S](../media/azure-netapp-files/azure-netapp-files-io-test.png)

## <a name="latency"></a>Latencia

La distancia entre las máquinas virtuales de prueba y el volumen de Azure NetApp Files repercute en el rendimiento de E/S.  El gráfico siguiente compara la IOPS frente a las curvas de respuesta de latencia para dos conjuntos diferentes de máquinas virtuales.  Un conjunto de máquinas virtuales está cerca de Azure NetApp Files y el otro conjunto está más lejos.  La mayor latencia para el conjunto de máquinas virtuales que se sitúa más lejos influye en la cantidad de IOPS que se consigue en un nivel determinado de paralelismo.  En cualquier caso, las lecturas en un volumen pueden superar los 300 000 IOPS, tal como se muestra a continuación: 

![Estudio de la latencia](../media/azure-netapp-files/azure-netapp-files-latency-study.png)

## <a name="summary"></a>Resumen

Las cargas de trabajo sensibles a la latencia (bases de datos) pueden tener un tiempo de respuesta de un milisegundo. El rendimiento transaccional puede ser de más de 300 K IOPS para un único volumen.

Las aplicaciones sensibles al rendimiento (para streaming y creación de imágenes) pueden tener 4,5 GiB/s de rendimiento.

## <a name="example-scripts"></a>Scripts de ejemplo

Los siguientes scripts de ejemplo tienen solo fines de demostración.  No deben usarse para entornos de producción.  

    #
    #This script makes the following assumptions about the environment
    #VM Naming Convention:
    #   VM naming convention: vdbench-vnet1-1 .. vdbench-vnet1-x
    #
    #VM Count:
    #   The script is written for 24 VM's, 
    #   If you wish to test with a different number of vm's replace {1..24} with {1..Some Number}
    #
    #Volume mount point:
    #    The volumes in this script are mounted at the following mount points on all virtual machines
    #    /mnt/vdb1 ... /mnt/vdb6
    #   
    #Virtual machines must have
    #   make sure that vdbench is present on all vms in the environment and that java has been installed on each vm as well as nfs-utils
    #
    #The following tunables were configured on all virtual machines in the environment
    #    No special tunables were used to extract the results identified in this paper.
    #    Even rsize and wsize were left at the default (64K)
    #
    #Special Notes (thread counts) 
    #   You can hone in on the amount of I/O you hope to achieve in your testing using littles law,
    #   Run each test with a thread count of 1 to determnine the best possible latency.
    #   Thread count equals latency in seconds * desired I/O rate.  
    #   If you find a minimum latency of 1.14ms for example, divide that by 1000 to convert 1.3ms to .0013 seconds.
    #   
    #   If your goal from a single machine is for example 3,500 IOPS, a thread count of 4.0 is required.
    
    #   thread count 4.0 == .0013s * 5,000ops
    #
    #    
    #Special Notes: Increased window size:
    #   As round-trip time increases; corresponding increases are required in TCP Window Size – think Littles law as shown above. 
    #   Check the values of the following and adjust as needed: net.core.rmem_max, net.core.rmem_default, net.ipv4.tcp_rmem
    #
    #Run the utility like this:
    #    vdbench -f vnet1-SeqMix-workload
    #
    #When the utility is finished its run, grep 'avg' from the summary file to see overall run performance.
     
    
    
    #LUN CONFIG FILES
    for vnet in 1; do
        echo "sd=default,size=693g" > vnet${vnet}-luns-nfs
        for vm in {1..24}; do
            for vol in {1..6}; do
                echo "sd=sd-${vm}-${vol},host=vm${vm},lun=/mnt/vdb${vol}/file-${vm}-${vol},openflags=o_direct" >> vnet${vnet}-luns-nfs
            done
        done
    done
    
    
    #HOST CONFIG FILES
    for vnet in 1; do
        echo "hd=default,jvms=1,shell=ssh" > vnet${vnet}-hosts-nfs
        for vm in {1..24}; do
            echo "hd=vm${vm},system=vdbench-vnet${vnet}-${vm},user=root" >> vnet${vnet}-hosts-nfs
        done
    done
    
    #VDBENCH WORK SCRIPTS
    for vnet in 1; do
        for pattern in FillWrite SeqRead SeqWrite RndRead RndWrite SeqMix RndMix; do
            echo "include=vnet${vnet}-hosts-nfs" > vnet${vnet}-${pattern}-workload
            echo "include=vnet${vnet}-luns-nfs" >> vnet${vnet}-${pattern}-workload
        done 
        #Fill Write File
        echo "wd=FillWrite,sd=sd*,rdpct=0,seekpct=eof,xfersize=64k" >> vnet${vnet}-FillWrite-workload
        echo "* Run Defaults" >> vnet${vnet}-FillWrite-workload
        echo 'rd=Initialize-LUNs-Full,wd=FillWrite,sd=("sd*"),iorate=max,interval=1,forthreads=1,elapsed=259200' >> vnet${vnet}-FillWrite-workload
    
        #The actual workload files - feel free to add more or use less.
        #   The thread counts shown below were used to generate the performance collateral, 
        #   feel free to change to suite your needs.
    
        echo "wd=SeqWrite,sd=sd*,rdpct=0,seekpct=0,xfersize=64k" >> vnet${vnet}-SeqWrite-workload
        echo "wd=SeqRead,sd=sd*,rdpct=100,seekpct=0,xfersize=64k" >> vnet${vnet}-SeqRead-workload
        echo "wd=RndRead,sd=sd*,rdpct=100,seekpct=100,xfersize=8k" >> vnet${vnet}-RndRead-workload
        echo "wd=RndWrite,sd=sd*,rdpct=0,seekpct=100,xfersize=8k" >> vnet${vnet}-RndWrite-workload
        echo "wd=SeqMix,sd=sd*,rdpct=50,seekpct=0,xfersize=64k" >> vnet${vnet}-SeqMix-workload
        echo "wd=RndMix,sd=sd*,rdpct=50,seekpct=100,xfersize=8k" >> vnet${vnet}-RndMix-workload
        for pattern in SeqRead SeqWrite RndRead RndWrite SeqMix RndMix; do
            echo "* Run Defaults" >> vnet${vnet}-${pattern}-workload
            #echo "rd=default,curve=(50,100),iorate=curve,warmup=60,elapsed=600,iorate=curve" >> vnet${vnet}-${pattern}-workload
            echo "rd=default,warmup=60,elapsed=600,iorate=max" >> vnet${vnet}-${pattern}-workload
            echo "* Run Definitions" >> vnet${vnet}-${pattern}-workload
        done
        echo 'rd=RndRead,wd=RndRead,sd=("sd*"),threads=25' >> vnet${vnet}-RndRead-workload
        echo 'rd=RndWrite,wd=RndWrite,sd=("sd*"),threads=20' >> vnet${vnet}-RndWrite-workload
        echo 'rd=SeqRead-4,wd=SeqRead,sd=("sd*"),threads=20' >> vnet${vnet}-SeqRead-workload
        echo 'rd=SeqWrite-26,wd=SeqWrite,sd=("sd*"),threads=26' >> vnet${vnet}-SeqWrite-workload
        echo 'rd=SeqWrite-32,wd=SeqWrite,sd=("sd*"),threads=32' >> vnet${vnet}-SeqWrite-workload
        echo 'rd=SeqWrite-40,wd=SeqWrite,sd=("sd*"),threads=40' >> vnet${vnet}-SeqWrite-workload
        echo 'rd=RndMix,wd=RndMix,sd=("sd*"),threads=25' >> vnet${vnet}-RndMix-workload
        echo 'rd=SeqMix-6,wd=SeqMix,sd=("sd*"),threads=6' >> vnet${vnet}-SeqMix-workload
        echo 'rd=SeqMix-7,wd=SeqMix,sd=("sd*"),threads=7' >> vnet${vnet}-SeqMix-workload
    done
