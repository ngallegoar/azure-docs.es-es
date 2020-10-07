---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: include
ms.date: 07/17/2019
ms.author: pafarley
ms.openlocfilehash: 07e7cc991f127bf4bb4f466c0108962786e45bce
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "68423579"
---
Observe el control deslizante **Probability Threshold** (Umbral de probabilidad) situado en el panel izquierdo de la pestaña **Performance** (Rendimiento). Este es el nivel de confianza que debe tener una predicción para que se considere correcta (para los fines de calcular la precisión y la coincidencia). 

Al interpretar llamadas de predicción con un umbral alto de probabilidad, tienden a devolver resultados muy precisos pero con una baja coincidencia; las clasificaciones detectadas son correctas, pero muchas siguen sin detectarse. Un umbral bajo de probabilidad tiene el efecto contrario: la mayoría de las clasificaciones reales se detectan, pero hay más falsos positivos en ese conjunto. Teniendo esto en cuenta, debe establecer el umbral de probabilidad según las necesidades específicas de su proyecto. Posteriormente, si va a recibir resultados de predicción en el cliente, debe usar el mismo valor de umbral de probabilidad que el empleado aquí.