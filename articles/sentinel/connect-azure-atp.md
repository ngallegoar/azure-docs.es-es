---
title: Conexión de datos de Azure ATP con Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo transmitir registros desde Azure Advanced Threat Protection (ATP) a Azure Sentinel con tan solo un clic.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: f58c38ccfa234752a80c05c300d245c6c9e97cf0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85559171"
---
# <a name="connect-data-from-azure-advanced-threat-protection-atp"></a>Conectar datos de Azure Advanced Threat Protection (ATP)

> [!IMPORTANT]
> El conector de datos de Azure Advanced Threat Protection en Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta característica se ofrece sin contrato de nivel de servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Se pueden transmitir registros desde [Azure Advanced Threat Protection](https://docs.microsoft.com/azure-advanced-threat-protection/what-is-atp) a Azure Sentinel con tan solo un clic.

## <a name="prerequisites"></a>Requisitos previos

- Un usuario que sea administrador global o que tenga permisos de administrador de seguridad
- Debe ser cliente de versión preliminar de Azure ATP y habilitar la integración entre Azure ATP y Microsoft Cloud App Security. Para obtener más información, consulte [Integración de Azure Advanced Threat Protection](https://docs.microsoft.com/cloud-app-security/aatp-integration).

## <a name="connect-to-azure-atp"></a>Conectar a Azure ATP

Asegúrese de que la versión preliminar de Azure ATP está [habilitada en la red](https://docs.microsoft.com/azure-advanced-threat-protection/install-atp-step1).
Si Azure ATP ya está implementado e ingiriendo datos, las alertas sospechosas se pueden transmitir a Azure Sentinel muy fácilmente. Las alertas pueden tardar hasta 24 horas en empezar a transmitirse a Azure Sentinel.


1. Para conectar Azure ATP a Azure Sentinel, primero debe habilitar la integración entre Azure ATP y Microsoft Cloud App Security. Para obtener información sobre cómo hacerlo, consulte [Integración de Azure Advanced Threat Protection](https://docs.microsoft.com/cloud-app-security/aatp-integration).

1. En Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, haga clic en el icono de **Azure Advanced Threat Protection (versión preliminar)** .

1. Puede seleccionar si desea que las alertas de Azure ATP generen incidentes automáticamente en Azure Sentinel. En **Creación de incidentes** seleccione **Habilitar** para habilitar la regla analítica predeterminada que crea incidentes automáticamente a partir de alertas generadas en el servicio de seguridad conectado. Luego, puede editar esta regla en **Analytics** (Análisis) y **Active rules** (Reglas activas).

1. Haga clic en **Conectar**.

1. Para usar el esquema correspondiente en Log Analytics para encontrar alertas de Azure ATP, busque **SecurityAlert**.

> [!NOTE]
> Si las alertas superan los 30 KB, Azure Sentinel dejará de mostrar el campo Entidades en las alertas.

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Azure Advanced Threat Protection a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).

