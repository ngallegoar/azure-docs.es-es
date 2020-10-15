---
title: Conexión de datos de secRMM de Squadra Technologies a Azure Sentinel | Microsoft Docs
description: Obtenga información sobre la conexión de datos de secRMM de Squadra Technologies a Azure Sentinel.
services: sentinel
author: yelevin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/20/2020
ms.author: yelevin
ms.openlocfilehash: d904e51321870fb4b61a237c23e425034b76dc0b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "77588117"
---
# <a name="connect-your-squadra-technologies-secrmm-data-to-azure-sentinel"></a>Conexión de datos de secRMM de Squadra Technologies a Azure Sentinel. 

> [!IMPORTANT]
> El conector de datos Security Removable Media Manager (secRMM) de Squadra Technologies de Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta característica se ofrece sin contrato de nivel de servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


El conector secRMM de Squadra Technologies le permite conectar fácilmente sus registros de la solución de seguridad secRMM de Squadra Technologies a Azure Sentinel. Le permite ver paneles, crear alertas personalizadas y mejorar la investigación. Este conector le ofrece información sobre los eventos de almacenamiento extraíble USB. La integración entre secRMM de Squadra Technologies y Azure Sentinel usa la API de REST.


> [!NOTE]
> Los datos se almacenarán en la ubicación geográfica del área de trabajo en la que Azure Sentinel se ejecute.

## <a name="configure-and-connect-squadra-technologies-secrmm"></a>Configuración y conexión de secRMM de Squadra Technologies 

El conector secRMM de Squadra Technologies puede integrar y exportar los registros directamente a Azure Sentinel.
1. En el portal de Azure Sentinel, haga clic en Data connectors (Conectores de datos), seleccione el conector secRMM de Squadra Technologies y luego Open connector page (Abrir página de conectores).

2. Siga los pasos descritos en la [guía de incorporación de Squadra Technologies para Azure Sentinel](http://www.squadratechnologies.com/StaticContent/ProductDownload/secRMM/9.9.0.0/secRMMAzureSentinelAdministratorGuide.pdf) para obtener datos de secRMM de Squadra en Azure Sentinel.   


## <a name="find-your-data"></a>Búsqueda de sus datos

Una vez establecida una conexión correcta, los datos aparecen en Log Analytics en los registros de CustomLogs secRMM_CL.
Para usar el esquema de Log Analytics pertinente a secRMM Squadra Technologies, busque secRMM_CL.

## <a name="validate-connectivity"></a>Validar conectividad
Hasta que los registros empiecen a aparecer en Log Analytics, pueden transcurrir más de 20 minutos. 


## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar secRMM de Squadra Technologies a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.

