---
title: Creación de una cuenta de Azure Remote Rendering
description: Describe los pasos para crear una cuenta para Azure Remote Rendering
author: florianborn71
ms.author: flborn
ms.date: 02/11/2020
ms.topic: how-to
ms.openlocfilehash: 58757dba9a8956d97c19269c2ac913d801f73746
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/26/2020
ms.locfileid: "83844512"
---
# <a name="create-an-azure-remote-rendering-account"></a>Creación de una cuenta de Azure Remote Rendering

Este capítulo le guía por los pasos necesarios para crear una cuenta para el servicio de **Azure Remote Rendering**. Para completar cualquiera de los inicios rápidos o tutoriales se precisa una cuenta válida.

## <a name="create-an-account"></a>Crear una cuenta

Los siguientes pasos son necesarios para crear una cuenta para el servicio de Azure Remote Rendering:

1. Vaya a la [página de la versión preliminar de Mixed Reality](https://aka.ms/MixedRealityPrivatePreview).
1. Haga clic en el botón "Crear un recurso".
1. En el campo de búsqueda ("Buscar en Marketplace"), escriba "Remote Rendering" y presione Entrar.
1. En la lista de resultados, haga clic en el icono "Remote Rendering".
1. En la siguiente pantalla, haga clic en el botón "Crear". Se abre un formulario para crear una nueva cuenta de Remote Rendering:
    1. Establezca "Nombre del recurso" en el nombre de la cuenta.
    1. Actualice "Suscripción", si es necesario.
    1. Establezca "Grupo de recursos" en el grupo de recursos que desee.
1. Una vez creada la cuenta, vaya a ella y:
    1. En la pestaña *Información general*, anote el identificador de cuenta.
    1. En la pestaña *Configuración > Claves de acceso*, anote la clave principal, que es la clave de cuenta secreta de la cuenta.

### <a name="retrieve-the-account-information"></a>Recuperación de la información de la cuenta

Los ejemplos y los tutoriales requieren que proporcione el identificador de cuenta y una clave. Por ejemplo, en el archivo **arrconfig.json** que se usa para los scripts de ejemplo de PowerShell:

```json
    "accountSettings": {
        "arrAccountId": "<fill in the account ID from the Azure portal>",
        "arrAccountKey": "<fill in the account key from the Azure portal>",
        "region": "<select from available regions>"
    },
```

Consulte la [lista de regiones disponibles](../reference/regions.md) para rellenar la opción *región*.

Los valores de **`arrAccountId`** y **`arrAccountKey`** se pueden encontrar en el portal, tal y como se describe en los pasos siguientes:

* Vaya a [Azure Portal](https://www.portal.azure.com).
* Busque la **"Cuenta de Remote Rendering"** ; debe estar en la lista **"Recursos recientes"** . También puede buscarla en la barra de búsqueda de la parte superior. En ese caso, asegúrese de que la suscripción que quiere usar está seleccionada en el filtro de suscripción predeterminada (icono de filtro junto a la barra de búsqueda):

![Filtro de suscripción](./media/azure-subscription-filter.png)

Al hacer clic en su cuenta, se le lleva a esta pantalla, en la que se muestra el **identificador de cuenta** inmediatamente:

![Identificador de cuenta de Azure](./media/azure-account-id.png)

En la clave, seleccione **Claves de acceso** en el panel de la izquierda. En la página siguiente se muestra una clave principal y una secundaria:

![Claves de acceso de Azure](./media/azure-account-primary-key.png)

El valor de **`arrAccountKey`** puede ser una clave principal o secundaria.

## <a name="link-storage-accounts"></a>Vinculación de cuentas de almacenamiento

Aquí se explica cómo vincular cuentas de almacenamiento a la cuenta de Remote Rendering. Cuando se vincula una cuenta de almacenamiento, no es necesario generar un URI de SAS cada vez que desee interactuar con los datos de la cuenta, por ejemplo al cargar un modelo. En su lugar, puede usar los nombres de cuenta de almacenamiento directamente como se describe en la [sección Carga de un modelo](../concepts/models.md#loading-models).

Los pasos de este procedimiento deben realizarse para cada cuenta de almacenamiento que deba usar este método de acceso alternativo. Si aún no ha creado las cuentas de almacenamiento, puede seguir el paso correspondiente en el [inicio rápido para convertir un modelo para la representación](../quickstarts/convert-model.md#storage-account-creation).

Ahora se supone que tiene una cuenta de almacenamiento. Vaya a ella en el portal y, después, vaya a la pestaña **Access Control (IAM)** de esa cuenta de almacenamiento:

![IAM de la cuenta de almacenamiento](./media/azure-storage-account.png)

 Asegúrese de que tiene permisos de propietario en esta cuenta de almacenamiento para confirmar que puede agregar asignaciones de roles. Si no tiene acceso, se deshabilitará la opción **Agregar una asignación de roles**.

 Debe agregar tres roles distintos, como se describe en los pasos siguientes. Si no proporciona los tres niveles de acceso, habrá problemas con los permisos al intentar acceder a la cuenta de almacenamiento.

 Haga clic en el botón **Agregar** en el icono "Agregar una asignación de roles" para agregar el primer rol:

![IAM de la cuenta de almacenamiento](./media/azure-add-role-assignment.png)

* El primer rol que se va a asignar es el de **Propietario**, como se muestra en la captura de pantalla anterior.
* Seleccione la cuenta **Remote Rendering** en la lista desplegable de **Asignar acceso a**.
* Seleccione la suscripción y la cuenta de Remote Rendering en las últimas listas desplegables.

> [!WARNING]
> En caso de que la cuenta de Remote Rendering no aparezca en la lista, consulte la [sección de solución de problemas](../resources/troubleshoot.md#cant-link-storage-account-to-arr-account).

Repita la adición de nuevos roles dos veces más para las selecciones correspondientes en la lista desplegable **Rol**:
* **Colaborador de la cuenta de almacenamiento**
* **Colaborador de datos de blobs de almacenamiento**

Las demás listas desplegables se seleccionan como en el primer paso.

Si ha agregado los tres roles, la cuenta de Azure Remote Rendering tiene acceso a la cuenta de almacenamiento mediante las identidades de servicio administradas asignadas por el sistema.

## <a name="next-steps"></a>Pasos siguientes

* [Autenticación](authentication.md)
* [n](frontend-apis.md)
* [Scripts de PowerShell de ejemplo](../samples/powershell-example-scripts.md)
