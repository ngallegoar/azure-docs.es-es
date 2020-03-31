---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
author: rolyon
ms.service: active-directory
ms.topic: include
ms.date: 04/29/2019
ms.author: rolyon
ms.custom: include file
ms.openlocfilehash: d791c4ba46587ac5709d72cb31bc76f087118b03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67476244"
---
## <a name="create-one-or-more-access-reviews"></a>Creación de una o varias revisiones de acceso

1. Haga clic en **Nuevo** para crear una revisión de acceso nueva.

1. Ponga un nombre a la revisión de acceso. Opcionalmente, asigne a la revisión una descripción. El nombre y la descripción se muestran a los revisores.

    ![Creación de una revisión de acceso: nombre y descripción de la revisión](./media/active-directory-privileged-identity-management-access-reviews/name-description.png)

1. Establezca un valor para **Fecha de inicio**. De forma predeterminada, una revisión de acceso ocurre una vez, se inicia a la misma hora en que se crea y finaliza en un mes. Puede cambiar las fechas de inicio y de finalización para hacer que una revisión de acceso se inicie en el futuro, transcurridos tantos días como desee.

    ![Fecha de inicio, frecuencia, duración, finalización, número de veces y fecha de finalización](./media/active-directory-privileged-identity-management-access-reviews/start-end-dates.png)

1. Para realizar que la revisión de acceso sea periódica, cambie la opción **Frecuencia** de **Una vez** a **Semanal**, **Mensual**,  **Trimestral**, **Anual** o **Semestral**. Use el control deslizante o el cuadro de texto **Duración** para definir cuántos días se abrirá cada revisión de la serie periódica para que los revisores escriban datos. Por ejemplo, la duración máxima que puede establecer para una revisión mensual es 27 días, con el fin de evitar la superposición de revisiones.

1. Use el valor **Fin** para especificar cómo finalizar la serie de revisión de acceso periódica. La serie puede terminar de tres formas: se ejecuta continuamente para iniciar revisiones indefinidamente, hasta una fecha concreta, o hasta que se haya completado un número definido de veces. Un administrador de usuarios o un administrador de empresa puede detener la serie después de su creación cambiando la fecha en **Configuración**, de manera que termine en esa fecha.

1. En la sección **Usuarios**, seleccione uno o varios roles cuya pertenencia quiere revisar.

    ![Ámbito de los usuarios para revisar la pertenencia a rol de](./media/active-directory-privileged-identity-management-access-reviews/users.png)

    > [!NOTE]
    > Si selecciona más de un rol, se crearán varias revisiones de acceso. Por ejemplo, al seleccionar cinco roles, se crearán cinco revisiones de acceso independientes.

    Si va a crear una revisión de acceso de los roles de Azure AD, a continuación se muestra un ejemplo de la lista de revisión de pertenencia.

    ![Panel para la revisión de pertenencia que muestra los roles de Azure AD que puede seleccionar](./media/active-directory-privileged-identity-management-access-reviews/review-membership.png)

    Si va a crear una revisión de acceso de los roles de recursos de Azure, a continuación se muestra un ejemplo de la lista de revisión de pertenencia.

    ![Panel para la revisión de pertenencia que muestra los recursos de Azure que puede seleccionar](./media/active-directory-privileged-identity-management-access-reviews/review-membership-azure-resource-roles.png)

1. En la sección **Revisores**, seleccione una o más personas para que revisen a todos los usuarios. También puede seleccionar que los miembros revisen su propio acceso.

    ![Lista de los revisores de los usuarios o miembros (por sí mismos) seleccionados](./media/active-directory-privileged-identity-management-access-reviews/reviewers.png)

    - **Usuarios seleccionados**: use esta opción cuando no sepa quién necesita acceso. Con esta opción, puede asignar la revisión a un propietario de recursos o al administrador de grupos.
    - **Miembros (por sí mismos)** : use esta opción para hacer que los usuarios revisen sus propias asignaciones de roles.

### <a name="upon-completion-settings"></a>Configuración de finalización

1. Para especificar lo que sucede una vez finalizada una revisión, expanda la sección **Configuración de finalización**.

    ![Al terminar, las opciones Aplicar automáticamente y Debe revisar no responden.](./media/active-directory-privileged-identity-management-access-reviews/upon-completion-settings.png)

1. Si desea quitar automáticamente el acceso para los usuarios que se han denegado, establezca **Aplicar automáticamente los resultados al recurso** en **Habilitar**. Si desea aplicar manualmente los resultados cuando se complete la revisión, establezca el conmutador en **Deshabilitar**.

1. Use la lista **Si el revisor no responde** para especificar lo que ocurre con los usuarios a los que el revisor no ha revisado dentro del período de revisión. Este valor no afecta a los usuarios que los revisores revisaron manualmente. Si la decisión final del revisor es Denegar, se quitará el acceso del usuario.

    - **Sin cambios**: dejar el acceso del usuario sin cambios
    - **Quitar acceso**: quitar el acceso del usuario
    - **Aprobar acceso**: aprobar el acceso del usuario
    - **Aceptar recomendaciones**: aceptar la recomendación del sistema sobre la denegación o aprobación del acceso continuo del usuario

### <a name="advanced-settings"></a>Configuración avanzada

1. Para especificar configuraciones adicionales, expanda la sección **Configuración avanzada**.

    ![Opciones avanzadas para mostrar recomendaciones, requerir el motivo de la aprobación, notificaciones por correo y recordatorios.](./media/active-directory-privileged-identity-management-access-reviews/advanced-settings.png)

1. Establezca **Mostrar recomendaciones** en **Habilitar** para mostrar las recomendaciones del sistema de los revisores según la información del acceso del usuario.

1. Establezca **Requerir motivo de la aprobación** en **Habilitar** para requerir que el revisor proporcione un motivo para la aprobación.

1. Establezca **Notificaciones de correo** en **Habilitar**para que Azure AD envíe notificaciones de correo electrónico a los revisores cuando se inicia una revisión de acceso y a los administradores cuando se complete.

1. Establezca **Avisos** en **Habilitar** para que Azure AD envíe recordatorios de revisiones de acceso en curso a los revisores que no hayan completado su revisión.
