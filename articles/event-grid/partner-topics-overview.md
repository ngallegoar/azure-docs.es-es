---
title: Temas del asociado de Azure Event Grid
description: Envíe eventos de los asociados PaaS y SaaS de Event Grid de terceros directamente a los servicios de Azure con Azure Event Grid.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/18/2020
ms.author: babanisa
ms.openlocfilehash: f47d63ce79846e94e992df93af1768aad3c17e67
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84170960"
---
# <a name="partner-topics-in-azure-event-grid-preview"></a>Temas del asociado en Azure Event Grid (versión preliminar)
Mediante el uso de los temas del asociado, puede conectar orígenes de eventos de terceros directamente a Azure Event Grid. Esta integración le permite suscribirse a eventos de los asociados de la misma manera que se suscribe a los eventos de los servicios de Azure. 

## <a name="available-partners"></a>Asociados disponibles
El primer asociado disponible mediante los temas del asociado de Event Grid es Auth0. El [tema de asociado Auth0](auth0-overview.md) se puede usar para conectar las cuentas de Auth0 y de Azure. La integración permite reaccionar a eventos de Auth0 en tiempo real, así como registrarlos y supervisarlos.

[Para probarlo](auth0-how-to.md), inicie sesión en su cuenta de Auth0 y cree una integración de Event Grid. Después de hacer clic en **Crear** en Auth0, verá un tema Auth0 pendiente en su cuenta de Azure. Seleccione **Activar** y podrá crear suscripciones a Event Grid para enrutar, filtrar y enviar los eventos de la misma manera que con cualquier otro origen de eventos.

## <a name="pricing"></a>Precios
Los temas del asociado se cobran con la misma tarifa de operación que los temas del sistema.

## <a name="limits"></a>Límites
Los temas del asociado se encuentran en versión preliminar pública. Durante la versión preliminar pública, los temas del asociado están sujetos a los [mismos límites](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#event-grid-limits) que los temas del sistema y los temas personalizados.

## <a name="how-do-i-become-an-event-grid-partner"></a>¿Cómo ser un asociado de Event Grid?
La infraestructura creada para admitir esta versión hace que los nuevos asociados puedan integrar de forma fácil y rápida sus funcionalidades de generación de eventos con Event Grid. Para más información, consulte la [documentación de incorporación de asociados](partner-onboarding-overview.md).

## <a name="next-steps"></a>Pasos siguientes

- [Tema del asociado Auth0](auth0-overview.md)
- [Cómo usar el tema del asociado Auth0](auth0-how-to.md)
- [Convertirse en asociado de Event Grid](partner-onboarding-overview.md)