---
title: Prueba gratuita del servicio Voz
titleSuffix: Azure Cognitive Services
description: Comenzar con el servicio Voz es fácil y asequible. Hay dos opciones disponibles gratis para que pueda descubrir lo que puede hacer el servicio y decidir si es adecuado para sus necesidades.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/26/2020
ms.author: erhopf
ms.custom: seodec18, seo-javascript-october2019
ms.openlocfilehash: f74e3ea3d20ad2666b434e009cf62add6f88d200
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218686"
---
# <a name="try-the-speech-service-for-free"></a>Prueba gratuita del servicio Voz

En este artículo, elige una opción para probar fácilmente el servicio Voz disponible gratis para que pueda descubrir lo que puede hacer el servicio y decidir si es adecuado para sus necesidades. Elija una de las dos opciones siguientes en función de su situación y caso de uso:

- [Opción 1](#no-card): obtener inmediatamente las claves de API de **evaluación gratuita** sin proporcionar la información de la tarjeta de crédito (debe tener una cuenta de Azure existente). La **evaluación gratuita** dura 30 días y los datos se eliminarán al final. Esta opción es la más adecuada para una experimentación rápida del servicio.
- [Opción 2](#new-resource): cree un nuevo recurso Voz en Azure, sin cargo alguno, mediante una **suscripción gratuita** (se requiere la información de la tarjeta de crédito). Una **suscripción gratuita** tiene, principalmente, unos límites de velocidad más estrictos que una suscripción de pago. Esta opción es preferible si desea probar el servicio, pero también si tiene previsto actualizar a una suscripción de pago en el futuro y no desea perder datos.

## <a name="try-the-speech-service-without-credit-card-info"></a><a id="no-card"></a>Prueba del servicio Voz sin la información de la tarjeta de crédito

Complete los pasos siguientes para activar una evaluación gratuita de 30 días y obtener las claves de API. El período de prueba se iniciará inmediatamente una vez que se hayan completado los pasos siguientes.

1. Vaya a [Pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/).
1. Seleccione la pestaña **Speech API**.
1. Elija **Obtener clave de API**.

Se le presentarán opciones de facturación. Elija la opción gratis y, después, lea y apruebe el acuerdo de usuario. Se le presentarán claves que puede usar para probar el servicio Voz gratis durante 30 días.

## <a name="try-the-speech-service-by-creating-an-azure-resource"></a><a id="new-resource"></a>Prueba del servicio Voz mediante la creación de un recurso de Azure

En los pasos siguientes, necesitará un cuenta de Microsoft y una cuenta de Azure. Si no tiene un cuenta de Microsoft, puede registrarse para obtener una gratuita en el [portal de la cuenta de Microsoft](https://account.microsoft.com/account). Seleccione **Iniciar sesión con Microsoft** y, luego, cuando se le pida que inicie sesión, seleccione **Crear una cuenta de Microsoft**. Siga los pasos para crear y comprobar la nueva cuenta Microsoft.

Cuando tenga la cuenta de Microsoft, vaya a la [página de suscripción a Azure](https://azure.microsoft.com/free/ai/), seleccione **Comenzar gratis** y cree una cuenta de Azure con su cuenta de Microsoft.

> [!NOTE]
> El servicio Voz tiene dos niveles de servicio: gratis y suscripción, que tienen diferentes limitaciones y ventajas. Si se registra para obtener una cuenta gratuita de Azure, obtendrá 200 USD en crédito de servicios que puede aplicar a una suscripción del servicio Voz de pago durante 30 días.
>
> Si usa el nivel gratis del servicio Voz de bajo volumen, puede conservar esta suscripción gratuita incluso después de que expire la evaluación gratuita o el crédito del servicio.
>
> Para más información, consulte [Precios de Cognitive Services: servicio de voz](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="create-the-resource"></a>Crear el recurso

Para agregar un recurso de servicio de voz (plan gratuito o de pago) a la cuenta de Azure:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con la cuenta Microsoft.

1. Seleccione **Crear un recurso** en la parte superior izquierda del portal. Si no ve **Crear un recurso**, siempre puede encontrarlo mediante la selección del menú contraído en la parte superior izquierda:

   ![botón de navegación contraído](media/index/collapsed-nav.png)

1. En la ventana **Nuevo**, escriba "speech" en el cuadro de búsqueda y presione ENTRAR.

1. En los resultados de la búsqueda, seleccione **Voz**.

   ![resultados de la búsqueda de voz](media/index/speech-search.png)

1. Seleccione **Crear** y, después:

   - Dé un nombre único al nuevo recurso. El nombre ayuda a distinguir entre varias suscripciones vinculadas al mismo servicio.
   - Elija la suscripción de Azure a la que esté asociado el recurso nuevo para determinar cómo se facturan las tarifas.
   - Elija la [región](regions.md) donde se va a usar el recurso.
   - Elija un plan de tarifa de pago (S0) o gratis (F0). Para completar la información sobre los precios y las cuotas de uso de cada plan, seleccione **Ver todos los detalles de los precios**.
   - Cree un nuevo grupo de recursos para esta suscripción de voz o asígnela a un grupo de recursos existente. Los grupos de recursos ayudan a mantener organizadas las distintas suscripciones de Azure.
   - Seleccione **Crear**. Esto le llevará a la información general de la implementación y mostrará mensajes del progreso de la implementación.

> [!NOTE]
> Puede crear un número ilimitado de suscripciones de plan estándar en una o varias regiones. Pero solo puede crear una suscripción de plan gratuito. Las implementaciones de modelo del plan gratuito que permanezcan inactivas durante siete días se retirarán automáticamente.

La implementación del recurso de voz nuevo puede tardar unos instantes. Una vez completada la implementación, seleccione **Ir al recurso** y, en el panel de navegación izquierdo, seleccione **Claves** para mostrar las claves de suscripción del servicio Voz. Cada suscripción tiene dos claves; puede usar cualquiera de ellas en la aplicación. Para copiar y pegar rápidamente una clave en el editor de código o en otra ubicación, seleccione el botón Copiar que se encuentra junto a cada clave, cambie de ventana para pegar el contenido del portapapeles en la ubicación deseada.

> [!IMPORTANT]
> Estas claves de suscripción se usan para tener acceso a la API de Cognitive Services. No comparta las claves. Almacénelas de forma segura, por ejemplo, con Azure Key Vault. También se recomienda regenerar estas claves periódicamente. Solo se necesita una clave para realizar una llamada API. Al volver a generar la primera clave, puede usar la segunda clave para seguir teniendo acceso al servicio.

## <a name="switch-to-a-new-subscription"></a>Cambiar a una nueva suscripción

Para cambiar de una suscripción a otra, por ejemplo, cuando la evaluación gratuita expire o al publicar la aplicación, sustituya la clave de región y suscripción del código por la clave de región y suscripción del nuevo recurso de Azure.

## <a name="about-regions"></a>Acerca de las regiones

- Si la aplicación usa [Speech SDK](speech-sdk.md), proporcione el código de región, por ejemplo, `westus`, al crear una configuración de voz.
- Si la aplicación usa una de las [API de REST](rest-apis.md) del servicio de voz, la región forma parte del URI del punto de conexión que se emplea al realizar solicitudes.
- Las claves creadas para una región son válidas únicamente en esa región. Si intenta usarlas con otras regiones se producen errores de autenticación.

## <a name="next-steps"></a>Pasos siguientes

Realice alguno de los inicios rápidos de 10 minutos o visite los ejemplos de SDK:

> [!div class="nextstepaction"]
> [Inicio rápido: Reconocimiento de voz en C#](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet)
> [Ejemplos del SDK de Voz](speech-sdk.md#get-the-samples)
