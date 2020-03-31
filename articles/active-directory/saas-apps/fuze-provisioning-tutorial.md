---
title: 'Tutorial: Configuración de Fuze para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Aprenda a configurar Azure Active Directory para aprovisionar y cancelar automáticamente el aprovisionamiento de cuentas de usuario en Fuze.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 34718201-4f0e-4260-9af0-b3b70a1e8265
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: a58402297380116f83214e52ae7f2796412755b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77057917"
---
# <a name="tutorial-configure-fuze-for-automatic-user-provisioning"></a>Tutorial: Configuración de Fuze para aprovisionar usuarios automáticamente

El objetivo de este tutorial es mostrar los pasos que se realizan en Fuze y Azure Active Directory (Azure AD) para configurar Azure AD a fin de aprovisionar y cancelar automáticamente el aprovisionamiento de usuarios o grupos en Fuze.

> [!NOTE]
> Este tutorial describe un conector que se crea sobre el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Este conector está actualmente en versión preliminar pública. Para más información sobre los términos de uso generales de Microsoft Azure para las características en versión preliminar, consulte [Términos de uso complementarios para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Prerrequisitos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

* Un inquilino de Azure AD.
* [Un inquilino de Fuze](https://www.fuze.com/).
* Una cuenta de usuario de Fuze con permisos de administrador.

## <a name="assigning-users-to-fuze"></a>Asignación de usuarios a Fuze

Azure Active Directory usa un concepto denominado *asignaciones* para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto del aprovisionamiento automático de usuarios, solo se sincronizan los usuarios y grupos que se han asignado a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, debe decidir qué usuarios o grupos de Azure AD necesitan acceder a Fuze. Una vez que lo decida, puede seguir estas instrucciones para asignar dichos usuarios o grupos a Fuze:

* [Asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-fuze"></a>Sugerencias importantes para asignar usuarios a Fuze

* Se recomienda asignar un único usuario de Azure AD a Fuze para probar la configuración de aprovisionamiento automático de usuarios. Más tarde, se pueden asignar otros usuarios o grupos.

* Al asignar un usuario a Fuze, debe seleccionar un rol válido específico de la aplicación (si está disponible) en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="setup-fuze-for-provisioning"></a>Configuración de Fuze para el aprovisionamiento

Antes de configurar Fuze para el aprovisionamiento automático de usuarios con Azure AD, deberá habilitar el aprovisionamiento SCIM en Fuze. 

1. Para empezar, póngase en contacto con su representante de Fuze y solicítele la siguiente información:

    * Lista de SKU de productos de Fuze en uso en su empresa.
    * Lista de códigos de ubicación de las ubicaciones de su empresa.
    * Lista de códigos de departamentos de su empresa.

2. Puede encontrar estas SKU y estos códigos en su contrato de Fuze y en los documentos de configuración, así como si se pone en contacto con su representante de Fuze.

3. Cuando haya recibido los requisitos, el representante de Fuze le proporcionará un token de autenticación de Fuze, que es necesario para habilitar la integración. Este valor se escribe en el campo Token secreto de la pestaña Aprovisionamiento de la aplicación Fuze en Azure Portal.

## <a name="add-fuze-from-the-gallery"></a>Adición de Fuze desde la galería

Antes de configurar Fuze para el aprovisionamiento automático de usuarios con Azure AD, es preciso agregar Fuze desde la galería de aplicaciones de Azure AD a la lista de aplicaciones SaaS administradas.

**Para agregar Fuze desde la galería de aplicaciones de Azure AD, siga estos pasos:**

1. En **[Azure Portal](https://portal.azure.com)** , en el panel de navegación izquierdo, seleccione **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, seleccione el botón **Nueva aplicación** en la parte superior del panel.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Fuze**, seleccione **Fuze** en el panel de resultados y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Fuze en la lista de resultados](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-fuze"></a>Configuración del aprovisionamiento automático de usuarios en Fuze 

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de Azure AD para crear, actualizar y deshabilitar usuarios o grupos en Fuze en función de las asignaciones de grupos y usuarios de Azure AD.

> [!TIP]
> También puede optar por habilitar el inicio de sesión único basado en SAML para Fuze siguiendo las instrucciones del [tutorial de inicio de sesión único de Fuze](fuze-tutorial.md). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático de usuarios, aunque estas dos características se complementan entre sí.

### <a name="to-configure-automatic-user-provisioning-for-fuze-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para Fuze en Azure AD:

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Seleccione **Aplicaciones empresariales** y luego **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Fuze**.

    ![Vínculo a Fuze en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Pestaña Aprovisionamiento](common/provisioning.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Pestaña Aprovisionamiento](common/provisioning-automatic.png)

5. En la sección **Credenciales de administrador**, escriba `https://api.fuze.com/scim/v2` en la **URL de inquilino**. Escriba el valor **SCIM Authentication Token** (Token de autenticación de SCIM) que le ha proporcionado anteriormente el representante de Fuze en **Token secreto**. Haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a Fuze. Si la conexión no se establece, asegúrese de que la cuenta de Fuze tiene permisos de administrador y pruebe de nuevo.

    ![Token de URL de inquilino](common/provisioning-testconnection-tenanturltoken.png)

6. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Correo electrónico de notificación](common/provisioning-notification-email.png)

7. Haga clic en **Save**(Guardar).

8. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to Fuze** (Sincronizar usuarios de Azure Active Directory con Fuze).

    ![Asignaciones de usuario de Fuze](media/fuze-provisioning-tutorial/image01.png)

9. Examine los atributos de grupo que se sincronizan entre Azure AD y Fuze en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Fuze con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Asignaciones de usuario de Fuze](media/fuze-provisioning-tutorial/image00.png)

10. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

11. Para habilitar el servicio de aprovisionamiento de Azure AD para Fuze, cambie el **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Estado de aprovisionamiento activado](common/provisioning-toggle-on.png)

12. Elija los valores deseados en **Ámbito**, en la sección **Configuración**, para definir los usuarios o grupos que desea que se aprovisionen en Fuze.

    ![Ámbito del aprovisionamiento](common/provisioning-scope.png)

15. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Guardar la configuración de aprovisionamiento](common/provisioning-configuration-save.png)

Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. Puede usar la sección **Detalles de sincronización** para supervisar el progreso y seguir los vínculos al informe de actividad de aprovisionamiento, donde se describen todas las acciones que ha llevado a cabo el servicio de aprovisionamiento de Azure AD en Fuze.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte el tutorial de [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Limitaciones del conector

* Fuze admite atributos SCIM personalizados, denominados **Derechos**. Estos atributos se pueden crear, pero no actualizar. 

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../app-provisioning/configure-automatic-user-provisioning-portal.md).
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../app-provisioning/check-status-user-account-provisioning.md).