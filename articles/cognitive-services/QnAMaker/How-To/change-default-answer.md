---
title: 'Obtención de la respuesta predeterminada: QnA Maker'
description: Se devuelve la respuesta predeterminada cuando no hay ninguna coincidencia con la pregunta. Puede que desee cambiar la respuesta predeterminada de la respuesta predeterminada estándar.
ms.topic: conceptual
ms.date: 01/10/2020
ms.openlocfilehash: fae5c38fd64435a3fae56862bad04e000916e88b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "76843282"
---
# <a name="set-default-answer-for-a-knowledge-base"></a>Establecimiento de la respuesta predeterminada para una base de conocimiento

Se devuelve la respuesta predeterminada cuando no hay ninguna coincidencia con la pregunta. Puede que desee cambiar la respuesta predeterminada de la respuesta predeterminada estándar.

## <a name="change-default-answer"></a>Cambio de la respuesta predeterminada

1. Vaya a [Azure Portal](https://portal.azure.com) y navegue hasta el grupo de recursos que representa el servicio QnA Maker que creó.

2. Haga clic para abrir **App Service**.

    ![En Azure Portal, acceda a App Service para QnA Maker](../media/qnamaker-concepts-confidencescore/set-default-response.png)

3. Haga clic en **Configuración de la aplicación** y edite el campo **DefaultAnswer** a la respuesta predeterminada deseada. Haga clic en **Save**(Guardar).

    ![Seleccione Configuración de la aplicación y, a continuación, edite el valor de DefaultAnswer para QnA Maker](../media/qnamaker-concepts-confidencescore/change-response.png)

4. Reiniciar App Service

    ![Después de cambiar DefaultAnswer, reinicie la instancia de App Service de QnA Maker](../media/qnamaker-faq/qnamaker-appservice-restart.png)

## <a name="next-steps"></a>Pasos siguientes

* [Creación de un bot con QnA Maker y LUIS](../tutorials/integrate-qnamaker-luis.md)