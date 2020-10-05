---
title: Cómo agregar otros propietarios a un laboratorio en Azure Lab Services
description: En este artículo se muestra cómo un administrador puede agregar un usuario como propietario de un laboratorio en Azure Lab Services.
ms.topic: article
ms.date: 09/04/2020
ms.openlocfilehash: 4b46dd08a3b9527cdfc7bdf766ee4976e67656d0
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2020
ms.locfileid: "89482676"
---
# <a name="how-to-add-additional-owners-to-an-existing-lab-in-azure-lab-services"></a>Cómo agregar otros propietarios a un laboratorio existente en Azure Lab Services
En este artículo se muestra cómo, como administrador, puede agregar otros propietarios a un laboratorio existente.

## <a name="add-user-to-the-reader-role-for-the-lab-account"></a>Adición de un usuario al rol de lector de la cuenta de laboratorio
Para agregar un usuario como propietario adicional a un laboratorio existente, primero debe conceder al usuario permisos de **lectura** en la cuenta de laboratorio.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** en el menú de la izquierda. Busque **Lab Services** y selecciónelo.
3. Seleccione la **cuenta de laboratorio** en la lista. 
2. En la **página Cuenta de laboratorio**, seleccione **Control de acceso (IAM)** en el menú izquierdo. 
2. En la página **Control de acceso (IAM)** , seleccione **Agregar** en la barra de herramientas y, a continuación, **Agregar asignación de roles**.

    ![Asignación de roles para la cuenta de laboratorio ](./media/how-to-add-user-lab-owner/lab-account-access-control-page.png)
3. En la página **Agregar una asignación de roles**, siga estos pasos: 
    1. Seleccione **Lector** para el **rol**. 
    2. Seleccione el usuario. 
    3. Seleccione **Guardar**. 

        ![Adición de un usuario al rol de lector de la cuenta de laboratorio ](./media/how-to-add-user-lab-owner/reader-lab-account.png)

## <a name="add-user-to-the-owner-role-for-the-lab"></a>Adición de un usuario al rol de propietario del laboratorio

> [!NOTE]
> Si el usuario solo tiene acceso de lector en el laboratorio, el laboratorio no se muestra en labs.azure.com.

1. En la página **Cuenta de laboratorio**, seleccione **Todos los laboratorios** en el menú de la izquierda.
2. Seleccione el **laboratorio** al que quiere agregar el usuario como propietario. 
    
    ![Selección del laboratorio ](./media/how-to-add-user-lab-owner/select-lab.png)    
3. En la página **Laboratorio**, seleccione **Control de acceso (IAM)** en el menú de la izquierda.
4. En la página **Control de acceso (IAM)**, seleccione **Agregar** en la barra de herramientas y, a continuación, **Agregar asignación de roles**.
5. En la página **Agregar una asignación de roles**, siga estos pasos: 
    1. Seleccione **Propietario** para el **rol**. 
    2. Seleccione el usuario. 
    3. Seleccione **Guardar**. 

## <a name="next-steps"></a>Pasos siguientes
Confirme que el usuario ve el laboratorio al iniciar sesión en el [portal de Lab Services](https://labs.azure.com).
