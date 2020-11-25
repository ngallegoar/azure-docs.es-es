---
title: 'Instrucciones: Uso de comandos personalizados con Voz personalizada: servicio de voz'
titleSuffix: Azure Cognitive Services
description: En este artículo, especificará la voz de salida de una aplicación de comandos personalizados.
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/16/2020
ms.author: sausin
ms.openlocfilehash: 4a5c14909606dcb862fcf53d99bc5bc00fba63bd
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2020
ms.locfileid: "95025691"
---
# <a name="use-custom-commands-with-custom-voice"></a>Uso de Comandos personalizados con Voz personalizada

En este artículo, obtendrá información sobre cómo seleccionar una voz de salida personalizada para una aplicación de comandos personalizados.

## <a name="select-a-custom-voice"></a>Selección de una voz personalizada

1. En la aplicación de Comandos personalizados, seleccione **Configuración** en el panel izquierdo.
1. Seleccione **Voz personalizada** en el panel central.
1. Seleccione la voz pública o personalizada que desee en la tabla.
1. Seleccione **Guardar**.

> [!div class="mx-imgBorder"]
> ![Oraciones de ejemplo con parámetros](media/custom-commands/select-custom-voice.png)

> [!NOTE]
> - Para **Public voices** (Voces públicas), **Neural types** (Tipos neuronales) solo está disponible para regiones específicas. Para comprobar la disponibilidad, consulte las [voces estándar y neuronales por región o punto de conexión](./regions.md#standard-and-neural-voices).
> - Para **Custom voices** (Voces personalizadas), se pueden crear desde la página de proyecto Custom Voice (Voz personalizada). Consulte [Introducción a Voz personalizada](./how-to-custom-voice.md).

Ahora la aplicación responderá en la voz seleccionada, en lugar de en la voz predeterminada.