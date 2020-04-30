---
title: Usuarios de administración de Azure Data Box Gateway | Microsoft Docs
description: Describe cómo usar Azure Portal para administrar usuarios en Azure Data Box Gateway.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/25/2019
ms.author: alkohli
ms.openlocfilehash: 137531ab0d96c24a3ce19e429a5765d6e4a13c93
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82561258"
---
# <a name="use-the-azure-portal-to-manage-users-on-your-azure-data-box-gateway"></a>Uso de Azure Portal para administrar usuarios en Azure Data Box Gateway

En este artículo se describe cómo administrar usuarios en Azure Data Box Gateway. Azure Data Box Gateway se puede administrar a través de Azure Portal o de la interfaz de usuario web local. Usar Azure Portal para agregar, modificar o eliminar usuarios. 

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

1. En Azure Portal, vaya al recurso Data Box Gateway y, después, a **Información general**. Haga clic en **+ Agregar usuario** en la barra de comandos.

    ![Hacer clic en agregar usuario](media/data-box-gateway-manage-users/add-user-1.png)

2. Especifique el nombre de usuario y la contraseña del usuario que desea agregar. Confirme la contraseña y haga clic en **Agregar**.

    ![Hacer clic en agregar usuario](media/data-box-gateway-manage-users/add-user-2.png)

    > [!IMPORTANT] 
    > El sistema reserva a estos usuarios y no se deben usar: Administrator, EdgeUser, EdgeSupport, HcsSetupUser, WDAGUtilityAccount, CLIUSR, DefaultAccount, Guest.  

3. Recibirá una notificación tanto cuando comience la creación del usuario como cuando se complete. Una vez que se crea el usuario, en la barra de comandos, haga clic en **Actualizar** para ver la lista actualizada de usuarios.


## <a name="modify-user"></a>Modificación de un usuario

Una vez que se crea un usuario es posible cambiar la contraseña asociada con él. Seleccione y haga clic en la lista de usuarios. Indique y confirme la nueva contraseña. Guarde los cambios.
 
![Modificación de un usuario](media/data-box-gateway-manage-users/modify-user-1.png)


## <a name="delete-a-user"></a>Eliminar un usuario

Siga estos pasos en Azure Portal para eliminar un usuario.

1. Seleccione un usuario en la lista de usuarios, haga clic en él y, después, en **Eliminar**.  

   ![Eliminar un usuario](media/data-box-gateway-manage-users/delete-user-1.png)

2. Cuando se le solicite, confirme la eliminación. 

   ![Eliminar un usuario](media/data-box-gateway-manage-users/delete-user-2.png)

La lista de usuarios se actualiza para reflejar el usuario eliminado.

![Eliminar un usuario](media/data-box-gateway-manage-users/delete-user-3.png)


## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [administrar el ancho de banda](data-box-gateway-manage-bandwidth-schedules.md).
