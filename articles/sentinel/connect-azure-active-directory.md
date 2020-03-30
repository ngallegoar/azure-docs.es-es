---
title: Conectar datos de Azure AD a Azure Sentinel | Microsoft Docs
description: Aprenda a conectar datos de Azure Active Directory a Azure Sentinel.
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
ms.openlocfilehash: be9241a6156621d3f90dbab2da5bebeb463b4232
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588627"
---
# <a name="connect-data-from-azure-active-directory"></a>Conectar datos desde Azure Active Directory



Azure Sentinel permite recopilar datos de [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) y transmitirlos a Azure Sentinel. Puede elegir si transmitir [registros de inicio de sesión](../active-directory/reports-monitoring/concept-sign-ins.md) y [registros de auditoría](../active-directory/reports-monitoring/concept-audit-logs.md).

## <a name="prerequisites"></a>Prerequisites

- Si quiere exportar datos de inicio de sesión desde Active Directory, debe tener una licencia de Azure AD P1 o P2.

- Usuario con permisos de administrador global o administrador de seguridad en el inquilino desde el que se van a transmitir los registros.

- Para poder ver el estado de conexión, debe tener permiso para acceder a los registros de diagnóstico de Azure AD. 


## <a name="connect-to-azure-ad"></a>Conectarse a Azure

1. En Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, haga clic en el icono de **Azure Active Directory**.

1. Haga clic en **Connect** (Conectar) junto a los registros que quiere transmitir a Azure Sentinel.

1. Puede seleccionar si desea que las alertas de Azure AD generen incidentes automáticamente en Azure Sentinel. En **Creación de incidentes** seleccione **Habilitar** para habilitar la regla analítica predeterminada que crea incidentes automáticamente a partir de alertas generadas en el servicio de seguridad conectado. Luego, puede editar esta regla en **Analytics** (Análisis) y **Active rules** (Reglas activas).

1. Para usar el esquema correspondiente en Log Analytics para encontrar alertas de Azure AD, busque **SigninLogs** y **AuditLogs**.




## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Azure AD a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
