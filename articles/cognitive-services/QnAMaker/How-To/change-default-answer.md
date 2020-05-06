---
title: 'Obtención de la respuesta predeterminada: QnA Maker'
description: Se devuelve la respuesta predeterminada cuando no hay ninguna coincidencia con la pregunta. Puede que desee cambiar la respuesta predeterminada de la respuesta predeterminada estándar.
ms.topic: how-to
ms.date: 04/22/2020
ms.openlocfilehash: db5a79ec612a73066ac37365a1815841fafb3862
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82097105"
---
# <a name="change-default-answer-for-a-qna-maker-resource"></a>Cambiar la respuesta predeterminada para un recurso de QnA Maker

Se devuelve la respuesta predeterminada cuando no hay ninguna coincidencia con la pregunta. Puede que desee cambiar la respuesta predeterminada de la respuesta predeterminada estándar.

## <a name="change-default-answer-in-the-azure-portal"></a>Cambiar la respuesta predeterminada en Azure Portal

1. Vaya a [Azure Portal](https://portal.azure.com) y navegue hasta el grupo de recursos que representa el servicio QnA Maker que creó.

2. Haga clic para abrir **App Service**.

    ![En Azure Portal, acceda a App Service para QnA Maker](../media/qnamaker-concepts-confidencescore/set-default-response.png)

3. Haga clic en **Configuración de la aplicación** y edite el campo **DefaultAnswer** a la respuesta predeterminada deseada. Haga clic en **Save**(Guardar).

    ![Seleccione Configuración de la aplicación y, a continuación, edite el valor de DefaultAnswer para QnA Maker](../media/qnamaker-concepts-confidencescore/change-response.png)

4. Reiniciar App Service

    ![Después de cambiar DefaultAnswer, reinicie la instancia de App Service de QnA Maker](../media/qnamaker-faq/qnamaker-appservice-restart.png)

## <a name="next-steps"></a>Pasos siguientes

* [Creación de una base de conocimientos](../How-to/manage-knowledge-bases.md)