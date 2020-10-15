---
title: Mapa del índice corporal en Azure Kinect
description: Aprenda a consultar un mapa del índice de seguimiento de personas en Azure Kinect DK.
author: qm13
ms.author: quentinm
ms.reviewer: yijwan
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: kinect, portabilidad, cuerpo, corporal, personas, seguimiento, índice, segmentación, mapa
ms.openlocfilehash: 8c128e59cef515aafb4f59794f0f644b90fd625d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85277030"
---
# <a name="azure-kinect-body-tracking-index-map"></a>Mapa del índice de seguimiento de personas de Azure Kinect

El mapa del índice corporal incluye el mapa de segmentación de la instancia para cada cuerpo en la captura de cámara de profundidad. Cada píxel se asigna al píxel correspondiente en la imagen de profundidad o IR. El valor de cada píxel representa a qué cuerpo pertenece el píxel. Puede ser el segundo plano (valor `K4ABT_BODY_INDEX_MAP_BACKGROUND`) o el índice de un `k4abt_body_t`detectado.

![Ejemplo de mapa del índice corporal](./media/concepts/body-index-map.png)

>[!NOTE]
> El índice corporal es diferente del identificador corporal. Para consultar el identificador corporal de un índice corporal dado, puede llamar a la API: [k4abt_frame_get_body_id()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga1d612404d133a279af847974e9359a92.html#ga1d612404d133a279af847974e9359a92).


## <a name="using-body-index-map"></a>Uso del mapa del índice corporal

El mapa del índice corporal se almacena como `k4a_image_t` y tiene la misma resolución que la imagen de profundidad o IR. Cada píxel es un valor de 8 bits. Se puede consultar desde un `k4abt_frame_t` llamando a `k4abt_frame_get_body_index_map`. El desarrollador es responsable de liberar la memoria para el mapa del índice corporal mediante una llamada a `k4a_image_release()`.

## <a name="next-steps"></a>Pasos siguientes

[Creación de la primera aplicación de seguimiento de personas](build-first-body-app.md)
