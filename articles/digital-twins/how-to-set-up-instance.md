---
title: Creación de una instancia de Azure Digital Twins
titleSuffix: Azure Digital Twins
description: Vea cómo configurar una instancia del servicio Azure Digital Twins.
author: baanders
ms.author: baanders
ms.date: 4/22/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: fecacbd2c7c6549a1321367157bb179321779ca9
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86206506"
---
# <a name="set-up-an-azure-digital-twins-instance"></a>Configuración de una instancia de Azure Digital Twins

Este artículo le guiará por los pasos básicos para configurar una nueva instancia de Azure Digital Twins. Esto incluye la creación de la instancia y la asignación de permisos de [Azure Active Directory (AAD)](../active-directory/fundamentals/active-directory-whatis.md) a la instancia por sí mismo.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [Cloud Shell for Azure Digital Twins](../../includes/digital-twins-cloud-shell.md)]

## <a name="set-up-an-azure-digital-twins-instance"></a>Configuración de una instancia de Azure Digital Twins

A continuación, creará un grupo de recursos de Azure para usarlo en este procedimiento. Luego, puede **crear una instancia de Azure Digital Twins** dentro de ese grupo de recursos. 

También deberá indicar un nombre para la instancia y elegir una región para la implementación. Para ver qué regiones admiten Azure Digital Twins, visite [Productos de Azure disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=digital-twins).

>[!NOTE]
> El nombre de la nueva instancia debe ser único dentro de la región (lo que significa que si otra instancia de Azure Digital Twins en esa región ya usa el nombre elegido, tendrá que elegir otro nombre).

Cree el grupo de recursos y la instancia con los comandos siguientes:

```azurecli
az group create --location <region> --name <name-for-your-resource-group>
az dt create --dt-name <name-for-your-Azure-Digital-Twins-instance> -g <your-resource-group> -l <region>
```

El resultado de estos comandos tiene un aspecto similar al siguiente, e incluye información sobre los recursos que ha creado:

:::image type="content" source="media/how-to-set-up-instance/create-instance.png" alt-text="Ventana Comandos con la creación correcta del grupo de recursos y la instancia de Azure Digital Twins":::

Anote *hostName*, *name* y *resourceGroup* del resultado de la instancia de Azure Digital Twins. Estos son todos los valores clave que puede necesitar a medida que sigue trabajando con la instancia de Azure Digital Twins para configurar la autenticación y los recursos de Azure relacionados.

> [!TIP]
> Puede ver estas propiedades, junto con todas las propiedades de la instancia, en cualquier momento si ejecuta `az dt show --dt-name <your-Azure-Digital-Twins-instance>`.

### <a name="assign-azure-active-directory-permissions"></a>Asignación de permisos de Azure Active Directory

Azure Digital Twins usa [Azure Active Directory (AAD)](../active-directory/fundamentals/active-directory-whatis.md) para el control de acceso basado en roles (RBAC). Esto significa que, para poder realizar llamadas de plano de datos a la instancia de Azure Digital Twins, primero debe asignarse a sí mismo un rol con estos permisos.

Para usar Azure Digital Twins con una aplicación cliente, también debe asegurarse de que la aplicación cliente pueda autenticarse en Azure Digital Twins. Para esto, configure un registro de aplicación de Azure Active Directory (AAD); para obtener información sobre esto, puede leer [Procedimiento: Autenticación de una aplicación cliente](how-to-authenticate-client.md).

#### <a name="assign-yourself-a-role"></a>Asignación de un rol a sí mismo

Cree una asignación de roles para sí mismo en la instancia de Azure Digital Twins mediante el correo electrónico asociado al inquilino de AAD en su suscripción a Azure. 

Para poder hacerlo, debe clasificarse como propietario en su suscripción de Azure. Puede comprobarlo al ejecutar el comando `az role assignment list --assignee <your-Azure-email>` y al comprobar en la salida que el valor de *roleDefinitionName* sea *Propietario*. Si observa que el valor es *Colaborador* o algo distinto de *Propietario*, póngase en contacto con el administrador de suscripciones con la capacidad de conceder permisos en su suscripción. Puede elevar el rol en toda la suscripción para poder ejecutar el siguiente comando, o bien un propietario puede ejecutar el siguiente comando en su nombre para configurar los permisos de Azure Digital Twins.

Para asignar permisos de usuario "propietario" en la instancia de Azure Digital Twins, use el siguiente comando (debe ejecutarlo un propietario de la suscripción de Azure):

```azurecli
az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<your-AAD-email>" --role "Azure Digital Twins Owner (Preview)"
```

El resultado de este comando es la información de salida acerca de la asignación de roles que se ha creado.

> [!TIP]
> Si en su lugar obtiene un error *400: BadRequest*, ejecute el siguiente comando para obtener el *ObjectID* para el usuario:
> ```azurecli
> az ad user show --id <your-AAD-email> --query objectId
> ```
> A continuación, repita el comando de asignación de roles *id. de objeto* de su usuario en lugar de su correo electrónico.

Ahora tiene lista una instancia de Azure Digital Twins y los permisos para administrarla.

## <a name="next-steps"></a>Pasos siguientes

Consulte cómo configurar y autenticar una aplicación cliente para que funcione con su instancia:
* [Procedimiento: Autenticación de una aplicación cliente](how-to-authenticate-client.md)
