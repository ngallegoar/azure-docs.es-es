---
title: Cómo corregir manualmente los problemas de sincronización de ServiceNow
description: Restablecimiento de la conexión a ServiceNow para que las alertas en Microsoft Azure puedan volver a llamar a ServiceNow
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 04/12/2020
ms.openlocfilehash: 2a9a8f3fe7422468ff2c8886dff7415322cf70f0
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92676872"
---
# <a name="how-to-manually-fix-servicenow-sync-problems"></a>Cómo corregir manualmente los problemas de sincronización de ServiceNow

Azure Monitor puede conectarse a otros proveedores de Administración de servicios de TI (ITSM) de terceros. ServiceNow es uno de estos proveedores.

Por motivos de seguridad, puede que tenga que actualizar el token de autenticación que se usa para la conexión con ServiceNow.
Use el proceso de sincronización siguiente para volver a activar la conexión y actualizar el token:


1. Busque la solución en el banner de búsqueda superior y, luego, seleccione las soluciones apropiadas.

    ![Captura de pantalla que muestra el banner de búsqueda principal y dónde seleccionar las soluciones pertinentes.](media/itsmc-resync-servicenow/solution-search-8bit.png)

1. En la pantalla de la solución, elija "Seleccionar todo" en el filtro de suscripción y, a continuación, filtre por "ServiceDesk".

    ![Captura de pantalla que muestra dónde elegir "Seleccionar todo" y dónde filtrar según ServiceDesk.](media/itsmc-resync-servicenow/solutions-list-8bit.png)

1. Seleccione la solución de la conexión de ITSM.
1. Seleccione la conexión de ITSM en el banner izquierdo.

    ![Captura de pantalla que muestra dónde seleccionar las conexiones de ITSM.](media/itsmc-resync-servicenow/itsm-connector-8bit.png)

1. Seleccione cada conector en la lista. 
    1. Haga clic en el nombre del conector para configurarlo.
    1. Elimine los conectores que ya no se estén usando.

    1. Actualice los campos de acuerdo con [estas definiciones](./itsmc-connections.md) según el tipo de asociado.

    1. Haga clic en Sincronizar

       ![Captura de pantalla que resalta el botón Sincronizar.](media/itsmc-resync-servicenow/resync-8bit2.png)

    1. Haga clic en Guardar

        ![Nueva conexión](media/itsmc-resync-servicenow/save-8bit.png)

f.    Revise las notificaciones para comprobar si el proceso finalizó correctamente 

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las [Conexiones IT Service Management](itsmc-connections.md)
