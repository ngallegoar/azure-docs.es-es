---
title: Proceso de limitación de Cognitive Services
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo solicitar acceso anticipado a nuevos contenedores y API de Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 04/24/2020
ms.author: aahi
ms.openlocfilehash: a161cc0675eedee50608b8d6c288c57147fd6d23
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83599348"
---
# <a name="gating-process-for-azure-cognitive-services"></a>Proceso de limitación para Azure Cognitive Services

> [!NOTE]
> Una vez que una oferta de servicio completa una versión preliminar limitada, pasa a encontrarse en versión preliminar pública "sin limitaciones" y no requiere ninguna aplicación para el acceso. Después del proceso de versión preliminar, se publica para la disponibilidad general (GA).

A medida que se incorporan nuevas ofertas de Azure Cognitive Services, estas pasan a encontrarse en versión preliminar limitada y los clientes deben solicitar acceso a ellas a través de una aplicación. Este proceso de limitación ayuda a identificar oportunidades de mejoras en las ofertas de servicio antes de que estén disponibles de forma general. 

En este artículo le guiaremos por el proceso de las aplicaciones para obtener las ofertas limitadas de Cognitive Services.

## <a name="eligibility-and-approval-process"></a>Proceso de aprobación e idoneidad

El proceso de limitación se usa para medir el interés y comprender mejor las necesidades de los clientes. El equipo de Microsoft acepta [aplicaciones](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyQZ7B8Cg2FEjpibPziwPcZUNlQ4SEVORFVLTjlBSzNLRlo0UzRRVVNPVy4u) de clientes comerciales de Microsoft con una suscripción válida a Azure y un escenario empresarial válido. Se denegarán las aplicaciones de los clientes en los casos siguientes:

 - Si no están asociadas a ninguna organización.
 - Si no disponen de ninguna suscripción válida a Azure.
 - Si la aplicación se envió a través del correo electrónico personal (@hotmail.com, @gmail.com, @yahoo.com).
 - Si no se proporcionó ninguna justificación ni ningún escenario empresarial adecuado.

Dada la demanda de los distintos segmentos de clientes, estamos intentando acelerar el proceso de aprobación. Sin embargo, no podemos comprometernos con una escala de tiempo. Una vez que se tome la decisión, el equipo de Microsoft se pondrá en contacto con usted y su equipo de administración de cuentas para informarle sobre los pasos siguientes. Agradecemos su comprensión y paciencia.

Si la aplicación se aprueba, el equipo de Microsoft enviará un correo electrónico con los detalles, la documentación y la guía. Los detalles de precios de Cognitive Services están disponibles [aquí](https://azure.microsoft.com/pricing/details/cognitive-services/).


Actualmente, se ofrecen los servicios siguientes mediante el proceso de limitación:

## <a name="gated-web-apis"></a>API web limitada

|Servicio  |
|---------|
|Anomaly Detector v2     | 

## <a name="gated-online-containers"></a>Contenedores en línea limitados

| Servicio                             | Contenedores                                                                  |
|-------------------------------------|-------------------------------------------------------------------------------|
| [Computer Vision][cv-containers]    | Lectura                                                                          |
| [Face][fa-containers]               | Caras                                                                          |
| [Form Recognizer][fr-containers]    | Form Recognizer                                                               |
| [Speech Service API][sp-containers] | Speech to Text (personalizado y estándar) y Text to Speech (personalizado y estándar) |
| [Translator Text][tt-containers]    | Translator Text                                                               |

> [!IMPORTANT]
> Si un servicio o una oferta en contenedor no aparecen en la lista, significa que no están limitados o no están disponibles.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Suscribirse a los servicios limitados](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyQZ7B8Cg2FEjpibPziwPcZUNlQ4SEVORFVLTjlBSzNLRlo0UzRRVVNPVy4u)

[ad-containers]: ./anomaly-detector/anomaly-detector-container-howto.md
[cv-containers]: ./computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: ./face/face-how-to-install-containers.md
[fr-containers]: ./form-recognizer/form-recognizer-container-howto.md
[lu-containers]: ./luis/luis-container-howto.md
[sp-containers]: ./speech-service/speech-container-howto.md
[ta-containers]: ./text-analytics/how-tos/text-analytics-how-to-install-containers.md
[tt-containers]: ./translator/how-to-install-containers.md
