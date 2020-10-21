---
title: Inicio de sesión en el portal de LUIS
description: Si es un usuario que inicia sesión por primera vez en el portal de LUIS, la experiencia de inicio de sesión será ligeramente diferente en función de su cuenta de usuario actual.
services: cognitive-services
ms.custom: ''
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 09/08/2020
ms.topic: how-to
ms.author: a-sakand
author: skandil
ms.openlocfilehash: d801971ca62c416c66608b40aab3e8052fe941a1
ms.sourcegitcommit: 50802bffd56155f3b01bfb4ed009b70045131750
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91931417"
---
# <a name="sign-in-to-luis-portal"></a>Inicio de sesión en el portal de LUIS

Si es un usuario que inicia sesión por primera vez en el portal de LUIS, la experiencia de inicio de sesión será ligeramente diferente en función de su cuenta de usuario actual:
  * La cuenta está asociada a una suscripción de Azure
  * La cuenta no está asociada a una suscripción de Azure

## <a name="determine-account-type"></a>Determinación del tipo de cuenta

Cuando inicie sesión por primera vez en el portal de LUIS, use los siguientes indicadores visuales para determinar el tipo de cuenta.

### <a name="account-without-azure-subscription"></a>Cuenta sin suscripción a Azure

Una cuenta que no está asociada a una suscripción de Azure tiene el icono de Azure en la barra de navegación superior derecha. Después de migrar al tipo de cuenta asociado, el icono ya no aparece.

:::image type="content" source="media/sign-in/sign-in-with-account-without-azure-subscription.png" alt-text="Captura de pantalla parcial de la barra de navegación de LUIS con el icono de Azure.":::

### <a name="account-with-azure-subscription"></a>Cuenta con suscripción a Azure

Una cuenta asociada a una suscripción de Azure le permite seleccionar la suscripción y el recurso que se usarán.

:::image type="content" source="media/sign-in/resource-selection.png" alt-text="Captura de pantalla parcial de la barra de navegación de LUIS con el icono de Azure.":::

## <a name="sign-in-with-account-associated-with-an-azure-subscription"></a>Inicio de sesión con una cuenta asociada a una suscripción de Azure

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai) y acepte las condiciones de uso.

1. Tendrá dos opciones de registro:

    * Seguir usando un recurso de Azure, que es la opción recomendada y pronto será la única disponible. Esta opción le permite vincular la cuenta de LUIS con un recurso de creación de Azure mientras se registra, mediante la elección de un recurso existente en la suscripción o la creación de un nuevo recurso. Equivale a migrar la suscripción sin necesidad de seguir el [proceso de migración](luis-migration-authoring.md#what-is-migration) más adelante. Todos los usuarios deberán haber migrado el 2 de noviembre de 2020.

    * Seguir usando la clave de inicio o de prueba. Esta opción le permite iniciar sesión en LUIS con el recurso de inicio o de prueba que se proporciona, sin tener que crear ningún recurso. Si elige esta opción, se le acabará pidiendo que [migre su cuenta](luis-migration-authoring.md#migration-steps) y vincule las aplicaciones a un recurso de creación. Por eso, se recomienda elegir la opción en la que se va a continuar con el recurso de Azure.

    [Obtenga más información sobre las claves de creación e inicio](luis-how-to-azure-subscription.md#luis-resources). Ambos recursos le ofrecen 1 millón de transacciones de creación gratuitas y 1000 transacciones de punto de conexión de predicción gratuitas.

    :::image type="content" source="media/sign-in/signup-landing-page.png" alt-text="Captura de pantalla parcial de la barra de navegación de LUIS con el icono de Azure.":::

1. Utilice un recurso de creación existente.

    :::image type="content" source="media/sign-in/signup-choose-resource.png" alt-text="Captura de pantalla parcial de la barra de navegación de LUIS con el icono de Azure.":::

    Si ya tiene recursos de creación de LUIS en su suscripción y asocia uno a su cuenta de LUIS durante el inicio de sesión, elija la opción **Use existing Authoring Resource** (Usar recurso de creación existente) y proporcione la siguiente información:

    * **Inquilino**: el inquilino con el que está asociada la suscripción de Azure. No podrá cambiar los inquilinos de la ventana existente. Para cambiar los inquilinos, seleccione en la barra superior el avatar que se encuentra más a la derecha y que contiene sus iniciales.
    * **Nombre de la suscripción**: la suscripción que se asociará con el recurso. Si tiene más de una suscripción que pertenece a su inquilino, seleccione la que desee en la lista desplegable.
    * **Resource Name** (Nombre del recurso): el recurso de creación al que quiere asociar la cuenta.

    > [!Note]
    > Si el recurso de creación que está buscando está atenuado en la lista desplegable, significa que ha iniciado sesión en otro portal regional. [Descripción del concepto de portales regionales](luis-reference-regions.md#luis-authoring-regions)

1. Cree un nuevo recurso de creación.

    :::image type="content" source="media/sign-in/signup-create-resource.png" alt-text="Captura de pantalla parcial de la barra de navegación de LUIS con el icono de Azure." (Su cuenta se ha migrado correctamente). Para finalizar, seleccione **Continue** (Continuar).

    :::image type="content" source="media/sign-in/signup-confirm-2.png" alt-text="Captura de pantalla parcial de la barra de navegación de LUIS con el icono de Azure.":::

    > [!Note]
    > Si tiene una suscripción y al menos un recurso de creación en la misma región en la que se va a suscribir en el portal, puede iniciar sesión automáticamente en LUIS con la migración efectuada y con un recurso asociado sin necesidad de elegir la opción para acceder.


## <a name="sign-in-with-user-account-not-associated-with-an-azure-subscription"></a>Inicio de sesión con una cuenta de usuario no asociada a una suscripción de Azure

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai) y marque la opción para aceptar las condiciones de uso.

1. Para finalizar, seleccione **Continue** (Continuar). Iniciará sesión automáticamente con una clave de prueba o de inicio. Esto significa que se le acabará pidiendo que [migre su cuenta](luis-migration-authoring.md#migration-steps) y vincule las aplicaciones a un recurso de creación. Para realizar el proceso de migración, debe iniciar sesión para obtener una [Evaluación gratuita de Azure](https://azure.microsoft.com/free/).

    :::image type="content" source="media/sign-in/signup-no-subscription.png" alt-text="Captura de pantalla parcial de la barra de navegación de LUIS con el icono de Azure.":::

## <a name="troubleshooting"></a>Solución de problemas

* Si crea un recurso de creación desde Azure Portal en una región distinta a la del portal en el que inicia sesión, el recurso de creación estará atenuado.
* Al crear un nuevo recurso, asegúrese de que su nombre solo incluye caracteres alfanuméricos o el carácter '-', y que no comienza ni termina por '-'. De lo contrario, no se realizará correctamente.
* Asegúrese de que tiene los [permisos adecuados en su suscripción para crear un recurso de Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md#azure-roles). Si no dispone de estos permisos, póngase en contacto con el administrador de la suscripción para que se los proporcione.

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [iniciar una nueva aplicación](luis-how-to-start-new-app.md).
