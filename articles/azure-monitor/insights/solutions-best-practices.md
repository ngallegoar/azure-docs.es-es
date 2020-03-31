---
title: Procedimientos recomendados para la solución de administración en Azure | Microsoft Docs
description: ''
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 04/27/2017
ms.openlocfilehash: 837fb87d73698961ec1550b122840563d1707f8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77663205"
---
# <a name="best-practices-for-creating-management-solutions-in-azure-preview"></a>Procedimientos recomendados para la creación de soluciones de administración en Azure (versión preliminar)
> [!NOTE]
> La versión de la documentación para crear soluciones de administración en Azure está actualmente en fase preliminar. Cualquier esquema descrito a continuación está sujeto a cambios.  

En este artículo se presentan procedimientos recomendados para [crear un archivo de solución de administración](solutions-solution-file.md) en Azure.  Esta información se actualizará a medida que se detecten procedimientos recomendados adicionales.

## <a name="data-sources"></a>Orígenes de datos
- Los orígenes de datos se pueden [configurar con una plantilla de Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md), pero no deben incluirse en un archivo de solución.  El motivo es que la configuración de orígenes de datos no es actualmente idempotente, lo que significa que la solución podría sobrescribir la configuración existente en el área de trabajo del usuario.<br><br>Por ejemplo, la solución podría requerir eventos de advertencia y error desde el registro de eventos de la aplicación.  Si se especifica esto como un origen de datos en la solución, corre el riesgo de quitar eventos de información si el usuario lo tuviera configurado en su área de trabajo.  Si incluyera todos los eventos, podría estar recopilando excesivos eventos de información en el área de trabajo del usuario.

- Si la solución requiere datos de uno de los orígenes de datos estándar, debe definirlo como requisito previo.  Especifique en la documentación que el cliente debe configurar el origen de datos por sí mismo.  
- Agregue un mensaje de [Comprobación del flujo de datos](../../azure-monitor/platform/view-designer-tiles.md) a todas las vistas de la solución para indicar al usuario cuáles son los orígenes de datos que deben configurarse para que se recopilen los datos necesarios.  Este mensaje se muestra en el icono de la vista cuando no se encuentran los datos necesarios.


## <a name="runbooks"></a>Runbooks
- Agregue una [programación de Automation](../../automation/automation-schedules.md) para cada runbook de la solución que necesite ejecutar según una programación.
- Incluya el [módulo IngestionAPI](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) en la solución para que lo usen los runbooks que escriben datos en el repositorio de Log Analytics.  Configure la solución para que [haga referencia](solutions-solution-file.md#solution-resource) a este recurso de manera que permanezca si se quita la solución.  Esto permite que varias soluciones compartan el módulo.
- Use [las variables de Automation](../../automation/automation-schedules.md) para proporcionar valores a la solución que los usuarios pueden cambiar más adelante.  Incluso si la solución se configura para contener la variable, su valor puede cambiarse.

## <a name="views"></a>Vistas
- Todas las soluciones deben incluir una sola vista que se muestra en el portal del usuario.  La vista puede contener varios [elementos de visualización](../../azure-monitor/platform/view-designer-parts.md) para ilustrar los distintos conjuntos de datos.
- Agregue un mensaje de [Comprobación del flujo de datos](../../azure-monitor/platform/view-designer-tiles.md) a todas las vistas de la solución para indicar al usuario cuáles son los orígenes de datos que deben configurarse para que se recopilen los datos necesarios.
- Configure la solución para que [contenga](solutions-solution-file.md#solution-resource) la vista de manera que se quite si la solución se quita.

## <a name="alerts"></a>Alertas
- Defina la lista de destinatarios como un parámetro en el archivo de solución de manera que el usuario pueda definirlos al instalar la solución.
- Configure la solución para que [haga referencia](solutions-solution-file.md#solution-resource) a reglas de alerta de manera que el usuario pueda cambiar su configuración.  Es posible que desee realizar cambios, como modificar la lista de destinatarios, cambiar el umbral de la alerta o deshabilitar la regla de alerta. 


## <a name="next-steps"></a>Pasos siguientes
* Recorra el proceso básico de [diseño y creación de una solución de administración](solutions-creating.md).
* Aprenda a [crear un archivo de solución](solutions-solution-file.md).
* [Incorporación de búsquedas y alertas guardadas](solutions-resources-searches-alerts.md) a la solución de administración.
* [Incorporación de vistas](solutions-resources-views.md) a la solución de administración.
* [Incorporación de runbooks de Automation y otros recursos](solutions-resources-automation.md) a la solución de administración.

