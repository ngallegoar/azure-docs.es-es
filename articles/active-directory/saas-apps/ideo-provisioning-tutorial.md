---
title: 'Tutorial: Configuración de IDEO para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Aprenda a configurar Azure Active Directory para aprovisionar y desaprovisionar automáticamente cuentas de usuario en IDEO.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: de4f06a3-83e9-46ce-80ee-03d706b91c81
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2019
ms.author: Zhchia
ms.openlocfilehash: f5f163109d648a4fc021b41325c6d585a5a7a3e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77057594"
---
# <a name="tutorial-configure-ideo-for-automatic-user-provisioning"></a>Tutorial: Configuración de IDEO para el aprovisionamiento automático de usuarios

El objetivo de este tutorial es mostrar los pasos que se realizan en IDEO y Azure Active Directory (Azure AD) para configurar Azure AD a fin de aprovisionar y desaprovisionar automáticamente usuarios o grupos en IDEO.

> [!NOTE]
> Este tutorial describe un conector que se crea sobre el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Este conector está actualmente en versión preliminar pública. Para más información sobre los términos de uso generales de Microsoft Azure para las características en versión preliminar, consulte [Términos de uso complementarios para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Prerrequisitos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

* Un inquilino de Azure AD
* [Un inquilino de IDEO](https://www.shape.space/product/pricing)
* Una cuenta de usuario en IDEO | Shape con permisos de administrador.

## <a name="assign-users-to-ideo"></a>Asignación de usuarios a IDEO

Azure Active Directory usa el concepto de asignación para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto del aprovisionamiento automático de usuarios, solo se sincronizan los usuarios y grupos que se han asignado a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, debe decidir qué usuarios o grupos de Azure AD necesitan acceder a IDEO. Una vez que lo decida, puede seguir estas instrucciones para asignar dichos usuarios o grupos a IDEO:

* [Asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-ideo"></a>Sugerencias importantes para asignar usuarios a IDEO

* Se recomienda asignar un único usuario de Azure AD a IDEO para probar la configuración de aprovisionamiento automático de usuarios. Más tarde, se pueden asignar otros usuarios o grupos.

* Al asignar un usuario a IDEO, debe seleccionar un rol válido específico de la aplicación (si está disponible) en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="set-up-ideo-for-provisioning"></a>Configuración de IDEO para el aprovisionamiento

Antes de configurar IDEO para el aprovisionamiento automático de usuarios con Azure AD, debe recuperar alguna información de aprovisionamiento de IDEO.

1. Para el **Token secreto**, ponte en contacto con el equipo de soporte técnico de IDEO en productsupport@ideo.com. Este valor se escribirá en el campo **Token secreto** de la pestaña Aprovisionamiento de la aplicación IDEO en Azure Portal. 

## <a name="add-ideo-from-the-gallery"></a>Adición de IDEO desde la galería

Para configurar IDEO para el aprovisionamiento automático de usuarios con Azure AD, es preciso agregar IDEO desde la galería de aplicaciones de Azure AD a la lista de aplicaciones SaaS administradas.

1. En **[Azure Portal](https://portal.azure.com)** , en el panel de navegación izquierdo, seleccione **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, seleccione el botón **Nueva aplicación** en la parte superior del panel.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **IDEO** y seleccione **IDEO** en el panel de resultados. 

    ![IDEO en la lista de resultados](common/search-new-app.png)

5. Seleccione el botón **Registrarse en IDEO**; se le redirigirá a la página de inicio de sesión de IDEO. 

    ![Incorporación de OIDC IDEO](media/ideo-provisioning-tutorial/signup.png)

6. Dado que IDEO es una aplicación de OpenIDConnect, inicie sesión en IDEO con su cuenta profesional de Microsoft.

    ![Inicio de sesión de OIDC IDEO](media/ideo-provisioning-tutorial/login.png)

7. Después de autenticarse, acepte la petición de consentimiento de la página de consentimiento. La aplicación se agregará automáticamente a su inquilino y será redirigido a su cuenta de IDEO.

    ![Consentimiento de OIDC IDEO](media/ideo-provisioning-tutorial/consent.png)

## <a name="configure-automatic-user-provisioning-to-ideo"></a>Configuración del aprovisionamiento automático de usuarios en IDEO 

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de Azure AD para crear, actualizar y deshabilitar usuarios o grupos en IDEO en función de las asignaciones de grupos y usuarios de Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-ideo-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para IDEO en Azure AD:

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Seleccione **Aplicaciones empresariales** y luego **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **IDEO**.

    ![Vínculo a IDEO en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Pestaña Aprovisionamiento](common/provisioning.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Pestaña Aprovisionamiento](common/provisioning-automatic.png)

5. En la sección **Credenciales de administrador**, escriba `https://profile.ideo.com/api/scim/v2` en la **URL de inquilino**. Escriba en **Token secreto** el valor que recuperó del equipo de soporte técnico de IDEO. Haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a IDEO. Si la conexión no se establece, asegúrese de que la cuenta de IDEO tenga permisos de administrador y pruebe de nuevo.

    ![URL de inquilino + Token](common/provisioning-testconnection-tenanturltoken.png)

6. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Correo electrónico de notificación](common/provisioning-notification-email.png)

7. Haga clic en **Save**(Guardar).

8. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to IDEO** (Sincronizar usuarios de Azure Active Directory con IDEO).

    ![Asignaciones de usuario de IDEO](media/ideo-provisioning-tutorial/usermappings.png)

9. Examine los atributos de grupo que se sincronizan entre Azure AD e IDEO en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de IDEO con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Atributos de usuario de IDEO](media/ideo-provisioning-tutorial/userattributes.png)

10. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

11. Para habilitar el servicio de aprovisionamiento de Azure AD para IDEO, cambie el **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Estado de aprovisionamiento activado](media/ideo-provisioning-tutorial/groupmappings.png)

12. Elija los valores deseados en **Ámbito**, en la sección **Configuración**, para definir los usuarios o grupos que desea que se aprovisionen en IDEO.

    ![Ámbito del aprovisionamiento](media/ideo-provisioning-tutorial/groupattributes.png)

13. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Guardar la configuración de aprovisionamiento](common/provisioning-configuration-save.png)

Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. Puede usar la sección **Detalles de sincronización** para supervisar el progreso y seguir los vínculos al informe de actividad de aprovisionamiento, donde se describen todas las acciones que ha llevado a cabo el servicio de aprovisionamiento de Azure AD en IDEO.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte el tutorial de [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../app-provisioning/check-status-user-account-provisioning.md)


