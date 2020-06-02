---
title: 'Inicio rápido: Uso de comandos personalizados con Voz personalizada (versión preliminar): servicio de voz'
titleSuffix: Azure Cognitive Services
description: En este artículo, especificará la voz de salida de una aplicación de comandos personalizados.
services: cognitive-services
author: anhoang
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: anhoang
ms.openlocfilehash: c2b9b4d51e89975d988ed94bf85695bd8a1cc770
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82872474"
---
# <a name="quickstart-use-custom-commands-with-custom-voice-preview"></a>Inicio rápido: Uso de comandos personalizados con Voz personalizada (versión preliminar)

En el [artículo anterior](./quickstart-custom-speech-commands-create-parameters.md), creamos un nuevo proyecto de comandos personalizados para responder a comandos con parámetros.

En este artículo, seleccionaremos una voz de salida personalizada para la aplicación que creamos.

## <a name="select-a-custom-voice"></a>Selección de una voz personalizada

1. Abra el proyecto [creado anteriormente](./quickstart-custom-speech-commands-create-parameters.md).
1. Seleccione **Configuración** en el panel izquierdo.
1. Seleccione **Voz personalizada** en el panel central.
1. Seleccione la voz pública o personalizada deseada en la tabla.
1. Seleccione **Guardar**.

> [!div class="mx-imgBorder"]
> ![Oraciones de ejemplo con parámetros](media/custom-speech-commands/select-custom-voice.png)

> [!NOTE]
> - Para **Public voices** (Voces públicas), **Neural types** (Tipos neuronales) solo está disponible para regiones específicas. Para comprobar la disponibilidad, consulte las [voces estándar y neuronales por región o punto de conexión](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#standard-and-neural-voices).
> - Para **Custom voices** (Voces personalizadas), se pueden crear desde la página de proyecto Custom Voice (Voz personalizada). Consulte [Introducción a Voz personalizada](./how-to-custom-voice.md).

Ahora la aplicación responderá en la voz seleccionada, en lugar de en la voz predeterminada.

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Inicio rápido: conexión a una aplicación de comandos personalizados con el SDK de Voz (versión preliminar)](./quickstart-custom-speech-commands-speech-sdk.md)

