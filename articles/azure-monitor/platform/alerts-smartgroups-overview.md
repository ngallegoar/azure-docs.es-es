---
title: Grupos inteligentes
description: Los grupos inteligentes son agregaciones de las alertas que le ayudan a reducir el ruido de las alertas
ms.topic: conceptual
ms.subservice: alerts
ms.date: 05/15/2018
ms.openlocfilehash: 05b05f8bc079bb3768ac2f1a03593bc9260b41aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77665500"
---
# <a name="smart-groups"></a>Grupos inteligentes

Un desafío común al gestionar las alertas consiste en cribar el ruido para averiguar lo que realmente importa: los grupos inteligentes están diseñados para ser la solución a ese problema.  

Los grupos inteligentes se crean de manera automática a través de algoritmos de aprendizaje automático para combinar las alertas relacionadas que representan un solo problema.  Cuando se crea una alerta, el algoritmo la agrega a un grupo inteligente nuevo o a un grupo inteligente existente según información como los patrones históricos, las propiedades similares y la estructura similar. Por ejemplo, si el porcentaje de CPU en varias máquinas virtuales de una suscripción alcanza su máximo simultáneamente dando lugar a muchas alertas individuales y, si estas alertas se han producido juntas en cualquier momento pasado, es probable que se agrupen en un único grupo inteligente, lo que sugiere una posible causa principal común. Esto significa que para los usuarios que solucionan los problemas de las alertas, los grupos inteligentes no solo les permiten reducir el ruido mediante la administración de las alertas relacionadas como una única unidad agregada, sino que también les orientan hacia las posibles causas raíz comunes de sus alertas.

Actualmente, el algoritmo solo considera las alertas provenientes del mismo servicio de supervisión dentro de una suscripción. Los grupos inteligentes pueden reducir hasta un 99 por ciento de ruido de alertas a través de esta consolidación. Puede ver la razón por la que las alertas se incluyeron en un grupo en la página de detalles del grupo inteligente.

Puede ver los detalles de los grupos inteligentes y establecer el estado de manera similar a las alertas. Cada alerta forma parte de un solo grupo inteligente. 

## <a name="smart-group-state"></a>Estado de grupo inteligente

El estado de grupo inteligente es un concepto similar al estado de alerta, que le permite administrar el proceso de resolución en el nivel de un grupo inteligente. Al igual que el estado de alerta, cuando se crea un grupo inteligente, tiene el estado **Nuevo**, que se puede cambiar a **Confirmado** o **Cerrado**.

Se admiten los siguientes estados de grupo inteligente.

| State | Descripción |
|:---|:---|
| Nuevo | Se acaba de detectar el problema y todavía no se ha revisado. |
| Confirmado | Un administrador revisó el grupo inteligente y empezó a trabajar en él. |
| Closed | Se resolvió el problema. Después de cerrar un grupo inteligente, puede volver a abrirlo mediante el cambio a otro estado. |

[Obtenga información sobre cómo cambiar el estado del grupo inteligente](https://aka.ms/managing-alert-smart-group-states).

> [!NOTE]
>  El cambio de estado de un grupo inteligente no modifica el estado de las alertas individuales que lo componen.

## <a name="smart-group-details-page"></a>Página de detalles del grupo inteligente

La página de detalles del grupo inteligente se muestra al seleccionar un grupo inteligente. Proporciona detalles acerca del grupo inteligente, incluido el razonamiento que se usó para crear el grupo, y le permite cambiar su estado.
 
![Detalles del grupo inteligente](media/alerts-smartgroups-overview/smart-group-detail.png)


La página de detalles del grupo inteligente incluye las secciones siguientes.

| Sección | Descripción |
|:---|:---|
| Alertas | Muestra las alertas individuales que se incluyen en el grupo inteligente. Seleccione una alerta para abrir su página de detalles de la alerta. |
| Historial | Muestra cada acción que realiza el grupo inteligente y los cambios hechos en él. Se limita actualmente a los cambios de estado y a los cambios de pertenencia de una alerta. |

## <a name="smart-group-taxonomy"></a>Taxonomía del grupo inteligente

El nombre de un grupo inteligente es el nombre de su primera alerta. No se puede crear ni cambiar el nombre de un grupo inteligente.

## <a name="next-steps"></a>Pasos siguientes

- [Administración de grupos inteligentes](https://aka.ms/managing-smart-groups)
- [Cambio de estado de alerta y de grupo inteligente](https://aka.ms/managing-alert-smart-group-states)


