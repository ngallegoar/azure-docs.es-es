---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: albecker1
ms.service: virtual-machines
ms.topic: include
ms.date: 07/07/2020
ms.author: albecker1
ms.custom: include file
ms.openlocfilehash: 66380e0bab0a83d086ebebb5e595908cecd11643
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89663364"
---
Este artículo ayuda a aclarar el rendimiento de disco y su funcionamiento con cuando se combinan Azure Virtual Machines y Azure Disks. En él, se describe también cómo diagnosticar cuellos de botella en la E/S del disco y los cambios que puede realizar para optimizar el rendimiento.

## <a name="how-does-disk-performance-work"></a>Funcionamiento del rendimiento
Las máquinas virtuales de Azure tienen límites de rendimiento y de E/S por segundo en función del tipo y tamaño de la máquina virtual. Los discos de sistema operativo y los discos de datos, que se pueden conectar a máquinas virtuales, tienen sus propios límites de rendimiento y de E/S por segundo. Cuando una aplicación que se ejecuta en las máquinas virtuales solicita más E/S por segundo o rendimiento que los que se asigna a la máquina virtual o a los discos conectados, se limita el rendimiento de la aplicación. Si esto ocurre, la aplicación experimentará un rendimiento que está lejos de ser óptimo y hasta pueden producirse algunas consecuencias negativas, como un aumento de la latencia. Vamos a ejecutar un par de ejemplos para solidificar esta información. Para facilitar el seguimiento de los ejemplos, solo veremos la E/S por segundo, pero la misma lógica se aplica al rendimiento.

## <a name="disk-io-capping"></a>Limitación de E/S de disco
Configuración:
- Standard_D8s_v3 
    - E/S por segundo no almacenadas en caché: 12.800
- Disco de SO E30
    - E/S por segundo: 500 
- 2 discos de disco de datos E30
    - E/S por segundo: 500

![Limitación del nivel de disco](media/vm-disk-performance/disk-level-throttling.jpg)

La aplicación que se ejecuta en la máquina virtual realiza una solicitud que requiere 10 000 E/S por segundo para la máquina virtual. La máquina virtual permite todas ellas por que la máquina virtual Standard_D8s_v3 puede ejecutar hasta 12 800 E/S por segundo. Esas 10 000 solicitudes de E/S se dividen en tres solicitudes diferentes para los distintos discos. Se solicitan 1000 E/S por segundo al disco del sistema operativo y se 4.500 a cada disco de datos. Dado que todos los discos conectados son E30 y solo pueden controlar 500 E/S por segundo, responden con 500 E/S por segundo cada uno. Luego, los discos conectados limitan el rendimiento de la aplicación y solo puede procesar 1500 E/S por segundo. Podría funcionar a un rendimiento máximo de 10 000 E/S por segundo si se usaran discos de mejor rendimiento, como discos P30 SSD Premium.

## <a name="virtual-machine-io-capping"></a>Límite de E/S de máquinas virtuales
Configuración:
- Standard_D8s_v3 
    - E/S por segundo no almacenadas en caché: 12.800
- Disco de SO P30
    - E/S por segundo: 5.000 
- Dos discos de datos P30 
    - E/S por segundo: 5.000

![Limitación del nivel de máquina virtual](media/vm-disk-performance/vm-level-throttling.jpg)

La aplicación que se ejecuta en la máquina virtual realiza una solicitud que requiere 15 000 E/S por segundo. Desafortunadamente, la máquina virtual Standard_D8s_v3 solo está aprovisionada para controlar 12 800 E/S por segundo. A partir de esa cifra, la máquina virtual limita la aplicación y debe asignar las 12 800 E/S por segundo. Luego, las 12 800 E/S por segundo solicitadas se dividen en tres solicitudes diferentes para los distintos discos. Se solicitan 4267 E/S por segundo al disco del sistema operativo y se 4266 a cada disco de datos. Dado que todos los discos conectados son P30, que pueden controlar 5000 E/S por segundo, responden con sus cantidades solicitadas.