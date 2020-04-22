---
title: Administración de clientes potenciales para Dynamics 365 for Customer Engagement | Azure Marketplace
description: Configure la administración de clientes potenciales para Dynamics 365 for Customer Engagement.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: dsindona
ms.openlocfilehash: 5b3e35b6d19905e3c5262dfea3e52511510c9ffe
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2020
ms.locfileid: "81252969"
---
# <a name="configure-lead-management-for-dynamics-365-for-customer-engagement"></a>Configurar la administración de clientes potenciales para Dynamics 365 for Customer Engagement

En este artículo se describe cómo configurar Dynamics 365 for Customer Engagement (antiguamente, Dynamics CRM Online). Obtenga más información sobre el cambio [aquí](https://docs.microsoft.com/dynamics365/customerengagement/on-premises/admin/on-prem-server-based-sharepoint-online) para procesar clientes potenciales a partir de la oferta de Marketplace. 

>[!Note]
>Estas instrucciones son específicas para el entorno de Dynamics 365 for Customer Engagement de Microsoft hospedado en la nube. Actualmente no se admite la conexión directa a un entorno local de Dynamics. Hay otras opciones para que reciba clientes potenciales, como configurar un [punto de conexión https](./commercial-marketplace-lead-management-instructions-https.md) o una [tabla de Azure](./commercial-marketplace-lead-management-instructions-azure-table.md).

## <a name="prerequisites"></a>Prerrequisitos

Para completar los pasos de este artículo, se requieren los siguientes permisos de usuario:

* Debe ser administrador de la instancia de Dynamics 365 for Customer Engagement para poder instalar una solución y seguir estas instrucciones.
* Debe ser administrador de inquilinos para crear una nueva cuenta de servicio para el servicio de clientes potenciales que se usa para enviar clientes potenciales a partir de las ofertas de Marketplace.
* Debe tener acceso al portal de administración de Office 365.
* Debe tener acceso a Azure Portal.

## <a name="install-the-solution"></a>Instalar la solución

1.  Descargue la [solución Microsoft Marketplace Lead Writer](https://mpsapiprodwus.blob.core.windows.net/documentation/MicrosoftMarketplacesLeadIntegrationSolution_1_0_0_0_target_CRM_6.1_managed.zip) y guárdela en una unidad local de su equipo.

2.  Abra Dynamics 365 for Customer Engagement, para ello, vaya a la dirección URL de la instancia de Dynamics (como `https://tenant.crm.dynamics.com`).

3.  Para acceder a la configuración, seleccione el icono de engranaje y **Configuración avanzada** en la barra de navegación superior.
 
    ![Dynamics: Configuración avanzada](./media/commercial-marketplace-lead-management-instructions-dynamics/dynamics-advanced-settings.png)

4.  En la página Configuración, acceda al menú Configuración desde la barra de navegación superior y seleccione **Soluciones**.

    >[!Note]
    >Si no ve las opciones en la siguiente captura de pantalla, no tiene los permisos que necesita para continuar. Póngase en contacto con un administrador de su instancia de Dynamics 365 for Customer Engagement.

    ![Dynamics 365: Soluciones](./media/commercial-marketplace-lead-management-instructions-dynamics/dynamics-solutions.png)

5. Una vez en la página Soluciones, seleccione **Importar** y navegue hasta la ubicación donde guardó la solución *Microsoft Marketplace Lead Writer* que descargó en el paso 1.

    ![Dynamics 365 for Customer Engagement: Importar](./media/commercial-marketplace-lead-management-instructions-dynamics/dynamics-crm-import.png)

6. Complete la importación de la solución siguiendo el Asistente para importar soluciones.

## <a name="configure-user-permissions"></a>Configurar los permisos de usuario

Para escribir clientes potenciales en su instancia de Dynamics 365 for Customer Engagement, tiene que compartir una cuenta de servicio con nosotros y configurar los permisos para la cuenta.

Utilice los siguientes pasos para crear la cuenta de servicio y asignar permisos. Puede usar **Azure Active Directory** u **Office 365**.

>[!Note]
>En función de la opción de autenticación que seleccione, puede ir directamente a las instrucciones correspondientes en función de su elección. Consulte [Azure Active Directory](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics#azure-active-directory) u [Office 365](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics#office-365).

### <a name="azure-active-directory"></a>Azure Active Directory

Se recomienda esta opción, ya que obtiene la ventaja adicional de nunca tener que actualizar el nombre de usuario y la contraseña para seguir recibiendo clientes potenciales. Para usar la opción de Azure Active Directory, debe proporcionar el identificador de la aplicación, la clave de aplicación y el identificador de directorio de la aplicación de Active Directory.

Siga los pasos a continuación para configurar Azure Active Directory para Dynamics 365 for Customer Engagement.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) y después seleccione el servicio Azure Active Directory en el panel de navegación de la izquierda.

2. Seleccione **Propiedades** en el panel de navegación izquierdo de Azure Active Directory y copie el valor de **Id. de directorio** en esa página. Guarde este valor, ya que es el valor de *Id. de directorio* que tendrá que proporcionar en el portal de publicación para recibir clientes potenciales para su oferta de Marketplace.

    ![Azure Active Directory: Propiedades](./media/commercial-marketplace-lead-management-instructions-dynamics/aad-properties.png)

3. Seleccione **Registros de aplicaciones** del panel de navegación izquierdo de Azure Active Directory y, después, seleccione **Nuevo registro** en esa página.
4. Escriba un nombre para la aplicación. Especifique un nombre de aplicación descriptivo.
5. En Tipos de cuenta compatibles, seleccione **Cuentas en cualquier directorio organizativo**.
6. En URI de redirección, seleccione **Web** y proporcione un URI (por ejemplo, `https://contosoapp1/auth`). 
7. Seleccione **Registrar**.

    ![Registro de una aplicación](./media/commercial-marketplace-lead-management-instructions-dynamics/register-an-application.png)

8. Ahora que ha registrado la aplicación, acceda a la página de información general de la aplicación y copie el valor **Id. de aplicación (cliente)** en esa página. Guarde este valor, ya que es el valor de *Id. de aplicación (cliente)* que tendrá que proporcionar en el portal de publicación y en Dynamics para recibir clientes potenciales para su oferta de Marketplace.

    ![Id. de aplicación (cliente)](./media/commercial-marketplace-lead-management-instructions-dynamics/application-id.png)

9. Seleccione **Certificados y secretos** en el panel de navegación izquierdo de la aplicación y seleccione **Nuevo secreto de cliente** en esa página. Escriba una descripción clara para el secreto de cliente y seleccione la opción **Nunca** en Expira. Seleccione **Agregar** para crear el secreto de cliente.

    ![Aplicación: Certificación y secretos](./media/commercial-marketplace-lead-management-instructions-dynamics/aad-certificates-secrets.png)

10. En cuanto el secreto de cliente se cree correctamente, **copie el valor de secreto de cliente**. No podrá recuperar el valor después de salir de la página. Guarde este valor, ya que es el valor de *Secreto de cliente* que tendrá que proporcionar en el portal de publicación para recibir clientes potenciales para su oferta de Marketplace. 
11. Seleccione **Permisos de API** en el panel de navegación izquierdo de la aplicación y, luego, seleccione **Agregar un permiso**.
12. Seleccione API de Microsoft y, luego, seleccione **Dynamics CRM** como API.
13. En *¿Qué tipo de permiso necesita la aplicación web?* , asegúrese de que **Permisos delegados** esté seleccionado. Compruebe el permiso para **user_impersonation** *Access Common Data Service as organization users* (Acceder a Common Data Service como usuarios de la organización). Seleccione **Agregar permisos**.

    ![Adición de permisos](./media/commercial-marketplace-lead-management-instructions-dynamics/api-permissions.png)

14. Después de completar los pasos 1-13 de Azure Portal, navegue a la dirección URL para ir a la instancia de Dynamics 365 for Customer Engagement (por ejemplo, `https://tenant.crm.dynamics.com`).
15. Para acceder a la configuración, seleccione el icono de engranaje y **Configuración avanzada** en la barra de navegación superior.
16. En la página Configuración, acceda al menú Configuración desde la barra de navegación superior y seleccione **Seguridad**.
17. En la página Seguridad, seleccione **Usuarios**.  En la página Usuarios, seleccione la lista desplegable "Usuarios habilitados" para cambiar a **Usuarios de la aplicación**.
18. Seleccione **Nuevo** para crear un nuevo usuario. 

    ![Creación de un nuevo usuario](./media/commercial-marketplace-lead-management-instructions-dynamics/application-users.png)

19. En **Nuevo usuario**, asegúrese de que USUARIO: USUARIO DE LA APLICACIÓN esté seleccionado. Escriba un nombre de usuario, un nombre completo y una dirección de correo electrónico para el usuario que quiera usar con esta conexión. Además, pegue el **Id. de aplicación** de la aplicación que creó en Azure Portal en el paso 8. Seleccione **Guardar y cerrar** para completar la adición del usuario.

    ![Nuevo usuario](./media/commercial-marketplace-lead-management-instructions-dynamics/new-user-info.png)

20. Vaya a “Configuración de seguridad” en este artículo para finalizar la configuración de la conexión para este usuario.

### <a name="office-365"></a>Office 365

Si no quiere usar Azure Active Directory, puede registrar un nuevo usuario en el *Centro de administración de Microsoft 365*. Tendrá que actualizar cada 90 días su nombre de usuario y contraseña para continuar obteniendo clientes potenciales.

Siga los pasos a continuación para configurar Office 365 para Dynamics 365 for Customer Engagement.

1. Inicie sesión en el [Centro de administración de Microsoft 365](https://admin.microsoft.com).

2. Seleccione **Agregar un usuario**.

    ![Centro de administración de Microsoft 365: Agregar un usuario](./media/commercial-marketplace-lead-management-instructions-dynamics/ms-365-add-user.png)

4. Cree un usuario nuevo para el servicio de escritura de clientes potenciales. Configure las siguientes opciones:

    * Proporcionar un nombre de usuario
    * Proporcione una contraseña y desactive la opción "Hacer que este usuario cambie la contraseña cuando inicie sesión por primera vez".
    * Seleccione el rol para que sea “Usuario, sin acceso como administrador” como el rol para el usuario.
    * Seleccione "Plan Dynamics 365 Customer Engagement" como licencia de producto, como se muestra en la siguiente captura de pantalla. Se le cobrará por la licencia que elija. 

Guarde este valor, ya que son los valores de *Nombre de usuario y Contraseña* que tendrá que proporcionar en el portal de publicación para recibir clientes potenciales para su oferta de Marketplace.

![Centro de administración de Microsoft 365: Nuevo usuario](./media/commercial-marketplace-lead-management-instructions-dynamics/ms-365-new-user.png)

## <a name="security-settings"></a>Configuración de seguridad

El paso final es permitir que el usuario que ha creado escriba los clientes potenciales.

1. Abra Dynamics 365 for Customer Engagement, para ello, vaya a la dirección URL de la instancia de Dynamics (como `https://tenant.crm.dynamics.com`).
2. Para acceder a la configuración, seleccione el icono de engranaje y **Configuración avanzada** en la barra de navegación superior.
3. En la página Configuración, acceda al menú Configuración desde la barra de navegación superior y seleccione **Seguridad**.
4. Una vez en la página Seguridad, seleccione **Usuarios** y seleccione el usuario que creó en la sección Configurar permisos de usuario de este documento y, luego, seleccione **Administrar roles**. 

    ![Administrar roles](./media/commercial-marketplace-lead-management-instructions-dynamics/security-manage-roles.png)

5. Busque el nombre del rol "Microsoft Marketplace responsable del escritor" y selecciónelo para asignar el rol al usuario.

    ![Administrar los roles de usuario](./media/commercial-marketplace-lead-management-instructions-dynamics/security-manage-user-roles.png)

    >[!Note]
    >Este rol lo crea la solución que importó y solo tiene permisos para escribir clientes potenciales y para realizar un seguimiento de la versión de la solución a fin de garantizar la compatibilidad.

6. Vuelva a la página Seguridad y seleccione **Roles de seguridad**. Busque el rol "Microsoft Marketplace Lead Writer" y selecciónelo.

    ![Roles de seguridad](./media/commercial-marketplace-lead-management-instructions-dynamics/security-roles.png)

7. Una vez en el rol de seguridad, seleccione la pestaña **Registros principales**. Busque la entidad "Configuración de UI de la entidad de usuario" y habilite los permisos Crear, Leer y Escribir para el usuario (1/4 de círculo amarillo) para esa entidad haciendo clic una vez en cada uno de los círculos correspondientes.

    ![Microsoft Marketplace Lead Writer: Registros principales](./media/commercial-marketplace-lead-management-instructions-dynamics/marketplace-lead-writer.png)

8. Ahora, vaya a la pestaña **Personalización**. Busque la entidad "Trabajo del sistema" y habilite los permisos de Leer, Escribir y Anexar a la organización (verde sólido) para esa entidad haciendo clic cuatro veces en cada uno de los círculos correspondientes.

    ![Microsoft Marketplace Lead Writer: Personalización](./media/commercial-marketplace-lead-management-instructions-dynamics/marketplace-lead-writer-customization.png)

9. **Guarde y cierre**.

## <a name="configure-your-offer-to-send-leads-to-dynamics-365-for-customer-engagement"></a>Configuración de la oferta para enviar clientes potenciales a Dynamics 365 for Customer Engagement

Cuando esté listo para configurar la información de administración de clientes potenciales de la oferta en el portal de publicación, siga estos pasos:

1. Vaya a la página **Configuración de la oferta** de su oferta.
2. Seleccione **Conectar** en la sección Administración de clientes potenciales.

    ![Conexión a la administración de clientes potenciales](./media/commercial-marketplace-lead-management-instructions-dynamics/connect-lead-management.png)

3. En la ventana emergente Detalles de conexión, seleccione **Dynamics 365 for Customer Engagement** como destino de clientes potenciales.

    ![Detalles de la conexión: Destino de clientes potenciales](./media/commercial-marketplace-lead-management-instructions-dynamics/connection-details-lead-destination.png)

4. Proporcione la **dirección URL de la instancia de Dynamics 365**, como `https://contoso.crm4.dynamics.com`.

5. Seleccione el método de **Autenticación**, Azure Active Directory u Office 365. 
6. Si ha seleccionado Azure Active Directory, proporcione el **Id. de aplicación (cliente)** (ejemplo: `23456052-aaaa-bbbb-8662-1234df56788f`), **Id. de directorio** (ejemplo: `12345678-8af1-4asf-1234-12234d01db47`) y **Secreto de cliente** (ejemplo: `1234ABCDEDFRZ/G/FdY0aUABCEDcqhbLn/ST122345nBc=`).

    ![Detalles de conexión: Azure Active Directory](./media/commercial-marketplace-lead-management-instructions-dynamics/connection-details-application-id.png)

7. Si ha seleccionado Office 365, proporcione el **Nombre de usuario** (por ejemplo: `contoso@contoso.onmicrosoft.com`) y la Contraseña (por ejemplo: `P@ssw0rd`).

    ![Detalles de conexión: Nombre de usuario](./media/commercial-marketplace-lead-management-instructions-dynamics/connection-details-authentication.png)

8. **Correo electrónico de contacto**: proporcione correos electrónicos de los usuarios de su empresa que deban recibir notificaciones de correo electrónico cuando se reciba un nuevo cliente potencial. Puede proporcionar varias direcciones de correo electrónico separadas con un punto y coma.
9. Seleccione **Aceptar**.

Para asegurarse de que se ha conectado correctamente a un destino de clientes potenciales, haga clic en el botón validar. Si se realiza correctamente, tendrá un cliente potencial de prueba en el destino de clientes potenciales.

![Administración de clientes potenciales: detalles de conexión de la cuenta de almacenamiento](./media/commercial-marketplace-lead-management-instructions-dynamics/dynamics-connection-details.png)

>[!Note]
>Debe terminar de configurar el resto de las partes de la oferta y publicarla para poder recibir clientes potenciales para la oferta.
