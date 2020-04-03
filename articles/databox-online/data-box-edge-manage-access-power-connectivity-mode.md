---
title: Acceso al dispositivo, encendido y modo de conectividad de Azure Data Box Edge | Microsoft Docs
description: Describe cómo administrar el acceso, encendido y modo de conectividad del dispositivo de Azure Data Box Edge que ayuda a transferir datos a Azure
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 06/24/2019
ms.author: alkohli
ms.openlocfilehash: be4b780161003470622cb367d78138cfeffe341b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79454339"
---
# <a name="manage-access-power-and-connectivity-mode-for-your-azure-data-box-edge"></a>Administración del acceso, encendido y modo de conectividad de Azure Data Box Edge

En este artículo se describe cómo administrar el acceso, encendido y modo de conectividad de Azure Data Box Edge. Estas operaciones se realizan mediante la interfaz de usuario web local o en Azure Portal.

En este artículo aprenderá a:

> [!div class="checklist"]
> * Administración del acceso al dispositivo
> * Administración del modo de conectividad
> * Administración del encendido


## <a name="manage-device-access"></a>Administración del acceso al dispositivo

El acceso al dispositivo Data Box Edge se controla mediante una contraseña de dispositivo. Puede cambiar la contraseña mediante la interfaz de usuario web local. También puede restablecer la contraseña del dispositivo en Azure Portal.

### <a name="change-device-password"></a>Cambiar la contraseña del dispositivo

Siga estos pasos en la interfaz de usuario local para cambiar la contraseña del dispositivo.

1. En la interfaz de usuario web local, vaya a **Mantenimiento > Cambio de contraseña**.
2. Escriba la contraseña actual y, a continuación, la nueva contraseña. La contraseña proporcionada debe contener entre 8 y 16 caracteres. La contraseña debe contener tres de los siguientes caracteres: caracteres en mayúsculas, minúsculas, números y caracteres especiales. Confirme la nueva contraseña.

    ![Cambiar contraseña](media/data-box-edge-manage-access-power-connectivity-mode/change-password-1.png)

3. Seleccione **Cambiar contraseña**.
 
### <a name="reset-device-password"></a>Restablecimiento de la contraseña del dispositivo

El flujo de trabajo de restablecimiento no requiere que el usuario recupere la contraseña antigua y es útil cuando se pierde la contraseña. Este flujo de trabajo se realiza en Azure Portal.

1. En Azure Portal, vaya a **Información general > Restablecer la contraseña del administrador**.

    ![Restablecimiento de contraseña](media/data-box-edge-manage-access-power-connectivity-mode/reset-password-1.png)


2. Escriba la nueva contraseña y confírmela. La contraseña proporcionada debe contener entre 8 y 16 caracteres. La contraseña debe contener tres de los siguientes caracteres: caracteres en mayúsculas, minúsculas, números y caracteres especiales. Seleccione **Restablecer**.

    ![Restablecimiento de contraseña](media/data-box-edge-manage-access-power-connectivity-mode/reset-password-2.png)

## <a name="manage-resource-access"></a>Administración del acceso a recursos

Para crear un recurso de Data Box Edge o Data Box Gateway, IoT Hub y Azure Storage, necesita permisos de colaborador o superiores en el nivel del grupo de recursos. También debe registrar los proveedores de recursos correspondientes. Para las operaciones que implican credenciales y claves de activación, también se necesitan permisos para Microsoft Graph API. Estos se describen en las secciones siguientes.

### <a name="manage-microsoft-graph-api-permissions"></a>Administración de permisos para Microsoft Graph API

Cuando genere la clave de activación para el dispositivo de Data Box Edge o realice cualquier operación que requiera credenciales, necesitará permisos para Microsoft Graph API. Las operaciones que podrían necesitar credenciales son:

-  Crear un recurso compartido con una cuenta de almacenamiento asociada.
-  Crear un usuario que puede tener acceso a los recursos compartidos en el dispositivo.

Debe tener acceso de `User` en el inquilino de Active Directory, ya que necesita la capacidad de `Read all directory objects`. No puede ser un usuario invitado, ya que estos no tienen permisos para `Read all directory objects`. Si es un invitado, se producirán errores en operaciones como la generación de una clave de activación, la creación de un recurso compartido en el dispositivo Data Box Edge, la creación de un usuario, la configuración de un rol de proceso perimetral o el restablecimiento de la contraseña del dispositivo.

Para obtener más información sobre cómo proporcionar a los usuarios acceso a Microsoft Graph API, consulte [Referencia de permisos de Microsoft Graph](https://docs.microsoft.com/graph/permissions-reference).

### <a name="register-resource-providers"></a>Registro de proveedores de recursos

Para aprovisionar un recurso de Azure (en el modelo de Azure Resource Manager), necesita un proveedor de recursos que sea compatible con la creación de ese recurso. Por ejemplo, para aprovisionar una máquina virtual, debe tener un proveedor de recursos "Microsoft.Compute" disponible en la suscripción.
 
Los proveedores de recursos se registran en el nivel de la suscripción. De forma predeterminada, cualquier nueva suscripción a Azure se registra previamente con una lista de proveedores de recursos usados con frecuencia. El proveedor de recursos para "Microsoft.DataBoxEdge" no se incluye en esta lista.

No necesita conceder permisos de acceso en el nivel de suscripción para que los usuarios puedan crear recursos como "Microsoft.DataBoxEdge" dentro de los grupos de recursos para los que tienen derechos de propietario, siempre y cuando los proveedores de recursos para estos recursos ya estén registrados.

Antes de que intente crear cualquier recurso, asegúrese de que el proveedor de recursos está registrado en la suscripción. Si el proveedor de recursos no está registrado, deberá asegurarse de que el usuario que crea el nuevo recurso tiene derechos suficientes para registrar al proveedor de recursos necesarios en el nivel de suscripción. Si aún no lo ha hecho, verá el siguiente error:

*La suscripción \<nombre de suscripción> no tiene permisos para registrar el proveedor de recursos: Microsoft.DataBoxEdge.*


Para obtener una lista de los proveedores de recursos registrados en la suscripción actual, ejecute el siguiente comando:

```PowerShell
Get-AzResourceProvider -ListAvailable |where {$_.Registrationstate -eq "Registered"}
```

Para el dispositivo de Data Box Edge, `Microsoft.DataBoxEdge` debe registrarse. Para registrar `Microsoft.DataBoxEdge`, el administrador de suscripciones debe ejecutar el siguiente comando:

```PowerShell
Register-AzResourceProvider -ProviderNamespace Microsoft.DataBoxEdge
```

Para obtener más información sobre cómo registrar un proveedor de recursos, consulte [Resolución de errores del registro del proveedor de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors).

## <a name="manage-connectivity-mode"></a>Administración del modo de conectividad

Aparte del modo predeterminado totalmente conectado, el dispositivo también se puede ejecutar en modo parcialmente conectado o en modo totalmente desconectado. A continuación se describe cada uno de estos modos:

- **Totalmente conectado**: este es el modo normal predeterminado en el que opera el dispositivo. En este modo están habilitadas la carga y descarga de datos en la nube. Puede usar Azure Portal o la interfaz de usuario web local para administrar el dispositivo.

- **Parcialmente desconectado**: en este modo, el dispositivo no puede cargar ni descargar datos de recurso compartido; sin embargo, se pueden administrar mediante Azure Portal.

    Este modo se suele usar cuando se encuentra en una red de satélite de uso medido y el objetivo es minimizar el consumo de ancho de banda de red. Todavía puede producirse un consumo de red mínimo por las operaciones de supervisión del dispositivo.

- **Desconectado**: en este modo, el dispositivo está completamente desconectado de la nube y las cargas y descargas de la nube están deshabilitadas. Solo se puede administrar el dispositivo mediante la interfaz de usuario web local.

    Este modo se suele usar cuando desea desconectar el dispositivo.

Para cambiar el modo del dispositivo, siga estos pasos:

1. En la interfaz de usuario web local del dispositivo, vaya a **Configuración > Configuración de la nube**.
2. En la lista desplegable, seleccione el modo con el que quiere operar el dispositivo. Puede seleccionar entre las opciones **totalmente conectado**, **parcialmente conectado**, y **totalmente desconectado**. Para ejecutar el dispositivo en modo parcialmente desconectado, habilite **Administración en Azure Portal**.

    ![Modo de conectividad](media/data-box-edge-manage-access-power-connectivity-mode/connectivity-mode.png)
 
## <a name="manage-power"></a>Administración del encendido

Puede apagar o reiniciar el dispositivo físico mediante la interfaz de usuario web local. Se recomienda que antes de reiniciarlo, desconecte los recursos compartidos del servidor de datos y, luego, el dispositivo. Esta acción minimizará la posibilidad de daños en los datos.

1. En la interfaz de usuario web local, vaya a **Mantenimiento > Configuración de encendido**.
2. Seleccione **Apagar** o **Reiniciar** según lo que se quiera hacer.

    ![Configuración de encendido](media/data-box-edge-manage-access-power-connectivity-mode/shut-down-restart-1.png)

3. Cuando se le pida confirmación, seleccione **Sí** para continuar.

> [!NOTE]
> Si apaga el dispositivo físico, deberá presionar el botón de encendido en el dispositivo para encenderlo.

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [administrar los recursos compartidos](data-box-edge-manage-shares.md).
