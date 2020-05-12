---
title: Uso de Update Management de Azure Automation con clientes de Configuration Manager
description: Este artículo pretende ayudarle a configurar Microsoft Endpoint Configuration Manager con esta solución para implementar actualizaciones de software en clientes de Configuration Manager.
services: automation
ms.subservice: update-management
author: mgoedtel
ms.author: magoedte
ms.date: 12/11/2019
ms.topic: conceptual
ms.openlocfilehash: 2dbc33aa56c7e930596ba6806ba1dd2e128e1c82
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/04/2020
ms.locfileid: "82780222"
---
# <a name="deploy-updates-to-microsoft-endpoint-configuration-manager-clients-with-update-management"></a>Implementación de actualizaciones en los clientes de Microsoft Endpoint Configuration Manager con Update Management

Los clientes que han invertido en Microsoft Endpoint Configuration Manager para administrar equipos, servidores y dispositivos móviles también dependen de su solidez y nivel de madurez para administrar actualizaciones de software como parte de su ciclo de Administración de actualizaciones de software (SUM).

Puede crear informes sobre los servidores Windows administrados y actualizarlos. Para ello, cree y preconfigure implementaciones de actualizaciones de software en Configuration Manager y obtenga el estado detallado de las implementaciones de actualizaciones finalizadas mediante [Update Management](automation-update-management.md). Si utiliza Configuration Manager para los informes de cumplimiento de las actualizaciones, pero no para administrar implementaciones de actualizaciones con los servidores de Windows, puede seguir enviando notificaciones a Configuration Manager mientras se administran las actualizaciones de seguridad con Update Management.

## <a name="prerequisites"></a>Prerrequisitos

* Debe tener [Update Management](automation-update-management.md) agregada a su cuenta de Automation.
* Los servidores de Windows que actualmente administra su entorno de Configuration Manager también deben informar al área de trabajo de Log Analytics que también tiene Update Management habilitada.
* Esta característica se habilita en la rama actual de Configuration Manager, versión 1606 y posteriores. Para integrar el sitio de administración central de Configuration Manager o un sitio primario independiente con registros de Azure Monitor e importar colecciones, revise [Conectar Configuration Manager con Azure Monitor](../azure-monitor/platform/collect-sccm.md).  
* Los agentes de Windows deben estar configurados para comunicarse con un servidor de Windows Server Update Services (WSUS) o tener acceso a Microsoft Update si no reciben actualizaciones de seguridad de Configuration Manager.

La forma de administrar los clientes hospedados en IaaS de Azure con el entorno existente de Configuration Manager depende, principalmente, de la conexión que tiene entre centros de datos de Azure y su infraestructura. Esta conexión afecta a los cambios de diseño que pueda necesitar hacer en la infraestructura de Configuration Manager y a los costos derivados de los cambios necesarios. Para comprender qué consideraciones de planeación debe evaluar antes de continuar, revise [Configuration Manager en Azure - Preguntas más frecuentes](https://docs.microsoft.com/configmgr/core/understand/configuration-manager-on-azure#networking).

## <a name="configuration"></a>Configuración

### <a name="manage-software-updates-from-configuration-manager"></a>Administrar actualizaciones de software de Configuration Manager

Siga estos pasos si va a continuar administrando las implementaciones de actualizaciones desde Configuration Manager. Azure Automation se conecta a Configuration Manager para aplicar actualizaciones a los equipos cliente conectados al área de trabajo de Log Analytics. El contenido de la actualización está disponible en la caché del equipo cliente como si la implementación se administrara mediante Configuration Manager.

1. Cree una implementación de actualizaciones de software desde el sitio de nivel superior de la jerarquía de Configuration Manager siguiendo el proceso que se describe en [Implementación de actualizaciones de software](https://docs.microsoft.com/configmgr/sum/deploy-use/deploy-software-updates). La única opción que se debe configurar de forma distinta de una implementación estándar es la opción **No instalar actualizaciones de software** para controlar el comportamiento de descarga del paquete de implementación. Este comportamiento se administra en Update Management mediante la creación de una implementación de actualizaciones programada en el paso siguiente.

1. En Azure Automation, seleccione **Update Management**. Cree una nueva implementación según los pasos descritos en [Creación de una implementación de actualizaciones](automation-tutorial-update-management.md#schedule-an-update-deployment) y seleccione **Grupos importados** en la lista desplegable **Tipo** para seleccionar la colección adecuada de Configuration Manager. Tenga en cuenta los siguientes puntos importantes: a. Si una ventana de mantenimiento se define en la colección de dispositivos de Configuration Manager, los miembros de la colección respetan sus condiciones, en lugar de la configuración **Duración** definida en la implementación programada.
    b. Los miembros de la colección de destino deben tener una conexión a Internet (ya sea directamente, a través de un servidor proxy o a través de la puerta de enlace de Log Analytics).

Después de completar la implementación de actualizaciones mediante Azure Automation, los equipos de destino que son miembros del grupo de equipos seleccionado instalarán las actualizaciones a la hora programada desde la memoria caché del cliente local. También puede [ver el estado de implementación de actualizaciones](automation-tutorial-update-management.md#view-results-of-an-update-deployment) para supervisar los resultados de la implementación.

### <a name="manage-software-updates-from-azure-automation"></a>Administrar actualizaciones de software desde Azure Automation

Para administrar actualizaciones para VM Windows Server que son clientes de Configuration Manager, debe configurar la directiva de cliente para deshabilitar la característica Administración de actualizaciones de software para todos los clientes que Update Management administra. De forma predeterminada, la configuración de cliente está dirigida a todos los dispositivos de la jerarquía. Para más información sobre esta configuración de directiva y cómo configurarla, consulte [Cómo establecer la configuración de cliente en Configuration Manager](https://docs.microsoft.com/configmgr/core/clients/deploy/configure-client-settings).

Después de aplicar este cambio de configuración, cree una nueva implementación según los pasos descritos en [Creación de una implementación de actualizaciones](automation-tutorial-update-management.md#schedule-an-update-deployment) y seleccione **Grupos importados** en la lista desplegable **Tipo** para seleccionar la colección adecuada de Configuration Manager.

## <a name="next-steps"></a>Pasos siguientes

Cree una nueva implementación siguiendo los pasos descritos en [Creación de una implementación de actualización](automation-tutorial-update-management.md#schedule-an-update-deployment).