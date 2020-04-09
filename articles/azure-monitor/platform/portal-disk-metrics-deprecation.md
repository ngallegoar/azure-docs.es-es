---
title: Desuso de las métricas de disco en Azure Portal | Microsoft Docs
description: Obtenga información sobre qué métricas de disco han quedado en desuso y cómo actualizar las alertas de métricas para usar nuevas métricas.
services: azure-monitor
ms.subservice: metrics
ms.topic: conceptual
author: albecker1
ms.author: albecker
ms.date: 03/12/2020
ms.openlocfilehash: f2b960c2198800e04da77ad6b5be78d7b4762354
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79300491"
---
# <a name="disk-metrics-deprecation-in-the-azure-portal"></a>Desuso de las métricas de disco en Azure Portal

Las métricas de disco en desuso se quitarán pronto de Azure Portal. Hay disponible una nueva versión de cada métrica en desuso. En este artículo se muestran las métricas nuevas y cómo actualizar las alertas de métricas para utilizarlas.

## <a name="list-of-new-metrics"></a>Lista de nuevas métricas

En esta tabla se asocia cada métrica en desuso con su nueva métrica correspondiente. 

|Métrica en desuso|Nueva métrica (de reemplazo)|
|----|----|
|Profundidad de la cola de disco de datos (en desuso)|Profundidad de cola de discos de datos (versión preliminar)|
|Bytes de lectura de discos de datos por segundo (en desuso)|Bytes de lectura de discos de datos por segundo (versión preliminar)|
|Operaciones de lectura de discos de datos por segundo (en desuso)|Operaciones de lectura de discos de datos por segundo (versión preliminar)|
|Bytes de escritura de discos de datos por segundo (en desuso)|Bytes de escritura de discos de datos por segundo (versión preliminar)|
|Operaciones de escritura de discos de datos por segundo (en desuso)|Operaciones de lectura de discos de datos por segundo (versión preliminar)|
|Profundidad de la cola de SO (en desuso)|Profundidad de cola de SO (versión preliminar)|
|Bytes de lectura de SO por segundo (en desuso)|Bytes de lectura de SO por segundo (versión preliminar)|
|Operaciones de lectura de SO por segundo (en desuso)|Operaciones de lectura de SO por segundo (versión preliminar)|
|Bytes de escritura de SO por segundo (en desuso)|Bytes de escritura de SO por segundo (versión preliminar)|
|Operaciones de escritura de SO por segundo (en desuso)|Operaciones de escritura de SO por segundo (versión preliminar)|

<a id="update-metrics" />

## <a name="migrate-metrics-in-your-metric-alerts"></a>Migración de métricas en las alertas de métricas

Actualice las alertas de métricas para usar nuevas métricas.

1. En Azure Portal, busque **Alertas**. A continuación, en la sección **Servicios**, seleccione **Alertas**.

   > [!div class="mx-imgBorder"]
   > ![Descripción de la imagen](./media/portal-disk-metrics-deprecation/alert-service-azure-portal.png)

2. En la página **Alertas**, seleccione el botón **Administrar reglas de alertas**. 

   > [!div class="mx-imgBorder"]
   > ![Descripción de la imagen](./media/portal-disk-metrics-deprecation/manage-alert-rules-button.png)

3. En la lista desplegable **Grupo de recursos**, active la casilla **Máquinas virtuales** y, en la lista desplegable **Tipo de señal**, active la casilla **Métricas**. 

   > [!div class="mx-imgBorder"]
   > ![Descripción de la imagen](./media/portal-disk-metrics-deprecation/filter-alerts.png)

4. En la lista de métricas, identifique condiciones relacionadas con los discos. Haga clic en el nombre de la regla. 

   El nombre se muestro como un hipervínculo en la columna **Nombre** de la tabla.

   > [!div class="mx-imgBorder"]
   > ![Descripción de la imagen](./media/portal-disk-metrics-deprecation/find-disk-conditions.png)

5. En la sección **Condiciones** de la página **Administración de reglas**, haga clic en la condición de la alerta. 

   La condición se muestra como un hipervínculo.  

   > [!div class="mx-imgBorder"]
   > ![Descripción de la imagen](./media/portal-disk-metrics-deprecation/adjust-condition.png)

   Aparecerá la página **Configurar lógica de señal**, en cuya sección **Lógica de alerta** se muestra la configuración de la condición.

6. Tome nota de esta configuración, ya que desaparecerá al quitar la métrica en desuso.

   > [!div class="mx-imgBorder"]
   > ![Descripción de la imagen](./media/portal-disk-metrics-deprecation/condition-rules.png)

   > [!TIP] 
   > Puede realizar una captura de esta configuración o guardarla en un archivo de texto. 

7. Haga clic en el vínculo **Volver a la selección de la señal**.

   > [!div class="mx-imgBorder"]
   > ![Descripción de la imagen](./media/portal-disk-metrics-deprecation/back-to-signal-selection.png)

8. En la página **Configurar lógica de señal**, seleccione la métrica de reemplazado adecuada (nueva métrica). Use la [tabla](#update-metrics) de este artículo para identificar el nombre de la nueva métrica.

   > [!TIP] 
   > Empiece a escribir en la barra de búsqueda para restringir la lista de nombres de métricas. 

   > [!div class="mx-imgBorder"]
   > ![Descripción de la imagen](./media/portal-disk-metrics-deprecation/choose-new-metric.png)

9. Haga clic en el botón **Listo**. 

   > [!div class="mx-imgBorder"]
   > ![Descripción de la imagen](./media/portal-disk-metrics-deprecation/set-new-metric.png)

10. Seleccione el botón **Guardar** para confirmar los cambios. 

    > [!div class="mx-imgBorder"]
    > ![Descripción de la imagen](./media/portal-disk-metrics-deprecation/save-new-metric.png)






