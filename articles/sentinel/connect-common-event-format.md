---
title: Conexión de datos CEF a la versión preliminar de Azure Sentinel | Microsoft Docs
description: Conecte una solución externa que envía mensajes de formato de evento común (CEF) a Azure Sentinel mediante el uso de una máquina Linux como proxy.
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
ms.openlocfilehash: d63893ab219854a270652da38c474e3ccad83abc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91630515"
---
# <a name="connect-your-external-solution-using-common-event-format"></a>Conexión de su solución externa con Common Event Format

Al conectar una solución externa que envía mensajes de CEF, hay tres pasos para conectarse con Azure Sentinel:

PASO 1: [Conecte CEF implementando un reenviador de Syslog/CEF](connect-cef-agent.md) PASO 2: [Seguir los pasos específicos para la solución](connect-cef-solution-config.md) PASO 3: [Comprobar la conectividad](connect-cef-verify.md)

En este artículo se describe el funcionamiento de la conexión, se proporcionan los requisitos previos y los pasos para implementar el agente en soluciones de seguridad que envían mensajes de formato de evento común (CEF) sobre Syslog. 

> [!NOTE] 
> Los datos se almacenan en la ubicación geográfica del área de trabajo en la que se ejecute Azure Sentinel.

Para establecer esta conexión, debe implementar un servidor de reenviador Syslog para admitir la comunicación entre el dispositivo y Azure Sentinel.  El servidor consta de una máquina Linux dedicada (VM o local) con el agente de Log Analytics para Linux instalado. 

En el siguiente diagrama se describe la configuración en el caso de una VM Linux en Azure:

 ![CEF en Azure](./media/connect-cef/cef-syslog-azure.png)

Esta configuración también existirá si usa una VM en otra nube o en una máquina local: 

 ![CEF local](./media/connect-cef/cef-syslog-onprem.png)

## <a name="security-considerations"></a>Consideraciones sobre la seguridad

Asegúrese de configurar la seguridad de la máquina de acuerdo con la directiva de seguridad de su organización. Por ejemplo, puede configurar la red para que se alinee con la directiva de seguridad de la red corporativa y cambiar los puertos y protocolos del demonio para que se adapten a sus requisitos. Puede usar las siguientes instrucciones para mejorar la configuración de seguridad de la máquina:   [Protección de máquinas virtuales en Azure](../virtual-machines/security-policy.md), [Procedimientos recomendados de seguridad de la red](../security/fundamentals/network-best-practices.md).

Para usar la comunicación TLS entre el origen y el reenviador de Syslog, debe configurar el demonio de Syslog (rsyslog o syslog-ng) para que se comunique en TLS: [Cifrado del tráfico de Syslog con TLS -rsyslog](https://www.rsyslog.com/doc/v8-stable/tutorials/tls_cert_summary.html), [Cifrado de los mensajes de registro con TLS –syslog-ng](https://support.oneidentity.com/technical-documents/syslog-ng-open-source-edition/3.22/administration-guide/60#TOPIC-1209298).
 
## <a name="prerequisites"></a>Requisitos previos

Asegúrese de que la máquina Linux que usa como proxy ejecute uno de los siguientes sistemas operativos:

- 64 bits
  - CentOS 7 y subversiones, y versiones posteriores (no la 6)
  - Amazon Linux 2017.09
  - Oracle Linux 6 y 7
  - Red Hat Enterprise Linux (RHEL) Server 7 y subversiones, y versiones posteriores (no la 6)
  - Debian GNU/Linux 8 y 9
  - Ubuntu Linux 14.04 LTS, 16.04 LTS y 18.04 LTS
  - SUSE Linux Enterprise Server 12
- 32 bits
   - CentOS 7
   - Oracle Linux 6
   - Red Hat Enterprise Linux Server 7
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
    - Asegúrese de que tiene Python (2.7 o versiones posteriores) en ejecución en el equipo.

## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a conectar dispositivos CEF a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).

