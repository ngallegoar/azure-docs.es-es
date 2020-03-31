---
title: 'Tutorial: Configuración de Snowflake para aprovisionar usuarios automáticamente con Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo configurar Azure Active Directory para aprovisionar y cancelar automáticamente el aprovisionamiento de cuentas de usuario de Snowflake.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: f9ce85f4-0992-4bc6-8276-4b2efbce8dcb
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: 2c5d91894ba35233f3fbebffdff9104edcfdd27b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063170"
---
# <a name="tutorial-configure-snowflake-for-automatic-user-provisioning"></a>Tutorial: Configuración de Snowflake para el aprovisionamiento automático de usuarios

El objetivo de este tutorial es mostrar los pasos que se realizan en Snowflake y Azure Active Directory (Azure AD) para configurar Azure AD a fin de aprovisionar y cancelar automáticamente el aprovisionamiento de usuarios o grupos en Snowflake.

> [!NOTE]
> Este tutorial describe un conector que se crea sobre el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Este conector está actualmente en versión preliminar pública. Para más información sobre los términos de uso generales de Microsoft Azure para las características en versión preliminar, consulte [Términos de uso complementarios para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Prerrequisitos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

* Un inquilino de Azure AD.
* [Un inquilino de Snowflake](https://www.Snowflake.com/pricing/).
* Una cuenta de usuario de Snowflake con permisos de administrador.

## <a name="assigning-users-to-snowflake"></a>Asignación de usuarios a Snowflake

Azure Active Directory usa un concepto denominado *asignaciones* para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto del aprovisionamiento automático de usuarios, solo se sincronizan los usuarios y grupos que se han asignado a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, debe decidir qué usuarios o grupos de Azure AD necesitan acceder a Snowflake. Una vez que lo decida, puede seguir estas instrucciones para asignar dichos usuarios o grupos a Snowflake:
* [Asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-snowflake"></a>Sugerencias importantes para asignar usuarios a Snowflake

* Se recomienda asignar un solo usuario de Azure AD a Snowflake para probar la configuración del aprovisionamiento automático de usuarios. Más tarde, se pueden asignar otros usuarios o grupos.

* Al asignar un usuario a Snowflake, debe seleccionar un rol válido específico de la aplicación (si está disponible) en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="setup-snowflake-for-provisioning"></a>Configuración de Snowflake para el aprovisionamiento

Antes de configurar Snowflake para aprovisionar automáticamente usuarios con Azure AD, deberá habilitar el aprovisionamiento SCIM en Snowflake.

1. Inicie sesión en la consola de administración de Snowflake. Escriba la consulta que se muestra a continuación en la hoja de cálculo resaltada y haga clic en **Ejecutar**.

    ![Consola de administración de Snowflake](media/Snowflake-provisioning-tutorial/image00.png)

2.  Se generará un token de acceso de SCIM para el inquilino de Snowflake. Para recuperarlo, haga clic en el vínculo resaltado a continuación.

    ![Snowflake, agregar SCIM](media/Snowflake-provisioning-tutorial/image01.png)

3. Copie el valor del token generado y haga clic en **Done** (Listo). Este valor se escribe en el campo **Secret Token** (Token secreto) de la pestaña Provisioning (Aprovisionamiento) de la aplicación Snowflake en Azure Portal.

    ![Snowflake, agregar SCIM](media/Snowflake-provisioning-tutorial/image02.png)

## <a name="add-snowflake-from-the-gallery"></a>Adición de Snowflake desde la galería

Para configurar Snowflake para el aprovisionamiento automático de usuarios con Azure AD, es preciso agregar Snowflake desde la galería de aplicaciones de Azure AD a la lista de aplicaciones SaaS administradas.

**Para agregar Snowflake desde la galería de aplicaciones de Azure AD, siga estos pasos:**

1. En **[Azure Portal](https://portal.azure.com)** , en el panel de navegación izquierdo, seleccione **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, seleccione el botón **Nueva aplicación** en la parte superior del panel.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Snowflake**, seleccione **Snowflake** en el panel de resultados y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Snowflake en la lista de resultados](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-snowflake"></a>Configuración del aprovisionamiento automático de usuarios en Snowflake 

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de Azure AD para crear, actualizar y deshabilitar usuarios o grupos en Snowflake en función de las asignaciones de grupos y usuarios de Azure AD.

> [!TIP]
> También puede optar por habilitar el inicio de sesión único basado en SAML para Snowflake siguiendo las instrucciones del [tutorial de inicio de sesión único de Snowflake](Snowflake-tutorial.md). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático de usuarios, aunque estas dos características se complementan entre sí.

### <a name="to-configure-automatic-user-provisioning-for-snowflake-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para Snowflake en Azure AD, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Seleccione **Aplicaciones empresariales** y luego **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Snowflake**.

    ![Vínculo a Snowflake en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Pestaña Aprovisionamiento](common/provisioning.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Pestaña Aprovisionamiento](common/provisioning-automatic.png)

5. En la sección Credenciales de administrador, escriba `https://<Snowflake Account URL>/scim/v2` en la URL de inquilino. Un ejemplo de la dirección URL del inquilino es: `https://acme.snowflakecomputing.com/scim/v2`

6. Escriba el valor **SCIM Authentication Token** (Token de autenticación de SCIM) recuperado anteriormente en **Token secreto**. Haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a Snowflake. Si la conexión no se establece, asegúrese de que la cuenta de Snowflake tiene permisos de administrador e inténtelo de nuevo.

    ![URL de inquilino + Token](common/provisioning-testconnection-tenanturltoken.png)

7. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Correo electrónico de notificación](common/provisioning-notification-email.png)

8. Haga clic en **Save**(Guardar).

9. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to Snowflake** (Sincronizar usuarios de Azure Active Directory con Snowflake).

    ![Asignaciones de usuarios de Snowflake](media/Snowflake-provisioning-tutorial/user-mapping.png)

10. Revise los atributos de usuario que se sincronizan entre Azure AD y Snowflake en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Snowflake con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Atributos de usuario de Snowflake](media/Snowflake-provisioning-tutorial/user-attribute.png)

11. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Groups to Snowflake** (Sincronizar grupos de Azure Active Directory con Snowflake).

    ![Asignaciones de grupos de Snowflake](media/Snowflake-provisioning-tutorial/group-mapping.png)

12. Revise los atributos de grupo que se sincronizan entre Azure AD y Snowflake en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con los grupos de Snowflake con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Atributos de grupos de Snowflake](media/Snowflake-provisioning-tutorial/group-attribute.png)

13. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

14. Para habilitar el servicio de aprovisionamiento de Azure AD para Snowflake, cambie el **estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Estado de aprovisionamiento activado](common/provisioning-toggle-on.png)

15. Elija los valores deseados en **Ámbito**, en la sección **Configuración**, para definir los usuarios o grupos que desea que se aprovisionen en Snowflake. Si esta opción no está disponible, configure los campos obligatorios en Credenciales de administrador, haga clic en **Guardar** y actualice la página. 

    ![Ámbito del aprovisionamiento](common/provisioning-scope.png)

16. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Guardar la configuración de aprovisionamiento](common/provisioning-configuration-save.png)

    Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. Puede usar la sección **Detalles de sincronización** para supervisar el progreso y seguir los vínculos al informe de actividad de aprovisionamiento, donde se describen todas las acciones que ha llevado a cabo el servicio de aprovisionamiento de Azure AD en Snowflake.

    Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Limitaciones del conector

* Los tokens de SCIM que genera Snowflake expiran en 6 meses. Tenga en cuenta que deben actualizarse antes de que expiren para que las sincronizaciones de aprovisionamiento sigan funcionando. 

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../app-provisioning/configure-automatic-user-provisioning-portal.md).
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes
* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../app-provisioning/check-status-user-account-provisioning.md).
