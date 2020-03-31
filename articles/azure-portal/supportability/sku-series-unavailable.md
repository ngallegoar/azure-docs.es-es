---
title: 'Serie de SKU o región no disponibles: Azure'
description: Algunas series de SKU no están disponibles para la suscripción seleccionada de esta región, lo que puede requerir una solicitud de soporte técnico de administración de suscripciones.
services: Azure Supportability
author: stevendotwang
ms.service: azure-supportability
ms.topic: article
ms.date: 01/27/2020
ms.author: xingwan
ms.openlocfilehash: b0f0762ded6804c0b0d90a19223c082f0fb8fd49
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76843653"
---
# <a name="region-or-sku-unavailable"></a>Región o SKU no disponible

En este artículo se describe cómo resolver el problema de que una suscripción a Azure no tenga acceso a una región o a la SKU de una máquina virtual.

## <a name="symptoms"></a>Síntomas

Al implementar una máquina virtual, recibirá uno de los siguientes mensajes de error:

```
Code: SkuNotAvailable
Message: The requested size for resource '<resource>' is currently not available in location 
'<location>' zones '<zone>' for subscription '<subscriptionID>'. Please try another size or 
deploy to a different location or zones. See https://aka.ms/azureskunotavailable for details.
```

```
Message: Your subscription doesn’t support virtual machine creation in <location>. Choose a 
different location. Supported locations are <list of locations>
```

```
Code: NotAvailableForSubscription
Message: This size is currently unavailable in this location for this subscription
```

Al comprar instancias reservadas de máquina virtual, recibirá uno de los siguientes mensajes de error:

```
Message: Your subscription doesn’t support virtual machine reservation in <location>. Choose a 
different location. Supported locations are: <list of locations>  
```

```
Message: This size is currently unavailable in this location for this subscription
```

Al crear una solicitud de soporte técnico para aumentar la cuota de núcleos de proceso, no se podrán seleccionar regiones ni familias de SKU.

## <a name="solution"></a>Solución

En primer lugar, le recomendamos que considere usar una región o SKU alternativa que se adapte sus necesidades empresariales.

Si no encuentra una región o SKU que sea adecuada, cree una [solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) del tipo **Administración de la suscripción** siguiendo estos pasos:

1. En el menú de [Azure Portal](https://portal.azure.com), seleccione **Ayuda y soporte técnico**. Seleccione **Nueva solicitud de soporte técnico**.

1. En **Datos básicos**, seleccione **Administración de suscripciones** para el **Tipo de problema**.

1. Seleccione una **Suscripción** y escriba una breve descripción en **Resumen**.

   ![Pestaña de Datos básicos de una Nueva solicitud de soporte técnico](./media/SKU-series-unavailable/support-request-basics.png)

1. En **Tipo de problema**, seleccione **Elija un tipo de problema**.

1. En **Elija un tipo de problema**, elija una opción; por ejemplo, **Unable to access my subscription or resource (No puedo acceder a mi suscripción o recurso)**  > **My issue is not listed above (Mi problema no aparece en la lista anterior)** . Seleccione **Guardar**.

   ![Especificar un problema para la solicitud](./media/SKU-series-unavailable/support-request-select-problem-type.png)

1. Seleccione **Siguiente: Soluciones** para explorar las posibles soluciones. Si es necesario, seleccione **Siguiente: Detalles** para continuar.

1. Escriba toda la información adicional que pueda proporcionar, junto con su información de contacto.

1. Seleccione **Revisar + crear**. Después de comprobar la información, seleccione **Crear** para crear la solicitud.

## <a name="send-us-your-suggestions"></a>Envíenos sus sugerencias

Siempre estamos abiertos a todo tipo de comentarios y sugerencias. No dude en enviarnos sus [sugerencias](https://feedback.azure.com/forums/266794-support-feedback). Además, puede ponerse en contacto con nosotros en [Twitter](https://twitter.com/azuresupport) o los [foros de MSDN](https://social.msdn.microsoft.com/Forums/azure).

## <a name="learn-more"></a>Más información

[Preguntas más frecuentes de soporte técnico de Azure](https://azure.microsoft.com/support/faq)
