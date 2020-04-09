---
title: Preguntas más frecuentes sobre el rendimiento de SMB para Azure NetApp Files | Microsoft Docs
description: Respuestas a algunas preguntas más frecuentes sobre el rendimiento de SMB para Azure NetApp Files
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
ms.date: 03/17/2020
ms.author: b-juche
ms.openlocfilehash: 24b3710861f0ee158619ae9103584dcdb181f3d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460456"
---
# <a name="faqs-about-smb-performance-for-azure-netapp-files"></a>Preguntas más frecuentes sobre el rendimiento de SMB para Azure NetApp Files

En este artículo se responden algunas preguntas frecuentes sobre los procedimientos recomendados relativos al rendimiento de SMB para Azure NetApp Files.

## <a name="is-smb-multichannel-enabled-in-smb-shares"></a>¿Está habilitado SMB multicanal en recursos compartidos SMB? 

Sí, SMB multicanal está habilitado de forma predeterminada, un cambio que entró en vigor a comienzos de enero de 2020. Todos los recursos compartidos anteriores de los volúmenes SMB existentes tienen la característica habilitada, y todos los volúmenes recién creados también tendrán la característica habilitada en el momento de la creación. 

Cualquier conexión con SMB establecida antes de la habilitación de la característica tendrá que restablecerse para aprovechar las ventajas de la funcionalidad de SMB multicanal. Para restablecerlo, puede desconectar y volver a conectar el recurso compartido de SMB.

## <a name="is-rss-supported"></a>¿Se admite RSS?

Sí, Azure NetApp Files admite el escalado de recepción (RSS).

Con SMB multicanal habilitado, un cliente de SMB3 establece varias conexiones TCP en el servidor SMB de Azure NetApp Files mediante una tarjeta de interfaz de red (NIC) que es compatible con RSS. 

## <a name="which-windows-versions-support-smb-multichannel"></a>¿Qué versiones de Windows admiten SMB multicanal?

Windows admite SMB multicanal desde Windows 2012 para permitir un mejor rendimiento.  Consulte [Implementación de SMB multicanal](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3Dws.11)) y [Aspectos básicos de SMB multicanal](https://blogs.technet.microsoft.com/josebda/2012/06/28/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0/) para más información. 


## <a name="does-my-azure-virtual-machine-support-rss"></a>¿Mi máquina virtual de Azure admite RSS?

Para ver si las NIC de las máquinas virtuales de Azure admiten RSS, ejecute el comando `Get-SmbClientNetworkInterface`, tal como se indica a continuación, y compruebe el campo `RSS Capable`: 

![Compatibilidad de RSS con las máquinas virtuales de Azure](../media/azure-netapp-files/azure-netapp-files-formance-rss-support.png)

## <a name="does-azure-netapp-files-support-smb-direct"></a>¿Azure NetApp Files admite SMB directo?

No, Azure NetApp Files no admite SMB directo. 

## <a name="what-is-the-benefit-of-smb-multichannel"></a>¿Cuál es la ventaja de SMB multicanal? 

La característica SMB multicanal permite a un cliente de SMB3 establecer un grupo de conexiones mediante una sola tarjeta de interfaz de red (NIC) o varias y usarlas para enviar solicitudes para una única sesión de SMB. Por el contrario, por diseño, SMB1 y SMB2 requieren que el cliente establezca una conexión y envíe todo el tráfico de SMB para una sesión determinada por esa conexión. Esta conexión única limita el rendimiento general del protocolo que se puede lograr desde un solo cliente.

## <a name="should-i-configure-multiple-nics-on-my-client-for-smb"></a>¿Debo configurar varias NIC en mi cliente para SMB?

No. El cliente SMB coincidirá con el número de NIC que devuelve el servidor SMB.  Cada volumen de almacenamiento es accesible desde un solo punto de conexión de almacenamiento.  Esto significa que solo se usará una NIC para cualquier relación SMB determinada.  

Como se muestra en la salida de `Get-SmbClientNetworkInterace` siguiente, la máquina virtual tiene dos interfaces de red: 15 y 12.  Como se muestra a continuación bajo el comando `Get-SmbMultichannelConnection`, aunque hay dos NIC compatibles con RSS, solo se usa la interfaz 12 en relación con el recurso compartido de SMB; la interfaz 15 no está en uso.

![Tarjetas NIC compatibles con RSS](../media/azure-netapp-files/azure-netapp-files-rss-capable-nics.png)

## <a name="is-nic-teaming-supported-in-azure"></a>¿Se admite la formación de equipos NIC en Azure?

La formación de equipos NIC no se admite en Azure. Aunque se admiten varias interfaces de red en las máquinas virtuales de Azure, representan una construcción lógica más que física. Como tal, no proporcionan tolerancia a errores.  Además, el ancho de banda disponible para una máquina virtual de Azure se calcula para el propio equipo y no para ninguna interfaz de red individual.

## <a name="whats-the-performance-like-for-smb-multichannel"></a>¿Cómo es el rendimiento para SMB multicanal?

Las siguientes pruebas y gráficos muestran la eficacia de SMB multicanal en cargas de trabajo de instancia única.

### <a name="random-io"></a>E/S aleatoria  

Con SMB multicanal deshabilitado en el cliente, las pruebas de lectura y escritura de 8 KiB puras se realizaron con FIO y un conjunto de trabajo 40 GiB.  El recurso compartido de SMB se desasoció entre cada prueba, con incrementos del recuento de conexiones de cliente SMB por configuración de interfaz de red RSS de `1`,`4`,`8`,`16``set-SmbClientConfiguration -ConnectionCountPerRSSNetworkInterface <count>`. Las pruebas muestran que el valor predeterminado de `4` es suficiente para cargas de trabajo intensivas de E/S; el incremento en `8` y `16` no tiene ningún efecto. 

El comando `netstat -na | findstr 445` ha mostrado que se establecieron conexiones adicionales con incrementos de `1` a `4` a `8` y a `16`.  Se usaron completamente cuatro núcleos de CPU para SMB durante cada prueba, como confirma la estadística `Per Processor Network Activity Cycles` de Perfmon (no se incluye en este artículo).

![Pruebas de E/S aleatorias](../media/azure-netapp-files/azure-netapp-files-random-io-tests.png)

La máquina virtual de Azure no afecta a los límites de E/S de almacenamiento SMB (ni a NFS).  Como se muestra a continuación, el tipo de instancia D16 tiene una tasa limitada de 32 000 para IOPS de almacenamiento en caché y 25 600 para IOPS de almacenamiento no en caché.  Sin embargo, el gráfico anterior muestra significativamente más E/S sobre SMB.

![Comparación de E/S aleatoria](../media/azure-netapp-files/azure-netapp-files-random-io-tests-list.png)

### <a name="sequential-io"></a>E/S secuencial 

Las pruebas similares a las pruebas de E/S aleatorias descritas anteriormente se realizaron con la E/S secuencial de 64 KiB. Aunque los aumentos en el recuento de conexiones de cliente por interfaz de red RSS más allá de 4' no tenían ningún efecto apreciable en la E/S aleatoria, no se aplica a la E/S secuencial. Como se muestra en el siguiente gráfico, cada aumento se asocia con un aumento correspondiente en el rendimiento de lectura. El rendimiento de escritura seguía siendo plano debido a las restricciones de ancho de banda de red colocadas por Azure para cada tipo de instancia o tamaño. 

![Pruebas de E/S secuencial](../media/azure-netapp-files/azure-netapp-files-sequential-io-tests.png)

Azure coloca los límites de velocidad de red en cada tipo o tamaño de máquina virtual. El límite de velocidad se impone solo en el tráfico de salida. El número de NIC presentes en una máquina virtual no afecta a la cantidad total de ancho de banda disponible para el equipo.  Por ejemplo, el tipo de instancia D16 tiene un límite de red impuesto de 8000 Mbps (1000 MiB/s).  Como se muestra en el gráfico secuencial anterior, el límite afecta al tráfico saliente (escrituras), pero no a las lecturas multicanal.

![Comparación de E/S secuencial](../media/azure-netapp-files/azure-netapp-files-sequential-io-tests-list.png)

## <a name="is-accelerated-networking-recommended"></a>¿Se recomienda el uso de la opción Redes aceleradas?

Para obtener el máximo rendimiento, se recomienda configurar [Redes aceleradas](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell) siempre que sea posible. Tenga en cuenta las siguientes consideraciones:  

* Azure Portal habilita Redes aceleradas de forma predeterminada para las máquinas virtuales que admiten esta característica.  Sin embargo, es posible que no se puedan aplicar otros métodos de implementación como Ansible y herramientas de configuración similares.  Si no se habilita Redes aceleradas, el rendimiento de la máquina puede verse afectado.  
* Si la opción Redes aceleradas no está habilitada en la interfaz de red de una máquina virtual debido a su falta de compatibilidad con un tipo o tamaño de instancia, permanecerá deshabilitada con tipos de instancia mayores. En esos casos, necesitará una intervención manual.

## <a name="are-jumbo-frames-supported"></a>¿Se admiten tramas gigantes?

No se admiten tramas gigantes con máquinas virtuales de Azure.

## <a name="is-smb-signing-supported"></a>¿Se admite la firma SMB? 

El protocolo de SMB ofrece la base para el uso compartido de impresión y archivos, así como de otras muchas operaciones de redes, como la administración de Windows remota. Para evitar los ataques de tipo "Man in the middle" que modifican los paquetes SMB en tránsito, el protocolo de SMB admite la firma digital de los paquetes SMB. 

La firma SMB admite todas las versiones del protocolo SMB compatibles con Azure NetApp Files. 

## <a name="what-is-the-performance-impact-of-smb-signing"></a>¿Cómo afecta al rendimiento el uso de la firma SMB?  

La firma SMB tiene un efecto perjudicial sobre el rendimiento de SMB. Entre otras posibles causas de la degradación del rendimiento, la firma digital de cada paquete consume CPU del cliente adicional como se muestra en la salida de Perfmon siguiente. En este caso, el núcleo 0 aparece como responsable de SMB, incluida la firma SMB.  Una comparación con los números de rendimiento de lectura secuencial no multicanal en la sección anterior muestra que la firma SMB reduce el rendimiento general de 875 MiB/s a aproximadamente 250 MiB/s. 

![Impacto en el rendimiento de la firma SMB](../media/azure-netapp-files/azure-netapp-files-smb-signing-performance.png)


## <a name="next-steps"></a>Pasos siguientes  

- [Preguntas más frecuentes acerca de Azure NetApp Files](azure-netapp-files-faqs.md)
- Consulte [Azure NetApp Files: Recursos compartidos de archivos empresariales administrados para cargas de trabajo SMB](https://cloud.netapp.com/hubfs/Resources/ANF%20SMB%20Quickstart%20doc%20-%2027-Aug-2019.pdf?__hstc=177456119.bb186880ac5cfbb6108d962fcef99615.1550595766408.1573471687088.1573477411104.328&__hssc=177456119.1.1573486285424&__hsfp=1115680788&hsCtaTracking=cd03aeb4-7f3a-4458-8680-1ddeae3f045e%7C5d5c041f-29b4-44c3-9096-b46a0a15b9b1) sobre el uso de recursos compartidos de archivos SMB con Azure NetApp Files.
