---
title: Planeación de la solución de telefonía y SMS de Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Obtenga información sobre cómo planear de forma eficaz el uso de números de teléfono y telefonía.
author: prakulka
manager: nmurav
services: azure-communication-services
ms.author: prakulka
ms.date: 10/05/2020
ms.topic: overview
ms.custom: references_regions
ms.service: azure-communication-services
ms.openlocfilehash: 6a63df282cadf86668e69d2422a6c791e86010b6
ms.sourcegitcommit: d9ba60f15aa6eafc3c5ae8d592bacaf21d97a871
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91767152"
---
# <a name="plan-your-telephony-and-sms-solution"></a>Planeación de la solución de telefonía y SMS

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]


Azure Communication Services le permite usar números de teléfono para realizar llamadas de voz y enviar mensajes SMS con la red telefónica conmutada pública (RTC). En este documento, revisaremos los tipos de número de teléfono, los planes y la disponibilidad de regiones para planear la solución de telefonía y SMS con Communication Services.

[!INCLUDE [Emergency Calling Notice](../../includes/emergency-calling-notice-include.md)]


## <a name="phone-number-types-in-azure-communication-services"></a>Tipos de número de teléfono en Azure Communication Services
 
Communication Services ofrece dos tipos de número de teléfono: **local** y **gratuito**. 

### <a name="local-numbers"></a>Números locales
Los números locales (geográficos) son números de teléfono de 10 dígitos que se componen de los códigos de área locales de Estados Unidos. Por ejemplo, `+1 (206) XXX-XXXX` es un número local con el código de área `206`. Este código de área está asignado a la ciudad de Seattle. Estos números de teléfono suelen ser usados por personas y empresas locales. Azure Communication Services ofrece números locales de Estados Unidos. Estos números se pueden usar para realizar llamadas telefónicas, pero no para enviar mensajes SMS. 

### <a name="toll-free-numbers"></a>Números de teléfono gratuitos
Los números gratuitos son números de teléfono de 10 dígitos con códigos de área distintos, a los que se puede llamar desde cualquier número de teléfono de forma gratuita. Por ejemplo, `+1 (800) XXX-XXXX` es un número gratuito en la región de Estados Unidos. Estos números de teléfono se suelen usar con fines de servicio al cliente. Azure Communication Services ofrece números de teléfono gratuitos de Estados Unidos. Estos números se pueden usar para realizar llamadas telefónicas y para enviar mensajes SMS. Los números de teléfono gratuitos no se pueden usar para personas y solo se pueden asignar a aplicaciones.

#### <a name="choosing-a-phone-number-type"></a>Elección de un tipo de número de teléfono

Si la aplicación va a usar el número de teléfono (por ejemplo, para realizar llamadas o enviar mensajes en nombre del servicio), puede seleccionar un número de teléfono gratuito o local (geográfico). Puede seleccionar un número de teléfono gratuito si la aplicación envía mensajes SMS o realiza llamadas.

Si el número de teléfono lo utiliza una persona (por ejemplo, un usuario de la aplicación que realiza las llamadas), debe usar un número de teléfono local (geográfico). 

En la tabla siguiente se resumen estos tipos de números de teléfono: 

| Tipo de número de teléfono | Ejemplo                              | Disponibilidad de país    | Funcionalidad del número de teléfono |Caso de uso común                                                                                                     |
| ----------------- | ------------------------------------ | ----------------------- | ------------------------|------------------------------------------------------------------------------------------------------------------- |
| Local (geográfico)        | +1 (código de área local) XXX XX XX  | US                      | Llamadas (salientes) | Asignación de números de teléfono a los usuarios de las aplicaciones  |
| Gratuito         | \+ 1 (*código* de área gratuito) XXX XX XX | US                      | Llamadas (salientes), SMS (entrantes y salientes)| Asignación de números de teléfono a sistemas o bots de respuesta interactiva de voz (IVR) y aplicaciones de SMS                                        |


## <a name="phone-number-plans-in-azure-communication-services"></a>Planes de número de teléfono en Azure Communication Services 

Para la mayoría de los números de teléfono, puede configurar un conjunto de planes "a la carta". Algunos desarrolladores solo necesitan un plan de llamadas salientes, mientras otros optan por los planes de llamadas salientes y SMS salientes. Estos planes se pueden seleccionar a medida que alquile los números de teléfono en Azure Communication Services.

Los planes que están disponibles dependen del país en el que opera, el caso de uso y el tipo de número de teléfono que haya seleccionado. Estos planes varían según el país debido a los requisitos normativos. Azure Communication Services ofrece los siguientes planes:

- **SMS de salida unidireccional**: este plan le permite enviar mensajes SMS a los usuarios. Este plan es útil para escenarios como notificaciones y alertas de autenticación en dos fases. 
- **SMS de entrada y salida bidireccional**: este plan le permite enviar y recibir mensajes de los usuarios mediante números de teléfono. Este plan es útil en escenarios de servicio al cliente.
- **Llamada telefónica de salida unidireccional**: este plan permite realizar llamadas a los usuarios y configurar el identificador del autor de la llamada para las llamadas salientes realizadas por el servicio. Este plan es útil en escenarios de servicio al cliente y notificación mediante voz.

## <a name="countryregion-availability"></a>Disponibilidad de país/región

En la tabla siguiente se muestra dónde puede adquirir diferentes tipos de números de teléfono junto con las funcionalidades de llamadas y SMS entrantes y salientes asociadas a estos tipos de números de teléfono.

|Tipo de número de teléfono| Adquirir números en | Realizar llamadas a                                        | Recibir llamadas de                                    |Enviar mensajes a       | Recibir mensajes de |
|-----------| ------------------ | ---------------------------------------------------  |-------------------------------------------------------|-----------------------|--------|
| Local (geográfico)  | US                 | Estados Unidos, Canadá, Reino Unido, Alemania, Francia. +más*| Estados Unidos, Canadá, Reino Unido, Alemania, Francia. +más* |No disponible| No disponible |
| Gratuito | US                 | US                                                   | US                                                    |US                | US |

*Para más información sobre los destinos de llamada y los precios, consulte la [página de precios](../pricing.md).

## <a name="azure-subscriptions-eligibility"></a>Elegibilidad de las suscripciones de Azure

Para adquirir un número de teléfono, debe disponer de una suscripción de Azure de pago. No se pueden adquirir números de teléfono en cuentas de evaluación gratuita. 

## <a name="next-steps"></a>Pasos siguientes

### <a name="quickstarts"></a>Guías de inicio rápido

- [Obtención de un número de teléfono](../../quickstarts/telephony-sms/get-phone-number.md)
- [Realización de una llamada](../../quickstarts/voice-video-calling/calling-client-samples.md)
- [Enviar un mensaje de texto.](../../quickstarts/telephony-sms/send.md)

### <a name="conceptual-documentation"></a>Documentación conceptual

- [Conceptos de voz y vídeo](../voice-video-calling/about-call-types.md)
- [Flujos de llamadas](../call-flows.md)
- [Precios](../pricing.md)
