---
title: Conexión de datos CEF a la versión preliminar de Azure Sentinel | Microsoft Docs
description: Conecte una solución externa que envía mensajes de formato de evento común (CEF) a Azure Sentinel mediante una máquina Linux como reenviador de registros.
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
ms.date: 10/01/2020
ms.author: yelevin
ms.openlocfilehash: fd08e6cc953f9d8526174fc96dd4e4d1dc9063f5
ms.sourcegitcommit: 5831eebdecaa68c3e006069b3a00f724bea0875a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94517978"
---
# <a name="connect-your-external-solution-using-common-event-format"></a>Conexión de su solución externa con Common Event Format

Al conectar una solución externa que envía mensajes de CEF, hay tres pasos para conectarse con Azure Sentinel:

PASO 1: [Conecte CEF implementando un reenviador de Syslog/CEF](connect-cef-agent.md) PASO 2: [Seguir los pasos específicos para la solución](connect-cef-solution-config.md) PASO 3: [Comprobar la conectividad](connect-cef-verify.md)

En este artículo se describe el funcionamiento de la conexión, se enumeran los requisitos previos y se muestran los pasos para implementar un mecanismo con soluciones de seguridad que envía mensajes de formato de evento común (CEF) además de Syslog. 

> [!NOTE] 
> Los datos se almacenan en la ubicación geográfica del área de trabajo en la que se ejecute Azure Sentinel.

Para establecer esta conexión, debe implementar un servidor de reenviador Syslog para admitir la comunicación entre el dispositivo y Azure Sentinel.  El servidor consta de una máquina Linux dedicada (VM o local) con el agente de Log Analytics para Linux instalado. 

En el siguiente diagrama se describe la configuración en el caso de una máquina virtual Linux en Azure:

 ![CEF en Azure](./media/connect-cef/cef-syslog-azure.png)

Esta configuración también existirá si usa una VM en otra nube o en una máquina local: 

 ![CEF local](./media/connect-cef/cef-syslog-onprem.png)

## <a name="security-considerations"></a>Consideraciones sobre la seguridad

Asegúrese de configurar la seguridad de la máquina de acuerdo con la directiva de seguridad de su organización. Por ejemplo, puede configurar la red para que se alinee con la directiva de seguridad de la red corporativa y cambiar los puertos y protocolos del demonio para que se adapten a sus requisitos. Puede usar las siguientes instrucciones para mejorar la configuración de seguridad de la máquina:  [Protección de VM en Azure](../virtual-machines/security-policy.md), [Prácticas recomendadas para la seguridad de red](../security/fundamentals/network-best-practices.md).

Para usar la comunicación TLS entre el origen y el reenviador de Syslog, debe configurar el demonio de Syslog (rsyslog o syslog-ng) para que se comunique en TLS: [Cifrado del tráfico de Syslog con TLS -rsyslog](https://www.rsyslog.com/doc/v8-stable/tutorials/tls_cert_summary.html), [Cifrado de los mensajes de registro con TLS –syslog-ng](https://support.oneidentity.com/technical-documents/syslog-ng-open-source-edition/3.22/administration-guide/60#TOPIC-1209298).
 
## <a name="prerequisites"></a>Requisitos previos

Asegúrese de que la máquina Linux que usa como reenviador de registros ejecuta alguno de los siguientes sistemas operativos:

- 64 bits
  - CentOS 7 y 8, incluidas las versiones secundarias (no la 6)
  - Amazon Linux 2017.09
  - Oracle Linux 7
  - Red Hat Enterprise Linux (RHEL) Server 7 y 8, incluidas las versiones secundarias (no la 6)
  - Debian GNU/Linux 8 y 9
  - Ubuntu Linux 14.04 LTS, 16.04 LTS y 18.04 LTS
  - SUSE Linux Enterprise Server 12, 15

- 32 bits
  - CentOS 7 y 8, incluidas las versiones secundarias (no la 6)
  - Oracle Linux 7
  - Red Hat Enterprise Linux (RHEL) Server 7 y 8, incluidas las versiones secundarias (no la 6)
  - Debian GNU/Linux 8 y 9
  - Ubuntu Linux 14.04 LTS y 16.04 LTS
 
- Versiones de demonio
  - Syslog-ng: 2.1 - 3.22.1
  - Rsyslog: v8
  
- RFC de Syslog compatibles
  - Syslog RFC 3164
  - RFC de Syslog 5424
 
Asegúrese de que la máquina cumpla también con los requisitos siguientes: 

- Permisos
  - Debe tener permisos elevados (sudo) en la máquina. 

- Requisitos de software
  - Asegúrese de que Python 2.7 o 3 se esté ejecutando en la máquina.

## <a name="next-steps"></a>Pasos siguientes

En este documento ha aprendido cómo recopila Azure Sentinel los registros de CEF de los dispositivos y las soluciones de seguridad. Para obtener información sobre cómo conectar la solución a Azure Sentinel, vea los siguientes artículos:

- PASO 1: [Conexión de CEF mediante la implementación de un reenviador de Syslog/CEF](connect-cef-agent.md)
- PASO 2: [Ejecución de pasos específicos de la solución](connect-cef-solution-config.md)
- PASO 3: [Comprobar la conectividad](connect-cef-verify.md)

Para obtener más información sobre qué hacer con los datos recopilados en Azure Sentinel, vea los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).

