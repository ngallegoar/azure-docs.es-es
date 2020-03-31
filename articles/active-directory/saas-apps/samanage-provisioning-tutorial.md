---
title: 'Tutorial: Configuración de Samanage para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo configurar Azure Active Directory para aprovisionar y cancelar automáticamente el aprovisionamiento de cuentas de usuario de Samanage.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 62d0392f-37d4-436e-9aff-22f4e5b83623
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 988efc2087b3b30e6073bd7f6e2cf08f91fd397c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77060531"
---
# <a name="tutorial-configure-samanage-for-automatic-user-provisioning"></a>Tutorial: Configuración de Samanage para el aprovisionamiento automático de usuarios

Este tutorial muestra los pasos que se deben realizar en Samanage y Azure Active Directory (Azure AD) para configurar Azure AD con el objetivo de aprovisionar y desaprovisionar automáticamente usuarios y grupos en Samanage.

> [!NOTE]
> En este tutorial se describe un conector que se basa en el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md).

## <a name="prerequisites"></a>Prerrequisitos

En el escenario que se describe en este tutorial se supone que ya cuenta con los elementos siguientes:

* Un inquilino de Azure AD.
* Un [inquilino de Samanage](https://www.samanage.com/pricing/) con el paquete profesional.
* Una cuenta de usuario de Samanage con permisos de administrador.

> [!NOTE]
> La integración de aprovisionamiento de Azure AD se basa en la [API REST de Samanage](https://www.samanage.com/api/). Esta API está disponible para los desarrolladores de Samanage para las cuentas con el paquete profesional.

## <a name="add-samanage-from-the-azure-marketplace"></a>Adición de Samanage desde Azure Marketplace

Antes de configurar Samanage para el aprovisionamiento automático de usuarios con Azure AD, agregue Samanage desde Azure Marketplace en su lista de aplicaciones SaaS administradas.

Para eliminar Samanage de Marketplace, siga estos pasos.

1. En el panel de navegación de la izquierda de [Azure Portal](https://portal.azure.com), seleccione **Azure Active Directory**.

    ![Icono de Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una aplicación nueva, en la parte superior del cuadro de diálogo, seleccione **Nueva aplicación**.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Samanage** y seleccione **Samanage** del panel de resultados. Para agregar la aplicación, seleccione **Agregar**.

    ![Samanage en la lista de resultados](common/search-new-app.png)

## <a name="assign-users-to-samanage"></a>Asignación de usuarios a Samanage

Azure Active Directory usa un concepto denominado *asignaciones* para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto de aprovisionamiento automático de usuarios, solo se sincronizan los usuarios o grupos que se han asignado a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, decida qué usuarios o grupos de Azure AD necesitan acceder a Samanage. Para asignar estos usuarios o grupos a Samanage, siga las instrucciones de [Asignación de un usuario o un grupo a una aplicación empresarial](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

### <a name="important-tips-for-assigning-users-to-samanage"></a>Sugerencias importantes para asignar usuarios a Samanage

*    Actualmente, los roles de Samanage se rellenan automática y dinámicamente en la interfaz de usuario de Azure Portal. Antes de asignar roles de Samanage a los usuarios, asegúrese de completar una sincronización inicial con Samanage para recuperar los roles más recientes en el inquilino de Samanage.

*    Se recomienda asignar un único usuario de Azure AD a Samanage para probar la configuración inicial de aprovisionamiento automático de usuarios. Puede asignar usuarios y grupos adicionales más adelante tras la correcta realización de las pruebas.

*    Cuando asigne un usuario a Samanage, seleccione un rol específico de la aplicación válido (si está disponible) en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="configure-automatic-user-provisioning-to-samanage"></a>Configuración del aprovisionamiento automático de usuarios en Samanage

Esta sección le guía por los pasos para configurar el servicio de aprovisionamiento de Azure AD. Úselo para crear, actualizar y deshabilitar usuarios o grupos en Samanage en función de las asignaciones de usuarios o grupos en Azure AD.

> [!TIP]
> También puede habilitar el inicio de sesión único basado en SAML para Samanage. Siga las instrucciones del [Tutorial del inicio de sesión único de Samanage](samanage-tutorial.md). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático de usuarios, aunque estas dos características se complementan entre sí.

### <a name="configure-automatic-user-provisioning-for-samanage-in-azure-ad"></a>Configuración del aprovisionamiento automático de usuarios para Samanage en Azure AD

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Seleccione **Aplicaciones empresariales** > **Todas las aplicaciones** > **Samanage**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Samanage**.

    ![El vínculo a Samanage en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Aprovisionamiento de Samanage](./media/samanage-provisioning-tutorial/ProvisioningTab.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Pestaña Aprovisionamiento](common/provisioning-automatic.png)

5. En la sección **Credenciales de administrador**, escriba los valores de **URL de inquilino** y **Token de secreto** de Samanage. Haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a Samanage. Si la conexión no se establece, asegúrese de que la cuenta de Samanage tiene permisos de administrador e inténtelo de nuevo.

    ![Prueba de conexión de Samanage](./media/samanage-provisioning-tutorial/provisioning.png)

6. En el cuadro **Correo electrónico de notificación**, escriba la dirección de correo electrónico de la persona o el grupo que deben recibir las notificaciones de error de aprovisionamiento. Seleccione la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Notificación por correo electrónico de Samanage](./media/samanage-provisioning-tutorial/EmailNotification.png)

7. Seleccione **Guardar**.

8. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to Samanage** (Sincronizar usuarios de Azure Active Directory con Samanage).

    ![Sincronización de usuarios de Samanage](./media/samanage-provisioning-tutorial/UserMappings.png)

9. Revise los atributos de usuario que se sincronizan entre Azure AD y Samanage, en la sección **Attribute Mappings** (Asignaciones de atributos). Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Samanage con el objetivo de realizar operaciones de actualización. Para guardar los cambios, seleccione **Guardar**.

    ![Atributos de usuario coincidentes de Samanage](./media/samanage-provisioning-tutorial/UserAttributeMapping.png)

10. Para habilitar las asignaciones de grupos, en la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Groups to Samanage** (Sincronizar grupos de Azure Active Directory con Samanage).

    ![Sincronización de grupos de Samanage](./media/samanage-provisioning-tutorial/GroupMappings.png)

11. Establezca **Habilitado** a **Sí** para sincronizar los grupos. Revise los atributos de grupo que se sincronizan entre Azure AD y Samanage en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Samanage con el objetivo de realizar operaciones de actualización. Para guardar los cambios, seleccione **Guardar**.

    ![Atributos de grupo coincidentes de Samanage](./media/samanage-provisioning-tutorial/GroupAttributeMapping.png)

13. Para configurar filtros de ámbito, consulte las instrucciones del [tutorial sobre filtros de ámbito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

13. Para habilitar el servicio de aprovisionamiento de Azure AD para Samanage, vaya a la sección **Configuración** y cambie el **estado de aprovisionamiento** a **Activado**.

    ![Estado de aprovisionamiento de Samanage](./media/samanage-provisioning-tutorial/ProvisioningStatus.png)

14. Defina los usuarios o grupos que quiere que se aprovisionen en Samanage. En la sección **Configuración**, seleccione los valores que quiere incluir en el **Ámbito**. Cuando seleccione la opción **Sincronizar todos los usuarios y grupos**, tenga en cuenta las limitaciones que se describen más adelante en la sección "Limitaciones del conector".

    ![Ámbito de Samanage](./media/samanage-provisioning-tutorial/ScopeSync.png)

15. Cuando esté listo para realizar el aprovisionamiento, seleccione **Guardar**.

    ![Guardado de Samanage](./media/samanage-provisioning-tutorial/SaveProvisioning.png)


Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las sincronizaciones posteriores. Se produce aproximadamente cada 40 minutos mientras se ejecuta el servicio de aprovisionamiento de Azure AD. 

Puede usar la sección **Detalles de sincronización** para supervisar el progreso y hacer clic en los vínculos al informe de actividad de aprovisionamiento. En el informe se describen todas las acciones que el servicio de aprovisionamiento de Azure AD realiza en Samanage.

Para obtener información acerca de cómo leer los registros de aprovisionamiento de Azure AD, consulte [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Limitaciones del conector

Si selecciona la opción **Sincronizar todos los usuarios y grupos** y configura un valor para el atributo **roles** de Samanage, el valor del cuadro **Valor predeterminado si es nulo (opcional)** se debe expresar en el formato siguiente:

- {"displayName":"rol"}, donde el rol es el valor predeterminado que quiera.

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/samanage-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/samanage-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/samanage-provisioning-tutorial/tutorial_general_03.png
