---
title: Migración al recurso de Azure para la creación 2
titleSuffix: Azure Cognitive Services
description: Migre a una clave de recurso de creación de Azure 2.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 06/17/2020
ms.author: diberry
ms.openlocfilehash: 5f0778436db7bd8c3a09e3ba346d8a9a6c4af454
ms.sourcegitcommit: 55b2bbbd47809b98c50709256885998af8b7d0c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2020
ms.locfileid: "84983824"
---
# <a name="migrate-to-an-azure-resource-authoring-key"></a>Migración a una clave de creación de recursos de Azure

La autenticación de creación de Language Understanding (LUIS) cambió de una cuenta de correo electrónico a un recurso de Azure. Aunque actualmente no es necesario, se aplicará el cambio a un recurso de Azure en el futuro.


## <a name="what-is-migration"></a>¿Qué es la migración?

La migración es el proceso de cambiar la autenticación de creación de una cuenta de correo electrónico a un recurso de Azure. La cuenta se vinculará a una suscripción de Azure y un recurso de creación de Azure después de la migración. **Todos los usuarios de LUIS (propietarios o colaboradores) tendrán que migrarse.** La migración debe realizarse desde el portal de LUIS. Aunque cree las claves de creación, como con la CLI de LUIS, aún tendrá que completar el proceso de migración en el portal de LUIS. Puede seguir teniendo coautores en las aplicaciones después de la migración, pero se agregarán en el nivel de recursos de Azure en lugar de en el nivel de aplicación.

> [!Note]
> Antes de la migración, los coautores se conocen como _colaboradores_ en el nivel de aplicación de LUIS. Después de la migración, el rol de _colaborador_ de Azure se usa para la misma funcionalidad, pero en el nivel de recurso de Azure.

## <a name="note-before-you-migrate"></a>A tener en cuenta antes de la migración

* La migración es un proceso **unidireccional**. La migración no se puede revertir.
* Las aplicaciones se **migrarán automáticamente** si usted es el **propietario** de la aplicación.
* El propietario no puede elegir un subconjunto de las aplicaciones que se van a migrar y el proceso no es reversible.
* Las aplicaciones **desaparecerán de los lados del colaborador** después de que el **propietario se migre**.
* Se solicitará a los propietarios que envíen correos electrónicos a los colaboradores para informarles de la migración.
* Las aplicaciones **no se migrarán** con usted si es **colaborador** en la aplicación.
* No hay ninguna manera de que un propietario sepa que sus colaboradores se han migrado.
* **La migración no** recopila automáticamente colaboradores ni los mueve o agrega al recurso de creación de Azure. El propietario de la aplicación es el que necesita completar este paso después de la migración. Este paso requiere [permisos para el recurso de creación adecuado](https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-collaborate).
* Una vez que se haya asignado al recurso de Azure, **los colaboradores deberán migrarse para acceder a las aplicaciones**. De lo contrario, no tendrán acceso para crear las aplicaciones.
* Un usuario migrado no se puede agregar como colaborador de la aplicación.
* Si **posee claves de predicción asignadas a aplicaciones que pertenecen a otro usuario**, esto **bloqueará la migración** para el propietario y los colaboradores. Consulte a continuación las recomendaciones.

> [!Note]
> Si necesita crear un recurso de predicción en tiempo de ejecución, hay [un proceso independiente](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) para hacerlo.

## <a name="migration-prerequisites"></a>Requisitos previos de la migración

* Debe estar asociado con una suscripción válida de Azure. Pídale a su administrador de inquilinos que le agregue a la suscripción, o bien puede registrarse para obtener una [aquí](https://azure.microsoft.com/free/).
* Debe crear un recurso de creación de LUIS de Azure desde el portal de LUIS o desde Azure Portal. La creación de un recurso de creación desde el portal de LUIS forma parte del flujo de migración que se describe en la sección siguiente.
* Si tiene rol de **colaborador en las aplicaciones**, estas no se migrarán automáticamente. **Se recomienda que realice una copia de seguridad de estas aplicaciones exportándolas**, o bien que use la [API de exportación](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40). Puede volver a importar la aplicación en LUIS después de la migración. El proceso de importación crea una aplicación nueva con un nuevo identificador de aplicación, para el que usted es el propietario.
* Si es el **propietario de la aplicación**, no tendrá que exportar las aplicaciones, ya que se migrarán automáticamente. **Se recomienda guardar la lista de colaboradores de cada aplicación**. Una plantilla de correo electrónico que tenga esta lista se proporciona opcionalmente como parte del proceso de migración.


|Portal|Propósito|
|--|--|
|[Azure](https://azure.microsoft.com/free/)|* Crear recursos de predicción y creación.<br>* Asignar colaboradores en los recursos.|
|[LUIS](https://www.luis.ai)|* Migrar a recursos de creación nuevos.<br>* Crear un nuevo recurso de creación en el flujo de migración.<br>* Asignar o anular la asignación de recursos de predicción y creación a las aplicaciones desde la página **Administrar -> Recursos de Azure**. <br>* Mover las aplicaciones de un recurso de creación a otro.  |

> [!Note]
> **La creación de la aplicación de LUIS es gratuita**, lo que viene indicado por el nivel `F0`. [Más información sobre planes de tarifa](luis-limits.md#key-limits).


## <a name="migration-steps"></a>Pasos de migración

1. En el portal de LUIS en el que está trabajando, puede comenzar el proceso de migración desde el icono de **Azure** en la barra de herramientas superior.

   > [!div class="mx-imgBorder"]
   > ![Icono de migración](./media/migrate-authoring-key/migration-button.png)

2. La ventana emergente de la migración le permite continuar con la migración o migrar más adelante. Seleccione **Migrate now** (Migrar ahora).

   > [!div class="mx-imgBorder"]
   > ![Primera ventana emergente del proceso de migración, selección de Migrar ahora](./media/migrate-authoring-key/prompt-when-migrating-2.png).

3. Opcionalmente, si alguna de las aplicaciones tiene colaboradores, se le pedirá que **les envíe un correo electrónico** para informarles de la migración. Se trata de un paso opcional.

   Para cada colaborador y aplicación, la aplicación de correo electrónico predeterminada se abre con un correo electrónico ligeramente formateado. Puede editar el correo electrónico antes de enviarlo. La plantilla de correo electrónico incluye el id. de la aplicación y el nombre de la aplicación exactos.
   
   ```html
   Dear Sir/Madam,

   I will be migrating my LUIS account to Azure. Consequently, you will no longer have access to the following app:

   App Id: <app-ID-omitted>
   App name: Human Resources
      
   Thank you
   ```
   
   > [!Note]
   > Una vez que haya migrado la cuenta a Azure, las aplicaciones dejarán de estar disponibles para los colaboradores.

4. Opcionalmente, si es un colaborador en cualquier aplicación, se le pedirá que **exporte una copia de las aplicaciones** seleccionando esta opción durante el flujo de migración. Una vez seleccionada la opción, encontrará la página siguiente, donde puede hacer clic en el botón para descargar a la izquierda para exportar las aplicaciones que desee. Puede volver a importar estas aplicaciones después de migrar, ya que no se migrarán automáticamente con usted. Se trata de un paso opcional.

   > [!div class="mx-imgBorder"]
   > ![Solicitud para exportar la aplicación](./media/migrate-authoring-key/export-app-for-collabs-2.png).

5. Puede optar por crear un nuevo recurso de creación de LUIS o migrar a un recurso de creación existente si ya ha creado uno de Azure. Elija la opción que desee seleccionando el botón adecuado de abajo.

   > [!div class="mx-imgBorder"]
   > ![Creación de un recurso de creación](./media/migrate-authoring-key/choose-existing-authoring-resource.png)

### <a name="create-new-authoring-resource-from-luis-to-migrate"></a>Creación de un nuevo recurso de creación a partir de LUIS para migrar

Si desea crear un nuevo recurso de creación, seleccione **Create a new authoring resource** (Crear un nuevo recurso de creación) y proporcione esta información en la siguiente ventana.

> [!div class="mx-imgBorder"]
> ![Creación de un recurso de creación](./media/migrate-authoring-key/create-new-authoring-resource-2.png)

* **Nombre del recurso**: el nombre personalizado que elija, que se usa como parte de la dirección URL para las consultas de punto de conexión de creación y predicción.
* **Nombre de la suscripción**: la suscripción que se asociará con el recurso. Si tiene más de una suscripción que pertenece a su inquilino, seleccione la que desee en la lista desplegable.
* **Grupo de recursos**: nombre del grupo de recursos personalizado que elija de la lista desplegable. Los grupos de recursos le permiten agrupar los recursos de Azure para el acceso y la administración.
* **Inquilino**: el inquilino con el que está asociada la suscripción de Azure. Esta opción está establecida de forma predeterminada en el inquilino que está eligiendo actualmente. Puede cambiar los inquilinos seleccionando el avatar que se encuentra más a la derecha y que contiene sus iniciales.

Después de escribir la información, seleccione **Listo**.

Tenga en cuenta que puede tener 10 recursos de creación gratuitos por región y suscripción. Si la suscripción tiene más de 10 recursos de creación en la misma región, no podrá crear uno nuevo.

* Cuando se crea el recurso de creación, se muestra el mensaje de operación correcta. Seleccione **Cerrar** para cerrar la ventana emergente.

  > [!div class="mx-imgBorder"]
  > ![El recurso de creación se creó correctamente](./media/migrate-authoring-key/migration-success-2.png).

### <a name="use-existing-authoring-resource-to-migrate"></a>Uso del recurso de creación existente para migrar

Si la suscripción ya está asociada a un recurso de Azure de creación de LUIS o si ha creado uno en Azure Portal y desea migrar a él en lugar de crear un nuevo recurso, seleccione **Use Existing Authoring resource** (Usar el recurso de creación existente) y proporcione esta información en la ventana siguiente.

> [!div class="mx-imgBorder"]
>![Creación de un recurso de creación](./media/migrate-authoring-key/choose-existing-authoring-resource-2.png)

* **Inquilino**: el inquilino con el que está asociada la suscripción de Azure. Esta opción está establecida de forma predeterminada en el inquilino que está eligiendo actualmente.
* **Nombre de la suscripción**: la suscripción que se asociará con el recurso. Si tiene más de una suscripción que pertenece a su inquilino, seleccione la que desee en la lista desplegable.
* **Nombre del recurso**: seleccione el recurso de creación al que desea migrar.

> [!Note]
> Si no puede ver el recurso de creación en la lista desplegable, asegúrese de que lo creó en la **ubicación adecuada** en función del portal de LUIS en el que haya iniciado sesión. Asegúrese también de que lo que ha creado es realmente un **recurso de creación** y no un **recurso de predicción**.


* Valide el nombre del recurso de creación y haga clic en el botón **Migrar ahora**.

 > [!div class="mx-imgBorder"]
 > ![Creación de un recurso de creación](./media/migrate-authoring-key/choose-authoring-resource-and-migrate-2.png)

* Se muestra el mensaje de operación correcta. Seleccione **Cerrar** para cerrar la ventana emergente.

 > [!div class="mx-imgBorder"]
 > ![El recurso de creación se creó correctamente](./media/migrate-authoring-key/migration-success-2.png).

## <a name="using-apps-after-migration"></a>Uso de aplicaciones después de la migración

* Después del proceso de migración, todas las aplicaciones de LUIS que sean de su propiedad estarán ahora asignadas a un único recurso de creación de LUIS.
* La lista **Aplicaciones** muestra las aplicaciones que se migraron al recurso de creación nuevo.
* Antes de acceder a las aplicaciones, seleccione la suscripción y el recurso de creación de LUIS para ver las aplicaciones que puede crear.

 > [!div class="mx-imgBorder"]
 > ![Seleccione la suscripción y el recurso de creación de LUIS para ver las aplicaciones que puede crear.](./media/create-app-in-portal-select-subscription-luis-resource.png)

* No es necesario conocer la clave del recurso de creación para seguir editando las aplicaciones en el portal de LUIS.
* Si tiene previsto editar las aplicaciones mediante programación, necesitará los valores de la clave de creación. Estos valores se muestran en la página **Administrar -> Recursos de Azure** en el portal de LUIS y también están disponibles en la página **Claves** del recurso de Azure Portal. También puede crear más recursos de creación y asignarlos desde la misma página.

 > [!div class="mx-imgBorder"]
 > ![Administración de un recurso de creación](./media/migrate-authoring-key/manage-authoring-resource-2.png).

## <a name="add-contributors-to-authoring-resources"></a>Adición de colaboradores a recursos de creación

[!INCLUDE [Manage contributors for the Azure authoring resource for language understanding](./includes/manage-contributors-authoring-resource.md)]

Aprenda a [agregar colaboradores](luis-how-to-collaborate.md) en el recurso de creación. Los colaboradores tendrán acceso a todas las aplicaciones de ese recurso.

Puede agregar colaboradores al recurso de creación desde _Azure Portal_, en la página **Access Control (IAM)** correspondiente a ese recurso. Para más información, consulte la sección sobre [agregar acceso para un colaborador](luis-migration-authoring-steps.md#after-the-migration-process-add-contributors-to-your-authoring-resource).

> [!Note]
> Si el propietario de la aplicación de LUIS ha realizado la migración y ha agregado el usuario como colaborador en el recurso de Azure, el colaborador seguirá sin tener acceso a la aplicación, salvo que también migre.

## <a name="luis-portal-migration-reminders"></a>Recordatorios de migración del portal de LUIS

El [portal de LUIS](https://www.luis.ai) proporciona el proceso de migración.

Se le pedirá que realice la migración si:
* Tiene aplicaciones en el sistema de autenticación por correo electrónico para la creación.
* Y usted es el propietario de la aplicación.

Cada semana se le pedirá que migre las aplicaciones. Puede cancelar esta ventana sin realizar la migración. Si desea migrar antes del siguiente período programado, puede empezar el proceso de migración desde el icono de **Azure** de la barra de herramientas superior del portal de LUIS.

Puede retrasar el proceso de migración si cancela fuera de la ventana. Se le pedirá periódicamente que realice la migración hasta que migre o se pase la fecha límite de la migración. Puede iniciar el proceso de migración desde el icono de candado de la barra de navegación superior.

## <a name="prediction-resources-blocking-migration"></a>Recursos de predicción que bloquean la migración
La migración afecta negativamente a cualquier entorno de ejecución de aplicaciones. Al migrar, se quitan todos los colaboradores de sus aplicaciones y se le quita como colaborador de otras aplicaciones. Este proceso significa que las claves que un colaborador asigna también se quitan, lo que podría interrumpir la aplicación si está en producción. Este es el motivo por el que bloqueamos la migración hasta que quite los colaboradores o las claves asignadas a ellos manualmente.

### <a name="when-does-prediction-resources-block-migration"></a>¿Cuándo se bloquea la migración por los recursos de predicción?
* La migración se bloquea si ha asignado recursos de predicción o tiempo de ejecución en las aplicaciones que no son de su propiedad.
* La migración se bloquea si otros usuarios asignan recursos de predicción o tiempo de ejecución en las aplicaciones que son de su propiedad.

### <a name="recommended-steps-to-do-if-you-are-the-owner-of-the-app"></a>Pasos recomendados que debe realizar si es el propietario de la aplicación
Si es propietario de algunas aplicaciones y los colaboradores han asignado una clave de predicción o tiempo de ejecución a esta aplicación, se muestra un error al migrar que enumera los identificadores de aplicación que tienen claves de predicción asignadas por otros usuarios.

Se recomienda lo siguiente:
* Informar a los colaboradores de la migración.
* Quitar todos los colaboradores de las aplicaciones que se muestran en el error.
* Someterse al proceso de migración que debe realizarse correctamente si se quitan manualmente los colaboradores.
* Dotar de capacidad de contribución a los colaboradores en el nuevo recurso de creación.
* Los colaboradores deben migrar y reasignar los recursos de predicción a las aplicaciones.
Tenga en cuenta que esto provocará una interrupción temporal en la aplicación hasta que se vuelvan a asignar los recursos de predicción.

Otra solución aquí es que, antes de la migración del propietario, los colaboradores pueden agregar propietarios de aplicaciones como colaboradores en sus suscripciones de Azure desde Azure Portal. Esto concederá al propietario acceso al recurso de predicción en tiempo de ejecución. Si el propietario se migra utilizando la nueva suscripción a la que se ha agregado (que se encontrará bajo un nuevo inquilino), esto no solo desbloqueará el proceso de migración tanto para el colaborador como para el propietario de la aplicación, sino que permitirá una migración fluida de las aplicaciones que tienen aún asignada la clave de predicción sin interrumpir las aplicaciones.


### <a name="recommended-steps-to-do-if-you-are-a-collaborator-on-an-app"></a>Pasos recomendados para realizar si es colaborador en una aplicación
Si está colaborando en aplicaciones y ha asignado una clave de predicción/tiempo de ejecución a esta aplicación, se muestra un error al migrar que enumera los identificadores de aplicación y las rutas de acceso de clave que están bloqueando la migración.

Se recomienda lo siguiente:
* Exportar aplicaciones como copia de seguridad. Esto se proporciona como un paso opcional en el proceso de migración.
* Anular la asignación de los recursos de predicción desde la página **Administrar > Recursos de Azure**.
* Someterse al proceso de migración.
* Importar aplicaciones después de la migración.
* Volver a asignar las claves de predicción a la página **Administración -> Recursos de Azure** de las aplicaciones.

> [!Note]
> Al importar las aplicaciones después de la migración, estas tendrán distintos identificadores de aplicación y serán distintas de los que se obtienen en producción. Ahora será el propietario de estas aplicaciones.

## <a name="troubleshooting-migration-process"></a>Solución de problemas del proceso de migración

Cuando intenta migrar pero no puede encontrar su suscripción de Azure en la lista desplegable:
* Asegúrese de tener una suscripción válida de Azure que tenga autorización para crear recursos de Cognitive Services. Vaya a [Azure Portal](https://ms.portal.azure.com) y compruebe el estado de la suscripción. Si no dispone de ninguna, [cree una evaluación gratuita](https://azure.microsoft.com/free/).
* Asegúrese de que se encuentra en el inquilino adecuado asociado a su suscripción válida. Puede cambiar los inquilinos desde el avatar más a la izquierda en la barra de herramientas siguiente: ![Cambiar inquilinos](./media/migrate-authoring-key/switch-user-tenant-2.png).

 Si ya tiene un recurso de creación existente, pero no puede encontrarlo al seleccionar la opción "Use Existing Authoring Resource" (Usar el recurso de creación existente):
* El recurso se creó probablemente en una ubicación diferente a la del portal en el que inició sesión. Consulte [Regiones de creación de LUIS y los portales](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-authoring-regions).
* En su lugar, cree un recurso nuevo desde el portal de LUIS.

Si selecciona la opción "Create a new authoring resource" (Crear un nuevo recurso de creación) y se produce un error en la migración con el mensaje "Failed retrieving user's Azure information, retry again later" (Error al recuperar la información de Azure del usuario, vuelva a intentarlo más tarde).
* Es posible que la suscripción tenga 10 o más recursos de creación por región y suscripción. Si ese es el caso, no podrá crear un nuevo recurso de creación.
* Migre seleccionando la opción "Use Existing Authoring Resource" (Usar el recurso de creación existente) y seleccione uno de los recursos existentes en su suscripción.

Si ve el siguiente error, consulte la [sección Pasos recomendados que debe realizar si es el propietario de la aplicación] ![Error en la migración de los propietarios](./media/migrate-authoring-key/migration-failed-for-owner-2.png).

Si ve el siguiente error, consulte la [sección Pasos recomendados para realizar si es colaborador en una aplicación] ![Error en la migración de los colaboradores](./media/migrate-authoring-key/migration-failed-for-collab-2.png).


## <a name="next-steps"></a>Pasos siguientes

* Revisión de [conceptos](luis-concept-keys.md) sobre las claves de creación y en tiempo de ejecución
* Revisión de [cómo asignar claves](luis-how-to-azure-subscription.md) y agregar [colaboradores](luis-how-to-collaborate.md)
