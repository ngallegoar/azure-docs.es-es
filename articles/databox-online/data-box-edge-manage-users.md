---
title: Usuarios de administración de Azure Data Box Edge | Microsoft Docs
description: Describe cómo usar Azure Portal para administrar usuarios en Azure Data Box Edge.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/11/2019
ms.author: alkohli
ms.openlocfilehash: 443ed983a0eec5dfd8f7a917fbc1440cd66c3db3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78946140"
---
# <a name="use-the-azure-portal-to-manage-users-on-your-azure-data-box-edge"></a>Uso de Azure Portal para administrar usuarios en Azure Data Box Edge

En este artículo se describe cómo administrar usuarios en Azure Data Box Edge. Azure Data Box Edge se puede administrar a través de Azure Portal o de la interfaz de usuario web local. Usar Azure Portal para agregar, modificar o eliminar usuarios.

En este artículo aprenderá a:

> [!div class="checklist"]
> * Adición de un usuario
> * Modificación de un usuario
> * Eliminar un usuario

## <a name="about-users"></a>Acerca de los usuarios

Los usuarios pueden ser de solo lectura o tener privilegios totales. Como indican los nombres, los usuarios de solo lectura solo pueden ver los datos de los recursos compartidos. Los usuarios con privilegios totales pueden leer datos de los recursos compartidos, escribir en estos recursos compartidos y modificar o eliminar los datos de los recursos compartidos.

 - **Usuario con privilegios totales**: un usuario local con acceso completo.
 - **Usuario de solo lectura**: un usuario local con acceso de solo lectura. Estos usuarios están asociados a los recursos compartidos que permiten operaciones de solo lectura.

Los permisos del usuario se definen por primera vez cuando se crea el usuario durante la creación de los recursos compartidos. Actualmente no se admite la modificación de permisos en el nivel de recurso compartido.

## <a name="add-a-user"></a>Adición de un usuario

Para agregar un usuario siga estos pasos en Azure Portal.

1. En Azure Portal, vaya al recurso de Data Box Edge y luego a **Overview > Users** (Información general > Usuarios). Seleccione **+ Agregar usuario** en la barra de comandos.

    ![Seleccionar Agregar usuario](media/data-box-edge-manage-users/add-user-1.png)

2. Especifique el nombre de usuario y la contraseña del usuario que desea agregar. Configure la contraseña y seleccione **Agregar**.

    ![Especificación de nombre de usuario y contraseña](media/data-box-edge-manage-users/add-user-2.png)

    > [!IMPORTANT] 
    > El sistema reserva a estos usuarios y no se deben usar: Administrator, EdgeUser, EdgeSupport, HcsSetupUser, WDAGUtilityAccount, CLIUSR, DefaultAccount, Guest.  

3. Aparece una notificación cuando se inicia y se completa la creación del usuario. Una vez que se crea el usuario, en la barra de comandos, seleccione **Actualizar** para ver la lista actualizada de usuarios.


## <a name="modify-user"></a>Modificación de un usuario

Una vez que se crea un usuario es posible cambiar la contraseña asociada con él. Seleccione en la lista de usuarios. Escriba la contraseña nueva y confírmela. Guarde los cambios.
 
![Modificación de un usuario](media/data-box-edge-manage-users/modify-user-1.png)


## <a name="delete-a-user"></a>Eliminar un usuario

Siga estos pasos en Azure Portal para eliminar un usuario.


1. En Azure Portal, vaya al recurso de Data Box Edge y luego a **Overview > Users** (Información general > Usuarios).

    ![Seleccionar el usuario que se va a eliminar](media/data-box-edge-manage-users/delete-user-1.png)

2. Seleccione un usuario en la lista de usuarios y, luego, seleccione **Eliminar**.  

   ![Seleccionar Eliminar](media/data-box-edge-manage-users/delete-user-2.png)

3. Cuando se le solicite, confirme la eliminación. 

   ![Confirmar eliminación](media/data-box-edge-manage-users/delete-user-3.png)

La lista de usuarios se actualiza para reflejar el usuario eliminado.

![Lista de usuarios actualizada](media/data-box-edge-manage-users/delete-user-4.png)


## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [administrar el ancho de banda](data-box-edge-manage-bandwidth-schedules.md).
