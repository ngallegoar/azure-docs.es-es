---
title: Conexión de datos de Azure Active Directory a Azure Sentinel | Microsoft Docs
description: Aprenda a recopilar datos de Azure Active Directory y a transmitir registros de inicio de sesión y registros de auditoría de Azure AD a Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 0a8f4a58-e96a-4883-adf3-6b8b49208e6a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: 37106517c47c86f4a4a562eebd6d120e31e22334
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85564534"
---
# <a name="connect-data-from-azure-active-directory-azure-ad"></a>Conexión de datos desde Azure Active Directory (Azure AD)



Azure Sentinel permite recopilar datos de [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) y transmitirlos a Azure Sentinel. Puede elegir si transmitir [registros de inicio de sesión](../active-directory/reports-monitoring/concept-sign-ins.md) y [registros de auditoría](../active-directory/reports-monitoring/concept-audit-logs.md).

## <a name="prerequisites"></a>Requisitos previos

- Si quiere exportar datos de inicio de sesión desde Azure AD, debe tener una licencia de Azure AD P1 o P2.

- Usuario con permisos de administrador global o administrador de seguridad en el inquilino desde el que se van a transmitir los registros.

- Para poder ver el estado de conexión, debe tener permiso para acceder a los registros de diagnóstico de Azure AD. 


## <a name="connect-to-azure-active-directory"></a>Conectarse a Azure Active Directory

1. En el menú de navegación de Azure Sentinel, seleccione **Conectores de datos**.

1. En la galería de conectores de datos, seleccione **Azure Active Directory** y, a continuación, haga clic en el botón **Abrir la página del conector**.

1. Active las casillas que se encuentran junto a los registros que quiere transmitir a Azure Sentinel y haga clic en **Conectar**.

1. Puede seleccionar si desea que las alertas de Azure AD generen incidentes automáticamente en Azure Sentinel. En **Creación de incidentes** seleccione **Habilitar** para habilitar la regla analítica predeterminada que crea incidentes automáticamente a partir de alertas generadas en el servicio de seguridad conectado. Luego, puede editar esta regla en **Analytics** (Análisis) y **Active rules** (Reglas activas).

1. Si quiere usar el esquema correspondiente en Log Analytics para consultar las alertas de Azure AD, escriba `SigninLogs` o `AuditLogs` en la ventana de consulta.




## <a name="next-steps"></a>Pasos siguientes
En este documento, aprendió a conectar Azure Active Directory a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
