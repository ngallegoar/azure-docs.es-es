---
title: 'Tutorial: Introducción a la integración con las canalizaciones'
description: En este tutorial, aprenderá a integrar las canalizaciones y las actividades mediante Synapse Studio.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: pipeline
ms.topic: tutorial
ms.date: 10/27/2020
ms.openlocfilehash: af01d5b5e424dd5ea229115f7aa3570d0b7cd511
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92744930"
---
# <a name="integrate-with-pipelines"></a>Integración con las canalizaciones

En este tutorial, aprenderá a integrar las canalizaciones y las actividades mediante Synapse Studio. 

## <a name="overview"></a>Información general

En Azure Synapse, puede organizar una gran variedad de tareas.

1. En Synapse Studio, vaya al centro **Integrate** (Integración).
1. Seleccione **+**  > **Canalización** para crear una canalización.
1. Vaya al centro de conectividad **Develop** (Desarrollo) y seleccione uno de los cuadernos que creó anteriormente.
1. Arrastre el cuaderno a la canalización ( **Nota** : Agregue el paso de los módulos de importación en el cuaderno, tal y como se especifica en el [documento](https://docs.microsoft.com/azure/synapse-analytics/spark/synapse-spark-sql-pool-import-export#transfer-data-to-or-from-a-sql-pool-attached-with-the-workspace). Estos módulos son necesarios durante la ejecución desde la canalización).
1. En la canalización, seleccione **Agregar desencadenador** > **Nuevo/editar**.
1. En **Choose trigger** (Elegir desencadenador), seleccione **New** (Nuevo) y establezca el valor de **Recurrence** (Periodicidad) en "cada 1 hora".
1. Seleccione **Aceptar**. 
1. Seleccione **Publish All** (Publicar todo).
1. Para que la canalización se ejecute inmediatamente sin esperar a la próxima hora, seleccione **Add trigger** > **Trigger now** (Agregar desencadenador > Desencadenar ahora).



## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Visualización de datos con Power BI](get-started-visualize-power-bi.md)
                                 
