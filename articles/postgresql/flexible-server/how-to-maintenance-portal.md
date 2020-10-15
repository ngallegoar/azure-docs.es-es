---
title: 'Servidor flexible de Azure Database for PostgreSQL (versión preliminar) - Mantenimiento programado: Azure Portal'
description: Obtenga información sobre cómo configurar las opciones de mantenimiento programado para un servidor flexible de Azure Database for PostgreSQL desde Azure Portal.
author: niklarin
ms.author: nlarin
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: be6040b8b84a4b86746d62bd2f1c07f0ffea0a3b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91336299"
---
# <a name="manage-scheduled-maintenance-settings-for-azure-database-for-postgresql--flexible-server"></a>Administración de la configuración de mantenimiento programado para Servidor flexible de Azure Database for PostgreSQL
 
En la suscripción de Azure puede especificar opciones de mantenimiento para cada servidor flexible. Las opciones incluyen la programación de mantenimiento y la configuración de notificación para eventos de mantenimiento próximos y finalizados.

> [!IMPORTANT]
> Servidor flexible de Azure Database for PostgreSQL está en versión preliminar.

## <a name="prerequisites"></a>Requisitos previos
Para completar esta guía, necesita:
- Un [servidor flexible de Azure Database for PostgreSQL](quickstart-create-server-portal.md)
 
## <a name="specify-maintenance-schedule-options"></a>Especificación de opciones de programación de mantenimiento
 
1. En la página del servidor PostgreSQL, bajo el título **Configuración**, elija **Mantenimiento** para abrir las opciones de mantenimiento programado.
2. La programación predeterminada (administrada por el sistema) es un día de la semana aleatorio y una ventana de 60 minutos para el mantenimiento que se inicia entre las 23:00 y las 7:00 (hora del servidor local). Si quiere personalizar esta programación, elija **Programación personalizada**. Después, puede seleccionar el día de la semana que prefiera y una ventana de 60 minutos para la hora de inicio del mantenimiento.
 
## <a name="notifications-about-scheduled-maintenance-events"></a>Notificaciones sobre eventos de mantenimiento programado
 
Puede usar Azure Service Health para [ver notificaciones](../../service-health/service-notifications.md) sobre el próximo mantenimiento programado y realizado en el servidor flexible. También puede [configurar](../../service-health/resource-health-alert-monitor-guide.md) alertas en Azure Service Health para recibir notificaciones sobre los eventos de mantenimiento.
 
## <a name="next-steps"></a>Pasos siguientes  
 
* Información sobre el [mantenimiento programado en el servidor flexible de Azure Database for PostgreSQL](concepts-maintenance.md)
* Información sobre [Azure Service Health](../../service-health/overview.md)
