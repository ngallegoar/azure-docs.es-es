---
title: Introducción a los registros de plataforma de Azure | Microsoft Docs
description: Información general sobre los registros de Azure Monitor, que proporcionan datos exhaustivos y frecuentes sobre el funcionamiento de un recurso de Azure.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/19/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 89de6b3737c8a1e91832aba8f749078806b64e90
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77659327"
---
# <a name="overview-of-azure-platform-logs"></a>Introducción a los registros de plataforma Azure
Los registros de plataforma proporcionan información detallada de diagnóstico y auditoría para los recursos de Azure y la plataforma Azure de la que dependen. Se generan automáticamente, aunque es necesario configurar determinados registros de plataforma para que se reenvíen a uno o varios destinos para la conservación. En este artículo se proporciona información general sobre los registros de plataforma, incluida la información que proporcionan y cómo puede configurarlos para la recopilación y el análisis.

## <a name="types-of-platform-logs"></a>Tipos de registros de plataforma
En la tabla siguiente se enumeran los registros de plataforma específicos disponibles en diferentes capas de Azure.

| Log | Nivel | Descripción |
|:---|:---|:---|
| Registros del recurso | Recursos de Azure. | Proporcionan una visión general de las operaciones realizadas dentro del mismo recurso de Azure (en el *plano de datos*), por ejemplo, obtener un secreto de un almacén de claves o realizar una solicitud en una base de datos. El contenido de estos registros de recurso varía según el servicio de Azure y el tipo de recurso.<br><br>*Los registros de recurso se conocían anteriormente como registros de diagnóstico*.  |
| Registro de actividades | Suscripción de Azure | Proporciona una visión general de las operaciones de cada recurso de Azure de la suscripción desde fuera (en el *plano de administración*) y de las actualizaciones de los eventos de Service Health. Use el registro de actividad para dar respuesta a los interrogantes _qué_, _quién_ y _cuándo_ de las operaciones de escritura (PUT, POST, DELETE) en los recursos de la suscripción. También puede conocer el estado de la operación y otras propiedades relevantes.  Hay un único registro de actividad para cada suscripción de Azure. |
| Registros de Azure Active Directory | Inquilino de Azure |  Contienen el historial de la actividad de inicio de sesión y la traza de auditoría de los cambios realizados en Azure Active Directory para un inquilino determinado. Consulte [¿Qué son los informes de Azure Active Directory?](../../active-directory/reports-monitoring/overview-reports.md) para obtener una descripción completa de los registros de Azure Active Directory.   |

> [!NOTE]
> El registro de actividad de Azure sirve principalmente para las actividades que se producen en Azure Resource Manager. No hace seguimiento de los recursos que usan el modelo Clásico/RDFE. Algunos tipos de recursos clásicos tienen un proveedor de recursos de servidor proxy en Azure Resource Manager (por ejemplo, Microsoft.ClassicCompute). Si interactúa con un tipo de recurso clásico a través de Azure Resource Manager con estos proveedores de recursos de servidor proxy, las operaciones aparecen en el registro de actividad. Si interactúa con un tipo de recurso clásico fuera de los servidores proxy de Azure Resource Manager, sus acciones solo se registran en el registro de operaciones. Se puede examinar el registro de operaciones en una sección independiente del portal.

![Introducción a los registros de plataforma](media/platform-logs-overview/logs-overview.png)




## <a name="viewing-platform-logs"></a>Visualización de los registros de plataforma
Existen diferentes opciones para ver y analizar los distintos registros de plataforma de Azure.

- Vea el registro de actividades en los eventos de acceso y Azure Portal desde PowerShell y la CLI. Consulte [Visualización y recuperación de eventos del registro de actividad de Azure](activity-log-view.md) para obtener más detalles. 
- Consulte los informes de actividad y seguridad de Azure Active Directory en Azure Portal. Consulte [¿Qué son los informes de Azure Active Directory?](../../active-directory/reports-monitoring/overview-reports.md)  para obtener más detalles.
- Los registros de recurso los generan los recursos de Azure compatibles automáticamente, pero su visualización no está disponible a menos que los envíe a un [destino](#destinations). 

## <a name="destinations"></a>Destinations
Puede enviar registros de plataforma a uno o varios de los destinos de la tabla siguiente en función de sus requisitos de supervisión. Configure los destinos para los registros de plataforma mediante la [creación de una configuración de diagnóstico](diagnostic-settings.md).

| Destination | Escenario | Referencias |
|:---|:---|:---|:---|
| Área de trabajo de Log Analytics | Analice los registros con otros datos de supervisión y aproveche las características de Azure Monitor, como las consultas y las alertas de registro. | [Registro de actividad y registros de recursos](resource-logs-collect-workspace.md)<br>[Registros de Azure Active Directory](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md) |
| Almacenamiento de Azure | Archive los registros para auditoría, análisis estático o copia de seguridad. |[Registro de actividad y registros de recursos](archive-diagnostic-logs.md)<br>[Registros de Azure Active Directory](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md) |
| Centro de eventos | Transmita los registros a sistemas de registro y telemetría de terceros.  |[Registro de actividad y registros de recursos](resource-logs-stream-event-hubs.md)<br>[Registros de Azure Active Directory](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md) |



## <a name="next-steps"></a>Pasos siguientes

* [Visualización del esquema del registro de actividad para distintas categorías](activity-log-schema.md)
* [Ver el esquema del registro de recurso para los diferentes servicios de Azure](diagnostic-logs-schema.md)
