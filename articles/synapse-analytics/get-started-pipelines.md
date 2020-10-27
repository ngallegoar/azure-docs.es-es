---
title: 'Tutorial: Introducción a la organización con canalizaciones'
description: En este tutorial, aprenderá a organizar canalizaciones y actividades mediante Synapse Studio.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: pipeline
ms.topic: tutorial
ms.date: 10/16/2020
ms.openlocfilehash: 42d2ac6cf6592f8e22b0a66aee84c3436d466572
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92329890"
---
# <a name="orchestrate-with-pipelines"></a>Organización con canalizaciones

En este tutorial, aprenderá a organizar canalizaciones y actividades mediante Synapse Studio. 

## <a name="overview"></a>Información general

Puede organizar una gran variedad de tareas en Azure Synapse.

1. En Synapse Studio, vaya al centro **Integrate** (Integración).
1. Seleccione **+**  > **Canalización** para crear una canalización.
1. Vaya al centro de conectividad **Develop** (Desarrollo) y seleccione uno de los cuadernos que creó anteriormente.
1. Arrastre el cuaderno a la canalización ( **Nota** : Agregue el paso de módulos de importación en el cuaderno tal y como se especifica en este [documento](https://docs.microsoft.com/azure/synapse-analytics/spark/synapse-spark-sql-pool-import-export#transfer-data-to-or-from-a-sql-pool-attached-with-the-workspace), son necesarios durante la ejecución desde la canalización).
1. En la canalización, seleccione **Agregar desencadenador** > **Nuevo/editar** .
1. En **Choose trigger** (Elegir desencadenador), seleccione **New** (Nuevo) y establezca el valor de **Recurrence** (Periodicidad) en "cada 1 hora".
1. Seleccione **Aceptar** . 
1. Seleccione **Publish All** (Publicar todo).
1. Para que la canalización se ejecute inmediatamente sin esperar a la próxima hora, seleccione **Add trigger** > **Trigger now** (Agregar desencadenador > Desencadenar ahora).



## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Visualización de datos con Power BI](get-started-visualize-power-bi.md)
                                 
