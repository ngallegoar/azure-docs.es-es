---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/18/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 8c5c0c8f649e7cbab2c16688717de1aaabfb93c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "75474114"
---
Esta referencia detalla los objetivos de escalabilidad y rendimiento de Azure Storage. Los objetivos de escalabilidad y rendimiento que mencionamos aquí son objetivos exigentes, pero se pueden lograr. En todos los casos, la velocidad de solicitudes y el ancho de banda obtenido por la cuenta de almacenamiento depende del tamaño de los objetos almacenados, de los patrones de acceso utilizados y del tipo de carga de trabajo que realiza la aplicación.

Asegúrese de probar el servicio para determinar si el rendimiento se ajusta a sus requisitos. Si es posible, evite picos en la tasa de tráfico y asegúrese de que este se distribuya equitativamente entre las particiones.

Cuando la aplicación alcanza el límite de lo que puede administrar una partición para la carga de trabajo, Azure Storage comienza a responder con el código de error 503 (servidor ocupado) o el código de error 500 (tiempo de espera de operación). Si se producen errores 503, considere la posibilidad de modificar la aplicación para utilizar una directiva de retroceso exponencial para los reintentos. El retroceso exponencial permite que disminuya la carga de la partición y evita los picos de tráfico en esa partición.
