---
title: Conexión de datos de inteligencia sobre amenazas con Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo conectar datos de inteligencia sobre amenazas a Azure Sentinel.
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
ms.date: 11/22/2019
ms.author: yelevin
ms.openlocfilehash: 205cc6eea5d1ac3be2d0e266621067dc8e20d2f9
ms.sourcegitcommit: b8a175b6391cddd5a2c92575c311cc3e8c820018
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96121747"
---
# <a name="connect-data-from-threat-intelligence-providers"></a>Conectar datos de proveedores de inteligencia sobre amenazas

> [!IMPORTANT]
> El conector de datos de inteligencia sobre amenazas en Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta característica se ofrece sin contrato de nivel de servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Sentinel le permite importar los indicadores de amenazas que usa su organización, lo que puede mejorar la capacidad de los analistas de seguridad para detectar y priorizar las amenazas conocidas. Varias características de Azure Sentinel están disponibles o se han mejorado:

- **Analytics** incluye un conjunto de plantillas de reglas programadas que puede habilitar para generar alertas e incidentes basados en coincidencias con eventos de registro de los indicadores de amenazas.

- **Libros** proporciona información resumida sobre los indicadores de amenazas importados a Azure Sentinel y las alertas generadas a partir de las reglas de análisis que coinciden con los indicadores de amenazas.

- **Búsqueda** de consultas permite a los investigadores de seguridad usar indicadores de amenazas en el contexto de los escenarios de búsqueda habituales.

- **Notebooks** puede usar indicadores de amenazas al investigar anomalías y buscar comportamientos malintencionados.

Puede transmitir los indicadores de amenazas a Azure Sentinel mediante uno de los productos integrados de la plataforma de inteligencia sobre amenazas (TIP) que se enumeran en la sección siguiente, la conexión a servidores TAXII o la integración directa con la [API Microsoft Graph Security tiIndicators](/graph/api/resources/tiindicator).

## <a name="integrated-threat-intelligence-platform-products"></a>Productos de la plataforma de inteligencia sobre amenazas integrada

- [Plataforma de inteligencia sobre amenazas de código abierto de MISP](https://www.misp-project.org/)
    
    Para ver un script de ejemplo que proporciona a los clientes instancias de MISP para migrar indicadores de amenazas a Microsoft Graph Security API, consulte el artículo [MISP to Microsoft Graph Security Script](https://github.com/microsoftgraph/security-api-solutions/tree/master/Samples/MISP) (MISP para un script de Microsoft Graph Security).

- [Anomali ThreatStream](https://www.anomali.com/products/threatstream)

    Para descargar ThreatStream Integrator y sus extensiones, así como las instrucciones para conectar ThreatStream Intelligence a la API de Microsoft Graph Security, consulte la página de [descargas de ThreatStream](https://ui.threatstream.com/downloads).

- [Palo Alto Networks MineMeld](https://www.paloaltonetworks.com/products/secure-the-network/subscriptions/minemeld)
    
    Para obtener instrucciones detalladas, consulte [Sending IOCs to the Microsoft Graph Security API using MineMeld](https://live.paloaltonetworks.com/t5/MineMeld-Articles/Sending-IOCs-to-the-Microsoft-Graph-Security-API-using-MineMeld/ta-p/258540) (Envío de IOC a Microsoft Graph Security API mediante MineMeld).

- [Plataforma ThreatConnect](https://threatconnect.com/solution/)

    Para obtener información, consulte las [Integraciones de ThreatConnect](https://threatconnect.com/integrations/) y busque la API Microsoft Graph Security en la página.

- [Plataforma EclecticIQ](https://www.eclecticiq.com/solutions)

- [Plataforma de inteligencia sobre amenazas ThreatQ](https://www.threatq.com/)

    Puede encontrar información e instrucciones guiadas en [Microsoft Sentinel Connector para la integración con ThreatQ](https://appsource.microsoft.com/product/web-apps/threatquotientinc1595345895602.microsoft-sentinel-connector-threatq?src=health&tab=Overview).

## <a name="connect-azure-sentinel-to-your-threat-intelligence-platform"></a>Conexión de Azure Sentinel a su plataforma de inteligencia sobre amenazas

### <a name="prerequisites"></a>Requisitos previos  

- Rol de Azure AD de administrador global o de administrador de seguridad para conceder permisos al producto TIP o a la aplicación personalizada que usa integración directa con Microsoft Graph Security tiIndicators API.

- Permisos de lectura y escritura en el área de trabajo de Azure Sentinel para almacenar los indicadores de amenazas.

### <a name="instructions"></a>Instructions

1. [Registre una aplicación](/graph/auth-v2-service#1-register-your-app) en Azure Active Directory para obtener un identificador de aplicación, un secreto de aplicación y un identificador de inquilino de Azure Active Directory. Necesitará estos valores para cuando configure el producto TIP o la aplicación integrada que usa la integración directa con Microsoft Graph Security tiIndicators API.

2. [Configure los permisos de la API](/graph/auth-v2-service#2-configure-permissions-for-microsoft-graph) para la aplicación registrada: Agregue el permiso de aplicación de Microsoft Graph **ThreatIndicators.ReadWrite.OwnedBy** a la aplicación registrada.

3. Pida a su administrador de inquilinos de Azure Active Directory que conceda consentimiento de administrador a la aplicación registrada de su organización. En el Portal de Azure: **Azure Active Directory** > **Registros de aplicaciones** >  **\<_app name_>**  > **Ver permisos de API** > **Conceder consentimiento de administrador para \<_tenant name_>** .

4. Configure el producto TIP o la aplicación que usa la integración directa con Microsoft Graph Security tiIndicators API para enviar indicadores a Azure Sentinel especificando lo siguiente:
    
    a. Los valores del identificador, el secreto y el identificador de inquilino de la aplicación registrada.
    
    b. Para el producto de destino, especifique Azure Sentinel.
    
    c. Para la acción, especifique alerta.

5. En Azure Portal, vaya a **Azure Sentinel** > **Data connectors** (Conectores de datos) y, a continuación, seleccione el conector **Plataformas de inteligencia sobre amenazas (versión preliminar)** .

6. Seleccione **Open connector page** (Abrir página del conductor) y, a continuación, **Conectar**.

7. Para ver los indicadores de amenazas que se importan en Azure Sentinel, vaya a **Azure Sentinel: Registros** > **SecurityInsights** y amplíe **ThreatIntelligenceIndicator**.

## <a name="connect-azure-sentinel-to-taxii-servers"></a>Conexión de Azure Sentinel con servidores TAXII

### <a name="prerequisites"></a>Requisitos previos

- Permisos de lectura y escritura en el área de trabajo de Azure Sentinel para almacenar los indicadores de amenazas.

- Identificador de colección y URI del servidor TAXII 2.0.

### <a name="instructions"></a>Instructions

1. En Azure Portal, vaya a **Azure Sentinel** > **Data connectors** (Conectores de datos) y, a continuación, seleccione el conector **Inteligencia sobre amenazas: TAXII (versión preliminar)** .

2. Seleccione **Open connector page** (Abrir página del conector).

3. Especifique la información necesaria y opcional en los cuadros de texto.

4. Seleccione **Agregar** para habilitar la conexión al servidor TAXII 2.0.

5. Si tiene más servidores TAXII 2.0: repita los pasos 3 y 4.

6. Para ver los indicadores de amenazas que se importan en Azure Sentinel, vaya a **Azure Sentinel: Registros** > **SecurityInsights** y amplíe **ThreatIntelligenceIndicator**.

## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a conectar su proveedor de inteligencia sobre amenazas a Azure Sentinel. Para obtener más información sobre Azure Sentinel, consulte los siguientes artículos.

- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](./tutorial-detect-threats-built-in.md).