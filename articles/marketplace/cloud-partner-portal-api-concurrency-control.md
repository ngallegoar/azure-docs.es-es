---
title: 'Control de simultaneidad: Azure Marketplace'
description: Estrategias de control de simultaneidad para las API de publicación de Cloud Partner Portal.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/08/2020
ms.openlocfilehash: b66d266500745d08bef98a42e51cc8a7bab63958
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86102742"
---
# <a name="concurrency-control"></a>Control de simultaneidad

> [!NOTE]
> Las API de Cloud Partner Portal se integran con el Centro de partners y seguirán funcionando después de migrar las ofertas al Centro de partners. La integración presenta pequeños cambios. Revise los cambios que se muestran en la [referencia de las API de Cloud Partner Portal](./cloud-partner-portal-api-overview.md) para asegurarse de que el código sigue funcionando después de la migración al Centro de partners.

Cada llamada a la API de publicación de Cloud Partner Portal debe especificar explícitamente la estrategia de control de simultaneidad que se usará. Si no se proporciona el encabezado **If-Match**, se producirá una respuesta de error HTTP 400. Ofrecemos dos estrategias para el control de simultaneidad.

-   **Optimista**: el cliente que realiza que la actualización comprueba si los datos han cambiado desde la última lectura de los datos.
-   **El último gana**: el cliente actualiza directamente los datos, independientemente de si otra aplicación los ha modificado desde la última lectura.

<a name="optimistic-concurrency-workflow"></a>Flujo de trabajo de simultaneidad optimista
-------------------------------

Se recomienda usar la estrategia de simultaneidad optimista, con el siguiente flujo de trabajo, para garantizar que no se realizan modificaciones inesperadas a los recursos.

1.  Recupere una entidad mediante las API. La respuesta incluye un valor de ETag que identifica la versión de la entidad almacenada actualmente (en el momento de la respuesta).
2.  En el momento de la actualización, incluya este valor de ETag en el encabezado de solicitud **If-Match** obligatorio.
3.  La API compara el valor ETag recibido en la solicitud con el valor ETag actual de la entidad en una transacción atómica.
    *   Si los valores de ETag son diferentes, la API devuelve un respuesta HTTP `412 Precondition Failed`. Este error indica que cualquier otro proceso ha actualizado la entidad desde que el cliente la recuperó por última vez, o que el valor de ETag especificado en la solicitud es incorrecto.
    *  Si los valores de ETag son iguales, o el encabezado **If-Match** contiene el carácter comodín asterisco (`*`), la API realiza la operación solicitada. La operación de API también actualiza el valor de ETag almacenado de la entidad.


> [!NOTE]
> Especificar el carácter comodín (*) en el encabezado **If-Match** lleva a que la API use la estrategia de simultaneidad El último gana. En este caso, no se realiza la comparación de ETag y el recurso se actualiza sin ninguna comprobación. 
