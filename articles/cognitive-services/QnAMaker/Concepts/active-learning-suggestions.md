---
title: 'Sugerencias de aprendizaje activo: QnA Maker'
description: Las sugerencias de aprendizaje activo le permiten mejorar la calidad de la base de conocimiento al sugerir preguntas alternativas, en función de los envíos del usuario, para el par de preguntas y respuestas.
ms.topic: conceptual
ms.date: 03/19/2020
ms.openlocfilehash: af4f6b399bfd537b38ea741d03e59371ee81e588
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80053153"
---
# <a name="active-learning-suggestions"></a>Sugerencias de aprendizaje activo

La característica _Sugerencias de aprendizaje activo_ le permiten mejorar la calidad de la base de conocimiento al sugerir preguntas alternativas, en función de los envíos del usuario, para el par de preguntas y respuestas. Usted revisa las sugerencias, ya sea al agregarlas a preguntas existentes o rechazarlas.

La base de conocimiento no cambia automáticamente. Debe aceptar las sugerencias para que cualquier cambio surta efecto. Estas sugerencias agregan preguntas, pero no cambian ni quitan preguntas existentes.

## <a name="what-is-active-learning"></a>¿Qué es el aprendizaje activo?

QnA Maker aprende las nuevas variaciones de las preguntas con comentarios implícitos y explícitos.

* [Comentarios implícitos:](#how-qna-makers-implicit-feedback-works) el clasificador entiende cuando la pregunta de un usuario tiene varias respuestas con puntuaciones muy similares y lo considera un comentario. No es necesario hacer nada para que esto ocurra.
* [Comentarios explícitos:](#how-you-give-explicit-feedback-with-the-train-api) cuando se devuelven desde la base de conocimiento varias respuestas con poca variación en las puntuaciones, la aplicación cliente le pregunta al usuario cuál es la pregunta correcta. Los comentarios explícitos del usuario se envían a QnA Maker con la [API Train](../How-to/improve-knowledge-base.md#train-api).

Ambos métodos proporcionan al clasificador consultas similares que se agrupan en clústeres.

## <a name="how-active-learning-works"></a>Cómo funciona el aprendizaje activo

El aprendizaje activo se desencadena en función de las puntuaciones de las mejores respuestas devueltas por QnA Maker. Si las diferencias de puntuación entre los conjuntos de QnA que coinciden con la consulta se encuentran dentro de un intervalo pequeño, la consulta se considera una posible sugerencia (como una pregunta alternativa) para cada uno de los pares de QnA posibles. Una vez que acepta la pregunta sugerida para un par específico de QnA, se rechaza para los otros pares. Debe recordar, guardar y entrenar, después de aceptar las sugerencias.

El aprendizaje activo ofrece las mejores sugerencias posibles en los casos donde los puntos de conexión reciben una cantidad razonable y una variedad de consultas de uso. Cuando 5 o más consultas similares se agrupan en clústeres, cada 30 minutos, QnA Maker sugiere las preguntas basadas en el usuario al diseñador de la base de conocimiento para que las acepte o rechace. Todas las sugerencias se agrupan en clústeres por similitud, y las sugerencias principales para preguntas alternativas se muestran según la frecuencia de las consultas en particular por los usuarios finales.

Una vez que se sugieren preguntas en el portal de QnA Maker, deberá revisar y aceptar o rechazar las sugerencias. No hay ninguna API para administrar las sugerencias.

## <a name="turn-on-active-learning"></a>Activación del aprendizaje activo

El aprendizaje activo está **desactivado** de forma predeterminada.
Para usar el aprendizaje activo:
* Debe [activarlo](../How-To/use-active-learning.md#turn-on-active-learning-for-alternate-questions) para que QnA Maker recopile preguntas alternativas para la base de conocimiento.
* Para ver las preguntas alternativas sugeridas, [use las opciones de vista](../How-To/improve-knowledge-base.md#view-suggested-questions) en la página de edición.

## <a name="how-qna-makers-implicit-feedback-works"></a>Cómo funcionan los comentarios implícitos de QnA Maker

Los comentarios implícitos de QnA Maker usan un algoritmo para determinar la proximidad de la puntuación y, después, hacen sugerencias de aprendizaje activo. El algoritmo para determinar la proximidad no es un cálculo sencillo. Los intervalos en el ejemplo siguiente no están diseñados para ser fijos, pero deben usarse como guía para comprender el impacto del algoritmo únicamente.

Cuando la puntuación de una pregunta tiene una confianza alta, por ejemplo, un 80 %, el intervalo de puntuaciones que se considera para el aprendizaje activo es amplio, dentro del 10 % aproximadamente. A medida que la puntuación de confianza se reduce, por ejemplo, un 40 %, el intervalo de puntuaciones también disminuye aproximadamente dentro del 4 %.

En la respuesta JSON siguiente de una consulta a generateAnswer de QnA Maker, las puntuaciones de A, B y C son cercanas y se podrían considerar como sugerencias.

```json
{
  "activeLearningEnabled": true,
  "answers": [
    {
      "questions": [
        "Q1"
      ],
      "answer": "A1",
      "score": 80,
      "id": 15,
      "source": "Editorial",
      "metadata": [
        {
          "name": "topic",
          "value": "value"
        }
      ]
    },
    {
      "questions": [
        "Q2"
      ],
      "answer": "A2",
      "score": 78,
      "id": 16,
      "source": "Editorial",
      "metadata": [
        {
          "name": "topic",
          "value": "value"
        }
      ]
    },
    {
      "questions": [
        "Q3"
      ],
      "answer": "A3",
      "score": 75,
      "id": 17,
      "source": "Editorial",
      "metadata": [
        {
          "name": "topic",
          "value": "value"
        }
      ]
    },
    {
      "questions": [
        "Q4"
      ],
      "answer": "A4",
      "score": 50,
      "id": 18,
      "source": "Editorial",
      "metadata": [
        {
          "name": "topic",
          "value": "value"
        }
      ]
    }
  ]
}
```

QnA Maker no sabrá qué respuesta es la mejor respuesta. Use la lista de sugerencias del portal de QnA Maker para seleccionar la mejor respuesta y entrenar nuevamente.


## <a name="how-you-give-explicit-feedback-with-the-train-api"></a>Cómo proporcionar comentarios explícitos con Train API

QnA Maker necesita comentarios explícitos sobre cuál de las respuestas era la mejor. La forma de determinar la mejor respuesta depende de usted y puede incluir:

* Comentarios del usuario, seleccione una de las respuestas.
* Lógica de negocios, como determinar un intervalo de puntuación aceptable.
* Una combinación de los comentarios del usuario y la lógica de negocios.

Use [Train API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/train) para enviar la respuesta correcta a QnA Maker, después de que el usuario la seleccione.

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Consulta a una base de conocimiento](query-knowledge-base.md)