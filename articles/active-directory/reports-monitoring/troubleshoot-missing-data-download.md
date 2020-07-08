---
title: 'Solución de problemas: Faltan datos en los registros de actividad descargados | Microsoft Docs'
description: Proporciona una solución para los datos que faltan en los registros de actividad de Azure Active Directory descargados.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5003d7b644a0c75401a17ed6a37f31acd8180aa9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85608082"
---
# <a name="i-cant-find-all-the-data-in-the-azure-active-directory-activity-logs-i-downloaded"></a>No encuentro todos los datos en los registros de actividad de Azure Active Directory que he descargado

## <a name="symptoms"></a>Síntomas

Descargue los registros de actividad (auditoría o inicios de sesión) y no veo todos los registros del tiempo que elegí. ¿Por qué? 

 ![Notificación](./media/troubleshoot-missing-data-download/01.png)
 
## <a name="cause"></a>Causa

Al descargar los registros de actividad en Azure Portal, limitamos la escala a 250 000 registros, con el más reciente primero. 

## <a name="resolution"></a>Solución

Puede sacar provecho de la [API de creación de informes de Azure AD](concept-reporting-api.md) para capturar hasta un millones de registros en cualquier momento dado.

## <a name="next-steps"></a>Pasos siguientes

* [P+F sobre los informes de Azure Active Directory](reports-faq.md)

