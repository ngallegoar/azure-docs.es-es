---
title: 'Obtención de la respuesta predeterminada: QnA Maker'
description: Se devuelve la respuesta predeterminada cuando no hay ninguna coincidencia con la pregunta. Puede que desee cambiar la respuesta predeterminada de la respuesta predeterminada estándar.
ms.topic: how-to
ms.date: 07/02/2020
ms.openlocfilehash: 005442938167c1bf7927603c44d6f870795cbeee
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/06/2020
ms.locfileid: "85979984"
---
# <a name="change-default-answer-for-a-qna-maker-resource"></a>Cambiar la respuesta predeterminada para un recurso de QnA Maker

La respuesta predeterminada para una base de conocimiento está pensada para que se devuelva cuando no se encuentra una respuesta. Si usa una aplicación cliente, como [Azure Bot Service](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-qna?view=azure-bot-service-4.0&tabs=cs#calling-qna-maker-from-your-bot), es posible que también tenga una respuesta predeterminada independiente, lo que indica que ninguna respuesta ha alcanzado el umbral de puntuación.

## <a name="set-default-answer-when-you-create-knowledge-base"></a>Establecer la respuesta predeterminada al crear la base de conocimiento

Al crear una nueva base de conocimiento, el texto de respuesta predeterminado es uno de los valores de configuración. Si decide no establecerlo durante el proceso de creación, puede cambiarlo más adelante con el siguiente procedimiento.

## <a name="change-default-answer-in-qna-maker-portal"></a>Cambiar la respuesta predeterminada en el portal de QnA Maker

La respuesta predeterminada de la base de conocimiento se devuelve cuando no se devuelve ninguna respuesta del servicio QnA Maker.

1. Inicie sesión en el [portal de QnA Maker](https://www.qnamaker.ai/) y seleccione la base de conocimiento en la lista.
1. Seleccione **Configuración** en la barra de navegación.
1. Cambie el valor de **Default answer text** (Texto de respuesta predeterminado) en la sección **Manage knowledge base** (Administrar base de conocimiento).

    :::image type="content" source="../media/qnamaker-concepts-confidencescore/change-default-answer.png" alt-text="Captura de pantalla del portal de QnA Maker, página de configuración, con el cuadro de texto respuesta predeterminado resaltado.":::

1. Seleccione **Guardar y entrenar** para almacenar los cambios.

## <a name="next-steps"></a>Pasos siguientes

* [Creación de una base de conocimientos](../How-to/manage-knowledge-bases.md)