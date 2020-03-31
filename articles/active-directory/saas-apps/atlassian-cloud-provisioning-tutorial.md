---
title: 'Tutorial: Configuración de Atlassian Cloud para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Aprenda a configurar Azure Active Directory para aprovisionar y desaprovisionar automáticamente las cuentas de usuario para Atlassian Cloud.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 12/27/2019
ms.author: jeedes
ms.openlocfilehash: 7ddccef00cf1b5ad524c0e1eaa7aed52c0e55197
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77059375"
---
# <a name="tutorial-configure-atlassian-cloud-for-automatic-user-provisioning"></a>Tutorial: Configuración de Atlassian Cloud para el aprovisionamiento automático de usuarios

El objetivo de este tutorial es mostrar los pasos que se realizan en Atlassian Cloud y Azure Active Directory (Azure AD) para configurar Azure AD con el objetivo de aprovisionar y desaprovisionar automáticamente usuarios o grupos en Atlassian Cloud.

> [!NOTE]
> Este tutorial describe un conector que se crea sobre el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md).

## <a name="prerequisites"></a>Prerrequisitos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

* Un inquilino de Azure AD
* [Un inquilino de Atlassian Cloud](https://www.atlassian.com/licensing/cloud)
* Una cuenta de usuario de Atlassian Cloud con permisos de administrador.

> [!NOTE]
> La integración del aprovisionamiento de Azure AD usa **SCIM API de Atlassian Cloud**, que está disponible para los equipos de Atlassian Cloud.

## <a name="add-atlassian-cloud-from-the-gallery"></a>Agregar Atlassian Cloud desde la galería

Antes de configurar Atlassian Cloud para el aprovisionamiento automático de usuarios con Azure AD, es preciso agregar Atlassian Cloud desde la galería de aplicaciones de Azure AD a la lista de aplicaciones SaaS administradas.

**Para agregar Atlassian Cloud desde la galería de aplicaciones de Azure AD, siga estos pasos:**

1. En **[Azure Portal](https://portal.azure.com)** , en el panel de navegación izquierdo, seleccione **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, seleccione el botón **Nueva aplicación** en la parte superior del panel.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Atlassian Cloud**, seleccione **Atlassian Cloud** en el panel de resultados y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Atlassian Cloud en la lista de resultados](common/search-new-app.png)

## <a name="assigning-users-to-atlassian-cloud"></a>Asignación de usuarios a Atlassian Cloud

Azure Active Directory usa un concepto denominado *asignaciones* para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto del aprovisionamiento automático de usuarios, solo se sincronizan los usuarios y grupos que se han asignado a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, debe decidir qué usuarios o grupos de Azure AD necesitan acceder a Atlassian Cloud. Una vez que lo decida, puede seguir estas instrucciones para asignar dichos usuarios o grupos a Atlassian Cloud:

* [Asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-atlassian-cloud"></a>Sugerencias importantes para asignar usuarios a Atlassian Cloud

* Se recomienda asignar un único usuario de Azure AD a Atlassian Cloud para probar la configuración del aprovisionamiento automático de usuarios. Más tarde, se pueden asignar otros usuarios o grupos.

* Al asignar un usuario a Atlassian Cloud, debe seleccionar un rol válido específico de la aplicación (si está disponible) en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="configuring-automatic-user-provisioning-to-atlassian-cloud"></a>Configurar el aprovisionamiento automático de usuarios en Atlassian Cloud 

Esta sección le guiará por los pasos necesarios para configurar el servicio de aprovisionamiento de AD Azure para crear, actualizar y deshabilitar usuarios o grupos en Atlassian Cloud en función de las asignaciones de grupos y usuarios de Azure AD.

> [!TIP]
> También puede optar por habilitar el inicio de sesión único basado en SAML para Atlassian Cloud siguiendo las instrucciones del [tutorial de inicio de sesión único de Atlassian Cloud](atlassian-cloud-tutorial.md). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático de usuarios, aunque estas dos características se complementan entre sí.

### <a name="to-configure-automatic-user-provisioning-for-atlassian-cloud-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para Atlassian Cloud en Azure AD, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y, a continuación, **Atlassian Cloud**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Atlassian Cloud**.

    ![Vínculo de Atlassian Cloud en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/provisioning-tab.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/credentials.png)

5. Navegue a [Atlassian Organization Manager](https://admin.atlassian.com) **> seleccione la organización > directorio**.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/select-directory.png)

6. Haga clic en **Aprovisionamiento de usuarios** y en **Crear un directorio**. Copie los valores de **Directory base URL** (URL del directorio base) y **Bearer Token** (Token de portador) en los campos **URL del inquilino** y **Token secreto** respectivamente.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/secret-token-1.png) ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/secret-token-2.png) ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/secret-token-3.png)

7. En la sección **Credenciales de administrador**, escriba **URL de inquilino** y **Token secreto** de la cuenta de Atlassian Cloud. Algunos ejemplos de estos valores son:

   * En el campo **URL del inquilino**, rellene el punto de conexión del inquilino específico recibido de Atlassian, tal como se describe en el paso 6. Por ejemplo: `https://api.atlassian.com/scim/directory/{directoryId}`.

   * En el campo **Token secreto**, rellene el token secreto tal y como se describe en el paso 6.

8. Tras rellenar los campos que se muestran en el paso 7, haga clic en **Probar conexión** para asegurarse de que Azure AD pueda conectarse a Atlassian Cloud. Si la conexión no se establece, asegúrese de que la cuenta de Atlassian Cloud tiene permisos de administrador y pruébelo de nuevo.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/test-connection.png)

9. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/notification.png)

10. Haga clic en **Save**(Guardar).

11. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to Atlassian Cloud** (Sincronizar usuarios de Azure Active Directory con Atlassian Cloud).

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/provision-users.png)

12. Revise los atributos de usuario que se sincronizan entre Azure AD y Atlassian Cloud en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Atlassian Cloud con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/user-mapping.png)

13. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Groups to Atlassian Cloud** (Sincronizar grupos de Azure Active Directory con Atlassian Cloud).

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/provision-groups.png)

14. Revise los atributos de grupo que se sincronizan entre Azure AD y Atlassian Cloud en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con los grupos de Atlassian Cloud para las operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/group-mapping.png)

15. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

16. Para habilitar el servicio de aprovisionamiento de Azure AD para Atlassian Cloud, cambie **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/provisioning-on.png)

17. Elija los valores deseados en **Ámbito**, en la sección **Configuración**, para definir los usuarios o grupos que desea que se aprovisionen en Atlassian Cloud.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/provisioning-options.png)

18. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/save.png)

Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. Puede usar la sección **Detalles de sincronización** para supervisar el progreso y seguir los vínculos al informe de actividad de aprovisionamiento, donde se describen todas las acciones que ha llevado a cabo el servicio de aprovisionamiento de Azure AD en Atlassian Cloud.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte el tutorial de [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Limitaciones del conector

* Atlassian Cloud permite el aprovisionamiento de usuarios solo de [dominios comprobados](https://confluence.atlassian.com/cloud/organization-administration-938859734.html).
* Atlassian Cloud no admite cambios de nombre de grupo actualmente. Esto significa que los cambios realizados en la propiedad displayName de un grupo en Azure AD no se actualizará ni reflejará en Atlassian Cloud.
* El valor del atributo de usuario **mail** de Azure AD solo se rellena si el usuario tiene un buzón de Microsoft Exchange. Si el usuario no tiene ninguno, se recomienda asignar un atributo diferente al atributo **emails** en Atlassian Cloud.

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-03.png