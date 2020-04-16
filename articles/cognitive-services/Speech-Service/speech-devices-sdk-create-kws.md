---
title: Creación de palabras clave personalizadas - Servicio de voz
titleSuffix: Azure Cognitive Services
description: El dispositivo siempre espera una palabra clave (o frase). Cuando el usuario dice la palabra clave, el dispositivo envía todo el audio subsiguiente a la nube, hasta que el usuario deja de hablar. La personalización de la palabra clave es una forma eficaz de diferenciar el dispositivo y de reforzar la marca.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/11/2019
ms.author: trbye
ms.openlocfilehash: 8e67d624c77eb838f7646731bbdedd8f97f81b96
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400057"
---
# <a name="create-a-custom-keyword-using-speech-studio"></a>Creación de una palabra clave personalizada mediante Speech Studio

El dispositivo siempre espera una palabra clave (o frase). Por ejemplo, "Hola Cortana" es una palabra clave para el asistente Cortana. Cuando el usuario dice la palabra clave, el dispositivo envía todo el audio subsiguiente a la nube, hasta que el usuario deja de hablar. La personalización de la palabra clave es una forma eficaz de diferenciar el dispositivo y de reforzar la marca.

En este artículo, va a aprender a crear una palabra clave personalizada para el dispositivo.

## <a name="create-your-keyword"></a>Creación de la palabra clave

Antes de poder usar una palabra clave personalizada, deberá crear una palabra clave con la página [Custom Keyword](https://aka.ms/sdsdk-wakewordportal) (Palabra clave personalizada) en [Speech Studio](https://aka.ms/sdsdk-speechportal). Después de proporcionar una palabra clave, se genera un archivo que se implementa en el dispositivo.

1. Vaya a [Speech Studio](https://aka.ms/sdsdk-speechportal) e **inicie sesión** o, si todavía no tiene una suscripción a Voz, elija [**Crear una suscripción**](https://go.microsoft.com/fwlink/?linkid=2086754).

1. En la página [Custom Keyword](https://aka.ms/sdsdk-wakewordportal) (Palabra clave personalizada), cree un **Nuevo proyecto**. 

1. Escriba un **Nombre**, una **Descripción** opcional y seleccione el idioma. Necesitará un proyecto por idioma, y la compatibilidad está limitada actualmente al idioma en-US.

    ![Descripción del proyecto de palabra clave](media/custom-keyword/custom-kws-portal-new-project.png)

1. Seleccione el proyecto de la lista. 

    ![Selección del proyecto de palabra clave](media/custom-keyword/custom-kws-portal-project-list.png)

1. Para iniciar un nuevo modelo de palabra clave, haga clic en **Entrenar modelo**.

1. Escriba un **Nombre** para el modelo de palabra clave y una **Descripción** opcional, y escriba la **Palabra clave** de su elección y haga clic en **Siguiente**. Existen algunas [directrices](speech-devices-sdk-kws-guidelines.md#choose-an-effective-keyword) para ayudarlo a elegir una palabra clave efectiva.

    ![Escriba su palabra clave](media/custom-keyword/custom-kws-portal-new-model.png)

1. Ahora, el portal creará pronunciaciones candidatas para la palabra clave. Para escuchar a cada candidato, haga clic en los botones de reproducción y desactive las pronunciaciones que sean incorrectas. Una vez que solo las pronunciaciones correctas estén marcadas como activas, haga clic en **Entrenar** para empezar a generar la palabra clave. 

    ![Revisión de la palabra clave](media/custom-keyword/custom-kws-portal-choose-prons.png)

1. El modelo puede tardar hasta treinta minutos en generarse. La lista de palabras clave cambiará de **Procesando** a **Correcto** cuando el modelo esté completo. Luego puede descargar el archivo.

    ![Revisión de la palabra clave](media/custom-keyword/custom-kws-portal-download-model.png)

1. Guarde el archivo .zip en el equipo. Necesitará este archivo para implementar la palabra clave personalizada en el dispositivo.

## <a name="next-steps"></a>Pasos siguientes

Pruebe la palabra clave personalizada con el [inicio rápido del SDK de dispositivos de Voz](https://aka.ms/sdsdk-quickstart).
