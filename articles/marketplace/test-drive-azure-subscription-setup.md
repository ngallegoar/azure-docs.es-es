---
title: Configuración de una suscripción a Azure Marketplace para las versiones de prueba hospedadas
description: Explica cómo configurar una suscripción a Azure Marketplace para versiones de prueba de Dynamics 365 for Customer Engagement y Dynamics 365 for Operations.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: trkeya
ms.author: trkeya
ms.date: 11/09/2020
ms.openlocfilehash: 6f74f3b1f5aad153903ba5d290a290973203a875
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94489393"
---
# <a name="set-up-an-azure-marketplace-subscription-for-hosted-test-drives"></a>Configuración de una suscripción a Azure Marketplace para las versiones de prueba hospedadas

Explica cómo configurar una suscripción a Azure Marketplace para versiones de prueba de **Dynamics 365 for Customer Engagement** y **Dynamics 365 for Operations**.

## <a name="set-up-for-dynamics-365-for-customer-engagement"></a>Configuración de Dynamics 365 for Customer Engagement

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con una cuenta de administrador.
2. Compruebe que se encuentra en el inquilino asociado a la instancia de la versión de prueba de Dynamics 365. Para ello, mantenga el puntero sobre el icono de la cuenta en la esquina superior derecha. Si no está en el inquilino correcto, seleccione el icono de cuenta para cambiar al inquilino correcto.

    :::image type="content" source="./media/test-drive/select-correct-tenant.png" alt-text="Selección del inquilino correcto.":::

3. Compruebe que la licencia del **Plan Dynamics 365 for Customer Engagement** está disponible.

    [![Comprobación de la licencia del plan.](media/test-drive/check-plan-license.png)](media/test-drive/check-plan-license.png#lightbox)

4. Cree una aplicación de Azure Active Directory (Azure AD) en Azure. AppSource usará esta aplicación para aprovisionar y desaprovisionar el usuario de la versión de prueba en el inquilino.
    1. En el panel de filtro, seleccione **Azure Active Directory**.
    2. Seleccione **App registrations** (Registros de aplicaciones).

        [![Selección de los registros de aplicaciones.](media/test-drive/app-registrations.png)](media/test-drive/app-registrations.png#lightbox)

    3. Seleccione **Nuevo registro**.
    4. Proporcione un nombre adecuado para la aplicación.

        :::image type="content" source="./media/test-drive/register-an-application.png" alt-text="Registro de una aplicación.":::

    5. En Tipos de cuenta admitidos, seleccione **Cuentas en cualquier directorio organizativo y cuentas Microsoft personales**.
    6. Seleccione **Crear** y espere a que la aplicación se cree.
    7. Una vez creada la aplicación, anote el **Id. de la aplicación** que se muestra en la pantalla de información general. Necesitará este valor más adelante al configurar la versión de prueba.
    8. Para agregar un URI de redireccionamiento de nativeclient, seleccione la hoja **Autenticación**. En **Configuración de la plataforma**, seleccione el mosaico de aplicación **Agregar plataforma** > **Móvil** > **Escritorio**. Elija el URI de redireccionamiento **nativeclient** y seleccione **Configurar**.

        :::image type="content" source="./media/test-drive/configure-desktop-devices.png" alt-text="Adición de un URI de redireccionamiento de nativeclient.":::

    9. En **Administrar aplicación**, seleccione **Permisos de API**.
    10. Seleccione **Agregar un permiso** y, a continuación, **Microsoft Graph API**.
    11. Seleccione la categoría de permisos **Aplicación**, y, a continuación, los permisos **Directory.Read.All** y **Directory.ReadWrite.All**.

        :::image type="content" source="./media/test-drive/microsoft-graph.png" alt-text="Configuración de los permisos de aplicación.":::

    12. Para agregar el acceso **Dynamics CRM: Suplantación de usuario** para la lista de permitidos de la aplicación de Azure AD, seleccione **Agregar permiso** de nuevo.

        :::image type="content" source="./media/test-drive/request-api-permissions.png" alt-text="Solicitud de los permisos de aplicación.":::

    13. Una vez que se ha agregado el permiso, seleccione **Conceder consentimiento de administrador para Microsoft**.
    14. En el mensaje de alerta, seleccione **Sí**.

        [![Muestra que los permisos de aplicación se concedieron correctamente.](media/test-drive/api-permissions-confirmation-customer.png)](media/test-drive/api-permissions-confirmation-customer.png#lightbox)

    15. Para generar un secreto para la aplicación de Azure AD:
        1. En **Administrar aplicación**, seleccione **Certificados y secretos**.
        2. En Secretos de cliente, seleccione **Nuevo secreto de cliente**.
        3. Escriba una descripción, como *Versión de prueba*, y seleccione una duración adecuada. La versión de prueba se interrumpirá una vez que expire esta clave, momento en el que tendrá que generar y proporcionar una nueva clave a AppSource.
        4. Seleccione **Agregar** para generar el secreto de la aplicación de Azure. Copie este valor, ya que se ocultará en cuanto abandone esta hoja. Necesitará este valor más adelante al configurar la versión de prueba.

            :::image type="content" source="./media/test-drive/add-client-secret.png" alt-text="Adición de un secreto de cliente.":::

5. A veces se tarda más de lo esperado en sincronizar un usuario de Azure AD con una instancia de CRM. Para ayudar con esto, se ha agregado un proceso para forzar la sincronización de usuarios, pero se requiere que el Centro de partners agregue la aplicación de Azure AD a la lista de permitidos. Para ello, consulte [Sincronización de usuarios con la instancia de Customer Engagement](https://github.com/microsoft/AppSource/blob/master/Microsoft%20Hosted%20Test%20Drive/CDS_Utility_to_ForceUserSync_in_CRM_Instance.md).
6. Agregue el rol Entidad de servicio a la aplicación para permitir que la aplicación de Azure AD quite usuarios de su inquilino de Azure.
    1. Abra un símbolo del sistema de PowerShell de nivel administrativo.
    2. Install-Module MSOnline (ejecute este comando si MSOnline no está instalado).
    3. Connect-MsolService (esto mostrará una ventana emergente; inicie sesión con el inquilino organizacional recién creado).
    4. $applicationId = **<ID_DE_SU_APLICACIÓN>** .
    5. $sp = Get-MsolServicePrincipal -AppPrincipalId $applicationId.
    6. Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId $sp.ObjectId -RoleMemberType servicePrincipal.

        :::image type="content" source="./media/test-drive/sign-in-to-account.png" alt-text="Inicio de sesión en su cuenta.":::

7. Agregue la aplicación de Azure creada anteriormente como usuario de la aplicación a la instancia de la versión de prueba de CRM.
    1. Agregue un nuevo usuario a **Azure Active Directory**. Solo se requieren los valores de **Nombre** y **Nombre de usuario** (pertenecientes al mismo inquilino) para crear este usuario, deje los demás campos con los valores predeterminados. Copie el valor de nombre de usuario.
    2. Inicie sesión en **Instancia de CRM** y seleccione **Configuración** > **Seguridad** > **Usuarios**.
    3. Cambie la vista a **Usuarios de la aplicación**.

        :::image type="content" source="./media/test-drive/application-users.png" alt-text="Definición de la información de la cuenta de un usuario.":::

    4. Agregue un nuevo usuario (asegúrese de que el formulario sea para USUARIO DE LA APLICACIÓN).
    5. Escriba el mismo nombre de usuario en los campos **Correo electrónico principal** y **Nombre de usuario**. Agregue el **Id. de Aplicación de Azure** en **Id. de la aplicación**.
    6. Escriba cualquier **Nombre completo**.
    7. Seleccione **Guardar**.
    8. Seleccione **Administrar roles**.
    9. Asigne un rol de seguridad personalizado u OOB que contenga privilegios de lectura, escritura y asignación de roles, como *Administrador del sistema*.

        :::image type="content" source="./media/test-drive/security-roles-selection.png" alt-text="Selección de los privilegios del rol.":::

    10. Asigne al usuario de la aplicación el rol de seguridad personalizado que ha creado para la versión de prueba.

## <a name="set-up-for-dynamics-365-for-operations"></a>Configuración de Dynamics 365 for Operations

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con una cuenta de administrador.
2. Compruebe que se encuentra en el inquilino asociado a la instancia de la versión de prueba de Dynamics 365. Para ello, mantenga el puntero sobre el icono de la cuenta en la esquina superior derecha. Si no está en el inquilino correcto, seleccione el icono de cuenta para cambiar al inquilino correcto.

    :::image type="content" source="./media/test-drive/select-correct-tenant.png" alt-text="Selección del inquilino correcto.":::

3. Cree una aplicación de Azure AD en Azure. AppSource usará esta aplicación para aprovisionar y desaprovisionar el usuario de la versión de prueba en el inquilino.
    1. En el panel de filtro, seleccione **Azure Active Directory**.
    2. Seleccione **App registrations** (Registros de aplicaciones).

        :::image type="content" source="./media/test-drive/app-registrations.png" alt-text="Selección de un registro de aplicación.":::

    3. Seleccione **Nuevo registro**.
    4. Proporcione un nombre adecuado para la aplicación.

        :::image type="content" source="./media/test-drive/register-an-application.png" alt-text="Registro de una aplicación.":::

    5. En Tipos de cuenta admitidos, seleccione **Cuentas en cualquier directorio organizativo y cuentas Microsoft personales**.
    6. Seleccione **Crear** y espere a que la aplicación se cree.
    7. Una vez creada la aplicación, anote el **Id. de la aplicación** que se muestra en la pantalla de información general. Necesitará este valor más adelante al configurar la versión de prueba.
    8. En **Administrar aplicación**, seleccione **Permisos de API**.
    9. Seleccione **Agregar un permiso** y, a continuación, **Microsoft Graph API**.
    10. Seleccione la categoría de permisos **Aplicación**, y, a continuación, los permisos **Directory.Read.All** y **Directory.ReadWrite.All**.

        :::image type="content" source="./media/test-drive/microsoft-graph.png" alt-text="Definición de los permisos para la aplicación.":::

    11. Seleccione **Agregar permiso**.
    12. Una vez que se ha agregado el permiso, seleccione **Conceder consentimiento de administrador para Microsoft**.
    13. En el mensaje de alerta, seleccione **Sí**.

        [![Muestra que los permisos de aplicación se concedieron correctamente.](media/test-drive/api-permissions-confirmation-operations.png)](media/test-drive/api-permissions-confirmation-operations.png#lightbox)

    14. Para generar un secreto para la aplicación de Azure AD:
        1. En **Administrar aplicación**, seleccione **Certificados y secretos**.
        2. En Secretos de cliente, seleccione **Nuevo secreto de cliente**.
        3. Escriba una descripción, como *Versión de prueba*, y seleccione una duración adecuada. La versión de prueba se interrumpirá una vez que expire esta clave, momento en el que tendrá que generar y proporcionar una nueva clave a AppSource.
        4. Seleccione **Agregar** para generar el secreto de la aplicación de Azure. Copie este valor, ya que se ocultará en cuanto abandone esta hoja. Necesitará este valor más adelante al configurar la versión de prueba.

            :::image type="content" source="./media/test-drive/add-client-secret.png" alt-text="Adición de un secreto de cliente.":::

4. Agregue el rol Entidad de servicio a la aplicación para permitir que la aplicación de Azure AD quite usuarios de su inquilino de Azure.
    1. Abra un símbolo del sistema de PowerShell de nivel administrativo.
    2. Install-Module MSOnline (ejecute este comando si MSOnline no está instalado).
    3. Connect-MsolService (esto mostrará una ventana emergente; inicie sesión con el inquilino organizacional recién creado).
    4. $applicationId = **<ID_DE_SU_APLICACIÓN>** .
    5. $sp = Get-MsolServicePrincipal -AppPrincipalId $applicationId.
    6. Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId $sp.ObjectId -RoleMemberType servicePrincipal.

        :::image type="content" source="./media/test-drive/sign-in-to-account.png" alt-text="Inicio de sesión en su cuenta.":::

5. Ahora agregue la aplicación anterior a **Dynamics 365 for Operations** para permitir que la aplicación administre los usuarios.
    1. Busque la instancia de **Dynamics 365 for Operations**.
    2. En la esquina superior izquierda, haga clic en el menú de tres líneas (hamburguesa).
    3. Seleccione **Administración del sistema**.
    4. Seleccione **Aplicaciones de Azure Active Directory**.
    5. Seleccione **+ Nuevo**.
    6. Escriba el **Id. de cliente de la aplicación de Azure AD** en cuyo nombre va a realizar las acciones.

    > [!NOTE]
    > El Id. de usuario en cuyo nombre se realizarán las acciones (normalmente, el administrador del sistema de la instancia o un usuario que tenga privilegios para agregar otros usuarios).

    [![El Id. de usuario en cuyo nombre se realizarán las acciones (normalmente, el administrador del sistema de la instancia o un usuario que tenga privilegios para agregar otros usuarios).](media/test-drive/system-admin-user-id.png)](media/test-drive/system-admin-user-id.png#lightbox)
<!--
## Next steps

- [Commercial marketplace partner and customer usage attribution](azure-partner-customer-usage-attribution.md) -->
