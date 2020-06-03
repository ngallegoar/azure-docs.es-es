---
title: Temas del asociado de Azure Event Grid
description: Envíe eventos de los asociados PaaS y SaaS de Event Grid de terceros directamente a los servicios de Azure con Azure Event Grid.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/18/2020
ms.author: babanisa
ms.openlocfilehash: 4546874b8a6cd8a7e45c3e6957a5181d66c7433f
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83690041"
---
# <a name="partner-topics-in-azure-event-grid-preview"></a>Temas del asociado en Azure Event Grid (versión preliminar)
Los temas del asociado le permiten conectar orígenes de eventos de terceros directamente a Event Grid. Esta integración le permite suscribirse a eventos de los asociados de la misma manera que se suscribe a los eventos de los servicios de Azure. 

## <a name="available-partners"></a>Asociados disponibles
El primer asociado disponible a través de los temas del asociado de Event Grid es Auth0. El [tema del asociado Auth0](auth0-overview.md) le permite conectar sus cuentas de Auth0 y Azure. La integración permite reaccionar, registrar y supervisar eventos de Auth0 en tiempo real.

[Pruébela](auth0-how-to.md) hoy mismo; para ello, inicie sesión en su cuenta de Auth0 y cree una integración de Event Grid. Una vez que haya hecho clic en crear en Auth0, verá un tema pendiente de Auth0 en su cuenta de Azure. Haga clic en activar y podrá crear suscripciones a eventos,así como enrutar, filtrar y enviar los eventos de la misma manera que con cualquier otro origen de eventos.

## <a name="pricing"></a>Precios
Los temas del asociado se cobran con la misma tarifa de operación que los temas del sistema.

## <a name="limits"></a>límites
Los temas del asociado se encuentran en versión preliminar pública. Durante la versión preliminar pública, los temas del asociado están sujetos a los [mismos límites](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#event-grid-limits) que los temas del sistema y los temas personalizados.

## <a name="how-do-i-become-an-event-grid-partner"></a>¿Cómo ser un asociado de Event Grid?
La infraestructura creada para admitir esta versión hace que los nuevos asociados puedan integrar de forma fácil y rápida sus funcionalidades de generación de eventos con Event Grid. Consulte la [documentación de incorporación de asociados](partner-onboarding-overview.md) para obtener más información.

## <a name="next-steps"></a>Pasos siguientes

- [Tema de asociado de Auth0](auth0-overview.md)
- [Cómo usar el tema del asociado de Auth0](auth0-how-to.md)
- [Convertirse en asociado de Event Grid](partner-onboarding-overview.md)