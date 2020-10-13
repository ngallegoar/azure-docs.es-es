---
title: Consideraciones sobre el rendimiento de Azure NetApp Files | Microsoft Docs
description: Obtenga información sobre el rendimiento de Azure NetApp Files, incluida la relación de la cuota y el límite de rendimiento, y cómo aumentar o reducir la cuota de volumen de forma dinámica.
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
ms.date: 09/22/2020
ms.author: b-juche
ms.openlocfilehash: edfebe3d9470defbe70b3694d5574e58ca3b5938
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91325528"
---
# <a name="performance-considerations-for-azure-netapp-files"></a>Consideraciones sobre el rendimiento de Azure NetApp Files

El [límite de rendimiento](azure-netapp-files-service-levels.md) de un volumen con QoS automático se determina mediante una combinación de la cuota asignada al volumen y el nivel de servicio seleccionado. En el caso de los volúmenes con QoS manual, el límite de rendimiento se puede definir individualmente. Cuando realiza planes de rendimiento de Azure NetApp Files, debe tener en cuenta varias consideraciones. 

## <a name="quota-and-throughput"></a>Cuotas y rendimiento  

El límite de rendimiento es solo un factor determinante del rendimiento real que se realizará.  

Las consideraciones típicas de rendimiento del almacenamiento, incluyendo la mezcla de lectura y escritura, el tamaño de transferencia, los patrones aleatorios o secuenciales y muchos otros factores, contribuirán al rendimiento total entregado.  

El rendimiento empírico máximo que se ha observado en las pruebas es de 4500 MiB/s.  En la capa de almacenamiento Premium, una cuota de volumen con QoS automático de 70,31 TiB aprovisionará un límite de rendimiento que sea lo suficientemente alto para alcanzar ese nivel de rendimiento.  

En el caso de los volúmenes con QoS automático, si quiere asignar cantidades de cuota de volumen más allá de los 70,31 TiB, puede asignar una cuota adicional a un volumen para almacenar datos adicionales. Sin embargo, la cuota adicional no dará como resultado un aumento adicional en el rendimiento real.  

El mismo límite de rendimiento empírico se aplica a los volúmenes con QoS manual. El rendimiento máximo que se puede asignar a un volumen es de 4500 MiB/s.

## <a name="automatic-qos-volume-quota-and-throughput"></a>Cuota de volumen y rendimiento con QoS automático

En esta sección se describe la administración de cuotas y el rendimiento de los volúmenes con el tipo de QoS automático.

### <a name="overprovisioning-the-volume-quota"></a>Aprovisionamiento excesivo de la cuota de volumen

Si el rendimiento de una carga de trabajo se limita en función del límite de rendimiento, es posible aprovisionar en exceso la cuota de volumen con QoS automático para establecer un nivel de rendimiento más alto y así lograr un mayor rendimiento.  

Por ejemplo, si un volumen con QoS automático en la capa de almacenamiento Premium tiene solo 500 GiB de datos pero requiere 128 MiB/s de rendimiento, puede establecer la cuota en 2 TiB para que el nivel de rendimiento se establezca en consecuencia (64 MiB/s por TB * 2 TiB = 128 MiB/s).  

Si aprovisiona constantemente un volumen en exceso para lograr un rendimiento más alto, puede usar volúmenes con QoS manual o un nivel de servicio más alto en su lugar.  En el ejemplo anterior, puede alcanzar el mismo límite de rendimiento con la mitad de la cuota de volumen con QoS automático usando el nivel de almacenamiento Ultra en su lugar (128 MiB/s por TiB * 1 TiB = 128 MiB/s).

### <a name="dynamically-increasing-or-decreasing-volume-quota"></a>Aumentar o disminuir dinámicamente la cuota de volumen

Si sus requisitos de rendimiento son temporales o si ha aumentado las necesidades de rendimiento durante un período de tiempo fijo, puede aumentar o disminuir de forma dinámica la cuota de volumen para ajustar instantáneamente el límite de rendimiento.  Tenga en cuenta las siguientes consideraciones: 

* La cuota de volumen se puede aumentar o disminuir sin necesidad de pausar E/S, y el acceso al volumen no se interrumpe ni se ve afectado.  

    Puede ajustar la cuota durante una transacción activa de E/S a un volumen.  Tenga en cuenta que la cuota de volumen nunca puede reducirse por debajo de la cantidad de datos lógicos almacenados en el volumen.

* Cuando se cambia la cuota de volumen, el cambio correspondiente en el límite de rendimiento es casi instantáneo. 

    El cambio no interrumpe ni afecta el acceso al volumen o al E/S.  

* El ajuste de cuota de volumen podría requerir un cambio en el tamaño del grupo de capacidad.  

    El tamaño del grupo de capacidad se puede ajustar dinámicamente y sin que afecte a la disponibilidad de volumen o E/S.

## <a name="manual-qos-volume-quota-and-throughput"></a>Cuota de volumen y rendimiento con QoS manual 

Si usa volúmenes con QoS manual, no tiene que aprovisionar en exceso la cuota de volumen para lograr un mayor rendimiento, ya que el rendimiento se puede asignar a cada volumen de forma independiente. Sin embargo, debe asegurarse de que el grupo de capacidad se ha aprovisionado previamente con un rendimiento suficiente para las necesidades de rendimiento. El rendimiento de un grupo de capacidad se aprovisiona según su tamaño y nivel de servicio. Para más información, consulte [Niveles de servicio para Azure NetApp Files](azure-netapp-files-service-levels.md).


## <a name="next-steps"></a>Pasos siguientes

- [Niveles de servicio para Azure NetApp Files](azure-netapp-files-service-levels.md)
- [Bancos de pruebas de rendimiento para Linux](performance-benchmarks-linux.md)