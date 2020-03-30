---
title: Plataformas compatibles con Azure Security Center | Microsoft Docs
description: En este documento se proporciona una lista de plataformas compatibles con Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/29/2019
ms.author: memildin
ms.openlocfilehash: e13149ba802f0f8b9a565e0aabd86ae05167f18b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78208830"
---
# <a name="supported-platforms"></a>Plataformas compatibles 

## <a name="virtual-machines--servers"></a>Máquinas virtuales/servidores <a name="vm-server"></a>

Security Center admite máquinas virtuales/servidores en diferentes tipos de entornos híbridos:

* Solo Azure
* Azure y entorno local
* Azure y otras nubes
* Azure, otras nubes y entorno local

En el caso de un entorno de Azure activado en una suscripción de Azure, Azure Security Center detectará automáticamente los recursos de IaaS que se implementan en la suscripción.

> [!NOTE]
> Para recibir el conjunto completo de características de seguridad, debe tener el [agente de log Analytics](../azure-monitor/platform/agents-overview.md#log-analytics-agent), que se usa en Azure Security Center, instalado y [configurado correctamente para enviar datos a Azure Security Center](security-center-enable-data-collection.md#manual-agent).

En las secciones siguientes se enumeran los sistemas operativos de servidor admitidos en los que se puede ejecutar el [agente de log Analytics](../azure-monitor/platform/agents-overview.md#log-analytics-agent), utilizado por Azure Security Center.

### <a name="windows-server-operating-systems"></a>Sistemas operativos Windows Server <a name="os-windows"></a>

|SO|Compatible con Azure Security Center|Compatibilidad para la integración con Microsoft Defender ATP|
|:---|:-:|:-:|
|Windows Server 2019|✔|X|
|Windows Server 2016|✔|✔|
|Windows Server 2012 R2|✔|✔|
|Windows Server 2008 R2|✔|✔|

Para obtener más información acerca de las características admitidas para los sistemas operativos Windows, enumerados anteriormente, consulte [Características compatibles con máquinas virtuales y servidores](security-center-services.md#vm-server-features).

### <a name="windows-operating-systems"></a>Sistemas operativos Windows <a name="os-windows (non-server)"></a>

Azure Security Center se integra con servicios de Azure para supervisar y proteger las máquinas virtuales Windows.

### <a name="linux-operating-systems"></a>Sistemas operativos Linux <a name="os-linux"></a>

64 bits

* CentOS 6 y 7
* Amazon Linux 2017.09
* Oracle Linux 6 y Oracle Linux 7
* Red Hat Enterprise Linux Server 6 y 7
* Debian GNU/Linux 8 y 9
* Ubuntu Linux 14.04 LTS, 16.04 LTS y 18.04 LTS
* SUSE Linux Enterprise Server 12

32 bits
* CentOS 6
* Oracle Linux 6
* Red Hat Enterprise Linux Server 6
* Debian GNU/Linux 8 y 9
* Ubuntu Linux 14.04 LTS y 16.04 LTS

> [!NOTE]
> Puesto que la lista de sistemas operativos Linux compatibles cambia constantemente, si lo prefiere, haga clic [aquí](https://github.com/microsoft/OMS-Agent-for-Linux#supported-linux-operating-systems) para ver la lista más actualizada de las versiones compatibles por si ha habido cambios desde que este tema se publicó por última vez.

Para obtener más información acerca de las características admitidas para los sistemas operativos Linux, enumerados anteriormente, consulte [Características compatibles con máquinas virtuales y servidores](security-center-services.md#vm-server-features).

### <a name="managed-virtual-machine-services"></a>Servicios de máquinas virtuales administradas <a name="virtual-machine"></a>

También se crean máquinas virtuales en una suscripción de cliente como parte de algunos servicios administrados de Azure, como Azure Kubernetes (AKS), Azure Databricks, etc. Estas máquinas virtuales también se detectan por Azure Security Center, y el agente de Log Analytics se puede instalar y configurar según los [sistemas operativos Windows/Linux](#os-windows) admitidos, que se indican más arriba.

### <a name="cloud-services"></a>Cloud Services <a name="cloud-services"></a>

También se admiten máquinas virtuales que se ejecuten en un servicio en la nube. Se supervisan los roles de trabajo y web de servicios en la nube que se ejecutan en espacios de producción. Para más información sobre Cloud Services, consulte la [información general sobre Azure Cloud Services](../cloud-services/cloud-services-choose-me.md).

También se admite la protección para las máquinas virtuales que residan en Azure Stack. Para obtener más información acerca de la integración de Security Center con Azure Stack, consulte [Incorporación de máquinas virtuales de Azure Stack a Security Center](https://docs.microsoft.com/azure/security-center/quick-onboard-azure-stack).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [Security Center recopila datos y sobre el agente de Log Analytics](security-center-enable-data-collection.md).
- Obtenga información sobre cómo [Security Center administra y protege los datos](security-center-data-security.md).
- Aprenda a [planear y entender las consideraciones de diseño para adoptar Azure Security Center](security-center-planning-and-operations-guide.md).
- Obtenga información [sobre las características disponibles para los diferentes entornos de nube](security-center-services.md).
- Más información sobre la [protección frente a amenazas en máquinas Windows y Linux en Azure Security Center](threat-protection.md#windows-machines).
