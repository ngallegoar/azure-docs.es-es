---
title: Adición de detalles técnicos para la oferta de Aplicación de Azure
description: Aprenda a agregar detalles técnicos para la oferta de Aplicación de Azure en el Centro de partners.
author: aarathin
ms.author: aarathin
ms.reviewer: dannyevers
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
ms.date: 11/06/2020
ms.openlocfilehash: a8658242bca48ee490e7fd44582f46e7c9c78e5b
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2020
ms.locfileid: "94369863"
---
# <a name="how-to-add-technical-details-for-your-azure-application-offer"></a>Adición de detalles técnicos para la oferta de Aplicación de Azure

En este artículo se describe cómo especificar detalles técnicos que ayuden al marketplace comercial de Microsoft a conectarse a su solución. Esta conexión nos permite aprovisionar su oferta para el cliente, en caso de que este elija adquirirla y administrarla.

Complete esta sección solo si la oferta incluye una aplicación administrada que emitirá eventos de medición con las [API de facturación de uso medido de Marketplace](partner-center-portal/marketplace-metering-service-apis.md), y tiene un servicio que se autenticará con un token de seguridad de Azure AD. Para más información, consulte [Estrategias de autenticación del servicio de medición de Marketplace](partner-center-portal/marketplace-metering-service-authentication.md) sobre las distintas opciones de autenticación.

## <a name="technical-configuration-offer-level"></a>Configuración técnica (nivel de oferta)

La pestaña **Technical configuration** (Configuración técnica) solo se aplica si se va a crear una aplicación administrada que emite eventos de medición mediante las [API de facturación de uso medido de Marketplace](partner-center-portal/marketplace-metering-service-apis.md). Si este es el caso, realice los siguientes pasos. Si no, vaya a [Pasos siguientes](#next-steps). 

Para más información sobre estos campos, consulte [Planeamiento de una oferta de Aplicación de Azure para el marketplace comercial](plan-azure-application-offer.md#technical-configuration).

1. En la pestaña **Technical configuration** (Configuración técnica), proporcione el **identificador de inquilino de Azure Active Directory** y el **identificador de aplicación de Azure Active Directory** usados para validar que la conexión entre los dos servicios está detrás de una comunicación autenticada.

1. Seleccione **Guardar borrador** antes de continuar con la siguiente pestaña: Información general del plan.

## <a name="next-steps"></a>Pasos siguientes

- [Creación de planes para la oferta de Aplicación de Azure](create-new-azure-apps-offer-plans.md)
