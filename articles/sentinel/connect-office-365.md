---
title: Conexión de registros de Office 365 a Azure Sentinel | Microsoft Docs
description: Obtenga información sobre el uso del conector de registro de Office 365 para incluir información sobre las actividades actuales de los usuarios y del administrador en Exchange y SharePoint (incluido OneDrive).
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/21/2020
ms.author: yelevin
ms.openlocfilehash: 180b25f80bd27caea20b1c17cd84fda38c172e0f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85559345"
---
# <a name="connect-office-365-logs-to-azure-sentinel"></a>Conexión de registros de Office 365 a Azure Sentinel

El conector de registro de [Office 365](https://docs.microsoft.com/office/) incluye información de Azure Sentinel sobre las actividades de usuario y administración continuas en **Exchange** y **SharePoint** (incluido **OneDrive**). Esta información incluye detalles de acciones como descargas de archivos, solicitudes de acceso enviadas, cambios en eventos de grupo y operaciones de buzón de correo, así como los detalles del usuario que realizó las acciones. La conexión de registros de Office 365 a Azure Sentinel le permite ver y analizar estos datos en los libros, consultarlos para crear alertas personalizadas e incorporarlas para mejorar el proceso de investigación, lo que le proporciona más información sobre la seguridad de Office 365.

## <a name="prerequisites"></a>Requisitos previos

- Debe tener permisos de lectura y escritura en el área de trabajo de Azure Sentinel.

- Debe ser administrador global o administrador de seguridad en el inquilino.

- La implementación de Office 365 debe estar en el mismo inquilino que el área de trabajo de Azure Sentinel.

> [!IMPORTANT]
> - Para que el conector pueda acceder a los datos a través de la API de Actividad de administración de Office 365, debe tener el **registro de auditoría unificado** habilitado en la implementación de Office 365. En función del tipo de licencia de Office 365/Microsoft 365 que tenga, puede estar habilitada o deshabilitada de forma predeterminada. Consulte el [Centro de seguridad y cumplimiento de Office 365](https://docs.microsoft.com/office365/servicedescriptions/office-365-platform-service-description/office-365-securitycompliance-center) para comprobar el estado del registro de auditoría unificado según el tipo de licencia.
> - También puede habilitar, deshabilitar y comprobar manualmente el estado actual del registro de auditoría unificado de Office 365. Para obtener instrucciones, consulte [Activar o desactivar la búsqueda de registros de auditoría de Office 365](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off).
> - Para más información, consulte [Referencia de la API de Actividad de administración de Office 365](https://docs.microsoft.com/office/office-365-management-api/office-365-management-activity-api-reference).


   > [!NOTE]
   > Como se indicó anteriormente, y como verá en la página del conector en **Tipos de datos**, el conector de Office 365 de Azure Sentinel admite actualmente la ingesta de registros de auditoría solo de Microsoft Exchange y SharePoint (incluido OneDrive). Sin embargo, hay algunas soluciones externas si está interesado en incorporar [datos de Teams](https://techcommunity.microsoft.com/t5/azure-sentinel/protecting-your-teams-with-azure-sentinel/ba-p/1265761) u [otros datos de Office](https://techcommunity.microsoft.com/t5/azure-sentinel/ingesting-office-365-alerts-with-graph-security-api/ba-p/984888) a Azure Sentinel. 

## <a name="enable-the-office-365-log-connector"></a>Habilitación del conector de registro de Office 365

1. En el menú de navegación de Azure Sentinel, seleccione **Conectores de datos**.

1. En la lista **Conectores de datos**, haga clic en **Office 365** y, a continuación, en el botón **Abrir la página del conector** de la esquina inferior derecha.

1. En la sección con la etiqueta **Configuración**, active las casillas de los registros de actividad de Office 365 que desea conectar a Azure Sentinel y haga clic en **Aplicar cambios**. 

   > [!NOTE]
   > Si ya ha conectado anteriormente varios inquilinos a Azure Sentinel con una versión anterior del conector de Office 365 que lo permita, podrá ver y modificar los registros recopilados de cada inquilino. No podrá agregar inquilinos adicionales, pero puede quitar los inquilinos agregados anteriormente.

1. Para consultar los datos de registro de Office 365 en Log Analytics, escriba `OfficeActivity` en la primera línea de la ventana de consulta.

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Office 365 a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a detectar amenazas con Azure Sentinel mediante las reglas [integradas](tutorial-detect-threats-built-in.md) o [personalizadas](tutorial-detect-threats-custom.md).

