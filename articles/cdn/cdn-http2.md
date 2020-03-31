---
title: Compatibilidad con HTTP/2 en Azure CDN | Microsoft Docs
description: Aprenda sobre la compatibilidad con HTTP/2 y la red CDN.
services: cdn
documentationcenter: ''
author: lichard
manager: erikre
editor: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/04/2017
ms.author: ril
ms.openlocfilehash: 703623e3f7f314d87417458f3f9a218dfdf45427
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67849971"
---
# <a name="http2-support-in-azure-cdn"></a>Compatibilidad con HTTP/2 en Azure CDN

HTTP/2 es una revisión principal de HTTP/1.1\. Proporciona un rendimiento web más rápido, tiempo de respuesta reducido y experiencia de usuario mejorada, al tiempo que se mantienen los métodos HTTP conocidos, los códigos de estado y la semántica. Aunque HTTP/2 está diseñado para trabajar con HTTP y HTTPS, muchos exploradores web de cliente solo admiten HTTP/2 sobre TLS.

### <a name="http2-benefits"></a>Ventajas HTTP/2

Las ventajas de HTTP/2 incluyen:

*   **Multiplexación y simultaneidad**

    Con HTTP 1.1, realizar varias solicitudes de recursos requiere varias conexiones TCP y cada conexión tiene asociada una sobrecarga de rendimiento. HTTP/2 permite que se soliciten varios recursos en una única conexión TCP.

*   **Compresión de encabezados**

    Al comprimir los encabezados HTTP de los recursos atendidos, el tiempo en la red se reduce considerablemente.

*   **Dependencias de secuencias**

    Las dependencias de secuencia permiten al cliente indicar al servidor qué recursos tiene prioridad.


## <a name="http2-browser-support"></a>Compatibilidad con exploradores HTTP/2

Todos los exploradores principales han implementado la compatibilidad con HTTP/2 en sus versiones actuales. Los exploradores no compatibles conmutan automáticamente a HTTP/1.1.

|Browser|Versión mínima|
|-------------|------------|
|Microsoft Edge| 12|
|Google Chrome| 43|
|Mozilla Firefox| 38|
|Opera| 32|
|Safari| 9|

## <a name="enabling-http2-support-in-azure-cdn"></a>Habilitación de la compatibilidad con HTTP/2 en Azure CDN

Actualmente, la compatibilidad con HTTP/2 está activa para todos los perfiles de Azure CDN. No es necesaria ninguna otra acción por parte de los clientes.

## <a name="next-steps"></a>Pasos siguientes

Para ver las ventajas de HTTP/2 en acción, consulte [esta demostración de Akamai](https://http2.akamai.com/demo).

Para más información sobre HTTP/2, consulte los siguientes recursos:

*   [Página principal de especificación de HTTP/2](https://http2.github.io/)
*   [Preguntas más frecuentes oficiales de HTTP/2](https://http2.github.io/faq/)
*   [Información de HTTP/2 de Akamai](https://http2.akamai.com/)

Para más información sobre las características disponibles de Azure CDN, consulte la [información general sobre Azure CDN](https://azure.microsoft.com/documentation/articles/cdn-overview/).