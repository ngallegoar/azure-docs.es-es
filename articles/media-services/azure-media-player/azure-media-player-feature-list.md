---
title: Lista de características de Azure Media Player
description: Un documento de referencia sobre las características de Azure Media Player.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: reference
ms.date: 04/20/2020
ms.openlocfilehash: 88048c3328114f17b30859efb41bb9f059b71439
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91296372"
---
# <a name="feature-list"></a>Lista de características #
Esta es una lista de las características probadas y de las no admitidas:

| Característica | PROBADAS | PROBADAS PARCIALMENTE | NO PROBADAS | NO ADMITIDAS | NOTAS |
| ------- | ------ | ---------------- | -------- | ----------- | ----- |
| **Reproducción**                                |        |                  |          |             |                                                                                                                      |
| Reproducción básica a petición                | X      |                  |          |             | Solo se admiten frecuencias de Azure Media Services                                                                      |
| Reproducción básica en directo                     | X      |                  |          |             | Solo se admiten frecuencias de Azure Media Services                                                                      |
| AES                                     | X      |                  |          |             | Admite el servicio de entrega de claves de Azure Media Services                                                                   |
| DRM múltiple                               |        | X                |          |             |                                                                                                                      |
| PlayReady                               | X      |                  |          |             | Admite el servicio de entrega de claves de Azure Media Services                                                                   |
| Widevine                                |        | X                |          |             | Admite los cuadros PSSH de Widevine que se describen en el manifiesto                                                                    |
| FairPlay                                |        | X                |          |             | Admite el servicio de entrega de claves de Azure Media Services                                                                   |
| **Tecnologías**                                   |        |                  |          |             |                                                                                                                      |
| MSE/EME (AzureHtml5JS)                  | X      |                  |          |             |                                                                                                                      |
| Reserva de Flash (FlashSS)                | X      |                  |          |             | No todas las características están disponibles en esta tecnología.                                                                         |
| SilverlightSS de reserva de Silverlight      | X      |                  |          |             | No todas las características están disponibles en esta tecnología.                                                                         |
| Paso a través de HLS nativo (Html5)         |        | X                |          |             | No todas las características están disponibles en esta tecnología debido a las restricciones de la plataforma.                                            |
| **Características**                                |        |                  |          |             |                                                                                                                      |
| Compatibilidad de la API                             | X      |                  |          |             | Consulte la lista de problemas conocidos                                                                                                |
| Interfaz de usuario básica                                | X      |                  |          |                                                                                                                                    |
| Inicialización mediante JavaScript       | X      |                  |          |             |                                                                                                                      |
| Inicialización mediante la etiqueta de vídeo        |        | X                |          |             |                                                                                                                      |
| Direccionamiento de segmentos: basado en tiempo         | X      |                  |          |             |                                                                                                                      |
| Direccionamiento de segmentos: basado en índice        |        |                  |          | X           |                                                                                                                      |
| Direccionamiento de segmentos: basado en bytes         |        |                  |          | X           |                                                                                                                      |
| Reescritura de direcciones URL de Azure Media Services       |        | X                |          |             |                                                                                                                      |
| Accesibilidad: títulos y subtítulos  | X      |                 |          |             |  WebVTT (a petición), CEA 708 (a petición y en directo) and IMSC1 (a petición y en directo)                                                       |
| Accesibilidad: teclas de acceso rápido                 | X      |                  |          |             |                                                                                                                      |
| Accesibilidad: contraste alto           |        | X                |          |             |                                                                                                                      |
| Accesibilidad: enfoque con tabulación               |        | X                |          |             |                                                                                                                      |
| Mensajería de errores                         |        | X                |          |             | Los mensajes de error son incoherentes entre las tecnologías                                                                         |
| Desencadenamiento de eventos                        | X      |                  |          |             |                                                                                                                      |
| Diagnóstico                             |        | X                |          |             | La información de diagnóstico solo está disponible en AzureHtml5JS y está disponible parcialmente en Silverlightss. |
| Pedido de tecnología personalizable                 |        | X                |          |             |                                                                                                                      |
| Heurística: básico                      | X      |                  |          |             |                                                                                                                      |
| Heurística: personalización              |        |                  | X        |             | La personalización solo está disponible con AzureHtml5JS.                                                          |
| Discontinuidades                         | X      |                  |          |             |                                                                                                                      |
| Seleccionar velocidad de bits                          | X      |                  |          |             | Esta API solo está disponible en AzureHtml5JS y FlashSS.                                                    |
| Secuencias de audio múltiples                      |        | X                |          |             | El conmutador de audio mediante programación es compatible con AzureHtml5JS y FlashSS, y está disponible mediante selección en la interfaz de usuario de AzureHtml5JS, FlashSS y HTML5 nativo (en Safari).  La mayoría de las plataformas requieren los mismos datos privados de códec para cambiar las secuencias de audio (el mismo códec, canal, velocidad de muestreo, etc.). |
| Localización de interfaz de usuario                         |        | X                |          |             |                                                                                                                      |
| Reproducción de instancias múltiples                 |        |                  |          | X           | Este escenario puede funcionar con algunas tecnologías, pero actualmente no se admite y no se realizan pruebas. También puede obtenerlo para trabajar con iframes |
| Compatibilidad con anuncios                             |        | X                |          |             | Azure Media Player admite la inserción de anuncios al principio, a la mitad y al final de la reproducción a partir de servidores de anuncios compatibles con VAS para VoD en AzureHtml5JS. |
| Análisis                               |        | X                |          |             | Azure Media Player proporciona la posibilidad de escuchar análisis y eventos de diagnóstico para enviarlos al back-end de análisis de su elección.  No todos los eventos y propiedades están disponibles en las diferentes tecnologías debido a las limitaciones de la plataforma.                                                                            |
| Máscaras personalizadas                            |        |                  | X        |             | Para lograr este escenario, establezca los controles de configuración en false en AMP y use su propio HTML y SS.           |
| Arrastrar el cabezal de reproducción de la barra de búsqueda                      |        |                  |          | X           |                                                                                                                      |
| Jugada con truco                              |        |                  |          | X           |                                                                                                                      |
| Solo audio                              | X      |                  |          |           | Se admite en AzureHtml5JS. La reproducción progresiva de MP3 puede funcionar con HTML5 si la plataforma lo admite.                                                                                                        |
| Solo vídeo                              | X      |                  |          |           | Se admite en AzureHtml5JS.                                                                                                        |
| Presentación de varios períodos               |        |                  |          | X                                                                                                                                  |
| Varios ángulos de cámara                  |        |                  |          | X           |                                                                                                                      |
| Velocidad de reproducción                          |        | X                |          |             | La velocidad de reproducción se admite en la mayoría de los escenarios, excepto en el caso de los dispositivos móviles debido a un error parcial en Chrome.                 |

## <a name="next-steps"></a>Pasos siguientes ##
- [Inicio rápido de Azure Media Player](azure-media-player-quickstart.md)