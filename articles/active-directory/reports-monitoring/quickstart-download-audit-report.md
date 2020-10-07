---
title: 'Guía de inicio rápido: Descarga de un informe de auditoría con Azure Portal | Microsoft Docs'
description: Obtenga información sobre cómo descargar un informe de auditoría mediante Azure Portal.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4de121ea-f4aa-4c8a-aae4-700c2c5e97a2
ms.service: active-directory
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: a6cbea49fe39c92c8a2fc50e501cb4ef5cff74b1
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "68989691"
---
# <a name="quickstart-download-an-audit-report-using-the-azure-portal"></a>Guía de inicio rápido: Descarga de un informe de auditoría con Azure Portal

En este inicio rápido, obtendrá información sobre cómo descargar un archivo CSV de los registros de auditoría para el inquilino durante las 24 últimas horas. Puede descargar hasta 250 000 registros de Azure Portal. Los registros se ordenan a partir de los más recientes, así que, de forma predeterminada, se obtienen los 250 000 últimos registros. 

## <a name="prerequisites"></a>Prerequisites

Necesita:

* Un inquilino de Azure Active Directory. 
* Un usuario, que tenga el rol **Administrador de seguridad**, **Lector de seguridad** o **Administrador global** del inquilino. Además, cualquier usuario del inquilino puede acceder a sus propios registros de auditoría.

## <a name="quickstart-download-an-audit-report"></a>Guía de inicio rápido: Descarga de un informe de auditoría

1. Acceda a [Azure Portal](https://portal.azure.com).
2. Seleccione **Azure Active Directory** desde el panel de navegación izquierdo y use el botón **Cambiar directorio** para seleccionar su directorio activo.
3. En el panel, seleccione **Azure Active Directory** y luego **Registros de auditoría**. 
4. Elija **Últimas 24 horas** en el menú desplegable de filtro **Intervalo de fechas** y seleccione **Aplicar** para ver los registros de auditoría de las 24 últimas horas. 
5. Seleccione el botón **Descargar**, elija **CSV** como el formato de archivo y especifique un nombre de archivo para descargar un archivo CSV que contiene los registros filtrados. 

![Notificación](./media/quickstart-download-audit-report/download-audit-logs.png)

## <a name="next-steps"></a>Pasos siguientes

* [Informes de actividad de inicio de sesión en el portal de Azure Active Directory](concept-sign-ins.md)
* [Retención de informes de Azure Active Directory](reference-reports-data-retention.md)
* [Latencias de informes de Azure Active Directory](reference-reports-latencies.md)
