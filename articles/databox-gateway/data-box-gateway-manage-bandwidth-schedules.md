---
title: Administración de programaciones del ancho de banda en Azure Data Box Gateway | Microsoft Docs
description: Describe cómo usar Azure Portal para administrar las programaciones del ancho de banda en Azure Data Box Gateway.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: how-to
ms.date: 10/14/2020
ms.author: alkohli
ms.openlocfilehash: 55676128f482c8fb4dbdeab0523e4556f3957af7
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96581159"
---
# <a name="use-the-azure-portal-to-manage-bandwidth-schedules-on-your-azure-data-box-gateway"></a>Uso de Azure Portal para administrar las programaciones del ancho de banda en Azure Data Box Gateway  

En este artículo se describe cómo administrar usuarios en Azure Data Box Gateway. Las programaciones del ancho de banda le permiten configurar el uso del ancho de banda de red en programaciones en varias horas del día. Dichas programaciones se pueden aplicar a las operaciones de carga y descarga desde el dispositivo a la nube.

Puede agregar, modificar o eliminar las programaciones del ancho de banda de Data Box Gateway a través de Azure Portal.

En este artículo aprenderá a:

> [!div class="checklist"]
>
> * Adición de una programación
> * Modificar una programación
> * Eliminación de una programación

## <a name="add-a-schedule"></a>Adición de una programación

Para agregar un usuario siga estos pasos en Azure Portal.

1. En la instancia de Azure Portal para el recurso de Data Box Gateway, vaya a **Ancho de banda**.
2. En el panel derecho, haga clic en **+ Agregar programación**.

    ![Hacer clic en agregar usuario](media/data-box-gateway-manage-bandwidth-schedules/add-schedule-1.png)

3. En **Agregar programación**: 

   1. Especifique los valores de **Día de inicio**, **Día de finalización**, **Hora de inicio** y **Hora de finalización** de la programación. 
   2. Si esta programación se ejecuta todo el día, puede seleccionar la opción **Todo el día**. 
   3. **Velocidad de ancho de banda** es el ancho de banda, en Megabits por segundo (Mbps), que utiliza el dispositivo en operaciones que afectan a la nube (tanto cargas como descargas). Proporcione un número entre 1 y 1.000 para este campo. 
   4. Seleccione el ancho de banda **Ilimitado** si no desea regular la carga y la descarga de la fecha. 
   5. Haga clic en **Agregar**.

      ![Hacer clic en agregar usuario 2](media/data-box-gateway-manage-bandwidth-schedules/add-schedule-2.png)

3. Se crea una programación con los parámetros especificados. Esta programación se muestra en la lista de programaciones de ancho de banda del portal.


## <a name="edit-schedule"></a>Edición de programación

Siga estos pasos para editar una programación del ancho de banda. 

1. En Azure Portal, vaya al recurso Data Box Gateway y, después, a Ancho de banda. 
2. En la lista de programaciones de ancho de banda, seleccione y haga clic en la programación que desee modificar.

    ![Modificación de un usuario](media/data-box-gateway-manage-bandwidth-schedules/modify-schedule-1.png)

3. Realice los cambios deseados y guárdelos.

    ![Modificación de un usuario 2](media/data-box-gateway-manage-bandwidth-schedules/modify-schedule-2.png)

4. Después de que se modifique la programación, se actualiza la lista de programaciones para reflejar la programación modificada.

    ![Modificación de un usuario 3](media/data-box-gateway-manage-bandwidth-schedules/modify-schedule-3.png)


## <a name="delete-a-schedule"></a>Eliminación de una programación

Siga estos pasos para eliminar una programación de ancho de banda asociada a un dispositivo de Data Box Gateway.

1. En Azure Portal, vaya al recurso Data Box Gateway y, después, a **Ancho de banda**.  

2. En la lista de programaciones de ancho de banda, seleccione la programación que desea eliminar. Haga clic con el botón derecho para mostrar el menú contextual y seleccione **Eliminar**. 

   ![Eliminar un usuario](media/data-box-gateway-manage-bandwidth-schedules/delete-schedule-1.png)

3.  Después de que se elimina la programación se actualiza la lista de programaciones.



## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [administrar el ancho de banda](data-box-gateway-manage-bandwidth-schedules.md). 
