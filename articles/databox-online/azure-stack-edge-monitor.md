---
title: Supervisión del dispositivo Azure Stack Edge | Microsoft Docs
description: Describe cómo usar Azure Portal y la interfaz de usuario web local para supervisar Azure Stack Edge.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: 2fc7435988a07968e65aaf265c33878c6e72e85b
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82568645"
---
# <a name="monitor-your-azure-stack-edge"></a>Supervisión de Azure Stack Edge

En este artículo se describe cómo supervisar Azure Stack Edge. Para supervisar el dispositivo, puede usar Azure Portal o la interfaz de usuario web local. Use Azure Portal para ver los eventos de dispositivo, configurar y administrar las alertas, y ver las métricas. Use la interfaz de usuario web local en el dispositivo físico para ver el estado del hardware de los distintos componentes del dispositivo.

En este artículo aprenderá a:

> [!div class="checklist"]
> * Ver los eventos de dispositivo y las alertas correspondientes
> * Ver el estado del hardware de los componentes del dispositivo
> * Ver las métricas de transacción y de capacidad del dispositivo
> * Configurar y administrar las alertas

## <a name="view-device-events"></a>Ver los eventos de dispositivo

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-view-device-events.md)]

## <a name="view-hardware-status"></a>Visualización del estado del hardware

Realice los pasos siguientes en la interfaz de usuario web local para ver el estado del hardware de los componentes del dispositivo.

1. Conéctese a la interfaz de usuario web local del dispositivo.
2. Vaya a **Mantenimiento > Estado de hardware**. Puede ver el estado de los distintos componentes del dispositivo. 

    ![Visualización del estado del hardware](media/azure-stack-edge-monitor/view-hardware-status.png)

## <a name="view-metrics"></a>Visualización de métricas

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-view-metrics.md)]

## <a name="manage-alerts"></a>Administrar alertas

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-manage-alerts.md)]

## <a name="next-steps"></a>Pasos siguientes 

Aprenda a [administrar el ancho de banda](azure-stack-edge-manage-bandwidth-schedules.md).