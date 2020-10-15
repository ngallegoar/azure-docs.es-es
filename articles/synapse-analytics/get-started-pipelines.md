---
title: 'Tutorial: Introducción a la organización con canalizaciones'
description: En este tutorial, aprenderá a organizar canalizaciones y actividades mediante Synapse Studio.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 72eea7c46dd005cd16ae5b8f0022c1174dd28f27
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "89667474"
---
# <a name="orchestrate-with-pipelines"></a>Organización con canalizaciones

En este tutorial, aprenderá a organizar canalizaciones y actividades mediante Synapse Studio. 

## <a name="overview"></a>Información general

Puede organizar una gran variedad de tareas en Azure Synapse.

1. En Synapse Studio, vaya al centro **Orchestrate** (Organización).
1. Seleccione **+**  > **Canalización** para crear una canalización.
1. Vaya al centro de conectividad **Develop** (Desarrollo) y seleccione uno de los cuadernos que creó anteriormente.
1. Arrastre el cuaderno a la canalización.
1. En la canalización, seleccione **Agregar desencadenador** > **Nuevo/editar**.
1. En **Choose trigger** (Elegir desencadenador), seleccione **Nuevo** y, después, en el apartado en que se elige la **periodicidad**, configure el desencadenador para que se ejecute cada hora.
1. Seleccione **Aceptar**.
1. Seleccione **Publish All** (Publicar todo). La canalización se ejecuta cada hora.
1. Para que la canalización se ejecute ahora sin esperar a la próxima hora, seleccione **Agregar desencadenador** > **Nuevo/editar**.



## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Visualización de datos con Power BI](get-started-visualize-power-bi.md)
                                 
