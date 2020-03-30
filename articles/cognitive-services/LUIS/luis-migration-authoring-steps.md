---
title: Migración a un recurso de creación de Azure
titleSuffix: Azure Cognitive Services
description: Migre a un recurso de creación de Azure.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: diberry
ms.openlocfilehash: f5197ae79670e4543c58224a33838706edae6218
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78194652"
---
# <a name="steps-to-migrate-to-the-azure-authoring-resource"></a>Pasos para migrar al recurso de creación de Azure

En el portal de Language Understanding (LUIS), migre todas las aplicaciones que posee para usar el recurso de creación de Azure.

## <a name="prerequisites"></a>Prerequisites

* **De manera opcional**, haga una copia de seguridad de las aplicaciones desde la lista de aplicaciones del portal de LUIS; para ello, exporte cada aplicación o use la [API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) de exportación.
* **Opcionalmente**, guarde la lista del colaborador de cada aplicación. Se puede enviar un correo electrónico a todos los colaboradores como parte del proceso de migración.
* **Obligatorio**, debe tener una [suscripción a Azure](https://azure.microsoft.com/free/). Una parte del proceso de suscripción requiere información de facturación. Sin embargo, puede usar los planes de tarifa gratis (F0) cuando use LUIS. Es posible que necesite encontrar un nivel de pago a medida que aumente su uso.

Si no tiene una suscripción a Azure, [suscríbase](https://azure.microsoft.com/free/).

## <a name="access-the-migration-process"></a>Acceso al proceso de migración

Cada semana se le pedirá que migre las aplicaciones. Puede cancelar esta ventana sin realizar la migración. Si desea migrar antes del siguiente período programado, puede empezar el proceso de migración desde el icono de **Azure** de la barra de herramientas superior del portal de LUIS.

> [!div class="mx-imgBorder"]
> ![Icono de migración](./media/migrate-authoring-key/migration-button.png)

## <a name="app-owner-begins-the-migration-process"></a>El propietario de la aplicación comienza el proceso de migración

El proceso de migración está disponible si es el propietario de cualquier aplicación de LUIS.

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai) y acepte las condiciones de uso.
1. La ventana emergente de la migración le permite continuar con la migración o migrar más adelante. Seleccione **Migrate now** (Migrar ahora). Si decide migrar más adelante, tendrá 9 meses para realizar la migración a la clave de creación nueva de Azure.

    ![Primera ventana emergente del proceso de migración, seleccione Migrate now (Migrar ahora).](./media/migrate-authoring-key/migrate-now.png)

1. Opcionalmente, si alguna de las aplicaciones tiene colaboradores, se le pedirá que **les envíe un correo electrónico** para informarles de la migración. Se trata de un paso opcional.

    Una vez que haya migrado la cuenta a Azure, las aplicaciones dejarán de estar disponibles para los colaboradores.

    Para cada colaborador y aplicación, la aplicación de correo electrónico predeterminada se abre con un correo electrónico ligeramente formateado. Puede editar el correo electrónico antes de enviarlo.

    La plantilla de correo electrónico incluye el id. de la aplicación y el nombre de la aplicación exactos.

    ```html
    Dear Sir/Madam,

    I will be migrating my LUIS account to Azure. Consequently, you will no longer have access to the following app:

    App Id: <app-ID-omitted>
    App name: Human Resources

    Thank you
    ```

1. Para crear un recurso de creación de LUIS, seleccione usar un recurso de creación existente o crear un nuevo recurso de creación.

    > [!div class="mx-imgBorder"]
    > ![Creación de un recurso de creación](./media/migrate-authoring-key/choose-existing-authoring-resource.png)

1. En la ventana siguiente, escriba la información de la clave de recurso. Después de escribir la información, seleccione **Crear recurso**. Puede tener 10 recursos de creación gratuitos por región y suscripción.

    ![Creación de un recurso de creación](./media/migrate-authoring-key/choose-authoring-resource-form.png)

    Al **crear un nuevo recurso de creación**, proporcione la información siguiente:

    * **Nombre del recurso**: el nombre personalizado que elija, que se usa como parte de la dirección URL para las consultas de punto de conexión de creación y predicción.
    * **Inquilino**: el inquilino con el que está asociada la suscripción de Azure.
    * **Nombre de la suscripción**: la suscripción que se facturará por el recurso.
    * **Grupo de recursos**: nombre del grupo de recursos personalizado que elija o cree. Los grupos de recursos le permiten agrupar los recursos de Azure para el acceso y la administración.
    * **Ubicación**: la opción de ubicación se basa en la selección del **grupo de recursos**.
    * **Plan de tarifa**: el plan de tarifa determina el número máximo de transacciones por segundo y mes.

1. Valide el recurso de creación y **migre ahora**.

    ![Creación de un recurso de creación](./media/migrate-authoring-key/choose-authoring-resource-and-migrate.png)

1. Cuando se crea el recurso de creación, se muestra el mensaje de operación correcta. Seleccione **Cerrar** para cerrar la ventana emergente.

    ![El recurso de creación se creó correctamente.](./media/migrate-authoring-key/migration-success.png)

    La lista **Aplicaciones** muestra las aplicaciones que se migraron al recurso de creación nuevo.

    No es necesario conocer la clave del recurso de creación para seguir editando las aplicaciones en el portal de LUIS. Si tiene previsto editar las aplicaciones mediante programación, necesita los valores de clave de creación. Estos valores se muestran en la página **Administrar -> Recursos de Azure** en el portal de LUIS y también están disponibles en la página **Claves** del recurso de Azure Portal.

1. Antes de acceder a las aplicaciones, seleccione la suscripción y el recurso de creación de LUIS para ver las aplicaciones que puede crear.

    ![Seleccione la suscripción y el recurso de creación de LUIS para ver las aplicaciones que puede crear.](./media/migrate-authoring-key/app-list-by-subscription-and-resource.png)


## <a name="app-contributor-begins-the-migration-process"></a>El colaborador de la aplicación comienza el proceso de migración

Siga los mismos pasos que el propietario de la aplicación para la migración. El proceso crea un nuevo recurso de creación de tipo `LUIS.Authoring`.

Debe migrar la cuenta para ser agregado como colaborador a las aplicaciones migradas que sean propiedad de otros usuarios.

## <a name="after-the-migration-process-add-contributors-to-your-authoring-resource"></a>Después del proceso de migración, agregue colaboradores al recurso de creación.

[!INCLUDE [Manage contributors for the Azure authoring resource for language understanding](./includes/manage-contributors-authoring-resource.md)]

Aprenda a [agregar colaboradores](luis-how-to-collaborate.md).

## <a name="troubleshooting-errors-with-the-migration-process"></a>Solución de problemas de errores con el proceso de migración

Si recibe un error `MissingSubscriptionRegistration` en el portal de LUIS con una barra de notificación roja durante el proceso de migración, cree un recurso de Cognitive Services en [Azure Portal](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) o la [CLI de Azure](luis-how-to-azure-subscription.md#create-resources-in-azure-cli). Obtenga más información sobre las [causas de este error](../../azure-resource-manager/templates/error-register-resource-provider.md#cause).

## <a name="next-steps"></a>Pasos siguientes


* Revisión de [conceptos](luis-concept-keys.md) sobre las claves de creación y en tiempo de ejecución
* Revisión de [cómo asignar claves](luis-how-to-azure-subscription.md) y agregar [colaboradores](luis-how-to-collaborate.md)
