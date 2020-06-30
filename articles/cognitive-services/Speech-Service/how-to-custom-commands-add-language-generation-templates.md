---
title: 'Instrucciones: Uso de las plantillas de generación de idioma para respuestas de voz: servicio de voz'
titleSuffix: Azure Cognitive Services
description: En este artículo, aprenderá a usar plantillas de generación de idioma con Comandos personalizados. Las plantillas de generación de idioma le permiten personalizar las respuestas enviadas al cliente e introducen variedad en las respuestas.
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: sausin
ms.openlocfilehash: 0cbc57922b31f1b3879bb2cad8a988a1ba4cc368
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2020
ms.locfileid: "85307336"
---
# <a name="add-language-generation-templates-for-speech-responses"></a>Adición de plantillas de generación de idioma para respuestas de voz

En este artículo, aprenderá a usar plantillas de generación de idioma con los Comandos personalizados. Las plantillas de generación de idioma le permiten personalizar las respuestas enviadas al cliente e introducen variedad en las respuestas. Puede personalizar la generación de idioma de las siguientes formas:

- Uso de plantillas de generación de idioma
- Uso de expresiones adaptadas

## <a name="prerequisites"></a>Requisitos previos

Debe haber completado los pasos descritos en los siguientes artículos:

> [!div class="checklist"]
> * [Cómo: Creación de una aplicación con comandos simples](./how-to-custom-commands-create-application-with-simple-commands.md)
> * [Cómo: Adición de parámetros a comandos](./how-to-custom-commands-add-parameters-to-commands.md)

## <a name="language-generation-templates-overview"></a>Introducción a las plantillas de generación de idioma

Las plantillas de Comandos personalizados están basadas en las [plantillas LG](https://aka.ms/speech/cc-lg-format) de Bot Framework. Dado que los Comandos personalizados crean una nueva plantilla LG cuando es necesario (es decir, para las respuestas de voz de los parámetros o acciones), no es necesario especificar el nombre de la plantilla LG. Por lo tanto, en lugar de definir la plantilla como:

 ```
    # CompletionAction
    - Ok, turning {OnOff} the {SubjectDevice}
    - Done, turning {OnOff} the {SubjectDevice}
    - Proceeding to turn {OnOff} {SubjectDevice}
 ```

Solo debe definir el cuerpo de la plantilla sin el nombre, como se indica a continuación.

> [!div class="mx-imgBorder"]
> ![ejemplo del editor de plantillas](./media/custom-commands/template-editor-example.png)


Este cambio ofrece variedad a las respuestas de voz que se envían al cliente. Por lo tanto, para la misma experiencia, la respuesta de voz correspondiente se elegirá de forma aleatoria de las opciones proporcionadas.

Al aprovechar las plantillas LG, también podrá definir respuestas de voz complejas para los comandos mediante expresiones adaptables. Para obtener más información, puede consultar el [formato de plantillas LG](https://aka.ms/speech/cc-lg-format). De forma predeterminada, los Comandos personalizados admiten todas sus funcionalidades, con las siguientes variaciones mínimas:

* En las plantillas LG, las entidades se representan como ${nombreDeEntidad}. En Comandos personalizados, no se usan entidades, aunque los parámetros se pueden usar como variables con una de estas representaciones: ${nombreDeParámetro} o {nombreDeParámetro}.
* La redacción y expansión de las plantillas no son compatibles con Comandos personalizados. Esto se debe a que nunca se modifica directamente el archivo `.lg`, sino solo las respuestas de las plantillas creadas automáticamente.
* En Comandos personalizados, no se admiten las funciones personalizadas que inserta LG. Todavía se admiten las funciones predefinidas.
* Las opciones (strict, replaceNull y lineBreakStyle) no son compatibles con Comandos personalizados.

## <a name="add-template-responses-to-turnonoff-command"></a>Adición de respuestas de plantilla al comando TurnOnOff

Modifique el comando **TurnOnOff** para agregar un nuevo parámetro con la siguiente configuración:

| Configuración            | Valor sugerido       | 
| ------------------ | --------------------- | 
| Nombre               | `SubjectContext`         | 
| Es global          | Desactivado             | 
| Obligatorio           | Desactivado               | 
| Tipo               | String                |
| Valor predeterminado      | `all` |
| Configuración      | Aceptar valores de entrada predefinidos del catálogo interno | 
| Valores de entrada predefinidos | `room`, `bathroom`, `all`|

### <a name="modify-completion-rule"></a>Modificación de una regla de finalización

Edite la sección **Acciones** de la regla de finalización existente **ConfirmationResponse**. En el elemento emergente **Editar acción**, cambie al **Editor de plantillas** y reemplace el texto por el siguiente ejemplo.

```
- IF: @{SubjectContext == "all" && SubjectDevice == "lights"}
    - Ok, turning all the lights {OnOff}
- ELSEIF: @{SubjectDevice == "lights"}
    - Ok, turning {OnOff} the {SubjectContext} {SubjectDevice}
- ELSE:
    - Ok, turning the {SubjectDevice} {OnOff}
    - Done, turning {OnOff} the {SubjectDevice}
```

**Entrene** y **pruebe** su aplicación como se muestra a continuación. Observe la variación de las respuestas debido tanto al uso de varias alternativas para el valor de plantilla como al uso de expresiones adaptables.

* Entrada: turn on the tv (enciende el televisor)
* Salida: Ok, turning the tv on (de acuerdo, encenderé el televisor)
* Entrada: turn on the tv (enciende el televisor)
* Salida: Done, turned on the tv (listo, encendí el televisor)
* Entrada: turn off the lights (apaga las luces)
* Salida: Ok, turning all the lights off (de acuerdo, apagaré las luces)
* Entrada: turn off room lights (apaga las luces de la habitación)
* Salida: Ok, turning off the room lights (de acuerdo, apagaré las luces de la habitación)

## <a name="use-custom-voice"></a>Uso de la Voz personalizada

Otra manera de personalizar las respuestas de Comandos personalizados consiste en seleccionar una voz de salida personalizada. Siga estos pasos para cambiar la voz predeterminada por una voz personalizada.

1. En la aplicación de Comandos personalizados, seleccione **Configuración** en el panel izquierdo.
1. Seleccione **Voz personalizada** en el panel central.
1. Seleccione la voz pública o personalizada que desee en la tabla.
1. Seleccione **Guardar**.

> [!div class="mx-imgBorder"]
> ![Oraciones de ejemplo con parámetros](media/custom-commands/select-custom-voice.png)

> [!NOTE]
> - Para **Public voices** (Voces públicas), **Neural types** (Tipos neuronales) solo está disponible para regiones específicas. Para comprobar la disponibilidad, consulte las [voces estándar y neuronales por región o punto de conexión](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#standard-and-neural-voices).
> - Para **Custom voices** (Voces personalizadas), se pueden crear desde la página de proyecto Custom Voice (Voz personalizada). Consulte [Introducción a Voz personalizada](./how-to-custom-voice.md).

Ahora la aplicación responderá en la voz seleccionada, en lugar de en la voz predeterminada.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Integre los comandos personalizados mediante el SDK de Voz](./how-to-custom-commands-setup-speech-sdk.md).