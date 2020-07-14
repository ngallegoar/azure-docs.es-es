---
title: Configuración de las credenciales de implementación
description: Obtenga información acerca de los tipos de credenciales de implementación que se encuentran en Azure App Service y cómo configurarlas y usarlas.
ms.topic: article
ms.date: 08/14/2019
ms.reviewer: byvinyal
ms.custom: seodec18
ms.openlocfilehash: c6f7c2422e043da6df498fe81da938576687b916
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "83649128"
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>Configuración de credenciales de implementación para Azure App Service
[Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) admite dos tipos de credenciales para la [implementación de GIT local](deploy-local-git.md) y la [implementación FTP/S](deploy-ftp.md). Estas credenciales no son las mismas que las de su suscripción a Azure.

[!INCLUDE [app-service-deploy-credentials](../../includes/app-service-deploy-credentials.md)]

## <a name="configure-user-level-credentials"></a><a name="userscope"></a>Configuración de las credenciales de usuario

Puede configurar las credenciales de nivel de usuario en cualquier [página de recursos](../azure-resource-manager/management/manage-resources-portal.md#manage-resources) de la aplicación. Independientemente de en qué aplicación configure estas credenciales, son válidas para todas las aplicaciones y para todas las suscripciones de su cuenta de Azure. 

### <a name="in-the-cloud-shell"></a>En Cloud Shell

Para configurar el usuario de implementación en [Cloud Shell](https://shell.azure.com), ejecute el comando [az webapp deployment user set](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set). Reemplace \<username> y \<password> por un nombre de usuario y una contraseña de usuario de implementación. 

- El nombre de usuario debe ser único dentro de Azure y no debe contener el símbolo "\@" para las inserciones de Git local. 
- La contraseña debe tener al menos ocho caracteres y dos de los tres elementos siguientes: letras, números y símbolos. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

La salida JSON muestra la contraseña como `null`. Si se produce un error `'Conflict'. Details: 409`, cambie el nombre de usuario. Si se produce un error `'Bad Request'. Details: 400`, use una contraseña más segura. 

### <a name="in-the-portal"></a>En el portal

En Azure Portal, debe tener al menos una aplicación para poder acceder a la página de credenciales de implementación. Para configurar las credenciales de nivel de usuario:

1. En [Azure Portal](https://portal.azure.com), en el menú de la izquierda, seleccione **App Services** >  **\<any_app>**  > **Centro de implementación** > **FTP** > **Panel**.

    ![](./media/app-service-deployment-credentials/access-no-git.png)

    O bien, si ya ha configurado la implementación de Git, seleccione **App Services** >  **&lt;any_app>**  > **Centro de implementaciones** > **FTP/Credenciales**.

    ![](./media/app-service-deployment-credentials/access-with-git.png)

2. Seleccione **Credenciales de usuario**, configure el nombre de usuario y la contraseña, y luego haga clic en **Guardar credenciales**.

Una vez que haya configurado las credenciales de implementación, puede encontrar el nombre de usuario de implementación de *Git* en la página **Información general** de la aplicación.

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

Si se configura la implementación de Git, la página muestra un **nombre de usuario de implementación o Git**; de lo contrario, un **nombre de usuario de implementación o FTP**.

> [!NOTE]
> Azure no muestra la contraseña de la implementación de nivel de usuario. Si se le olvida la contraseña, podrá restablecer las credenciales siguiendo los pasos descritos en esta sección.
>
> 

## <a name="use-user-level-credentials-with-ftpftps"></a>Uso de credenciales de nivel de usuario con FTP/FTPS

La autenticación en un punto de conexión FTP o FTPS con credenciales de nivel de usuario requiere un nombre de usuario con el formato siguiente: `<app-name>\<user-name>`

Puesto que las credenciales de nivel de usuario están vinculadas al usuario y no a un recurso específico, el nombre de usuario debe tener este formato para dirigir la acción de inicio de sesión al punto de conexión de la aplicación adecuada.

## <a name="get-and-reset-app-level-credentials"></a><a name="appscope"></a>Obtención y restablecimiento de las credenciales de nivel de aplicación
Para obtener las credenciales de nivel de aplicación:

1. En [Azure Portal](https://portal.azure.com), en el menú de la izquierda, seleccione **App Services** >  **&lt;any_app>**  > **Centro de implementaciones** > **FTP/Credenciales**.

2. Seleccione **Credenciales de la aplicación** y el vínculo **Copiar** para copiar el nombre de usuario o la contraseña.

Para restablecer las credenciales en el nivel de aplicación, seleccione **Restablecer credenciales** en el mismo cuadro de diálogo.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo usar estas credenciales para implementar la aplicación desde [GIT local](deploy-local-git.md) o con [FTP/S](deploy-ftp.md).
