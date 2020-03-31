---
title: 'Tutorial: Configuración de la escritura diferida de SuccessFactors en Azure Active Directory | Microsoft Docs'
description: Aprenda a configurar la escritura diferida de atributos en SuccessFactors desde Azure AD.
services: active-directory
author: cmmdesai
documentationcenter: na
manager: jodadzie
ms.assetid: ad255bd4-9e50-43a1-a92b-359215868b6b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2019
ms.author: chmutali
ms.openlocfilehash: 2de0cdd32428884170f549afacdbd52c3a10c93f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77060055"
---
# <a name="tutorial-configure-attribute-writeback-from-azure-ad-to-sap-successfactors-preview"></a>Tutorial: Configuración de la escritura diferida de atributos de Azure AD en SAP SuccessFactors (versión preliminar)
El objetivo de este tutorial es mostrar los pasos que debe realizar para la escritura diferida de atributos de Azure AD en SuccessFactors Employee Central. El único atributo que se admite actualmente para la escritura diferida es el atributo de correo electrónico. 

## <a name="overview"></a>Información general

Después de configurar la integración del aprovisionamiento de entrada mediante la aplicación de aprovisionamiento de [SuccessFactors a AD local](sap-successfactors-inbound-provisioning-tutorial.md) o la aplicación de aprovisionamiento de [SuccessFactors a Azure AD](sap-successfactors-inbound-provisioning-cloud-only-tutorial.md), puede configurar opcionalmente la aplicación SuccessFactors Writeback para que escriba la dirección de correo electrónico en SuccessFactors. 

### <a name="who-is-this-user-provisioning-solution-best-suited-for"></a>¿Para quién es más adecuada esta solución de aprovisionamiento de usuarios?

La solución de aprovisionamiento de usuarios SuccessFactors Writeback es idónea para:

* Organizaciones que usan Office 365 y que desean realizar la escritura diferida de atributos autoritativos administrados por TI (como la dirección de correo electrónico) en SuccessFactors

## <a name="configuring-successfactors-for-the-integration"></a>Configuración de SuccessFactors para la integración

Un requisito común de todos los conectores de aprovisionamiento de SuccessFactors es que requieren credenciales de una cuenta de SuccessFactors con los permisos correctos para invocar las API OData de SuccessFactors. En esta sección se describen los pasos para crear la cuenta de servicio de SuccessFactors y conceder los permisos adecuados. 

* [Creación e identificación de una cuenta de usuario de API en SuccessFactors](#createidentify-api-user-account-in-successfactors)
* [Creación de un rol de permisos de API](#create-an-api-permissions-role)
* [Creación de un grupo de permisos para el usuario de la API](#create-a-permission-group-for-the-api-user)
* [Concesión del rol de permisos al grupo de permisos](#grant-permission-role-to-the-permission-group)

### <a name="createidentify-api-user-account-in-successfactors"></a>Creación e identificación de una cuenta de usuario de API en SuccessFactors
Trabaje con el equipo de administración de SuccessFactors o con el asociado de implementación para crear o identificar una cuenta de usuario en SuccessFactors que se usará para invocar las API OData. Las credenciales de nombre de usuario y contraseña de esta cuenta se necesitarán al configurar las aplicaciones de aprovisionamiento en Azure AD. 

### <a name="create-an-api-permissions-role"></a>Creación de un rol de permisos de API

* Inicie sesión en SAP SuccessFactors con una cuenta de usuario que tenga acceso al centro de administración.
* Busque *Manage Permission Roles* (Administrar roles de permisos) y, después, seleccione **Manage Permission Roles** (Administrar roles de permisos) en los resultados de la búsqueda.
  ![Administración de roles de permisos](./media/sap-successfactors-inbound-provisioning/manage-permission-roles.png)
* En Permission Role List (Lista de roles de permisos), haga clic en **Create New** (Crear).
  > [!div class="mx-imgBorder"]
  > ![Creación de un rol de permisos](./media/sap-successfactors-inbound-provisioning/create-new-permission-role-1.png)
* Agregue valores en **Role Name** (Nombre de rol) y **Description** (Descripción) para el nuevo rol de permisos. El nombre y la descripción deben indicar que el rol se utilizará para los permisos de uso de la API.
  > [!div class="mx-imgBorder"]
  > ![Detalle de rol de permisos](./media/sap-successfactors-inbound-provisioning/permission-role-detail.png)
* En Permission settings (Configuración de los permisos), haga clic en **Permission...** (Permiso), desplácese hacia abajo en la lista de permisos y haga clic en **Manage Integration Tools** (Administrar herramientas de integración). Active la casilla **Allow Admin to Access to OData API through Basic Authentication** (Permitir que el administrador acceda a la API OData mediante autenticación básica).
  > [!div class="mx-imgBorder"]
  > ![Administración de herramientas de integración](./media/sap-successfactors-inbound-provisioning/manage-integration-tools.png)
* Desplácese hacia abajo en el mismo cuadro y seleccione **Employee Central API**. Agregue permisos, tal como se muestra a continuación, de lectura y edición mediante la API ODATA. Seleccione la opción de edición si tiene previsto usar la misma cuenta para el escenario de escritura diferida en SuccessFactors. 
  > [!div class="mx-imgBorder"]
  > ![Permisos de lectura y escritura](./media/sap-successfactors-inbound-provisioning/odata-read-write-perm.png)
* Haga clic en **Done** (Acabado). Haga clic en **Guardar cambios**.

### <a name="create-a-permission-group-for-the-api-user"></a>Creación de un grupo de permisos para el usuario de la API

* En el centro de administración de SuccessFactors, busque *Manage Permission Groups* (Administrar grupos de permisos) y, después, seleccione **Manage Permission Groups** (Administrar grupos de permisos) en los resultados de la búsqueda.
  > [!div class="mx-imgBorder"]
  > ![Administración de grupos de permisos](./media/sap-successfactors-inbound-provisioning/manage-permission-groups.png)
* En la ventana Manage Permission Groups (Administrar grupos de permisos), haga clic en **Create New** (Crear).
  > [!div class="mx-imgBorder"]
  > ![Incorporación de un nuevo grupo](./media/sap-successfactors-inbound-provisioning/create-new-group.png)
* Agregue un valor en Group Name (Nombre de grupo) para el nuevo grupo. El nombre del grupo debe indicar que se utilizará para los usuarios de la API.
  > [!div class="mx-imgBorder"]
  > ![Nombre del grupo de permisos](./media/sap-successfactors-inbound-provisioning/permission-group-name.png)
* Agregue miembros al grupo. Por ejemplo, puede seleccionar **Username** (Nombre de usuario) en el menú desplegable People Pool (Grupo de personas) y, a continuación, escribir el nombre de usuario de la cuenta de API que se usará para la integración. 
  > [!div class="mx-imgBorder"]
  > ![Adición de miembros del grupo](./media/sap-successfactors-inbound-provisioning/add-group-members.png)
* Haga clic en **Done** (Listo) para terminar de crear el grupo de permisos.

### <a name="grant-permission-role-to-the-permission-group"></a>Concesión del rol de permisos al grupo de permisos

* En el centro de administración de SuccessFactors, busque *Manage Permission Roles* (Administrar roles de permisos) y, después, seleccione **Manage Permission Roles** (Administrar roles de permisos) en los resultados de la búsqueda.
* En **Permission Role List** (Lista de roles de permisos), seleccione el rol que creó para los permisos de uso de la API.
* En **Grant this role to...** (Conceder este rol a), haga clic en el botón **Add...** (Agregar).
* Seleccione **Permission Group...** (Grupo de permisos) en el menú desplegable y, a continuación, haga clic en **Select...** (Seleccionar) para abrir la ventana Groups (Grupos), en la que deberá buscar y seleccionar el grupo creado anteriormente. 
  > [!div class="mx-imgBorder"]
  > ![Incorporación del grupo de permisos](./media/sap-successfactors-inbound-provisioning/add-permission-group.png)
* Revise la concesión del rol de permisos al grupo de permisos. 
  > [!div class="mx-imgBorder"]
  > ![Detalles de rol y grupo de permisos](./media/sap-successfactors-inbound-provisioning/permission-role-group.png)
* Haga clic en **Guardar cambios**.

## <a name="configuring-successfactors-writeback"></a>Configuración de SuccessFactors Writeback

En esta sección se proporcionan los pasos necesarios para realizar las siguientes actividades: 

* [Incorporación de la aplicación del conector de aprovisionamiento y configuración de la conectividad con SuccessFactors](#part-1-add-the-provisioning-connector-app-and-configure-connectivity-to-successfactors)
* [Configuración de las asignaciones de atributos](#part-2-configure-attribute-mappings)
* [Habilitar e iniciar el aprovisionamiento de usuarios](#enable-and-launch-user-provisioning)

### <a name="part-1-add-the-provisioning-connector-app-and-configure-connectivity-to-successfactors"></a>Parte 1: Incorporación de la aplicación del conector de aprovisionamiento y configuración de la conectividad con SuccessFactors

**Para configurar SuccessFactors Writeback:**

1. Vaya a <https://portal.azure.com>.

2. En la barra de navegación de la izquierda, seleccione **Azure Active Directory**.

3. Seleccione **Enterprise Applications** (Aplicaciones empresariales) y **All Applications** (Todas las aplicaciones).

4. Seleccione **Add an application** (Agregar una aplicación) y seleccione la categoría **All** (Todos).

5. Busque **SuccessFactors Writeback** y agregue esa aplicación desde la galería.

6. Una vez que se haya agregado la aplicación y se muestre la pantalla de detalles de la aplicación, seleccione **Aprovisionamiento**.

7. Cambie el **Modo de** **aprovisionamiento** a **Automático**.

8. Cumplimente la sección **Credenciales de administrador** del siguiente modo:

   * **Nombre de usuario de administrador**: escriba el nombre de usuario de la cuenta de usuario de SuccessFactors API, con el identificador de empresa anexado. El formato es: **username\@companyID**

   * **Contraseña de administrador:** escriba la contraseña de la cuenta de usuario de SuccessFactors API. 

   * **URL de inquilino:** escriba el nombre del punto de conexión de servicios de la API OData de SuccessFactors. Especifique solo el nombre de host del servidor sin http o https. Este valor debería ser similar al siguiente: **nombre-servidor-api.successfactors.com**.

   * **Correo electrónico de notificación**: escriba su dirección de correo electrónico y marque la casilla "Enviar una notificación por correo electrónico cuando se produzca un error".
    > [!NOTE]
    > El servicio de aprovisionamiento de Azure AD envía la notificación por correo electrónico si el trabajo de aprovisionamiento entra en un estado de[cuarentena](/azure/active-directory/manage-apps/application-provisioning-quarantine-status).

   * Haga clic en el botón **Probar conexión**. Si la prueba de conexión se lleva a cabo correctamente, haga clic en el botón **Guardar** situado en la parte superior. Si se produce un error, compruebe que las credenciales y la dirección URL de SuccessFactors son válidas.
    >[!div class="mx-imgBorder"]
    >![Azure Portal](./media/sap-successfactors-inbound-provisioning/sfwb-provisioning-creds.png)

   * Una vez que las credenciales se guardan correctamente, en la sección **Asignaciones** se mostrará la asignación predeterminada **Synchronize Azure Active Directory Users to SuccessFactors** (Sincronizar usuarios de Azure Active Directory con SuccessFactors).

### <a name="part-2-configure-attribute-mappings"></a>Parte 2: configuración de las asignaciones de atributos

En esta sección, configurará cómo fluyen los datos de los usuarios de SuccessFactors a Active Directory.

1. En la pestaña Aprovisionamiento, en **Asignaciones**, haga clic en **Synchronize Azure Active Directory Users to SuccessFactors** (Sincronizar usuarios de Azure Active Directory con SuccessFactors).

1. En el campo **Ámbito de objeto de origen** puede seleccionar los conjuntos de usuarios de Azure AD que deben tenerse en cuenta para Writeback; para ello debe definir un conjunto de filtros basados en atributos. El ámbito predeterminado es "Todos los usuarios de Azure AD". 
   > [!TIP]
   > Al configurar la aplicación de aprovisionamiento por primera vez, deberá probar y verificar las expresiones y asignaciones de atributos para asegurarse de que ofrece el resultado deseado. Microsoft recomienda usar los filtros de ámbito en **Ámbito de objeto de origen** para probar las asignaciones con algunos usuarios de prueba de Azure AD. Una vez haya verificado que las asignaciones funcionan, puede quitar el filtro o expandirlo gradualmente para incluir más usuarios.

1. El campo **Acciones del objeto de destino** solo admite la operación **Actualizar**.

1. En la sección **Asignaciones de atributos** solo se puede cambiar el identificador de coincidencia que se usa para vincular un perfil de usuario de SuccessFactors con el usuario de Azure AD y qué atributo de Azure AD actúa como origen del correo electrónico. 
    >[!div class="mx-imgBorder"]
    >![Azure Portal](./media/sap-successfactors-inbound-provisioning/sfwb-attribute-mapping.png)

   >[!NOTE]
   >La escritura diferida de SuccessFactors solo admite el atributo de correo electrónico. No use **Agregar nueva asignación** para agregar nuevos atributos. 

1. Para guardar las asignaciones, haga clic en **Guardar** en la parte superior de la sección Asignación de atributos.

Una vez completada la configuración de la asignación de atributos, ahora puede [habilitar e iniciar el servicio de aprovisionamiento de usuarios](#enable-and-launch-user-provisioning).

## <a name="enable-and-launch-user-provisioning"></a>Habilitar e iniciar el aprovisionamiento de usuarios

Una vez completadas las configuraciones de la aplicación de aprovisionamiento de SuccessFactors, puede activar el servicio de aprovisionamiento en Azure Portal.

> [!TIP]
> De forma predeterminada, al activar el servicio de aprovisionamiento, se iniciarán las operaciones de aprovisionamiento para todos los usuarios del ámbito. Si hay errores en la asignación o problemas con los datos de Workday, puede que se produzcan errores con el trabajo de aprovisionamiento y que entre en estado de cuarentena. Para evitar esto, como procedimiento recomendado, es conveniente configurar el filtro **Ámbito de objeto de origen** y probar las asignaciones de atributos con algunos usuarios de prueba antes de iniciar la sincronización completa de todos los usuarios. Una vez haya verificado que las asignaciones funcionan y que obtiene los resultados deseados, puede quitar el filtro o expandirlo gradualmente para incluir más usuarios.

1. En la pestaña **Aprovisionamiento**, establezca **Estado de aprovisionamiento** en **Activado**.

2. Haga clic en **Save**(Guardar).

3. Esta operación dará comienzo a la sincronización inicial, que puede tardar una cantidad de horas variable, según el número de usuarios que haya en el inquilino de SuccessFactors. Puede consultar en la barra de progreso el seguimiento del progreso del ciclo de sincronización. 

4. En cualquier momento, compruebe la pestaña **Registros de auditoría** en Azure Portal para ver las acciones que ha realizado el servicio de aprovisionamiento. Los registros de auditoría muestran todos los eventos de sincronización individuales realizados por el servicio de aprovisionamiento, por ejemplo, los usuarios que se leen fuera de Workday y que luego se agregan o actualizan en Active Directory. 

5. Una vez completada la sincronización inicial, se escribe un informe resumido de auditoría en la pestaña **Aprovisionamiento**, tal y como se muestra a continuación.

   > [!div class="mx-imgBorder"]
   > ![Barra de progreso de aprovisionamiento](./media/sap-successfactors-inbound-provisioning/prov-progress-bar-stats.png)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../app-provisioning/check-status-user-account-provisioning.md)
* [Aprenda a configurar el inicio de sesión único entre SuccessFactors y Azure Active Directory](successfactors-tutorial.md)
* [Aprenda a integrar otras aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [Aprenda a exportar e importar las configuraciones de aprovisionamiento](../app-provisioning/export-import-provisioning-configuration.md)

