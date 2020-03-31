---
title: 'Tutorial: Configuración de 15Five para aprovisionar usuarios automáticamente con Azure Active Directory | Microsoft Docs'
description: Aprenda a configurar Azure Active Directory para aprovisionar y desaprovisionar automáticamente cuentas de usuario en 15Five.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: a276c004-9f71-4efc-8cca-1f615760249f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: f1f66a7b69048180bc41c8f2fa432598f00f7f09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77059324"
---
# <a name="tutorial-configure-15five-for-automatic-user-provisioning"></a>Tutorial: Configuración de 15Five para aprovisionar usuarios automáticamente

El objetivo de este tutorial es mostrar los pasos que se realizan en 15Five y Azure Active Directory (Azure AD) para configurar Azure AD a fin de aprovisionar y cancelar automáticamente el aprovisionamiento de usuarios o grupos en 15Five.

> [!NOTE]
> Este tutorial describe un conector que se crea sobre el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Este conector está actualmente en versión preliminar pública. Para más información sobre los términos de uso generales de Microsoft Azure para las características en versión preliminar, consulte [Términos de uso complementarios para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Prerrequisitos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

* Un inquilino de Azure AD.
* [Un inquilino de 15Five](https://www.15five.com/pricing/).
* Una cuenta de usuario de 15Five con permisos de administrador.

## <a name="assigning-users-to-15five"></a>Asignación de usuarios a 15Five

Azure Active Directory usa un concepto denominado *asignaciones* para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto del aprovisionamiento automático de usuarios, solo se sincronizan los usuarios y grupos que se han asignado a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, debe decidir qué usuarios o grupos de Azure AD necesitan acceder a 15Five. Una vez que lo decida, puede seguir estas instrucciones para asignar dichos usuarios o grupos a 15Five:
* [Asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-15five"></a>Sugerencias importantes para asignar usuarios a 15Five

* Se recomienda asignar un único usuario de Azure AD a 15Five para probar la configuración de aprovisionamiento automático de usuarios. Más tarde, se pueden asignar otros usuarios o grupos.

* Al asignar un usuario a 15Five, debe seleccionar un rol válido específico de la aplicación (si está disponible) en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="setup-15five-for-provisioning"></a>Configuración de 15Five para el aprovisionamiento

Antes de configurar 15Five para el aprovisionamiento automático de usuarios con Azure AD, deberá habilitar el aprovisionamiento SCIM en 15Five.

1. Inicie sesión en la [consola de administración de 15Five](https://my.15five.com/). Vaya a **Features > Integrations** (Características > Integraciones).

    ![Consola de administración de 15Five](media/15five-provisioning-tutorial/integration.png)

2.  Haga clic en **SCIM 2.0**.

    ![Consola de administración de 15Five](media/15five-provisioning-tutorial/image00.png)

3.  Vaya a **SCIM integration > Generate OAuth token** (Integración de SCIM > Generar token de OAuth).

    ![SCIM de incorporación de 15Five](media/15five-provisioning-tutorial/image02.png)

4.  Copie los valores de **URL base de SCIM 2.0** y **token de acceso**. Estos valores se escriben en el campo **URL de inquilino** y **Token secreto** de la pestaña Aprovisionamiento de la aplicación 15Five en Azure Portal.
    
    ![SCIM de incorporación de 15Five](media/15five-provisioning-tutorial/image03.png)

## <a name="add-15five-from-the-gallery"></a>Adición de 15Five desde la galería

Para configurar 15Five para el aprovisionamiento automático de usuarios con Azure AD, es preciso agregar 15Five desde la galería de aplicaciones de Azure AD a la lista de aplicaciones SaaS administradas.

**Para agregar 15Five desde la galería de aplicaciones de Azure AD, siga estos pasos:**

1. En **[Azure Portal](https://portal.azure.com)** , en el panel de navegación izquierdo, seleccione **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, seleccione el botón **Nueva aplicación** en la parte superior del panel.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **15Five**, seleccione **15Five** en el panel de resultados y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![15Five en la lista de resultados](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-15five"></a>Configuración del aprovisionamiento automático de usuarios en 15Five 

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de Azure AD para crear, actualizar y deshabilitar usuarios o grupos en 15Five en función de las asignaciones de grupos y usuarios de Azure AD.

> [!TIP]
> También puede optar por habilitar el inicio de sesión único basado en SAML para 15Five siguiendo las instrucciones del [tutorial de inicio de sesión único de 15Five](15five-tutorial.md). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático de usuarios, aunque estas dos características se complementan entre sí.

### <a name="to-configure-automatic-user-provisioning-for-15five-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para 15Five en Azure AD:

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Seleccione **Aplicaciones empresariales** y luego **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **15Five**.

    ![Vínculo a 15Five en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Pestaña Aprovisionamiento](common/provisioning.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Pestaña Aprovisionamiento](common/provisioning-automatic.png)

5.  En la sección Credenciales de administrador, escriba los valores de **URL base de SCIM 2.0 y token de acceso** que recuperó anteriormente en **URL de inquilino** y **Token secreto** respectivamente. Haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a 15Five. Si la conexión no se establece, asegúrese de que la cuenta de 15Five tiene permisos de administrador y pruebe otra vez.

    ![URL de inquilino + Token](common/provisioning-testconnection-tenanturltoken.png)

6. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Correo electrónico de notificación](common/provisioning-notification-email.png)

7. Haga clic en **Save**(Guardar).

8. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to 15Five** (Sincronizar usuarios de Azure Active Directory con 15Five).

    ![Asignaciones de usuario de 15Five](media/15five-provisioning-tutorial/usermapping.png)

9. Examine los atributos de usuario que se sincronizan entre Azure AD y 15Five en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de 15Five con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Atributos de usuario de 15Five](media/15five-provisioning-tutorial/userattribute.png)

10. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Groups to 15Five** (Sincronizar grupos de Azure Active Directory con 15Five).

    ![Asignaciones de grupos de 15Five](media/15five-provisioning-tutorial/groupmapping.png)

11. Examine los atributos de grupo que se sincronizan entre Azure AD y 15Five en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades **Coincidentes** se usan para establecer correspondencia con las cuentas del usuario en 15Five a fin de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Atributos de grupos de 15Five](media/15five-provisioning-tutorial/groupattribute.png)

12. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

13. Para habilitar el servicio de aprovisionamiento de Azure AD para 15Five, cambie el **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Estado de aprovisionamiento activado](common/provisioning-toggle-on.png)

14. Elija los valores deseados en **Ámbito**, en la sección **Configuración**, para definir los usuarios o grupos que desea que se aprovisionen en 15Five.

    ![Ámbito del aprovisionamiento](common/provisioning-scope.png)

15. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Guardar la configuración de aprovisionamiento](common/provisioning-configuration-save.png)

    Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. Puede usar la sección **Detalles de sincronización** para supervisar el progreso y seguir los vínculos al informe de actividad de aprovisionamiento, donde se describen todas las acciones que ha llevado a cabo el servicio de aprovisionamiento de Azure AD en 15Five.

    Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../app-provisioning/check-status-user-account-provisioning.md).
    
## <a name="connector-limitations"></a>Limitaciones del conector

* 15Five no admite las eliminaciones permanentes de los usuarios.

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../app-provisioning/configure-automatic-user-provisioning-portal.md).
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../app-provisioning/check-status-user-account-provisioning.md).
