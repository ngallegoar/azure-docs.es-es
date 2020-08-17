---
title: Introducción a Azure Arc para servidores (versión preliminar)
description: Aprenda a usar Azure Arc para servidores para administrar máquinas hospedadas fuera de Azure, como si se tratase de un recurso de Azure.
keywords: azure automation, DSC, powershell, desired state configuration, update management, change tracking, inventory, runbooks, python, graphical, hybrid
ms.custom: references_regions
ms.date: 08/06/2020
ms.topic: overview
ms.openlocfilehash: f11eedaf5f70cb24fa6c1588b7f26b2eed4734ce
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2020
ms.locfileid: "88121806"
---
# <a name="what-is-azure-arc-for-servers-preview"></a>¿Qué es Azure Arc para servidores (versión preliminar)?

Azure Arc para servidores (versión preliminar) le permite administrar las máquinas Windows y Linux hospedadas fuera de Azure en la red corporativa o en otro proveedor en la nube, de forma similar a cómo se administran las máquinas virtuales nativas de Azure. Cuando una máquina híbrida se conecta a Azure, se convierte en una máquina conectada y se trata como un recurso de Azure. Cada máquina conectada tiene un identificador de recurso, se administra como parte de un grupo de recursos de una suscripción y se beneficia de las construcciones estándar de Azure como Azure Policy y la aplicación de etiquetas.

Para ofrecer esta experiencia con las máquinas híbridas hospedadas fuera de Azure, el agente de Azure Connected Machine debe estar instalado en cada una de las máquinas que planea conectar con Azure. Este agente no proporciona ninguna otra funcionalidad y no reemplaza al [agente de Log Analytics](../../azure-monitor/platform/log-analytics-agent.md). El agente de Log Analytics para Windows y Linux es necesario si desea supervisar de forma proactiva el sistema operativo y las cargas de trabajo que se ejecutan en la máquina, administrarlos mediante runbooks de Automation o soluciones como Update Management, o usar otros servicios de Azure como [Azure Security Center](../../security-center/security-center-intro.md).

>[!NOTE]
>Esta versión preliminar está pensada para fines de evaluación y se recomienda no administrar máquinas de producción críticas.
>

## <a name="supported-scenarios"></a>Escenarios admitidos

Al conectar la máquina a Azure Arc para servidores (versión preliminar), se habilita la capacidad de realizar las siguientes tareas de administración de la configuración:

- Asigne [configuraciones de invitado de Azure Policy](../../governance/policy/concepts/guest-configuration.md) de la misma manera que en la asignación de directivas para máquinas virtuales de Azure.

- Supervise el rendimiento del sistema operativo invitado de la máquina conectada y descubra los componentes de la aplicación para supervisar sus procesos y dependencias con otros recursos con los que la aplicación se comunica mediante [Azure Monitor para VM](../../azure-monitor/insights/vminsights-overview.md).

- Simplifique la implementación con otros servicios de Azure, como el área de trabajo de Log Analytics de Azure Monitor y State Configuration de Azure Automation, con las [extensiones de máquina virtual de Azure](manage-vm-extensions.md) admitidas para las máquinas Windows o Linux que no sean de Azure. Esto incluye realizar la instalación de software o la configuración posterior a la implementación mediante la extensión de script personalizado.

Los datos de registro recopilados y almacenados en un área de trabajo de Log Analytics desde la máquina híbrida ahora contienen propiedades específicas de la máquina, como un identificador de recurso. Se puede usar para admitir el acceso al registro [resource-context](../../azure-monitor/platform/design-logs-deployment.md#access-mode).

## <a name="supported-regions"></a>Regiones admitidas

En Azure Arc para servidores (versión preliminar), solo se admiten determinadas regiones:

- EastUS
- WestUS2
- Oeste de Europa
- SoutheastAsia

En la mayoría de los casos, la ubicación que seleccione al crear el script de instalación debe ser la región de Azure más cercana geográficamente a la ubicación de la máquina. Los datos en reposo se almacenarán en la ubicación geográfica de Azure que contiene la región que especifique, lo que también puede afectar a su elección de región si tiene requisitos de residencia de datos. Aunque la región de Azure a la que está conectada la máquina experimente una interrupción, la máquina conectada no se verá afectada, aunque es posible que las operaciones de administración que usan Azure no se puedan completar. En el caso de una interrupción regional, si dispone de varias ubicaciones que proporcionen un servicio con redundancia geográfica, es mejor conectar las máquinas de cada ubicación a una región diferente de Azure.

### <a name="agent-status"></a>Estado del agente

El agente Connected Machine envía un mensaje de latido al servicio cada 5 minutos. Si el servicio deja de recibir estos mensajes de latido de una máquina, esa máquina se considera sin conexión y el estado cambiará automáticamente a **Desconectado** en el portal en un plazo de 15 a 30 minutos. Al recibir un mensaje de latido subsiguiente del agente de Connected Machine, su estado se cambiará automáticamente a **Conectado**.

## <a name="next-steps"></a>Pasos siguientes

Antes de evaluar o habilitar Arc para los servidores (versión preliminar) en varias máquinas híbridas, revise el artículo [Introducción al agente de Connected Machine ](agent-overview.md) para conocer lo que es necesario, los detalles técnicos sobre el agente y los métodos de implementación.
