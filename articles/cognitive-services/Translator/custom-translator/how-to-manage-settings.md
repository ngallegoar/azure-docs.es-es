---
title: ¿Cómo administrar la configuración? - Custom Translator
titleSuffix: Azure Cognitive Services
description: Cómo administrar la configuración, crear áreas de trabajo, compartir áreas de trabajo y administrar la clave de suscripción en Custom Translator.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: fe2c0eed2d476d01dffd22a61dd431140f26c475
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83587257"
---
# <a name="how-to-manage-settings"></a>Cómo administrar la configuración

En la página de configuración de Custom Translator, puede crear una nueva área de trabajo, compartir el área de trabajo y agregar o modificar su clave de suscripción de Microsoft Translation.

Para acceder a la página de configuración, realice lo siguiente:

1. Inicie sesión en el portal de [Custom Translator](https://portal.customtranslator.azure.ai/).
2. En el portal de Custom Translator, haga clic en el icono de engranaje de la barra lateral.

    ![Vínculo de configuración](media/how-to/how-to-settings.png)

## <a name="associating-microsoft-translator-subscription"></a>Asociación de la suscripción de Microsoft Translator

Debe tener una clave de suscripción de Traductor asociada con el área de trabajo para entrenar o implementar modelos.

Si no tiene una suscripción, siga los pasos a continuación:

1. Suscribirse a Traductor. En este artículo se muestra cómo suscribirse a Traductor.
2. Apunte la clave para la suscripción a Translator Text. Tanto la clave Key1 como la clave Key2 son aceptables.
3. Navegue de vuelta al portal de Custom Translator.

### <a name="add-existing-key"></a>Adición de una clave existente

1.  Vaya a la página "Configuración" de su área de trabajo.
2.  Haga clic en Agregar clave.

    ![Cómo incorporar la clave de suscripción](media/how-to/how-to-add-subscription-key.png)

3. En el cuadro de diálogo, escriba la clave de la suscripción del traductor y, a continuación, haga clic en el botón "Agregar".

    ![Cómo incorporar la clave de suscripción](media/how-to/how-to-add-subscription-key-dialog.png)
4.  Después de agregar una clave, puede modificarla o eliminarla en cualquier momento.

    ![Clave de suscripción después de agregarla](media/how-to/subscription-key-after-add.png)

## <a name="manage-your-workspace"></a>Administración del área de trabajo

Un área de trabajo es un área para crear y compilar el sistema de traducción personalizada. Un área de trabajo puede contener varios proyectos, modelos y documentos.

Si una parte distinta del trabajo debe compartirse con personas distintas, puede resultar útil crear varias áreas de trabajo.

## <a name="create-a-new-workspace"></a>Crear un área de trabajo

1.  Vaya a la página "Configuración" del área de trabajo.
2.  Haga clic en el botón "Nueva área de trabajo" de la sección "Crear área de trabajo nueva".

    ![Crear área de trabajo nueva](media/how-to/create-new-workspace.png)

4.  En el cuadro de diálogo, escriba el nombre de la nueva área de trabajo.
5.  Haga clic en "Crear".

    ![Cuadro de diálogo Crear área de trabajo nueva](media/how-to/create-new-workspace-dialog.png)

## <a name="share-your-workspace"></a>Compartir el área de trabajo

En Custom Translator, puede compartir el área de trabajo con otros usuarios si una parte distinta del trabajo debe compartirse con personas diferentes.

1.  Vaya a la página "Configuración" del área de trabajo.
2.  Haga clic en el botón "Compartir" de la sección "Configuración de uso compartido".

    ![Compartir área de trabajo](media/how-to/share-workspace.png)

3.  En el cuadro de diálogo, escriba una lista separada por comas de direcciones de correo electrónico con las que quiera compartir esta área de trabajo. Asegúrese de compartirla con la dirección de correo electrónico que la persona usa para iniciar sesión en Custom Translator. A continuación, seleccione el nivel de permisos de uso compartido adecuado.

4.  Si el área de trabajo todavía tiene el nombre predeterminado "Mi área de trabajo", será necesario cambiarlo antes de compartir el área de trabajo.
5.  Haga clic en "Guardar".

## <a name="sharing-permissions"></a>Permisos de uso compartido

1.  **Lector:** un lector en el área de trabajo podrá ver toda la información del área de trabajo.

2.  **Editor:** un editor en el área de trabajo podrá agregar documentos, entrenar modelos y eliminar documentos y proyectos. Puede agregar una clave de suscripción, pero no puede modificar con quién se comparte el área de trabajo, no puede eliminar el área de trabajo ni cambiarle el nombre.

3.  **Propietario:** un propietario tiene permisos totales en el área de trabajo.

## <a name="change-sharing-permission"></a>Cambiar los permisos de uso compartido

Cuando se comparte un área de trabajo, en la sección "Configuración de uso compartido" se muestran todas las direcciones de correo electrónico con las que se comparte esta área de trabajo. Puede cambiar los permisos de uso compartido existentes para cada dirección de correo electrónico si tiene acceso de propietario al área de trabajo.

1.  En la sección "Configuración de uso compartido", se muestra un menú desplegable para cada correo electrónico donde se muestra el nivel de permisos actual.

2.  Haga clic en el menú desplegable y seleccione el nuevo nivel de permisos que quiere asignarle a dicho correo electrónico.

    ![Configuración de los permisos de uso compartido](media/how-to/sharing-permission-settings.png)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre [cómo migrar áreas de trabajo y proyectos](how-to-migrate.md) desde [Microsoft Translator Hub](https://hub.microsofttranslator.com)
