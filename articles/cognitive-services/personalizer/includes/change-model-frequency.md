---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: include
ms.custom: include file
ms.date: 01/15/2020
ms.author: diberry
ms.openlocfilehash: 7ef219e6b5f7547029612ec3898efec51abd4712
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76122748"
---
## <a name="change-the-model-update-frequency"></a>Cambio de la frecuencia de actualización del modelo

En la página **Configuración** del recurso Personalizer de Azure Portal, cambie el valor de **Model update frequency** (Frecuencia de actualización del modelo) a 10 segundos. Con esta duración breve, el modelo se entrenará rápidamente, lo que permite ver cómo cambia la acción principal en cada iteración.

![Cambiar la frecuencia de actualización del modelo](../media/settings/configure-model-update-frequency-settings.png)

La primera vez que se instancia un bucle de Personalizer, no hay ningún modelo porque no se ha producido ninguna llamada a API Reward a partir de la que realizar el entrenamiento. Las llamadas de Rank devolverán las mismas probabilidades para cada elemento. La aplicación siempre debe clasificar el contenido mediante la salida de RewardActionId.