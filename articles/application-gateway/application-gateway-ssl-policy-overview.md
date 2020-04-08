---
title: Introducción a la directiva TLS para Azure Application Gateway
description: Obtenga información sobre cómo configurar la directiva TLS para Azure Application Gateway y reducir la sobrecarga de cifrado y descifrado de una granja de servidores back-end.
services: application gateway
author: amsriva
ms.service: application-gateway
ms.topic: article
ms.date: 11/16/2019
ms.author: amsriva
ms.openlocfilehash: 871cb930e867002d8af1e7755de27d4873327543
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80257383"
---
# <a name="application-gateway-tls-policy-overview"></a>Introducción a la directiva TLS de Application Gateway

Puede utilizar Azure Application Gateway para centralizar la administración de certificados TLS/SSL y reducir la sobrecarga de cifrado y descifrado de una granja de servidores back-end. Este control centralizado de TLS también le permite especificar una directiva TLS central que se adapte a los requisitos de seguridad de la organización. Esto le ayuda a cumplir los requisitos de cumplimiento normativo, así como las directrices de seguridad y los procedimientos recomendados.

La directiva TLS incluye el control de la versión del protocolo TLS, así como de los conjuntos de cifrado y el orden en el que se usan los cifrados durante un protocolo de enlace TLS. Application Gateway proporciona dos mecanismos para controlar la directiva TLS. Puede usar una directiva predefinida o una directiva personalizada.

## <a name="predefined-tls-policy"></a>Directiva TLS predefinida

Application Gateway tiene tres directivas de seguridad predefinidas. Puede configurar la puerta de enlace con cualquiera de estas directivas para conseguir el nivel adecuado de seguridad. Los nombres de directiva se anotan según el año y mes en el que se configuraron. Cada directiva ofrece diferentes versiones de protocolos TLS y conjuntos de cifrado. Se recomienda usar las directivas TLS más recientes para garantizar la máxima seguridad TLS.

### <a name="appgwsslpolicy20150501"></a>AppGwSslPolicy20150501

|Propiedad  |Value  |
|---|---|
|Nombre     | AppGwSslPolicy20150501        |
|MinProtocolVersion     | TLSv1_0        |
|Valor predeterminado| True (si no se ha especificado ninguna directiva predefinida) |
|CipherSuites     |TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_DHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_DHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_DHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_DHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA<br>TLS_DHE_DSS_WITH_AES_256_CBC_SHA256<br>TLS_DHE_DSS_WITH_AES_128_CBC_SHA256<br>TLS_DHE_DSS_WITH_AES_256_CBC_SHA<br>TLS_DHE_DSS_WITH_AES_128_CBC_SHA<br>TLS_RSA_WITH_3DES_EDE_CBC_SHA<br>TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA |
  
### <a name="appgwsslpolicy20170401"></a>AppGwSslPolicy20170401
  
|Propiedad  |Value  |
|   ---      |  ---       |
|Nombre     | AppGwSslPolicy20170401        |
|MinProtocolVersion     | TLSv1_1        |
|Valor predeterminado| False |
|CipherSuites     |TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA |
  
### <a name="appgwsslpolicy20170401s"></a>AppGwSslPolicy20170401S

|Propiedad  |Value  |
|---|---|
|Nombre     | AppGwSslPolicy20170401S        |
|MinProtocolVersion     | TLSv1_2        |
|Valor predeterminado| False |
|CipherSuites     |TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256 <br>    TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 <br>    TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA <br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA <br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA<br> |

## <a name="custom-tls-policy"></a>Directiva TLS personalizada

Si una directiva TLS predefinida se debe configurar de acuerdo con sus requisitos, debe definir su propia directiva TLS personalizada. Con una directiva TLS personalizada, tiene un control completo sobre la versión mínima compatible del protocolo TLS, así como de los conjuntos de cifrado compatibles y su orden de prioridad.
 
### <a name="tlsssl-protocol-versions"></a>Versiones del protocolo TLS/SSL

* SSL 2.0 y 3.0 están deshabilitados de forma predeterminada en todas las instancias de Application Gateway. Estas versiones de protocolo no son configurables.
* Una directiva TLS personalizada le ofrece la opción de seleccionar cualquiera de los siguientes tres protocolos como la versión mínima de protocolo TLS para la puerta de enlace: TLSv1_0, TLSv1_1 y TLSv1_2.
* Si no se define ninguna directiva TLS, los tres protocolos (TLSv1_0, TLSv1_1 y TLSv1_2) están habilitados.

### <a name="cipher-suites"></a>Conjuntos de cifrado

Application Gateway admite los siguientes conjuntos de cifrado desde los que puede elegir la directiva personalizada. El orden de los conjuntos de cifrado determina el orden de prioridad durante la negociación TLS.


- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_DHE_RSA_WITH_AES_256_CBC_SHA
- TLS_DHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_3DES_EDE_CBC_SHA
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA

> [!NOTE]
> Los conjuntos de cifrado TLS que se usan para la conexión también se basan en el tipo de certificado que se utiliza. En las conexiones de cliente a Application Gateway, los conjuntos de cifrado usados se basan en el tipo de certificados de servidor en el agente de escucha de Application Gateway. En las conexiones de Application Gateway a grupo de back-end, los conjuntos de cifrado usados se basan en el tipo de certificados de servidor en los servidores de grupo de back-end.

## <a name="known-issue"></a>Problema conocido
Application Gateway v2 no es compatible actualmente con los siguientes cifrados:
- DHE-RSA-AES128-GCM-SHA256
- DHE-RSA-AES128-SHA
- DHE-RSA-AES256-GCM-SHA384
- DHE-RSA-AES256-SHA
- DHE-DSS-AES128-SHA256
- DHE-DSS-AES128-SHA
- DHE-DSS-AES256-SHA256
- DHE-DSS-AES256-SHA

## <a name="next-steps"></a>Pasos siguientes

Si desea aprender a configurar una directiva TLS, consulte [Configurar versiones de directivas SSL y conjuntos de cifrado en Application Gateway](application-gateway-configure-ssl-policy-powershell.md).
