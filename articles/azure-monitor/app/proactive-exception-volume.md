---
title: 'Aumento anómalo del volumen de excepciones: Azure Application Insights'
description: Supervise las excepciones de las aplicaciones con la detección inteligente en Azure Application Insights para detectar patrones poco habituales del volumen de excepciones.
ms.topic: conceptual
ms.date: 12/08/2017
ms.openlocfilehash: 00b7a28a51f91c969b41d2ab85b611f6dde51396
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86499433"
---
# <a name="abnormal-rise-in-exception-volume-preview"></a>Aumento anormal del volumen de excepciones (versión preliminar)

Application Insights analiza automáticamente las excepciones generadas en la aplicación y puede avisarle sobre los patrones inusuales en la telemetría de excepciones.

Esta característica no requiere ninguna configuración especial, excepto la [configuración de los informes de excepciones](./asp-net-exceptions.md#set-up-exception-reporting) de la aplicación. Se activará cuando la aplicación genere suficiente telemetría de excepciones.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>¿Cuándo recibiré este tipo de notificación de detección inteligente?
Es posible que obtenga este tipo de notificación si la aplicación presenta un aumento anormal en el número de excepciones de un tipo específico durante un día, en comparación con una base de referencia calculada durante los últimos siete días.
Se usan algoritmos de aprendizaje automático para detectar el aumento en el número de excepciones, teniendo en cuenta un crecimiento natural en el uso de la aplicación.

## <a name="does-my-app-definitely-have-a-problem"></a>Entonces, ¿mi aplicación tiene un problema?
No, una notificación no significa que la aplicación tenga un problema. Aunque un número excesivo de excepciones suele indicar un problema de la aplicación, es posible que estas sean benignas y que la aplicación las controle correctamente.

## <a name="how-do-i-fix-it"></a>¿Cómo puedo corregirlo?
Las notificaciones incluyen información de diagnóstico para facilitar el proceso de diagnóstico:
1. **Evaluación de prioridades**. La notificación muestra el número de usuarios o solicitudes afectados. Esto puede ayudarlo a asignar una prioridad al problema.
2. **Ámbito.** ¿El problema afecta a todo el tráfico o solo a alguna operación? Esta información puede obtenerse de la notificación.
3. **Diagnóstico.** La detección contiene información sobre el método desde el que se generó la excepción, así como del tipo de excepción. También puede usar los elementos relacionados y los vínculos de informes para obtener información que puede ayudarle a efectuar un diagnóstico más exhaustivo del problema.
