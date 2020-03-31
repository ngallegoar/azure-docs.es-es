---
title: 'Tutorial: Configuración de Smartsheet para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo configurar Azure Active Directory para aprovisionar y cancelar automáticamente el aprovisionamiento de cuentas de usuario de Smartsheet.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: na
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2019
ms.author: jeedes
ms.openlocfilehash: 9fbdf8a1c4b1881fc6dfd9d7b95a4103761e9ce7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063210"
---
# <a name="tutorial-configure-smartsheet-for-automatic-user-provisioning"></a>Tutorial: Configuración de Smartsheet para el aprovisionamiento automático de usuarios

El objetivo de este tutorial es mostrar los pasos que se realizan en Smartsheet y Azure Active Directory (Azure AD) para configurar Azure AD con el objetivo de aprovisionar y desaprovisionar automáticamente usuarios o grupos en Smartsheet.

> [!NOTE]
> Este tutorial describe un conector que se crea sobre el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Este conector está actualmente en versión preliminar pública. Para más información sobre los términos de uso generales de Microsoft Azure para las características en versión preliminar, consulte [Términos de uso complementarios para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Prerrequisitos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

* Un inquilino de Azure AD
* [Un inquilino de Smartsheet](https://www.smartsheet.com/pricing)
* Una cuenta de usuario en un plan de Smartsheet Enterprise o Premier Enterprise con permisos de administrador del sistema.

## <a name="assign-users-to-smartsheet"></a>Asignación de usuarios a Smartsheet

Azure Active Directory usa un concepto denominado *asignaciones* para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto del aprovisionamiento automático de usuarios, solo se sincronizan los usuarios y grupos que se han asignado a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, debe decidir qué usuarios o grupos de Azure AD necesitan acceder a Smartsheet. Una vez que lo decida, puede seguir estas instrucciones para asignar dichos usuarios o grupos a Smartsheet:

* [Asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-smartsheet"></a>Sugerencias importantes para asignar usuarios a Smartsheet

* Se recomienda asignar un único usuario de Azure AD a Smartsheet para probar la configuración de aprovisionamiento automático de usuarios. Más tarde, se pueden asignar otros usuarios o grupos.

* Al asignar un usuario a Smartsheet, debe seleccionar un rol válido específico de la aplicación (si está disponible) en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

* Para garantizar la paridad en las asignaciones de roles de usuario entre Smartsheet y Azure AD, se recomienda usar las mismas asignaciones de roles que se encuentran en la lista completa de usuarios de Smartsheet. Para recuperar esta lista de usuarios de Smartsheet, vaya a **Account Admin > User Management > More Actions > Download User List (csv)** (Administrador de cuenta > Administración de usuarios > Más acciones > Descargar lista de usuarios).

* Para acceder a ciertas características de la aplicación, Smartsheet requiere que un usuario tenga varios roles. Para obtener más información sobre los tipos de usuarios y permisos en Smartsheet, vaya a [User Types and Permissions](https://help.smartsheet.com/learning-track/shared-users/user-types-and-permissions) (Tipos de usuarios y permisos).

*  Si un usuario tiene varios roles asignados en Smartsheet, **DEBE** asegurarse de que estas asignaciones de roles se replican en Azure AD para evitar un escenario en el que los usuarios puedan perder el acceso a los objetos de Smartsheet de forma permanente. Cada rol único en Smartsheet **DEBE** asignarse a un grupo diferente en Azure AD. El usuario **DEBE** agregarse a cada uno de los grupos correspondientes a los roles deseados. 

## <a name="set-up-smartsheet-for-provisioning"></a>Configurar Smartsheet para el aprovisionamiento

Antes de configurar Smartsheet para el aprovisionamiento automático de usuarios con Azure AD, deberá habilitar el aprovisionamiento SCIM en Smartsheet.

1. Inicie sesión como **SysAdmin** en el **[portal de Smartsheet](https://app.smartsheet.com/b/home)** y vaya a **Administrador de cuenta**.

    ![Administrador de cuenta de Smartsheet](media/smartsheet-provisioning-tutorial/smartsheet-accountadmin.png)

2. Vaya a **Security Controls > User Auto Provisioning > Edit** (Controles de seguridad > Aprovisionamiento automático de usuarios > Editar).

    ![Controles de seguridad de Smartsheet](media/smartsheet-provisioning-tutorial/smartsheet-securitycontrols.png)

3. Agregue y valide los dominios de correo electrónico para los usuarios que planea aprovisionar desde Azure AD a Smartsheet. Elija **No habilitado** para asegurarse de que todas las acciones de aprovisionamiento solo se originen en Azure AD, y también para asegurarse de que su lista de usuarios de Smartsheet esté sincronizada con las asignaciones de Azure AD.

    ![Aprovisionamiento de usuarios de Smartsheet](media/smartsheet-provisioning-tutorial/smartsheet-userprovisioning.png)

4. Una vez que se complete la validación, deberá activar el dominio. 

    ![Dominio de activación de Smartsheet](media/smartsheet-provisioning-tutorial/smartsheet-activatedomain.png)

5. Genere el **token secreto** necesario para configurar el aprovisionamiento automático de usuarios con Azure AD; para ello, vaya a **Apps and Integrations** (Aplicaciones e integraciones).

    ![Instalación de Smartsheet](media/smartsheet-provisioning-tutorial/Smartsheet05.png)

6. Elija el **acceso a la API**. Haga clic en **Generate new access token** (Generar nuevo token de acceso).

    ![Instalación de Smartsheet](media/smartsheet-provisioning-tutorial/Smartsheet06.png)

7. Defina el nombre del token de acceso de la API. Haga clic en **OK**.

    ![Instalación de Smartsheet](media/smartsheet-provisioning-tutorial/Smartsheet07.png)

8. Copie el token de acceso de la API y guárdelo, ya que esta será la única vez que podrá verlo. Este se pedirá en el campo del **token secreto**  en Azure AD.

    ![Token de Smartsheet](media/smartsheet-provisioning-tutorial/Smartsheet08.png)

## <a name="add-smartsheet-from-the-gallery"></a>Agregar Smartsheet desde la galería

Para configurar Smartsheet para el aprovisionamiento automático de usuarios con Azure AD, es preciso agregar Smartsheet desde la galería de aplicaciones de Azure AD a la lista de aplicaciones SaaS administradas.

1. En **[Azure Portal](https://portal.azure.com)** , en el panel de navegación izquierdo, seleccione **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, seleccione el botón **Nueva aplicación** en la parte superior del panel.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Smartsheet** y seleccione **Smartsheet** en el panel de resultados. 

    ![Smartsheet en la lista de resultados](common/search-new-app.png)

5. Seleccione el botón **Sign-up for Smartsheet** (Registrarse en Smartsheet) que lo redireccionará a la página de inicio de sesión de Smartsheet. 

    ![Agregar Smartsheet OIDC](media/smartsheet-provisioning-tutorial/smartsheet-OIDC-add.png)

6. Como Smartsheet es una aplicación de OpenIDConnect, inicie sesión en Smartsheet con su cuenta profesional de Microsoft.

    ![Inicio de sesión en Smartsheet OIDC](media/smartsheet-provisioning-tutorial/smartsheet-OIDC-login.png)

7. Después de autenticarse, acepte la petición de consentimiento de la página de consentimiento. La aplicación se agregará automáticamente a su inquilino y será redirigido a su cuenta de Smartsheet.

    ![Consentimiento de Smartsheet OIDC](media/smartsheet-provisioning-tutorial/smartsheet-OIDC-consent.png)

## <a name="configure-automatic-user-provisioning-to-smartsheet"></a>Configurar el aprovisionamiento automático de usuarios para Smartsheet 

Esta sección le guiará por los pasos necesarios para configurar el servicio de aprovisionamiento de AD Azure para crear, actualizar y deshabilitar usuarios o grupos en Smartsheet en función de las asignaciones de grupos y usuarios de Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-smartsheet-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para Smartsheet en Azure AD, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Seleccione **Aplicaciones empresariales** y luego **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Smartsheet**.

    ![Vínculo a Smartsheet en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Pestaña Aprovisionamiento](common/provisioning.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Pestaña Aprovisionamiento](common/provisioning-automatic.png)

5. En la sección **Credenciales de administrador**, escriba `https://scim.smartsheet.com/v2/` en la **URL de inquilino**. Escriba el valor que recuperó y guardó anteriormente en Smartsheet en el **token secreto**. Haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a Smartsheet. Si la conexión no se establece, asegúrese de que la cuenta de Smartsheet tiene permisos de administrador del sistema e inténtelo de nuevo.

    ![Token](common/provisioning-testconnection-tenanturltoken.png)

6. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Correo electrónico de notificación](common/provisioning-notification-email.png)

7. Haga clic en **Save**(Guardar).

8. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to Smartsheet** (Sincronizar usuarios de Azure Active Directory con Smartsheet).

    ![Asignaciones de usuarios de Smartsheet](media/smartsheet-provisioning-tutorial/smartsheet-user-mappings.png)

9. Revise los atributos de usuario que se sincronizan entre Azure AD y Smartsheet en la sección **Attribute Mapping** (Asignación de atributos). Los atributos seleccionados como propiedades **Matching** se usan para buscar coincidencias con las cuentas de usuario de Smartsheet con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Atributos de usuario de Smartsheet](media/smartsheet-provisioning-tutorial/smartsheet-user-attributes.png)

10. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

11. Para habilitar el servicio de aprovisionamiento de Azure AD para Smartsheet, cambie el **estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Estado de aprovisionamiento activado](common/provisioning-toggle-on.png)

12. Elija los valores deseados en **Ámbito**, en la sección **Configuración**, para definir los usuarios o grupos que desea que se aprovisionen en Smartsheet.

    ![Ámbito del aprovisionamiento](common/provisioning-scope.png)

13. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Guardar la configuración de aprovisionamiento](common/provisioning-configuration-save.png)

Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. Puede usar la sección **Detalles de sincronización** para supervisar el progreso y seguir los vínculos al informe de actividad de aprovisionamiento, donde se describen todas las acciones que ha llevado a cabo el servicio de aprovisionamiento de Azure AD en Smartsheet.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte el tutorial de [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Limitaciones del conector

* Smartsheet no admite las eliminaciones temporales. Cuando el atributo **activo** de un usuario se establece en "False", Smartsheet elimina al usuario de forma permanente.

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../app-provisioning/check-status-user-account-provisioning.md)
