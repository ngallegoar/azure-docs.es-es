---
title: 'Implementación de un modelo de Habla personalizada: Servicio de voz'
titleSuffix: Azure Cognitive Services
description: En este documento, aprenderá a crear e implementar un punto de conexión mediante el portal de Habla personalizada.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: erhopf
ms.openlocfilehash: e41653e4644b09cc357510223e5ee86c3806ae2c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "74806120"
---
# <a name="deploy-a-custom-model"></a>Implementación de un modelo personalizado

Después de haber cargado e inspeccionar los datos y de haber evaluado y entrenado un modelo personalizado, puede implementar un punto de conexión personalizado para usar con sus productos, herramientas y aplicaciones. En este documento, aprenderá a crear e implementar un punto de conexión mediante el [portal de Habla personalizada](https://speech.microsoft.com/customspeech).

## <a name="create-a-custom-endpoint"></a>Creación de un punto de conexión personalizado

Para crear un punto de conexión personalizado, inicie sesión en el [portal de Habla personalizada](https://speech.microsoft.com/customspeech) y seleccione **Implementaciones** en el menú Habla personalizada de la parte superior de la página. Si se trata de la primera ejecución, observará que no aparece ningún punto de conexión en la tabla. Después de crear un punto de conexión, usará esta página para realizar el seguimiento de cada punto de conexión implementado.

A continuación, seleccione **Agregar punto de conexión** y escriba un **nombre** y una **descripción** para el punto de conexión personalizado. Luego, seleccione el modelo personalizado que le gustaría asociar a este punto de conexión. Desde esta página, también puede habilitar el registro. El registro le permite supervisar el tráfico del punto de conexión. Si está deshabilitado, no se almacenará el tráfico.

![Implementación de un modelo](./media/custom-speech/custom-speech-deploy-model.png)

> [!NOTE]
> No olvide aceptar los términos de uso y los detalles de precios.

A continuación, seleccione **Crear**. Esta acción le devolverá a la página **Implementación**. Ahora, la tabla incluye una entrada que se corresponde con el punto de conexión personalizado. El estado del punto de conexión muestra su estado actual. Se puede tardar hasta 30 minutos en crear una instancia de un nuevo punto de conexión con los modelos personalizados. Cuando el estado de la implementación cambie a **Completado**, el punto de conexión estará listo para su uso.

Una vez implementado el punto de conexión, su nombre aparece como un vínculo. Haga clic en el vínculo para mostrar información específica del punto de conexión, como la clave o la dirección URL, y código de ejemplo.

## <a name="view-logging-data"></a>Visualización de datos de registro

Los datos de registro están disponibles descargar en **Punto de conexión > Detalles**.

## <a name="next-steps"></a>Pasos siguientes

* Use el punto de conexión personalizado con [Speech SDK](speech-sdk.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Preparación y prueba de los datos](how-to-custom-speech-test-data.md)
* [Inspección de los datos](how-to-custom-speech-inspect-data.md)
* [Evaluación de los datos](how-to-custom-speech-evaluate-data.md)
* [Entrenamiento del modelo](how-to-custom-speech-train-model.md)
* [Implementación del modelo](how-to-custom-speech-deploy-model.md)
