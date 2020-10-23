---
title: Administración de estados de alertas y de grupos inteligentes
description: Administración de los estados de las instancias de alertas y de grupos inteligentes
ms.subservice: alerts
ms.topic: conceptual
author: anantr
ms.date: 09/24/2018
ms.openlocfilehash: b91c3d59a5a37457d67fdfbc646334fccb331df5
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92104808"
---
# <a name="manage-alert-and-smart-group-states"></a>Administración de estados de alertas y de grupos inteligentes

Las alertas en Azure Monitor ahora tienen un [estado de alerta y una condición de supervisión](./alerts-overview.md) y, de forma similar, los grupos inteligentes tienen un [estado de grupo inteligente](./alerts-smartgroups-overview.md?toc=%252fazure%252fazure-monitor%252ftoc.json). Los cambios en el estado ahora se registran en el historial que está asociado a la alerta o al grupo inteligente. Este artículo le guiará a través del proceso de cambiar el estado de las alertas y los grupos inteligentes.

## <a name="change-the-state-of-an-alert"></a>Cambio en el estado de una alerta

1. Puede cambiar el estado de una alerta de las siguientes formas: 
    * En la página Todas las alertas, haga clic en la casilla de verificación situada junto a las alertas cuyo estado desea cambiar y, después, en Cambiar estado.   
    ![Captura de pantalla que muestra la página Todas las alertas con la opción Cambiar estado seleccionada.](./media/alerts-managing-alert-states/state-all-alerts.jpg)
    * Puede hacer clic en Cambiar estado desde la página Detalles de la alerta de una instancia de alerta específica   
    ![Captura de pantalla que muestra la página Detalles de alerta con la opción Cambiar el estado de alerta seleccionada.](./media/alerts-managing-alert-states/state-alert-details.jpg)
    * En la página Detalles de la alerta de una instancia de alerta específica, dentro del panel Grupo inteligente, puede hacer clic en la casilla de verificación junto a las alertas que quiera    
    ![Captura de pantalla que muestra la página Detalles de la alerta de la alerta de latido con algunas instancias que tienen marcas de verificación.](./media/alerts-managing-alert-states/state-alert-details-sg.jpg)

    * En la página Detalles del grupo inteligente, puede marcar la casilla al lado de las alertas que aparecen en la lista de miembros si desea cambiar su estado y, a continuación, puede hacer clic en Cambiar estado.   
    ![Captura de pantalla que muestra la página Detalles del grupo inteligente, donde puede seleccionar las alertas para las que desea cambiar el estado.](./media/alerts-managing-alert-states/state-sg-details-alerts.jpg)
1. Al hacer clic en Cambiar estado, se abre un menú emergente que permite seleccionar el estado (Nuevo, Confirmado o Cerrado) y escribir un comentario si es necesario.   
![Captura de pantalla que muestra el cuadro de diálogo Detalles con Cambiar el estado de alerta.](./media/alerts-managing-alert-states/state-alert-change.jpg)
1. Una vez hecho esto, el cambio de estado se registra en el historial de la alerta correspondiente. Puede comprobar que se haya registrado si abre la página de detalles correspondiente y observa la sección de historial.    
![Captura de pantalla que muestra el historial de cambios de estado.](./media/alerts-managing-alert-states/state-alert-history.jpg)

## <a name="change-the-state-of-a-smart-group"></a>Cambios en el estado del grupo inteligente
1. Puede cambiar el estado de un grupo inteligente de las siguientes formas:
    1. En la página con la lista de grupos inteligentes, puede hacer clic en la casilla de verificación situada junto a los grupos inteligentes cuyo estado desea cambiar y, después, en Cambiar estado  
    ![Captura de pantalla que muestra la página Cambiar estado para grupos inteligentes.](./media/alerts-managing-alert-states/state-sg-list.jpg)
    1. En la página de detalles del grupo inteligente, puede hacer clic en Cambiar estado        
    ![Captura de pantalla que muestra la página de detalles del grupo inteligente con Cambiar estado del grupo inteligente seleccionado.](./media/alerts-managing-alert-states/state-sg-details.jpg)
1. Al hacer clic en Cambiar estado, se abre un menú emergente que permite seleccionar el estado (Nuevo, Confirmado o Cerrado) y escribir un comentario si es necesario. 
![Captura de pantalla que muestra el cuadro de diálogo Cambiar estado para el grupo inteligente.](./media/alerts-managing-alert-states/state-sg-change.jpg)
   > [!NOTE]
   >  El cambio de estado de un grupo inteligente no modifica el estado de las alertas individuales que lo componen.

1. Una vez hecho esto, el cambio de estado se registra en el historial del grupo inteligente correspondiente. Puede comprobar que se haya registrado si abre la página de detalles correspondiente y observa la sección de historial.     
![Captura de pantalla que muestra el historial de cambios del grupo inteligente.](./media/alerts-managing-alert-states/state-sg-history.jpg)