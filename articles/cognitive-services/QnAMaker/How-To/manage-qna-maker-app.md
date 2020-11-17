---
title: 'Administración de una aplicación de QnA Maker: QnA Maker'
description: QnA Maker permite que varias personas colaboren en una base de conocimiento. QnA Maker ofrece la capacidad de mejorar la calidad de la base de conocimiento con el aprendizaje activo. Es posible revisar, aceptar o rechazar y agregar sin quitar ni cambiar las preguntas existentes.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 11/09/2020
ms.openlocfilehash: 93d9cc871d1cb114f7f08b68eb8ae9d597e228b9
ms.sourcegitcommit: 051908e18ce42b3b5d09822f8cfcac094e1f93c2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2020
ms.locfileid: "94376492"
---
# <a name="manage-qna-maker-app"></a>Administración de una aplicación de QnA Maker

QnA Maker permite colaborar con distintos autores y editores de contenido, ya que ofrece una funcionalidad para restringir el acceso de los colaboradores según el rol del colaborador.
Obtenga más información sobre los [conceptos de autenticación de colaboradores en QnA Maker](../Concepts/role-based-access-control.md).

También es posible mejorar la calidad de la base de conocimiento al sugerir preguntas alternativas a través del [aprendizaje activo](../Concepts/active-learning-suggestions.md). Los envíos de usuarios se tienen en cuenta y aparecen como sugerencias en la lista de preguntas alternativas. Tiene la flexibilidad de agregar esas sugerencias como preguntas alternativas o rechazarlas.

La base de conocimiento no cambia automáticamente. Debe aceptar las sugerencias para que cualquier cambio surta efecto. Estas sugerencias agregan preguntas, pero no cambian ni quitan preguntas existentes.

## <a name="add-azure-role-based-access-control-azure-rbac"></a>Adición del control de acceso basado en roles de Azure (Azure RBAC)

QnA Maker permite que varias personas colaboren en todas las bases de conocimiento del mismo recurso de QnA Maker. Esta característica se proporciona con el [control de acceso basado en roles de Azure (Azure RBAC)](../../../active-directory/role-based-access-control-configure.md).

## <a name="access-at-the-qna-maker-resource-level"></a>Acceso en el nivel de recursos de QnA Maker

No puede compartir una base de conocimiento particular en un servicio QnA Maker. Si desea un control de acceso más pormenorizado, considere la distribución de las bases de conocimiento entre diferentes recursos de QnA Maker y, a continuación, agregue los roles a cada recurso.

## <a name="add-a-role-to-a-resource"></a>Adición de un rol a un recurso

### <a name="add-a-user-account-to-the-qna-maker-resource"></a>Agregar una cuenta de usuario al recurso de QnA Maker

En los pasos siguientes se usa el rol de colaborador, pero cualquiera de los [roles](../reference-role-based-access-control.md) se puede agregar mediante estos pasos.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) y vaya al recurso de QnA Maker.

    ![Lista de recursos de QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.png)

1. Vaya a la pestaña **Access Control (IAM)** .

    ![QnA Maker (IAM)](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.png)

1. Seleccione **Agregar**.

    ![Agregar QnA Maker (IAM)](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.png)

1. Seleccione un rol en la lista siguiente:

    |Role|
    |--|
    |Propietario|
    |Colaborador|
    |Lector de QnA Maker de Cognitive Services|
    |Editor de QnA Maker de Cognitive Services|
    |Usuario de Cognitive Services|

    :::image type="content" source="../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-add-role-iam.png" alt-text="Adición de rol de QnA Maker (IAM).":::

1. Escriba la dirección de correo electrónico del usuario y presione **Guardar**.

    ![Agregar correo electrónico de QnA Maker (IAM)](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.png)

### <a name="view-qna-maker-knowledge-bases"></a>Visualización de knowledge bases de QnA Maker

Cuando la persona con quien comparte el servicio QnA Maker inicia sesión en el [portal de QnA Maker](https://qnamaker.ai), puede ver todas las bases de conocimiento de ese servicio según el rol.

Cuando esa persona selecciona una knowledge base, su rol actual en ese recurso de QnA Maker es visible junto al nombre de la knowledge base.

:::image type="content" source="../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-knowledge-base-role-name.png" alt-text="Captura de pantalla de knowledge base en modo de edición con el nombre de rol entre paréntesis junto al nombre de la knowledge base en la esquina superior izquierda de la página web.":::

## <a name="upgrade-runtime-version-to-use-active-learning"></a>Actualización de la versión en tiempo de ejecución para usar el aprendizaje activo

# <a name="qna-maker-ga-stable-release"></a>[Disponibilidad general de QnA Maker (versión estable)](#tab/v1)

El aprendizaje activo se admite en el tiempo de ejecución versión 4.4.0 y superior. Si la base de conocimiento se creó en una versión anterior, [actualice el tiempo de ejecución](set-up-qnamaker-service-azure.md#get-the-latest-runtime-updates) para usar esta característica.

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker administrado (versión preliminar)](#tab/v2)

En QnA Maker administrado (versión preliminar), como el runtime se hospeda en el propio servicio QnA Maker, no es necesario actualizarlo de forma manual.

---

## <a name="turn-on-active-learning-for-alternate-questions"></a>Activación del aprendizaje activo para preguntas alternativas

# <a name="qna-maker-ga-stable-release"></a>[Disponibilidad general de QnA Maker (versión estable)](#tab/v1)

El aprendizaje activo está desactivado de forma predeterminada. Actívelo para ver preguntas sugeridas. Una vez activado el aprendizaje activo, deberá enviar información desde la aplicación cliente a QnA Maker. Para obtener más información, consulte [Flujo arquitectónico para usar GenerateAnswer y Train API desde un bot](improve-knowledge-base.md#architectural-flow-for-using-generateanswer-and-train-apis-from-a-bot).

1. Seleccione **Publicar** para publicar la base de conocimiento. Las consultas de aprendizaje activo se recopilan solo desde el punto de conexión de predicción de GenerateAnswer API. Las consultas en el panel de prueba del portal de QnA Maker no afectarán al aprendizaje activo.

1. Para activar el aprendizaje activo en el portal de QnA Maker, vaya a la esquina superior derecha, seleccione su **Nombre** y vaya a [**Configuración del servicio**](https://www.qnamaker.ai/UserSettings).

    ![Active las preguntas sugeridas alternativas de aprendizaje activo desde la página Configuración del servicio. Seleccione su nombre de usuario en el menú superior derecho y seleccione Continuación del servicio.](../media/improve-knowledge-base/Endpoint-Keys.png)


1. Busque el servicio QnA Maker, y active **Aprendizaje activo**.

    > [!div class="mx-imgBorder"]
    > [![En la página Configuración del servicio, active la característica Aprendizaje activo. Si no es capaz de activar o desactivar la característica, deberá actualizar el servicio.](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png)](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png#lightbox)
    > [!Note]
    > La versión exacta de la imagen anterior se muestra solo como un ejemplo. Su versión puede ser diferente.
    Una vez que **Aprendizaje activo** está habilitado, la base de conocimiento sugiere nuevas preguntas a intervalos regulares según las preguntas enviadas por el usuario. Para deshabilitar **Aprendizaje activo**, vuelva a cambiar la configuración.

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker administrado (versión preliminar)](#tab/v2)

De forma predeterminada, el aprendizaje activo está **en funcionamiento** en QnA Maker administrado (versión preliminar). Para ver las preguntas alternativas sugeridas, [use las opciones de vista](../How-To/improve-knowledge-base.md#view-suggested-questions) en la página de edición.

---

## <a name="review-suggested-alternate-questions"></a>Revisión de preguntas alternativas sugeridas

[Revise las preguntas sugeridas alternativas](improve-knowledge-base.md) en la página **Editar** de cada base de conocimiento.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una base de conocimientos](./manage-knowledge-bases.md)