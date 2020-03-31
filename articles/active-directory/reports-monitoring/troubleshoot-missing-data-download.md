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
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: ee07fdb6f8a4e69600297bdb16b6ad74793f10c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74007714"
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

