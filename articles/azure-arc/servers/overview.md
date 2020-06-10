---
title: Introducción a Azure Arc para servidores (versión preliminar)
description: Aprenda a usar Azure Arc para servidores para administrar máquinas hospedadas fuera de Azure, como si se tratase de un recurso de Azure.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
keywords: azure automation, DSC, powershell, desired state configuration, update management, change tracking, inventory, runbooks, python, graphical, hybrid
ms.date: 03/24/2020
ms.topic: overview
ms.openlocfilehash: c9e69e221dc21baae96ab814cb9fb0b65ce98126
ms.sourcegitcommit: 0a5bb9622ee6a20d96db07cc6dd45d8e23d5554a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2020
ms.locfileid: "84449488"
---
# <a name="what-is-azure-arc-for-servers-preview"></a>¿Qué es Azure Arc para servidores (versión preliminar)?

Azure Arc para servidores (versión preliminar) le permite administrar las máquinas Windows y Linux hospedadas fuera de Azure en la red corporativa o en otro proveedor en la nube, de forma similar a cómo se administran las máquinas virtuales nativas de Azure. Cuando una máquina híbrida se conecta a Azure, se convierte en una máquina conectada y se trata como un recurso de Azure. Cada máquina conectada tiene un identificador de recurso, se administra como parte de un grupo de recursos de una suscripción y se beneficia de las construcciones estándar de Azure como Azure Policy y la aplicación de etiquetas.

Para ofrecer esta experiencia con las máquinas híbridas hospedadas fuera de Azure, el agente de Azure Connected Machine debe estar instalado en cada una de las máquinas que planea conectar con Azure. Este agente no proporciona ninguna otra funcionalidad y no reemplaza al [agente de Log Analytics](../../azure-monitor/platform/log-analytics-agent.md). El agente de Log Analytics para Windows y Linux es necesario si desea supervisar de forma proactiva el sistema operativo y las cargas de trabajo que se ejecutan en la máquina, administrarlos mediante runbooks de Automation o soluciones como Update Management, o usar otros servicios de Azure como [Azure Security Center](../../security-center/security-center-intro.md).

>[!NOTE]
>Esta versión preliminar está pensada para fines de evaluación y se recomienda no administrar máquinas de producción críticas.
>

## <a name="supported-scenarios"></a>Escenarios admitidos

Azure Arc para servidores (versión preliminar) admite los siguientes escenarios con máquinas conectadas:

- Asigne [configuraciones de invitado de Azure Policy](../../governance/policy/concepts/guest-configuration.md) de la misma manera que en la asignación de directivas para máquinas virtuales de Azure.
- Datos de registro recopilados por el agente de Log Analytics, almacenados en el área de trabajo de Log Analytics donde está registrada la máquina. Los datos de registro de la máquina híbrida contienen ahora propiedades específicas de la máquina, como un identificador de recurso, que se puede usar para admitir el acceso al registro de [contexto de recursos](../../azure-monitor/platform/design-logs-deployment.md#access-mode).

## <a name="supported-regions"></a>Regiones admitidas

En Azure Arc para servidores (versión preliminar), solo se admiten determinadas regiones:

- EastUS
- WestUS2
- Oeste de Europa
- SoutheastAsia

En la mayoría de los casos, la ubicación que seleccione al crear el script de instalación debe ser la región de Azure más cercana geográficamente a la ubicación de la máquina. Los datos en reposo se almacenarán en la ubicación geográfica de Azure que contiene la región que especifique, lo que también puede afectar a su elección de región si tiene requisitos de residencia de datos. Aunque la región de Azure a la que está conectada la máquina experimente una interrupción, la máquina conectada no se verá afectada, aunque es posible que las operaciones de administración que usan Azure no se puedan completar. Para lograr resistencia en caso de una interrupción regional, si dispone de varias ubicaciones que proporcionan un servicio con redundancia geográfica, es mejor conectar las máquinas de cada ubicación a una región diferente de Azure.

### <a name="agent-status"></a>Estado del agente

El agente Connected Machine envía un mensaje de latido al servicio cada 5 minutos. Si el servicio deja de recibir estos mensajes de latido de una máquina, esa máquina se considera sin conexión y el estado cambiará automáticamente a **Desconectado** en el portal en un plazo de 15 a 30 minutos. Al recibir un mensaje de latido subsiguiente del agente de Connected Machine, su estado se cambiará automáticamente a **Conectado**.

## <a name="next-steps"></a>Pasos siguientes

Antes de evaluar o habilitar Arc para los servidores (versión preliminar) en varias máquinas híbridas, revise el artículo [Introducción al agente de Connected Machine ](agent-overview.md) para conocer lo que es necesario, los detalles técnicos sobre el agente y los métodos de implementación.
