---
title: Conectar datos de firewall de aplicaciones web a Azure Sentinel
description: Aprenda a conectar datos de firewall de aplicaciones web de Microsoft a Azure Sentinel.
author: yelevin
manager: rkarlin
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: a5cef16694fa2cfae036152d22cfa4473956fc72
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588185"
---
# <a name="connect-data-from-microsoft-web-application-firewall"></a>Conectar datos del firewall de aplicaciones web de Microsoft



Los registros de firewall de aplicaciones web (WAF) de Microsoft de Azure Application Gateway se pueden transmitir. Este WAF protege las aplicaciones de vulnerabilidades web comunes, como la inyección de código SQL o los ataques de scripts de sitios, y permite personalizar reglas para reducir los falsos positivos. Siga estas instrucciones para transmitir los registros de firewall de aplicaciones web de Microsoft a Azure Sentinel.


## <a name="prerequisites"></a>Prerrequisitos

- Un recurso de puerta de enlace de aplicaciones existente.

## <a name="connect-to-microsoft-web-application-firewall"></a>Una conexión a un firewall de aplicaciones web de Microsoft.

Si ya tiene un firewall de aplicaciones web de Microsoft, asegúrese de que tiene un recurso de puerta de enlace existente.
Cuando el firewall de aplicaciones web de Microsoft esté implementado y obteniendo datos, los datos de alertas se pueden transmitir a Azure Sentinel muy fácilmente.
    
1. En el portal de Azure Sentinel, seleccione **Data connectors** (Conectores de datos).
1. En la página Data connectors (Conectores de datos), seleccione el icono de **WAF**.
1. Vaya a [Application Gateway resource](https://ms.portal.azure.com/#blade/HubsExtension/BrowseAllResourcesBlade/resourceType/Microsoft.Network%2FapplicationGateways)  (Recurso de Application Gateway) y elija el WAF.
    1. Seleccione **Configuración de diagnóstico**.
    1. Seleccione **+ Add diagnostic setting** (+ Agregar configuración de diagnóstico) debajo de la tabla.
    1. En la página **Diagnostic settings** (Configuración de diagnóstico), indique un nombre en **Name** y seleccione **Send to Log Analytics** (Enviar a Log Analytics).
    1. En **Log Analytics Workspace** (Área de trabajo de Log Analytics), seleccione el área de trabajo Azure Sentinel.
    1. Seleccione los tipos de registro que quiere analizar. Nuestra recomendación: ApplicationGatewayAccessLog y ApplicationGatewayFirewallLog.
1. Para usar el esquema correspondiente de Log Analytics para las alertas de firewall de aplicaciones de web de Microsoft, busque **AzureDiagnostics**.

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar un firewall de aplicaciones de web de Microsoft a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
