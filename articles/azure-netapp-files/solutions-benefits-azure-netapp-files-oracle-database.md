---
title: Ventajas del uso de Azure NetApp Files con Oracle Database | Microsoft Docs
description: Se describe la tecnología y se proporciona una comparación del rendimiento entre Oracle Direct NFS (dNFS) y el cliente NFS tradicional. Se muestran las ventajas de usar dNFS con Azure NetApp Files.
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
ms.date: 04/23/2020
ms.author: b-juche
ms.openlocfilehash: 0f431015caf5bb3364fe5628a599f760c6318c47
ms.sourcegitcommit: 50802bffd56155f3b01bfb4ed009b70045131750
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91932505"
---
# <a name="benefits-of-using-azure-netapp-files-with-oracle-database"></a>Ventajas del uso de Azure NetApp Files con Oracle Database

Oracle Direct NFS (dNFS) eleva el rendimiento a cotas más altas que el propio controlador NFS del sistema operativo. En este artículo se explica la tecnología y se proporciona una comparación del rendimiento entre dNFS y el cliente NFS tradicional (NFS del kernel). También se muestran las ventajas y la facilidad de uso de dNFS con Azure NetApp Files.  

## <a name="how-oracle-direct-nfs-works"></a>Funcionamiento de Oracle Direct NFS

En el resumen siguiente se explica cómo funciona Oracle Direct NFS en un nivel alto:

* Oracle Direct NFS pasa por alto la caché del búfer del sistema operativo. Los datos se almacenan en caché solo una vez en el espacio de usuario, lo que elimina la sobrecarga de copias de memoria.  

* El cliente NFS tradicional usa un solo flujo de red, como se muestra a continuación:    

    ![Cliente NFS tradicional con un solo flujo de red](../media/azure-netapp-files/solutions-traditional-nfs-client-using-single-network-flow.png)

    Por el contrario, Oracle Direct NFS mejora el rendimiento al equilibrar la carga del tráfico de red entre varios flujos de red. Tal como se ha probado y se muestra a continuación, se establecieron dinámicamente 650 conexiones de red distintas mediante Oracle Database:  

    ![Mejora del rendimiento de Oracle Direct NFS](../media/azure-netapp-files/solutions-oracle-direct-nfs-performance-load-balancing.png)

En [Preguntas frecuentes de Oracle sobre Direct NFS](http://www.orafaq.com/wiki/Direct_NFS) se muestra que Oracle dNFS es un cliente de NFS optimizado. dNFS proporciona acceso rápido y escalable al almacenamiento de NFS que se encuentra en los dispositivos de almacenamiento NAS (accesibles mediante TCP/IP). Está integrado en el kernel de la base de datos, al igual que ASM, que se usa principalmente con el almacenamiento SAN o DAS. Por lo tanto, *la instrucción es usar dNFS al implementar el almacenamiento NAS y ASM al implementar el almacenamiento SAN.*

dNFS es la opción predeterminada en Oracle 18c.

dNFS está disponible a partir de Oracle Database 11g. En el diagrama siguiente se compara dNFS con NFS nativo. Cuando se usa dNFS, las bases de datos de Oracle que se ejecutan en una máquina virtual de Azure pueden generar más E/S que el cliente NFS nativo.

![Comparación de dNFS de Oracle y Azure NetApp Files con NFS nativo](../media/azure-netapp-files/solutions-oracle-azure-netapp-files-comparing-dnfs-native-nfs.png)

Puede habilitar o deshabilitar dNFS mediante la ejecución de dos comandos y el reinicio de la base de datos.

Para habilitar las siguientes opciones:  
`cd $ORACLE_HOME/rdbms/lib ; make -f ins_rdbms.mk dnfs_on`

Para deshabilitarlo:  
`cd $ORACLE_HOME/rdbms/lib ; make -f ins_rdbms.mk dnfs_off`

## <a name="azure-netapp-files-combined-with-oracle-direct-nfs"></a>Azure NetApp Files combinado con Oracle Direct NFS

Puede mejorar el rendimiento de Oracle dNFS con el servicio Azure NetApp Files. El servicio proporciona control total sobre el rendimiento de la aplicación y puede satisfacer las aplicaciones más exigentes. La combinación de Oracle dNFS con Azure NetApp Files proporciona una gran ventaja para las cargas de trabajo.

## <a name="next-steps"></a>Pasos siguientes

- [Arquitecturas de las soluciones con Azure NetApp Files](azure-netapp-files-solution-architectures.md)
- [Introducción a las aplicaciones y soluciones de Oracle en Azure](../virtual-machines/workloads/oracle/oracle-overview.md)