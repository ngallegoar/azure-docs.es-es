---
title: 'Conceptos de la herramienta de revisión: Content Moderator'
titleSuffix: Azure Cognitive Services
description: Conozca la herramienta de revisión de Content Moderator, un sitio web que coordina una labor combinada de moderación de revisiones humanas e inteligencia artificial.
services: cognitive-services
author: PatrickFarley
manager: mikemcca
ms.date: 03/15/2019
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.author: pafarley
ms.openlocfilehash: a23e6d46ee6e79fd7a5cabf4434c561f7d83b31b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "76169501"
---
# <a name="content-moderator-review-tool"></a>Herramienta de revisión de Content Moderator

Azure Content Moderator proporciona servicios para combinar la moderación de contenido de aprendizaje automático con revisiones humanas, y el sitio web de la [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com) es un front-end fácil de usar que ofrece acceso detallado a estos servicios.

![El panel de la herramienta de revisión en un explorador](./images/0-dashboard.png)

## <a name="what-it-does"></a>Qué hace

La [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com), cuando se usa en combinación con las API de moderación asistida por máquina, permite acometer las siguientes tareas en el proceso de moderación de contenido:

- Use un conjunto de herramientas para moderar el contenido en varios formatos (texto, imágenes y vídeo).
- Automatice la creación de [revisiones](../review-api.md#reviews) humanas cuando lleguen los resultados de la API de moderación.
- Asigne o escale las revisiones de contenido a varios equipos de revisión organizados por categoría de contenido o nivel de experiencia.
- Use filtros de lógica personalizados o predeterminados ([flujos de trabajo](../review-api.md#workflows)) para ordenar el contenido y realizar su seguimiento, sin escribir código.
- Use [conectores](./configure.md#connectors) para procesar el contenido con Microsoft PhotoDNA, Text Analytics y el servicio Face, además de las API de Content Moderator.
- Construya su propio conector para crear flujos de trabajo para cualquier API o proceso de negocio.
- Obtenga métricas clave de rendimiento de los procesos de moderación de contenido.

## <a name="review-tool-dashboard"></a>Panel de la herramienta de revisión

En la pestaña **Dashboard** (Panel), puede ver las métricas clave de las revisiones de contenido realizadas dentro de la herramienta. Vea el número de revisiones totales, completas y pendientes del contenido de imágenes, texto y vídeo. También puede ver el desglose de usuarios y equipos que han completado revisiones, así como las etiquetas de moderación que se han aplicado.

![Visualización del panel](images/0-dashboard.png)

## <a name="review-tool-credentials"></a>Credenciales de la herramienta de revisión

Cuando se registra en la [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com), se pedirá que seleccione una región de Azure para que la cuenta. El motivo es que la [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com) genera una clave de prueba gratuita para los servicios de Azure Content Moderator; esta clave la necesitará para acceder a cualquiera de los servicios desde una llamada de REST o SDK de cliente. Para ver su clave y la dirección URL del punto de conexión de API, seleccione **Settings** > **Credentials** (Configuración > Credenciales).

![Credenciales de Content Moderator](images/settings-6-credentials.png)

## <a name="next-steps"></a>Pasos siguientes

Consulte [Configuración de la herramienta de revisión](./configure.md) para saber cómo acceder a los recursos de la herramienta de revisión y cambiar la configuración.