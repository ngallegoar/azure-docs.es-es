---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/06/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 57368331e83111836e13b6c3796cacad9816e318
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521280"
---
Para ver y copiar la cadena de conexión o las claves de acceso de la cuenta de almacenamiento desde Azure Portal:

1. Acceda a [Azure Portal](https://portal.azure.com).
2. Busque su cuenta de almacenamiento.
3. En **Configuración**, seleccione **Claves de acceso**. Aparecen las claves de acceso de la cuenta, así como la cadena de conexión completa de cada clave.
4. Busque el valor de **Clave** en **key1** y haga clic en el botón **Copiar** para copiar la clave de la cuenta.
5. Como alternativa, puede copiar la cadena de conexión completa. Busque el valor de **Cadena de conexión** en **key1**y haga clic en el botón **Copiar** para copiar la cadena de conexión.

    ![Captura de pantalla que muestra cómo ver las claves de acceso en Azure Portal](media/storage-view-keys-include/portal-connection-string.png)

Puede utilizar cualquiera de las dos claves para acceder a Azure Storage, pero en general es recomendable usar la primera clave y reservar el uso de la segunda clave para cuando se roten las claves.

Para ver o leer las claves de acceso de una cuenta, el usuario debe ser un administrador de servicios o debe tener asignado un rol de RBAC que incluya **Microsoft.Storage/storageAccounts/listkeys/action**. Algunos de los roles de RBAC integrados que incluyen esta acción son **Propietario**, **Colaborador** y **Rol de servicio del operador de claves de cuentas de almacenamiento**. Para obtener más información sobre el rol de administrador de servicios, consulte [Roles de administrador de suscripciones clásico de RBAC de Azure y de administrador de Azure AD](../articles/role-based-access-control/rbac-and-directory-admin-roles.md). Para obtener información detallada sobre los roles integrados de RBAC para Azure Storage, consulte la sección **Almacenamiento** en [Roles integrados de Azure para RBAC de Azure](../articles/role-based-access-control/built-in-roles.md#storage).
