---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/29/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 84736b7f1dcdf8b186fddbced5dd773e008c0dd2
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80887403"
---
La ráfaga de discos es compatible con SSD Premium. La ráfaga se admite en cualquier tamaño de disco SSD Premium <= 512 GiB (P20 o inferior). Estos tamaños de disco admiten la ampliación en la medida de lo posible y usan un sistema de crédito para administrarla. Los créditos se acumulan en un cubo de ráfagas siempre que el tráfico del disco está por debajo del objetivo de rendimiento aprovisionado para el tamaño del disco, y consume créditos cuando el tráfico supera el objetivo. Se realiza un seguimiento del tráfico contra IOPS y el ancho de banda en el objetivo aprovisionado. La seguridad de disco no sorteará las limitaciones de tamaño de la maquina virtual (VM) en IOPS o rendimiento.

La ampliación del disco está habilitada de forma predeterminada en las nuevas implementaciones de los tamaños del disco que la admiten. Los tamaños del disco existentes, si admiten la ampliación del disco, pueden habilitar la seguridad a través de cualquiera de los métodos siguientes:

- Desconecte el disco y vuelva a conectarlo.
- Detenga e inicie la VM.

## <a name="burst-states"></a>Estados de ráfaga

Todos los tamaños de disco aplicables de la ráfaga comenzarán con un cubo de crédito de ráfaga completo cuando el disco esté conectado a una máquina virtual. La duración máxima de la ampliación viene determinada por el tamaño del cubo de crédito de ráfaga. Solo se pueden acumular créditos no usados hasta el tamaño del cubo de crédito. En cualquier momento, el cubo de crédito de ráfaga del disco puede estar en uno de los tres estados siguientes: 

- Acumulación, cuando el tráfico del disco usa menos del objetivo de rendimiento aprovisionado. Puede acumular crédito si el tráfico del disco es superior a los objetivos de IOPS o ancho de banda, o de ambos. Todavía puede acumular créditos de E/S si está consumiendo ancho de banda del disco completo, y viceversa.  

- Rechazo, cuando el tráfico del disco usa más del objetivo de rendimiento aprovisionado. El tráfico de ráfaga consumirá créditos de IOPS o ancho de banda de forma independiente. 

- Constante restante, cuando el tráfico del disco se encuentra exactamente en el objetivo de rendimiento aprovisionado. 

En la tabla siguiente se resumen los tamaños del disco que proporcionan compatibilidad con la ampliación y las especificaciones de ráfaga.

## <a name="regional-availability"></a>Disponibilidad regional

La ráfaga de discos está disponible en todas las regiones de la nube pública.

## <a name="disk-sizes"></a>Tamaños de disco

[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

## <a name="example-scenarios"></a>Escenarios de ejemplo

Para obtener una idea más clara de cómo funciona, estos son algunos escenarios de ejemplo:

- Un escenario común que se puede beneficiar de la ampliación del disco es el arranque de VM más rápido y el inicio de aplicaciones en discos del sistema operativo. Tomemos una VM Linux con una imagen del sistema operativo de 8 GiB como ejemplo. Si usamos un disco P2 como disco del sistema operativo, el objetivo aprovisionado es de 120 IOPS y 25 MiB. Cuando se inicie la VM, se producirá un pico de lectura en el disco del sistema operativo que carga los archivos de arranque. Con la introducción de la ampliación, puede leer la velocidad máxima de ráfaga de 3500 IOPS y 170 MiB, lo que acelera el tiempo de carga 6 veces como mínimo. Después del arranque de la VM, el nivel de tráfico en el disco del sistema operativo suele ser bajo, ya que la mayoría de las operaciones de datos de la aplicación se realizarán en los discos de datos conectados. Si el tráfico está por debajo del objetivo aprovisionado, se acumularán créditos.

- Si hospeda un entorno de escritorio virtual remoto, siempre que un usuario activo inicie una aplicación como AutoCAD, el tráfico de lectura en el disco del sistema operativo aumentará significativamente. En este caso, el tráfico de ráfaga consumirá créditos acumulados, lo que le permitirá ir más allá del objetivo aprovisionado e iniciar la aplicación mucho más rápido.

- Un disco P1 tiene un objetivo aprovisionado de 120 IOPS y 25 MiB. Si el tráfico real del disco era de 100 IOPS y 20 MiB en el último intervalo de 1 segundo, los 20 E/S y 5 MB no usados se abonan en el cubo de ráfagas del disco. Los créditos del cubo de ráfagas se pueden usar posteriormente cuando el tráfico supera el objetivo aprovisionado hasta el límite máximo de ráfagas. El límite máximo de ráfagas define el límite superior del tráfico del disco, incluso si tiene créditos de ráfagas para consumir. En este caso, aunque tenga 10 000 IOPS en el cubo de crédito, un disco P1 no puede emitir más de la ráfaga máxima de 3500 IOPS.  
