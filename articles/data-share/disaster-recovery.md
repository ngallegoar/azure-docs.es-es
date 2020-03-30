---
title: Recuperación ante desastres para Azure Data Share
description: Recuperación ante desastres para Azure Data Share
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 12/18/2019
ms.openlocfilehash: a736e3ddfcf785f9ce27140eed58374a0732c1f1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75475970"
---
# <a name="disaster-recovery-for-azure-data-share"></a>Recuperación ante desastres para Azure Data Share

En este artículo, veremos cómo configurar un entorno de recuperación ante desastres para Azure Data Share. Las interrupciones del centro de datos de Azure son poco frecuentes, pero pueden durar desde unos minutos hasta algunas horas. Las interrupciones del centro de datos pueden provocar interrupciones en los entornos que se basan en los datos compartidos por el proveedor de datos. Al seguir los pasos que se detallan en este artículo, los proveedores de datos pueden seguir compartiendo los datos con los consumidores de datos en caso de que se produzca una interrupción del centro de datos en la región primaria que hospeda el recurso compartido de datos. 

## <a name="achieving-business-continuity-for-azure-data-share"></a>Consecución de la continuidad empresarial para Azure Data Share

Para estar preparado para una interrupción del centro de datos, el proveedor de datos puede tener un entorno de recurso compartido de datos aprovisionado en una región secundaria. Hay medidas que se pueden realizar, lo que garantizará una conmutación por error fluida en caso de que se produzca una interrupción del centro de datos. 

Los proveedores de datos pueden aprovisionar recursos secundarios de Azure Data Share en una región adicional. Estos recursos de Data Share se pueden configurar para incluir conjuntos de datos que existen en el entorno de recurso compartido de datos principal. Los consumidores de datos se pueden agregar al recurso compartido de datos al configurar el entorno de recuperación ante desastres, o agregarse en un momento posterior en el tiempo (es decir, como parte de los pasos de una conmutación por error manual).

Si los consumidores de datos tienen una suscripción a recurso compartido activa en un entorno secundario aprovisionado con fines de recuperación ante desastres, pueden habilitar la programación de instantáneas como parte de una conmutación por error. Si los consumidores de datos no quieren suscribirse a una región secundaria para fines de recuperación ante desastres, se pueden invitar al recurso compartido de datos secundario en un momento posterior. 

Los consumidores de datos pueden tener una suscripción a recurso compartido activa que esté inactiva para propósitos de recuperación ante desastres, o bien los proveedores de datos pueden agregarlos en un momento posterior como parte de los procedimientos de conmutación por error manual. 

## <a name="related-information"></a>Información relacionada

- [Continuidad empresarial y recuperación ante desastres](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)
- [Creación de alta disponibilidad a su estrategia de continuidad empresarial y recuperación ante desastres](https://docs.microsoft.com/azure/architecture/solution-ideas/articles/build-high-availability-into-your-bcdr-strategy)

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de cómo empezar a compartir datos, vaya al tutorial que cubre cómo [compartir sus datos](share-your-data.md).




