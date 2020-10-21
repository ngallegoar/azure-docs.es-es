---
title: Análisis de cambios en Azure Monitor para VM
description: La integración de Azure Monitor para VM con la integración de Application Change Analysis permite ver los cambios realizados en una máquina virtual que pueden haber afectado a su rendimiento.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/23/2020
ms.openlocfilehash: 59799a09436d5968a441f6f17655d3138a2d84d8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91711105"
---
# <a name="change-analysis-in-azure-monitor-for-vms"></a>Análisis de cambios en Azure Monitor para VM
La integración de Azure Monitor para VM con la integración de [Application Change Analysis](../app/change-analysis.md) permite ver los cambios realizados en una máquina virtual que pueden haber afectado a su rendimiento.

## <a name="overview"></a>Introducción
Supongamos que tiene una máquina virtual que empieza a ejecutarse lentamente y desea investigar si los cambios recientes en su configuración podrían haber afectado a su rendimiento. El rendimiento de la máquina virtual se visualiza mediante Azure Monitor para VM y se detecta un aumento del uso de memoria en la última hora. El análisis de cambios puede ayudarle a determinar si los cambios de configuración realizados en torno a este momento fueron la causa de este aumento.

El servicio Application Change Analysis agrega cambios respecto a [Azure Resource Graph](../../governance/resource-graph/how-to/get-resource-changes.md), así como cambios de propiedades animadas como las reglas de seguridad de red de Azure Resource Manager. 

## <a name="enabling-change-analysis"></a>Habilitación de análisis de cambios
Para incorporar análisis de cambios en Azure Monitor para VM, debe registrar el proveedor de recursos *Microsoft.ChangeAnalysis*. La primera vez que inicie Azure Monitor para VM o Application Change Analysis en Azure Portal, este proveedor de recursos se registrará automáticamente. Application Change Analysis es un servicio gratuito que no tiene ninguna sobrecarga de rendimiento en los recursos.

## <a name="view-change-analysis"></a>Consulta de análisis de cambios
El análisis de cambios está disponible en la pestaña **Rendimiento** o **Asignación** de Azure Monitor para VM seleccionando la opción **Cambiar**. 

[![Investigación de cambios](media/vminsights-change-analysis/investigate-changes-screenshot.png)](media/vminsights-change-analysis/investigate-changes-screenshot-zoom.png#lightbox)


Haga clic en el botón **Investigate changes** (Investigar los cambios) para iniciar la página Application Change Analysis filtrada para la máquina virtual. Puede revisar los cambios mostrados para ver si hay alguno que pueda haber causado el problema. Si no está seguro de un cambio concreto, puede hacer referencia a la columna **Change by** (Cambiar por) para determinar la persona que realizó el cambio.

[![Cambiar detalles](media/vminsights-change-analysis/change-details-screenshot.png)](media/vminsights-change-analysis/change-details-screenshot.png#lightbox)

## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre [Application Change Analysis](../app/change-analysis.md).
- Obtenga más información sobre [Azure Resource Graph](../../governance/resource-graph/how-to/get-resource-changes.md). 

