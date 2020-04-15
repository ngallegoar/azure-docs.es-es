---
title: ¿Qué es Personalizer?
titleSuffix: Azure Cognitive Services
description: Personalizer es un servicio de API basado en la nube que permite elegir la mejor experiencia, mostrársela a los usuarios y aprender de su comportamiento en tiempo real.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 01/21/2020
ms.author: diberry
ms.openlocfilehash: ac6d72cbb69796851e58098138d51bb0988be4ec
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2020
ms.locfileid: "80520973"
---
# <a name="what-is-personalizer"></a>¿Qué es Personalizer?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure Personalizer es un servicio de API basado en la nube que ayuda a la aplicación cliente a elegir el mejor elemento de _contenido_ que se mostrará a cada usuario. El servicio selecciona el mejor, de entre todos los elementos de contenido, en función de la información en tiempo real colectiva que se proporciona sobre el contenido y el contexto.

Después de presentar el elemento de contenido al usuario, el sistema supervisa el comportamiento del usuario y devuelve una puntuación de recompensa a Personalizer con el fin de mejorar su capacidad para seleccionar el mejor contenido en función de la información contextual que recibe.

El **contenido** puede ser cualquier unidad de información, como texto, imágenes, direcciones URL o correos electrónicos que quiera seleccionar para que se muestren al usuario.

<!--
![What is personalizer animation](./media/what-is-personalizer.gif)
-->

## <a name="how-does-personalizer-select-the-best-content-item"></a>¿Cómo selecciona Personalizer el mejor elemento de contenido?

Personalizer usa el **aprendizaje de refuerzo** para seleccionar el mejor elemento (_acción_) en función del comportamiento colectivo y las puntuaciones de recompensa de todos los usuarios. Las acciones son los elementos de contenido, como artículos de noticias, películas específicas o productos que se eligen.

La llamada **Rank** toma el elemento de acción, junto con las características de la acción, y las características de contexto para seleccionar el elemento de acción superior:

* **Acciones con características**: elementos de contenido con características específicas de cada elemento
* **Características de contexto**: características de los usuarios, su contexto o su entorno cuando se usa la aplicación

La llamada Rank devuelve el identificador del elemento de contenido, __acción__, que se mostrará al usuario, en el campo **Reward Action ID** (Id. de acción de recompensa).
La __acción__ mostrada al usuario se elige con modelos de aprendizaje automático, y se intenta maximizar la cantidad total de recompensas a lo largo del tiempo.

Algunos escenarios de ejemplo son:

|Tipo de contenido|**Acciones (con características)**|**Características de contexto**|Identificador de acción de recompensa que se devuelve<br>(se muestra este contenido)|
|--|--|--|--|
|Lista de noticias|a. `The president...` (nacional, política, [texto])<br>b. `Premier League ...` (global, deportes, [texto, imagen, vídeo])<br> c. `Hurricane in the ...` (regional, información meteorológica, [texto, imagen]|Las noticias del dispositivo se leen desde<br>Mes o temporada<br>|a `The president...`|
|Lista de películas|1. `Star Wars` (1977, [acción, aventura, fantasía], George Lucas)<br>2. `Hoop Dreams` (1994, [documental, deportes], Steve James<br>3. `Casablanca` (1942, [romance, drama, guerra], Michael Curtiz)|La película del dispositivo se visiona desde<br>tamaño de pantalla<br>Tipo de usuario<br>|3. `Casablanca`|
|Lista de productos|i. `Product A` (3 kg, $$$$, entrega en 24 horas)<br>ii. `Product B` (20 kg, $$, envío en 2 semanas con tasas de aduanas)<br>iii. `Product C` (3 kg, $$$, entrega en 48 horas)|El envío del dispositivo se lee desde<br>Nivel de gastos del usuario<br>Mes o temporada|ii. `Product B`|

Personalizer usó el aprendizaje de refuerzo para seleccionar la mejor acción, lo que se conoce como _identificador de acción de recompensa_, según una combinación de:
* Modelo entrenado: información pasada que recibió el servicio Personalizer
* Datos actuales: acciones específicas con características y características de contexto

## <a name="when-to-call-personalizer"></a>Cuándo llamar a Personalizer

Se llama a **API** [Rank](https://go.microsoft.com/fwlink/?linkid=2092082) de Personalizer _cada vez_ que se presenta contenido, en tiempo real. Esto se conoce como un **evento**, anotado con un _identificador de evento_.

Se puede llamar a **API** [Reward](https://westus2.dev.cognitive.microsoft.com/docs/services/personalizer-api/operations/Reward) de Personalizer en tiempo real o con retardo para que se adapte mejor a su infraestructura. La puntuación de recompensa se determinará según sus necesidades empresariales. La puntuación de recompensa oscila entre 0 y 1. Esta puntuación debe ser un solo valor, por ejemplo, 1 para bueno y 0 para malo, o un número generado por un algoritmo que cree teniendo en cuenta las métricas y los objetivos empresariales.

## <a name="personalizer-content-requirements"></a>Requisitos de contenido de Personalizer

Use Personalizer cuando el contenido:

* Tenga un conjunto limitado de elementos (50 como máximo) para seleccionar. Si tiene una lista más grande, [use un motor de recomendaciones](where-can-you-use-personalizer.md#how-to-use-personalizer-with-a-recommendation-solution) para reducir la lista a 50 elementos.
* Contenga información que describa el contenido que quiere clasificar: _acciones con características_ y _características de contexto_.
* Tenga un número mínimo de eventos relacionados con contenido de 1 KB/día para que Personalizer funcione de forma efectiva. Si Personalizer no recibe el tráfico mínimo necesario, el servicio tarda más en determinar el mejor elemento de contenido.

Dado que Personalizer usa información colectiva casi en tiempo real para devolver el mejor elemento de contenido, el servicio no:
* Conserva ni administra la información de perfil de usuario
* Registra las preferencias ni el historial de usuarios individuales
* Necesita contenido limpio ni etiquetado

## <a name="how-to-design-and-implement-personalizer-for-your-client-application"></a>Cómo diseñar e implementar Personalizer para la aplicación cliente

1. [Diseñe](concepts-features.md) y planee contenido, **_acciones_** y **_contexto_** . Determine el algoritmo de recompensa para la puntuación de **_recompensa_** .
1. Cada [recurso de Personalizer](how-to-settings.md) que cree se considera un bucle de aprendizaje. El bucle recibirá las llamadas Rank y Reward para ese contenido o experiencia de usuario.
1. Agregue Personalizer a su sitio web o sistema de contenido:
    1. Agregue una llamada **Rank** a Personalizer en su aplicación, sitio web o sistema para determinar el mejor elemento de _contenido_ antes de que se muestre el contenido al usuario.
    1. Muestre al usuario el mejor elemento de _contenido_, que es el _identificador de acción de recompensa_ devuelto.
    1. Aplique el _algoritmo_ a la información recopilada sobre cómo se comportará el usuario, para determinar la puntuación de **recompensa**, por ejemplo:

        |Comportamiento|Puntuación de recompensa calculada|
        |--|--|
        |El usuario seleccionó el mejor elemento de _contenido_ (identificador de acción de recompensa)|**1**|
        |El usuario seleccionó otro contenido|**0**|
        |El usuario hizo una pausa, se desplazó sin decidirse, antes de seleccionar el mejor elemento de _contenido_ (identificador de acción de recompensa)|**0,5**|

    1. Agregue una llamada **Reward** para enviar una puntuación de recompensa entre 0 y 1
        * Inmediatamente después de mostrar el contenido
        * O, en algún momento posterior en un sistema sin conexión
    1. [Evalúe el bucle](concepts-offline-evaluation.md) con una evaluación sin conexión después de un período de uso. La evaluación sin conexión permite probar y evaluar la eficacia del servicio Personalizer sin cambiar el código o sin que se vea afectada la experiencia del usuario.

## <a name="next-steps"></a>Pasos siguientes


* [Funcionamiento de Personalizer](how-personalizer-works.md)
* [¿Qué es el aprendizaje de refuerzo?](concepts-reinforcement-learning.md)
* [Más información sobre las características y acciones de la solicitud de Rank](concepts-features.md)
* [Más información sobre cómo determinar la puntuación de la solicitud de Reward](concept-rewards.md)
* [Guías de inicio rápido](sdk-learning-loop.md)
* [Tutorial](tutorial-use-azure-notebook-generate-loop-data.md)
* [Uso de la demostración interactiva](https://personalizationdemo.azurewebsites.net/)
