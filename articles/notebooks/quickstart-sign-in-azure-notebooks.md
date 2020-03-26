---
title: Inicio de sesión en Azure Notebooks (versión preliminar)
description: Inicie sesión rápidamente en Azure Notebooks (versión preliminar) y configure un identificador de usuario, lo que le ofrece la posibilidad de acceder a proyectos guardados y compartir cuadernos con otras personas.
ms.topic: quickstart
ms.date: 04/15/2019
ms.openlocfilehash: b6572a7c0b965b2b72916db577b47eff4f1921c0
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "75647023"
---
# <a name="quickstart-sign-in-and-set-a-user-id-for-azure-notebooks-preview"></a>Inicio rápido: Inicio de sesión y establecimiento de un identificador de usuario para Azure Notebooks (versión preliminar)

Aunque siempre puede ver Azure Notebooks sin iniciar sesión, sí que deberá iniciar sesión para ejecutar cuadernos, acceder a proyectos y cuadernos guardados, y para compartir cuadernos con otras personas.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

## <a name="sign-in"></a>Iniciar sesión

1. Seleccione **Iniciar sesión** en la parte superior derecha de [notebooks.azure.com](https://notebooks.azure.com/).

    ![Ubicación del comando de inicio de sesión en Azure Notebooks](media/accounts/sign-in-command.png)

1. Cuando se le solicite, escriba la dirección de correo electrónico de la cuenta de Microsoft o de una cuenta profesional o educativa, y seleccione **Siguiente**. Los tipos de cuentas se describen en [Su cuenta de usuario de Azure Notebooks](azure-notebooks-user-account.md). Si no tiene una cuenta de Microsoft, o quiere crear una para usarla específicamente con Azure Notebooks, seleccione **Crear una**:

    ![Comando Crear cuenta de Microsoft en el símbolo del sistema de inicio de sesión](media/accounts/create-new-microsoft-account.png)

    > [!Tip]
    > Si intenta crear una nueva cuenta con una dirección de correo electrónico que ya tiene una cuenta asociada a ella, aparecerá el mensaje "You can't sign up here with a work or school email address. Use a personal email, such as Gmail or Yahoo!, or get a new Outlook email." (No se puede iniciar sesión aquí con una dirección de correo electrónico profesional o educativa. Use un correo personal como Gmail o Yahoo! u obtenga un nuevo correo de Outlook). En ese caso, intente iniciar sesión con la dirección de correo electrónico del trabajo sin necesidad de crear una nueva cuenta.

1. Escriba la contraseña cuando se le solicite.

1. Si va a iniciar sesión por primera vez, Azure Notebooks solicita permiso para acceder a su cuenta. Seleccione **Sí** para continuar:

    ![Símbolo del sistema de permisos de la cuenta](media/accounts/account-permission-prompt.png)

## <a name="set-a-user-id"></a>Establecer un id. de usuario

1. Tras el primer inicio de sesión, se le asigna un id. de usuario temporal, como "anon idrca3". Cuando tenga un identificador de usuario que comience con "anon-", Azure Notebooks le pedirá que cree su propio identificador. El identificador de usuario se usa en cualquier dirección URL que obtenga para compartir sus proyectos y cuadernos, así que elija algo que sea único y significativo para usted.

    ![Solicitud para introducir el id. de usuario de Azure Notebooks](media/accounts/create-user-id.png)

    Si selecciona **No, gracias**, Azure Notebooks seguirá solicitándole un identificador de usuario cada vez que inicie sesión. El identificador de usuario también se puede configurar en cualquier momento en su [perfil de usuario](azure-notebooks-user-profile.md).

1. Después de iniciar sesión correctamente, Azure Notebooks se desplaza a la página de perfil público, donde puede seleccionar **Edit Profile Information** (Editar la información del perfil) para rellenar el resto de la información (para obtener más información, consulte [Your profile and user ID](azure-notebooks-user-profile.md) (Su perfil e id. de usuario):

    ![Vista inicial de una página de perfil de Azure Notebooks](media/accounts/profile-page-new.png)

> [!NOTE]
> Si aparece el mensaje "User ID is already in use," (El identificador del usuario ya está en uso), pruebe con un identificador diferente. Los identificadores de usuario son únicos entre todas las cuentas de Azure Notebooks y, además, Azure Notebooks se reserva también ciertos identificadores de usuario como, por ejemplo, nombres de marcas de Microsoft.

## <a name="sign-out"></a>Cerrar sesión

Para cerrar la sesión, seleccione el nombre del usuario en la esquina superior derecha de la página y luego seleccione **Cerrar sesión**:

![Ubicación del comando de cierre de sesión en Azure Notebooks](media/accounts/sign-out-command.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Inicio rápido: Creación y uso compartido de un cuaderno](quickstart-create-share-jupyter-notebook.md)
