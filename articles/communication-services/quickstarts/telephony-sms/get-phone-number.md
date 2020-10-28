---
title: 'Inicio rápido: obtención de un número de teléfono de Azure Communication Services'
description: Obtenga información sobre cómo comprar un número de teléfono de Communication Services mediante Azure Portal.
author: prakulka
manager: nmurav
services: azure-communication-services
ms.author: prakulka
ms.date: 10/05/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.custom: references_regions
ms.openlocfilehash: cff39f93f9caddfdbe48788f14b62642a373e2bf
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92148186"
---
# <a name="quickstart-get-a-phone-number-using-the-azure-portal"></a>Inicio rápido: Obtención de un número de teléfono mediante Azure Portal

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

Comience a usar Azure Communication Services mediante Azure Portal para comprar un número de teléfono.

## <a name="prerequisites"></a>Requisitos previos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Recurso activo de Communication Services.](../create-communication-resource.md)

## <a name="get-a-phone-number"></a>Obtención de un número de teléfono

Para empezar a aprovisionar números, vaya al recurso de Communication Services en [Azure Portal](https://portal.azure.com).

:::image type="content" source="../media/manage-phone-azure-portal-start.png" alt-text="Captura de pantalla que muestra la página principal de un recurso de Communication Services.":::

### <a name="getting-new-phone-numbers"></a>Obtención de nuevos números de teléfono

Vaya a la hoja **Phone Numbers** (Números de teléfono) en el menú de recursos.

:::image type="content" source="../media/manage-phone-azure-portal-phone-page.png" alt-text="Captura de pantalla que muestra la página principal de un recurso de Communication Services.":::

Presione el botón **Obtener** para iniciar el asistente. El asistente de la hoja **Phone numbers** (Números de teléfono) le guiará por una serie de preguntas que le ayudarán a elegir el número de teléfono que mejor se adapte a su escenario. 

En primer lugar, deberá elegir el valor de **País o región** donde desea aprovisionar el número de teléfono. Después de seleccionar el país o la región, deberá seleccionar el valor de **Use case** (Caso de uso) que mejor se adapte a sus necesidades. 

:::image type="content" source="../media/manage-phone-azure-portal-get-numbers.png" alt-text="Captura de pantalla que muestra la página principal de un recurso de Communication Services.":::

### <a name="select-your-phone-number-features"></a>Selección de las características de los números de teléfono

La configuración del número de teléfono se divide en dos pasos: 

1. Selección del tipo del [tipo de número](../../concepts/telephony-sms/plan-solution.md#phone-number-types-in-azure-communication-services).
2. Selección de las [características del número](../../concepts/telephony-sms/plan-solution.md#phone-number-features-in-azure-communication-services).

Puede seleccionar entre dos tipos de números de teléfono: **Geographic** (Geográfico) y **Número gratuito** . Después de seleccionar un tipo de número, puede elegir las características.

En nuestro ejemplo hemos seleccionado un tipo **Número gratuito** con las características **Llamadas salientes** y **SMS entrantes y salientes** .

:::image type="content" source="../media/manage-phone-azure-portal-select-plans.png" alt-text="Captura de pantalla que muestra la página principal de un recurso de Communication Services.":::

Desde aquí, haga clic en el botón **Next: Numbers** (Siguiente: números) en la parte inferior de la página para personalizar los números de teléfono que le gustaría aprovisionar.

### <a name="customizing-phone-numbers"></a>Personalización de los números de teléfono

En la página **Numbers** (Números), personalizará los números de teléfono que le gustaría aprovisionar.

:::image type="content" source="../media/manage-phone-azure-portal-select-numbers-start.png" alt-text="Captura de pantalla que muestra la página principal de un recurso de Communication Services.":::

> [!NOTE]
> En este inicio rápido se muestra el flujo de personalización del tipo **Número gratuito** . La experiencia puede ser ligeramente diferente si ha elegido el tipo de número **Geographic** (Geográfico), pero el resultado final será el mismo.

Elija el valor de **Código de área** de la lista de códigos de área disponibles y escriba la cantidad que desea aprovisionar y, a continuación, haga clic en **Buscar** para buscar números que cumplan los requisitos seleccionados. Los números de teléfono que satisfagan sus necesidades se mostrarán junto con el costo mensual.

:::image type="content" source="../media/manage-phone-azure-portal-found-numbers.png" alt-text="Captura de pantalla que muestra la página principal de un recurso de Communication Services.":::

> [!NOTE]
> La disponibilidad depende del tipo de número, la ubicación y las características que haya seleccionado.
> Los números se reservan durante un breve período de tiempo antes de que expire la transacción. Si la transacción expira, tendrá que volver a seleccionar los números.

Para ver el resumen de la compra y realizar el pedido, haga clic en el botón **Next: Summary** (Siguiente: resumen) situado en la parte inferior de la página.

### <a name="place-order"></a>Hacer pedido

En la página de resumen se revisará el tipo de número, las características, los números de teléfono y el costo mensual total para aprovisionar los números de teléfono.

> [!NOTE]
> Los precios que se muestran son los **cargos periódicos mensuales** que cubren el costo por conceder el número de teléfono seleccionado. **Los costos de pago por uso** , que se incurren al realizar o recibir llamadas, no se incluyen en esta vista. Los costos de lista están [disponibles aquí](../../concepts/pricing.md). Estos costos dependen del tipo de número y de la ubicación del número a llamar. Por ejemplo, precio por minuto para una llamada de un número regional de Seattle a un número regional de Nueva York y una llamada del mismo número a un número de teléfono móvil del Reino Unido puede ser diferente.

Finalmente, haga clic en **Place order** (Realizar pedido) situado en la parte inferior de la página para confirmar.

:::image type="content" source="../media/manage-phone-azure-portal-get-numbers-summary.png" alt-text="Captura de pantalla que muestra la página principal de un recurso de Communication Services.":::

## <a name="find-your-phone-numbers-on-the-azure-portal"></a>Busque los números de teléfono en Azure Portal

En [Azure Portal](https://portal.azure.com), navegue hasta el recurso de comunicación de Azure:

:::image type="content" source="../media/manage-phone-azure-portal-start.png" alt-text="Captura de pantalla que muestra la página principal de un recurso de Communication Services.":::

Seleccione la hoja Números de teléfono en el menú para administrar los números de teléfono.

:::image type="content" source="../media/manage-phone-azure-portal-phones.png" alt-text="Captura de pantalla que muestra la página principal de un recurso de Communication Services.":::

> [!NOTE]
> Los números aprovisionados pueden tardar unos minutos en mostrarse en esta página.


### <a name="customizing-phone-numbers"></a>Personalización de los números de teléfono

En la página **Números** puede seleccionar un número de teléfono para configurarlo.

:::image type="content" source="../media/manage-phone-azure-portal-capability-update.png" alt-text="Captura de pantalla que muestra la página principal de un recurso de Communication Services.":::

Seleccione las características de las opciones disponibles y haga clic en **Confirmar** para aplicar la selección.

## <a name="troubleshooting"></a>Solución de problemas

Preguntas y problemas comunes:

- En este momento, Estados Unidos es el único país en que se permite comprar números de teléfono. Esto se basa en la dirección de facturación de la suscripción a la que está asociado el recurso. En este momento, los recursos no se pueden cambiar de suscripción.

- Si se libera un número de teléfono, este no se liberará ni se podrá volver a comprar hasta el final del período de facturación.

- Si se elimina un recurso de Communication Services, los números de teléfono asociados a ese recurso se liberarán automáticamente al mismo tiempo.

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha aprendido a hacer lo siguiente:

> [!div class="checklist"]
> * Comprar un número de teléfono
> * Administrar el número de teléfono
> * Liberar un número de teléfono

> [!div class="nextstepaction"]
> [Enviar un SMS](../telephony-sms/send.md)
> [Introducción a las llamadas](../voice-video-calling/getting-started-with-calling.md)
